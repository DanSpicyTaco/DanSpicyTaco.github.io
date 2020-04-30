---
categories: [C#]
excerpt: "C# - The Basics"
toc: true
# header:
#   overlay_image: /assets/images/posts/distributed/communication_header.jpg
#   caption: "Photo Credit: [technologymoon](https://technologymoon.com/wp-content/uploads/2019/12/Canva-Share-and-send-media-files-between-phones.-Paper-planes-letter-sent-from-phone-to-phone-technology-linking-people-1536x1025.jpg)"
---

So I've decided to learn C# for a graduate role I'm hoping to get.
The position starts in a year so there's plenty of time to gain experience in it!
My plan is to:

1. Understand the basics of the language - function/class structure, best practices for variables, etc.
2. Learn the common data structures and the pitfalls of using each in C#.
3. Practice coding questions on LeetCode.
4. Do some fun UI projects - let's plan that for later.

## The Basics

To learn the basics, I'm using the [Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/csharp/) on C#.
C# is a part of the C family - which includes C, C++, Java and JavaScript.
It supports OOP, but also **component-orientated programming (COP)** - which is the ability to create self-contained packages.
COP allows you to swap these packages with ease, provided the overall functionality is the same.
A package usually means an assembly file (`.dll`) - a compiled version of the program.
A program will have one or more **namespaces** in it, which are groups of classes.

C# includes a bunch of features too:

- **Garbage collection**: automatically reclaims memory occupied by unused objects.
  In other words, if an object hasn't been used for a while, C# will delete it for you automatically.
- **Exception handling**: provides a structured and extensible approach to error detection and recovery.
- **Type-safe**: prevents a user from reading uninitialised variables, index arrays beyond their bounds, or perform unchecked type casts.
- **Unified type system**: all variable types (`int`, `double`, etc.) come from a single root `object` type.
  Therefore, all types share a set of common operations, which makes it easy to share.

## Data Structures

### Dictionary

```C#
Dictionary<TKey, TValue>
```

|         Method          | Time Complexity |
| :---------------------: | :-------------: |
|   `Add(TKey, TValue)`   |      O(1)       |
|   `ContainsKey(TKey)`   |      O(1)       |
| `ContainsValue(TValue)` |      O(1)       |

### HashSet

Similar to a dictionary, but useful for existence checking.

```C#
Dictionary<T>
```

|    Method     | Time Complexity |
| :-----------: | :-------------: |
|   `Add(T)`    |      O(1)       |
| `Contains(T)` |      O(1)       |
