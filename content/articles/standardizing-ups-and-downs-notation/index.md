---
title: "Standardizing ups and downs notation"
date: 2024-10-04T00:46:07-07:00
lastmod: 2025-10-21
---

**21 Oct 2025 update: I have now updated the note naming script on GitHub, but the old version is still linked where it was before. The new version can be found [here](https://github.com/tristanbay/microtuning-scripts/blob/main/note-namer.c).**

**27 Sep 2025 update: Kite has decided to deprecate drops and lifts and instead have new symbols for _quip_ (quintuple up) and _quid_ (quintuple down) so that ups and downs work like a tally system, meaning there's no need for the proposed standardization rules below. I'm thinking of updating the script I linked to below but I haven't done it at the time of writing this. I have, however been working on a [Lua module for the Xenharmonic Wiki](https://en.xen.wiki/w/Module:Ups_and_downs_sharpness) with ArrowHead294 which does something similar to that script, but for wiki articles.**
{{< linebreaks 3 >}}
There are different ways to notate microtonal music. For just intonation, there's Helmholtz-Ellis notation, Ben Johnston notation, and of course, the Functional Just system. There is also Sagittal notation which can notate both equal temperaments and just intonation, and things like Maneri-Sims which were made for a specific tuning system (in this case, 72-tone equal temperament).

But what I'm a fan of is Kite Giedraitis's ups and downs notation. It's meant to notate equal divisions of the octave (EDOs), which includes the aforementioned 72-tone equal temperament, or 72edo, as well as rank-2 or "two-dimensional" temperaments such as _porcupine_ and _miracle_. The thing that makes it so great in my opinion is its simplicity and its compatibility with standard musical notation. Deviations from a chain of fifths are notated with ups for going up in pitch and are notated with downs for going down in pitch. Drops and lifts can be added for even smaller pitch increments in the case of large numbers of notes in an octave. Sharps, flats, naturals, clefs, staff lines, etc. all stay the same.

However, I noticed a problem with this system. For those higher EDOs, there's no universally agreed-upon number of scale degrees (edosteps) that an up or a down should represent. For example, 270edo is a very accurate (yet not particularly practical) tuning system where, in theory, one could choose how many scale degrees to assign to an up or a down&mdash;one person might choose to notate an up as 5 steps, but another may choose 6 steps. Furthermore, one person may be used to seeing it one way and read it incorrectly or more inefficiently when they read the work of another composer who another number of steps to an up or a down, even when it's specified at the beginning of the sheet music. I'm aware that such a large number of steps in an octave cannot reasonably be played on basically any existing instrument without some form of major modification, but 270edo is just an example, and it can be loosely followed on fretless instruments when using it as a model of just intonation. This issue can also apply to lower EDOs, which are often more physically playable on musical instruments.

So I wanted to come up with a formula to set the number of edosteps in a "standard" up and down in each EDO, and combine this with other rules to form a "standardized" ups and downs notation system. After discussing my ideas with different microtonalists online, I settled on these rules, which I propose to form the universal standard for ups and downs notation:

- For EDOs 1, 2, 3, 4, and 6:
    * Use an evenly-spaced subset of 12edo/12-tone equal temperament names.
- For EDOs 7, 14, 21, 28, and 35:
    * No sharps and flats are used. This is because the augmented unison is tempered out for these EDOs.
    * Ups and downs represent shifts of 1 edostep, and they are used for the non-natural notes.
- For EDOs 9, 11, 16, and 23:
    * Flats for a letter/nominal are higher in pitch than the sharps for that same letter.
        - This reflects the fact that the augmented unison is mapped to a negative number of steps in these EDOs, since their best approximation of 3/2 is flatter than that of 7edo.
        - For example, D-sharp is lower in pitch than D-flat in these systems.
- For all other EDOs below 66, as well as all EDOs above 66 that support meantone temperament via their closest approximations of 5/4 and 3/2, except for 129edo:
    * Ups and downs represent shifts of 1 edostep.
    * Sharps and flats are used in the standard way.
- For 129edo:
    * Ups and downs are 3 edosteps to preserve the notational structure of 43edo within 129edo.
- For all other EDOs:
    * Ups and downs are the number of edosteps in the interval 81/80 (the syntonic comma) using the closest approximations of 5/4 and 3/2 and _not necessarily_ the closest approximation of 81/80 directly.
        - For example, in 66edo, the closest approximations of 3/2 and 5/4 are sharp and flat respectively, which distorts 81/80 to 3 edosteps, but using the closest approximation of 81/80 directly (which is 1 edostep) means at least either 3/2 or 5/4 must be approximated less accurately so that the intervals add together to get that closer approximation of 81/80.
- Drops and lifts are used whenever ups and downs correspond to an interval larger than 1 edostep, and are themselves always 1 edostep in these situations.

It might seem rather complicated at first, but I actually designed the rules this way so that the intervals that ups and downs correspond to are defined simply with few exceptions. I chose 66edo as a general cutoff point for drops and lifts since 64edo is the last EDO to map 81/80 to a negative number of edosteps using the closest approximations of 3/2 and 5/4, and it wouldn't matter for 65edo since it maps 81/80 to 1 edostep in this way anyway.

I wrote a script in C to generate the note names for different EDOs as a demonstration and implementation of this standard. I did add some other stylization that is optional and based on preference, but handy for certain musical elements and scenarios. This includes half-accidentals for when the number of edosteps in the closest approximation of 3/2 is even, and only having 5 note letters/nominals for EDOs 5, 8, 10, 13, 15, 18, 20, 25, and 30, even though the other nominals could technically be added as alternative names for other nominals or out of their usual ascending note order depending on whether the EDO is divisible by 5 or not. The script can be found [here](https://github.com/tristanbay/microtuning-scripts/blob/main/note-namer_old.c). I may update the script with a feature to show note names without half-accidentals, which would be better for the established system of chord names in ups and downs notation.

I hope you found this article interesting, and if you're a microtonal composer (especially if you work with sheet music and higher EDOs), feel free to share your thoughts and criticism of the rules in this article.
