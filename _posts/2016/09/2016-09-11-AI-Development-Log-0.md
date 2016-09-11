---
layout: post
date: 2016-09-11 20:20:18
title: AI Development Log 0
tags: [ai, javascript, programming]
---

## Introduction
The goal is to develop an Artificial Intelligence (AI) for a Space Invaders style shooter game which mimics the actions and decisions of a human player.

This development log will chronicle my thought processes while developing the algorithm; and, in an effort to better understand them, hopefully tread the same path others took when developing modern machine learning and neural network algorithms.

_Note:_ Requires some understanding of probability and code to read. Code examples are given in excessively verbose plain Javascript or [pseudocode](https://en.wikipedia.org/wiki/Pseudocode) for higher level concepts.


## The Game
![game](/images/2016-09-11-game.png)

The AI (pictured in orange) vs the player (green). Points are scored whenever a player's projectile collides with the opponent; the more points the better.

The [Phaser](http://phaser.io) framework is used for the game.

A traditional game loop structure provided by Phaser is used to run the game 60 times a second and the AI makes a decision every `x` number of rounds of the game loop.


## The AI
There are three actions the AI can take at every round:

1. Move to the left
2. Move to the right
3. Fire projectile

_The decision of which action to take at each round makes up the AI._

Every improvement to the AI will be to make a more informed decision on which action to take and when.


## Most Basic Algorithm (MBA)
The most basic algorithm (MBA) I started with was to use simple probability to decide which action the AI should take; because what can be easier than making an absolutely random choice?

For the most basic algorithm (MBA), I divided the probabilities equally and assigned 33% to each of the actions the AI can take. (Yes, yes I know I should have assigned 33.333...% but sadly my computer cannot handle infinite precision.)

The actual action that is taken is left to the Random Number Generator (RNG) which spits out a number within a range ([0, 100) in this case for total probability with everything in percentages).

That number is checked against the 3 ranges [0, 33), [33, 66), [66, 100), and the respective action is taken based on which range the number falls into, as demonstrated in the code below.

```javascript
var PROB_MOVE_LEFT = 33;
var PROB_MOVE_RIGHT = 33;
var PROB_FIRE = 33;

// Math.random() returns a random number between 0 and 1 (exclusive)
var randomNumber = Math.random() * (PROB_MOVE_LEFT + PROB_MOVE_RIGHT + PROB_FIRE);

// Get the actual ranges to check
var probabilityMoveLeft = PROB_MOVE_LEFT;
var probabilityMoveRight = PROB_MOVE_LEFT + PROB_MOVE_RIGHT;

if (randomNumber < probabilityMoveLeft) {
    // Move left
} else if (randomNumber < probabilityMoveRight) {
    // Move right
} else {
    // Fire
}
```

This AI mimics the actions of an absolutely random<sup>[0](#0)</sup> player who gives no thought whatsoever to their decisions.

<sup><a name="0">0</a></sup>`Math.random()` is technically considered a pseudo-random generator in that the numbers it outputs is not [truly random](https://en.wikipedia.org/wiki/Random_number_generation#.22True.22_vs._pseudo-random_numbers) and should definitely not be used for any kind of sound cryptography. It is however random enough for our purposes.


## State
To improve on the most basic algorithm (MBA) and make the AI give a bit of thought to its actions, I checked the state of the AI at every round.

State of the AI here refers to the AI is on the game world and is generally a true/false (Boolean) condition.

For now, 2 states are considered for the AI which answer these 2 questions, namely:

1. Is AI at right bounds?
2. Is AI at left bounds?

Why are these 2 states important? Because they limit the available actions the AI can take at each round.

If the AI is at the right bounds of the game world, choosing to move to the right at this point would be a wasteful decision as the AI cannot move to the right any more. And similarly for the left bounds.

Therefore, I added a check to the decision process to remove wasteful actions such that the algorithm goes something like this:

```javascript
var PROB_MOVE_LEFT = 33;
var PROB_MOVE_RIGHT = 33;
var PROB_FIRE = 33;

// Returns random number between 0 and (33+33+33)
var randomNumber = Math.random() * (PROB_MOVE_LEFT + PROB_MOVE_RIGHT + PROB_FIRE);

// Get the actual ranges to check
var probabilityMoveLeft = PROB_MOVE_LEFT;
var probabilityMoveRight = PROB_MOVE_LEFT + PROB_MOVE_RIGHT;

if (atLeftBounds === true && atRightBounds === false) {
    probabilityMoveLeft = 0;
} else if (atLeftBounds === false && atRightBounds === true) {
    probabilityMoveRight = PROB_MOVE_LEFT;
} else if (atLeftBounds === false && atRightBounds === false) {
    // Do nothing
} else if (atLeftBounds === true && atRightBounds === true) {
    // Something went terribly wrong!
}

if (randomNumber < probabilityMoveLeft) {
    // Move left
} else if (randomNumber < probabilityMoveRight) {
    // Move right
} else {
    // Fire
}
```


## Break Time!
```
     ___T_
    | o o |
    |__v__|
    /| []|\
  ()/|___|\()
     |_|_|
     /_|_\
```
Credits to [https://walsh9.github.io/asciibots/](https://walsh9.github.io/asciibots/)


## Awareness of Surroundings
The previous algorithm still essentially makes decisions randomly, just minus the decisions that are totally 100% useless.

To improve upon that (again), I made the AI aware of its surroundings.

This consisted of having a zone in front of the AI which checked for incoming projectiles.

![awareness](/images/2016-09-11-awareness.png)

Whenever an incoming projectile collides with the awareness zone (pictured as a white line above), the AI is notified and dodges away from the incoming projectile.

```
if (projectile collides with awareness zone) {
    if (projectile is on the left of AI) {
        move AI to the right
    } else {
        move AI to the left
    }
}
```


## Courage
Merely dodging projectiles leads to a very boring AI - one that can't be hit.

> In a game, AIs have a clear point: attempt to win. The key word is attempt; an AI that always wins leaves the player no chance to win, and players play your game in pursuit of meaningful choices that lead to a clear, compelling goal. The AI wants to win, but ultimately is built to lose.
> 
>  _- [Andrew Gray, AI Programming: Noob to Pro](http://gamejolt.com/f/ai-programming-noob-to-pro/666)_

To improve on the AI (why would I want to de-prove (totally a legit word) it?), I introduced the concept of courage to the AI.

>> The true courage is in facing danger when you are afraid.
>> 
>> _- Frank L Baum, Wizard of Oz_
>
> _- Google, Google_

What would courage mean to the AI (because AIs don't have emotions, ...right?)? In this case, courage would influence

1. its probability to move, and
2. its sensitivity to incoming projectiles.

`Courage` is represented by a number; the greater the number, the bolder the AI becomes.


### Probability to Move
Another state comes into play here, the state of whether the AI has been hit by a projectile in the previous round.

The idea is based on "once bitten, twice shy"; if the AI was previously hit, `courage` will decrease and the probability to move will increase (and the probability that the AI will fire will decrease accordingly), if the AI was not previously hit, `courage` will increase and the respective probabilities adjusted accordingly.

```
if (was hit in previous round) {
    courage = courage - 1;
    probabilityToFire = PROB_FIRE - courage
    probabilityToMove = PROB_MOVE + courage
} else {
    courage = courage + 1;
    probabilityToFire = PROB_FIRE + courage
    probabilityToMove = PROB_MOVE - courage
}
```


### Sensitivity to Incoming Projectiles
The awareness zone from before is tweaked slightly to reduce wasteful dodging actions when the incoming projectiles are too far away from the AI.

![awareness2](/images/2016-09-11-awareness2.png)

A check is added to get the AI to dodge only if the incoming projectile is within a certain distance (between the red lines pictured above) away from the AI.

This distance is determined based on `courage`: as `courage` increases, the distance that triggers the AI to dodge decreases making the AI more willing to risk getting hit by projectiles.

```
if (projectile collides with awareness zone) {
    if (projectile is (DISTANCE_LOOK_SIDE - courage) away from AI) {
        move AI
    }
}
```


## Conclusion
The AI is certainly far from done and there's plenty to improve on. But what I've done so far seems sufficient for a decent defensive AI. At least when I tried it...

All code is available on [github](https://github.com/ksami/shooter) with this particular iteration of the AI on [v0.2.1](https://github.com/ksami/shooter/tree/v0.2.1). If you want to quickly try out the most updated game, you could also do so at [https://ksami.github.io/shooter](https://ksami.github.io/shooter).

Coming up next! What do you do when the AI comes for you?