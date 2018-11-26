# Workshop
Hi. In this workshop, EJ and I are going to show you how to use gen~ in the context of Max. We'll also cover using gen~ to export code from Max, and how to use JUCE to run that code in a VST or Audio Unit.

## Getting started
Getting started is super easy. All you need to do is create a gen~ object. This is just like a patcher object, in that double-clicking on the object takes you to a subpatch. Here though, we're in the gen-dsp context, which has a different set of objects from regular Max. So, let's start with something very simple: multiplying two signals together.

*picture of multiplying two signals together*

Easy. Rather than multiplying by a signal we supply externally, we might want to multiply the signal by a constant parameter. We can do this by creating a parameter. Parameters are analogous to parameters in the context of a VST or Audio Unit. In fact, it's common to assign one VST/AU param to each gen~ param.

*picture of multiplying a signal by a param*

It's handy that these parameters can be named, and that we can use this name as an argument to a multiply object. Note that when you use a multiply like this, its other inlet disappears. This is a difference between the Max and gen domains.

## Exporting to a VST

So we haven't done very much, but already we've done enough that we should be able to create a VST or Audio Unit using the code exported from this object. This is the part where we follow the steps outlined in the README. The tricky bit, at least on my version of Mac OS, is getting the Audio Unit recognized. But there are a couple fiddly things you can do to make it work.

## Karplus-Strong

About the very simplest thing that we can do with gen~ that we can't do with plain old Max is Karplus-Strong. If you're not familiar with the algorithm, the idea is simply to take a short delay line with a high resonance. Karplus-Strong is a special case of waveguide string synthesis. It's kind of fun to see how different interpolations affect the timbre of the sound.

*Build the dirt-simple Karplus-Strong*

