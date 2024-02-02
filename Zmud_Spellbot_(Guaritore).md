This script produces a spellbot with a queuing system. It was designed
using zmud v7.21

## Code

The prompt used for this bot is:  
Spells: %m mana  
If you use a different prompt you must edit the trigger that captures
your current mana from your prompt.  
I'm not 100% sure everything has been captured in this trigger, so if
any parts don't work properly please let me know.

`#CLASS {spells}`  
`#VAR mana ""`  
`#VAR spellcount ""`  
`#VAR spelltarget ""`  
`#VAR spelllist ""`  
`#TRIGGER {%1 tells the group 'full'} {#var spelltarget %1;#if (!%ismember( %1, @spelllist)) {gt You're not on the list. GT 'next' without the quotes to be added};#if (%ismember( %1, @spelllist)) {#IF ((%1 != %item( @spelllist, 1)) & (@mana < 1500)) {gt You'll have to wait your turn and I need more mana. @mana/1600};#IF ((%1 != %item( @spelllist, 1)) & (@mana >= 1500)) {gt You'll have to wait your turn};#IF ((%1 = %item( @spelllist, 1)) & (@mana < 1500)) {gt You're next but I need more mana. @mana/1500};#IF ((%1 = %item( @spelllist, 1)) & (@mana >= 1500)) {#ad spellcount 1;wake;tell %1 water will be last;c 'holy sight' %1;c foci %1;c fort %1;c invin %1;c awen %1;tell %1 all done;c wa %1;#delitem spelllist %1;sleep;#ad spells/list/%1 1}}}`  
`#TRIGGER {%1 tells the group 'split'} {#var spelltarget %1;#if (!%ismember( %1, @spelllist)) {gt You're not on the list. GT 'next' without the quotes to be added};#if (%ismember( %1, @spelllist)) {#IF ((%1 != %item( @spelllist, 1)) & (@mana < 1500)) {gt You'll have to wait your turn and I need more mana. @mana/1600};#IF ((%1 != %item( @spelllist, 1)) & (@mana >= 1500)) {gt You'll have to wait your turn};#IF ((%1 = %item( @spelllist, 1)) & (@mana < 1500)) {gt You're next but I need more mana. @mana/1500};#IF ((%1 = %item( @spelllist, 1)) & (@mana >= 1500)) {#ad spellcount 1;wake;tell %1 water will be last;c 'holy sight' %1;c foci %1;c fort %1;c invin %1;c armor %1;c 'holy armor' %1;c 'holy aura' %1;c bless %1;c sanc %1;tell %1 all done;c wa %1;#delitem spelllist %1;#ad spells/list/%1 1;sleep}}}`  
`#TRIGGER {%1 tells the group 'next'} {#additem spelllist %1;gt |BC|People in line: @spelllist;gt |BC|gt |BR|split full |BC|or |BR|help|BC| to see a list of all other spells/commands;gt |BM|Please check your alignment, i cannot fullspell evil chars;gt |BC|gt 'split awen' without the quotes if you didn't check your align and just need the awen spells}`  
`#TRIGGER {joins your group.} {gt |BC|gt |BR|'next'|BC| without the quotes to be added to the queue for |BR|FULL |BC|and |BR|SPLIT |BC|spells;gt |BC|gt |BR|help |BC|for a list of all available spells/commands;gt |BC|if someone is not responding or queue is not empty when it should be gt clear then gt next to rejoin the queue.}`  
`#TRIGGER {%1 tells the group 'help'} {gt |BC|gt split, full, invin, invis, sanc, frenzy, fort, foci, wa(waterbreathing), invig, holy sight, remove curse, div, div2, div3, div4, div5, split awen(just awen spells), pp(portal), next(to be added to the queue), line(for list of queue), clear(clears queue DO NOT ABUSE)}`  
`#TRIGGER {%1 tells the group 'sanc'} {wake;c sanc %1;sleep}`  
`#TRIGGER {%1 tells the group 'split awen'} {wake;c armor %1;c 'holy armor' %1;c 'holy aura' %1;c bless %1;c sanc %1;sleep}`  
`#TRIGGER {%1 tells the group 'invin'} {wake;c invin %1;sleep}`  
`#TRIGGER {%1 tells the group 'div'} {wake;c div %1;sleep}`  
`#TRIGGER {%1 tells the group 'div2'} {wake;augment 2;c div %1;augment off;sleep}`  
`#TRIGGER {%1 tells the group 'div3'} {wake;augment 3;c div %1;augment off;sleep}`  
`#TRIGGER {%1 tells the group 'div4'} {wake;augment 4;c div %1;augment off;sleep}`  
`#TRIGGER {%1 tells the group 'div5'} {wake;augment 5;c div %1;augment off;sleep}`  
`#TRIGGER {%1 stops following you.} {#delitem spelllist %1}`  
`#TRIGGER {%1 tells the group 'clear'} {#var spelllist "";sleep}`  
`#TRIGGER {%1 tells the group 'remove curse'} {wake;c 'remove curse' %1;sleep}`  
`#TRIGGER {%1 now follows you.} {group %1}`  
`#TRIGGER {Your senses return to normal.} {wake;heighten;sleep}`  
`#TRIGGER {%1 tells the group 'line'} {gt People in line: @spelllist}`  
`#TRIGGER {tells you} {#cap spellcap}`  
`#TRIGGER {You dream of %1 telling you} {#cap spellcap}`  
`#TRIGGER {%1 tells the group 'fort'} {wake;c fort %1;sleep}`  
`#TRIGGER {%1 tells the group 'foci'} {wake;c foci %1;sleep}`  
`#TRIGGER {%1 tells the group 'wa'} {wake;c wa %1;sleep}`  
`#TRIGGER {tells the group 'pp %1'} {wake;c portal %1;sleep}`  
`#TRIGGER {%1 tells the group 'invig'} {wake;c invig %1;sleep}`  
`#TRIGGER {%1 tells the group 'frenzy'} {wake;c frenzy %1;sleep}`  
`#TRIGGER {You fail to heighten your senses.} {wake;heighten;sleep}`  
`#TRIGGER {%1 tells the group 'holy sight'} {wake;c 'holy sight' %1;sleep}`  
`#TRIGGER {%1 tells the group 'invis'} {wake;c invis %1;sleep}`  
`#TRIGGER {Spells: %1 mana} {#var mana %1}`  
`#CLASS 0`

You can copy and paste this code into your command line or copy and
paste it into a text file and import it.

[Category: Zmud Scripting](Category:_Zmud_Scripting "wikilink")
