This is a simple script that will prompt you which spell you wish to
[charge](Charge_Shield.md "wikilink") a
[shield](:Category:Shields.md "wikilink") with. Designed for hero level
shields, but will work with lower-level shields.

The script includes a counter to show how many castings it took, as well
as a current damage status, prompting you to get your shield repaired
below 80% hit points.

## The Script

This script is in two parts. The first is in the class "Chargeshield"
and will be disabled by default. The second part needs to be saved to
any active class (I use "Base") and will enable the "Chargeshield" class
when the alias **Chargeshield** is run.

Save the following two blocks of code as a separate .xml files, and
import it into Cmud:  

## Part one

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="ChargeShield">
        <trigger priority="10640">
          <pattern>^The spell goes off in your face!</pattern>
          <value>c 'charge shield' @shieldtype @spell
    cntincrease = @cntincrease+1 </value>
        </trigger>
        <trigger priority="10650">
          <pattern>^(*) begins to hum with the power of your spell!</pattern>
          <value>c 'charge shield' @shieldtype @spell
    cntincrease = @cntincrease+1 </value>
        </trigger>
        <trigger priority="10660">
          <pattern>^(*) already stores all the magic it can hold.</pattern>
          <value>c identify @shieldtype
    #BEEP</value>
        </trigger>
        <trigger priority="10670">
          <pattern>^Nothing seems to happen.</pattern>
          <value>c 'charge shield' @shieldtype @spell
    cntincrease = @cntincrease+1 </value>
        </trigger>
        <trigger priority="10680">
          <pattern>^(*) hums briefly, and then falls silent.</pattern>
          <value>c 'charge shield' @shieldtype @spell
    cntincrease = @cntincrease+1 </value>
        </trigger>
        <var name="shieldtype">heroes</var>
        <var name="spell">flametongue</var>
        <trigger priority="10720">
          <pattern>^Object Quality ~((%d) ~/ (%d) hps~)</pattern>
          <value>#var ChargeShield/shieldmaxhp %2
    #var ChargeShield/shieldcurhp %1
    #math ChargeShield/shieldpercent %eval(@shieldcurhp*100/@shieldmaxhp)</value>
        </trigger>
        <trigger priority="10730" enabled="false">
          <pattern>^It has (%d) charges of (*).</pattern>
          <value>#var ChargeShield/cntcharges %1
    #var ChargeShield/spell %2
    #echo -- Charge Shield Report --
    #echo The shield has %1 charges of %proper(%2). It took %eval(@cntgoesoff+@cntincrease+@cntnothing+@cntsilent) castings.
    #if (@shieldpercent &#38;lt; 90) {#echo -- DANGER! Your @shieldtype shield is at @shieldpercent %. Repair it soon! --}
    #wait 1000
    clearshield
    </value>
        </trigger>
        <var name="cntcharges">0</var>
        <var name="cntsilent">0</var>
        <var name="cntnothing">0</var>
        <var name="cntincrease">0</var>
        <var name="cntgoesoff">0</var>
        <var name="shieldmaxhp">0</var>
        <var name="shieldcurhp" type="Integer">0</var>
        <var name="shieldpercent">0</var>
        <alias name="clearshield">
          <value>#yesno "Do you wish to reset the Charge Shield counter?" {clearshield1} {#exit;#echo Run the clearshield alias when you wish to reset the counter.}
    </value>
        </alias>
        <alias name="clearshield1">
          <value>#var ChargeShield/cntcharges 0
    #var ChargeShield/cntgoesoff 0
    #var ChargeShield/cntincrease 0
    #var ChargeShield/cntnothing 0
    #var ChargeShield/cntsilent 0
    #var ChargeShield/shieldcurhp 0
    #var ChargeShield/shieldmaxhp 0
    #var ChargeShield/shieldpercent 0
    #echo -
    #echo -- Charge Shield counter reset --
    #T- "^It has (%d) charges of (*)."
    #class ChargeShield 0</value>
        </alias>
      </class>
    </cmud>

## Part two

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <alias name="ChargeShield">
        <value>#CLASS ChargeShield 1
    #PR ChargeShield/shieldtype "Input a keyword for the shield I'm about to charge."
    #VAR ChargeShield/spell %pick("p:Select a spell","o:1","flametongue","arc lightning","frostbite","jolt","flash","venom")
    c identify @shieldtype
    c 'charge shield' @shieldtype @spell 
    cntincrease = @cntincrease+1 
    #wait 1000
    #T+ "^It has (%d) charges of (*)."</value>
      </alias>
    </cmud>

## Designer comments

Feel free to note me [here](User:Shalineth.md "wikilink") or on board 2
to Shalineth with any feedback or suggestions.

Updated for CMud v3.32.

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
