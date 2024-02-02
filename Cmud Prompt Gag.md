Save the following code as an .xml file, and import it into Cmud:

<alias name="gagprompt" autoappend="true" id="33">  
` `<value>`prompt prompt ~%h ~%H ~%m ~%M ~%v ~%V ~%w ~%W ~%T ~%S ~%Q ~%A ~%n`</value>  
</alias>  
<trigger priority="950" id="95">  
` `<pattern>`^(%*)prompt (%d) (%d) (%d) (%d) (%d) (%d) (%x) (%x) (%d) (%x) (%x) (%x)`</pattern>  
` `<value>`#gagspace`  
` #var pstart %1`  
` #var hp %2`  
` #var maxhp %3`  
` #var mana %4`  
` #var maxmana %5`  
` #var move %6`  
` #var maxmove %7`  
` #var mon %8`  
` #var maxmon %9`  
` #var tnl %10`  
` #var surge %11`  
` #var quicken %12`  
` #var augment %13`</value>  
</trigger>  
<stat name="prompt" priority="4830" id="483">  
` `<value>`%if(@tnl<300,~<LEVEL GEAR~>,"") @pstart @hp / @maxhp hps   @mana / @maxmana mana   @move / @maxmove move   @mon / @maxmon mon   @tnl tnl   surge @surge   quicken @quicken   augment @augment`</value>  
</stat>

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
