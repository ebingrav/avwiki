You may use variables, or parameters, in your aliases. Basically, what
this means is that you can put a placeholder in your aliases which will
be replaced with something you type (or nothing if you do not specify a
replacement).

To do this, use a %1-5 in your aliases. A good way to think of how it
will work is this:

`       %1     %2     %3     %4     %5`

alias <text> <text> <text> <text> <text>

For example: alias greet smile %1:bow %1:say Hello %1! If you typed:
greet Friend It would be like your alias said: smile Friend:bow
Friend:say Hello Friend!

NOTE: Just because you used a %# in your alias, you do not always have

`     to enter something to replace it.`

Using the above 'greet' example, if you only typed greet, it would act
like the alias said: smile :bow :say Hi !

Note that you may not create an alias that calls on another alias.

See also: [Alias](Alias "wikilink")

[Category: Commands](Category:_Commands "wikilink")
