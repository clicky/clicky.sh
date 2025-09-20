---
author: "clicky"
title: "Using Span<T>"
date: "2024-10-17"
description: "Using Span<T> to map memory"
tags: ["span", "low level", "c#"]
ShowToc: false
ShowBreadCrumbs: true
draft: true
---

I recently decided to figure out what `Span<T>` was all about and if I could find a use for it, I'd recently learnt a lot about c, the stack, the heap, memory allocation etc and `Span<T>` seemed like a really nice way to test and apply the concepts I've learnt to C#.

# Avoiding Copies
The simplest reason to use `Span<T>` is avoiding copies by manipulating memory directly.
Let's explore my first venture which was using strings.
The first thing I realised, which in hindsight is obvious, is that we cannot change the size of the memory chunk we're editing, not the worst limitation, but definitely a limitation which could prevent the use of spans.

Strings are an array of chars at heart, and whilst a char doesn't necessarily equal a valid letter, in many cases this isn't an issue.

Let's replace the stroke/width colours of an svg element, as we're swapping data around, it's always a guarantee that the string will be the same length before and after we're done.

``` c#
public static string UsingSpans(string svg)
{
  Span<char> charSpan = new(svg.ToArray());
  ModifySvg(charSpan);

  return new string(charSpan);  
}
```

# Mapping Memory

I've come to realise that computer memory is just a long list, and each class is a pointer to a chunk of memory with a length.

Take a simple struct, such as colour, each colour is a list of bytes in memory in order of the declaration of the properties. There is no header or constraints on how this memory is interpreted, it could be interpreted as an int if you wanted to as the length of memory is identical (assuming A,R,G,B)!

With this information I decided to experiment with span and see if it would be possible to map a Bitmap to a custom Colour struct, this idea in theory, is solid.

``` c# {hl_lines=[13]}
// Our custom colour struct
public record struct MyColour(byte A, byte R, byte G, byte B);

// Code to pull the pixels out of a bitmap
public void MyMappingFunction(Bitmap bitmap)
{
  using var data = bitmap.Lock();

  unsafe
  {
    var imageSize = bitmap.Width * bitmap.Height;
    Span<MyColour> pixels = new(data.Data.ToPointer(), imageSize);
    ;
```

Low and behold this works seamlessly, and I'm very happy with this application of Spans and memory management. I find GetPixel/SetPixel to be slower than I would like, but I'm not sure if that is internalised nonsense, or if my span solution is better. So let's benchmark it.

// ... Include Benchmark