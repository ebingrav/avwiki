This package was designed to auction items.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

`#CLASS {Auctioneer}`  
`#ALIAS astart {#t+ aucTrigItem;#var aucItem "" Auctioneer;c identify %1;emote completes their inspection}`  
`#CLASS 0`  
`#CLASS {Auctioneer|aucTrigItem}`  
`#TRIGGER {Weight (%d), value %d, level (%d).} {#additem aucItem.info {Weight[%1]} Auctioneer;#additem aucItem.info {Level[%2]} Auctioneer}`  
`#TRIGGER {Damage is (%d) to (%d) ~(average (%d)~).} {#additem aucItem.info {Min[%1]} Auctioneer;#additem aucItem.info {Max[%2]} Auctioneer;#additem aucItem.info {Average[%3]} Auctioneer}`  
`#TRIGGER {Draw strength is (%d).} {#additem aucItem.info {Draw[%1]} Auctioneer}`  
`#TRIGGER {'enchant weapon 'Modifies damage roll by (%d) continuous.} {#if {%1=2} {#additem aucItem.info {Single Brill} Auctioneer} {#if {%1=4} {#additem aucItem.info {Double Brill} Auctioneer} {#if {%1=6} {#additem aucItem.info {Triple Brill} Auctioneer} {#if {%1=8} {#additem aucItem.info {Quad Brill} Auctioneer} {#additem aucItem.info {Enchant[+%1~/%1]} Auctioneer}}}}}`  
`#TRIGGER {'enchant bow    'Modifies hit roll by (%d) continuous.} {#if {%1=3} {#additem aucItem.info {Single Brill} Auctioneer} {#if {%1=6} {#additem aucItem.info {Double Brill} Auctioneer} {#if {%1=9} {#additem aucItem.info {Triple Brill} Auctioneer} {#if {%1=12} {#additem aucItem.info {Quad Brill} Auctioneer} {#additem aucItem.info {Enchant[+%1]} Auctioneer}}}}}`  
`#TRIGGER {Armor class is (%d).} {#additem aucItem.info {Base[%1]} Auctioneer}`  
`#TRIGGER {'enchant armor  'Modifies armor class by -(%d) continuous.} {#if {%1=3} {#additem aucItem.info {Single Brill} Auctioneer} {#if {%1=6} {#additem aucItem.info {Double Brill} Auctioneer} {#if {%1=9} {#additem aucItem.info {Triple Brill} Auctioneer} {#if {%1=12} {#additem aucItem.info {Quad Brill} Auctioneer} {#additem aucItem.info {Enchant[-%1]} Auctioneer}}}}}`  
`#TRIGGER {%w completes their inspection.$} {#var aucItem.name {%prompt( @aucItem.name, "Verify the items name:")} Auctioneer;#var aucItem.info {%pick( p:What would you like to display?:, @aucItem.info)} Auctioneer;#var aucItem.extra {%prompt( "", "Extra Info?:")} Auctioneer;#var aucItem.info {%replace( @aucItem.info, "|", " ")} Auctioneer;#var aucItem.info {%replace( @aucItem.info, "[", ": ")} Auctioneer;#var aucItem.info {%replace( @aucItem.info, "]", "")} Auctioneer;#var aucItem.info {@aucItem.name @aucItem.info @aucItem.extra} Auctioneer;#var aucItem.bid {%prompt( 100k, "Min Bid?:")} Auctioneer;#var aucItem.count 0 Auctioneer;#echo @aucItem.info Min bid: @aucItem.bid;#t- aucTrigItem;#t+ aucTrigChat}`  
`#TRIGGER {Object '([ a-z A-Z])'} {#var aucItem.name {%1} Auctioneer}`  
`#CLASS 0`  
`#CLASS {Auctioneer|aucTrigChat}`  
`#ALIAS achat {#if {@aucItem.count=0} {auc @aucItem.info Min bid: @aucItem.bid};#if {@aucItem.count=1} {auc @aucItem.info @aucItem.bid%if( @aucItem.bidder, %char( 32)to @aucItem.bidder, " min") Going Once};#if {@aucItem.count=2} {auc @aucItem.info @aucItem.bid%if( @aucItem.bidder, %char( 32)to @aucItem.bidder, " min") Going Twice};#if {@aucItem.count=3} {auc @aucItem.info @aucItem.bid%if( @aucItem.bidder, %char( 32)to @aucItem.bidder, " min") Last Call};#if {@aucItem.count=4} {auc @aucItem.info @aucItem.bid%if( @aucItem.bidder, %char( 32)Sold to @aucItem.bidder, " Withdrawn");#t- aucTrigChat};#var aucItem.count %eval( @aucItem.count + 1) Auctioneer}`  
`#ALIAS abid {#var aucItem.bidder %replace( %prompt( @aucItem.lastchat, "Bidder name?"), " ", "") Auctioneer;#var aucItem.bid %replace( %prompt( @aucItem.amount, "Their bid?"), " ", "") Auctioneer;#var aucItem.bid %replace( @aucItem.bid , ";", "") Auctioneer;auc @aucItem.info @aucItem.bid to @aucItem.bidder heard;#var aucItem.count 1 Auctioneer}`  
`#ALIAS aclear {auc @aucItem.info Withdrawn;#t- aucTrigChat}`  
`#TRIGGER {(%w) auctions '(%d*)'} {#var aucItem.amount %2 Auctioneer;#var aucItem.lastchat %1 Auctioneer}`  
`#CLASS 0`

