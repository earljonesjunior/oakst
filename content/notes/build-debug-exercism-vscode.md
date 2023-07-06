---
title: "Build and Debugging Exercism Problems within Visual Studio Code"
date: 2023-07-06T17:11:25-05:00
slug: ""
draft: false
tags: ["vscode", "exercism"]
project: ""
description: ""
---

I've been working my way through [Exercism's C Track](https://exercism.org/tracks/c) to sharpen my programming skills. 
I like how Exercism provides a CLI you can use to write and submit programs from your own machine. Handy!

Exercism uses Unity as a testing framework and heavily encourages Test-Driven Development as part of the problem solving process.
One issue I have encountered with the way Exercism solutions are run is that I do not have access to `stdout`. My machine might not be configured correctly but I'm inclined to think [this behavior is intentional](https://github.com/exercism/csharp/issues/1395).

I'm an electronics engineer by profession, but software isn't the main thing we do in my office and most of my software and firmware projects are the "figure it out yourself" kind of situations. I admittedly haven't had any mentorship in proper software design and testing. For software projects, I tend to overuse console logging as the primary way to figure out what the heck is going on behind the scenes.

Without access to my handy `printf`, I figured it was time to figure out how to used Visual Studio Code's debugging functionality.

## Configuring Visual Studio Code

> Note: my host machine runs Fedora and I'm using GCC and GDB to build and debug. Things will be different with other configurations.

After using the Exercism CLI tool to download the problem files, your directory will look something like this:

```
problem/
    .exercism/
        config.json
        metadata.json
    test_framework/
        unity_internals.h
        unity.h
        unity.c
    HELP.md
    makefile
    problem.c
    problem.h
    README.md
    test_problem.c
```

Create a new folder `.vscode/` in the problem's root directory. Create two new files in this folder - `tasks.json` and `launch.json`. Your directory will now look something like this: 

```
problem/
    .exercism/
        config.json
        metadata.json
    .vscode/
        launch.json
        tasks.json
    test_framework/
        unity_internals.h
        unity.h
        unity.c
    HELP.md
    makefile
    problem.c
    problem.h
    README.md
    test_problem.c
```

### Building

The makefile is already configured to run the tests via the command `make test`. Modify `tasks.json` to make this command the default build task:

```json
// tasks.json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "make",
            "args": [
                "test"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```

### Debugging

To configure the debugger, modify `launch.json` as such:

```json
// launch.json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/tests.out",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "build"
        }
    ]
}
```

This configuration firsts runs the build task from the previous step and then launches the GDB debugger within VSCode. That's it!