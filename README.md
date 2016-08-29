# icpc-cycling
This is a problem from a programming competition. I came across this problem
when trying to prepare for a competition. The problem seemed to be very
straight forward at the beginning and after trying it for a few hours, it
turned out to be super challenging. After declaring it to be too hard, I gave
it up at the time. That was more than two years ago. But I always kept this
problem statement in my mind and occasionally wonder about this problem's final
solution.

This problem eventually came back to me. And now I don't have a valid excuse to
further ignore it. Time wise I am working and I have a bunch of free time after
work. I cycle to work and often wonder why I get to work at different speeds. I
also don't want to waste my life waiting for traffic lights.

All in all, being able to solve this problem means a lot to me.

# Approach Explained
TL;DR version?

Having read the problem description, there are a few possible scenarios:

- Shit I don't remember any of the motion related formulas in high school, game
  is over.
- Hell yea this isn't this problem just asking if we remember what is
  displacement given acceleration and time
- Damn this problem seems hard. Is it a dynamic programming question?

And so on. Actually the problem is a little more involved that asking for some
physics formulas. Here are a few of them.

## Formulas
Time `t` given acceleration `a` and displacement `s`

```
t = sqrt(2s/a)
```

Displacement `s` given initial velocity `v_0`, acceleration `a` and time `t`

```
s = v_0*t+(a*t^2)/2
```

Final velocity `v_final` given initial velocity `v_0`, acceleration `a` and
time `t`

```
v_final = v_0+a*t
```

## One Traffic Light
Before digging into the more complicated examples, what about the simplest form
of the problem? There are two cases when there is only one traffic light. The
simpler version of this is easy, if you look at the following diagram. The wavy
lines represent the times that you cannot cross that specific distance.

![oneTrafficLightCase1](/figures/oneTrafficLightCase1.jpg)

Here we have the traffic light value `100 1 30`. It is obvious that we can just
cycle pass the traffic light with full acceleration from the beginning.

Let's change the above example a little, with a more general case `100 25 10`.

![oneTrafficLightCase2](/figures/oneTrafficLightCase2.jpg)

Things become interesting here, we have too much time to get the to traffic
light. What do we do, do we get to the traffic light as quickly as possible and
then wait? This would be a little stupid because we then need to start with
velocity 0 when the light turns green. You may have guessed what I am trying to
imply here, we want to be as fast as we can at the 100 distance, as soon as it
turns green. I find it a little tricky to do a mathematical proof here. But the
following diagram might make sense to you.

![explainWaitCase](/figures/explainWaitCase.jpg)

If we plot `v-t` graph, we know what the slope of the plot is acceleration and
if we integrate the function we get displacement. In this case, we have a fixed
displacement we need to cover, and we have fixed maximum slope of the plot. How
to achieve maximum finishing speed? The red plot shows the case where we speed
up alternatively: accelerate, constant and then accelerate... For the green
plot wait until we are sure we can accelerate all the way.

# Acknowledgement
Say thanks to Felix, Jo and Tabbi

# Warning
You may find the algorithm useful and would like to try it out in real life. Be
at your own risk and note that a Dutch cyclist may just crash on your bike when
you are slowing down.

# Reference
[Problem Archive](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&category=569&page=show_problem&problem=4279)

# Project52
This is a project from my [Project52](https://github.com/jutkko/project52).
