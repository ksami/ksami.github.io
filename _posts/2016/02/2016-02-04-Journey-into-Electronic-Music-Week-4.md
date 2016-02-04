---
layout: post
date: 2016-02-04 21:12:03 +0800
title: Journey into Electronic Music - Week 4
tags: [music]
---

## Subtractor Composition Project
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/M2cBTBLVy7c?rel=0" frameborder="0" allowfullscreen></iframe>

The entire composition was made using Reason's Subtractor for instruments, Ableton's compressor was added onto the pad and bass tracks for the sidechain compressor effect, and a limiter on the master track for increasing gain; no other effects or plug-ins were used.


### Sounds

#### Rhythm
The rhythm sound was inspired by the short, plucked sounds in the beginning of [Avicii - You Make Me](https://www.youtube.com/watch?v=mgpH7-RWbVU). I managed to closely recreate this sound.

![rhythm](/images/2016-02-04-subtractor-rhythm.png)

I used a sawtooth waveform, a low-pass filter to take away the brightness and an ADSR envelope of \|\\_ (0 attack, short delay and 0 sustain) for the short, plucky sound.


#### Bass
I used the bass synth from [Avicii - Levels](https://youtu.be/_ovdm2yX4MA?t=22s) at around 0:22 onwards for the bass sound. I also managed to closely recreate this sound.

![bass](/images/2016-02-04-subtractor-bass.png)

Starting with a sawtooth waveform lowered by 2 octaves from the default 4, I added a low-pass filter and used an ADSR envelope of \|¯\\ (0 attack, full sustain, short release), the short release for simulating reverb. I also added an LFO on the oscillator with a triangle waveform, about 50% rate and 20% amount to give the bass the, for lack of a better description, fart-y sound.


#### Lead
For the lead sound, I used the lead synth from towards the ending of [Avicii - The Days](https://youtu.be/JDglMK9sgIQ?t=3m8s) from around 3:08 onwards. I think I got quite a similar result except for the vibrato effect which, upon listening to the original again now, I should have added in using an LFO.

![lead](/images/2016-02-04-subtractor-lead.png)

I used two sawtooth waveforms, one slightly detuned at +14 cents, one high-pass filter to cut out the unwanted lower frequencies and a second low-pass filter to take away some of the sharpness/brightness. Portamento increased to about 20% and polyphony set to 1.


#### Pad
I was going for a strings ensemble feel but at the same time, did not want something too draggy. I ended up with a cross between strings and an electric keyboard which fits in really well in the composition with the sidechained compressor acting on it.

![pad](/images/2016-02-04-subtractor-pad.png)

I used two sawtooth waveforms, one detuned by -7 cents and the other detuned by +13 cents, mix at about 40%, polyphony at 8 with MIDI chords for the chorus effect. I also used a low-pass filter to cut off the brightness and an ADSR envelope of /\`-\\ (medium attack, short delay, 40% sustain, long release)


#### Kick
I used a kick for the first non-pitched sound. Since the frequency is so low, I had trouble trying to make it sound louder and punchier on my earphones using just the tools in Subtractor. In the end, I raised the frequency slightly for a greater perceived loudness; but overall it still felt like a good kick.

![kick](/images/2016-02-04-subtractor-kick.png)

I used one sine waveform with keyboard tracking off, pitched at 1 octave and 4 semitones, and an ADSR envelope of /\\_ (short attack, short delay, 0 sustain), using the delay parameter to control how punchy the kick is.


#### Clap
I tried to make a closed hi-hat sound for the second non-pitched sound at first. However, I could not get the initial stick-on-hi-hat sound so I changed it to a clap instead. I couldn't get that right either, ending up with something that sounds more like a short pfft of air which, honestly, could be better.

![clap](/images/2016-02-04-subtractor-clap.png)

I used only white noise, a high-pass filter to cut out the lower frequencies and with a slight response to give a more metallic sound, and an ADSR envelope of /\\_ (short attack, short delay, 0 sustain).