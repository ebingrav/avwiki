**Every race** has a racial spell cost modifer associated with it. There
are three kinds of spells, arcane (mage/wzd/sor type), divine (cleric
type) and psionic.

A negative modifier means the spell cost is lower.

## Effective Mana

Note that the following shows the spell cost modifier. We can define the
*effective mana* as the mana a human would need to cast the same amount
as a particular race. If x is the racial spellcost modifier (in
percentage), then the effective mana is 100 / (100 + x).

For example, a -25% spell cost modifier for sprite translates into 33%
more effective mana.

Effective mana is the more directly useful notion, since finally, that's
what determines how much you can cast.

## Worship

The effect of worship on spellcost is multiplicative:

`racial-modifier * (1 + worship-modifier) = cost-modifier`

Bhyss (-10%) Sprite (75% normal cost):

`0.75 * (1 + -0.1) = 0.675, which is 67.5% of regular cost - a 32.5% reduction.`

Werredan (+10%) Minotaur (125%):

`1.25 * (1 + 0.1) = 1.375, a 37.5% increase in cost, driving for example Foci from 400 to 550.`

## The values

The entries have been computed usually by checking spell cost for
[Foci](Foci "wikilink") for arcane (400 mana for hum atheist),
[Fortitudes](Fortitudes "wikilink") for psionic (400) and
[Awen](Awen "wikilink") for divine (500) and then accounting for worship
reduction using the [Worship](Worship "wikilink") table.

{.md\| border="2" cellpadding="2" cellspacing="0" align="center" \|- !
colspan=2 \| Legend \|- align="center" \|
style="background-color:#8080ff;" \| Divine \|- align="center" \|
style="background-color:#c0c0ff;" \| Legendary \|- align="center" \|
style="background-color:#ddffdd;" \| Great \|- align="center" \|
style="background-color:#ffffbb;" \| Good \|- align="center" \| Average
\|- align="center" \| style="background-color:#ffdddd;" \| Below Average
\|- align="center" \| style="background-color:#ffaaaa;" \| Bad \|-
align="center" \| style="background-color:#ff6666;" \| Absurd \|}

| Race                                                                                          |  Arcane Modifier |  Divine Modifier |  Psionic Modifier |
|-----------------------------------------------------------------------------------------------|------------------|------------------|-------------------|
| [Centaur](Centaur "wikilink") .md\| style="background-color:#ffffbb;" \| -10%                 | -15%             | -10%             |                   |
| [Deep Gnome](Deep_Gnome "wikilink")                                                           | -7%              | -5%              | -5%               |
| [Draconian](Draconian "wikilink") .md\| 0                                                     | 0                | -5%              |                   |
| [Drow](Drow "wikilink")                                                                       | -12%             | -10%             | -10%              |
| [Duergar](Duergar "wikilink") .md\| style="background-color:#ffdddd;" \| +10%                 | -2%              | +2%              |                   |
| [Dwarf](Dwarf "wikilink")                                                                     | +10%             | -5%              | +6%               |
| [Elf](Elf "wikilink") .md\| style="background-color:#ffffbb;" \| -12%                         | -10%             | -10%             |                   |
| [Ent](Ent "wikilink")                                                                         | 0                | -20%             | 0                 |
| [Firedrake](Firedrake "wikilink") <sup>1</sup> .md\| style="background-color:#ffffbb;" \| -2% | -2%              | -2%              |                   |
| [Gargoyle](Gargoyle "wikilink")                                                               | -15%             | -10%             | -10%              |
| [Giant](Giant "wikilink") .md\| style="background-color:#ffdddd;" \| +10%                     | +10%             | +10%             |                   |
| [Gnome](Gnome "wikilink")                                                                     | -7%              | -5%              | -5%               |
| [Goblin](Goblin "wikilink") .md\| style="background-color:#ffdddd;" \| +10%                   | +10%             | +10%             |                   |
| [Halfling](Halfling "wikilink")                                                               | 0                | 0                | 0                 |
| [Half-Elf](Half-Elf "wikilink") .md\| style="background-color:#ffffbb;" \| -6%                | -5%              | -5%              |                   |
| [Half-Orc](Half-Orc "wikilink")                                                               | +5%              | 0                | +5%               |
| [Harpy](Harpy "wikilink") .md\| 0                                                             | +15%             | 0                |                   |
| [Human](Human "wikilink")                                                                     | 0                | 0                | 0                 |
| [Imp](Imp "wikilink")<sup>2</sup> .md\| style="background-color:#ffffbb;" \| -10%             | 0                | -10%             |                   |
| [Kobold](Kobold "wikilink")                                                                   | 30%              | 30%              | 30%               |
| [Kzinti](Kzinti "wikilink") .md\| 0                                                           | 0                | 0                |                   |
| [Lizardmen](Lizardmen "wikilink")                                                             | 0                | 0                | -5%               |
| [Ogres](Ogres "wikilink") .md\| style="background-color:#ffdddd;" \| +10%                     | 0                | +10%             |                   |
| [Orc](Orc "wikilink")                                                                         | +10%             | 0                | +10%              |
| [Troglodyte](Troglodyte "wikilink") .md\| style="background-color:#ffaaaa;" \| +25%           | +25%             | +22.5%           |                   |

