---
layout: post
title:  "How to Render an Ocean - Tessendorf Ocean"
comments: false
use_math: true
---

It's been awhile since I last wrote a blog on computer graphics.
Ever since I graduated from university, it's been a tough time finding a job and getting used to the new routine.
Now that I'm settled and feel stable, I'll start writing again!

Recently I've been working on a cool stuff.
A new Vulkan rendering engine project [mirinae](https://github.com/SausageTaste/mirinae) is here, and [Dalbaragi Engine](https://github.com/SausageTaste/dalbaragi) has become history.
It was total rewrite and I think it was worth it.

Check out some visuals.

<p><div style='position:relative; padding-bottom:calc(56.25% + 44px)'>
    <iframe
        scrolling='no'
        width='100%'
        height='100%'
        style='position:absolute;top:0;left:0;'
        src="https://www.youtube.com/embed/CfL3AIlM-_0"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
    ></iframe>
</div></p>

Many thanks to ThinMatrix for the cute player character model!

Anyways, this is the Tessendorf ocean that I've been working on for weeks.
You can adjust various ocean parameters and they take effect immediately, in real time.

There are some flaws, thought.
For instance, the water surface lighting is not correct at the moment.
On top of that I still can't figure out how to correctly handle foam calculation.
There are still some problems I need to solve.

However, I've learned a lot in the process of implementing it, and I thought I'd share it.
From Vulkan render pass management to compute shader, and mathematics.

Before we start, please watch a YouTube video *Ocean waves simulation with Fast Fourier transform* by Jump Trajectory.
This is the very video that inspired me to do it, and core concepts are elegantly explained and visualized in the video.
Even if you are not a mathematical person, you would still enjoy it.

<p><div style='position:relative; padding-bottom:calc(56.25% + 44px)'>
    <iframe
        scrolling='no'
        width='100%'
        height='100%'
        style='position:absolute;top:0;left:0;'
        src="https://www.youtube.com/embed/kGEqaX4Y4bQ"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
    ></iframe>
</div></p>

Do you feel like digging in much deeper?
Alright let's go!

We'll learn how ocean surface generation was implemented.
Ocean surface lighting, buoyancy are not covered here, at least for now because I'm still studying these.

# Ocean Spectrum

If you watched the video by Jump Trajectory, then you'd understand ocean wave can be described as a sum of many sine waves.
It's actually true.
Check out Acerola's first ocean renderer video where he actually used sum of sines to make ocean surface.
But the problem with this technique is that it's inevitably slow.

[Tessendorf's paper](https://people.computing.clemson.edu/~jtessen/reports/papers_files/coursenotes2004.pdf) demonstrates you can use Fourier transform to calculate ocean waves much faster.
Ok, before we dive into this topic, please first watch Fourier transform series by 3Blue1Brown.
This is my far the best explanation for it, and had I missed it I would never be able to truly understand the whole ocean rendering algorithm.

It's ok if you don't fully understand the computation of it.
The importance concepts are *time domain* and *frequency domain*.

# Fourier Transform

# Fast Fourier Transform on GPU

# Tessellation
