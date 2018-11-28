[multiplying-signals]: 			../img/multiplying-signals.png
[simplistic-karplus]: 			../img/simplistic-karplus.png
[simplistic-karplus-parent]: 	../img/simplistic-karplus-parent.png
[amp-mult]:			 			../img/amp-mult.png
[onepole-filter]:			 	../img/onepole-filter.png
[setparam]:			 			../img/setparam.png
[stereo-filter]:				../img/stereo-filter.png

# Workshop
Hi. In this workshop, EJ and I are going to show you how to use gen~ in the context of Max. We'll also cover using gen~ to export code from Max, and how to use JUCE to run that code in a VST or Audio Unit.

## Getting started
Getting started is super easy. All you need to do is create a gen~ object. This is just like a patcher object, in that double-clicking on the object takes you to a subpatch. Here though, we're in the gen-dsp context, which has a different set of objects from regular Max. So, let's start with something very simple: multiplying two signals together.

![Multiplying Signals][multiplying-signals]

Easy. Rather than multiplying by a signal we supply externally, we might want to multiply the signal by a constant parameter. We can do this by creating a parameter. Parameters are analogous to parameters in the context of a VST or Audio Unit. In fact, it's common to assign one VST/AU param to each gen~ param.

![Multiplying by a parameter][amp-mult]

It's handy that these parameters can be named, and that we can use this name as an argument to a multiply object. Note that when you use a multiply like this, its other inlet disappears. This is a difference between the Max and gen domains.

## Karplus-Strong

About the very simplest thing that we can do with gen~ that we can't do with plain old Max is Karplus-Strong. If you're not familiar with the algorithm, the idea is simply to take a short delay line with a high resonance. Karplus-Strong is a special case of waveguide string synthesis. It's kind of fun to see how different interpolations affect the timbre of the sound.

![A very simplistic Karplus-Strong implementation][simplistic-karplus]

To make this patcher play sound, we can use a noise burst provided by the parent Max patcher.

![The parent patcher supplies a noise burst][simplistic-karplus-parent]

## One-pole filter

Another very easy thing to build in gen~ is a simple one-pole filter. We have to do a bit of math to convert from an expression of cutoff frequency in hertz to one appropriate for our filter. With this done, it's also very easy to get the highpass filter out.

![Picture of the dirt-simple one pole filter][onepole-filter]

It's not the most sophisticated DSP in the whole world, but with this done we've got enough to try exporting this from max. So the first thing to do is to try to do exactly that. See the README for this repository for directions on exporting a VST/AU.

There's a bit of setup to do this the first time, but once it's done we shouldn't need to do it again.

## Gen subpatcher and stereo effects

One thing that's a bit painful about our current effect: we only process one channel. But, using this patcher as an abstraction, it's very easy to make a stereo effect.

1. Use save-as to save the gen subpatcher somewhere. You can call it gen-onepole or something.
2. Notice that this saves as a .gendsp file. This file cannot be opened directly, but it can be loaded into a gen~ object.
3. Create and save a new Max patcher. Create a gen~ object and open the gen~ subpatcher.
4. Now, drop the .gendsp file into this parent gen subpatcher. We've successfully added gen to gen.

There's one tiny snag here, which is that we had a parameter named cutoff in our gen patcher. At the current moment, there's no automatic way to have params from the child gen~ to bubble up to the parent. We can use the setparam gen~ object to contextualize the signal for the child patcher.

![How to use setparam to great effect][setparam]

Great, now we're ready to create a new top-level patcher that uses this gen abstraction twice, one for each channel. In this way, it's easy to create a stereo effect.

![Picture of a top-level patcher, using a gen~ object that processes a two sound channels in stereo][stereo-filter]

If we want to, we can now export this effect again. It's much easier this time around—all we have to do is export the code and rebuild the Xcode project. Now host applications can use this stereo effect.