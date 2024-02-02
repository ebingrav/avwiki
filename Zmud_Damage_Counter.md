Tested for zmud 7.21

This trigger and alias set will track damage dealt and received for all
on a member list. For each line of combat the hit is stored by damage
verb if the person attacking was on the list, and a running damage total
is kept. Damage values were taken from [Damage](Damage.md "wikilink").
Report options include a display of {damage done/ average hit/ damage
taken} or a breakdown of all hits by verb. You may display these for
yourself, everyone or a partial selection. Latest triggers should catch
anything with a name and a damage verb.

## Code

''For quick input copy this section and paste it into a \*.txt file.
Then, in zMUD, go to "Settings -\> File -\> Import

`#CLASS {dcounter}`  
`#ALIAS dcadd {#additem DCtrack %1;#additem DCnames %1;#echo %1 added to the damage counting list.}`  
`#ALIAS dcrep {#YESNO "What report would you like to show?" {Short report (total damage done and taken) :dcrep1} {Breakdown of damage verbs:dcrep2}}`  
`#ALIAS dcclear {#FO @DCtrack {#UNVAR %i};#VAR DCtrack "you";#VAR DCnames "";dcadd your;#ECHO Damagecounter reset. Please re-add groupies.}`  
`#ALIAS dcrep2 {#if (!@DC.repto) {dcrepto};#YESNO "Who would you like to report for?" {Yourself:#VAR DC.reportfor "your"} {Everyone:#VAR DC.reportfor @DCnames} {Partial:#VAR DC.reportfor %pick( p:Who would you like to display reports for?:, @DCnames)};#LOOPDB @you {#var your.%key %eval( %db( @your, %key) + %val)};#var you "";#FO @DC.reportfor {#var %i.out "";#FO @dverbshort {#if (%db( @{%i}, %I)) {#var %i.out %concat( %db( @{%i}, out), " ", %I, ~(, ~|c~|, %db( @{%i}, %I), ~|n~|, ~))}};@DC.repto Breakdown for %if( %i="your", Myself, %upper( %left( %i, 1))%right( %i, 1)):%db( @{%i}, out)}}`  
`#ALIAS dcrep1 {#if (!@DC.repto) {dcrepto};#YESNO "Who would you like to report for?" {Yourself:#VAR DC.reportfor "your"} {Everyone:#VAR DC.reportfor @DCnames} {Partial:#VAR DC.reportfor %pick( p:Who would you like to display reports for?:, @DCnames)};#LOOPDB @you {#var your.%key %eval( %db( @your, %key) + %val)};#var you "";#var DC.total 0;#var DC.taken 0;#var DC.totalcount 0;#var DC.takencount 0;#FO @DCnames {#if ( %db( @{%i}, terminal)) {#var %i.total %eval( %db( @{%i}, dealt) + %db( @{%i}, dealt)/(%db( @{%i}, attacks) - %db( @{%i}, terminal))* %db( @{%i}, terminal))} {#var %i.total %db( @{%i}, dealt)};#if (@DC.total < %db( @{%i}, total)) {#var DC.total %db( @{%i}, total)};#if (@DC.taken < %db( @{%i}, taken)) {#var DC.taken %db( @{%i}, taken)};#var DC.totalcount %eval( @DC.totalcount + %db( @{%i}, total));#var DC.takencount %eval( @DC.takencount + %db( @{%i}, taken))};#FO @DC.reportfor {#if ( %db( @{%i}, terminal)) {#var %i.verb %item( @dverbs, %eval( @verbBinary(1,139,%eval(%db(@{%i},dealt)/(%db(@{%i},attacks)- %db( @{%i}, terminal)))) -1))} {#var %i.verb %item( @dverbs, %eval( @verbBinary(1,139,%eval(%db(@{%i},dealt)/%db(@{%i},attacks))) -1))};@DC.repto %if( %i="your", I, %upper( %left( %i, 1))%right( %i, 1)): Dealt->%if( %db( @{%i}, total) = @DC.total, ~|bc~|, ~|c~|)%db( @{%i}, total)|n|~(%eval( %db( @{%i}, total)*100/@DC.totalcount)%~) Averaged~[|bk|%replace( %db( @{%i}, verb), "=", "-")|n|~] Took->%if( %db( @{%i}, taken) = @DC.taken, ~|br~|, ~|r~|)%db( @{%i}, taken)|n|~(%eval( %db( @{%i}, taken)*100/@DC.takencount)%~)}}`  
`#ALIAS dcs {dcclear;#if (!@DCreport) {@DCreport = gt} {};#t+ "%w is leading (%d) player[ s]with";groupstat}`  
`#ALIAS dcrepto {#prompt DC.repto "Enter the channel to output to, eg gt, say ect."}`  
`#VAR dverbshort {nil|pathetic|weak|punishing|surprising|amazing|astonishing|mauling|decimating|devastating|pulverizing|maiming|eviscerating|mutilating|disemboweling|dismembering|massacring|mangling|demolishing|obliterating|annihilating|eradicating|vaporizing|destructive|extreme|porcine|divine|daunting|terminal}`  
`#VAR dvalues {0|2|4|8|10|14|18|22|26|30|34|38|42|46|49|55|60|65|70|75|80|85|90|95|100|110|120|130|140|150|160|170|180|190|200|225|250|275|300|325|350|375|400|425|450|475|500|540|574|606|675|730|769|810|884|915|1000|1100|1200|1300|1400|1500|1600|1700|1800|1900|2000|2200|2400|2600|2800|3000|3200|3400|3600|3800|4100|4500|5007|5901|5902|6200|6500|7000|7500|7800|8200|8500|9000|9500|10000|11000|12000|13000|14000|15000|16500|18000|19000|20000|21000|22000|23000|24000|25000|26000|27000|28000|29000|30000|31000|32000|33000|34000|35000|36000|37000|38000|39000|40000|41000|42000|43000|44500|47000|48000|50000|51000|53000|55000|57000|59000|61000|65000|70000|75000|80000|100000|0}`  
`#VAR dverbs {nil|pathetic|weak|punishing|surprising|amazing|astonishing|mauling|MAULING|MAULING*|MAULING**|MAULING***|decimating|DECIMATING|DECIMATING*|DECIMATING**|DECIMATING***|devastating|DEVASTATING|DEVASTATING*|DEVASTATING**|DEVASTATING***|pulverizing|PULVERIZING|PULVERIZING*|PULVERIZING**|PULVERIZING***|maiming|MAIMING|MAIMING*|MAIMING**|MAIMING***|eviscerating|EVISCERATING|EVISCERATING*|EVISCERATING**|EVISCERATING***|mutilating|MUTILATING|MUTILATING*|MUTILATING**|MUTILATING***|disemboweling|DISEMBOWELING|DISEMBOWELING*|DISEMBOWELING**|DISEMBOWELING***|dismembering|DISMEMBERING|DISMEMBERING*|DISMEMBERING**|DISMEMBERING***|massacring|MASSACRING|MASSACRING*|MASSACRING**|MASSACRING***|mangling|MANGLING|MANGLING*|MANGLING**|MANGLING***|demolishing|DEMOLISHING|DEMOLISHING*|DEMOLISHING**|DEMOLISHING***|obliterating|OBLITERATING|OBLITERATING*|OBLITERATING**|OBLITERATING***|annihilating|ANNIHILATING|ANNIHILATING*|ANNIHILATING**|ANNIHILATING***|ANNIHILATING***<|ANNIHILATING***<<|ANNIHILATING***<<<|ANNIHILATING***<<<<|eradicating|ERADICATING|ERADICATING*|ERADICATING**|ERADICATING***|ERADICATING***<|ERADICATING***<<|ERADICATING***<<<|ERADICATING***<<<<|vaporizing|VAPORIZING|VAPORIZING*|VAPORIZING**|VAPORIZING***|VAPORIZING***<|VAPORIZING***<<|VAPORIZING***<<<|VAPORIZING***<<<<|destructive|DESTRUCTIVE|DESTRUCTIVE*|DESTRUCTIVE**|DESTRUCTIVE***|DESTRUCTIVE****|DESTRUCTIVE****<|DESTRUCTIVE****<<|DESTRUCTIVE****<<<|DESTRUCTIVE****<<<<|DESTRUCTIVE***<<<<=|DESTRUCTIVE**<<<<==|DESTRUCTIVE*<<<<===|DESTRUCTIVE<<<<====|extreme|EXTREME|EXTREME*|EXTREME**|EXTREME***|EXTREME****|EXTREME****<|EXTREME****<<|EXTREME****<<<|EXTREME****<<<<|EXTREME***<<<<=|EXTREME**<<<<==|EXTREME*<<<<===|EXTREME<<<<====|porcine|PORCINE|PORCINE*|PORCINE**|PORCINE***|PORCINE***<|PORCINE***<<|PORCINE***<<<|PORCINE***<<<<|divine|daunting|terminal}`  
`#VAR verbBinary {%if( %1 = %2, %1, %if( %eval( %item( @dvalues, %eval( (%1+%2)/2.0)) > %3), @verbBinary(%1, %eval( (%1+%2)/2.0),%3), @verbBinary( %eval( (%1+%2)/2.0),%2,%3)))}`  
`#TRIGGER {^Welcome back to the AVATAR System, {lord|lady|hero} (%w).} {#var dcCurChar %lower( %1)}`  
`#TRIGGER {^Welcome back to the AVATAR System (%w),} {#var dcCurChar %lower( %1)}`  
`#TRIGGER {^({@DCtrack})[^>;] %w[^>;]with[*>= ]({@dverbshort})([*<= ])%w[!.]} {#var %1.%2 %eval( @%1.%2 + 1);#var %1.dealt %eval( @%1.dealt + %item( @dvalues, %ismember( %concat( %2, %3), @dverbs)));#var %1.attacks %eval( @%1.attacks + 1)} "" {notrig}`  
`#TRIGGER {^%w[^>;] ({@DCtrack})[^>;]with[*>= ]({@dverbshort})([*<= ])%w[!.]} {#var %1.taken %eval( @%1.taken + %item( @dvalues, %ismember( %concat( %2, %3), @dverbs)))}`  
`#TRIGGER {%w is leading (%d) player[ s]with} {#if (%1 < 10) {#loop %1 {#temp {^ %i|??? ???? (&%wDCtemp1)} {#if (%lower( @DCtemp1) != @dcCurChar) {dcadd %lower( @DCtemp1)}}}} {#loop 1,9 {#temp {^ %i|??? ???? (&%wDCtemp1)} {#if (%lower( @DCtemp1) != @dcCurChar) {dcadd %lower( @DCtemp1)}}};#loop 10,%1 {#temp {^%i|??? ???? (&%wDCtemp1)} {#if (%lower( @DCtemp1) != @dcCurChar) {dcadd %lower( @DCtemp1)}}}};#t- "%w is leading (%d) player[ s]with";group} "" {disable}`  
`#MENU {DC Add} {dcadd %lower( %selword)} ""`  
`#MENU {DC Clear} {dcclear} ""`  
`#MENU {DC List} {#ec -;#ec --- CHARACTERS ON THE DC LIST ---;#fo @DCnames {#ec %i}} ""`  
`#CLASS 0`

