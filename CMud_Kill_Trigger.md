The following is a simple kill trigger coded for CMUD

First let's define the tank and/or the leader:

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger priority="1960" copy="yes">
        <pattern>^You join (%w)'s group.$</pattern>
        <value>#yesno "Is this the tank?" {#var tank %1} {#var tank ""}
    gr</value>
      </trigger>
      <trigger priority="9040" copy="yes">
        <pattern>^%w removes you from %w group.$</pattern>
        <value>#var tank ""
    #var leader ""</value>
      </trigger>
      <trigger priority="8080" copy="yes">
        <pattern>^You stop following %w.</pattern>
        <value>#var tank ""
    #var leader ""</value>
      </trigger>
    <trigger priority="2000" copy="yes">
        <pattern>^&amp;leader's group:</pattern>
      </trigger>
    </cmud>

Next, create a toggle so that you can enable/disable with the click of a
button:

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <button type="Gauge" autosize="false" width="60" height="23" autopos="false" left="1" toolbar="0" color="aqua" gaugelowcol="red" gaugebackcol="#F0F0F0" priority="3290" copy="yes">
        <caption>Spunj</caption>
        <value>#if @spunj=0 {
    #echo Spunj triggers enabled.
      #var spunj 1 0
      } {#var spunj 0 0
    #echo Spunj triggers disabled.}
    </value>
        <expr>@spunj</expr>
        <gaugemax>1</gaugemax>
      </button>
    </cmud>

And of course the trigger to fire when the tank or leader emotes "is
killing":

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger priority="3280" copy="yes">
        <pattern>^{@tank|@leader} is killing %1.</pattern>
        <value>#var target %1
    #if @spunj=0 {#exit} {}
    k %1</value>
      </trigger>
    </cmud>

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