<big>**[Creatable Races](:Category:_Creatable_Races "wikilink") Spell
Cost Modifiers**</big>

<div style="clear: both;">
</div>

  

| Race                                                                              |  Arcane Modifier                         |  Divine Modifier                        |  Psionic Modifier |
|-----------------------------------------------------------------------------------|------------------------------------------|-----------------------------------------|-------------------|
| [Demonseed](Demonseed "wikilink") .md\| style="background-color:#ffffbb;" \| -13% | -10%                                     | -10%                                    |                   |
| [Dragon](Dragon "wikilink")                                                       | -10%                                     | -10%                                    | -15%              |
| [Drider](Driders.md "wikilink")                                                   | -2%                                      | -5%                                     | 0%                |
| [Gith](Gith "wikilink") .md\| style="background-color:#ffffbb;" \| -5%            | -5%                                      | -17%                                    |                   |
| [Golem](Golem "wikilink")                                                         | 0                                        | +10%                                    | +10%              |
| [Griffon](Griffon "wikilink") .md\| 0                                             | 0                                        | 0                                       |                   |
| [High Elf](High_Elf "wikilink")                                                   | -15%                                     | -15%                                    | -20%              |
| [Hobgoblin](Hobgoblin "wikilink") .md\| style="background-color:#ffdddd; \| +10%  | style="background-color:#ffdddd; \| +10% | style="background-color:#ffdddd; \| +5% |                   |
| [Ignatur](Ignatur "wikilink")                                                     | -8% <sup>3</sup>                         | 0                                       | -4%               |
| [Minotaur](Minotaur "wikilink") .md\| style="background-color:#ffaaaa;" \| +25%   | +15%                                     | +25%                                    |                   |
| [Miraar](Miraar "wikilink")                                                       | 0                                        | 0                                       | -10%              |
| [Sprite](Sprite "wikilink") .md\| style="background-color:#8080ff;" \| -25%       | -25%                                     | -25%                                    |                   |
| [Troll](Troll "wikilink")                                                         | +10%                                     | +15%                                    | +15%              |
| [Tuataur](Tuataur "wikilink") .md\| style="background-color:#ffffbb;" \| -10%     | -10%                                     | -20%                                    |                   |

<big>**[Remort Races](:Category:_Remort_Races "wikilink") Spell Cost
Modifiers**</big>

<sup>`1`</sup>` Firedrakes start with 0% spell cost reduction and gain -2% reduction after they evolve at hero 250`  
<sup>`2`</sup>` Spell cost reduction for a Pain Elemental, spell cost reductions is worse at other stages of evolution`  
<sup>`3`</sup>` -15% spell cost on fire spells for Ignatur`

[Category: Characters](Category:_Characters "wikilink") [Category:
Races](Category:_Races "wikilink")
