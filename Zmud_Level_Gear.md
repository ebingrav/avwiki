This is the level gear set of triggers from the amezyarak site.

Automatically wear level gear when close to level and Echoes your gains
to the screen.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

    #CLASS {LevelGear}
    #VAR lvlgear 0
    #ALIAS level {
      #IF (@lvlgear) {levelgearoff} {levelgearon}}
      }
    #ALIAS levelgearon {
      get all sat
      wear 'wide hat'
      wear 'silver cruci'
      put 'med just' sat
      put 'cherub helm' sat
      #var lvlgear 1
      }
    #ALIAS levelgearoff {
      get all sat
      wear 'cherub helm'
      wear 'med just'
      put 'wide hat' sat
      put 'silver cruci' sat
      #var lvlgear 0
      }
    #TRIGGER {^Your powers increase!!!$Your gain is: (%d)/(%d) hp, (%d)/(%d) m, (%d)/(%d) mv (%d)/(%d) prac.} {
      #T+ leveltest
      level
      #ec LEVEL INFO: <%1/%3/%5/%7>
      }
    #CLASS 0
    #CLASS {LevelGear|leveltest}
    #TRIGGER {(%d)/(%d)hp (%d)/(%d)m (%d)/(%d)v (%d)t (%*)/(%*)mo} {
      #VAR TNL %7
      #IF (@TNL<100) {
        level
        #T- leveltest
        } {}
      }
    #CLASS 0

## Designer comments

All the credits for the original trigger set go to amezyarak.

-   Only useful for hero runs due to the level up trigger being the hero
    line.
-   If killing mobs worth over 100 xp this trigger will not always
    trigger.
-   Gear switches (tank to hit, etc) will mess this trigger set up.
-   The levelgearon and levelgearoff aliases will need changing for your
    respective gear.
-   This assumes you are using the following prompt, prompt (%h/%Hhp
    %m/%Mm %v/%Vv %Tt %w/%Wmo)%n, therefore the trigger line will have
    to be changed for your prompt assuming you have Tnl in your prompt.

Originally emoted to the mud your gains when you leveled changed by me
to this method when the no trigger rule was in affect. Works well with
the [Zmud Tell Window](Zmud_Tell_Window "wikilink") script.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
