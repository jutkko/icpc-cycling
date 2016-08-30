# icpc-cycling (Problem Spec in cycling.pdf in this repo)
This is a problem from a programming competition. I came across this problem
when preparing for a programming competition. The problem seemed to be very
straight forward at the beginning and after trying it for a few hours, it
turned out to be super challenging. After declaring it to be too hard, I gave
it up at that time. That was more than two years ago. But this interesting
problem was always in my mind and occasionally wonder about this problem's
final solution.

It **eventually** came back to me. And now I don't have a valid excuse to
further ignore it. Time wise I am working and I have a bunch of free time after
work. I cycle to work and often wonder why I get to work at different speeds. I
also don't want to waste my life waiting for traffic lights.

All in all, being able to solve this problem means a lot to me.

# Approach Explained
TL;DR version? It might sound obvious but not many people do it. If possible,
don't stop completely at the traffic lights. It will slow you down! Obviously
this is not the final solution but it will at least help.

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
Before digging into the more complicated examples, how about having a look the
simplest form of the problem? There are two cases when there is only one
traffic light. The simpler version of this is easy, if you look at the
following diagram. The wavy lines represent the times that you **cannot** cross
that specific distance.

### Case Easy
![oneTrafficLightCase1](/figures/oneTrafficLightCase1.jpg)
<img src="/figures/oneTrafficLightCase1.jpg" width=0.8\textwidth>

Here we have the traffic light value `100 1 30`. It is obvious that we can just
cycle pass the traffic light with full acceleration from the beginning.

### Case Hard
Let's change the above example a little, with a more general case `100 25 10`.

![oneTrafficLightCase2](/figures/oneTrafficLightCase2.jpg)

Things become interesting here, we have too much time to get the to traffic
light. What do we do, do we get to the traffic light as quickly as possible and
then wait? This would be a little stupid because we then need to start with
velocity `0` when the light turns green. You may have guessed what I am trying
to imply here, we want to be as **fast** as we can **at** the first light, as soon
as the light turns green. I find it a little tricky to do a mathematical proof
here. But the following diagram might make sense to you.

![explainWaitCase](/figures/explainWaitCase.jpg)

If we plot `v-t` graph, we know what the slope of the plot is acceleration and
if we integrate the function we get displacement. In this case, we have a fixed
displacement we need to cover, and we have fixed maximum slope of the plot. How
to achieve maximum finishing speed? The red plot shows the case where we speed
up alternatively: accelerate, constant and then accelerate... For the green
plot wait until we are sure we can accelerate all the way. We can see that the
area under the two plots should be same (just imagine they are), and the red
plot is more spread over time. This makes the final velocity of the plot small.
This is also the case if we do accelerate, decelerate and go on... **Any**
combination of this will not achieve as high end velocity as the green plot.
One should be able to do a rigorous proof, and I am happy to discuss this.

The above two examples conclude the scenario when we only have one traffic
light. Now we can try the examples in the problem spec.

## Two Traffic Light
The first thing we want to ask is, do we apply the approach we had in single
traffic light case? That is going to make sure that we pass the **first** light
in shortest amount of time possible, is that helpful? Let's have a look at the
examples provided in the spec.

### Example 1 (Sample input 1)
The first traffic light's setup is `200 15 15`, second `225 31 10`. A quick
calculation using the above formulas, we know that we can pass the first
traffic light with full acceleration but **not** the second one. It takes us

```
t = sqrt(2s/a)
t = sqrt(800)
t = 28.28
```

So we **can** decide to pass the first traffic light as times `[28.28, 30]`. A
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

We also have for the small gap **between** first traffic light and the second
traffic light

```
v_final = v_0+a*t
```

Therefore replacing `v_final` in equation becomes

```
v_final = s/t+at/2
```

![sample1](/figures/sample1.jpg)

We know `v` is in `[0, 14.14]`. And we want to maximise `v_final` for valid
`t`. Given

```
v_0 = s/t-a*t/2
```

This equation will give us the upper and lower bounds of `t`, since the value
of `v_0` is negatively impacted by the value of `t`. Calculating that gives us

```
0 >= v_0 = 25/t-0.25t
0 >= 25/t-0.25t
t <= 10
```
And

