---
author: "clicky"
title: "Structs & Enums & Switches"
date: "2024-10-17"
description: "I absolutely love these things"
tags: ["testing", "combinatorial", "c#"]
ShowToc: false
ShowBreadCrumbs: true
draft: true
---

Title. I fucking love Enums, Structs and Switch statements.

These 3 items have never let me down and there's good reason for that, they're designed to be indestructible.

The longer I write code the more I realise maintaining code will always be the largest part of my time and the smaller the percentage that is the better. To this end, I have found that making code easy to read, understand and also trying to write code that doesn't fail incorrectly is super important.

# Enums

ALWAYS declare a default None = 0. Even if you wrote a program to return the output of a random coin flip which has only 2 real results, it's always possible that something in the code fails and None lets you accurately declare or know this.

This is a perfect enum.

``` cs
// Of course in the real world "Side" can happen, but we're inside a computer.
public enum CoinFlipResult { None = 0, Heads, Tails }
```

# Enums and switches are best friends.
Enums and the newer C# switch statement compliment each other perfectly and I especially love that the compiler will warn if the `_ =>` is not included. Personally I instruct my projects to see this as an error and prevent compilation.

``` cs
public string FlipCoinResultMessage(FlipCoinResult result)
  => result switch
  {
    Heads => "Heads!",
    Tails => "Tails!",

    // This bit here is why i love this switch pattern
    _ => "An error occurred, the digital equivalent of tl",
  };
```

# Structs

Structs are beautiful, immutable, non-nullable data representations. 

``` cs
public readonly struct HtmlThing(int Value)
{

  public static Thing None => default;

}
```

They also have this neato syntax that lets you take a copy with one property different.

``` cs
HtmlThing thing = new (20);
HtmlThing newThing = thing with { Value = 40 };
```

# Structs and switches are best friends

``` cs
// Example of using structs
```

# Structs, Enums and clicky are best friends