## Usage

dcs- Reset all variables and add everyone in your group (including
yourself)

dcrep- Brings up a menu with report options

dcrepto- Select your output channel (gt, say ect.)

## How It Works

`#ALIAS dcadd {#additem DCtrack %1;#additem DCnames %1;#echo %1 added to the damage counting list.}`

Alias to add people to the group variable, dcs will call this
automatically when you use it.

`#ALIAS dcrep {#YESNO "What report would you like to show?" {Short report (total damage done and taken) :dcrep1} {Breakdown of damage verbs:dcrep2}}`  
`#ALIAS dcrep2 {#if (!@DC.repto) {dcrepto};#YESNO "Who would you like to report for?" {Yourself:#VAR DC.reportfor "your"} {Everyone:#VAR DC.reportfor @DCnames} {Partial:#VAR DC.reportfor %pick( p:Who would you like to display reports for?:, @DCnames)};#LOOPDB @you {#var your.%key %eval( %db( @your, %key) + %val)};#var you "";#FO @DC.reportfor {#var %i.out "";#FO @dverbshort {#if (%db( @{%i}, %I)) {#var %i.out %concat( %db( @{%i}, out), " ", %I, ~(, ~|c~|, %db( @{%i}, %I), ~|n~|, ~))}};@DC.repto Breakdown for %if( %i="your", Myself, %upper( %left( %i, 1))%right( %i, 1)):%db( @{%i}, out)}}`  
`#ALIAS dcrep1 {#if (!@DC.repto) {dcrepto};#YESNO "Who would you like to report for?" {Yourself:#VAR DC.reportfor "your"} {Everyone:#VAR DC.reportfor @DCnames} {Partial:#VAR DC.reportfor %pick( p:Who would you like to display reports for?:, @DCnames)};#LOOPDB @you {#var your.%key %eval( %db( @your, %key) + %val)};#var you "";#var DC.total 0;#var DC.taken 0;#var DC.totalcount 0;#var DC.takencount 0;#FO @DCnames {#if ( %db( @{%i}, terminal)) {#var %i.total %eval( %db( @{%i}, dealt) + %db( @{%i}, dealt)/(%db( @{%i}, attacks) - %db( @{%i}, terminal))* %db( @{%i}, terminal))} {#var %i.total %db( @{%i}, dealt)};#if (@DC.total < %db( @{%i}, total)) {#var DC.total %db( @{%i}, total)};#if (@DC.taken < %db( @{%i}, taken)) {#var DC.taken %db( @{%i}, taken)};#var DC.totalcount %eval( @DC.totalcount + %db( @{%i}, total));#var DC.takencount %eval( @DC.takencount + %db( @{%i}, taken))};#FO @DC.reportfor {#if ( %db( @{%i}, terminal)) {#var %i.verb %item( @dverbs, %eval( @verbBinary(1,139,%eval(%db(@{%i},dealt)/(%db(@{%i},att acks)- %db( @{%i}, terminal)))) -1))} {#var %i.verb %item( @dverbs, %eval( @verbBinary(1,139,%eval(%db(@{%i},dealt)/%db(@{%i},attacks))) -1))};@DC.repto %if( %i="your", I, %upper( %left( %i, 1))%right( %i, 1)): Dealt->%if( %db( @{%i}, total) = @DC.total, ~|bc~|, ~|c~|)%db( @{%i}, total)|n|~(%eval( %db( @{%i}, total)*100/@DC.totalcount)%~) Averaged~[|bk|%replace( %db( @{%i}, verb), "=", "-")|n|~] Took->%if( %db( @{%i}, taken) = @DC.taken, ~|br~|, ~|r~|)%db( @{%i}, taken)|n|~(%eval( %db( @{%i}, taken)*100/@DC.takencount)%~)}}`

