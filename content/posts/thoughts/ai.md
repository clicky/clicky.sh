---
author: "clicky"
title: "What I think of AI"
date: "2026-01-21"
description: "Gotta put it somewhere"
tags: ["ai", "ramblings", "old man yells at cloud"]
ShowToc: true
ShowBreadCrumbs: true
---

I figured I'd take some time to jot what I think about AI. I've never formulated a cohesive opinion on it and this should help me whenever I have to discuss it or evaluate its part in a project. Let's break this whole thing down. I will likely add more in time, but this is a good start for now.

## My AI journey

I started using CoPilot and ChatGPT very early when I first heard about them (2022-3ish) and realised they could be a very powerful programming tool. I initially found them to be like an extremely powerful autocomplete and very helpful for generating smaller chunks of code. Things I still consider them to be good at. I experimented with using ChatGPT to create a Dungeons and Dragons DM after my groups moved away. It couldn't even come close. I used ChatGPT in a hackathon to communicate Grasshopper scripts to it ^[1], it did very well. This project has been continuously cited by various academic papers which has been quite exciting. Eventually I realised I might be leaning on AI more than I'd like and decided to stop using it as an experiment. I've still never shipped code AI generated that I haven't read, and I'm glad in retrospect I at least had that pride in my work. ~I've largely stopped using AI for programming (occasionally using it for debugging)~. I'm at an impasse where I cannot decide on the correct point in a programmers career to hand them AI. As an Architect my first year I was told to never use CAD and only learn using pens and pencils to learn the importance and intent of every line. I think AI should be treated the same way, but I don't know how many years or which concepts I should have mastered first.

**UPDATE : 16th May 2026**

I am now using Claude Code extensively and very much enjoying it doing lots of awful, boring grunt tasks and code research for me. Prototyping and iterating has become very quick and fun. I'm also using, and now creating, MCP Servers.

## AI Revolution

I've heard people compare AI to looms and the industrial revolution where we saw a huge shift from manual human labour to automated machines in factories. I don't think this is a good comparison. I'm from Manchester originally, I've seen (and used) manual looms and I've seen automated looms, they're an incredible thing to behold. But there's something that seems to be ignored when comparing AI to other parts of human history. AI is not determinate. A loom is very much `f(x) = y`. A loom set to a given encoded program will produce the exact same pattern every time. LLMs are non-determinate ^[2], they do not produce the same output for the same input.

I do think AI is likely to be a revolution, largely because of the gargantuan amount of human power and money that is being invested into pushing it along. Industries are clearly betting on this technology and there are areas that have found AI to be absolute game changer (e.g OCR, building renderings, girlfriends for losers, debugging programming, and other academic areas). That being said, I think LLMs are the start of this, not the end of this. They are WAY too unprofitable ^[3] to run at present.

**UPDATE : 16th May 2026**

It seems this determinative thing does not really matter at all. I'm able to iterate and get claude to create me things I need. Even if it does a slightly different job each time, this feels negligible. That being said, I am not sure how long this will last, or if this is a good comparison, the loom had a very determinative ROI, whereas AI has so much proprietary dependence.

What has spurred this update and change of page? In a word, Harnesses. The way Claude Code functions is absolutely excellent. I now use it every day and enjoy doing so. I read a long time ago that the winner of the AI Revolution will be the one who makes the best box to put AI in. I think they are right.

## Ethical Concerns

It's of course worth noting that AI data centers in the US are built unethically and degrade the communities around them. Consuming ungodly amounts of electricity and wasting ungodly amounts of water. I wonder how many tokens is equivalent to a mile of air flight for energy used?

My next concern is for artistry. Image generation is trained on stolen work, text is trained on stolen stories, all of which was created by talented people who have as yet never been compensated for this.

If I create and expect to be compensated for my creations, how can I use AI un-hypocritically?

## Trust

The longer I code, and the more I try to write programs to last forever, the more I evaluate and consider the trustworthiness before incorporating a technology, tool, library or language into my stack. Is it open source? Can I understand its code? What dependencies does it have?

I don't trust OpenAI, I don't trust Microsoft, I don't trust X/Grok, I don't  trust Claude, and I don't trust any for-profit (AI) company. They are unprofitable, and if they can make it past the ponzi scheme phase into profitability, that will, as usual, become their only concern. The AIs will start cramming adverts in, running at lower, dumber levels, and prices will be raised until morale improves amongst the shareholders.

So any company offering AI I can't trust that it'll be around in a year, two years, that the price will be reasonable or even how it works.

Local models largely resolve this problem. I could download a model from huggingface with some python code and stuff it onto a Raspberry pi that I never connect to the internet ever again.

## Consciousness

Imagine you are in a burning building. In front of you are two things. A cat, and ChatGPT on a thumb drive, the destruction of which will mean ChatGPT is gone and can never be restored. In effect it will "die".

Which do you chose?

If your answer is not the cat, then I think you're using your consciousness unconsciously. You'd save the cat, a living creature, with thoughts, memories, dreams and the ability to love. Nothing is worth more than that.

LLM AIs are emulating these things, they are not doing these things, they cannot surprise us, upset us and then make amends, or ever care about us.

--- 

^[1] https://github.com/enmerk4r/GHPT/

^[2] https://152334h.github.io/blog/non-determinism-in-gpt-4/

> NOTE : It does seem that there are and in theory LLMs could be run more deterministically because they are a function, just with a billion parameters. To expand on this point, if I change a small thing in my code, the output will also change slightly. If I change a prompt slightly, the output could change quite drastically. I think this is an important difference when comparing a loom and AI.

^[3] https://archive.ph/MRdRN

> QUOTE : Cursor estimated last year that a $200-per-month Claude Code subscription could use up to $2,000 in compute, suggesting significant subsidization by Anthropic. Today, that subsidization appears to be even more aggressive, with that $200 plan able to consume about $5,000 in compute, according to a different person who has seen analyses on the company’s compute spend patterns.