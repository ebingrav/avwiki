This trigger opens your default web browser to an AvatarWiki search of
your selected text when you choose an option from the right-click menu.
It also removes articles (the, an, a) at the beginning of search terms
as per wiki standards.  
![](Wikisearch.jpg "Wikisearch.jpg")

## The Script

Save the following code as an .xml file, and import it into Cmud:


    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="wikisearch">
        <alias name="wiki">
          <value>#URL http://avatar.melanarchy.info/index.php/Special:Search?search=%1</value>
        </alias>
        <menu priority="290">
          <caption>Search for '%selword' on AvatarWiki</caption>
          <value>#URL http://avatar.melanarchy.info/index.php/Special:Search?search=%selword</value>
        </menu>
        <menu priority="300">
          <caption>Search for '%selected' on AvatarWiki</caption>
          <value>#VAR wikistring %selected;
    #IF %begins( @wikistring, "an ") {#VAR wikistring %remove( "an ", @wikistring)};
    #IF %begins( @wikistring, "a ") {#VAR wikistring %remove( "a ", @wikistring)};
    #IF %begins( @wikistring, "the ") {#VAR wikistring %remove( "the ", @wikistring)};
    #IF %begins( @wikistring, "A ") {#VAR wikistring %remove( "A ", @wikistring)};
    wiki @wikistring</value>
        </menu>
        <var name="wikistring">grand serpent hood</var>
      </class>
    </cmud>

## Usage

Just select some text in the mud, right click on it and choose the
"Search for 'x' on AvatarWiki" option. For single word searches, just
right click the word, no need to select it.  
You can also do manual searches by typing wiki followed by your search
term in quotations.  
EX: `wiki "search requirements"`

Updated for CMud v3.32.

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
