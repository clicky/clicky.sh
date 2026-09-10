---
author: "clicky"
title: "Coding Style"
date: "2026-09-09"
description: "How I now code with an AI agent."
tags: ["c#", "coding", "style"]
ShowToc: false
ShowBreadCrumbs: true
---

This is a super small post but just to help my brain unload.

I vibe coded an AI Assistant and MCP for Rhino -> https://github.com/mcneel/rhinoai. This is fine. Until I needed to suddenly UP the quality and make this into a shippable product for a very large number of users. As I'm rewriting I am still vibing other stuff, which means for the first time I'm in a repo that's part human and part AI. And that's... fine... for now...

But as a way to know WHO did what, I've started with a simple convention.

Human Files -> `Myfile.cs`
AI Files    -> `MyFile.ai.cs`

So now I can slowly rework the vibed files into human written and properly considered files at a sensible pace and know how far along the rewrite is whilst still making the progress I need to make at the progress I need to make.

-- cs
