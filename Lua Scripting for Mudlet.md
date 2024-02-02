## Basic Info

Lua is used by many game engines such as World of Warcraft. It lets you
create custom UI.

Mudlet (www.mudlet.org) is an open source mud client that works on PC
and Mac that uses it. It's forum has links to help pages for Mudlet and
Lua and RegEx.

## Screenshot

Bear in mind the gauges and tabs you see are not part of the default
client. They are custom UI from this package.  
<img src="Avatar_Mudlet_Feenom_Screenshot.png" title="screenshot"
width="600" alt="screenshot" />  

## Mudlet Package files

I had to link it externally.  
\#Download the file, extract the 3 XML files.

1.  Open Mudlet. Load a Profile.
2.  Click on Package Manager and Install each one

  
forums.mudlet.org/download/file.php?id=657

## Associated Configs and Mud Side Aliases

**Configs  
conf -blank  
prompt \<-\|BR\|%h/%Hhp \|BG\|%m/%Mmp \|BW\|%v/%Vmv  
prompt2 \|BP\| %a \|BB\|%Ttnl\|N\|-\> \<\|BC\|%w/%W\|N\|\>%n  
**Aliases  
alias r rescue %1  
alias in inspect %1  
alias wowo open wooden:get all wooden:drop wooden:sac wooden:inspect
lockbox  

## Script Explanations

There are a few major components, that will be explained at this point.
The rest can be figured out by inspection.  
\* Chat - takes all communications and puts it into separate tabs on the
right: tell, group, public

-   Prompt - makes visual stat and progress gauges and removes the
    constant prompts
-   Auto Rescue - maintains and lets you manage the Rescue List
    -   local commands: ar on, ar off, ar add \_\_\_\_, ar remove
        \_\_\_\_, ar show, ar clear
    -   triggered commands from group tell: gt add me, gt remove me
-   Run Triggers
-   Spell Bot - a mid run spell bot. not comprehensive
    -   local commands: sb on, sb off
    -   currently does by tell: sanc, frenzy, cd, cp, cb, pp \_\_\_, nex
        \_\_\_
-   Lockboxes - untraps, picks, loots, and sacrifices all lockboxes in
    inventory

## For Help or Bugs

Message [Feenom](User:Feenom "wikilink")

[Category:Mudlet Scripting](Category:Mudlet_Scripting "wikilink")
