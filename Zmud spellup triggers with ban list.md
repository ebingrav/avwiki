Written by amp/leon/angurth/etc.

Major thanks to rapid for lots of help writing it and to the rest of
{DDM}. Also thanks to the hundreds (1834 when this was written) who
helped test it.

To use this, have someone BUZZ YOURNAME from somewhere in sanctum and be
in sanctum east south. If you want to only have it active for the room
you are in, delete the wheretrigs folder once its been imported, and
delete the "fromafar" trigger in psitrigs folder. Enjoy!

Please don't edit this unless you're very good with triggers.

Copy into a .txt file and import it, or copy paste it all into your zmud
command line. Not tested with versions earlier than 7.21.

*Important note:* The current rules on triggers don't allow ban lists,
as the triggers are supposed to be open to everyone. Use these triggers
at your own risk, or take out the 30 minute ban.

## Code

`#CLASS 0`  
`#VAR spellbanned {someone|peopleyouhate|peoplewhoabuseyourtriggers|{a %1}}`  
`#VAR gettingspelled {gold}`  
`#VAR numbersteeled {10}`  
`#TRIGGER {^You dream of %1 telling you '%2 is the Bacon God!'} {#VAR gettingspelled %1;wake;c steel %2;#show This trigger was made so that people who get renamed to strange things often can still get spells. like ragingbacon, etc.;sleep}`  
`#TRIGGER {^%1 tells you '%2 is the Bacon God!'} {#VAR gettingspelled %2;wake;steel %2;#show This trigger was made so that people who get renamed to strange things often can still get spells. like ragingbacon, etc.;sleep}`  
`#ALARM {4} {}`  
`#BUTTON 1 {abuse} {} {ALL OFF|PSI|CLE} {#T- psitrigs;#T+ offline;#T- cletrigs;#t- wheretrigs;#noop %btncol( 1, white, black);#show ..............ALL OFF...............;#co 10|#T- cletrigs;#T+ psitrigs;#T- offline;#noop %btncol( 1, 1, 10)|#T- psitrigs;#T+ cletrigs;#T- offline;#noop %btncol( 1, black, yellow)} {} {} {} {} {} {} {} {} {} {26} {0|0|0} {} {} "" {} {} {}`  
`#BUTTON 4 {spellups off} {#T+ spellaliases} {spellups on} {#T- spellaliases;#t- wheretrigs} {} {1} {} {} {} {} {} {} {} {} {26} {} {} "" {} {} {}`  
  
