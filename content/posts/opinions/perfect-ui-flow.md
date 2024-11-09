---
author: "Callum Sykes"
title: "The Perfect UI Flow"
date: "2024-11-08"
description: "How I like to code in c#"
tags: ["c#", "coding", "style"]
ShowToc: false
ShowBreadCrumbs: true
draft: true
---

I've been coding in C# now for around 8 years and I've slowly developed my own opinionated coding style. This isn't about indents, tabs vs spaces or editors, it's about about code structure, data and control flow. A note, not all of the code will compile, some items are simplified for brevity, and many of these concepts can apply to many languages. Some languages even make use of these concepts 


# #1. UIs should only return
A UI, dialog, panel etc should never modify anything it doesn't have to