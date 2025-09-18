---
author: "Callum Sykes"
title: "Advanced Combinatorial Testing in NUnit"
date: "2024-10-17"
description: "A guide on upgrading your testing potential"
tags: ["nunit", "testing", "combinatorial", "c#"]
ShowToc: false
ShowBreadCrumbs: true
---

If you're reading this article, I'm going to assume you have a basic understanding of [combinatorial](https://docs.nunit.org/articles/nunit/writing-tests/attributes/combinatorial.html) and parametrising NUnit tests and now you are pushing up to the limits of this system.

For reference, let's examine a simple Combinatorial Test.

``` c#
[Test, Combinatorial]
public void MyTest(
    [Values(1, 2, 3)] int x,
    [Values("A", "B")] string s)
{
    // ...
}
```

As the parameters must be entered into an attribute, we are limited to constant values, ints, doubles, strings, enums etc.

But what if our tests need to be more complex than that? If we need to compare mutable objects? Maybe we have a collection of "good" elements and "bad" elements we want a set of tests to cycle through. Writing an SVG Editor I have a collection of good SVGs that I know must be valid, and a set of bad SVGs that have bad data and will/should fail gracefully. Every time a user posts an issue about my editor and their svg, I verify the SVG and if it passes the sniff test, add the SVG to the good pile and re-run all of my tests against it.

So how can I maintain a comprehensive set of tests against this set of mutable objects?

# My first attempt, indices

``` c#
[Values(1, 2, 3)] int index
```

By using indices, which are constant, we can index into a list maintained elsewhere. Immediately however, I realised this has a few problems.
- It's not readable, when a new person looks at this test, what does this index mean?
- The intent is obscured, the index relates to a list only mentioned inside the test, likely confusing
- Indices and the list must be maintained separately which WILL fail, if we add too many indices, the failure will be obvious, but if we don't add new ones when we get a new test case, the tests will secretly not test that scenario which is a nightmare

# My next idea, funcs

``` c#
[Values(Element1, Element2, Element3)] Func<MyElement> element
```

This allows us to point at functions that return an element we need, but again, we run up against problems.
- It's hard to maintain, we'll need to create a function for EVERY element, and if we have lots, this is going to get messy.
- Again as before with the lists, how do we maintain our good element set against this?
- Also, unfortunately, these don't count as consts. So they wouldn't work anyway. Sigh.

# My least favourite option, admitting defeat
I hate losing.

``` c#
[Test, Combinatorial]
public void MyTest()
{
  foreach(var element in MyElements)
  {
    foreach(var otherValue in otherValues)
    {
      // ...
    }
  }
}
```
This means 1 test is shown in the test explorer for all the combinations, which makes debugging slow and tiresome, which makes this a bad test, abandoning combinatorial just isn't an option in my mind. So let's refuse defeat, and look at the problem differently.

# Problems so far with every approach
Let's review.
1. Every test has a copy of the inputs, which is impossible to maintain
2. Inputs are limited to what is readable, more than 10 per `[Values()]` gets messy
3. Inputs MUST be constant which is limiting.
4. I want to use combinatorial so I can easily see WHICH combination failed and iterate quickly

# My eventual solution
What I really want, is easy ways to specify a list of mutable objects for each test that requires this combination approach, and sometimes I want to absolutely hammer a test with combinations of variables, that could exceed 1000 combinations, but maintain a readable test and be sure that I haven't missed any weird edge cases. It might be said that what I am making is too complex, but honestly, sometimes it be what it be.

But how can we do this, we tried every _combination_... Or did we?
What about that Values attribute? Does it have some secrets we can gleam?
https://github.com/nunit/nunit/blob/main/src/NUnitFramework/framework/Attributes/ValuesAttribute.cs

``` c#
public class ValuesAttribute...
```

Oh sweet googly moogly the attribute is not sealed!

## Victory is oh so sweet

Learning this I decided I could inherit the attribute and create parameterised named collections. 

``` c#
public class ZoomLevelValuesAttribute : ValuesAttribute
{
  public ZoomLevelValuesAttribute() : base(25, 33, 100, 273, 500) { }
}

public class ToggleValuesAttribute : ValuesAttribute
{
  public ToggleValueAttribute() : base(true, false) { }
}

public class GoodElementsValuesAttribute : ValuesAttribute
{
  public GoodElementsValueAttribute() : base() { this.data = Combinatorials.NewElements().Cast<object>().ToArray(); }
}
```

And now, my tests have never been more powerful, more readable, and more maintainable.
``` c#
[Test, Combinatorial]
public void MyTest(
    [ToggleValues] bool onOff,
    [ZoomLevelValues] int zoom,
    [GoodElementsValues] object myElement)
{
    // ...
}
```