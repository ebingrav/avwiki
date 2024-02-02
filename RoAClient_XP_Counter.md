For a simple XP counter you need to add 3 code segments.

1\. A trigger to add up the XP received. The Trigger should be:

`^You receive %0 experience points.`

The Reaction should be:

`$varadd xpcount %0`  
`$varadd xpday %0`  

2\. An alias to report the xp gained, you can call it runreport.

`;This run we gained @xpcount xp, since I last reset the counter I have gained @xpday total.`

3\. An alias to reset the xpcounter, runreset.

`$var xpcount 0`

4\. (Optional) An alias to reset the ongoing xp counter: dayreset.

`$var xpday 0`

At the end of each run (or during it) type runreport to display the XP
gained. Entering runreset will reset the counter. It is important to
remember to press enter at the end of your commands, ROA doesn't send
the command unless it sees a carriage return.

[Category:RoAClient_Scripting](Category:RoAClient_Scripting "wikilink")
