Notes are the MUD's in-game messages, which you can read and send while
playing the game.

## Reading Notes

There are two ways to look at notes: scan the new ones until you've seen
them all, or pick the one you want to see.

Method \#1: If you intend to read all of the unread notes, NOTE SCAN
will scan through all message boards and display the first unread note
it finds. This is the preferred way if you normally read notes.

Method \#2: If you'd like to read a certain note, first find it using
NOTE LIST. Switch to the board the note is in, then type "note list".
Remember the number of the note, then type "note read \####", where
\#### is the number of the note.

Example:

  
  
board 2

note list

note read 10

Again, if you normally read all new notes, NOTE SCAN is easier to use.

## Posting Notes

How to post a note - an example.

Posting a note is easier than the help on notes leads you to believe.
There are a few things to remember when posting: who it goes to, what it
is about, and what message board it should be on. First, select the
proper note board - ie, 1 for a general note, 5 for a bug report, and so
on. See "boards" for help on selecting the board. Next, who will this go
to? Type "note to", and then a list of names to receive the note. A
short subject line goes after "note subject". Add lines one-by-one; type
"note +", followed by a single line of text. If you mess up a line,
delete it by typing "note -".

Need an example? I do! This message will be posted on the general board
(board \#1), It will go to all heroes and to Caveman. This is just what
the writer of the note typed, everything else has been cut out... he's
in a quiet room!

Example:

  
  
board 1

note to hero caveman

note subject Thanks for the help!

note + I wanted to say thanks to the heroes and to Caveman for helping

note + me retrieve my corpse after my 10th death yesterday. Tmaks alot!

note -

note + me retrieve my corpse after my 10th death yesterday. Thanks a
lot!

note submit

Oops, did he type that line twice? Nope, he retyped it... the writer
messed up the end of the second line ("tmaks alot"), so he deleted it
("note -") and retyped. Don't forget that you can type "note show" to
see what your note looks like so far.

N.B.: Lines in a note are limited to 80 characters. Notes are limited to
30 lines.

## Who to send notes to

Notes don't have to go to a single specific player, or a specific list
of players - instead, some "aliases" exist to make things a little
easier.

Here they are:

| All      | All players, from level 3 lowmort and up |
|:---------|------------------------------------------|
| Hero     | Hero tier players and above only         |
| Lord     | Lord tier players and above only         |
| Angel    | Angels and above only                    |
| Immortal | Immortals level 820 and up only          |
| Senior   | Immortals level 900 and up only          |
| Exec     | Exec staff - see wizlist                 |

When you set NOTE TO for a note, you can use a mixed list of names and
aliases for the note to go to.

Example:

  
  
note to bob fred immortal angel

## Note Commands

<table>
<thead>
<tr class="header">
<th style="text-align: left;"><p>Command:</p></th>
<th style="text-align: left;"><p>Description:</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;"><p>note to [followed by nothing]</p></td>
<td style="text-align: left;"><p>lists options for TO</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>note to <to-list></p></td>
<td style="text-align: left;"><p>sets the recipient(s)</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>note subject <string></p></td>
<td style="text-align: left;"><p>sets the subject line</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>note + &lt;1 line of text&gt;</p></td>
<td style="text-align: left;"><p>adds a new line of text (upto 80
letters long)</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>note -</p></td>
<td style="text-align: left;"><p>deletes last line of text</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>note show</p></td>
<td style="text-align: left;"><p>shows note in progress</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>note clear</p></td>
<td style="text-align: left;"><p>clears note in progress</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>note submit</p></td>
<td style="text-align: left;"><p>posts current note to boards</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>note list &lt;optional #&gt;</p></td>
<td style="text-align: left;"><p>lists notes on current board starting
from beginning &lt;#&gt;</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>note read OR note read next</p></td>
<td style="text-align: left;"><p>reads next unread note on current
board</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>note read &lt;note_#&gt;</p></td>
<td style="text-align: left;"><p>reads note of specified #</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>note scan</p></td>
<td style="text-align: left;"><p>reads next unread note even if it is on
another board</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>note remove &lt;note_#&gt;</p></td>
<td style="text-align: left;"><p>removes specified note from your
boards; if you are the sender, removes from everyone's</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>note catchup &lt;board_#&gt; note
catchup all</p></td>
<td style="text-align: left;"><p>marks all notes on specified default to
current or all board(s) as read; excludes immortal board</p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>note reset &lt;board_#&gt; OR
all</p></td>
<td style="text-align: left;"><p>marks all notes on specified default to
current or all boards as unread</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>note redirect &lt;note#&gt; TO
<name></p></td>
<td style="text-align: left;"><p>changes the TO field for &lt;note_#&gt;
on the current board to <name></p></td>
</tr>
<tr class="odd">
<td style="text-align: left;"><p>note search</p>
<note section>
<p><text></p></td>
<td style="text-align: left;"><p>search within a board using a specified
note section of either: to, from, subject, or body</p></td>
</tr>
<tr class="even">
<td style="text-align: left;"><p>note searchall</p>
<note section>
<p><text></p></td>
<td style="text-align: left;"><p>search all boards using a specified
note section of either: to, from, subject, or body</p></td>
</tr>
</tbody>
</table>

## Boards

The boards are different "threads" or "spools" of note lists.  
Use the notes commands within each board to read and manipulate
messages.

You can change the active board simply by typing board, followed by the
number of the board.  
To see a list of the boards available, simply type 'board'.

|                                      |
|:-------------------------------------|
| The boards are organized as follows: |
| General                              |
| Personal                             |
| Quest                                |
| Questions                            |
| Bugs                                 |
| Creative                             |
| Immortal                             |

[Category: Communication
Commands](Category:_Communication_Commands "wikilink")
