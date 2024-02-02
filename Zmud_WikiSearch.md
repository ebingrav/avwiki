This trigger opens your web browser to an AvatarWiki search of your
selected text when you choose an option from the right-click menu. It
also removes articles (the, an, a) at the beginning of search terms as
per wiki standards.  
![](Wikisearch.jpg "Wikisearch.jpg")

## Code

*Copy each line individually and then paste them into zMud:*

`#ALIAS wiki {#URL http://avatar.melanarchy.info/index.php/Special:Search?search=~"%1~"} "wikisearch"`  
`#MENU {Search for '%selword' on AvatarWiki} {#URL http://avatar.melanarchy.info/index.php/Special:Search?search=~"%selword~"} "wikisearch"`  
`#MENU {Search for '%selected' on AvatarWiki} {#VAR wikistring ~"%selected~";#IF {%begins( @wikistring, "an ")} {#VAR wikistring %remove( "an ", @wikistring)};#IF {%begins( @wikistring, "a ")} {#VAR wikistring %remove( "a ", @wikistring)};#IF {%begins( @wikistring, "the ")} {#VAR wikistring %remove( "the ", @wikistring)};wiki @wikistring} "wikisearch"`

## Usage

Just select some text in the mud, right click on it and choose the
"Search for 'x' on AvatarWiki" option. For single word searches, just
right click the word, no need to select it.  
You can also do manual searches by typing wiki followed by your search
term in quotations.  
EX: `wiki "search requirements"`

## How It Works

[Category: Zmud Scripting](Category:_Zmud_Scripting "wikilink")
