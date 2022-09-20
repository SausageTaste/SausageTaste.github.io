Reflections are beautiful.
Water surface so bright with sunshine breaking into fragments.

Think about the RTX hype among core gamers.
They were excited how the new technology could vastly improve reflections on every surface.
You can see tanks, soldiers, helocopters moving around through puddle reflection.
Your rifle scope finally recieves proper reflection though which you can observe enemise approaching behind.

RTX didn't bring about significant paradigm changes as to how games were played.

Reflection with ray tracing is perfect but with cost.
RTX graphic cards are expensive, and even with it, it's hard to maintain good enough framerate.

Now look at this.

> Half Life 2 water demonstration

It's Half-Life 2, which was released in 2004.
As you can see, the reflection is as dynamic and as vivid as ray traced ones.
When an object moves, the reflected image also changes accordingly, in real time.
It can't be that Valve in the past somehow managed to implement ray tracing reflection that can run on old potato machines.

However,
While the water refleciton is nearly perfect, it is rather embarassing to see other materials with shininess.
Look at the tile floor.
The reflection is not even accurately positioned on the floor.
The reflection image follows your view, just like how moon always stays same spot on the sky dome.

Why didn't they just use the same reflection techonoly for the floor?
It seems the way Valve implemented it's water reflection, has some kind of limitations.
We will study how this kind of reflections were implemented, and learn why the technology is not used these days.

Let's dig in to the world of *Planar Reflection*

# Variety examples of planar reflections

Along with Half-Life 2, there are many other games that uses this technique.

This is from Grand Theft Auto V.
You can find mirrors with perfect reflections in cloth stores arorund Los Santos.
And if you look close, the quality of the reflection image is not as good as the real world counterpart.
You will learn soon why it is the case.

Also the ocearn surface reflection is also implemented with planar reflection.
You can tell it by observing who the reflection image is not fading off as you lower you view direction.

# The idea

Let's start with the most simple case.
Here's a image of a water with a vibrant reflection.
This is how real world reflection exactly works.

Observe the reflection image carefully and you'll notice that the image is excalty upside-down version of the real world counterpart.
We are know it and take as granted, but it's a too familiar concept that it's easy to underappreciate it.
Actually, the fact that any reflection images are just flipped version of real world is the most important starting point.
