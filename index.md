# Event Loop. Render Queue 

The focus of this presentation is the Render phase of the Event Loop in particular. But at first we will go over some general information about the Event Loop and its related concepts.

## Event Loop. The Basics. Well Known Facts and Concepts

![Image](https://i.stack.imgur.com/E4wh6.gif)

JavaScript is **single-threaded**, meaning that there is only one call-stack and heap, which are not basically "part of the JS itself", but of the Engine that executes the code, like V8, Rhino, SpiderMonkey, etc. Having a single-thread would be cumbersome if there wasn't the idea of asynchronous programming. 

There are APIs in the browser (e.g. “setTimeout”). Those APIs, however, are not provided by the Engine, but are provided by the browser. 

And here is where the role of the Event Loop comes in. 

### Callback Queue and Job Queue

Callback queue is the well known one, where the asynchronous code gets pushed to, and waits for the execution.

Apart from the Callback queue browsers have introduced one more queue which is “Job Queue”, reserved only for new Promise() functionality. So when we use promises in our code, we add .then() method, which is a callback method. These `thenable` methods are added to Job Queue once the promise has returned/resolved, and then gets executed. 

```markdown
console.log('Message no. 1: Sync');
setTimeout(function() {
   console.log('Message no. 2: setTimeout');
}, 0);
var promise = new Promise(function(resolve, reject) {
   resolve();
});
promise.then(function(resolve) {
   console.log('Message no. 3: 1st Promise');
})
.then(function(resolve) {
   console.log('Message no. 4: 2nd Promise');
});
console.log('Message no. 5: Sync');
```
So here we have both a setTimeout and promises, what will be the result of running this snippet?

It will be the following:

```
// Message no. 1: Sync
// Message no. 5: Sync
// Message no. 3: 1st Promise
// Message no. 4: 2nd Promise
// Message no. 2: setTimeout
```

**Why?:** Job Queue has high priority in executing callbacks, if event loop tick comes to Job Queue, it will execute all the jobs in job queue first until it gets empty, then will move to callback queue.

The Event loop is, of course, not as simple as we imagine it. 

A more accurate illustartion for it would be the following. 

![Image](https://hsto.org/r/w1560/webt/l0/z9/q2/l0z9q2s-zdltplomxlim269pu7k.png)

The logic behind it is very simple. 

We have some tasks, "orders" from ourselves defined in our code and the highest priority is given to those. 
Only after them come the "orders" from other "cliets", i.e. the APIs. 
Here we have three types of "clients" and therefore three types of tasks: Render, Microtasks и Tasks.

![Image](https://hsto.org/r/w1560/webt/l0/z9/q2/l0z9q2s-zdltplomxlim269pu7k.png)



``` markdown
const stop = Date.now() + 1000;

// Schedule a (macro)task
function sT() {
  if (Date.now() >= stop) {
    return;
  }
  console.log(Date.now()/100 + ": (macro)task");
  setInterval(sT, 0);
}
sT();

// Schedule animation frame callback
function rAF() {
  if (Date.now() >= stop) {
    return;
  }
  console.log(Date.now()/100 + ": requestAnimationFrame");
  requestAnimationFrame(rAF);
}
rAF();

Promise.resolve().then(() => {
  console.log(Date.now()/100 + ": microtask");
});
```

Let's run and see the result of this coe snippet. 

``` markdown
const stop = Date.now() + 1000;

// Schedule a (macro)task
function sT() {
  if (Date.now() >= stop) {
    return;
  }
  console.log(Date.now()/100 + ": (macro)task");
  setInterval(sT, 0);
}
sT();

// Schedule animation frame callback
function rAF() {
  if (Date.now() >= stop) {
    return;
  }
  console.log(Date.now()/100 + ": requestAnimationFrame");
  requestAnimationFrame(rAF);
}
rAF();

function prom() {
    if (Date.now() >= stop) {
        return;
    }
    console.log(Date.now()/100 + ": microtask");
    Promise.resolve().then(prom);
}
prom();
```

And now let's see the difference of the above mentioned queues.


## Render Queue

## RequestAnimationFrame
