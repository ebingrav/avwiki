Making a breakdown in RoAClient isn't as easy as you might have hoped.
My implementation requires a lot of separate aliases and triggers. First
I'll briefly explain what each component does, then I'll show you all
the code.

## Description

Here's the basic idea of how a group breakdown trigger works:

1.  Figure out who's in your group via the "group" command
2.  Use "last" to figure out what class everyone is
3.  Sort this information in a useful way, and display it to your group

RoAClient behaves strangely with regards to when it updates the values
of variables. This is the primary reason why my system is so
complicated. Once you've loaded/copied all of my code, you'll need to
type "break1", "break2", then "names" to perform a group breakdown. If
you're sure that the group composition hasn't changed since your last
breakdown you can get away with just typing "names". Once you get the
whole framework set up it's pretty easy to write aliases to print out
different groups of characters. Instead of printing out the whole group
like I do, you could make an alias to print out just the brutes, or just
the psi/mnd.

## Downloadable Files

As promised, here's the files that contain all the triggers and aliases
necessary to do a group breakdown in RoAClient. To download them, click
on the links below, ignore the warning about malicious code, and
right-click/download the linked file.

Once you have all three of them, fire up RoAClient and import them to
start using them.

![](Breakdown_aliases.ndx "Breakdown_aliases.ndx"),
![](Breakdown_trigs.ndx "Breakdown_trigs.ndx"),
![](Breakdown_triggrps.ndx "Breakdown_triggrps.ndx")

## Actual Code

**If you're trying to copy/paste this code into the client manually,
remember that each alias and trigger needs to end with an empty line or
it will not function properly.**

### Aliases

#### break1

This alias does three things:

-   blanks all of the variables that are used in the process of group
    breakdown.
-   turns on the group of triggers involved in the process
-   sends the command "group" to the MUD

`$var bzkcount 0`  
`$var bzknames ""`  
`$var warcount 0`  
`$var warnames ""`  
`$var bodcount 0`  
`$var bodnames ""`  
`$var palcount 0`  
`$var palnames ""`  
`$var rancount 0`  
`$var rannames ""`  
`$var moncount 0`  
`$var monnames ""`  
`$var shfcount 0`  
`$var shfnames ""`  
`$var sorcount 0`  
`$var sornames ""`  
`$var magcount 0`  
`$var magnames ""`  
`$var wzdcount 0`  
`$var wzdnames ""`  
`$var psicount 0`  
`$var psinames ""`  
`$var mndcount 0`  
`$var mndnames ""`  
`$var prscount 0`  
`$var prsnames ""`  
`$var clecount 0`  
`$var clenames ""`  
`$var drucount 0`  
`$var drunames ""`  
`$var asncount 0`  
`$var asnnames ""`  
`$var rogcount 0`  
`$var rognames ""`  
`$var bcicount 0`  
`$var bcinames ""`  
`$var arccount 0`  
`$var arcnames ""`  
`$var fuscount 0`  
`$var fusnames ""`  
`$var brutecount 0`  
`$var firepowercount 0`  
`$var healercount 0`  
`$var hittercount 0`  
`$var brutestr ""`  
`$var firepowerstr ""`  
`$var healerstr ""`  
`$var hitterstr ""`  
`$groupon breakdown`  
`group`  

#### break2

After you've typed break1 variables like $rogcount, $magnames, etc have
been populated with meaningful data. This information is used by break2
to create four lines of text: $brutestr, $firepowerstr, $healerstr, and
$hitterstr. Each of these lines contain information about who the
brutes/firepower/healers/hitters in your group are.

