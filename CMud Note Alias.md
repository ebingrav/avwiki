This is a script for those of you tired of seeing the "80 character" max
issue. Basically you can just type "noteline" and an entire message and
it will automatically split it into lines of 80 characters or less and
send note + those lines.

Note sure if this is gonna work off copy/paste so I've uploaded a txt
file here: <http://avatar.melanarchy.info/images/b/bd/Noteline.txt>

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <alias name="noteline" copy="yes">
        <value>#var thelinevar {%-1}
    #var templinevar {%replace( @thelinevar, " ", "=")}
    #var linespam ""
    #var lineword ""
    #var thelinecount 0
    #loop 1,%numwords( @templinevar, "=") {
      #var lineword %word( %replace( @templinevar, "=", " "), %i)
      #echo @lineword
      #if %eval( %len( @linespam)+%len( @lineword))&gt;=80 {note + @linespam
      #var linespam @lineword} {#var linespam %trimleft(%concat(@linespam," ",@lineword))}
      }
    #if @linespam="" {} {note + @linespam
    #var linespam ""}</value>
      </alias>
    </cmud>

[Category:Cmud Scripting](Category:Cmud_Scripting "wikilink")
