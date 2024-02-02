OKAY FOR SOME REASON THE SITE WHEN YOU SAVE THE PAGE SCREWS WITH THE
CODE. IT MOVES IT ALL AROUND. IF YOU WANT A WORKING COPY OF THE XML
FILE. SEND A NOTE OR A TELL TO SCRAPE, BLOODSWORDE, BELLTOLL, METHADUS.
YOU'LL HAVE BETTER LUCK WITH A TELL. I'LL EMAIL YOU A COPY OF THE
WORKING CODE IN A FILE YOU CAN JUST IMPORT. I TOOK THE TIME TO UNTANGLE
THE MESS THE PAGE MADE AND HAVE NO PROB HELPING OUT ANYONE WHO DOESN'T
WANT TO GO THROUGH THE HEADACHE.

COPY AND PASTING THIS WILL NOT
WORK...................................................................................................................

<?xml version="1.0" encoding="ISO-8859-1" ?>

<cmud>  
`  `<class name="runcounter">  
`    `<trigger priority="1550">  
`      `<pattern>`^You receive (%d) experience points.`</pattern>  
`      `<value>`#ad exp %1;#ad netxp %1;#ad cnt 1`</value>  
`    `</trigger>  
`    `<trigger priority="1560">  
`      `<pattern>`^You attempt to bash`</pattern>  
`      `<value>`#ad bash 1;stand`</value>  
`    `</trigger>  
`    `<trigger priority="1570">  
`      `<pattern>`^You bash into (%*) goes down!`</pattern>  
`      `<value>`#ad bash 1;#ad sucbash 1`</value>  
`    `</trigger>  
`    `<trigger priority="1580">  
`      `<pattern>`^You toss (%*) to the ground!`</pattern>  
`      `<value>`#ad toss 1;#ad suctoss 1;#hi`</value>  
`    `</trigger>  
`    `<trigger priority="1590">  
`      `<pattern>`^You trip (%*) goes down!`</pattern>  
`      `<value>`#ad trip 1;#ad suctrip 1`</value>  
`    `</trigger>  
`    `<trigger priority="1600">  
`      `<pattern>`^You try to grab a hold, but miss!`</pattern>  
`      `<value>`#ad toss 1`</value>  
`    `</trigger>  
`    `<trigger priority="1610">  
`      `<pattern>`^You successfully rescue`</pattern>  
`      `<value>`#ad rescue 1;#ad sucrescue 1`</value>  
`    `</trigger>  
`    `<trigger priority="1620">  
`      `<pattern>`^You sweep, but they are just a little too quick for you.`</pattern>  
`      `<value>`#ad trip 1`</value>  
`    `</trigger>  
`    `<trigger priority="1630">  
`      `<pattern>`^You fail to rescue`</pattern>  
`      `<value>`#ad rescue 1`</value>  
`    `</trigger>  
`    `<trigger priority="1640">  
`      `<pattern>`^Your tail whacks (%*) in the head! They are stunned`</pattern>  
`      `<value>`#ad tail 1`</value>  
`    `</trigger>  
`    `<trigger priority="1650">  
`      `<pattern>`^You attempt a critical golden strike!`</pattern>  
`      `<value>`#ad gstrike 1`</value>  
`    `</trigger>  
`    `<trigger priority="1660">  
`      `<pattern>`^You attempt a golden strike!`</pattern>  
`      `<value>`#ad gstrike 1`</value>  
`    `</trigger>  
`    `<trigger priority="1670">  
`      `<pattern>`^Death sucks (%*) experience points from you as payment for resurrection.`</pattern>  
`      `<value>`#ad death 1;#ad deathloss %1;#ad netxp -%1`</value>  
`    `</trigger>  
`    `<trigger priority="1680">  
`      `<pattern>`^You flee (%*)! What a COWARD! You lose (%d) exps!`</pattern>  
`      `<value>`#ad netxp -%2;#ad fleexp %2`</value>  
`    `</trigger>  
`    `<trigger priority="1690">  
`      `<pattern>`^You couldn't get away! You lose (%d) exps.`</pattern>  
`      `<value>`#ad netxp -%1;#ad fleexp %1`</value>  
`    `</trigger>  
`    `<trigger priority="1700">  
`      `<pattern>`^You recall from combat! You lose (%d) exps.`</pattern>  
`      `<value>`#ad netxp -%1;#ad fleexp %1`</value>  
`    `</trigger>  
`    `<trigger priority="1710">  
`      `<pattern>`^You failed! You lose (%d) exps.`</pattern>  
`      `<value>`#ad netxp -%1;#ad fleexp %1`</value>  
`    `</trigger>  
`    `<trigger priority="1720">  
`      `<pattern>`^You are (%*) and a worshipper of (%x).`</pattern>  
`      `<value>`#var worship %2`</value>  
`    `</trigger>  
`    `<trigger priority="1730">  
`      `<pattern>`^You are (%*) and a devoted worshipper of (%x).`</pattern>  
`      `<value>`#var worship %2`</value>  
`    `</trigger>  
`    `<trigger priority="1740">  
`      `<pattern>`^Your gain is: (%d)/(%d) hp, (%d)/(%d) m, (%d)/(%d) mv (%d)/(%d) prac.`</pattern>  
`      `<value>`#ad lev 1;emote increases in power!! |by|%1 |y|hps|n|, |br|%3 |r|mana|n|, |bw|%7 |w|practices|n|.`</value>  
`    `</trigger>  
`    `<trigger priority="1750">  
`      `<pattern>`^You raise a level!!(%*)Your gain is: (%d)/(%d) hp, (%d)/(%d) m, (%d)/(%d) mv (%d)/(%d) prac.`</pattern>  
`      `<value>`#ad lev 1;emote increases in power!! |by|%2 |y|hps|n|, |br|%4 |r|mana|n|, |bw|%8 |w|practices|n|.`</value>  
`    `</trigger>  
`    `<alias name="runreport">  
`      `<value>`gtell This run, @worship gave me~: @exp xp, @cnt kills, @lev level%if(@lev=1,,s).`  
`  %if(@death=0 && @fleexp=0,,gtell " I've lost " %if( @death!=0, @deathloss xp by @death death~(s~))%if( @death!=0 and @fleexp!=0, " and " )%if( @fleexp!=0, @fleexp xp from fleeing ~and~/~or recalling) so my net gain is @netxp xp.)`  
`  `  
</value>  
`    `</alias>  
`    `<alias name="resetrun">  
`      `<value>`#var exp 0;#var cnt 0;#var lev 0;#var bash 0;#var sucbash 0;#var trip 0;#var suctrip 0;#var toss 0;#var suctoss 0;#var rescue 0;#var sucrescue 0;#var death 0;#var deathloss 0;#var netxp 0;#var fleexp 0;#var tail 0;#var gstrike 0;#ec --- Resetting counters ---`  
  
</value>  
`    `</alias>  
`    `<alias name="stats">  
`      `<value><![CDATA[%if (@bash=0 && @trip=0 && @toss=0 && @rescue=0 && @tail=0 && @gstrike=0,,gtell %if(@bash!=0," "Bashes~: @sucbash~/@bash)%if(@trip!=0," "Trips~: @suctrip~/@trip)%if(@toss!=0," "Tosses~: @suctoss~/@toss)%if(@rescue!=0," "Rescues~: @sucrescue~/@rescue)%if(@tail!=0," "Tails~: @tail)%if(@gstrike!=0," "Golden strikes~: @gstrike))]]></value>  
`    `</alias>  
`  `</class>  
</cmud>

[Category:Cmud Scripting](Category:Cmud_Scripting "wikilink")
