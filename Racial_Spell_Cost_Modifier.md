---
title: Racial Spell Cost Modifier
permalink: wiki/Racial_Spell_Cost_Modifier/
layout: wiki
tags:
 -  Characters
 -  Races
---

**Every race** has a racial spell cost modifer associated with it. There
are three kinds of spells, arcane (mage/wzd/sor type), divine (cleric
type) and psionic.

A negative modifier means the spell cost is lower.

Effective Mana
--------------

Note that the following shows the spell cost modifier. We can define the
*effective mana* as the mana a human would need to cast the same amount
as a particular race. If x is the racial spellcost modifier (in
percentage), then the effective mana is 100 / (100 + x).

For example, a -25% spell cost modifier for sprite translates into 33%
more effective mana.

Effective mana is the more directly useful notion, since finally, that's
what determines how much you can cast.

Worship
-------

The effect of worship on spellcost is multiplicative:

`racial-modifier * (1 + worship-modifier) = cost-modifier`

Bhyss (-10%) Sprite (75% normal cost):

`0.75 * (1 + -0.1) = 0.675, which is 67.5% of regular cost - a 32.5% reduction.`

Werredan (+10%) Minotaur (125%):

`1.25 * (1 + 0.1) = 1.375, a 37.5% increase in cost, driving for example Foci from 400 to 550.`

The values
----------

The entries have been computed usually by checking spell cost for
[Foci](/wiki/Foci "wikilink") for arcane (400 mana for hum atheist),
[Fortitudes](/wiki/Fortitudes "wikilink") for psionic (400) and
[Awen](/wiki/Awen "wikilink") for divine (500) and then accounting for worship
reduction using the [Worship](/wiki/Worship "wikilink") table.

| Legend        |
|---------------|
| Divine        |
| Legendary     |
| Great         |
| Good          |
| Average       |
| Below Average |
| Bad           |

| Race                                |  Arcane Modifier |  Divine Modifier |  Psionic Modifier |
|-------------------------------------|------------------|------------------|-------------------|
| [Centaur](/wiki/Centaur "wikilink")       | -10%             | -15%             | -10%              |
| [Deep Gnome](/wiki/Deep_Gnome "wikilink") | -7.5%            | -5%              | -5%               |
| [Draconian](/wiki/Draconian "wikilink")   | 0                | 0                | -5%               |
| [Drow](/wiki/Drow "wikilink")             | -12.5%           | -10%             | -10%              |
| [Duergar](/wiki/Duergar "wikilink")       | ?                | ?                | -5%?              |
| [Dwarf](/wiki/Dwarf "wikilink")           | 0                | -5%              | -5%               |
| [Elf](/wiki/Elf "wikilink")               | -12.5%           | -10%             | -10%              |
| [Ent](/wiki/Ent "wikilink")               | 0                | -20%             | 0                 |
| [Firedrake](/wiki/Firedrake "wikilink")   | 0                | 0                | 0                 |
| [Gargoyle](/wiki/Gargoyle "wikilink")     | -15%             | -10%             | -10%              |
| [Giant](/wiki/Giant "wikilink")           | +10%             | ?                | ?                 |
| [Gnome](/wiki/Gnome "wikilink")           | -7.5%            | -5%              | -5%               |
| [Goblin](/wiki/Goblin "wikilink")         | +10%             | +10%             | +10%              |
| [Halfling](/wiki/Halfling "wikilink")     | 0                | 0                | 0                 |
| [Half-Elf](/wiki/Half-Elf "wikilink")     | ?                | ?                | ?                 |
| [Half-Orc](/wiki/Half-Orc "wikilink")     | ?                | ?                | ?                 |
| [Harpy](/wiki/Harpy "wikilink")           | 0                | +15%             | 0                 |
| [Human](/wiki/Human "wikilink")           | 0                | 0                | 0                 |
| [Imp](/wiki/Imp "wikilink")               | Good             | Bad              | Good              |
| [Kobold](/wiki/Kobold "wikilink")         | ?                | ?                | ?                 |
| [Kzinti](/wiki/Kzinti "wikilink")         | 0                | ?                | 0                 |
| [Lizardmen](/wiki/Lizardmen "wikilink")   | 0                | 0                | -5%               |
| [Ogres](/wiki/Ogres "wikilink")           | +10%             | 0                | +10%              |
| [Orc](/wiki/Orc "wikilink")               | +10%             | 0                | +10%              |
| [Troglodyte](/wiki/Troglodyte "wikilink") | +25%             | +25%             | +22.5%            |

<div style="clear: both;">
</div>
  

| Race                              |  Arcane Modifier |  Divine Modifier |  Psionic Modifier |
|-----------------------------------|------------------|------------------|-------------------|
| [Demonseed](/wiki/Demonseed "wikilink") | -12.5%           | -10%             | -10%              |
| [Dragon](/wiki/Dragon "wikilink")       | -10%             | -10%             | -15%              |
| [Golem](/wiki/Golem "wikilink")         | 0                | +10%             | +10%              |
| [Griffon](/wiki/Griffon "wikilink")     | 0                | 0                | 0                 |
| [High Elf](/wiki/High_Elf "wikilink")   | -15%             | -15%             | -20%              |
| [Hobgoblin](/wiki/Hobgoblin "wikilink") | ?                | ?                | ?                 |
| [Minotaur](/wiki/Minotaur "wikilink")   | +25%             | +15%             | +25%              |
| [Miraar](/wiki/Miraar "wikilink")       | 0                | 0                | -10%              |
| [Sprite](/wiki/Sprite "wikilink")       | -25%             | -25%             | -25%              |
| [Troll](/wiki/Troll "wikilink")         | +5%              | +10%             | +10%              |
| [Tuataur](/wiki/Tuataur "wikilink")     | -10%             | -10%             | -20%              |


