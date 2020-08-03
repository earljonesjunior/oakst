---
title: "Rat Signal Part 3: The Endpoint"
date: 2020-08-02T11:25:39-05:00
slug: "rat-signal-part-3"
draft: false
tags: ['mqtt', 'node']
project: "rat-signal"
description: ""
---

The surf report data for Rat Signal is "generously provided" by [Magic Seaweed](https://magicseaweed.com/Okaloosa-Pier-Surf-Report/954/). 
Those words are in quotes because I do not (yet!) have explicit permission by MSW to access or even use their forecast data in the way I am. This is a strange territory and I'm not sure if I'm violating their terms of service or not but I'm leaning towards "most likely". No bad intentions, Magic Seaweed! This project is for 
personal use only.

## Magic Seaweed's Data API

Magic Seaweed does not provide a MQTT broker so the forecast data needs to be fetched manually.
Additionally, Magic Seaweed does not provide a public API to easily query for data. 

> **Update 08/01/20**: *Turns out Magic Seaweed DOES offer a developer API.
I have applied for an API key and will update Rat Signal when the application goes through.*

There are two primary methods you can use to pull data from a website that lacks a public API.
The first method, [scraping](https://en.wikipedia.org/wiki/Web_scraping), is dark and fraught with terror. There are different techniques in web scraping but they all involve downloading the entire webpage and stripping away the unneeded parts. Things get messy and can break easily.

The second method returns cleaner data but requires some detective work and isn't always a sure-thing. 
This method involves searching for a data API that the website itself uses and reverse-engineering it 
to make it give you what you need. For this to work, the website's API needs to be accessible 
by the public and not check for authentication or origin of request.

I pulled up the Chrome developer's console and navigated around Magic Seaweed while
monitoring cross-domain XHR requests for external API calls. It took some hunting but I found a few!
Here are some of the endpoints that I found:

```javascript
https://magicseaweed.com/api/mdkey/useroption
https://magicseaweed.com/api/mdkey/spot
https://magicseaweed.com/api/mdkey/region
https://magicseaweed.com/api/mdkey/photo
https://magicseaweed.com/api/mdkey/forecast
```

These API endpoints are accessible without any user authentication! That last endpoint path, `/forecast/`,
returns the I'm looking for. After a little bit of reverse-engineering I came up with this request 
string
```js
https://magicseaweed.com/api/mdkey/forecast?spot_id=954&fields=timestamp,threeHourTimeText,fadedRating,solidRating,swell.minBreakingHeight,swell.maxBreakingHeight,swell.period,wind.speed
```

Click [here](https://magicseaweed.com/api/mdkey/forecast?spot_id=954&fields=timestamp,threeHourTimeText,fadedRating,solidRating,swell.minBreakingHeight,swell.maxBreakingHeight,swell.period,wind.speed) to check it out for yourself.
This string hits the endpoint with the query parameter `spot_id=954` to tell MSW I'm only looking for
forecast data for [Okaloosa Pier](https://magicseaweed.com/Okaloosa-Pier-Surf-Report/954/) and
the query parameter `fields=...` to tell the API that I'm only interested in certain data points.
This query returns a week's worth of forecast data divided into 3 hour blocks:

```json
[
    {
        "timestamp": 1596348000,
        "fadedRating": 2,
        "solidRating": 0,
        "threeHourTimeText": "12am",
        "swell": {
            "period": 11,
            "minBreakingHeight": 2,
            "maxBreakingHeight": 3
        },
        "wind": {
            "speed": 6
        }
    },
    {
        "timestamp": 1596358800,
        "fadedRating": 2,
        "solidRating": 0,
        "threeHourTimeText": "3am",
        "swell": {
            "period": 10,
            "minBreakingHeight": 2,
            "maxBreakingHeight": 4
        },
        "wind": {
            "speed": 5
        }
    }
    ...
]
```

## The Node Server

I set up a Node server handle the API request and to publish the data to the MQTT broker. The 
request is promise-based and looks like this:
```ts
export function getMSWData(): Promise<MSWResponse> {

    return new Promise((resolve, reject) => {
        axios.get(mswEndpoint)
            .then(response => {
                return filterMSWData(response.data);
            })
            .then(filteredData => {
                resolve(filteredData);
            })
            .catch(err => {
                reject(err);
            })
    })
}
```

As mentioned above, this returns a week worth of objects - 56 in total. I use the UNIX timestamp in
the data to filter down to the nearest forecast time:

```ts
function filterMSWData(data: MSWResponse[]): MSWResponse {

    let timestamp = Math.floor(Date.now() / 1000);
    let i = 0;
    // setup delta to 1ms later than first timestamp in to keep loop happy
    let delta = Math.abs(timestamp - data[0].timestamp) + 1;
    // iterate through data until delta is no longer decreasing
    for (let element of data) {
        // if delta is still decreasing, carry on
        if (Math.abs(timestamp - element.timestamp) < delta) {
            delta = Math.abs(timestamp - element.timestamp);
            i++;
        } 
        // if no longer decreasing, return previous timestamp
        else {
            i = (i == 0) ? 0 : i - 1;
            break;
        }
    }
    return data[i];
}
```

The request is scheduled to happen one minute after every hour. I figured MSW *might* push forecast updates
every hour so I'm waiting the extra minute for the data to propagate. Once gathered, the data
is published to the MQTT broker.

```ts
const timestring = '0 1 */1 * * *';
const job = cron.schedule(timestring, () => {
    let time = new Date().toLocaleString();
    getMSWData()
        .then(res => {
            var rideable = (res.fadedRating >= 1 || res.solidRating >= 1) ? "1" : "0";
            client.publish("surf/rideable", rideable);
            client.publish("surf/time/timestamp", res.timestamp.toString());
            client.publish("surf/rating/solidRating", res.fadedRating.toString());
            client.publish("surf/rating/fadedRating", res.solidRating.toString());
            client.publish("surf/time/timeText", res.threeHourTimeText);
            client.publish("surf/period", res.swell.period.toString());
            client.publish("surf/height/minHeight", res.swell.minBreakingHeight.toString());
            client.publish("surf/height/maxHeight", res.swell.maxBreakingHeight.toString());
            client.publish("surf/windSpeed", res.wind.speed.toString());
        })
        .catch(err => {
            console.log(err);
        })
})
```
The `rideable` key isn't something that is provided by MSW. Magic Seaweed rates the surf using a star
rating. A solid star means there may be rideable surf. A faded star means their may be rideable surf
but other conditions, such as wind gusts, may make conditions unfavorable. My metric for `rideable` is if
the forecast is rated at least one solid star or two faded stars. Here on the Gulf Coast, that's about
as good as it gets!

The full source code for this server can be found in my [Github repo](https://github.com/bruthaearl/rat-signal/tree/master/endpoint).

This server will be deployed using Docker. More on that in a later post.

---

# The Rat Signal Series:

- [Rat Signal Part 1: Overview](/notes/rat-signal-part-1)
- [Rat Signal Part 2: The Broker](/notes/rat-signal-part-2)
- *[Rat Signal Part 3: The Endpoint](/notes/rat-signal-part-3)*
- [Rat Signal Part 4: The Middleman](/notes/rat-signal-part-4)
- [Rat Signal Part 5: Deployment](/notes/rat-signal-part-5)
- [Rat Signal Part 6: Red Alert](/notes/rat-signal-part-6)
- Rat Signal Part 7: Numbers are Nice (TBA)
- Rat Signal Part 8: The OK Wave off Kanagawa (TBA)
