This trigger counts the number of kills, amount of experience you have
gained (or lost) and the amount of levels gained on a run and shows it
to the group. It also counts bashes, trips, tosses and rescues,
succesful or not.

## Code

*Copy each line individually and then paste them into zMud*
<sup>\[1\]</sup>*:*

`#TRIGGER {^You receive (%d) experience points.} {#ad exp %1;#ad netxp %1;#ad cnt 1} {runcounter}`  
`#TRIGGER {^You attempt to bash} {#ad bash 1;stand} {runcounter}`  
`#TRIGGER {^You bash into (%*) goes down!} {#ad bash 1;#ad sucbash 1} {runcounter}`  
`#TRIGGER {^You toss (%*) to the ground!} {#ad toss 1;#ad suctoss 1;#hi} {runcounter}`  
`#TRIGGER {^You trip (%*) goes down!} {#ad trip 1;#ad suctrip 1} {runcounter}`  
`#TRIGGER {^You try to grab a hold, but miss!} {#ad toss 1} {runcounter}`  
`#TRIGGER {^You successfully rescue} {#ad rescue 1;#ad sucrescue 1} {runcounter}`  
`#TRIGGER {^You sweep, but they are just a little too quick for you.} {#ad trip 1} {runcounter}`  
`#TRIGGER {^You fail to rescue} {#ad rescue 1} {runcounter}`  
`#TRIGGER {^You successfully rescue} {#ad rescue 1;#ad sucrescue 1} {runcounter}`  
`#TRIGGER {^Your tail whacks (%*) in the head! They are stunned} {#ad tail 1} {runcounter}`  
`#TRIGGER {^You attempt a critical golden strike!} {#ad gstrike 1} {runcounter}`  
`#TRIGGER {^You attempt a golden strike!} {#ad gstrike 1} {runcounter}`  
`#TRIGGER {^Death sucks (%*) experience points from you as payment for resurrection.} {#ad death 1;#ad deathloss %1;#ad netxp -%1} {runcounter}`  
`#TRIGGER {^You flee (%*)! What a COWARD! You lose (%d) exps!} {#ad netxp -%2;#ad fleexp %2} {runcounter}`  
`#TRIGGER {^You couldn't get away!  You lose (%d) exps.} {#ad netxp -%1;#ad fleexp %1} {runcounter}`  
`#TRIGGER {^You recall from combat!  You lose (%d) exps.} {#ad netxp -%1;#ad fleexp %1} {runcounter}`  
`#TRIGGER {^You failed!  You lose (%d) exps.} {#ad netxp -%1;#ad fleexp %1} {runcounter}`  
`#TRIGGER {^You are (%*) and a worshipper of (%x).} {#var worship %2} {runcounter}`  
`#TRIGGER {^You are (%*) and a devoted worshipper of (%x).} {#var worship %2} {runcounter}`  
`#TRIGGER {^Your gain is: (%d)/(%d) hp, (%d)/(%d) m, (%d)/(%d) mv (%d)/(%d) prac.} {#ad lev 1;emote increases in power!!  |by|%1 |y|hps|n|, |br|%3 |r|mana|n|, |bw|%7 |w|practices|n|.} {runcounter}`  
`#TRIGGER {^You raise a level!!(%*)Your gain is: (%d)/(%d) hp, (%d)/(%d) m, (%d)/(%d) mv (%d)/(%d) prac.} {#ad lev 1;emote increases in power!! |by|%2 |y|hps|n|, |br|%4 |r|mana|n|, |bw|%8 |w|practices|n|.}`  
`#VAR worship {Snikt}`  
`#ALIAS runreport {get_color;gtell |bk|This run, |@bclr|@worship |bk|gave me: |@bclr|@exp |bk|xp, |@bclr|@cnt |bk|kills, |@bclr|@lev |bk|level(s).;#if {@death=0 && @fleexp=0} {} {gtell |bk|I've lost %if( @death!=0, ~|@bclr~|@deathloss ~|bk~|xp ~|bk~|by ~|@bclr~|@death ~|bk~|death~(s~))%if( @death!=0 and @fleexp!=0, ~|bk~| ~and ~|bk~|)%if( @fleexp!=0, ~|@bclr~|@fleexp ~|bk~|xp from fleeing ~and~/~or recalling) so my net gain is |@bclr|@netxp|bk| xp.};stats}`  
`#ALIAS stats {#if {@bash=0 && @trip=0 && @toss=0 && @rescue=0 && @tail=0 && @gstrike=0} {} {gtell %if( @bash!=0, ~|bk~| Bashes: ~|@bclr~|@sucbash~|bk~|~/~|@bclr~|@bash)%if( @trip!=0, ~|bk~| Trips: ~|@bclr~|@suctrip~|bk~|~/~|@bclr~|@trip)%if( @toss!=0, ~|bk~| Tosses: ~|@bclr~|@suctoss~|bk~|~/~|@bclr~|@toss)%if( @rescue!=0, ~|bk~| Rescues: ~|@bclr~|@sucrescue~|bk~|~/~|@bclr~|@rescue)%if( @tail!=0, ~|bk~| Tails: ~|@bclr~|@tail)%if( @gstrike!=0, ~|bk~| Golden strikes: ~|@bclr~|@gstrike) ~|w~|}}`  
`#ALIAS get_color {#var colors {y|g|b|r|c|p};#var bright_colors {by|bg|bb|br|bc|bp};#ad ccc 1;#if (@ccc>%numitems(@colors)) {#var ccc 1};#var clr %item(@colors,@ccc);#var bclr %item(@bright_colors,@ccc)}`  
`#ALIAS resetrun {#var exp 0;#var cnt 0;#var lev 0;#var bash 0;#var sucbash 0;#var trip 0;#var suctrip 0;#var toss 0;#var suctoss 0;#var rescue 0;#var sucrescue 0;#var death 0;#var deathloss 0;#var netxp 0;#var fleexp 0;#var tail 0;#var gstrike 0;#ec --- Resetting counters ---}`

