# How I started programming

It's been like 6 years since I started game engine programming.
I started it by learning PyGame and PyOpenGL.
I remember in those days I spent weeks to figure out how to draw a simple rectangle.
I was so excited when I finally managed to draw a rectangle with a bitmap texture on it.

I had a lot of fun with Python and OpenGL.
These are 2 most remarkable ones.

* [practiceOpenGL-FirstPerson](https://github.com/SausageTaste/practiceOpenGL-FirstPerson)
* [escapeRoom](https://github.com/SausageTaste/escapeRoom)

Second one is newer and more profound, so you might be interested in it.
It has a basic map loading system, and a console panel.

While I enjoyed programming in Python, I startetd think, I might as well try C++.
Because that's the de facto standard programming language in game development world.

Before that, I was just a university student who majors economics.
My degree had nothing to do with software development.
But ever since I started to dig in C++, my dream have gotten to be a serious software engineer.

Here are the source codes for two C++ game engine projects.

* [Little-Ruler](https://github.com/SausageTaste/Little-Ruler), the OpenGL project
* [Dalbaragi](https://github.com/SausageTaste/Dalbaragi), the Vulkan project

# The OpenGL Projects

It is a cross platform game engine.
It supports Windows, Linux, and Android.
It's written in C++, using OpenGL for rendering,

This is my first project that the number of code lines exceeded 20,000.
The program architecture might not so beautiful and the performance is pretty bad, espacially on Android devices.
But I'm certain I've learned a lot of stuff while working on this project.
This post is summarizing it.

# Cross Platform

When I first started `Little-Ruler` project, supporting Android platform was very important.
Because under a law of South Korea, your game must be distributed via authorized 'open markets' to avoid examination expenditure.
Those 'open markets' include Google Play Store.
So if I had any plan to release a real game on a market, I had no other choice but releasing on Android platform.
Later I found out Epic Games Store was also an open market but that's another story.

Anyways, that's why I decided to spent remakrable time and efforts to study NDK build system.

Basically Android uses Java for building applications.
But when you need to integrate C/C++ into your project, you can use JNI feature to call C functions in Java code.
You use NDK to build your C/C++ project and produce a `.so` file, to which JNI can link.

It wan't a easy to learn the toolset because it wasn't a common practice.
Most mobile game developers just use Unity engine or something.
NDK user pool was small, so support from Google was not so good, and the lack of tutorials/resources was killing me.
It was all on my own.
If I failed, the project stopped.

Sometimes I was like "Screw this. I'm done." and did something else for days, or for weeks.
Android Studio has always been my biggest cancer since then.
Even Vulkan doesn't compare.

But the peculier thing is that, after some refreshing breaks, I always come up with solutions, or more precisely, run into ones.
Maybe Google fixed it, or I was just too frustrated to focus on finding flaws.
Either ways, I always solved problems, and it was one of the most satisfying moments.

Everything I've learned from the process is still used in `Dalbaragi` project.
At this point I'm fairly comfortable with NDK build system.
Though I have some more challanges such as Android App Bundles, and supporting sound and network on mobile devices.
I belive these are just matters of time.

# In-house Model Format

There are some skeletal animation tutorials on the internet.
YouTube series by ThinMatrix was really great, as well as some good blog posts.
But what I really didn't like was they used Collada file format.

It's not bad.
The model files these tutorials provided with was working great with it.
I could see the correctly animated scenes.

But when I tried to import my own animted models from blender to my game engine, it didn't work out so well.
Weirdly streched limbs, some body parts being sucked into world space origin point, character lying on the ground...
Bugs I had to deal with.

But I didn't understand how Collada file stores animations, how they implement the animation system in the tutorials.
Of course, because I just copy-pasted everything!

It was another moment when I ceased the project about a month.
Such a frustrating moment.

I thought maybe I could study basic linear algebra and build the entire animation system by myself.
During the 1 month breaking time, I never opened Visual Studio.
Only a pen and a paper, and a book of math.
It's when I came up with a brilliant idea that helped understanding space transformation.

# Map Building With Blender

# Text Rendering

# Collision Resolve

# Vulkan

# Planar Reflection

# Portal
