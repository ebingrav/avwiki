Syntax: description + <string> Syntax: description - Syntax: description
clear Syntax: description show

Sets your long description to the given string. If the description
string starts with a '+', the part after the '+' is appended to your
current description, so that you can make multi-line descriptions. This
description is seen when a player looks at you.

Example:

desc + You are looking at a tall mage whose dancing green eyes catch

desc + your attention and hold it.

This would appear as a multi-line description. To remove the last line
of a multi-line description, you can use the \<description -\> command.
In the example above, \<desc -\> would delete "your attention and hold
it."

[Category: Commands](Category:_Commands "wikilink")
