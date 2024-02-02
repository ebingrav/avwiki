*If you're trying to maximize your lord stats you should
[worship](worship "wikilink") a gain-boosting deity and automorph. This
article is for those that are trying to morph as quickly as possible.*

This is an attempt to calculate the optimum [Morph](Morph "wikilink")
level. Scroll down for the bottom line if you don't care for the math.

Assumptions:

-   Success probability is linear between 300 (20%) and 500 (50%) and
    linear again between 500 and 999 (100%).

> • Note that, if true, this would theoretically mean levels between 300
> & 500 are worth .15% additional chance of success per level, whereas
> levels after 500 each add only .1% chance of success.

-   Expected fail levels is (1000 - level) / 10. I'm aware that this is
    slightly more than observed, but shouldn't be more than 10 levels or
    so.

Let

-   S(x) be the expected levels you do if you first try morph at level x
-   p(x) is probability of success at level x
-   f(x) is number of fail levels

Then:

S(x) = p(x) \* x + (1 - p(x)) \[f(x) + S(x + f(x))\]

( Note that you do f(x) "half-xp" levels so effectively you only "lose"
f(x) levels. )

We can just calculate the above for various x. For simplicity, assume
S(some sufficiently high level) is known. Below, I use S(500) to be
roughly 550 (since you expect to take 2 attempts to morph, so 50 lost
levels).

-   S(430) \~ 430 \* 0.4 + (1 - 0.4) \* \[S(480) + 50\] \~ 520 \[Since
    success probability at 430 is roughly 0.4)\]
-   S(370) = 370 \* 0.3 \* \[S(430) + 60\] \* 0.7 \~ 520
-   S(300) = 300 \* 0.2 + \[S(370) + 70\] \* 0.8 \~ 530

## Empirical Method

Using Avatar's Twitter feed to collect
[data](User:WinterRose/Morph_Data "wikilink"), we can take, for example,
all the non-999 morphs and see when, on average, a player morphs.

    mysql> select sum(level), count(success), sum(level)/count(success) from morph where level!=999 and success=1;
    +------------+----------------+---------------------------+
    | sum(level) | count(success) | sum(level)/count(success) |
    +------------+----------------+---------------------------+
    |     424924 |            814 |          522.019656019656 |
    +------------+----------------+---------------------------+
    (In the table above the success field is a boolean and should probably be called "attempt" instead.)

On average, players morphed at level 522. Chance to morph at this level
is 52.2% and is a safe place to start trying.

Is this number the *optimal morph level*? I wouldn't go into it since
I'm not a mathematician, but it does correlate with the number presented
in the earlier paragraph.

## Bottom Line

Morph early and often.

-   Optimum morph level is somewhere around 370-400 (389 should be
    33.35% chance of success). If you're risk averse, wait until 520-560
    \[this minimizes the likelihood of getting perpetually unlucky --
    the lower probability morph attempts have a lot more variance\].
-   TNL/worship etc. don't matter.
-   Difference between morphing at 300 and 400 and 500 is not more than
    10-20 levels. So it isn't all that important when you morph, as long
    as you do it early.
-   Expected levels you do at hero is roughly 520-550.

[Category: Gameplay](Category:_Gameplay "wikilink") [Category:
Hero](Category:_Hero "wikilink") [Category:
Lord](Category:_Lord "wikilink")
