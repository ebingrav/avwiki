A Mudlet version of my [Zmud Group Filter](Zmud_Group_Filter "wikilink")

Filters the group list based on a query string.

## Laying the groundwork

It has two dependencies. The first is a startsWith function

`startsWith = function(word, prefix)`  
`  local word = string.lower(word)`  
`  local prefix = string.lower(prefix)`  
`  return string.find(word, prefix) == 1`  
`end`

Drop that into a script file. The second dependency is a table called
'avgroup'. In one of my script files, I have a block of code that goes

`if not avgroup then`  
`  avgroup = {`  
`    // default values of handy variables contained here`  
`  }`  
`end`

You don't have to have anything inside this table, but the script will
make use of it existing.

## The alias

First make a new alias. Give it whatever name you feel is appropriate,
and give it a pattern of

`^check (\w+)$`

with a script contents of

`if matches[2] == "tanks" then`  
`  avgroup.groupWatch = avgroup.groupTanks`  
`elseif matches[2] == "fp" then`  
`  avgroup.groupWatch = avgroup.groupFirepower`  
`elseif matches[2] == "healers" then`  
`  avgroup.groupWatch = avgroup.groupHealers`  
`else`  
`  avgroup.groupWatch = {matches[2]}`  
`end`  
`enableTrigger("Start filtering group list")`  
`send("group")`

That will do three things:

1.  Set which names you want to appear. As you can see, it takes either
    the keywords "tanks", "fp", or "healers", or the name of a player.
2.  Enable a trigger that will do the filtering
3.  Send the 'group' command to the MUD

It's up to you to fill 'avgroup.groupTanks', '.groupFirepower', or
'.groupHealers' as you see fit.

The alias will now respond to "check tanks", "check playername", etc.

## The triggers

Step 1, create a new trigger group to hold the filter trigs.

Now create another trigger group inside it named "Start filtering group
list". Set it to start disabled; the alias we defined above will enable
it when it's time to filter the group list. Set its fire length to 999
lines, and give it an 'exact match' pattern of:

`##| Level   Name         Pos   HitPoints   ManaPoints  MovePoints  TNL    Align`

It will now act like a trigger that enables the child triggers inside it
when it fires. Note that the group enclosing this inner group does
nothing, but in my experience a 'top level' group won't work as a filter
trigger. Maybe I'm just unlucky.

Now, inside the group with the filter pattern on it, add two triggers.
First, one called 'Player in group list' that fires on each line of the
group command output. Give it the following 'perl regex' pattern

`^ ?\d+\| {0,2}\d+ \w+ {1,2}(\w+)`

and give it a script contents of

`avgroup.eraseLine = true`  
  
`for i, name in ipairs(avgroup.groupWatch) do`  
`  if startsWith(matches[2], name) then`  
`    avgroup.eraseLine = false`  
`    break`  
`  end`  
`end`  
  
`if avgroup.eraseLine then`  
`  deleteLine()`  
`end`

Since this makes use of the startsWith function we defined earlier, you
don't need to type out full names. You know whose names I'm talking
about. Don't you. Yeah, you do. Make another trigger called 'Undo on
prompt', with the 'Lua function' pattern

`return isPrompt()`

and the script contents

`disableTrigger("Start filtering group list")`

which will turn the filter triggers back off when the prompt is reached.
Note that you may need to fiddle with both Mudlet's and Avatar's Telnet
GA settings for isPrompt() to correctly identify your prompt.

## Addenum

For what it's worth, the full regular expression for a group list line
is

`^ ?\d+\| {0,2}\d+ \w+ {1,2}(\w+) +(?:Stand|Sleep|Rest|Fight|STUN!|DROWN) -?\d+/-?\d+ +-?\d+/-?\d+ +-?\d+/-?\d+ +\d+ +(?:-?\d+|\?\?\?\?\?)$`

but since the trigger that detects the group line is only active between
the start of the group output and your next prompt, there's no need to
harden it that much. The '?????' at the end is for sub-level-10 players
who can't see alignments.

[Category:Mudlet Scripting](Category:Mudlet_Scripting "wikilink")
