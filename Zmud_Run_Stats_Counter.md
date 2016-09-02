---
title: Zmud Run Stats Counter
permalink: wiki/Zmud_Run_Stats_Counter/
layout: wiki
tags:
 -  Zmud Scripting
---

This trigger counts the number of kills and amount of experience you
have gained on a run and shows it to the group.

This is a basic example, and it could be modified to show more
information such as:

-   Number of kills per hour / day
-   Average amount of experience per kill

Code
----

*Copy each line individually and then paste them into zMud:*

`#VAR runexp {0}`  
`#VAR runkills {0}`  
`#TRIGGER {^You receive (%d) experience points.} {#AD runexp %1;#AD runkills 1}`  
`#ALIAS showstats {gtell We killed @runkills Mob(s) for a total of @runexp experience!}`  
`#ALIAS resetstats {#VAR runkills 0;#VAR runexp 0}`

Usage
-----

To show the stats, type <B>showstats</B> at any time.  
To reset the stats to 0, type <B>resetstats</B>  

How It Works
------------

`#VAR runexp {0}`

This line creates a new Variable `#VAR` called "runexp" `runexp` and
sets the initial value to 0 `{0}`

`#VAR runkills {0}`

This line creates a new Variable `#VAR` called "runkills" `runkills` and
sets the initial value to 0 `{0}`

*zMud uses variables such as these to store data, it can store numbers
like this example uses, but they could also be used to store letters or
words.*

`#TRIGGER {^You receive (%d) experience points.} {#AD runexp %1;#AD runkills 1}`

This line creates a new Trigger `#TRIGGER` which will activate when it
sees "You receive (%d) experience points." from the mud `{^You`
`receive` `(%d)` `experience` `points.}`. It will then add the correct
amount of experience points to our runexp variable `{#AD` `runexp` `%1`
and add one kill to our run kills variable `;#AD` `runkills` `1}`.

*zMud uses triggers to interpret information sent from the mud. A
trigger accepts at least two arguments. The first argument tells it
which line from the mud will make the trigger activate, and the second
argument tells it what it should do when it is activated. Arguments are
generally placed inside brackets { }.*

*There are a couple of things of interest here. First off, the caret ^
in front of the first argument tells zMud to only activate if the text
is seen at the beginning of the line. We do this so if someone in chat
says: "You receive 581982051 experience points." then our trigger won't
mistakenly add experience points. The caret operator is very useful
against trigger abuse.*

''Also, look at the `(%d)` in the first argument. There are two
important things to notice about it. We use %d instead of an actual
number because there are many possible numbers that the mud could send
here. You could get 5 exp from a mob or you could 128 exp. By putting
%d, we tell zMud to accept any number here. The %d is in parentheses to
tell zMud that we want to keep track of specifically which number the
mud outputs. In the second argument, you can see a `%1`. This is where
the number is actually used.

''Whenever you see a pound sign before a word (generally in caps) such
as `#AD` you know that there is a zMud command being sent. In this case,
AD is short for the 'add' command which adds a number to a variable.

`#ALIAS showstats {gtell We killed @runkills Mob(s) for a total of @runexp experience!}`

This line makes a new Alias `#ALIAS` which will activate when you type
`showstats` thus sending a group tell to the mud saying the amount of
exp and kills so far `{gtell` `We` `killed` `@runkills` `Mob(s)` `for`
`a` `total` `of` `@runexp` `experience!}`.

*zMud aliases are very similar to [aliases in avatar](/wiki/Alias "wikilink").
They are essentially shortcuts. Anytime you type the first argument (in
this case "showstats"), it will send the second argument to the mud.
Unlike an avatar alias however, you can use a zMud alias to send
variables, which we do twice in this example. When zMud sees a word with
`@` in front of it, it will replace that word with whatever
number/word/letter is in the variable of the same name. So we use our
runkills and runexp aliases from before to show the number of kills and
experience.*

*This is the line you would change if you wanted to send your own custom
line to the mud.*

`#ALIAS resetstats {#VAR runkills 0;#VAR runexp 0}`

This line creates a new Alias `#ALIAS` which will activate when you type
resetstats `resetstats` and then reset our variables to 0 `{#VAR`
`runkills` `0;#VAR` `runexp` `0}`.

''This is an example of a zMud alias that doesn't actually send any
information to the mud. Note that there are two different commands
taking place in the second argument. Whenever you want to send two
different commands in the same argument, make sure they are seperated by
a semicolon.

''The \#VAR command takes 2 arguments, the first is the name of the
variable to be modified, and the second is the new value of the
variable. This command will replace whatever information is currently in
the variable.
