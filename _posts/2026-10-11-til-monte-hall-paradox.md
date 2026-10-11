---
layout: post
title:  "[TIL] Monte Hall paradox"
date:   2026-10-11 21:05:47 +0700
categories: statistic 
---

Today I spent some time learning about the Monty Hall paradox. I wrote this blog post to revisit the concept and help myself digest the core idea.

The term Monty Hall paradox comes from the game show Let's Make a Deal. In the final round, a prize is hidden behind one of three doors. The player must choose the correct door to win the prize.

The scenario is simple: the player selects Door 1. Then the host opens Door 3 and reveals that the prize is not there. The challenge for the player is whether to switch doors or keep the original choice.

My instinct was to stay with Door 1 because the probabilities between Door 1 and Door 2 seemed equivalent: 50/50. It turns out that intuition is not optimal from a statistical perspective.

Given the rules of the game, the prize always exists behind one of the three doors. In the initial state, each door has an equal probability of hiding the prize:
$$
P(S_1) = P(S_2) = P(S_3) = \frac{1}{3}
$$
where $P$ represents probability and $S_n$ represents the event that the prize is behind Door $n$.

Since the prize must be behind one of the doors:

$$
P(S_1) + P(S_2) + P(S_3) = 1
$$
Once the player chooses Door 1, the probability that the prize is behind either Door 2 or Door 3 is:
$$
P(S_2) + P(S_3) = \frac{2}{3}
$$
The host then reveals that Door 3 is empty.
$$
P(S_2) + 0 = \frac{2}{3}
$$
Therefore:
$$
P(S_2) = \frac{2}{3}
$$
while
$$
P(S_1) = \frac{1}{3}
$$
At this point, the math clearly shows that Door 2 has a higher probability than Door 1. The optimal strategy is to switch doors.
$$
P(S_1) < P(S_2)
$$
$$
\frac{1}{3} < \frac{2}{3}
$$
For me, this was the pivot point.

The revelation was not that Door 2 somehow became lucky. The important insight was that Door 1 never became more likely.

The prize never moved. The underlying state of the game never changed. What changed was the information available to me after the host opened Door 3.

That made me think about the problem beyond probability.

In a way, the Monty Hall paradox is about decision-making under uncertainty. The rational strategy is not to cling to the original choice, but to **update beliefs** when new information arrives.

It also reminded me of ideas from _Thinking, Fast and Slow_. Even after understanding the math, switching still feels uncomfortable. If I switch and lose, the regret feels much stronger than if I had simply stayed.

> Emotionally, staying feels safer. Mathematically, switching is better.

My biggest takeaway is that the Monty Hall paradox is not really about doors at all.

It is about recognizing the difference between the state of the world and the information we have about it. The state never changed. The prize never moved. But once new information was revealed, the rational decision changed completely.

That is what makes this simple game-show puzzle so memorable to me.