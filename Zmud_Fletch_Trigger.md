This is my old fletch trigger which I have used for years I used this so
that I could chat between fletch's rather than stacking fletch.

Usage: make <number> <ammo warhead> <ammo type> <poison item>

Example: make 500 piercing arrow

Example: make 500 poison arrow fang

This expects you to have a fletch kit in your inventory, anything that
stops you from fletching turns the trigger off eg. out of mana, fletch
kit runs out, no fletch kit equipped. The trigger will carry on till you
either make more than the number specified or one of the above
conditions happen. It also creates a button to click on to turn the
trigger off which is also bound to F4.

**Warning.** It is advised to be at keyboard while using this trigger,
it also possibly falls foul of the gear collecting rule of [help
trigger](Trigger-Using_Policy "wikilink"). This is why it is not fully
automated with sleeping when out of mana, wearing a fletch kit if you
run out, etc.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

    #CLASS {fletching}
    #ALIAS make {
      #if (!@fletchflag) {#bu fletchbut} {}
      #T+ fletchtrig
      #var fletchflag 1
      #var fletched 0
      #var poison %null
      #var FletchAmount %1
      #var FletchWhat %lower( %2)
      #var FletchType %lower( %3)
      #var poison %lower( %4)
      wear 'fletch kit'
      fletch @FletchType @FletchWhat @poison
      }
    #VAR FletchAmount {500}
    #VAR FletchWhat {ice}
    #VAR FletchType {arrow}
    #VAR fletched {22}
    #VAR poison {}
    #KEY F4 {#bu fletchbut}
    #BUTTON 3 {Fletching - Off} {
      #var fletchflag 1
      #t+ fletchtrig
      } {Fletching - On} {
      #T- fletchtrig
      #var fletchflag 0
      } {} {1} {} {Size} {70} {20} {Pos} {1} {210} {116} {47} {} {} "" {} {} {fletchbut}
    #CLASS 0
    #CLASS {fletching|fletchtrig}
    #TRIGGER {Your toolkit is empty of materials. You will need a replacement.} {#bu fletchbut}
    #TRIGGER {You fail to produce anything worth shooting.} {stand}
    #TRIGGER {Your efforts produced (%*) (%*) (%*)} {
      #ad fletched %1
      #ec made a total of @fletched out of @FletchAmount %2 %3 so far!
      stand
      }
    #TRIGGER {You are already standing.} {#if (@fletched<@FletchAmount) {fletch @FletchType @FletchWhat @poison} {#bu fletchbut}}
    #TRIGGER {You make (%d) (%w) (%w), loaded with (%w).} {
      #ad fletched %1
      #ec made a total of @fletched out of @FletchAmount %2 %3 so far!
      stand
      }
    #TRIGGER {You don't have enough mana to make} {#bu fletchbut}
    #TRIGGER {You discard your empty toolkit.} {#bu fletchbut}
    #TRIGGER {You lack the proper tools.} {#bu fletchbut}
    #CLASS 0

## Designer comments

This originally was posted by me on Oden's now defunct archer site.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
