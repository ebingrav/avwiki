This gear changing system can be expanded to any number of sets of gear
(up to your [ Carrying Capacity](Carrying_Capacity.md "wikilink"). It
relies on a storage system consisting of a main
[bag](:Category:_Containers.md "wikilink") with bags of gear in it, and
a miscelanious bag.

## Benefits of using this system

-   You don't need to rewrite these aliases when you get a new piece of
    gear.
-   You don't need to worry about any items being put in the wrong bag.
-   You don't need to etch anything other than your bags.
-   You don't Produce nearly any spam changing gear.

## Detriments of using this system

-   The system relies on you being in a known state, however that state
    can be easily achieved.

## Example Setup/ Bag Etchings

So in my case, I have 4 sets of gear. Tank, Hit, Mana, And Det AC. I
obtain one unique bag for each set (Mana - Urn, Det - BodyBag, Hit -
Head, Tank - Jumpsuit) As well as One Main Bag (Floating Watersphere)
and one Miscelanious Bag (Girdle of many Pouches)

The 4 bags which are to contain a set of gear are each etched with the
SAME keyword (I use BolGearBag), And I recomend etching the other 2
(Bolmainbag and bolmiscbag in my case)

## Example Aliases

Here are suggested aliases to change gear :

First we create one alias that will put on each set of gear...

Each of them should look something like this...

> [alias](Alias.md "wikilink") deton get 'body bolgearbag' 'flo
> bolmainbag':get all 'body bolgearbag':put 'body bolgearbag' 'gir
> bolmiscbag':wear all

This assumes you are naked, and have Bolmainbag and Bolmiscbag in your
inventory, as well as all bolgearbags inside bolmainbag, with their
apropriate sets of gear inside them. Note that it leaves the empty bag
in bolmiscbag, NOT in bolmainbag where you got it from. This is how we
track our state.

Now the tricky part. We make an alias that will remove ANY set of gear
regardless of which and put it away properly. Simply put, we just stash
everything were wearing into whichever bolgearbag is in bolmiscbag.

Heres the alias.

> [alias](Alias.md "wikilink") gearoff put all bolmiscbag:get bolgearbag
> bolmiscbag:rem all:put all bolgearbag:get bolmiscbag bolgearbag:get
> bolmainbag bolgearbag:get bolmainbag bolmiscbag:put all.bolgearbag
> bolmainbag

This dumps anything else in your inventory into bolmiscbag too.

Tne result is that when you run this alias, you end up with all your
gear neatly stowed back where it should be, and ready to run whichever
on alias you need.

Then, no matter what set your wearing, changing to tank gear is simply a
case of tying

gearoff=tankon

## Some hints

Just rememebr one thing, If you ever manage to stuff your gear up, take
it off, stick it all back intot the right containers, and stick them all
into mainbag, then use (setname)on to get whatever you need back out.

## More Complicated ALias Sets

Heres the alias set im currently using on Av. It covers some more
complex situations such as items shared between multiple gearsets,
shields that you want in the inventory for hit gear.

No explaination will be provided. Use at own risk.

> [alias](Alias.md "wikilink") deton get 'body bolgearbag' 'flo
> bolmainbag':get all 'body bolgearbag':put 'body bolgearbag' 'gir
> bolmiscbag':wear all

> [alias](Alias.md "wikilink") hiton get 'head bolgearbag' 'flo
> bolmainbag':get all 'head bolgearbag':put 'head bolgearbag' 'gir
> bolmiscbag':get 'jump bolgearbag' 'flo bolmainbag':get 'zarr gau'
> 'jump bolgearbag':get 'liv daem' 'jump bolgearbag':put 'jump
> bolgearbag' 'flo bolmainbag':wear all:get shield bolmiscbag

> [alias](Alias.md "wikilink") manaon get 'urn bolgearbag' 'flo
> bolmainbag':get all 'urn bolgearbag':put 'urn bolgearbag' 'gir
> bolmiscbag':wear all

> [alias](Alias.md "wikilink") tankon get 'jump bolgearbag' 'flo
> bolmainbag':get all 'jump bolgearbag':put 'jump bolgearbag' 'gir
> bolmiscbag':wear all

> [alias](Alias.md "wikilink") gearoff put all bolmiscbag:get bolgearbag
> bolmiscbag:rem all:put all bolgearbag:get bolmiscbag bolgearbag:get
> bolmainbag bolgearbag:get bolmainbag bolmiscbag:get 'liv dae' 'head
> bolgearbag':get 'zarr gau' 'head bolgearbag':get 'jump bolgearbag'
> bolmainbag:put 'zarr gau' 'jump bolgearbag':put 'liv dae' 'jump
> bolgearbag':put all.bolgearbag bolmainbag

[Category: Gear Changing
Aliases](Category:_Gear_Changing_Aliases "wikilink")