<sup>\[1\]</sup> Instead, you could just copy and paste it into a \*.txt
file. Then, in zMUD, go to "Settings -\> File -\> Import Text" and
select the file you saved to.

## Usage

After adding everything line by line, be sure to use resetrun at least
once before you run, so all the variables exist.

To set the worship part to the right deity, just look at your score.
Otherwise, or for atheist, it will show Snikt.

The command **runreport** shows run stats. If you haven't died and did
nothing special, it will just show your xp. If you've died or fled, it
will show you what you lost. If you've bashed, tripped, tnossed or
rescued, it will show you the, succesful and total, number of those.

The command **resetrun** will reset the counters.

## How It Works

`#TRIGGER {^You receive (%d) experience points.} {#ad exp %1;#ad netxp %1;#ad cnt 1} {runcounter}`  
`#TRIGGER {^Death sucks (%*) experience points from you as payment for resurrection.} {#ad death 1;#ad deathloss %1;#ad exp -%1} {runcounter}`  
`#TRIGGER {^You flee (%*)! What a COWARD! You lose (%d) exps!} {#ad netxp -%2;#ad fleexp %2} {runcounter}`  
`#TRIGGER {^You couldn't get away!  You lose (%d) exps.} {#ad netxp -%1;#ad fleexp %1} {runcounter}`  
`#TRIGGER {^You recall from combat!  You lose (%d) exps.} {#ad netxp -%1;#ad fleexp %1} {runcounter}`  
`#TRIGGER {^You failed!  You lose (%d) exps.} {#ad netxp -%1;#ad fleexp %1} {runcounter}`

All of these keep track of your xp gains and losses. 'exp' is the
variable for the experience gained, whereas 'netxp' is the netto xp,
with losses from fleeing and deaths subtracted. 'cnt' is the number of
kills.

`#TRIGGER {Your gain is: (%d)/(%d) hp, (%d)/(%d) m, (%d)/(%d) mv (%d)/(%d) prac.} {#ad lev 1;emote increases in power!!  |by|%1 |y|hps|n|, |br|%3 |r|mana|n|, |bw|%7 |w|practices|n|.} {runcounter}`

This line keeps track of the amount of levels you get. It also outputs,
in an emote for all to see, what your gains are.

`#TRIGGER {^You attempt to bash} {#ad bash 1;stand} {runcounter}`  
`#TRIGGER {^You bash into (%*) goes down!} {#ad bash 1;#ad sucbash 1} {runcounter}`  
`#TRIGGER {^You toss(%*) to the ground!} {#ad toss 1;#ad suctoss 1;#hi} {runcounter}`  
`#TRIGGER {^You trip (%*) goes down!} {#ad trip 1;#ad suctrip 1} {runcounter}`  
`#TRIGGER {^You try to grab a hold, but miss!} {#ad toss 1} {runcounter}`  
`#TRIGGER {^You successfully rescue} {#ad rescue 1;#ad sucrescue 1} {runcounter}`  
`#TRIGGER {^You sweep, but they are just a little too quick for you.} {#ad trip 1} {runcounter}`  
`#TRIGGER {^You fail to rescue} {#ad rescue 1} {runcounter}`  
`#TRIGGER {^You successfully rescue} {#ad rescue 1;#ad sucrescue 1} {runcounter}`

