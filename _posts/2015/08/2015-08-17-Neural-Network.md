---
layout: post
date: 2015-08-17 15:59:50
title: Neural Network
tags: [neural network, machine learning]
published: false
---

Neural networks are the in-thing now so I tried to get into it too. I learnt from [A Neural Network in 11 lines of Python](http://iamtrask.github.io/2015/07/12/basic-python-network/) and [Hacker's guide to Neural Networks](http://karpathy.github.io/neuralnets/) but I took quite long to understand it. Hence, I'll try my best to explain the most basic principle, backpropagation, in as simple a way as possible using only javascript with no dependencies.

Basic understanding of algebra and functions is required. Basic understanding of programming would be helpful.


## Machine Learning
Terms are a funny thing. I'm going to stick with machine learning because that's what my machine is doing: learning. The aim of this whole exercise is to get the machine to learn the (unknown to it) function using known inputs.

### Code

```javascript
// input dataset
var input = [
    [0,0,1],
    [0,1,1],
    [1,0,1],
    [1,1,1],
];

// output dataset
var output = [
    0,
    0,
    0,
    1,
];

// sigmoid function
function nonlin(x, deriv){
    if(deriv === true){
        return x*(1-x);
    }
    return 1/(1+Math.exp(-x));
}


// seed random numbers to make calculation deterministic
// obtain same results each time to compare
var seed = 1;
function random(){
    var n = Math.sin(seed++) * 10000;
    return n - Math.floor(n);
}

var l0 = [];
var l1 = [];
var l1_error = [];
var l1_delta = [];

// initialize weights randomly with mean 0
var syn0 = [];
syn0[0] = 2*random()-1;
syn0[1] = 2*random()-1;
syn0[2] = 2*random()-1;


// n iterations
for (var i = 0; i < 1000000; i++) {

    // for each set of data
    for (var j = 0; j < input.length; j++) {
        // forward propagation
        l0 = input;

        // multiply each data set in l0 by syn0
        // representing network's prediction
        // aim is to have l1[j] == output[j]
        // l0[j] = [x,y,z]; syn0 = [a,b,c]
        // l1[j] = a*x + b*y + c*z
        l1[j] = (l0[j][0] * syn0[0]) + (l0[j][1] * syn0[1]) + (l0[j][2] * syn0[2]);

        // maps result to value in the range 0-1
        l1[j] = nonlin(l1[j]);
     
        // how far off is the prediction?
        l1_error[j] = output[j] - l1[j];
        
        // multiply how much we missed by the
        // slope of the sigmoid at the values in l1
        // force predictions away from centre
        // be more decisive!
        l1_delta[j] = l1_error[j] * nonlin(l1[j], true);

        // update weights a,b,c
        syn0[0] += l0[j][0] * l1_delta[j];
        syn0[1] += l0[j][1] * l1_delta[j];
        syn0[2] += l0[j][2] * l1_delta[j];
    }

}

console.log("Prediction after training:");
console.log(l1);
console.log("\n");
console.log("After training, test with [1,1,1]:");
var result = nonlin(1*syn0[0] + 1*syn0[1] + 1*syn0[2]);
console.log(result);
```

So here in our example we have 4 sets of inputs and expected outputs. These are our training data. By the way, these data is obtained using the [boolean AND function](https://en.wikipedia.org/wiki/Truth_table#Logical_conjunction_.28AND.29) where the output is 1 if and only if all its inputs are 1. This AND function is what we hope the network will learn after its training.

### Modelling
To start the learning process, we first model the AND function using the equation

```
f = a*x + b*y + c*z
```

`f` is the output
`x`, `y` and `z` are the inputs  
`a`, `b` and `c` are the parameters that control the output  

The goal is hence to obtain the most accurate combination of the parameters `a`, `b` and `c` such that, when substituted into the equation with a set of inputs, the equation produces the same output expected from the AND function.