## Usage

Character must have the identify spell.

Type astart <itemname>, this will identify the item and capture the
items info. Next you'll be prompted to verify the items name, select
what info you wish to display, enter any extra info and set the min bid.
After this your starting auction chat message will be echoed to you.

Next type achat. This will auction the above info and keeps a count of
4. The first time you use this it will show info + min bid, then it will
go through going once/going twice/last call and then withdrawn if you
accepted no bids, or sold if there were. Once a bid is accepted this
also shows the bidder.

To accept bids you type abid. This will prompt for the bidders name and
their bid, and then auction that the bid was accepted. There is a
trigger to help with this also, that will capture the last name and bid
(if their auction message starts with a number).

If for some reason you would like to withdraw the item early, you can
type aclear.

## How It Works

`#ALIAS astart {#t+ aucTrigItem;#var aucItem "" Auctioneer;c identify %1;emote completes their inspection}`

Turns on the item capturing triggers, resets the item variable,
identifies the item.

`#TRIGGER {Weight (%d), value %d, level (%d).} {#additem aucItem.info {Weight[%1]} Auctioneer;#additem aucItem.info {Level[%2]} Auctioneer}`  
`#TRIGGER {Damage is (%d) to (%d) ~(average (%d)~).} {#additem aucItem.info {Min[%1]} Auctioneer;#additem aucItem.info {Max[%2]} Auctioneer;#additem aucItem.info {Average[%3]} Auctioneer}`  
`#TRIGGER {Draw strength is (%d).} {#additem aucItem.info {Draw[%1]} Auctioneer}`  
`#TRIGGER {'enchant weapon 'Modifies damage roll by (%d) continuous.} {#if {%1=2} {#additem aucItem.info {Single Brill} Auctioneer} {#if {%1=4} {#additem aucItem.info {Double Brill} Auctioneer} {#if {%1=6} {#additem aucItem.info {Triple Brill} Auctioneer} {#if {%1=8} {#additem aucItem.info {Quad Brill} Auctioneer} {#additem aucItem.info {Enchant[+%1~/%1]} Auctioneer}}}}}`  
`#TRIGGER {'enchant bow    'Modifies hit roll by (%d) continuous.} {#if {%1=3} {#additem aucItem.info {Single Brill} Auctioneer} {#if {%1=6} {#additem aucItem.info {Double Brill} Auctioneer} {#if {%1=9} {#additem aucItem.info {Triple Brill} Auctioneer} {#if {%1=12} {#additem aucItem.info {Quad Brill} Auctioneer} {#additem aucItem.info {Enchant[+%1]} Auctioneer}}}}}`  
`#TRIGGER {Armor class is (%d).} {#additem aucItem.info {Base[%1]} Auctioneer}`  
`#TRIGGER {'enchant armor  'Modifies armor class by -(%d) continuous.} {#if {%1=3} {#additem aucItem.info {Single Brill} Auctioneer} {#if {%1=6} {#additem aucItem.info {Double Brill} Auctioneer} {#if {%1=9} {#additem aucItem.info {Triple Brill} Auctioneer} {#if {%1=12} {#additem aucItem.info {Quad Brill} Auctioneer} {#additem aucItem.info {Enchant[-%1]} Auctioneer}}}}}`  
`#TRIGGER {Object '([ a-z A-Z])'} {#var aucItem.name {%1} Auctioneer}`  
`#TRIGGER {%w completes their inspection.$} {#var aucItem.name {%prompt( @aucItem.name, "Verify the items name:")} Auctioneer;#var aucItem.info {%pick( p:What would you like to display?:, @aucItem.info)} Auctioneer;#var aucItem.extra {%prompt( "", "Extra Info?:")} Auctioneer;#var aucItem.info {%replace( @aucItem.info, "|", " ")} Auctioneer;#var aucItem.info {%replace( @aucItem.info, "[", ": ")} Auctioneer;#var aucItem.info {%replace( @aucItem.info, "]", "")} Auctioneer;#var aucItem.info {@aucItem.name @aucItem.info @aucItem.extra} Auctioneer;#var aucItem.bid {%prompt( 100k, "Min Bid?:")} Auctioneer;#var aucItem.count 0 Auctioneer;#echo @aucItem.info Min bid: @aucItem.bid;#t- aucTrigItem;#t+ aucTrigChat}`

