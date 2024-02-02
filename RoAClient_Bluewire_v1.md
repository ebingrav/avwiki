## Description

This profile is aware of current *tier, class, race, sanc, sleep,
current mob, tank, combat, surge, augment, quicken, xp, priest xp, hp,
mana, moves, and monitorhp.* It includes a *promptset* alias and even
login/pwd aliases so you can quickly log onto the alt you need. Other
Important aliases include a *kill trigger, mid-round, start fight*
(first action after kill trigger), and, yes, a *backstab trigger* for
you rogues out there. There is the infrastructure for a heal/spell bot
built-in, but not the actual groups required. A simple and universal
*steel bot* is also included that checks for class and tier. The final
alias worth mentioning is *hardreset*. This alias resets all variables
to their default which include helps when they would be applied.

## The Script

![](Alias.ndx‎ "Alias.ndx‎") ![](Triggrps.ndx‎ "Triggrps.ndx‎")
![](Trigs.ndx‎ "Trigs.ndx‎") ![](Vars.ndx‎ "Vars.ndx‎")

------------------------------------------------------------------------

## ----Alias.ndx

    V45
    log
    $var login "%0"

    #
    pwd
    $var pwd "%0"

    #
    xp
    gt Current tally since last run reset of xp is @xp, and @xpday since my last daily reset of xp.

    $if ("@class" = "prs")

    {

    gt Tul has granted me @prsxp this run and @prsxpday overall.

    }

    #
    tl
    $var trustlist "Trust list:  @trustlist | %0

    #
    tank
    $var tank "%0"

    #
    hardreset
    $var combat 0

    $var trustlist "@login"

    $var tank "@login"

    $var rescue "Rescue List: "

    $var mob "None"

    $var sanc 0

    $var tier 0

    $var class "default"

    $var monitarget "@login"

    $var sleep 0

    $var rescue "Rescue List: "

    $var surge 0

    $var mr "t self mr <midround attack> --can leave blank--"

    $var sf "t self sf <2nd action after kill> --can leave blank--"

    $var kt "t self kt <Kill type, surprise, bs, kill, etc> --can leave blank--"

    $var bst "t self bst <backstab targets> --use equals to seperate targets--"

    $var xp 0

    $var xpday 0

    $var prsxp 0

    $var prsxpday 0

    $var login "admin"

    $var pwd "admin"

    #
    bst
    $var bst "%0"

    #
    numconv
    $if ("@fullmonihp" ! "-")

    {

    $var intfullmonihp @fullmonihp

    $var intmonihp @monihp

    }

    $if ("@minhp" ! "-")

    {

    $var intmaxhp @maxhp

    $var intminhp @minhp

    }

    $break

    #
    promptset
    prompt <%h/%Hhp %m/%Mm %v/%Vmv><%Ttnl><%a>%n

    prompt2 <%w/%Whp --- MonitorTarget>lag: %s%n

    #
    radd
    $var rescue "Rescue List (|BK|gt add me|N|):  @rescue | %0"

    #
    login
    $var login "%0"

    #
    kt
    $var kt "%0"

    #
    sf
    $var sf "%0"

    #
    mr
    $var mr "%0"

    #
    trust
    $var trustlist "@trustlist | %0"

    #

------------------------------------------------------------------------

## ----Triggrps.ndx

    V44
    Lord
    0
    Login
    0
    Psi
    0
    Temporary
    0
    Group
    0
    Rescue
    0
    Awareness
    0
    Routed
    0
    combat
    0

------------------------------------------------------------------------

