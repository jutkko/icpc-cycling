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

### Case Easy
![oneTrafficLightCase1](/figures/oneTrafficLightCase1.jpg)

Here we have the traffic light value `100 1 30`. It is obvious that we can just
cycle pass the traffic light with full acceleration from the beginning.

Let's change the above example a little, with a more general case `100 25 10`.

### Case Hard
![oneTrafficLightCase2](/figures/oneTrafficLightCase2.jpg)

Things become interesting here, we have too much time to get the to traffic
light. What do we do, do we get to the traffic light as quickly as possible and
then wait? This would be a little stupid because we then need to start with
velocity 0 when the light turns green. You may have guessed what I am trying to
imply here, we want to be as fast as we can at the first light, as soon as the
light turns green. I find it a little tricky to do a mathematical proof here.
But the following diagram might make sense to you.

![explainWaitCase](/figures/explainWaitCase.jpg)

If we plot `v-t` graph, we know what the slope of the plot is acceleration and
if we integrate the function we get displacement. In this case, we have a fixed
displacement we need to cover, and we have fixed maximum slope of the plot. How
to achieve maximum finishing speed? The red plot shows the case where we speed
up alternatively: accelerate, constant and then accelerate... For the green
plot wait until we are sure we can accelerate all the way. We can see that the
area under the two plots should be same (just imagine they are), and the red
plot is more spread over time. This makes the final velocity of the plot small.
This is also the case if we do accelerate, decelerate and go on... Any
combination of this will not achieve as high end velocity as the green plot.
One should be able to do a rigorous proof, and I am happy to discuss this.

The above two examples conclude the scenario when we only have one traffic
light. Now we can try the examples in the problem spec.

## Two Traffic Light
The first thing we want to ask is, do we apply the approach we had in single
traffic light case? That is going to make sure that we pass the first light in
shortest amount of time possible, is that helpful? Let's have a look at the
examples provided in the spec.

### Example 1 (Sample input 1)
The first traffic light's setup is `200 15 15`, second `225 31 10`. A quick
calculation using the above formulas, we know that we can pass the first
traffic light with full acceleration but not the second one. It takes us

```
t = sqrt(2s/a)
t = sqrt(800)
t = 28.28
```

So we can decide to pass the first traffic light as times `[28.28, 30]`. A
similar calculation for the second light we get `30` seconds to the second
traffic light with full acceleration. Provided the second traffic light turns
green at `31` seconds, we know the problem reduces to getting the maximum
velocity at the second traffic light at `31` seconds. We know the distance we
need to cover is `25`, we know that the initial velocity is in range
`[0, 14.14]`. And we have

```
s = v_0*t+(a*t^2)/2
v_0 = (s-(a*t^2)/2)/t
v_0 = s/t-a*t/2
```

We also have for the small gap between first traffic light and the second
traffic light

```
v_final = v_0+a*t
```

Therefore replacing `v_final` in equation becomes

```
v_final = s/t+at
```

![sample1](/figures/sample1.jpg)

We know `v` is in `[0, 14.14]`. And we want to maximise that for valid `t`.
What is the range for `t` here? From the diagram, we can see the `t` will
belong to `[g_1, g_1+g_2]` which is `[1, 2.72]`.

![plot](/figures/plot.jpg)


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
