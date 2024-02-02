This trigger counts the number of kills and amount of experience you
have gained on a run and shows it to the group.

## Code

*Each of the following is to be entered into MonkeyTerm under the either
the trigger och alias tab.*

## Trigger

*This will keep track of how much xp you have gathered, and for how many
kills*

Pattern: You receive \* experience points.

Commands:

`<% `  
`   var("kill/xp") = var("kill/xp") + CInt($1) `  
`   var("kill/kills") = var("kill/kills") + 1`  
`%>`

## Alias

*This will show the group how much experience you gathered. And how you
reset the counting.*

Pattern: show

Commands:

`<% `  
`  dim kill_string`  
`  kill_string = "|c|We ran for |w|" + var("kill/xp") + " |c|xp on |w|" + var("kill/kills") + "|c| kills."`  
` `  
`  Session.EvaluateCommand(("gtell " + kill_string + "\n"))`  
`%>`

Pattern: reset

Commands:

`<% `  
`   var("kill/kills") = CInt(0)`  
`   var("kill/xp") = CInt(0)`  
`%>`

## Usage

To show the stats, type <B>show</B> at any time.  
To reset the stats to 0, type <B>reset</B>  

## How It Works

`var("kill/xp") = var("kill/xp") + CInt($1)`

This will create a variable named "kill/xp" if it doesnt exist and then
store the amount of experience you have gained during the run.

`var("kill/kills") = var("kill/kills") + 1`

This line will in the variable named "kill/kills" keep track of how many
mobiles your group has killed so far during the run.

`dim kill_string`  
`kill_string = "|c|We ran for |w|" + var("kill/xp") + " |c|xp on |w|" + var("kill/kills") + "|c| kills."`

This will create a string with the experienced gained, and the number of
mobiles killed during the run.

`Session.EvaluateCommand(("gtell " + kill_string + "\n"))`

This command makes MonkeyTerm parse it, as if it was typed in the normal
way. So any aliases and such made in MonkeyTerm is accessable.

`var("kill/kills") = CInt(0)`  
`var("kill/xp") = CInt(0)`

These commands will reset the variables "kill/kills" and "kill/xp" to 0,
CInt(0) is for Monkeyterm to know that it should store the number 0 and
not the string "0" in the variable. *Since MonkeyTerm uses VBScripting
you dont have to create the variables before you use them, so if for
example the variable "kill/xp" doesnt exist, MonkeyTerm will create it.*

## Advanced

You could also if you like save your own name in a variable, and
depending on what that name is append different messages to the shown
strig.

`if var("char_name") = "`<your monks name>`" then`  
`   kill_string = kill_string + " - |w|" + `*`variable_name`*` + "|c| golden strikes."`  
`end if`

This assumes that you create an if statement for each type of char you
use this alias with.

[Category: MonkeyTerm
Scripting](Category:_MonkeyTerm_Scripting "wikilink")