`$if(@bzkcount > 0)`  
`{`  
`$alias "bzkstring"`  
`}`  
`$if(@warcount > 0)`  
`{`  
`$alias "warstring"`  
`}`  
`$if(@bodcount > 0)`  
`{`  
`$alias "bodstring"`  
`}`  
`$if(@palcount > 0)`  
`{`  
`$alias "palstring"`  
`}`  
`$if(@rancount > 0)`  
`{`  
`$alias "ranstring"`  
`}`  
`$if(@moncount > 0)`  
`{`  
`$alias "monstring"`  
`}`  
`$if(@shfcount > 0)`  
`{`  
`$alias "shfstring"`  
`}`  
`$if(@sorcount > 0)`  
`{`  
`$alias "sorstring"`  
`}`  
`$if(@magcount > 0)`  
`{`  
`$alias "magstring"`  
`}`  
`$if(@wzdcount > 0)`  
`{`  
`$alias "wzdstring"`  
`}`  
`$if(@psicount > 0)`  
`{`  
`$alias "psistring"`  
`}`  
`$if(@mndcount > 0)`  
`{`  
`$alias "mndstring"`  
`}`  
`$if(@prscount > 0)`  
`{`  
`$alias "prsstring"`  
`}`  
`$if(@clecount > 0)`  
`{`  
`$alias "clestring"`  
`}`  
`$if(@drucount > 0)`  
`{`  
`$alias "drustring"`  
`}`  
`$if(@asncount > 0)`  
`{`  
`$alias "asnstring"`  
`}`  
`$if(@rogcount > 0)`  
`{`  
`$alias "rogstring"`  
`}`  
`$if(@bcicount > 0)`  
`{`  
`$alias "bcistring"`  
`}`  
`$if(@arccount > 0)`  
`{`  
`$alias "arcstring"`  
`}`  
`$if(@fuscount > 0)`  
`{`  
`$alias "fusstring"`  
`}`  

#### <class>string

These aliases exist because of RoAClient's semi-retarded behavior. They
append the names of each member of a specific class ($warnames) to the
line of text that corresponds to the group to which that class belongs
($brutestr). You need an alias like this for each class, but I'm only
going to show examples for a few of them.

arcstring:

`$var hitterstr "@hitterstr.md|g|Arc(|bg|@arccount|g|) {@arcnames } "`  

bodstring:

`$var brutestr "@brutestr |c|Bod(|bc|@bodcount|c|) {@bodnames } "`  

clestring:

`$var healerstr "@healerstr |w|Cle(|bw|@clecount|w|) {@clenames } "`  

magstring:

`$var firepowerstr "@firepowerstr |r|Mag(|br|@magcount|r|) {@magnames } "`  

#### names

Here's the alias that prints it all out. It also turns off the
"breakdown" group of triggers.

`gt |c|@brutecount Brute(s)`  
`$if(@brutecount > 0)`  
`{`  
`gt @brutestr |n|`  
`}`  
`gt |r|@firepowercount Firepower`  
`$if(@firepowercount > 0)`  
`{`  
`gt @firepowerstr |n|`  
`}`  
`gt |w|@healercount Healer(s)`  
`$if(@healercount > 0)`  
`{`  
`gt @healerstr |n|`  
`}`  
`gt |g|@hittercount Hitter(s)`  
`$if(@hittercount > 0)`  
`{`  
`gt @hitterstr |n|`  
`}`  
`gt I'm too lazy to customize Waite's trigger, so mine looks the same as his!`  
`$groupoff breakdown`

### Triggers

If you've made it this far you're in luck. The triggers involved in this
process are a bit less complicated than the aliases. You only need two
of them: a trigger that reacts to each entry in "group", and a trigger
that reacts to the output of "last".

**If you're manually copy/pasting this code, make sure that you put
these triggers in a new group called "breakdown".**

#### Trigger off of "group" output

Here's what the trigger looks like:

`^%1|%2 Lord %3 %4`

and here's the reaction:

`last %3`  

In other words, it just lasts everybody that's in your group.

#### Trigger off of "last" output

Here's what the trigger looks like:

`^%1 the %2 is a %3 level Lord %4, %5`

and here's the reaction:

