---
author: "clicky"
title: "Null is Shit"
date: "2024-10-17"
description: "It's so shit"
tags: ["Oh", "God", "null", "is", "agh,", "it's", "so", "shit"]
ShowToc: false
ShowBreadCrumbs: true
draft: true
---

I find programming quite beautiful, writing good code, good data and logic flow is enchanting to me.

If you've ever watched Severance (no spoilers) the crew work on little computers in which they refine data entirely by how it makes them _feel_.

![Macro Data Refinement](/images/macro-data-refinement.png)

As daft as looking at a bunch of numbers on a screen and sorting them by vibe sounds I'd definitely liken it to my coding style. Good code _feels_ comfy, reassuring and trustworthy. Bad code _feels_ unnerving, concerning, sometimes even anxiety inducing as your brain cannot walk through the control flow, ok code feels, also ok. It feels quite meh and I know in earnest I'll likely return on another sunnier day to turn it into good code.

I really like working in a space with immutable structures, enums, switches, functions and closures. Likely because whilst this doesn't guarantee good code its a happy space for me I feel warm and fuzzy working with such trustworthy types that aren't going to throw surprises at me now or later.

Null shits all over this wonderful vibe like a racist Uncle at a Barbecue pervasively entering every conversation to interrupt with their unwelcome thoughts on Gay Marriage and Gender Identity.

In this analogy the efficient, enjoyable readable flow of the code is interrupted by having to double check everything, litter my code with `?`'s, `??`'s, try catches, and all the extra effort that comes with being forced to converse with a drunk idiot.

# Null

Null exists to represent that a value is not anything. This is theoretically useful, but there are a many critical issue with its general design.

### 1. I can still interact with null and I will be punished for doing so

In the case below, the program will attempt to Crash because it doesn't know what else to do, which is fair! We can't carry on without explicitly doing PerformTask or handling its failure.

``` cs
MyClass myClass = default;
myClass.PerformTask(); // <-- NullReferenceException
```

### 2. Null is hidden

In the above example, myClass is explicitly declared as null, but there are many times null was not intended and there's no way to know _for sure_ if a value will or will not be null so we need to remain constantly vigilant and eternally paranoid.

If instead myClass had a default (non null) value the `PerformTask()` method could still be run and return a failing value, using structs allows for exactly this.

``` cs
MyStruct myStruct = default;
myStruct.PerformTask();
```

### 3. Null is too generic

Null is the default nothing value for all reference types which isn't very useful, take this example.

``` cs
public bool MyFunction(IInterface thing)
{
  return thing switch
  {
    _ => false
  }
}
```

If thing is null, there's no way for me to know what it was supposed to be outside of the interface.
// This needs a better example

It would be much better if every class could inherit a kind of `INull` interface and offer a fallback value instead I'd much prefer this, and generally if I can, I do implement an explicit 


--> other article stuff

I would really really like to avoid this

``` cs
if (value is null)
{

}
```

this

``` cs
var result = value?.Property ?? defaultValue;
```

and this

``` cs
try
{
  // if arg is null the constructor throws a NullReferenceException
  var instance = new MyType(arg);
}
catch {}
```

## Nothing

A design note here, this isn't to say I always think a value should be "something", nothing is a very very useful state for a value

### a?.b ?? c
If the option was to have or not have `?` and `??` I'd vote to keep them, they're better than try catches  long form null checks everywhere. This example below is something I have to do every day and every time I have to add another one to fix a NullReferenceException I scream internally.

``` cs
public string MyFunction(object data1)
{
  string myValue = data1?.Property1?.Property2?.Name ?? string.Empty;
  
  if (string.IsNullOrEmpty(myValue))
    return "Empty";

  return myValue;
}
```

### Fixing nulls with exceptions

``` cs
public void MyFunction(object data1, object data2)
{
  if (data1 is null) throw new NullArgumentException(nameof(data1))
  // ...
}
```
I see this sort of thing A LOT. And I do NOT understand why. Thank you so much for resolving my null input by trying to crash my program. Bonus points if this exception is not documented in the doc strings. Returning a false is much safer and gives me a chance to handle it myself.

``` cs
public bool MyFunction(object data1, object data2)
{
  if (data1 is null) return false;
  // ...
}
```

### Not Sure

This function is already a disaster. The only 3 return options are an object, null or an exception. Hoo-.rah, and regardless of which it is, I need to check every possible result.

``` cs
public object MyFunction(object data1, object data2)
```

I'm a big proponent of the TrySomething pattern, as with above, any failures inside the method can return false.

``` cs
public bool TryMyFunction(object data1, object data2, out object result);
```

Most importantly, the function can return this phrase which is something I'm very fond of as now I can ALWAYS trust this method to return something usable or give me a false.
``` cs
return result is not null;
```

### Landmine Constructors

This constructor is like biting into a jam donut with a bee in it.

``` cs
public class MyClass
{
  public MyClass(object input1)
  {
    if (input1 is null) throw new NullArgumentException(nameof(input1))
  }
}
```

If it is SO important that the input is not null, why did you allow for it in your design? If I'm not supposed to come to your birthday party without a present, tell me in advance, don't unload a shotgun in my face when I walk through the door empty handed.

``` cs
public class MyClass
{
  private MyClass() {}

  public static bool TryCreate(object input1, out MyClass myClass)
  {
    myClass = new();
    if (input1 is null) return false;
    myClass.Input1 = input1;
    return true;
  }
}
```

I don't really like constructors for the reasons above, they're not a great pattern. Instead a TryCreate is much safer.

### The resulting Try Pattern everywhere

The result of all of this is I use this pattern every day everywhere. This is _kind of_ ok. As long as I don't need to do async, certain closures, parallelisation, and a few other scenarios.

But it still feels shit. Why do I have to do this everywhere just to be sure I won't stub my toe?

``` cs
public bool TryGetThing(object data, out object thing);
```

# Solutions

## Immutability

Using value types such as structs and enums can prevent the possibility of null entering our code.

``` cs
public enum Result { None = 0, Success, Failure }

public Result TryDoThing(int number); // <-- Readable and safe!!
```

``` cs
public readonly record struct Point(double X, double Y)
{
  public static Point Empty => new(double.NaN, double.NaN);
}

public Point GetHighestPoint(IEnumerable<Point> points)
{
  if (points is null || points.Length <= 0) return Point.Empty;
  // code
  return highestPoint;
}
```

The only issue of course is structs and immutability may not be the best approach.
Imagine a 10Mb Bitmap, that would be an awful struct.

## Option<T>
Rust doesn't have null, which is lovely. Instead it uses `Option<T>` and `Result<T>`.
C# doesn't have these natively, but they're not too hard to include.

This style of coding will prevent nulls, exceptions and heartache.

Imagine a Thanksgiving where your intolerant relatives just weren't invited? Doesn't that sound simpler, nicer and also leave more Turkey for everyone?