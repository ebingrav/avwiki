With the implementation of GMCP, one can utilize this to clean up
prompts.

The first thing you want is a GMCP trigger to set your variables:

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger type="GMCP" priority="110" copy="yes">
        <pattern>^Char.Vitals</pattern>
        <value>#var curhp {%gmcp.char.vitals.hp}
    #var maxhp {%gmcp.char.vitals.maxhp}
    #var curmana {%gmcp.char.vitals.mp}
    #var maxmana {%gmcp.char.vitals.maxmp}
    #var curmv {%gmcp.char.vitals.mv}
    #var maxmv {%gmcp.char.vitals.maxmv}
    #var tnl {%gmcp.char.vitals.tnl}
    #var racialtnl {%gmcp.char.vitals.maxtnl}</value>
      </trigger>
    </cmud>

Then you need pretty gauges to show your stats:

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <button type="Gauge" autosize="false" width="130" height="20" toolbar="2" color="lime" gaugelowcol="red" gaugebackcol="#F0F0F0" priority="530" copy="yes">
        <caption>@curhp/@maxhp h</caption>
        <expr>@curhp</expr>
        <gaugemax>@maxhp</gaugemax>
        <gaugelow>@maxhp/4</gaugelow>
      </button>
      <button type="Gauge" autosize="false" width="130" height="20" toolbar="2" color="aqua" gaugelowcol="red" gaugebackcol="#F0F0F0" priority="600" copy="yes">
        <caption>@curmana/@maxmana m</caption>
        <expr>@curmana</expr>
        <gaugemax>@maxmana</gaugemax>
        <gaugelow>@maxmana/4</gaugelow>
      </button>
      <button type="Gauge" autosize="false" width="130" height="20" toolbar="2" color="lime" gaugelowcol="red" gaugebackcol="#F0F0F0" priority="610" copy="yes">
        <caption>@curmv/@maxmv mv</caption>
        <expr>@curmv</expr>
        <gaugemax>@maxmv</gaugemax>
        <gaugelow>@maxmv/4</gaugelow>
      </button>
      <button type="Gauge" autosize="false" width="130" height="20" toolbar="2" color="yellow" gaugelowcol="lime" gaugebackcol="#F0F0F0" priority="630" copy="yes">
        <caption>@tnl/@racialtnl tnl</caption>
        <expr>@tnl</expr>
        <gaugemax>@racialtnl</gaugemax>
        <gaugelow>200</gaugelow>
      </button>
    </cmud>

Once you have all this working, you can get rid of hp/mana/mvs/tnl from
your prompt. I do highly recommend keeping lag and monitor in your
prompt though.

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
