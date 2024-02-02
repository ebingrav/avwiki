This script will make it easy to identify vials without having to cast
identify.

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger priority="3750" copy="yes">
        <pattern>(%*){a|an} %2 vial of healing</pattern>
        <value>#if (%lower( %2)="reddish-brown") {#var vialhealtype "cure crit"} {}
    #if (%lower( %2)="light brown") {#var vialhealtype "cure light"} {}
    #if (%lower( %2)="red-brown") {#var vialhealtype "divinity"} {}
    #if (%lower( %2)="orange-brown") {#var vialhealtype "cure serious"} {}
    #if (%lower( %2)="orange-red") {#var vialhealtype "cure crit+light"} {}
    #if (%lower( %2)="red striped") {#var vialhealtype "heal"} {}
    #if (%lower( %2)="yellow-brown") {#var vialhealtype "heal+cure light"} {}
    #if (%lower( %2)="yellow") {#var vialhealtype "heal+cure serious"} {}
    #if (%lower( %2)="yellow-orange") {#var vialhealtype "heal+cure crit"} {}
    #if (%lower( %2)="orange") {#var vialhealtype "heal+cure light+cure crit"} {}
    #psub {%2~(@vialhealtype~)} %x2
    #ad vialcount 1
    #var vcount %1
    #var vcount %remove((Magical),@vcount)
    #var vcount %remove((Humming),@vcount)
    #var vcount %remove((Glowing),@vcount)
    #var vcount %remove((Demonic),@vcount)
    #var vcount %replace(@vcount,"(","")
    #var vcount %replace(@vcount,")","")
    #var vcount %trim(@vcount)
    #if @vcount&gt;0 {#ad vialcount %eval(@vcount-1)} {}</value>
      </trigger>
    </cmud>

Here is a variation on the above theme. It uses if/else statements and
removes most of the variable work from above for optimized speed, as
well as coloring the vials.

    <trigger priority="6800" id="52">
      <pattern>{a|an} (%*) vial of (healing)$</pattern>
      <value>#IF (%1="light brown") {#psub {"cure light"} %x2;#pcol tan %x1 %x2} {
    #IF (%1="orange-brown") {#psub {"cure serious"} %x2;#pcol goldenrod %x1 %x2} {
    #IF (%1="reddish-brown") {#psub {"cure critical"} %x2;#pcol chocolate %x1 %x2} {
    #IF (%1="orange-red") {#psub {"cure critical, cure light"} %x2;#pcol orangered %x1 %x2} {
    #IF (%1="red striped") {#psub {"heal"} %x2;#pcol mxpred %x1 %x2} {
    #IF (%1="yellow-brown") {#psub {"heal, cure light"} %x2;#pcol darkgoldenrod %x1 %x2} {
    #IF (%1="yellow") {#psub {"heal, cure serious"} %x2;#pcol mxpyellow %x1 %x2} {
    #IF (%1="yellow-orange") {#psub {"heal, cure critical"} %x2;#pcol gold %x1 %x2} {
    #IF (%1="orange") {#psub {"heal, cure light, cure critical"} %x2;#pcol mxporange %x1 %x2} {
    #IF (%1="red-brown") {#psub {"divinity"} %x2;#pcol sienna %x1 %x2}
    }}}}}}}}}
    </value>
    </trigger>

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
