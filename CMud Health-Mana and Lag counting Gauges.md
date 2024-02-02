This is a script to gag your prompt and put those values into colorful
gauges

It also creates a gauge to count down any time you go into lag.

First, we need to set your prompt for the triggers to grab. To use this,
we set an alias so all you need to do is type "promptset"

-   DON'T FORGET TO TURN ON PROMPT2\*\*

<!-- -->

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <alias name="promptset" copy="yes">
        <value>prompt ~[ ~%h/~%H ~%m/~%M ~%v/~%V ~%g ~%T~*~%w/~%W~*~%xxpz~]
    prompt2 ~%e~&lt;~%s~&gt;~%n</value>
      </alias>
    </cmud>

Now we can go ahead and grab the values and gag it

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger priority="212" prompt="true" copy="yes">
        <pattern><![CDATA[^&flags%s{-|}&curhp~/&maxhp%s{-|}&curmana~/&maxmana%s{-|}&curmv~/&maxmv%s&curgold%s&tnl~*{-|}&curmonhp~/&maxmonhp~*(&curxp)xpz~]&positio~<&lagcount~>$]]></pattern>
        <value><![CDATA[#gag
    #var promptgag 1 0
    #var flags %left( @flags, %len( @flags)-1)
    #math racialtnl @tnl+@curxp
    #if @lagcount>0 {
      #if @lagcount>@lagcounting {#mat lagmax @lagcount*10} {}
      #mat lagcounting @lagcount*10
      } {#var lagcounting 0}
    #if @curhp>@lasthp {#echo ~+%eval( @curhp-@lasthp)~hp} {}
    #if @curmana>@lastmana {#echo ~+%eval( @curmana-@lastmana)~mana} {}
    #if @curhp<@lasthp {#echo ~-%eval( @lasthp-@curhp)~hp} {}
    #if @curmana<@lastmana {#echo ~-%eval( @lastmana-@curmana)~mana} {}
    #var lasthp @curhp
    #var lastmana @curmana
    #if @maxhp>10000 {#var hpperc %eval( @curhp/(@maxhp/100))} {#var hpperc %eval( (@curhp*100)/@maxhp)}
    #if @maxmana>10000 {#var manaperc %eval( @curmana/(@maxmana/100))} {#var manaperc %eval( (@curmana*100)/@maxmana)}]]></value>
      </trigger>
    </cmud>

Occasionally, there's an extra linefeed so we can go ahead and gag that
as well

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger type="Loop Pattern" param="6" priority="8990" prompt="true" copy="yes">
        <pattern>^$</pattern>
        <value>#if @promptgag=1 {#var promptgag 0 0
    #gag
    #exit} {}</value>
      </trigger>
    </cmud>

Now to set up your gauges. On the bottom left to right you will see
these in the following order: Hp, Mana, Move, Monitor Hp

Up top, you will see your TNL and lag counting gauges.

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <button type="Gauge" autosize="false" width="125" height="20" toolbar="2" inset="true" color="lime" gaugelowcol="red" gaugebackcol="#F0F0F0" priority="560" copy="yes">
        <caption>@curmonhp/@maxmonhp H</caption>
        <expr>@curmonhp</expr>
        <gaugemax>@maxmonhp</gaugemax>
        <gaugelow>@maxmonhp/4</gaugelow>
      </button>
      <button type="Gauge" autosize="false" width="125" height="20" toolbar="2" inset="true" color="lime" gaugelowcol="red" gaugebackcol="#F0F0F0" priority="550" copy="yes">
        <caption>@curmv/@maxmv MV</caption>
        <expr>@curmv</expr>
        <gaugemax>@maxmv</gaugemax>
        <gaugelow>@maxmv/4</gaugelow>
      </button>
      <button type="Gauge" autosize="false" width="300" height="23" inset="true" toolstyle="true" color="#FFFF99" gaugelowcol="red" gaugebackcol="None" priority="570" copy="yes">
        <caption>@tnl~tnl (@racialtnl)</caption>
        <expr>@tnl</expr>
        <gaugemax>@racialtnl</gaugemax>
        <gaugelow>@racialtnl/4</gaugelow>
      </button>
      <button name="hp" type="Gauge" autosize="false" width="125" height="20" toolbar="2" inset="true" color="lime" gaugelowcol="red" gaugebackcol="#F0F0F0" priority="530" copy="yes">
        <caption>@curhp/@maxhp M</caption>
        <expr>@curhp</expr>
        <gaugemax>@maxhp</gaugemax>
        <gaugelow>@maxhp/4</gaugelow>
      </button>
      <button type="Gauge" autosize="false" width="300" height="23" inset="true" toolstyle="true" color="#FFCC00" gaugelowcol="red" gaugebackcol="#F0F0F0" priority="580" copy="yes">
        <caption>Lag: %insert(".",@lagcounting,%len(@lagcounting)) seconds</caption>
        <expr>@lagcounting</expr>
        <gaugemax>@lagmax</gaugemax>
      </button>
      <button name="mana" type="Gauge" autosize="false" width="125" height="20" toolbar="2" inset="true" color="aqua" gaugelowcol="red" gaugebackcol="#F0F0F0" priority="540" copy="yes">
        <caption>@curmana/@maxmana M</caption>
        <expr>@curmana</expr>
        <gaugemax>@maxmana</gaugemax>
        <gaugelow>@maxmana/4</gaugelow>
      </button>
    </cmud>

Now you'll notice the lag counter isn't doing much so let's get that
working.

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger name="lagcounter" type="Alarm" priority="2970" copy="yes">
        <pattern>-.5</pattern>
        <value><![CDATA[#if @lagcounting>0 {#mat lagcounting @lagcounting-5} {}]]></value>
      </trigger>
    </cmud>

You can test out the lag counter with the simple in game "lag" social :D

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
