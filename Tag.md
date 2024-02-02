1 TAG 'TAG COMMAND'

    syntax:   tag show                     * show your tags
     -or-     tag show <player>            * show <player>'s tags
     -or-     tag show ?<prefix>           * show your tags starting with <prefix>
     -or-     tag show <player> ?<prefix>  * show <player>'s tags starting with <prefix>
     -or-     tag remove <label>           * remove the tag with <label>
     -or-     tag set <label>              * set a tag without info content
     -or-     tag set <label> <info>       * set a tag with info content
     -or-     tag clear all                * remove all tags

Tags are a method to provide information about your character to others.
Tags are considered public information, so language rules apply. A
maximum of 20 tags can be set. It is possible to search for players with
Tags via the 'who'-command (who tag or who tag ?<prefix>).

Parameters for tags:

    label - single word of maximum length 12 characters,
          - can contain letters, digits, '-' and '_'
    info  - multiple words with maximum 60 characters total length
          - colors codes may be used, but count towards max length

SEE ALSO: WHO, RULES-LANGUAGE

## Notes

Players will often use the Tag command to signal that they are in a
particular group or clan, or are currently in a Spellbot or Bot mode.

Players can use the [WHO](Who "wikilink") command to search for players
with a tag, or with a specific tag. Syntax is as follows:

who tag - Will show any players with any tag.  
who tag ?bot - Will show any players with the word "bot" in their tag.
(everything after the ?)

[Category: Commands](Category:_Commands "wikilink")
