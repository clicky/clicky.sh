---
author: "clicky"
title: "Structs"
date: "2025-02-18"
description: "How do Structs work, really?"
tags: ["testing", "combinatorial", "c#"]
ShowToc: false
ShowBreadCrumbs: true
draft: true
---

At work a developer had an issue with structs wiping values when being copied which is terrifying.

It's tempting to assume this is a compiler or runtime issue but something tells me that this might be structs being used badly and maybe structs are more complex than we know.

So I decided to dive _deep_ into structs and figure this out.


# What is a Struct
A struct is a data type that is value type and lives on the stack*. For example an integer or boolean. Easy.


*