These 3 aliases report all the info. The first one just asks you which
of the other two you want to use. DCrep2 handles the breakdown, looking
through each person in your group variable, then looping through their
variable concatenating each verb with a count into one string and then
outputting it. DCrep1 first loops through everyone, totals their damage,
and stores the highest damage taken and dealt. Next it reports for each
person selected, giving their numeric damage total, their equivilent
average damage verb and their total damage taken. It also highlights the
top damage taken and dealt.

`#ALIAS dcclear {#FO @DCtrack {#UNVAR %i};#VAR DCtrack "you";#VAR DCnames "";dcadd your;#ECHO Damagecounter reset. Please re-add groupies.}`

Pretty simple, clears out the variables but automatically re-adds you.

`#ALIAS dcs {dcclear;#if (!@DCreport) {@DCreport = gt} {};#t+ "%w is leading (%d) player[ s]with";groupstat}`

Has a little check like some of the other aliases have to make sure that
you've set a channel to announce the results on for reports and then
turns on a trigger to automatically count how many groupies there are
and add them, then lastly calls the groupstat command to set off the
previous trigger.

`#VAR dverbshort {nil|pathetic|weak|punishing|surprising|amazing|astonishing|mauling|decimating|devastating|pulverizing|maiming|eviscerating|mutilating|disemboweling|dismembering|massacring|mangling|demolishing|obliterating|annihilating|eradicating|vaporizing|destructive|extreme|porcine|divine|daunting|terminal}`  
`#VAR dvalues {0|2|4|8|10|14|18|22|26|30|34|38|42|46|49|55|60|65|70|75|80|85|90|95|100|110|120|130|140|150|160|170|180|190|200|225|250|275|300|325|350|375|400|425|450|475|500|540|574|606|675|730|769|810|884|915|1000|1100|1200|1300|1400|1500|1600|1700|1800|1900|2000|2200|2400|2600|2800|3000|3200|3400|3600|3800|4100|4500|5007|5901|5902|6200|6500|7000|7500|7800|8200|8500|9000|9500|10000|11000|12000|13000|14000|15000|16500|18000|19000|20000|21000|22000|23000|24000|25000|26000|27000|28000|29000|30000|31000|32000|33000|34000|35000|36000|37000|38000|39000|40000|41000|42000|43000|44500|47000|48000|50000|51000|53000|55000|57000|59000|61000|65000|70000|75000|80000|100000|0}`  
`#VAR dverbs {nil|pathetic|weak|punishing|surprising|amazing|astonishing|mauling|MAULING|MAULING*|MAULING**|MAULING***|decimating|DECIMATING|DECIMATING*|DECIMATING**|DECIMATING***|devastating|DEVASTATING|DEVASTATING*|DEVASTATING**|DEVASTATING***|pulverizing|PULVERIZING|PULVERIZING*|PULVERIZING**|PULVERIZING***|maiming|MAIMING|MAIMING*|MAIMING**|MAIMING***|eviscerating|EVISCERATING|EVISCERATING*|EVISCERATING**|EVISCERATING***|mutilating|MUTILATING|MUTILATING*|MUTILATING**|MUTILATING***|disemboweling|DISEMBOWELING|DISEMBOWELING*|DISEMBOWELING**|DISEMBOWELING***|dismembering|DISMEMBERING|DISMEMBERING*|DISMEMBERING**|DISMEMBERING***|massacring|MASSACRING|MASSACRING*|MASSACRING**|MASSACRING***|mangling|MANGLING|MANGLING*|MANGLING**|MANGLING***|demolishing|DEMOLISHING|DEMOLISHING*|DEMOLISHING**|DEMOLISHING***|obliterating|OBLITERATING|OBLITERATING*|OBLITERATING**|OBLITERATING***|annihilating|ANNIHILATING|ANNIHILATING*|ANNIHILATING**|ANNIHILATING***|ANNIHILATING***<|ANNIHILATING***<<|ANNIHILATING***<<<|ANNIHILATING***<<<<|eradicating|ERADICATING|ERADICATING*|ERADICATING**|ERADICATING***|ERADICATING***<|ERADICATING***<<|ERADICATING***<<<|ERADICATING***<<<<|vaporizing|VAPORIZING|VAPORIZING*|VAPORIZING**|VAPORIZING***|VAPORIZING***<|VAPORIZING***<<|VAPORIZING***<<<|VAPORIZING***<<<<|destructive|DESTRUCTIVE|DESTRUCTIVE*|DESTRUCTIVE**|DESTRUCTIVE***|DESTRUCTIVE****|DESTRUCTIVE****<|DESTRUCTIVE****<<|DESTRUCTIVE****<<<|DESTRUCTIVE****<<<<|DESTRUCTIVE***<<<<=|DESTRUCTIVE**<<<<==|DESTRUCTIVE*<<<<===|DESTRUCTIVE<<<<====|extreme|EXTREME|EXTREME*|EXTREME**|EXTREME***|EXTREME****|EXTREME****<|EXTREME****<<|EXTREME****<<<|EXTREME****<<<<|EXTREME***<<<<=|EXTREME**<<<<==|EXTREME*<<<<===|EXTREME<<<<====|porcine|PORCINE|PORCINE*|PORCINE**|PORCINE***|PORCINE***<|PORCINE***<<|PORCINE***<<<|PORCINE***<<<<|divine|daunting|terminal}`

