---
author: "Clicky"
title: "Coding Style"
date: "2024-11-08"
description: "How I like to code in c#"
tags: ["c#", "coding", "style"]
ShowToc: false
ShowBreadCrumbs: true
draft: true
---

I've been coding in C# now for around 8 years and I've slowly developed my own opinionated coding style. This isn't about indents, tabs vs spaces or editors, it's about about code structure, data and control flow. A note, not all of the code will compile, some items are simplified for brevity, and many of these concepts can apply to many languages. Some languages even make use of these concepts 


# #1. [Null is Shit.](../null-is-shit)
Avoid Null at all costs, it can only ever cause NullReferenceExceptions and problems. Null is a useless value to have in any situation. Never design null into your code flow or data types.

Use `#nullable enable` in your files and projects to catch nulls from outside your code.


# #2. Functions should only ever return void, bool or non-nullables
See [#1](#1-null-is-shit) regarding returning anything nullable, i.e a class, or record, or item that may be null. If something is null we don't want to even _think_ about using it. In which case, we should use a `TryGet` function that returns a bool with an out in the arguments. (Alternatively [Option/Result](../null-is-shit/#options-and-results) are also excellent)

Using `TryGet` with non-nullables is fine too! However returning non-nullables is always safe, see the `Add` function as an example, there's no real need for a TryGet because whatever result we get will not crash our program. And if we want to check or perform validation, that is often better outside of the Add method. For example, a calculation method may return `NaN`.

Returning void is fine, sometimes there isn't a useful returnable result. Void explicitly states that the success of the method cannot, or does not need to be determined, returning null or always returning true or false is worse than having no return.


``` C#
public void MyFunctionThatCantReturnStuff();

public bool TryGetData(MyInput input, out MyOutput result);

public DateTime GetTheTimeInSkopje();

public int Add(int a, int b);
```


# #3. Inheritance should be avoided
The only place I ever _prefer_ inheritance is when creating a class that abstracts things to keep life simpler for external developers who need to create things in my program, I think Grasshopper is an excellent example of this, inheriting the GH_Component allows for keeping the class declaration simple but also lets you go complex as necessary, it could be done as an interface, but I think inheritance works in that case. Internally I very rarely do this.


# #4. Switch statements encourage good flow
C# introduced a very nice style of switch statements which read very well and ensure every case is handled. Rust also uses them very nicely. They allow for failing by default, which is a very safe route in case a new Enum is created

``` c#
Validity result = ValidateResult(userName);
string message = result switch
{
  Validity.Empty => "Username is empty, please enter something",
  Validity.Valid => "Username is valid",
  Validity.Taken => "Username is already taken",
  Validity.HasSpace => "Username cannot have a space in it",
  Validity.HasNumbers => "Username cannot contain Numbers",

  _ => "Username is not valid, please try something else",
};
```
This switch statement guarantees no errors are let through and every eventuality is considered. It is very easy to add new cases as they arise to give better messages to users, it is very easy to see what the result is. 


# # 5. Never. Throw. Exceptions
Using exceptions in code is like using a 150 decibel alarm for your egg timer, it's completely overkill. The only time you should use an exception is when you want to crash your program. Using them intentionally along with try/catch is cursed, you may as well use GO-TO statements.

That being said, I do like _using_ exceptions! They are lots of excellent ways to use exceptions in control flow. But throwing them ruins control flow.

## Improving Exceptions
Here is how I like to wrap external libraries to avoid issues with Exceptions, this guarantees my program will never crash due to something happening I haven't planned on handling, and I can actually use the exceptions to my advantage to return useful error messages to a user, or take different control routes.

``` c#
public async Task<bool> TryConnectToServer(string url, out string errorMessage)
{
  try
  {
    var client = new HttpClient(url);
    var result = await client.ConnectAsync();
    return true;
  }
  catch(Exception ex)
  {
    errorMessage = ex switch
    {
        HubException => "Data Could Not Be Sent",
        WebSocketException => "Server Indicated Possible Closure",
        OperationCanceledException => "Server Closed Unexpectedly",

        _ => ex.Message,
    }
  }
  
  return false;
}
```


# #6. Use Record Structs and enums plentifully
Record structs offer very quick and convenient ways of creating small data structures which improve readability, offer alternatives to tuples and make life simpler. I rarely use Record Structs for public data types, but I will use records or structs for public data types, mostly because if it's public, it needs more functions and options.

``` c#
private record struct DumbColour(int )
```


# #7. Prefer immutability
Mutability can quite easily make intent and data flow confusing. Not only that but 


# #8. Closures are cool.
Talk about actions and funcs.


# Summary
The result of using all of the above principles means 