All these count the number of bashes, tosses, trips and rescues. It also
keeps track of whether or not you fail them.

`#VAR worship {Snikt}`  
`#TRIGGER {You are (%*) and a worshipper of (%x).} {#var worship %2} {runcounter}`

For the final output, it can matter what deity you worship since some
modify your xp gains or losses. Whenever you check your score window,
this line will switch you to your deity. Default is Snikt, declared in
the \#VAR line.

`#ALIAS runreport {get_color;gtell |bk|This run, |@bclr|@worship |bk|gave me: |@bclr|@exp |bk|xp, |@bclr|@cnt |bk|kills, |@bclr|@lev |bk|level(s).;#if {@death=0 && @fleexp=0} {} {gtell |bk|I've lost %if( @death!=0, ~|@bclr~|@deathloss ~|bk~|xp ~|bk~|by ~|@bclr~|@death ~|bk~|death~(s~))%if( @death!=0 and @fleexp!=0, ~|bk~| ~and ~|bk~|)%if( @fleexp!=0, ~|@bclr~|@fleexp ~|bk~|xp from fleeing ~and~/~or recalling) so my net gain is |@bclr|@netxp|bk| xp.};stats}`

Here's where it gets tricky. All the tildes (\~) are used to make sure
zMUD outputs them into your window, since they're all parts of commands
if not prefixed with a tilde. The first thing it does is call get_color,
to select a random color (see \#ALIAS get_color). Then the reports come
in. The first line will look like this:

`This run, Snikt gave me: a xp, b kills, c levels.`

After that, the \#if's begin. The next line, between the semicolons (;)
checks if you've died or fled/recalled from combat. If so, it will tell
you how much you've lost from this, and outputs your net gains. It will
look like this:

`I've lost a xp by b deaths and c xp from fleeing and/or recalling so my net gain is d xp.`

After that, the alias calls the next alias, stats.

`#ALIAS stats {#if {@bash=0 && @trip=0 && @toss=0 && @rescue=0} {} {gtell %if( @bash!=0, ~|bk~| Bashes: ~|@bclr~|@sucbash~|@bclr~|~/~|@bclr~|@bash)%if( @trip!=0, ~|bk~| Trips: ~|@bclr~|@suctrip~|bk~|~/~|@bclr~|@trip)%if( @toss!=0, ~|bk~| Tosses: ~|@bclr~|@suctoss~|bk~|~/~|@bclr~|@toss)%if( @rescue!=0, ~|bk~| Rescues: ~|@bclr~|@sucrescue~|bk~|~/~|@bclr~|@rescue) ~|w~|}}`

Stats outputs your bash, trip, toss and rescue scores, in that order.
But, only when you've used the commands during your run. In the initial
\#if, it checks if you've used any of the commands. If you've used them,
then it will continue with what's further down the line between the
curly brackets ({}). There, between the brackets, it checks per skill if
you've used them, and if so, it will output the results. It will look
something like this:

`Bashes: 1/2 Trips: 1/2 Tosses: 1/2 Rescues: 1/2`

Assuming, of course, you've used all commands twice and failed em all
once out of the two attempts.

`#ALIAS get_color {#var colors {y|g|b|r|c|p};#var bright_colors {by|bg|bb|br|bc|bp};#ad ccc 1;#if (@ccc>%numitems(@colors)) {#var ccc 1};#var clr %item(@colors,@ccc);#var bclr %item(@bright_colors,@ccc)}`

This is an alias that stores all the possible colors in an array. You
can (randomly) change the color used with get_color, use that color with
@clr or the bright version of that color with @bclr. Can be nice, makes
for a dynamic output.

`#ALIAS resetrun {#var exp 0;#var cnt 0;#var lev 0;#var bash 0;#var sucbash 0;#var trip 0;#var suctrip 0;#var toss 0;#var suctoss 0;#var rescue 0;#var sucrescue 0;#var death 0;#var deathloss 0;#var netxp 0;#var fleexp 0;#ec --- Resetting counters ---}`

This is the last alias, and it sets your variables all to 0 (except for
worship, which shouldn't have to reset itself since it's not used
cumulatively. The last part, '#ec --- Resetting counters ---' is only
visible to you, as a local echo.

[Category: Zmud Scripting](Category:_Zmud_Scripting "wikilink")