These 3 variables provide the means to lookup damage. First the damage
is looked up in dverbs, then that position is looked up in dvalues.
Dverbshort is used for pattern matching and in dcrep2 to loop through
each groupies database variable which counts hits per verb.

`#VAR verbBinary {%if( %1 = %2, %1, %if( %eval( %item( @dvalues, %eval( (%1+%2)/2.0)) > %3), @verbBinary(%1, %eval( (%1+%2)/2.0),%3), @verbBinary( %eval( (%1+%2)/2.0),%2,%3)))}`

Is a function variable. It uses a binary search to look through dvalues
and find the one closest to the damage given (mainly for looking up your
average damage). This position is then used with dverbs to give you a
word for your numeric damage.

`#TRIGGER {^Welcome back to the AVATAR System, {lord|lady|hero} (%w).} {#var dcCurChar %lower( %1)}`  
`#TRIGGER {^Welcome back to the AVATAR System (%w),} {#var dcCurChar %lower( %1)}`

These two just store what your characters name is so you don't get added
when the rest of the group does to the tracking variable. (Its setup to
automatically track you)

`#TRIGGER {^({@DCtrack})[^>;] %w[^>;]with[*>= ]({@dverbshort})([*<= ])%w[!.]} {#var %1.%2 %eval( @%1.%2 + 1);#var %1.dealt %eval( @%1.dealt + %item( @dvalues, %ismember( %concat( %2, %3), @dverbs)));#var %1.attacks %eval( @%1.attacks + 1)} "" {notrig}`  
`#TRIGGER {^%w[^>;] ({@DCtrack})[^>;]with[*>= ]({@dverbshort})([*<= ])%w[!.]} {#var %1.taken %eval( @%1.taken + %item( @dvalues, %ismember( %concat( %2, %3), @dverbs)))}`

