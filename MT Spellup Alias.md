This alias will allow you to spellup your groupmembers, and/or yourself.

## Alias

*This should be entered into a new alias in the Funky Monkey Tree.*

Pattern: spellup \*

Commands:

`<%if $1 <> "" then `  
`    Session.EvaluateCommand(("say Spelling " + $1 + "\n"))`  
`  end if`  
`%>`  
`c 'water breathing' %1`  
`c fortitudes %1`  
`c foci %1`  
`c 'steel skeleton' %1`  
`c 'iron skin' %1`  
`<%if $1 <> "" then `  
`    Session.EvaluateCommand(("cast awen " + $1 + "\n"))`  
`    Session.EvaluateCommand(("say Done Spelling " + $1 + "\n"))`  
`  else `  
`    Session.EvaluateCommand(("cast savvy \n"))`  
`  end if`  
`%>`

## Usage

To spellup any given person, type <B>spellup *<character>*</B> at any
time.  

## How It Works

`<%if $1 <> "" then `  
`    Session.EvaluateCommand(("say Spelling " + $1 + "\n"))`  
`  end if`  
`%> `

This will only show the say spelling, if your using an argument will
spellup. So if your spelling yourself up, then no message is given.

`<%if $1 <> "" then `  
`    Session.EvaluateCommand(("cast awen " + $1 + "\n"))`  
`    Session.EvaluateCommand(("say Done Spelling " + $1 + "\n"))`  
`  else `  
`    Session.EvaluateCommand(("cast savvy \n"))`  
`  end if`  
`%>`

If your spelling anyone else but yourself, then it will cast awen and
then say that you are done spelling. If cast on yourself, then you will
finish with any self-only spells, and no message is given.

## Advanced

You could also if you like save your own name in a variable, and
depending on what that name is cast different spells.

` if var("char_name") = "`*<your name>*`" then`  
`    Session.EvaluateCommand(("cast invincibility" + $1 + "\n"))`  
` end if`

This assumes that you create an if statement for each type of char you
use this alias with.

[Category: MonkeyTerm
Scripting](Category:_MonkeyTerm_Scripting "wikilink")
