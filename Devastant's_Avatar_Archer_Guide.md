This is an ad verbatim reproduction of the Archer Guide, written in
2003. Only formatting has been updated.

Due to the age of this document some information has become outdated
and/or some values changed.

## Introduction

I'm writing this in hopes of increasing understanding about how archers
work, and the ways in which they are different from other classes.
Hopefully, this will filter down to our lowest level players and make
the class more accessable to new players.

### TIPS FOR COMBAT

1.  Mob armor reduces archery damage. The most common cause of low
    archery damage is heavy mob armor. Armor piercing (or higher level
    alternatives) arrows ignore armor. Until these become available
    simply avoiding heavily armored mobs is probably the best bet.
2.  Splinter arrows (or higher level alternatives) suffer extra damage
    reduction to enemy armor.
3.  Many bows are awkward to use in close combat. When low level archers
    say they are missing a lot, it's usually because they're using a bow
    with a high melee penalty. See the chart in the "bow types" section.
4.  Archery damage is not increased by the mob being prone (if anything,
    it'd make them harder to hit from any kind of distance) but it IS
    affected by the reduced defense that comes with being bashed.
5.  Once an archer gets target leading, only monks and mobs with shields
    will be able to defend against them.

### GEAR TIPS

1.  Fletching "rare" arrow types (such as explosive) causes fatigue. A
    good idea is to fletch them 5 or 6 times, or until you get 2 or more
    failures in a row. Note that failures do not add to or refresh the
    fatigue.
2.  The amount of bonus given by archery bonus items is based on the
    level of the item, and the hitroll of the wearer,
3.  Arrows that never survive hitting a foe may survive and be found in
    the room if they miss. Archers should remember to check the ground
    after every fight.

## Weapons

### Item values

Bows are items of type \# 31. Occasionally you may come across standard
weapon type items (type \# 5) which are NAMED bows. A quick identify
will tell the difference. Bows have 4 value fields like all other items,
which signify:

`value0-        bow type (see below)`  
`value1-        minimum damage`  
`value2-        maximum damage`  
`value3-        draw strength or restring count`

Min and max damage are randomly determined by the mud when the item is
loaded.

If draw strength has not been specified in the area file, it is assigned
when the item is loaded, based on bow type.

### Bow Types

There are seven bow types, although only the first four are common.

`#  name            ammo    damage  meleemod  SCAT  LONG  restring  DRAW`  
`0  short bow       arrow   medium  moderate  y     n     full      12`  
`1  long bow        arrow   high    extreme   y     y     full      16`  
`2  light crossbow  bolt    low     none      n     n     limited   n/a`  
`3  heavy crossbow  bolt    high    moderate  n     y     limited   n/a`  
`4  compound bow    arrow   high    high      y     y     limited   10`  
`5  sling           stone   low     low       y     n     none      n/a`  
`6  gun (arbalest)  bullet  medium  none      n     y     none      n/a`

NOTES: Heavy crossbows have a chance of failure to reload in time to
fire. Compound bows are prone to breakage and have low hps. Short races
get a bonus to using slings.

**Ammo type** determines the type of ammo required. See below.

**Damage** refers to the increase given to the randomly rolled min and
max damage of the item. Those with "low" listed will have roughly the
same min/max as a normal weapon of the same level. The damage mod helps
scale archer power across the low mort levels, and should serve to
builders as a general guide to what bow types to use at what levels.
Anything listed "high" should never load lower than level 30.

**Meleemod** refers to the reduction in accuracy suffered when using
that bow type in direct melee combat. The penalty isn't a reduction in
hitroll, but rather a flat out % chance of missing.

**SCAT** tells whether or not that bow type can be used to fire
scattershot.

**LONG** tells whether or not that bow type can be used to fire a
longshot.

**Restring** refers to how much restringing the item can handle.
Crossbow restrings are limited by the item's level. Compound bow
restrings are limited because they are very prone to breakage.

**DRAW** refers to the default draw strength of an item of that type.
This effects how often a bow can be restrung. If a builder wished to
make a bow especially restringable or un-restringable, the draw strength
for that item could be set higher or lower than the default.

## Ammo

### Item values

Ammunition items are type \# 32. They are requried to fire bow type
items for both players and mobs. If a mob lacks the right ammo, it will
remove the bow item it is trying to use. Although they are single items,
ammo items represent many actual arrows/bolts/etc. and are subject to
many unigue rules. They will merge with others of the same type whenever
they are put in the same location, be it floor, inventory or container.
Currently the only way to split them is to drop a specific number. Mobs
don't use up ammo, so it isn't necessary to give them more than a
handful of arrows.

`value0-        warhead type (see below)`  
`value1-        ammo type (see below)`  
`value2-        poison type (see below)`  
`value3-        quantity`

**Warhead type** refers to exploding, piercing, etc. See the table
below.

**Ammo type** refers to arrows or bolts. See the table below.

The poison field only matters if the arrow has the ability to poison.

### Warhead Types

`#   name          level    notes`  
`0   standard      4`  
`1   steel         13       nigh unbreakable`  
`2   barbed        23       barb affect`  
`3   splinter      101(51)  weak vs armor`  
`4   flaming       42       `  
`5   explosive     101(51)  rare`  
`6   piercing      50       ignores armor`  
`7   poison        33       carries poison`  
`8   ice           125      `  
`9   lightning     125      ignores shield, stuns`  
`10  ebony         125      ignores armor and shield, rare, can shoot thru doors`  
`11  mithril       125      nigh unbreakable`  
`12  sableroix     4        quest prize arrow for low mortals`  
`13  glass         1        quest prize arrow for higher players`  
`14  cluster       250      weak vs armor`  
`15  sunray        250      ignores armor and shield, rare`  
`16  displacement  250      ignores armor`  
`17  terror        250      barb affect, carries poison`

### Ammo types

`#  name    used by`  
`0  arrow   long bow, short bow, compound bow`  
`1  bolt    light crossbow, heavy crossbow`  
`2  stone   sling`  
`3  bullet  gun`

### Poison types

`#   name         level`  
`0   poison       5`  
`1   toxin        20`  
`2   biotoxin     35`  
`3   virus        50`  
`4   venom        50`  
`5   plague       50`  
`6   necrotia     124`  
`7   heartbane    124`  
`8   doom toxin   124 (mob only)`  
`9   psychotia    249`  
`10  liquid pain  249`  
`11  pyrovirus    249`

[Category:Archers](Category:Archers "wikilink")
[Category:Fusiliers](Category:Fusiliers "wikilink")
[Category:Assassins](Category:Assassins "wikilink") [Category:Missile
Weapons](Category:Missile_Weapons "wikilink")
[Category:Ammunition](Category:Ammunition "wikilink")
