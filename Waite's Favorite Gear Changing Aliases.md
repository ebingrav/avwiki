This gear changing system uses one bag for each different set of gear
you would like to be able to switch between (typically hit, tank, and
mana), plus one additional "junk" bag. Here is a brief rundown of how
the system works:

1.  Put any random gear in your inventory into your junk bag
2.  Remove your current set of gear and put it in the appropriate bag
3.  Get the desired set of gear out of its bag and wear all

## Benefits of using this system

-   Because you're using "get all" and "wear all", you don't need to
    switch your aliases every time you get a new piece of gear
-   Using "get all", "wear all", "put all" keeps your aliases less
    spammy than some other options.
-   You don't have to etch each individual piece of gear
-   Any random gear you collect is automatically dumped into your junk
    bag, keeping your inventory clean and tidy

## Drawbacks of using this system

-   You have to have an alias for each possible set switch. For example,
    you can't just have a single alias "managear" that will put you into
    mana gear. Instead, you need an alias to swap from tank to mana, and
    another alias to swap from hit to mana.

<!-- -->

-   Because these aliases use "put all", there's a chance that one of
    the bags containing a set of gear will end up in the junk bag.
    Therefore, your aliases need to be able to handle this situation,
    making them a little more complex than the outline described above.

<!-- -->

-   If you type the wrong gear alias, then one of your sets of gear may
    end up in the wrong bag. This, however, is an easy problem to solve
    (see below)

## Example Aliases

Suppose you have four bags, named "tank", "mana", "hitgear", and "junk".
The following server-side aliases will implement this method of gear
switching:

` alias tankmana       put all junk:get hitgear junk:get tank junk:get mana junk:rem all:`  
` put all tank:get junk tank:get mana tank:get hitgear tank:get all mana:wear all`

` alias tankhit        put all junk:get hitgear junk:get tank junk:get mana junk:rem all:`  
` put all tank:get junk tank:get mana tank:get hitgear tank:get all hitgear:wear all`

` alias manatank       put all junk:get hitgear junk:get tank junk:get mana junk:rem all:`  
` put all mana:get junk mana:get tank mana:get hitgear mana:get all tank:wear all`

` alias manahit       put all junk:get hitgear junk:get tank junk:get mana junk:rem all:`  
` put all mana:get junk mana:get tank mana:get hitgear mana:get all hit:wear all`

` alias hittank        put all junk:get hitgear junk:get tank junk:get mana junk:rem all:`  
` put all hit:get junk hit:get mana hit:get tank hit:get all tank:wear all`

` alias hitmana        put all junk:get hitgear junk:get tank junk:get mana junk:rem all:`  
` put all hitgear:get junk hitgear:get tank hitgear:get mana hitgear:get all mana:wear all`

Level gear is also a snap. Just etch each piece of level gear "level",
and stow it in your junk bag. Then, use the following two aliases to
switch in and out of level gear:

` alias lvlgear        get all.level junk:rem all:wear all.level:wear all`  
` alias lvloff         rem all.level:put all.level junk:wear all`

If you type the wrong alias a set of your gear may end up in the wrong
bag. For example, if you are current in tank gear, and you type
"manatank", all your tank gear will end up in your mana gear bag, and
you'll be naked. Fortunately this is not a hard problem to solve. In
this example, just type "get 1. mana" until you get all your tank gear
out of the wrong bag.

[Category:Gear Changing
Aliases](Category:Gear_Changing_Aliases "wikilink")
