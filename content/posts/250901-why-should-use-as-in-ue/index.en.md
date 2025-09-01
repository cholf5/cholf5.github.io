---
author: 'Cholf'
title: 'Why you should use Angelscript in Unreal Engine'
date: '2025-09-01'
tags:
- angelscript
- unreal-engine
- game-development
---

> This article originally originated from a discussion on Twitter, but I decided it was too short for the full article, so I organized it into a single post.

I've recently been learning Unreal Engine (UE) and experimenting with some small projects. UE is an incredibly powerful and feature-rich game engine that can meet almost any game development need, but it has two major drawbacks that have held me back: C++ and Blueprints.

C++ is too complex and a bit too heavy for writing game logic, and it is a bit annoying to recompile it all the time.

The advantage of blueprints is that they are friendly to non-programmers, but they are almost all disadvantages for programmers:

1. Blueprint files are binary and cannot be diffed using version control system (git/p4/svn/etc...)
2. You cannot enjoy the benefits of automatic code generation brought by Copilot
3. Dragging blueprints is extremely inefficient (the better the programmer, the more they hate graphical interfaces, and blueprints can be said to be the exact opposite of these people)
4. It is easy to make spaghetti and difficult to maintain

C++ is too complicated, blueprints are too simple, and UE has always lacked a good intermediate language like C# used by Unity. It is said that UE6 will have Verse scripting language, which is used to solve this pain point, but after looking at the syntax, I don’t like it very much.

[Angelscript](https://angelscript.hazelight.se/) (hereinafter referred to as AS) solves the above problems very well, and has passed the success of It Takes Two, proving that it is capable of developing any large-scale game.

The following are some advantages I felt during my learning and using AS:

1. The syntax is close to C++, and almost no additional learning is required, and it can be used right away
2. All annoying features in C++ are abandoned, such as the need to include header files all the time, recompile at every turn, and tons of boilerplate code
3. Hot reload, no need to recompile, the effect can be seen immediately in the editor after the change, including adding new classes/enumerations/components, etc., which are also effective immediately
4. The syntax is simple but not crude, and it has everything it should have
6. The Binding of UE is done very well and is intuitive. Although the documentation is incomplete, there are [example projects](https://github.com/Hazelight/UnrealEngine-Angelscript/tree/angelscript-master/Script-Examples) for reference (I plan to release a set of tutorials in the future)
7. To do the same function, it may take only half the time of C++ or even less. I followed a course, the teacher used C++ and blueprints, and I used AS. He took 4 hours of work, and I could finish it in 1 hour with AS

Because of its powerful and complete functions and its current popularity in the game industry, I have wanted to learn UE more than once in the past few years, but I was discouraged by C++ and blueprints. It's not that I can't learn C++, on the contrary, C/C++ is my enlightenment language. I taught myself C language in high school, and I have been using C++ for the first few years after work, so C++ is not an obstacle for me, I just think it is inappropriate to appear here. And blueprints are completely anti-human things for me. For specific principles, see the 4 points mentioned above.

Until I came into contact with AS, it felt like a fish in water, and finally there was a language worthy of UE. This gave me a lot of learning pleasure while using AS to learn UE, because there was a sense of pleasure of dimensionality reduction. Watching others work hard to make an inconspicuous function with C++ and blueprints, while you can get it done in a few strokes with AS, the feeling is really indescribable.

Of course, there are other languages ​​to choose from, such as:
- C#, [USharp](https://github.com/pixeltris/USharp)
- Lua, [UnLua](https://github.com/Tencent/UnLua) and [sluaunreal](https://github.com/Tencent/sluaunreal)
- TypeScript/JavaScript, [puerts](https://github.com/Tencent/puerts)

Why did I choose AS?
- USharp has stopped maintenance
- Lua and ts/js weak types are not considered
- AS is still actively maintained, extremely easy to use and sufficient

UE6's Verse is worth looking forward to. If the integration is particularly easy to use, the ugly syntax is not unacceptable, but before that, AS is still the best choice.
