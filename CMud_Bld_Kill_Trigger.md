This is a kill trigger exclusively for anyone who initiates with sneak
attack. It will attempt to sneak attack, but if the mob is too hurt for
it, automatically try to kill. Please keep in mind that it's very basic
and you can utilize this more effectively by tracking your class on
login then use:

    #if %ismember(%lower(@myclass),"rog|bld|asn|bci") {#var target %1;surp %1} {~kill %1}

The following is a simplified version:

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger priority="3280" copy="yes">
        <pattern>^{@tank|@leader} is killing %1{.| with her mental power}</pattern>
        <value>#var target %1;surp %1</value>
      </trigger>
      <trigger priority="5390" copy="yes">
        <pattern>^%1 is hurt and suspicious ... you can't sneak up.</pattern>
        <value>~kill @target</value>
      </trigger>
    </cmud>

What I personally do is set up a variable called @spunj, and use it to
toggle on and off spunj triggers. The following will set up a toggle
button to turn your spunj on and off:

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <button type="Gauge" autosize="false" width="60" height="23" autopos="false" left="183" toolbar="0" color="aqua" gaugelowcol="red" gaugebackcol="#F0F0F0" priority="3308" copy="yes">
        <caption>Spunj</caption>
        <value>#if @spunj=0 {#var spunj 1 0} {#var spunj 0 0}</value>
        <expr>@spunj</expr>
        <gaugemax>1</gaugemax>
      </button>
    </cmud>

Once you have a toggle, you can insert this into your trigger to
activate/deactivate:

    #if @spunj=0 {#exit} {}

Finally, all my kill triggers do NOT fire if I'm currently lagged for
more than 2 seconds. This prevents double triggering and mistargetting
which is especially useful for smaller characters. It's a bit more
complex and requires you to track your prompt and lag with a built in
alarm timer so that's for more advanced users.

[Category:Bladedancers](Category:Bladedancers "wikilink")
[Category:Cmud_Scripting](Category:Cmud_Scripting "wikilink")
