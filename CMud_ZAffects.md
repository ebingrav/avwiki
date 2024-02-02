This script will keep a running affects board in a window titled "Status
Window." It has a 90 second ticker that will check your affects
regularly and update the window.

This script now includes tracking for [Dark
Embrace](Dark_Embrace.md "wikilink") for [Dark
races](Racial_Nosun.md "wikilink").

Used along with the [prompt](Cmud_Prompt.md "wikilink") script, it will
place most of the useful information into the Status Window.

## The Script

Save the following code as an .xml file, and import it into Cmud:


    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="zAffects">
        <stat name="Affects" showinbar="false" showinwindow="true" priority="1600">
          <value> @lblheighten @heighten                      @lblthren @thren_count
     @lblsneak @sneak        @lblmove  @move
     @lblsanc  @sanc         @lblffire @ffire        %ansi(high,bold,red)HP: %eval(@currhp*100/@maxhp)%
     @lblawen  @awen        @lblblind @blind       %ansi(high,bold,green)Mana: %eval(@currmana*100/@maxmana)%
     @lblfoci  @foci        @lbldark @dark        %ansi(high,bold,yellow)TNL: @currtnl
     @lblfort  @fort                        %ansi(high,bold,yellow)Exp: @runexp
                                    %ansi(high,bold,blue)Pred: @xpest
     @lblwater @water
     @lblinvinc @invinc             @lblbark  @bark
     @lblsteel @steelsp             @lbliron  @iron
     

       @currgear
       @message
     @message2

    %ansi(default)%ansi(high,red)F9 - Tank  %ansi(high,blue)F10 - Hit  %ansi(high,green)F11 - Mana  %ansi(high,yellow)F12 - Level</value>
        </stat>
        <trigger priority="1300">
          <pattern>Spell: 'awen'  for (%d) hours.</pattern>
          <value>#VAR lblawen %ansi(high,yellow)"Awen"
    #VAR awen %1
    </value>
        </trigger>
        <var name="awen" type="Literal">  </var>
        <trigger priority="1330">
          <pattern>Spell: 'sanctuary'  for (%d) hours.</pattern>
          <value>#VAR lblsanc %ansi(high,white)"Sanc"
    #VAR sanc %1</value>
        </trigger>
        <var name="sanc" type="Literal">  </var>
        <trigger priority="1360">
          <pattern>Spell: 'sneak'  for (%d) hours.</pattern>
          <value>#var lblsneak "Sneak"
    #VAR sneak %1</value>
        </trigger>
        <var name="sneak" type="Literal">  </var>
        <var name="foci" type="Literal">  </var>
        <trigger priority="1400">
          <pattern>Spell: 'foci'  for (%d) hours.</pattern>
          <value>#VAR lblfoci %ansi(high,blue)"Foci"
    #VAR foci %1
    #if (%1&gt;19) {#exec calcexp}</value>
        </trigger>
        <trigger priority="1410">
          <pattern>Spell: 'iron monk' for (%d) hours.</pattern>
          <value>#VAR lblsanc "Sanc"
    #VAR sanc %1</value>
        </trigger>
        <trigger priority="1420">
          <pattern>Spell: 'fortitudes'  for (%d) hours.</pattern>
          <value>#VAR lblfort %ansi(red)"Fort"
    #VAR fort %1</value>
        </trigger>
        <trigger priority="1430">
          <pattern>Spell: 'move hidden'  for (%d) hours.</pattern>
          <value>#VAR lblmove "Move"
    #VAR move %1</value>
        </trigger>
        <trigger priority="1440">
          <pattern>Spell: 'heighten senses'  for (%d) hours.</pattern>
          <value>#VAR lblheighten "Heigh"
    #VAR heighten %ansi(yellow)%1%ansi(green)</value>
        </trigger>
        <var name="heighten" type="Literal">  </var>
        <trigger priority="500">
          <pattern>You are affected by:</pattern>
          <value>clearaffects</value>
        </trigger>
        <alias name="clearaffects">
          <value>#VAR sneak "  "
    #VAR move "  "
    #VAR sanc "  "
    #VAR heighten "  "
    #VAR iron "  "
    #VAR foci "  "
    #VAR fort "  "
    #VAR awen "  "
    #VAR invinc "  "
    #VAR bark "  "
    #VAR steelsp "  "
    #VAR water "  "
    #var ffire "  "
    #VAR lblsneak "     "
    #VAR lblmove "    "
    #VAR lblsanc "    "
    #VAR lblheighten "     "
    #VAR lbliron "    "
    #VAR lblfoci "    "
    #VAR lblfort "    "
    #VAR lblawen "    "
    #VAR lblinvinc "     "
    #VAR lblbark "    "
    #VAR lblsteel "     "
    #VAR lblwater "     "
    #VAR lblffire "     "</value>
        </alias>
        <trigger priority="1490">
          <pattern>Spell: 'invincibility'  modifies armor class by (%n) for (%d) hours.</pattern>
          <value>#VAR lblinvinc %ansi(white)"Invin"
    #VAR invinc %2
    </value>
        </trigger>
        <trigger priority="1500">
          <pattern>Spell: 'iron skin'  modifies armor class by (%n) for (%d) hours.</pattern>
          <value>#VAR lbliron "Iron"
    #VAR iron %2
    </value>
        </trigger>
        <trigger priority="1510">
          <pattern>Spell: 'barkskin'  modifies armor class by (%n) for (%d) hours.</pattern>
          <value>#VAR lblbark "Bark"
    #VAR bark %2</value>
        </trigger>
        <trigger priority="1520">
          <pattern>Spell: 'water breathing'  for (%d) hours.</pattern>
          <value>#VAR lblwater %ansi(blue)"Water"
    #VAR water %1</value>
        </trigger>
        <trigger priority="1530">
          <pattern>Spell: 'steel skeleton'  modifies armor class by (%n) for (%d) hours.</pattern>
          <value>#VAR lblsteel "Steel"
    #VAR steelsp %2</value>
        </trigger>
        <var name="lblawen" type="Literal">    </var>
        <var name="move" type="Literal">  </var>
        <var name="iron" type="Literal">  </var>
        <var name="fort" type="Literal">  </var>
        <var name="invinc" type="Literal">  </var>
        <var name="bark" type="Literal">  </var>
        <var name="steelsp" type="Literal">  </var>
        <var name="water" type="Literal">  </var>
        <var name="lblsneak" type="Literal">     </var>
        <var name="lblmove" type="Literal">    </var>
        <var name="lblsanc" type="Literal">    </var>
        <var name="lblheighten" type="Literal">     </var>
        <var name="lbliron" type="Literal">    </var>
        <var name="lblfoci" type="Literal">    </var>
        <var name="lblfort" type="Literal">    </var>
        <var name="lblinvinc" type="Literal">     </var>
        <var name="lblbark" type="Literal">    </var>
        <var name="lblsteel" type="Literal">     </var>
        <var name="lblwater" type="Literal">     </var>
        <trigger priority="1880">
          <pattern>Leaving the AVATAR System for the 'real world'...</pattern>
          <value>clearaffects
    #VAR dark "  "
    #VAR lbldark "     "</value>
        </trigger>
        <trigger priority="2890">
          <pattern>You are not under the affects of any spells or skills.</pattern>
          <value>clearaffects</value>
        </trigger>
        <trigger priority="4250">
          <pattern>You no longer feel invincible!</pattern>
          <value>#VAR invinc ""
    #VAR lblinvinc ""</value>
        </trigger>
        <trigger priority="4260">
          <pattern>Your senses return to normal.</pattern>
          <value>#VAR heighten ""
    #VAR lblheighten ""
    heighten</value>
        </trigger>
        <trigger priority="4270">
          <pattern>Your lungs adapt to oxygen once again.</pattern>
          <value>#VAR water ""
    #VAR lblwater ""</value>
        </trigger>
        <trigger priority="4280">
          <pattern>The protective aura fades from around your body.</pattern>
          <value>#VAR sanc "  "
    #VAR lblsanc "    "</value>
        </trigger>
        <trigger priority="4290">
          <pattern>Your skin returns to normal.</pattern>
          <value>#VAR bark ""
    #VAR lblbark ""</value>
        </trigger>
        <trigger priority="4300">
          <pattern>Your skin feels soft again.</pattern>
          <value>#var foci ""
    #var lblfoci ""</value>
        </trigger>
        <trigger priority="4310">
          <pattern>The adrenaline rush wears off.</pattern>
          <value>#var fort ""
    #var lblfort ""</value>
        </trigger>
        <trigger priority="4320">
          <pattern>You feel lighter as your bones return to normal.</pattern>
          <value>#var steelsp ""
    #var lblsteel ""</value>
        </trigger>
        <trigger priority="4330">
          <pattern>Your senses are completely heightened.</pattern>
          <value>#VAR lblheighten "Heigh"
    #VAR heighten %ansi(yellow)%1%ansi(green)
    aff</value>
        </trigger>
        <trigger priority="4680">
          <pattern>You can't do that in your sleep.</pattern>
          <value>#if (@heighten = "") {wake;heighten;sleep}
    </value>
        </trigger>
        <var name="ffire" type="Literal">  </var>
        <var name="lblffire" type="Literal">     </var>
        <trigger priority="11760">
          <pattern>The pink aura around you fades away.</pattern>
          <value>#VAR ffire "  "
    #VAR lblffire "     "</value>
        </trigger>
        <trigger priority="11770">
          <pattern>You are surrounded by a pink outline.</pattern>
          <value>#VAR lblffire %ansi(blink,high,magenta)"FFire"</value>
        </trigger>
        <trigger priority="11780">
          <pattern>Spell: 'faerie fire'  modifies armor class by * for (%d) hours</pattern>
          <value>#VAR lblffire %ansi(blink,high,magenta)"FFire"
    #VAR ffire %2</value>
        </trigger>
        <trigger priority="11810">
          <pattern>Spell: 'blindness'  modifies hit roll by * for (%d) hours.</pattern>
          <value>#VAR lblblind %ansi(blink,high,white)"Blind"
    #VAR blind %2</value>
        </trigger>
        <var name="lblblind" type="Literal">     </var>
        <trigger priority="11830">
          <pattern>You are blinded!</pattern>
          <value>#VAR lblblind %ansi(blink,high,white)"Blind"
    </value>
        </trigger>
        <trigger priority="11840">
          <pattern>You can see again.</pattern>
          <value>#VAR blind "  "
    #VAR lblblind "     "</value>
        </trigger>
        <var name="blind" type="Literal">  </var>
        <class name="DarkRaces">
          <trigger priority="12800">
            <pattern>The safety of shade disappears.</pattern>
            <value>#VAR lbldark %ansi(high,white)"Dark-"</value>
          </trigger>
          <trigger priority="12810">
            <pattern>You can't take the bright sunlight!</pattern>
            <value>#VAR lbldark %ansi(blink,high,white)"Dark!"</value>
          </trigger>
          <trigger priority="12820">
            <pattern>You are embraced by darkness.</pattern>
            <value>#VAR dark "  "
    #VAR lbldark "     "</value>
          </trigger>
          <trigger priority="12830">
            <pattern>Your skin starts to char and smoke!</pattern>
            <value>#VAR lbldark %ansi(blink,high,red)"Dark!"</value>
          </trigger>
          <var name="lbldark" type="Literal"/>
          <var name="dark" type="Literal"/>
        </class>
      </class>

      <trigger type="Alarm" priority="4710">
        <pattern>-01:30</pattern>
        <value>aff</value>
      </trigger>
    </cmud>

## Notes

There's a lot more customization you can do, by simply looking at the
aliases and triggers in place and making your own.

## Designer comments

Feel free to note me [here](User:Shalineth.md "wikilink") or on board 2
to Shalineth with any feedback or suggestions.

Updated for CMud v3.32.

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