## ----Trigs.ndx

    V460
    0
    0
    0

    0

    Awareness
    0

    0
    0
    <%1/%2hp ---- %3>lag: %4
    $var fullmonihp "%2"

    $var monihp "%1"

    $var monitarget "%3"

    $break

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You stop quickening your spells.
    $var quicken 0

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You will now attempt to quicken your spells by %1 rounds.
    $var quicken %1

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You stop augmenting your healing spells.
    $var augment 0

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You will now attempt to augment your healing spells by %1.
    $var augment %1

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^Your Iron Monk style fades.
    $var sanc 0

    #
    0
    0
    0

    0

    combat
    0

    0
    0
    ^You will now attempt to surge your spells by %1.
    $var surge %1

    #
    0
    0
    0

    0

    combat
    0

    0
    0
    ^You follow %1 %2.
    $if ("%1" = "@tank")

    {

    @bst

    }

    #
    0
    0
    0

    0

    combat
    0

    0
    0
    ^You attempt to bash into %0 and you fall down!
    $if (@combat = 1)

    {

    bash

    }

    #
    0
    0
    0

    0

    Temporary
    0

    0
    0
    ^You see your quarry's trail head %1 from here!
    gt Track Direction: |R|%1||N| 



    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^Tul-Sith grants you %1 exp!
    $var prsxp %1

    $var prsxpday %1

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You receive %1 experience points.
    $varadd xp %1

    $varadd xpday %1

    #
    0
    0
    0

    0

    Psi
    0

    0
    1
    %0 falls to the ground, lifeless.
    $break

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^The protective aura fades from around your body.
    $var sanc 0

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You wake and stand up.
    $var sleep 0

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You sleep.
    $var sleep 1

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You are already standing
    $var sleep 0

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    %0 has been calmed by %1!
    $var combat 0

    #
    0
    1
    0

    0

    Awareness
    0

    0
    0
    Level %1 %3 %4 
    $if ("(" ? %3)

    {

    $break

    }

    $var tier 1


    $if ("Archer" ? "%4")

    {

    $var class "arc"

    }

    $if ("Assassin" ? "%4")

    {

    $var class "asn"

    }

    $if ("Berserker" ? "%4")

    {

    $var class "bzk"

    }

    $if ("Bladed" ? "%4")

    {

    $var class "bld"

    }

    $if ("Black" ? "%4")

    {

    $var class "bci"

    }

    $if ("Bodyguard" ? "%4")

    {

    $var class "bod"

    }

    $if ("Cleric" ? "%4")

    {

    $var class "cle"

    }

    $if ("Druid" ? "%4")

    {

    $var class "dru"

    }

    $if ("Fusilier" ? "%4")

    {

    $var class "fus"

    }

    $if ("Mage" ? "%4")

    {

    $var class "mag"

    }

    $if ("Mindbender" ? "%4")

    {

    $var class "mnd"

    }

    $if ("Monk" ? "%4")

    {

    $var class "mon"

    }

    $if ("Paladin" ? "%4")

    {

    $var class "pal"

    }

    $if ("Priest" ? "%4")

    {

    $var class "prs"

    }

    $if ("Psionic" ? "%4")

    {

    $var class "psi"

    }

    $if ("Ranger" ? "%4")

    {

    $var class "ran"

    }

    $if ("Rogue" ? "%4")

    {

    $var class "rog"

    }

    $if ("Shadow" ? "%4")

    {

    $var class "shf"

    }

    $if ("Sorc" ? "%4")

    {

    $var class "sor"

    }

    $if ("Stormlord" ? "%4")

    {

    $var class "stm"

    }

    $if ("Warrior" ? "%4")

    {

    $var class "war"

    }

    $if ("Wizard" ? "%4")

    {

    $var class "wzd"

    }

    #
    0
    0
    0

    0

    Group
    0

    0
    0
    ^*%1* tells the group 'pp %0'
    $if (@tier = 1)

    {

    $break

    }

    $if ("@class" = "bzk")

    {

    $break

    }

    $if ("@class" = "asn")

    {

    $break

    }

    $if ("@class" = "bci")

    {

    $break

    }

    c 'portal' %0

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    %1 wakes you.
    $var sleep 0

    #
    0
    0
    0

    0

    Group
    0

    0
    0
    %1 enters a %2.
    $if ("@tank" = "%1")

    {

    enter %2

    }

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    %0 is in AWE of %1!
    $var combat 0

    #
    0
    0
    0

    0

    Group
    0

    0
    0
    %1 removes you from its group.
    $var tank "@login"

    prompt2 <%w/%Whp ---- @login>lag: %s%n#
    0
    0
    0

    0

    Lord
    0

    0
    0
    You feel a slight headache growing stronger...
    $if ("@class" ? "arc")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" = "asn")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "bld")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "bci")

    {

    c 'psisting' self

    c 'detect e'

    }

    $if ("@class" ? "bod")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "cle")

    {

    c 'cause light' self

    c 'detect e'

    }

    $if ("@class" ? "cle")

    {

    c 'cause l' self

    c 'detect e'

    }

    $if ("@class" ? "fus")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "mag")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "mnd")

    {

    c 'psisting' self

    c 'detect e'

    }

    $if ("@class" ? "mon")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "pal")

    {

    c 'cure l'

    c 'detect e'

    }

    $if ("@class" ? "prs")

    {

    c 'cure l'

    c 'detect e'

    }

    $if ("@class" ? "psi")

    {

    c 'psisting' self

    c 'detect e'

    }

    $if ("@class" ? "ran")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "rog")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "shf")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "sor")

    {

    c 'leech' self

    c 'detect e'

    }

    $if ("@class" ? "stm")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "war")

    {

    c 'magic m' self

    c 'detect e'

    }

    $if ("@class" ? "wzd")

    {

    c 'magic m' self

    c 'detect e'

    }

    #
    0
    0
    0

    0

    Rescue
    0

    1
    0
    %1's pierce strikes 
    $if ("%3" !? "@rescue")

    {

    $break

    }

    $if ("%3" ? "@rescue")

    {

    rescue %3

    }

    #
    0
    0
    0

    0

    Rescue
    0

    1
    0
    %1's attacks strike %3 %4 times,
    $if ("%3" !? "@rescue")

    {

    $break

    }

    $if ("%3" ? "@rescue")

    {

    rescue %3

    }

    #
    0
    0
    0

    0

    Rescue
    0

    1
    0
    %1's attacks haven't hurt %3!
    $if ("%3" !? "@rescue")

    {

    $break

    }

    $if ("%3" ? "@rescue")

    {

    rescue %3

    }

    #
    0
    0
    0

    0

    Rescue
    0

    1
    0
    %1's attack strikes %3 1 time,
    $if ("%3" !? "@rescue")

    {

    $break

    }

    $if ("%3" ? "@rescue")

    {

    rescue %3

    }

    #
    0
    0
    0

    0

    Rescue
    0

    0
    0
    %1 tells the group 'add me'
    $if ("%1" !? "@rescue")

    {

    $alias "radd %1"

    gt %1 has been added to the rescue list.

    }

    #
    0
    0
    0

    0

    Group
    0

    0
    0
    %1 beckons for you to follow %2.
    $if ("%1" ? "@trustlist")

    {

    wa

    follow %1

    }

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    Level %1(%2) %3 %4
    $if (%1 = 51)

    {

    $var tier 2

    }

    $if (%1 = 125)

    {

    $var tier 3

    }

    $if ("Archer" ? "%4")

    {

    $var class "arc"

    }

    $if ("Assassin" ? "%4")

    {

    $var class "asn"

    }

    $if ("Berserker" ? "%4")

    {

    $var class "bzk"

    }

    $if ("Bladed" ? "%4")

    {

    $var class "bld"

    }

    $if ("Black" ? "%4")

    {

    $var class "bci"

    }

    $if ("Bodyguard" ? "%4")

    {

    $var class "bod"

    }

    $if ("Cleric" ? "%4")

    {

    $var class "cle"

    }

    $if ("Druid" ? "%4")

    {

    $var class "dru"

    }

    $if ("Fusilier" ? "%4")

    {

    $var class "fus"

    }

    $if ("Mage" ? "%4")

    {

    $var class "mag"

    }

    $if ("Mindbender" ? "%4")

    {

    $var class "mnd"

    }

    $if ("Monk" ? "%4")

    {

    $var class "mon"

    }

    $if ("Paladin" ? "%4")

    {

    $var class "pal"

    }

    $if ("Priest" ? "%4")

    {

    $var class "prs"

    }

    $if ("Psionic" ? "%4")

    {

    $var class "psi"

    }

    $if ("Ranger" ? "%4")

    {

    $var class "ran"

    }

    $if ("Rogue" ? "%4")

    {

    $var class "rog"

    }

    $if ("Shadow" ? "%4")

    {

    $var class "shf"

    }

    $if ("Sorc" ? "%4")

    {

    $var class "sor"

    }

    $if ("Stormlord" ? "%4")

    {

    $var class "stm"

    }

    $if ("Warrior" ? "%4")

    {

    $var class "war"

    }

    $if ("Wizard" ? "%4")

    {

    $var class "wzd"

    }

    #
    0
    0
    0

    0

    Login
    0

    0
    0
    ^What name shall you be known by, adventurer?
    @login

    @pwd

    =

    =

    =

    =

    =

    =

    score

    =

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    %0<%1/%2hp %3/%4m %5/%6mv><%7tnl><%8>
    $var minm %3


    $var maxm %4

    $var minhp "%1"


    $var maxhp "%2"


    $var tnl %7


    $break

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^Your attack
    $var combat 1

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    %0 is DEAD!!
    $var combat 0

    #
    0
    0
    0

    0

    Group
    0

    0
    0
    ^Okay, you are now monitoring %1.
    $var monitarget "%1"

    prompt2 <%w/%Whp ---- %1>lag: %s%n

    #
    6
    0
    0

    0

    Routed
    0

    0
    0
    ^{%1}%0
    #
    5
    0
    0

    0

    Routed
    0

    0
    0
    ^You dream
    #
    5
    0
    0

    0

    Routed
    0

    0
    0
    telling you
    #
    5
    0
    0

    0

    Routed
    0

    0
    0
    tells the group
    #
    5
    0
    0

    0

    Routed
    0

    0
    0
    tells you
    #
    5
    0
    0

    0

    Routed
    0

    0
    0
    you tell
    #
    5
    0
    0

    0

    Routed
    0

    0
    0
    You tell
    #
    0
    0
    0

    0

    Psi
    0

    0
    1
    You swat at your ear, a buzzing noise is coming from %1.
    $if (@tier = 1)

    {

    $break

    }

    $if (mnd ? @class)

    {

    $if (@sleep = 0)

    {

    c 'steel' %1

    $break

    }

    }

    $if (psi ? @class)

    {

    $if (@sleep = 0)

    {

    c 'steel' %1

    $break

    }

    }

    $if (@tier = 3)

    {

    $if (bci ? @class)

    {

    $if (@sleep = 0)

    {

    c 'steel' %1

    $break

    }

    }

    $if (mnd ? @class)

    {

    $if (@sleep = 1)

    {

    wake

    c 'steel' %1

    sl

    $break

    }

    }

    $if (psi ? @class)

    {

    $if (@sleep = 1)

    {

    wake

    c 'steel' %1

    sl

    $break

    }

    }

    $if (@tier = 3)

    {

    $if (bci ? @class)

    {

    $if (@sleep = 1)

    {

    wake

    c 'steel' %1

    sl

    $break

    }

    }

    $if (mnd ? @class)

    {

    $if (@sleep = 2)

    {

    stand

    c 'steel' %1

    rest

    $break

    }

    }

    $if (psi ? @class)

    {

    $if (@sleep = 2)

    {

    stand

    c 'steel' %1

    rest

    $break

    }

    }

    $if (@tier = 3)

    {

    $if (bci ? @class)

    {

    $if (@sleep = 2)

    {

    stand

    c 'steel' %1

    rest

    $break

    }

    }

    t %1 @class don't know Steel Skeleton. Translation: I don't cast Steel Skeleton.

    #
    0
    0
    0

    0

    combat
    0

    0
    0
    ^You stop surging your spells.
    $var surge 0

    #
    0
    0
    0

    0

    combat
    0

    0
    0
    %1 is killing %2.
    $if ("%1" = "@tank")

    {

    $var mob "%2"

    $var combat 1

    @kt %2

    @sf %2

    }

    #
    0
    0
    0

    0

    combat
    0

    0
    0
    ^Your attack
    @mr

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You are already in sanctuary.
    $var sanc 1

    #
    0
    0
    0

    0

    Group
    0

    0
    0
    Okay, you are now monitoring %1.
    $var monitarget "%1"

    prompt2 <%w/%Whp ---- %1>lag: %s%n

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You are already Glowing!
    $var sanc 1

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You are surrounded by a %1 aura.
    $if ("ink" ? "%1")

    {

    $break

    }

    $var sanc 1

    #
    0
    0
    0

    0

    Awareness
    0

    0
    0
    ^You concentrate on the Iron Monk style.
    $var sanc 1

    #
    0
    0
    0

    0

    Group
    0

    0
    0
    ^You join %1's group.
    $var tank "%1"

    #
    0
    0
    0

    0

    Group
    0

    0
    0
    You stop following %1.
    $var tank "@login"

    #

------------------------------------------------------------------------

## ----Vars.ndx

    V470
    intminhp
    1
    1732
    quicken
    1
    0
    augment
    1
    0
    intmaxhp
    1
    1732
    intmonihp
    1
    0
    intfullmonihp
    1
    0
    maxhp
    0
    1732#
    fullmonihp
    0
    -#
    monihp
    0
    -#
    tnl
    1
    668
    minhp
    0
    1732#
    maxm
    1
    3332
    minm
    1
    777
    prsxpday
    1
    0
    prsxp
    1
    0
    xpday
    1
    101
    xp
    1
    101
    bst
    0
    t self bst <backstab targets> --use equals to seperate targets--#
    surge
    1
    0
    sleep
    1
    0
    class
    0
    default#
    tier
    1
    0
    sanc
    1
    0
    mob
    0
    None#
    rescue
    0
    Rescue List: #
    tank
    0
    admin#
    trustlist
    0
    admin#
    combat
    1
    0
    time
    0
    15:12:57#
    date
    0
    05/25/12#
    pwd
    0
    admin#
    user
    0
    #
    mr
    0
    t self mr <midround attack> --can leave blank--#
    kt
    0
    t self kt <Kill type, surprise, bs, kill, etc> --can leave blank--#
    sf
    0
    t self sf <2nd action after kill> --can leave blank--#
    monitarget
    0
    Torrein#
    login
    0
    admin#

## Author Notes

Remember to review your alias list. There are tons of variables that are
set automatically, but through the aliases, you can manually and quickly
change variables on the fly. There are several assumed configurations: -
config -battleself, -battleother (or +battlenone) - an in-game *bash*
alias LORD ONLY - For Migraine: knowledge of Magic Missile (priority),
Psisting, Cause light, Cure light, and leech //The knowledge of these
spells is magic missile for no-inclass/arcane, psisting for psionic
classes and likewise, cure light and cause light for divine classes.

## Designer comments

This is my first avatar wiki contribution. It provides a decent profile
for RoA that is both aware and a skeleton template for any class and
tier. It is missing several functions such as a spell bot and heal bot,
but there are plans to add those triggers/aliases/variables in the
future as they are already a part of my usual client.

    Hit me up ingame: Setus, Resetus, Unsetus, Desetus, Presetus, and more!

[Category:RoAClient_Scripting](Category:RoAClient_Scripting "wikilink")
