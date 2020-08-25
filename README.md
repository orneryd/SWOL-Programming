SWOL Programming

What is SWOL?
=============

SWOL Stands for \"*Simplest Working unit Of Logic.*\" It\'s based partly
off the terminology for the Unit of Work and Repository patterns. It
describes how to think about problems in terms of isolated work and
contracts or interfaces.


*The Problem*

Imagine you have to teach a robot to walk across a room, turn around,
sit down in a chair, and then wave to you. How would you go about this?

At a very high level, when you look at the task at hand, some engineers
would start off by teaching the robot to walk from point A to point B.
Then, they would teach it to make a 180 degree turn, to sit in a chair,
and then finally teach it to wave. And for a lot of cases there\'s
nothing wrong with this. you achieve your goal, you now have a
limited-use robot, and everyone is happy. But, what if you took a
different approach?


*Simplify It*

What if we took a different approach?

Let\'s identify what the simplest working unit of logic could be. What
independent actions can we isolate?

1.  Walking.

2.  Turning.

3.  Sitting.

4.  Waiving. 

What if we broke it down even further?

1.  Walking.

    a.  Lift left leg

    b.  Lean forward

    c.  Lower left leg

    d.  Lift right leg

    e.  Lean forward

    f.  Lower right leg

    g.  Repeat

2.  Turning.

    a.  Lift left leg

    b.  Rotate on right leg

    c.  Lower left leg

    d.  (or vice versa)

3.  Sitting.

    a.  Lean forward

    b.  Bend legs at the knees

4.  Waiving. 

    a.  Bend arm upwards at the elbow

    b.  Move arm back and forth

    c.  Smile!

Do you see a pattern emerging?

if you look at the list of individual actions, the most common and
useful one would be actuating a joint on a limb such as an elbow or
knee. So what if we started there? What if we created individual actions
that could be done independently and composed them to form higher-order
and more productive actions? Sitting is useful in more than just the
context of a chair. Walking is useful outside of the A→ B directions. If
you had just taught the robot to just do the actions within the context,
we would have to start from scratch when we went into a different room
and all the external parameters had changed.

Let\'s look at a code example and let\'s say you wanted to implement
Array.map.

You could very easily implement it in a way such as this:

```javascript
Array.prototype.map = function(transform) {
  const newArr = \[\];\
  for (var i = 0; i \< this.length; i++) {
    newArr.push(transform(this\[i\], i));\
  }
  return newArr;
}
```

***But is it SWOL?***

What if you wanted to 

**What if we took a different approach? Let\'s break down what we are
doing:**

```javascript
Array.prototype.map = (transform) => {

**// We need to create a new array**

  const newArr = [];

**// We need to iterate over our existing list**

  for (var i = 0; i < this.length; i++) {

**// we need to execute a method for every item in our list.**

    newArr.push(transform(this[i], i));
  }

**// We need to return the new array reference to the caller.**

  return newArr;
}
```
What if we isolated the iteration part into its own method?
-----------------------------------------------------------
```javascript
// Now we have an iterator we can us for map.
Array.prototype.forEach = function(callback) {
  for (var i = 0; i < this.length; i++) {
    callback(this[i], i);\
  }
};

Array.prototype.map = function(transform) {
  let newArr = [];
  this.forEach((e, i) => newArr.push(transform(e, i)));
}

// now we can use our iterator to implement reduce
Array.prototype.reduce = function(reducer, accumulator) {
this.forEach((e, i) => (accumulator = reducer(accumulator, e, i)));
  return accumulator;
};
```
Now your code is SWOL. 
=======================
