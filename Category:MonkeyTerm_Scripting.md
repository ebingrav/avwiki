The first thing you got to know when scripting in MonkeyTerm is that to
begin a code segment you need to use \<% *code goes here* %\>, pretty
much as if you were creating an ASP homepage.

## First view

When scripting in MonkeyTerm you first see the "Funky Monkey Tree". It
is pretty selfexplaining, so any triggers you wanna create goes under
trigger submenu and any aliases goes under the alias submenu etc.

![](funkyMonkey.jpg "funkyMonkey.jpg")

## Basic Scripting

When you create a trigger or alias in MonkeyTerm you use an '\*' in the
pattern, so if you were to create an alias which when you typed greet
friend. You will first define the pattern as 'greet \*', and then in the
command use 'say hello there %1, my dear friend'.

![](MT_basicScript.jpg "MT_basicScript.jpg")

*Important to know is that if you use the string stored in the '\*' in a
code situation, inside a pair of \<% %\> that is. Then you need to use
$1 instead of %1.*

[Category: Scripting](Category:_Scripting "wikilink")
