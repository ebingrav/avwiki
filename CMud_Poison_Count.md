This script will keep track of how many poisons are left one your
weapons (Venom and Virus) after examining them once.

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="Poison Count" copy="yes">
        <trigger priority="18540" copy="yes">
          <pattern>^Crying out in pain, %1 succumbs to venom!</pattern>
          <value>#var venomcount %eval(@venomcount-1)
    #echo @venomcount venom doses left.</value>
        </trigger>
        <trigger priority="18550" copy="yes">
          <pattern>^%1 shudders as the virus takes hold!</pattern>
          <value>#var viruscount %eval(@viruscount-1)
    #echo @viruscount virus doses left.</value>
        </trigger>
        <trigger priority="18560" copy="yes">
          <pattern>^It carries roughly &amp;venomcount doses of venom, a very strong poison with no secondary affects.$</pattern>
        </trigger>
        <trigger priority="18570" copy="yes">
          <pattern>^It carries roughly &amp;viruscount doses of virus, not so much a poison as an illness, this attacks the muscles, greatly affecting strength and speed.</pattern>
        </trigger>
      </class>
    </cmud>

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
