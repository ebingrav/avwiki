This script will allow you to filter the group list to a specific subset
of characters. It is designed for Zmud, but should work with slight
modifications with Cmud. (In particular, the =\~ operator must be
changed, though Cmud will tell you this.)

## The Guts

This is the alias, **check**, that you use in place of the group
command. It accepts either the name of a character or a keyword. (In
this example, it's *tanks*.)

`#ALIAS check {`  
`  #if (%numparam( ) != 0) {`  
`    #if (%lower( %1) =~ "tanks") {#var watch @tanks} {#var watch %1}`  
`    #T+ check_trigger_on`  
`    group`  
`    }`  
`  }`

This pipe-delimeted string holds our list of tanks. It's up to you to
populate it. Remember to use **\|**, but the \#additem and \#delitem
commands will do this for you.

`#VAR tanks {}`

This variable is a list of characters we want to see. You never need to
touch this.

`#VAR watch {}`

This trigger simply turns on the following triggers at the start of the
group list output.

`#TRIGGER "check_trigger_on" {^##| Level Name Pos HitPoints ManaPoints MovePoints TNL Align$} {`  
`  #T+ check_trigger`  
`  #T+ check_trigger_off`  
`  #T- check_trigger_on`  
`  } "" {disable}`

This does the actual filtering. It's currently configured to only work
in hero and lord groups.

`#TRIGGER "check_trigger" {^{| }%d|{| | }%d {Lord|Hero} (%w)%s{Stand|Sleep|Rest|Fight|STUN!|DROWN}%s(%n)/(%d)%s(%n)/(%d)%s%n/%d%s%d%s%n$} {#if (!%ismember( %lower( %1), %lower( @watch))) {#gag}} "" {disable}`

This disables all the triggers so that the group command works properly
and will not be filtered. It is triggered by a blank line; namely, the
one between the group output and your next prompt. *This depends on
having **config +NEWLINE** turned on! If it is off, this trigger will
have to be custom tailored to fire on your specific prompt!*

`#TRIGGER "check_trigger_off" {^$} {`  
`  #T- check_trigger`  
`  #T- check_trigger_off`  
`  } "" {disable}`

## Usage

With the script running, use the **check** alias with the name of a
player to view a sort of poor man's [Saving
Grace](Saving_Grace "wikilink"), which is not extremely useful. More to
the point of this script, use **check** with a keyword to filter your
group list to a specific subset. (The example keyword in the script is
*tanks*, so if you typed **check tanks** you would see a brief status of
your group's hit points.) If you want to add more keywords, add the
appropriate \#if statements into the **check** alias, and create a
variable to store the names. If you use the alias with a keyword, you'll
need to have its variable populated with names before running. You can
either do this manually, or if you have a group breakdown script that
categorizes group members, you can edit the **check** alias to reference
the variables that your breakdown script uses.

All of the triggers start disabled and disable themselves after use, so
the **group** command will still work as normal.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
