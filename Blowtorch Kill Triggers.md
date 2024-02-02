## How To Create a Kill Trigger in Blowtorch for Android

-   1\. Click the three dots at the top right of the screen
-   2\. choose "Triggers"
-   3\. Choose "New"
-   4\. Choose a name. I chose "kill trigger".
-   5\. Add the pattern: "^(.+) is killing (.+).$
-   6\. Choose "New Action".
-   7\. Choose "Ack With"
-   8\. Add this to the Send this Command: "kill $2"
-   9\. Uncheck the literal box. You might get a warning, click ok.
-   10\. Click Done twice, and you have a kill trigger!

It should look like this: ![](OGWpoNH.png "OGWpoNH.png")

## Features

-   This trigger causes you to automatically attack any mob when someone
    uses the typical "is killing" emote.

[Category:Blowtorch Scripting](Category:Blowtorch_Scripting "wikilink")
