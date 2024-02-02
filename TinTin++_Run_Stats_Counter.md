This is a very basic example of a run stat counting trigger. It counts
the number of kills and the amount of experience you make.

## Code

*Put the following into a file in your tintin++ folder and use **\#read
<filename>** while in tintin to load them up. Though you probably want
them to autoload with your character.*

`#var runkills 0;`  
`#var runexp 0;`  
`#act {^You receive %1 experience points.} {#math runkills {$runkills + 1};#math runexp {$runexp + %1}}`  
`#alias echorun {#echo {Exp: $runexp Kills: $runkills}}`  
`#alias showrun {gtell Exp: $runexp Kills: $runkills}`  
`#alias resetrun {#var runkills 0;#var runexp 0;#CR}`

## Usage

Use **echorun** to see the stats, **showrun** to tell the group, and
**resetrun** to reset the counter. You can also add them to a custom
prompt if you'd like!

[Category: TinTin++ Scripting](Category:_TinTin++_Scripting "wikilink")
