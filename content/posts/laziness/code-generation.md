---
author: "clicky"
title: "Code Generation"
date: "2024-10-17"
description: "My thoughts on Code generation"
tags: ["nunit", "testing", "combinatorial", "c#"]
ShowToc: false
ShowBreadCrumbs: true
draft: true
---

A while ago, I wrote cunit, I was sick of other units libraries, and I wanted to create something better, faster, smaller and easier mostly to prove it was possible.

https://github.com/clicketyclackety/cunit

At the time of writing, I've mostly abandoned the project and nobody uses it, but I never set out with any expectations. I was curious to see if I could create a set of rules and generate a well written, strict and functional unit library.

# Code Generation
As units are constant ratios and all live within a fixed ecosystem, it seemed to me that generating the code would save me a lot of time, allow me to iterate faster, and also allow me to create code that would run much faster overall.


# Environment setup
I set up my vscode into 4 panels
On the top left I had my cunit code, unit generators etc.
on the bottom left a terminal running dotnet watch

On the top right I had the generated results, usually the Meter
On the bottom right I had another terminal running `dotnet watch --tests`.

The net result of this is that every time I changed the file and hit save I would receive instant feedback about that change, did the change break syntax, did it fail a test, did it even compile? Which let me develop at an absolutely astounding speed mostly because I could fix any bug almost instantly after introducing it.

# Conclusions
This project showed me just how far dontnet has come since .net framework and how fantastic a project can be to work in, I've massively upped my standards for my minimum projects and I'm really happy with the result.

Code generation is fucking cool.