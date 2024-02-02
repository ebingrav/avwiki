    Syntax: prompt                          Syntax: prompt2
    Syntax: prompt all                      Syntax: prompt2 all
    Syntax: prompt <%*>                     Syntax: prompt2 <%*>
    Syntax: prompt show                     Syntax: prompt2 show

    PROMPT without an argument will turn your prompt on or off. The same
    for PROMPT2.  Prompt2 will duplicate everything that prompt already
    does. It is simply an additional prompt to work with or toggle on and off.

    PROMPT ALL will set your prompt to "<hits mana moves>" but not turn it on/off.

    PROMPT SHOW will display both the underlying code behind your prompt
    and prompt2.

    PROMPT <%*> where the %* are the various variables you may set yourself.


            %c :  Display your current number of items
            %C :  Display your carry capacity of items
            %h :  Display your current hits
            %H :  Display your maximum hits
            %l :  Display your current weight
            %L :  Display your weight capacity
            %m :  Display your current mana
            %M :  Display your maximum mana
            %v :  Display your current moves
            %V :  Display your maximum moves
            %x :  Display your current experience
            %t :  Display the current time
            %T :  Display your experience needed to gain level
            %g :  Display your gold held
            %a :  Display your alignment
            %r :  Display the room name you are in
            %w :  Display the current hits of your monitor target
            %W :  Display the maximum hits of your monitor target
            %u :  Display the name of your monitor target
            %n :  Carriage return. Prompt continues on next line
            %o :  Displays your mana as a % of your maximum
            %p :  Displays your hits as a % of your maximum
            %q :  Displays your current progression towards the next level
            %P :  Displays your monitor target's hits as a %
            %S :  Displays your current surge level
            %Q :  Displays your current quicken level
            %A :  Displays your current augment level
            %s :  Displays your current amount of lag from a skill or spell
            %e :  Displays your current position
            %j :  Display your current Inner Qi (Lord+ mon/shf only)
            %J :  Display your maximum Inner Qi (Lord+ mon/shf only)
            %k :  Display your current Outer Qi (Lord+ mon/shf only)
            %K :  Display your maximum Outer Qi (Lord+ mon/shf only)
            %R :  Display the vnum you are in (ANGEL+ ONLY)
            %z :  Display the area name you are in (ANGEL+ ONLY)
            %i :  Display your visibility bit (IMMORTAL ONLY)
            %% :  Inserts a % into your prompt

    Example:  PROMPT <%hhp %mm %vmv> 
            Will set your prompt to a format like "<10hp 100m 100mv>"

    Color codes can be used in prompts.

    See also: COLORS, PROMPT SYMBOLS

## Comments

Prompts can be extremely useful if done correctly. With the addition of
color codes, effective spacing and separation symbols, players can put
all of the necessary information in two lines of prompt. Players can
also use their [client](:Category:Scripting.md "wikilink") software to
grab information from their prompts to put in information bars, gauges
and other fancy displays.

## Sample Prompt

I've found that the following prompt works very well for me for most of
my alts, caster, tank or hitter. Other players may have other examples.

    prompt <|w|%h|n|/%Hhp |w|%m|n|/%Mma %vv |y|%T|n|> %s lag 
    prompt2 %w %P surge %S %n

Notice there is one space just after the word 'lag'

This will put all of your info on a one line prompt, with your current
hps and mana in white, TNL in yellow and everything else in green.  
<tt><span style="color:#008000; background:#000000">\<<span style="color:#ffffff; background:#000000">582<span style="color:#008000; background:#000000">/582hp
<span style="color:#ffffff; background:#000000">1997<span style="color:#008000; background:#000000">/1997ma
663v
<span style="color:#808000; background:#000000">1500<span style="color:#008000; background:#000000">\>
0 lag - - surge off </span>

[Category: Commands](Category:_Commands "wikilink") [Category:
Configuration](Category:_Configuration "wikilink")