These all capture the item info except the last. The last one triggers
when you are out of lag from identify, and prompts you to verify the
items name, select what captured information you would like to display,
type in any extra info, then takes that string and removes the zmud \|
info/array breaks and replaces them with spaces. The next two replace
lines remove the \[ and \] to make the display look nicer. This is also
where you would color the numbers if you so chose. Ie:

`#var aucItem.info {%replace( @aucItem.info, "[", ":|w| ")} Auctioneer`  
`#var aucItem.info {%replace( @aucItem.info, "]", "|n|")} Auctioneer`

At the end it sets the items count to 0 (read by achat to determine that
this is the first bid and to display 'min bid', prompts for your min
bid, and switches over from item capturing triggers to the chat
alias/trigger set.

`#ALIAS achat {#if {@aucItem.count=0} {auc @aucItem.info Min bid: @aucItem.bid};#if {@aucItem.count=1} {auc @aucItem.info @aucItem.bid%if( @aucItem.bidder, %char( 32)to @aucItem.bidder, " min") Going Once};#if {@aucItem.count=2} {auc @aucItem.info @aucItem.bid%if( @aucItem.bidder, %char( 32)to @aucItem.bidder, " min") Going Twice};#if {@aucItem.count=3} {auc @aucItem.info @aucItem.bid%if( @aucItem.bidder, %char( 32)to @aucItem.bidder, " min") Last Call};#if {@aucItem.count=4} {auc @aucItem.info @aucItem.bid%if( @aucItem.bidder, %char( 32)Sold to @aucItem.bidder, " Withdrawn");#t- aucTrigChat};#var aucItem.count %eval( @aucItem.count + 1) Auctioneer}`  
`#ALIAS abid {#var aucItem.bidder %replace( %prompt( @aucItem.lastchat, "Bidder name?"), " ", "") Auctioneer;#var aucItem.bid %replace( %prompt( @aucItem.amount, "Their bid?"), " ", "") Auctioneer;#var aucItem.bid %replace( @aucItem.bid , ";", "") Auctioneer;auc @aucItem.info @aucItem.bid to @aucItem.bidder heard;#var aucItem.count 1 Auctioneer}`  
`#ALIAS aclear {auc @aucItem.info Withdrawn;#t- aucTrigChat}`  
`#TRIGGER {(%w) auctions '(%d*)'} {#var aucItem.amount %2 Auctioneer;#var aucItem.lastchat %1 Auctioneer}`

This last section deals with the actual auction. It keeps a count of how
many times you've typed achat since the last bid, has a withdraw
(aclear) alias, and a trigger to ease the process of capuring bids. The
last trigger may not always capture bids due to people typing them in
odd formats but you can still type abid and manually enter the new bid.

[Category: Zmud Scripting](Category:_Zmud_Scripting "wikilink")
