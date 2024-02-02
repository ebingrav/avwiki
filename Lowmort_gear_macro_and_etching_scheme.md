Running 4 lowmorts at a time on multiday can give you a headache. After
each multiday I've been trying to find ways to cut down annoyance time
and have more playing time ;) This etching scheme and the script below
cuts out the most annoying and time consuming part of running lowmorts
on a multiday... keeping track of about 250 pieces of equipment. If you
have your gear etched and bagged according to the etching scheme, the
script will always make you wear the best gear available to you on the
level - and within a second.

I use 3 locker alts. One carries bags of ac gear, one hit gear and one
arc gear. The bags are etched according to the gear kit and level range
they contain. The etch is b\<gear\>\<levelrange\>, bac20 for a bag of ac
gear you can wear from level 20-30, barc40 for a bag of arc gear you can
wear from 40-50 and so on. All of them have level 50 hero gear in a bag
as well, bags go like bhit47, bac47, barc47.

I etch items according to the gear kit they're in (ac, arc, hit) and the
level... Pay attention though, I didn't use the level of the item for
the etch but the level you can wear/wield it at. Wields are kept in a
seperate bag, called bwields. I put bows and shields in the barc/bac
bags as I would with the other archer/ac items. This list of examples
should cover all etches in use by the script:

    item                                     etch
    level 27 gargoyle bracer b7              ac24
    level 28 gargoyle bracer b7 -3           ac25
    level 29 gargoyle bracer b8 -3           ac26
    level 14 pants of illusion               hit11
    level 36 spidersilk bracers              arc33
    level 20 simple hunter's bow             arc15
    level 23 sharpened golden claw           wield18
    level 25 offhand 11/11 moonblade dagger  offhand20
    level 30 silver cavalry shield           ac27
    level 50 flaming pants                   hit47
    bag of lvl 10-20 wearable hitgear        bhit10
    bag of lvl 20-30 wearable arcgear        barc20 
    bag of lvl 30-40 wearable acgear         bac30
    bag of lvl 50 hero hit gear              bhit47
    bag of lvl 50 hero arc gear              barc47
    bag of wields                            bwields

Since I have just 2 nicely enchanted offhands for the 2 wars i dont
really use a loop for those but just "wear offhand". The three gargoyle
bracers here serve as an example... As long as you make sure the higher
level wrist item is better than the lower level wrist item, you can put
as many of them in your bags as you like, the script will make you wear
the best you can at the level. My bags of lowmort hit gear actually
contain everything for 2 sets, the second warrior will just use the best
of what's left over by the first ;)

Now I can start manipulating all these items with loops. What I do is
"drop all.barc", "drop all.bac" or "drop all.bhit=drop bwields" with the
locker carrying the gear, then hit the fixgear script with the lowmort,
then pick up the bags with the locker(!). This is the script, i put some
example output below it.

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''
    ; lowmort gear fixing macro :)
    ; /fixgear <gear> <level>   Wears hit, ac, arc or mana gear for level
    ; /fixgear <gear> 0         Will put the gear into the appropriate bags

    /def -i fixgear = \
            /let gear %1%;\
            /let level %2%;\
            rem all%;\
            /if (level >= 47) \
                    get all b%{gear}47%;\
                    wear all.%{gear}47%;\
            /endif%;\
            put all.%{gear}47 b%{gear}47%;\
            /let level_counter=%;\
            /test (level_counter:=level, level_counter > 46) & (level_counter:=46)%;\
            /while (level_counter > 10) \
                    /if ((level_counter == level) | (level_counter == 46)) \
                            get all b%{gear}$[(level_counter/10)]0%;\
                    /elseif (mod(level_counter, 10) == 9) \
                            put all.%{gear}$[(level_counter/10)+1] b%{gear}$[(level_counter/10)+1]0%;\
                            get all b%{gear}$[(level_counter/10)]0%;\
                    /endif%;\
                    wear all.%{gear}%{level_counter}%;\
                    /test --level_counter%;\
            /done%;\
    ;cleanup line
            /for c 1 4 put all.%{gear}%%{c} b%{gear}%%{c}0%;\
            /if (gear =~ "hit") \
                    get all bwields%;\
                    /for c $[level-9] %{level} wield wield%%{c}%;\
                    put all.wield bwields%;\
                    wear offhand%;\
            /endif%;\
    ;try to wear some level eq i might have
            wear "cult hat bonk"%;\
            wear "golden robe"%;\
            wear "wreath laurel"

Example (trimmed) output of "/fixgear hit 32":

    remove all
    get all bhit30
    wear all.hit32
    wear all.hit31
    wear all.hit30
    put all.hit3 bhit30
    get all bhit20
    wear all.hit29
    ... [snip] ...
    wear all.hit20
    put all.hit2 bhit20
    ... [snip, that'll continue to level 11] ...
    get all bwields
    wield wield23
    wield wield24
    ... [snip] ...
    wield wield32
    put all.wield bwields
    wear offhand
    wear "golden robe"
    wear "wreath laurel"

For armor pieces I use the fact that "wear all.\*" will not switch out
gear you're already wearing, for wields I use the fact that wield
wield20 \*will\* switch out the wield19. What's left to pack manually is
4 wreathes, a golden robe, 2 shields to bash with and a bunch of arrows
to shoot.

As a useful example but easier than the fixgear script, this would clean
up all hit gear:

    put all.wield bwields
    put all.hit47 bhit47
    /for c 1 4 put all.hit%c bhit%{c}0

Or for the arc gear it would be

    put all.arc47 barc47
    /for c 1 4 put all.arc%c barc%{c}0

To label all items, I wrote [label.tf](label.tf "wikilink") and used it
like

    /for c 1 20 /label %c.ac

HTH

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