`#CLASS 0`  
`#CLASS {spellaliases}`  
`#ALIAS steel {#var gettingspelled %1;wake;say |y|@gettingspelled, you are |bc|-KRA-|n| |w|steel|n|+|bk|iron|n| customer number|br| @numbersteeled|y|!|n|;quicken @myquicken;c steel @gettingspelled;quicken off;tell @gettingspelled |bp|All done!|bg| REMEMBER: I can now steel you once you |bw|BUZZ|bg| me from any room in sanctum, try it!|n| If you didn't get your spells, you probably moved to a different room.;c 'iron skin' @gettingspelled}`  
`#ALIAS awen {wake;c awen %1;sleep}`  
`#ALIAS splitawen {#GAG 10;wake;armor, holy armor, holy aura, bless,;frenzy, protection, and sanctuary.;c armor %1;c 'holy armor' %1;c 'holy aura' %1;c bless %1;tell %1 |bp|sanctuary is the last spell, and its next!|n|;c frenzy %1;tell %1 |bp|ALL DONE! get an alt or some exp!|n|;c sanctuary %1;sleep}`  
`#ALIAS fort {#GAG 5;wake;quicken off;c fort %1;quicken on;sleep;#show Casting Fort on %1;#co 3}`  
`#ALIAS setmyquicken {#var myquicken %1;#show quicken variable set to %1}`  
`#CLASS 0`  
`#CLASS {afarbuzz}`  
`#TRIGGER {You swat at your ear, a buzzing noise is coming from %1.} {#t+ wheretrigs;#IF (%ismember( %lower( %1), @spellbanned)) {tell %1 |bp|You just got spells a second ago|p|, OR are on my banned list. send note on board 2 if theres a problem.} {#VAR gettingspelled %1;#wait 100;stand;where;#ADDITEM spellbanned %lower( %1);#ALARM +30 {#DELITEM spellbanned %lower( %1)}};#alarm 4;#T- afarbuzz}`  
`#CLASS 0`  
`#CLASS {wheretrigs}`  
`#TRIGGER {^(@gettingspelled)(%s)Eastern Wing of the Sanctum} {wake;north;steel @gettingspelled;south;sleep}`  
`#TRIGGER {^(@gettingspelled)(%s)North Room of Eastern Wing} {wake;n;n;steel @gettingspelled;s;south;sleep}`  
`#TRIGGER {^(@gettingspelled)(%s)East Room of Eastern Wing} {wake;north;e;steel @gettingspelled;w;south;sleep}`  
`#TRIGGER {^(@gettingspelled)(%s)South Room of Eastern Wing} {wake;steel @gettingspelled;sleep}`  
`#TRIGGER {^(@gettingspelled)(%s)Upper Room of Eastern Wing} {wake;north;u;steel @gettingspelled;d;s;sleep}`  
`#TRIGGER {^(@gettingspelled)(%s)Lower Room of Eastern Wing} {wake;north;d;steel @gettingspelled;u;south;sleep}`  
`#TRIGGER {^(@gettingspelled)(%s)Western Wing of the Sanctum} {wake;n;w;w;steel @gettingspelled;e;e;s;sleep}`  
`#TRIGGER {^(@gettingspelled)(%s)North Room of Western Wing} {wake;n;w;w;n;steel @gettingspelled;s;e;e;s;sleep}`  
`#TRIGGER {^(@gettingspelled)(%s)West Room of Western Wing} {wake;n;w;w;w;steel @gettingspelled;e;e;e;s;sleep}`  
`#TRIGGER {^(@gettingspelled)(%s)South Room of Western Wing} {wake;n;w;w;s;steel @gettingspelled;n;e;e;s;sleep}`  
`#TRIGGER {^(@gettingspelled)(%s)Upper Room of Western Wing} {wake;n;w;w;u;steel @gettingspelled;d;e;e;s;sleep;#t- wheretrigs;#wait 1000;#t+ wheretrigs}`  
`#TRIGGER {^(@gettingspelled)(%s)Lower Room of Western Wing} {wake;n;w;w;d;steel @gettingspelled;u;e;e;south;sleep;#t- wheretrigs;#wait 1000;#t+ wheretrigs}`  
`#TRIGGER {^(@gettingspelled)(%s)Lower Sanctum} {wake;n;w;d;steel @gettingspelled;u;e;s;sleep;#t- wheretrigs;#wait 1000;#t+ wheretrigs}`  
`#TRIGGER {^(@gettingspelled)(%s)AVATAR's Donation Room} {wake;n;w;d;e;steel @gettingspelled;w;u;e;s;sleep;#t- wheretrigs;#wait 1000;#t+ wheretrigs}`  
`#TRIGGER {^(@gettingspelled)(%s)AVATAR's Sanctum Infirmary} {wake;n;w;d;w;steel @gettingspelled;e;u;e;s;sleep;#t- wheretrigs;#wait 1000;#t+ wheretrigs}`  
`#TRIGGER {^(@gettingspelled)(%s)The Sanctum} {wake;n;w;steel @gettingspelled;e;s;sleep;#t- wheretrigs;#wait 1000;#t+ wheretrigs}`  
`#TRIGGER {You didn't find any @gettingspelled.} {tell %1 hey, sorry but youre not in the area. remember not to try to abuse my triggers- you may have your service temporarily denied for 30 minutes or more.}`  
`#TRIGGER {^(@gettingspelled)(%s)The Midgaard Officers' Benevolent Association Kiosk} {wake;n;w;d;n;steel @gettingspelled;s;u;e;s;sleep;#t- wheretrigs;#wait 1000;#t+ wheretrigs}`  
`#TRIGGER {^(%w)(%s)%1 Room of %3 Wing} {#gagon;#wait 1;#gagoff}`  
`#TRIGGER {^%1 players found.} {#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#gagoff;#show %1 players found in sanctum, fyi!;#co 10}`  
`#CLASS 0`  
`#CLASS {psitrigs}`  
`#TRIGGER {You say '%1, you are -KRA- steel+iron customer number 100!'} {tell %1 congratz, you're my 100th customer! send me a note on board 2 and maybe ill get you a prize.;beep self}`  
`#TRIGGER {From afar,} {#T- heretrigs;#T+ afarbuzz;#t+ wheretrigs;#wait 4000;#T- afarbuzz;#T+ heretrigs;#wait 11000;#t- wheretrigs}`  
`#TRIGGER {%1 twists and folds your limbs until you look like a pretzel.} {#IF (%ismember( %lower( %1), @spellbanned)) {tell %1 You just got spells a second ago, or are on my banned list. beep me if theres a problem.} {fort %1;#ADDITEM spellbanned %lower( %1);#ALARM +30 {#DELITEM spellbanned %lower( %1)}}}`  
`#TRIGGER {@gettingspelled's bones turn into steel.} {#add numbersteeled 1;#ALARM +30 {#DELITEM spellbanned %lower( %1)}}`  
`#TRIGGER {%1 groups automatically.} {wake;quicken off;c regeneration %1;quicken on;sleep}`  
`#CLASS 0`  
`#CLASS {psitrigs|heretrigs}`  
`#TRIGGER {^You swat at your ear, a buzzing noise is coming from %1.} {#IF (%ismember( %lower( %1), @spellbanned)) {tell %1 |bp|You just got spells a second ago|p|,OR are on my banned list. send note on board 2 if theres a problem.} {#VAR gettingspelled %1;#wait 100;steel %1;sleep;#ADDITEM spellbanned {%lower( %1)};#ALARM +30 {#DELITEM spellbanned {%lower( %1)}}}}`  
`#CLASS 0`  
`#CLASS {@gettingspelled is not here!}`  
`#TRIGGER {@gettingspelled is not here!} {tell @gettingspelled Sorry but because you weren't here to get spells your service is being suspended for 30 minutes. Your other alts can still get spells.;#t- {~@gettingspelled is not here!};#wait 10000;#t+ {~@gettingspelled is not here!};#ALARM +1800 {#DELITEM spellbanned %lower( %1)}}`  
`#CLASS 0`  
`#CLASS {offline}`  
`#VAR myquicken {off}`  
`#TRIGGER {^You swat at your ear, a buzzing noise is coming from %1.} {#IF (%ismember( %lower( %1), @spellbanned)) {tell %1 |bp|You just got spells a second ago|p|, OR are on my banned list. send note on board 2 if theres a problem. but most likely you just got spells a second ago! wait a little and try again maybe?} {tell %1 |bg|Sorry I'm busy!|n| please try again later. |bp|beep|n| me if you think you reached this recording in error.}}`  
`#CLASS 0`  
`#CLASS {cletrigs}`  
`#CLASS 0`  
`#ALIAS READTHISONE {write your own triggers here, or try copying them from the psitriggers folder and editingthem for your cle/dru/prs/pal/whatever alt.}`  
`#CLASS 0`

[Category: Zmud Scripting](Category:_Zmud_Scripting "wikilink")
