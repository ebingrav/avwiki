1.  ALIAS gagprompt {prompt prompt %h %H %m %M %v %V %w %W %T %S %Q %A
    %n}

`#TRIGGER {^(%*)prompt (%d) (%d) (%d) (%d) (%d) (%d) (%x) (%x) (%d) (%x) (%x) (%x)} {#ga;#if {%pos(AFK,%1)<>0} {#no} {#no};#st {%if( %10<150, ~<LEVEL GEAR~>)%1 %2 / %3 hps " " %4 / %5 m " " %6 / %7 mv " "%if( %8!=~-, %8 ~/ %9 mon " ") %10 tnl "   " (@netxp @cnt) "   " surge %11, quicken %12, augment %13}} {prompt}`

This trigger requires your prompt to be in a specific format, so if you
use different clients at different locations, it can be a bit tricky. It
shows hit points, mana, movement, monitor when you're monitoring
someone, tnl, surge, quicken and augment. Oh, and xp gained on the run
you're on if you have the xp count trigger.

If you don't want to use the xp count trigger, remove the @exp from the
second line. If you want to remove surge, quicken and augment, remove
the

`"   " surge %11, quicken %12, augment %13`

in the \#TRIGGER line.

The **gagprompt** command is required for this. Be sure to add it just
like this, or it can cause problems. Do not remove the line break (%n)
or it won't show properly.

## Comments

I would recommend using 'config -blind' with this, to prevent most of
the blank line spam.

Also, you can replace @exp by whatever the variable name is of your xp
counter. Or remove it if you don't have one. I personally use (@exp
@cnt) where @cnt is for the kill count.

You can really work with this!

I switched some things around and found my sweet spot.

prompt prompt %h %H %m %M %v %V %T %w %W %l %L %n

1.  TRIGGER {^(%\*)prompt (%d) (%d) (%d) (%d) (%d) (%d) (%d) (%\*) (%\*)
    (%d) (%d)} {#ga;#if {%pos(AFK,%1)} {#no} {#no};#st {%if( %8\<150,
    \~\<LEVEL GEAR\~\>)%1 %2/%3hp" " %4/%5m " " %6/%7mv " " %9/%10mon "
    " %11/%12 lb " " (%8)}

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