```
14.14 <= v_0 = 25/t-0.25t
14.14 <= 25/t-0.25t
t >= 1.72
```

What is the range for `t` here? From the diagram, we can see the `t` will
belong to `[g_1, g_1+g_2]` which is `[1, 2.72]`.

If we combine the two results, the new range for `t` is `[1.72, 2.72]`.

![plot](/figures/plot.jpg)

The above plot is the function for `v_final`, and we know the function first
**strictly decreases** and then **strictly increases**. To find its maximum is
simple, a non-rigorous way would just be to try both ends. Or we can
differentiate the function and calculate its second derivative to prove that
the function has its local minimum. Given

```
v_final = s/t+at/2
```

If we substitute the end values of `t`

```
t = 1.72 -> v_final = 9.87
```

```
t = 2.72 -> v_final = 14.99
```

To conclude this section, we have `v_0 = 14.99` at `31` seconds. To get the
final answer, we solve

```
410-225 = 14.99t+(t^2)/4
185 = 14.99t+(t^2)/4
t = 10.50
```

And `t_final = 31+10.5 = 41.5`, boom! So how is this method like in practice?
One way to get this optimal time is to first wait **at origin** for `1` second
and then fully accelerate to destination!

### Example 2 (Sample input 2)
For this setup, we got `200 15 15` and `225 35.1 15`. We know that we **cannot**
pass the two traffic lights with full acceleration from the start (see above).
The first light's setup is identical to the previous example. So we have `v_0`
in `[0, 14.14]`.

For `v_final` we have

```
v_final = s/t+at/2
```

![sample2](/figures/sample2.jpg)

We know `v` is in `[0, 14.14]`. And we want to **maximise** `v_final` for valid
`t`. Given

```
v_0 = s/t-a*t/2
```

This equation will give us the **upper** and **lower** bounds of `t`, since the
value of `v_0` is **negatively** impacted by the value of `t`. Calculating that
gives us

```
0 >= v_0 = 25/t-0.25t
0 >= 25/t-0.25t
t <= 10
```

And

```
14.14 <= v_0 = 25/t-0.25t
14.14 <= 25/t-0.25t
t >= 1.72
```

If we combine the two results, the new range for `t` is `[1.72, 2.72]`.

What is the range for `t` here? From the diagram, we can see the `t` will
belong to `[g_1, g_1+g_2]` which is `[5.1, 6.82]`. So the new range for `t` is
`[5.1, 6.82]`.

To get the `v_final`, we just need to try out the two end values of `t`.

```
t = 5.1 -> v_final = 6.17
```

```
t = 6.82 -> v_final = 5.59
```

To conclude this section, we have `v_0 = 6.17` at `35.1` seconds. To get the
final answer, we solve

```
410-225 = 6.17t+(t^2)/4
185 = 6.17t+(t^2)/4
t = 17.53
```

And `t_final = 35.1+17.53 = 52.63`, boom! So how is this method different from
the above? We first wait at the origin for `1.72` seconds, we accelerate to the
first traffic light and instantly reduce the speed to `v_0 = s/t-a*t/2 = 3.62`,
then fully accelerate to the destination.

### Example 3 (Sample input 3)
I am too lazy to do this one now. Maybe later.

## Multiple Traffic Lights
Given the above examples, we can see how to get the time if we want to pass the
first traffic light's first green (provided that it is possible). But
[Example 3](https://github.com/jutkko/icpc-cycling#example-3-sample-input-3)
tells us that might not be the optimal way. So we need to try the different
combinations. One way to do this is devise a depth-first search on the optimal
time, providing a **aborting criteria** from a random run of the algorithm.

# Thanks
Thanks to [Felix](https://github.com/felixdesouza),
[Jo](https://github.com/js3611) and Tabbi for helping me to come up with all
this.

# Warning
You may find the algorithm useful and would like to try it out in real life. Be
at your **own** risk and note that a Dutch cyclist may just crash on your bike when
you are slowing down.

# Reference
[Problem Archive](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&category=569&page=show_problem&problem=4279)

# Project52
This is a project from my [Project52](https://github.com/jutkko/project52).
