The scripts listed in this category are written to work in the
\[h://www.zuggsoft.com/page.php?file=zmud/zmudinfo.htm zMUD\] client.

If you would like to request a script, please do so on the
[Requests](Requests "wikilink") page.

If you would like to report a bug or request a feature for a specific
script, please do so on that trigger's talk page.

## Tips for running zMUD

**Windows Vista**

First Right click on the install program and select Run as an
Administrator. Install to a directory other than "Program Files" which
is the default directory, EG. "C:\\zMUD", (This is because of a security
feature in vista that doesn't let programs write to the "Program Files"
directory hence you would not be able to save your settings).

When installed Right click on the zMUD program icon in the start menu,
select properties, select the compatibility tab and select run as
Administrator and then it should work, except for the help system
however go to \[h://www.zuggsoft.com/library/ zMUD Help Library\] for
the Online help system.

**Windows 7**

For some people zMud runs perfect on Windows 7 with no fiddling about,
for others it doesn't.

Here are methods that have worked for other players.

First, you need to go into system properties and turn off Data Execution
Prevention for zmud.exe. Because of Zmud's weird copy protection, the
executable is mostly encrypted on disk, but decrypts itself when loaded
into memory. Naturally that would cause DEP to shut it down, so it needs
to be excluded. Next, you have to run Zmud as an admin once before it
will work properly. After running it as admin once you should be able to
run it normally.

**NOTE:** These are tips that have been found to work by other players
they might not work for you for whatever reason. If these tips do not
work then another client might need to be used, note Zugg has stated
zMUD is not and won't be supported in Vista or 7.

## Scripts

**[Run Stats Counter](Zmud_Run_Stats_Counter.md "wikilink"):** *Counts
numerous things during your runs. Mostly xp gained and lost, and certain
actions performed during runs (bash, trip, rescue, flee, death, etc) and
shows them only when appropriate.*  
**[WikiSearch](Zmud_WikiSearch.md "wikilink"):** *Searches for the
selected text on AvatarWiki when you select and right click it.*  
**[Zmud Prompt Gag](Zmud_Prompt_Gag "wikilink"):** *This 'gags' your
prompt, placing it in the bar between the input and output frames of
your window.*  
Amezyarak used to have a website with various scripts some of which are
the basis for some of the scripts already added to the wiki. It had:

-   Several affect counters (alerts when spells drop, list of spells
    you're missing, etc) (This has been added)
-   Text highlighting code (highlights important things in your window)
    (This has been added)
-   Welcome/gratz triggers for groupies
-   Rescue triggers (Both of these have been added)
-   Track triggers
-   Lots more

**Please note** When importing scripts from a .txt file at times zmud
doesn't always import them right and leaves ;'s instead of a newline
which breaks some of the triggers. If you see complicated trigger/alias
lines being echo'd to the screen you have this problem. To correct find
the offending trigger/alias and look at the Value section and where ever
you find ; delete it and press return.

Clean Working zMUD 7.21
\[h://www.4shared.com/file/byaJAUnG/AutorescueDamageCounter_Workin.html
DamageCounter/Autorescue\]

**Before using triggers, make sure you are familiar with Avatar's
[Trigger Policy](Trigger-Using_Policy.md "wikilink").**

[Category: Scripting](Category:_Scripting "wikilink")
