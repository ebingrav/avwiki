    Syntax: ALIAS
    Syntax: ALIAS <keyword>
    Syntax: ALIAS <keyword> <command>:<command>:<command>.......

Typing ALIAS with no arguments displays the current aliases you have
set.

Typing ALIAS <keyword> will delete a previously set alias.

Typing ALIAS <keyword> <command>:<command>:<command>:<command>... sets
an alias.

Examples:

If you were to type alias food eat pie:drink waterskin:drink waterskin,
whenever you then typed food you would attempt to eat a pie and to drink
from a waterskin twice.

Typing alias food would then remove the alias.

You may also use variables in your aliases. See [HELP ALIAS
PARAMETERS](Alias_Parameters.md "wikilink") for more information. An
alias command cannot consist solely of a variable.

By etching your gear with a common word, you can create an alias to
easily wear different sets.

Example: alias tank get all.tankgear bag:wear all.tankgear

The only limit to an alias's length comes from the total number of
characters the MUD will process in one command (about 400).

See also: ALIAS PARAMETERS, ALIAS LAW, ETCH, PUT

See also [Mud Side Aliases](:Category:_Mud_Side_Aliases.md "wikilink").

[Category: Commands](Category:_Commands "wikilink")