The meat and potato triggers. Pretty gnarly looking but they seem to
work :) The first one catches all damage anyone in the variable @DCtrack
deals, adds an attack to the damage verb that they hit, adds damage to
their total damage tracker and adds to their total attacks (just so it
doesn't have to add them up later) The second catches all damage done to
you. It looks up the position in @dverbs, maps it to @dvalues and adds
them to your previous total. (Both of these triggers use that same
method)

`#TRIGGER {%w is leading (%d) player[ s]with} {#if (%1 < 10) {#loop %1 {#temp {^ %i|??? ???? (&%wDCtemp1)} {#if (%lower( @DCtemp1) != @dcCurChar) {dcadd %lower( @DCtemp1)}}}} {#loop 1,9 {#temp {^ %i|??? ???? (&%wDCtemp1)} {#if (%lower( @DCtemp1) != @dcCurChar) {dcadd %lower( @DCtemp1)}}};#loop 10,%1 {#temp {^%i|??? ???? (&%wDCtemp1)} {#if (%lower( @DCtemp1) != @dcCurChar) {dcadd %lower( @DCtemp1)}}}};#t- "%w is leading (%d) player[ s]with";group} "" {disable}`

The trigger that catches group members as mentioned earlier. It creates
a trigger for each group slot depending on how many people are in the
group and that trigger calls the dcadd alias on them

`#MENU {DC Add} {dcadd %lower( %selword)} ""`  
`#MENU {DC Clear} {dcclear} ""`  
`#MENU {DC List} {#ec -;#ec --- CHARACTERS ON THE DC LIST ---;#fo @DCnames {#ec %i}} ""`

Last but not... well ok they are the least... but just a couple of right
click menus that you don't really need but may make it easier to add a
groupie that may not of been in the group, clear the list or see who's
on the list.

[Category: Zmud Scripting](Category:_Zmud_Scripting "wikilink")
