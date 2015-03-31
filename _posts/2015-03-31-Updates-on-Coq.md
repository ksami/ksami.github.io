---
layout: post
date: 2015-03-31 11:50:00
title: Updates on Coq
tags: [Coq]
---

Just an explanation for the lack of updates: I've been really busy since, not just with schoolwork but also with enjoying myself XD

So far I think I have gotten used to the Coq language and its way of thinking. Basically everything has to be formally defined and proven; formal meaning using funny maths (read: set theory).

I'll just give a quick overview on it. For example, using the code from the previous post,

```
Theorem mult_0_plus : forall n m : nat,
  (0 + n) * m = n * m.
Proof.
  intros n m.
  rewrite -> plus_O_n.
  reflexivity.  Qed.
```

`nat` has been defined earlier in the code to be a natural number `[0,inf]`. Here we are trying to prove `(0 + n) * m = n * m`. Using a previously proven theorem `plus_O_n` which proved `0 + n = n`, we rewrite the current goal into `n * m = n * m`. Then we use `reflexivity` to prove that the terms in the LHS are identical to he terms in the RHS. Qed.

Simple isn't it? I think it actually goes down to the root of programming thinking:
    1. Split a problem down into smaller problems
    2. Look for already solved problems you can apply ie. Don't reinvent the wheel
    3. Profit.
The list is obviously not exhaustive and profiting is optional. But this could perhaps be the reason why we are learning Coq in a Programming Languages class?

'Till next time.