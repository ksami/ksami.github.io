---
layout: post
date: 2015-03-10 14:34:00
title: Programming Language
tags: [Coq, programming, proof]
---

The title of the class is "Programming Language" but the first thing we learn in class are proofs. Yes, mathematical rigourous proofs for such things as 1+1=2. And the language we are learning to write these proofs is [Coq](https://coq.inria.fr/) the formal proof assistant.

Still not clear on why we are learning proofs but from what the TA explained, we will be using Coq to proof and verify properties of programs and programming languages.

An example of a proof written using Coq

```
Theorem mult_0_plus : forall n m : nat,
  (0 + n) * m = n * m.
Proof.
  intros n m.
  rewrite -> plus_O_n.
  reflexivity.  Qed.
```
<caption>I'm not sure what's going on here</caption>

Coq is unlike any programming language I've learnt so far. As I've heard, Coq belongs to a category called functional programming languages which lets you use functions as though they were objects (?); perhaps something like function pointers in C/C++. Also, the functional programming style has no side effects ie. programs are purely functions which map input to output, exactly like how mathematical functions are.

So, until next time when I'm not so confused...