`$if("%4" = "Berserker")`  
`{`  
`$varadd bzkcount 1`  
`$varadd brutecount 1`  
`$var bzknames "@bzknames %1"`  
`}`  
`$if("%4" = "Warrior")`  
`{`  
`$varadd warcount 1`  
`$varadd brutecount 1`  
`$var warnames "@warnames %1"`  
`}`  
`$if("%4" = "Bodyguard")`  
`{`  
`$varadd bodcount 1`  
`$varadd brutecount 1`  
`$var bodnames "@bodnames %1"`  
`}`  
`$if("%4" = "Paladin")`  
`{`  
`$varadd palcount 1`  
`$varadd brutecount 1`  
`$var palnames "@palnames %1"`  
`}`  
`$if("%4" = "Ranger")`  
`{ `  
`$varadd rancount 1`  
`$varadd brutecount 1`  
`$var rannames "@rannames %1"`  
`}`  
`$if("%4" = "Monk")`  
`{`  
`$varadd moncount 1`  
`$varadd brutecount 1`  
`$var monnames "@monnames %1"`  
`}`  
`$if("%4" = "Shadowfist")`  
`{`  
`$varadd shfcount 1`  
`$varadd brutecount 1`  
`$var shfnames "@shfnames %1"`  
`}`  
`$if("%4" = "Sorcerer")`  
`{`  
`$varadd sorcount 1`  
`$varadd firepowercount 1`  
`$var sornames "@sornames %1"`  
`}`  
`$if("%4" = "Mage")`  
`{`  
`$varadd magcount 1`  
`$varadd firepowercount 1`  
`$var magnames "@magnames %1"`  
`}`  
`$if("%4" = "Wizard")`  
`{`  
`$varadd wzdcount 1`  
`$varadd firepowercount 1`  
`$var wzdnames "@wzdnames %1"`  
`}`  
`$if("%4" = "Psionicist")`  
`{`  
`$varadd psicount 1`  
`$varadd firepowercount 1`  
`$var psinames "@psinames %1"`  
`}`  
`$if("%4" = "Mindbender")`  
`{`  
`$varadd mndcount 1`  
`$varadd firepowercount 1`  
`$var mndnames "@mndnames %1"`  
`}`  
`$if("%4" = "Priest")`  
`{`  
`$varadd prscount 1`  
`$varadd healercount 1`  
`$var prsnames "@prsnames %1"`  
`}`  
`$if("%4" = "Cleric")`  
`{`  
`$varadd clecount 1`  
`$varadd healercount 1`  
`$var clenames "@clenames %1"`  
`}`  
`$if("%4" = "Druid")`  
`{`  
`$varadd drucount 1`  
`$varadd healercount 1`  
`$var drunames "@drunames %1"`  
`}`  
`$if("%4" = "Assassin")`  
`{`  
`$varadd asncount 1`  
`$varadd hittercount 1`  
`$var asnnames "@asnnames %1"`  
`}`  
`$if("%4" = "Rogue")`  
`{`  
`$varadd rogcount 1`  
`$varadd hittercount 1`  
`$var rognames "@rognames %1"`  
`}`  
`$if("%4" = "Black Circle Initiate")`  
`{`  
`$varadd bcicount 1`  
`$varadd hittercount 1`  
`$var bcinames "@bcinames %1"`  
`}`  
`$if("%4" = "Archer")`  
`{`  
`$varadd arccount 1`  
`$varadd hittercount 1`  
`$var arcnames "@arcnames %1"`  
`}`  
`$if("%4" = "Fusilier")`  
`{`  
`$varadd fuscount 1`  
`$varadd hittercount 1`  
`$var fusnames "@fusnames %1"`  
`}`  

And that's it! Feel free to pester me if you can't get it working.

[Waite](User:Waite "wikilink") 16:34, 1 Oct 2006 (CDT)

[Category:RoAClient Scripting](Category:RoAClient_Scripting "wikilink")
