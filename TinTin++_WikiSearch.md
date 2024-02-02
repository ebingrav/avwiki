This is a simple but useful script that allows you quickly open up an
AvatarWiki search from the TinTin++ client. **(Requires FireFox for
now<sup>\[1\]</sup>)**

## Code

*Put the following into a file in your tintin++ folder and use **\#read
<filename>** while in tintin to load them up. Though you probably want
this to autoload with your character.*

`#alias wiki {#system {firefox "`[`http://avatar.melanarchy.info/index.php/Special:Search?search=%0&go=Go`](http://avatar.melanarchy.info/index.php/Special:Search?search=%0&go=Go)`"}}`

## Usage

-   Just type: **wiki** *<search string>*
-   It'll go directly to whatever page you search for, or if it can't
    find a direct match, it'll show you similar results. Just like
    searching from here!

<sup>\[1\]</sup> This was made and tested in Ubuntu linux with Firefox
installed. It should work in other OSes and/or other browsers, though
you may have to change "firefox" to the path to your browser. There very
well may be a more graceful way to do this which doesn't rely on a
specific hard-coded browser. I'll look into it when I have a windows
test box around.

## OS X

OS X users can use a more general alias to open the wiki link in their
default browser:

`#alias wiki {#system {open "`[`http://avatar.melanarchy.info/index.php/Special:Search?search=%0&go=Go`](http://avatar.melanarchy.info/index.php/Special:Search?search=%0&go=Go)`"}}`

[Category: TinTin++ Scripting](Category:_TinTin++_Scripting "wikilink")
