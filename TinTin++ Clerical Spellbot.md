The following program assumes you have a Lord cleric, preferably a
gnome, named Teacup. If this is not the case, you will wish to edit the
relevant parts. It also assumes your prompt looks something like:

(6022/6022)(4251/5418 s:off a:off)(3450/3450)(0)

With the first parenthesis being hp/hpfull, second mana/manafull
s:surgelevel a:augmentlevel, and third move/movefull. Last parenthesis
is lag, which is also useful to have when debugging. Should you wish to
change any of this, feel free to use scissors and duct tape liberally.

Since this script is also a portalbot, the following lists and lists of
helpful wishes can also be cut away if you do not wish to do portals.

The infoset/channel switching is there to reduce spam. Once upon a time
the bot always had a deathinfo channel on to facilitate requiems, but
this part is now also neutered.

Also, the script does **NOT** handle priority queues nor does is care
one bit about casting failures. Use it on a well-established failrace
bot, or on a nofail race for best results.

Some bots will not take orders when awake to prevent being swamped by
div requests of a ton of spellup requests at the same time. In such a
scenario, the bot would check mana, find it sufficient for ONE casting
of spells, and then proceed to cast spellups - say 10 times - only to
hit the bottom end of its manapool. This is where priority queues come
in, and sequential checking of mana status prior to EVERY casting,
something this script does **not** do. In practice though, this has
proved to be a non-issue.

## Staying Online

First and foremost, you will wish to have the bot online at all times.
Achieve this by inserting the following code into your MAIN script (the
one responsible for logging in):

`#event {SESSION DISCONNECTED}`  
`{`  
`    #gts #delay {10} {#read teacup/teacup.char}`  
`}`

Replace the script name with your own script naturally.

In the same script insert the following commands:

`idleron`  
`spellboton`

The first command should enable the [TinTin++
Anti-Idler](TinTin++_Anti-Idler "wikilink") script, a separate piece of
code. The second one will insure that the bot goes online when you relog
after a link/mud crash etc.

## Main Code

*Put the following into a file in your tintin++ folder and use **\#read
<filename>** while in tintin to load them up. Though you probably want
them to autoload with your character.*

    #variable bestwishes[1] Don't die horribly.
    #variable bestwishes[2] Don't forget to spell up.
    #variable bestwishes[3] What could |W|possibly|N| go wrong?
    #variable bestwishes[4] Take care!
    #variable bestwishes[5] Hope you were savespelled.
    #variable bestwishes[6] Hmm. Portals always go to right places, |R|right|N|?
    #variable bestwishes[7] Lowmorts should |BW|look por|N| before entering.
    #variable bestwishes[8] Good luck!
    #variable bestwishes[9] Good hunting!
    #variable bestwishes[10] Donate corpses, for the hungry!
    #variable bestwishes[11] Do you need astral shield? Well, don't have one! Har. Har.
    #variable bestwishes[12] I'm the best spellbot. Admit it.
    #variable bestwishes[13] Playerinfo me for full spell list.
    #variable bestwishes[14] I'll cheer for you.
    #variable bestwishes[15] Killem all.
    #variable bestwishes[16] Show no mercy.
    #variable bestwishes[17] Take no prisoners.
    #variable bestwishes[18] Show them what you've got.
    #variable bestwishes[19] I'll heal you when you return.
    #variable bestwishes[20] Sounds like you've got work to do.
    #variable bestwishes[21] Sneaking? Hidden? Just checking.
    #variable bestwishes[22] I do collect gems you know? BIG ones.
    #variable bestwishes[23] Have no pity!
    #variable bestwishes[24] Leave no survivors.
    #variable bestwishes[25] Show no fear.
    #variable bestwishes[26] If you get lost, ask me to nexus to you.
    #variable bestwishes[27] Nothing can go wrong. Nothing.
    #variable bestwishes[28] Trust the portalbots implicitely.
    #variable bestwishes[29] Don't come back without some heads.
    #variable bestwishes[30] Hope you know what you're doing.
    #variable bestwishes[31] Doesn't sound dangerous. |R|Right|N|?
    #variable bestwishes[32] I wonder who's actually there.
    #variable bestwishes[33] Beep me to come with!
    #variable bestwishes[34] I'd wish you good luck but you'd die anyway.
    #variable bestwishes[35] When is the last time you |BW|playerinfo|N|ed me?
    #variable bestwishes[36] How often do you hug bots? We have feelings too you know.
    #variable bestwishes[37] Think of me on the bot-of-the-year voting, ok?
    #variable bestwishes[38] You sure you haven't forgotten something?
    #variable bestwishes[39] That will be 1 gold, thank you.
    #variable bestwishes[40] I hope I haven't messed up something, happens sometimes.
    #variable bestwishes[41] That went well.
    #variable bestwishes[42] Y'know, I just LOVE portalling.
    #variable bestwishes[43] Send me a postcard.
    #variable bestwishes[44] Did you pack everything?
    #variable bestwishes[45] Buy some milk on the way back.
    #variable bestwishes[46] Ever tried peeking through with only your head?
    #variable bestwishes[47] What happens if I seal it while you're halfway in?
    #variable bestwishes[48] I just love dialing that destination.
    #variable bestwishes[49] Watch your step over there.
    #variable bestwishes[50] Come back soon.
    #variable bestwishes[51] By the way, your fly is undone.
    #variable bestwishes[52] When was the last time you ate?
    #variable bestwishes[53] I'd like to go too!
    #variable bestwishes[54] Bring me something.
    #variable bestwishes[55] Should be safe enough.
    #variable bestwishes[56] Don't get eaten or something... animated even...
    #variable bestwishes[57] Say hello to whomever is there for me.
    #variable bestwishes[58] Gnomish Health Association says repetitious portaling is bad for health.
    #variable bestwishes[59] I charge these spells, you *do* know that? Look at me next time.
    #variable bestwishes[60] Remember, nothing like a good death at the end of the run.
    #variable bestwishes[61] Be safe.
    #variable bestwishes[62] Watch out, there may be |BW|mobs|N| over there!
    #variable bestwishes[63] Careful now.
    #variable bestwishes[64] When you get back I'll tell you a funny story about a priest and a nexus.
    #variable bestwishes[65] No portal is safe.
    #variable bestwishes[66] Ever tried dialing another plane?
    #variable bestwishes[67] If you collect some portals over there, bring them to me so I can replenish my supply.
    #variable bestwishes[68] Always enter weapons first.
    #variable bestwishes[69] I always start with a preemptive .. sneak .. as soon as I emerge. Try it!
    #variable bestwishes[70] Ever tried tossing a mob through nexus?
    #variable bestwishes[71] Is that mana gear you're wearing?
    #variable bestwishes[72] Got brandies ready?
    #variable bestwishes[73] You *do* know you can group me when I'm not botting, right?
    #variable bestwishes[74] Jump!
    #variable bestwishes[75] Whoa, that portal is unstable! Tee-hee, just kidding.
    #variable bestwishes[76] I collect mob fingers. Get me some.
    #variable bestwishes[77] Sometimes, deep at night, I cast portals to Aelmon because I'm bored. He is not amused.
    #variable bestwishes[78] Do you think that mob will appreciate you coming?
    #variable bestwishes[79] If you see other gnomes there, well... Kill them anyway.
    #variable bestwishes[80] Would you hate me if I sometimes sent you at random places?
    #variable bestwishes[81] Sequential poetry, pt. 1: Mary had a little portable portal, it crackled white as snow...
    #variable bestwishes[82] Sequential poetry, pt. 2: ...and everywhere that Mary went, to Aelmon she could go...
    #variable bestwishes[83] Sequential poetry, pt. 3: ...It portalled her to MUDschool one day: That was against the rule...
    #variable bestwishes[84] Sequential poetry, pt. 4: ...It made the IMMs laugh and play, to see Mary at MUDschool.
    #variable bestwishes[85] I collect lockboxes. Why not get me some?
    #variable bestwishes[86] To open lockboxes I need picks! Got a spare?
    #variable bestwishes[87] It's been a hectic day. Portal here, nexus there...
    #variable bestwishes[88] Wait, you ment the mob on Eragora, right?
    #variable bestwishes[89] I was thinking of portalling you to Thorngate but I had a change of heart.
    #variable bestwishes[90] So, I'm thinking of remorting sprite. Wouldn't that be grand? Naah, it wouldn't.
    #variable bestwishes[91] I know. I'll remort High Gnome. Then I'll go work for Zin.
    #variable bestwishes[92] Mind the head.
    #variable bestwishes[93] Aren't you a bit too small to go there?
    #variable bestwishes[94] Woah, bold choice.
    #variable bestwishes[95] Not my idea for a summer vacation.
    #variable bestwishes[96] So, aside for mobs, gear and money, why go there at all?
    #variable bestwishes[97] Hmph, I wanted to go there too.
    #variable bestwishes[98] Wait, come back, portaled to the wrong place!
    #variable bestwishes[99] Is that mob good deep-fried?
    #variable bestwishes[100] Kill it.

    #variable spellupdone[1] Off you go now.
    #variable spellupdone[2] You're done.
    #variable spellupdone[3] Scoot.
    #variable spellupdone[4] Shoo!
    #variable spellupdone[5] Next!
    #variable spellupdone[6] Gogogo!
    #variable spellupdone[7] You're spelled!
    #variable spellupdone[8] Go run!
    #variable spellupdone[9] There you go.
    #variable spellupdone[10] They're waiting on you.
    #variable spellupdone[11] There.
    #variable spellupdone[12] You did vote, right?
    #variable spellupdone[13] Lockboxes to me. Picks as well.
    #variable spellupdone[14] That'll be 14 coppers.
    #variable spellupdone[15] I accept gems as payment. Perfect ones too.
    #variable spellupdone[16] Are you still here?
    #variable spellupdone[17] Why are you not running yet?
    #variable spellupdone[18] You're gonna be late.
    #variable spellupdone[19] Tick-tock, spells ticking.
    #variable spellupdone[20] Good luck!

    #variable unreachable[1] The destination you are calling is currently unreachable.
    #variable unreachable[2] The Mob you have called has turned on their nolocate flag.
    #variable unreachable[3] Unreachable, check your order! Mob might be dead though.
    #variable unreachable[4] Are you sure your order was ok? Recheck it please.
    #variable unreachable[5] Noone's picking up on the other side. What if .. someone... |BW|murdered|N| him?
    #variable unreachable[6] He's dead, Jim.
    #variable unreachable[7] Noone's there! Should we wait?
    #variable unreachable[8] Nonsense. Dead or dying is not a good travel destination.
    #variable unreachable[9] No, that imp is unreachable.
    #variable unreachable[10] - Knock-knock.     - Who's there?    - NOONE.

    #variable spellbot undefined;
    #variable mana undefined;
    #variable manafull undefined;
    #variable summoner undefined;

    #nop ------------------------------------------
    #nop ------------- OPERATION ------------------
    #nop ------------------------------------------
    #alias {spellboton}{
            #variable spellbot 1;
            title is |R|spell|BW|BOT|N|, [|P|pinfo|N|]  |BW|-:SMG:-;
            stand;
            vis;
            sleep;
            config -prompt2;
            infoset -all;
            infoset +buddy;
            channel -grtz;
            tag set bot Botting Service Available;
    }
    #alias {spellbotoff}{
            #variable spellbot 0;
            title is |BG|NOT |N|botting atm.           |BW|--:SMG:--;
            stand;
            config +prompt2;
            channel +all;
            infoset +all;
            tag remove bot;
    }

    #action {^%1 tells you '%2'$} {
            #if {"$spellbot" == "1"}{
                    #math var_bestwishes 1d100;
                    #math var_spellupdone 1d20;
                    #math var_unreachable 1d10;
                    #showme INCOMING TELL. TELLER:---%1---|ORDER:---%2---|;
            }
    }

    #action {^%iYou dream of %1 telling you '%2'$} {
            #if {"$spellbot" == "1"}{
                    #math var_bestwishes 1d100;
                    #math var_spellupdone 1d20;
                    #math var_unreachable 1d10;
                    #showme INCOMING TELL. TELLER:---%1---|ORDER:---%2---|;
            }
    }

    #function gettime {#format result %t %H:%M}


    #nop -----------------------------------------
    #nop ------------- MANA TRACKER --------------
    #nop -----------------------------------------

    #action {(%1/%2)(%3/%4 s:%5 a:%6)} {
            #if {"$spellbot" == "1"}{
                    #variable mana %3;
                    #variable manafull %4;
                    #showme @gettime{}, $mana/$manafull mana.;
            }
    }

    #nop ------------------------------------------
    #nop ----------- DISPATCHER / BOT -------------
    #nop ------------------------------------------

    #nop ----------- COMM -------------------

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---help%2---|$} {
            #variable teller %1;
            tell $teller It's elementary, my dear $teller. |BW|playerinfo Teacup|N| will explain everything.;
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---than%2---|$} {
            #variable teller %1;
            tell $teller You're welcome.;
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---nothin%2---|$} {
            #variable teller %1;
            tell $teller Wanna bet?;
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---righ%2---|$} {
            #variable teller %1;
            tell $teller Right.;
    }
    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---report%2---|$} {
            stand;
            report mana;
            sleep;
    }
    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---reset%2---|$} {
            wake;
            c 'planeshift' mid;
            sanctum;
            #variable $summoner Teacup;
    }

    #nop ----------- PORTALBOT -----------------

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---pseal%2---|$} {
            #if {$mana >= 27} {
                    stand;
                    tell %1 I seal portals and nexuses by default if present when opening new ones. Sealing anyway.;
                    cast seal portal;
                    sleep;
            };
            #else {tell %1 Appologies slavedriver, I lack mana to do this. Mana - $mana/$manafull. Need to be at 24 for seal!;}
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---nseal%2---|$} {
            #if {$mana >= 27} {
                    tell %1 I seal portals and nexuses by default if present when opening new ones. Sealing anyway.;
                    stand;
                    cast seal nexus;
                    sleep;
            };
            #else {tell %1 Appologies, slavedriver, I lack mana to do this. Mana - $mana/$manafull. Need to be at 24 for seal!;}
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---pp %2---|$} {
            #variable teller %1;
            #variable destination %2;
            #if {$manafull > 10000} {tell $teller sorry, I don't do portals when on Thorngate. Call me to infirmary with |BW|tell Teacup mid|N|.;};
            #else {
                    #if {$mana >= 55} {
                            stand;
                            cast portal $destination;
                    };
                    #else {tell $teller Appologies, slavedriver, I lack mana to do this. Mana - $mana/$manafull. Need to be at 49 for portal!;};
            };
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---port%3 %2---|$} {
            #variable teller %1;
            #variable destination %2;
            #if {$manafull > 10000} {tell $teller sorry, I don't do portals when on Thorngate. Call me to infirmary with |BW|tell Teacup mid|N|.;};
            #else {
                    #if {$mana >= 55} {
                            stand;
                            cast portal $destination;
                    };
                    #else {tell $teller Appologies, slavedriver, I lack mana to do this. Mana - $mana/$manafull. Need to be at 49 for portal!;};
            };
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---nex %2---|$} {
            #variable teller %1;
            #variable destination %2;
            #if {$manafull > 10000} {tell $teller sorry, I don't nexus when on Thorngate. Call me to infirmary with |BW|tell Teacup mid|N|.;};
            #else {
                    #if {$mana >= 74} {
                            #if {"$teller" == "%i$destination"} {tell $teller Opening a gate to you now, stand by.;};
                            stand;
                            cast nexus $destination;
                    };
                    #else {tell $teller Appologies, slavedriver, I lack mana to do this. Mana - $mana/$manafull. Need to be at 66 for nexus!;};
            };
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---nexus %2---|$} {
            #variable teller %1;
            #variable destination %2;
            #if {$manafull > 10000} {tell $teller sorry, I don't nexus when on Thorngate. Call me to infirmary with |BW|tell Teacup mid|N|.;};
            #else {
                    #if {$mana >= 74} {
                            #if {"$teller" == "%i$destination"} {tell $teller Opening a gate to you now, stand by.;};
                            stand;
                            cast nexus $destination;
                    };
                    #else {tell $teller Appologies, slavedriver, I lack mana to do this. Mana - $mana/$manafull. Need to be at 66 for nexus!;};
            };
    }

    #action {^You punch a hole through the fabric of space, and a portal appears!$}{
            #if {"$spellbot" == "1"}{
                    tell $teller The portal to $destination is up. $bestwishes[$var_bestwishes];
                    sleep;
            }
    }

    #action {^Your magic renews the portal in a flash of brilliant white!$}{
            #if {"$spellbot" == "1"}{
                    tell $teller The portal to $destination is up. $bestwishes[$var_bestwishes];
                    sleep;
            }
    }

    #action {^You force open a nexus in the planes!$}{
            #if {"$spellbot" == "1"}{
                    #if {"$teller" == "%i$destination"} {tell $teller Come home safely.;};
                    #else {tell $teller The nexus to $destination is up. $bestwishes[$var_bestwishes];};
                    sleep;
            }
    }

    #action {^The nexus sparkles brightly as its energy is renewed by your spell!$}{
            #if {"$spellbot" == "1"}{
                    #if {"$teller" == "%i$destination"} {tell $teller Come home safely.;};
                    #else {tell $teller The nexus to $destination is up. $bestwishes[$var_bestwishes];};
                    sleep;
            }
    }

    #action {^In a fury of energy, you force the nexus to rechannel to a new destination!$}{
            #if {"$spellbot" == "1"}{
                    #if {"$teller" == "%i$destination"} {tell $teller Come home safely.;};
                    #else {tell $teller The nexus to $destination is up. $bestwishes[$var_bestwishes];};
                    sleep;
            }
    }

    #action {^Your spell fizzles.$}{
            #if {"$spellbot" == "1"}{
                    tell $teller $unreachable[$var_unreachable];
                    sleep;
            }
    }

    #action {^Your spell fizzled.$}{
            #if {"$spellbot" == "1"}{
                    #if {"$teller" == "%i$destination"} {tell $teller Can't bring you home, get out of the cursed room!;};
                    #else {tell $teller $unreachable[$var_unreachable];};
                    sleep;
            }
    }

    #action {^You were not able to overcome the magic of the first portal!$}{
            #if {"$spellbot" == "1"}{
                    cast seal portal;
                    cast portal $destination;
            }
    }

    #action {^The vertices of power seem too strong for you to rematrix!$}{
            #if {"$spellbot" == "1"}{
                    cast seal nexus;
                    cast nexus $destination;
            }
    }


    #nop --------------- SPELLS -----------------

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---water%2---|$} {
             #if {$mana >= 14} {
                    stand;
                    cast 'water breathing' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 14 for water breathing! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---hs%2---|$} {
             #if {$mana >= 19} {
                    stand;
                    cast 'holy sight' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 19 for holy sight! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---bark%2---|$} {
                    tell %1 Do I look like a druid? A green tree-hugger? |BW|Look |N|at me!;
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---invinc%2---|$} {
             #if {$mana >= 95} {
                    stand;
                    cast 'invincibility' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 95 for invincibility! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---foci%2---|$} {
            #if {$mana >= 372} {
                    stand;
                    cast 'foci' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 372 for foci! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---awen%2---|$} {
             #if {$mana >= 475} {
                     stand;
                     cast 'awen' %1;
                     sleep;
             };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 475 for awen! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---fort%2---|$} {
             #if {$mana >= 380} {
                     stand;
                     cast 'fortitudes' %1;
                     sleep;
             };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 380 for fortitudes! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---as%2---|$} {
            tell %1 Do i |BW|look |N|like a mage to you? do you see me hanging in some ivory tower?;
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---bless%2---|$} {
         #if {$mana >= 4} {
                    stand;
                    cast 'bless' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 4 for bless! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---dark%2---|$} {
         #if {$mana >= 14} {
                    stand;
                    cast 'dark embrace' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 14 for dark embrace! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---div1---|$} {
            #if {$mana >= 95} {
                    stand;
                    cast 'div' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 95 for divinity! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---div2---|$} {
            #if {$mana >= 285} {
                    stand;
                    augment 2;
                    cast 'div' %1;
                    augment off;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 285 for a2 divinity! Request a lower one.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---div---|$} {
            #if {$mana >= 570} {
                    stand;
                    augment 3;
                    cast 'div' %1;
                    augment off;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 570 for a3 divinity! Request a lower one.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---div3---|$} {
            #if {$mana >= 570} {
                    stand;
                    augment 3;
                    tell %1 I do div 3 by default, no need to request it.;
                    cast 'div' %1;
                    augment off;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 570 for a3 divinity! Request a lower one.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---div4---|$} {
            #if {$mana >= 950} {
                    stand;
                    augment 4;
                    cast 'div' %1;
                    augment off;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 950 for a4 divinity! Request a lower one.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---div5---|$} {
            #if {$mana >= 1425} {
                    stand;
                    augment 5;
                    cast 'div' %1;
                    augment off;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 1425 for a5 divinity! Request a lower one.;};
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---comf1---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 237} {
                            stand;
                            cast 'comfort' %1;
                            sleep;
                    };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 237 for comfort! Try again later.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.};

    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---comf2---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 712} {
                            stand;
                            augment 2;
                            cast 'comfort' %1;
                            augment off;
                            sleep;
                    };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 712 for a2 comfort! Request a lower one.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.};
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---comf---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 1425} {
                            stand;
                            augment 3;
                            cast 'comfort' %1;
                            augment off;
                            sleep;
                    };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 1425 for a3 comfort! Request a lower one.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.};
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---comf3---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 1425} {
                            stand;
                            augment 3;
                            tell %1 I do comf 3 by default, no need to request it.;
                            cast 'comfort' %1;
                            augment off;
                            sleep;
                    };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 1425 for a3 comfort! Request a lower one.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.};
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---comf4---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 2375} {
                            stand;
                            augment 4;
                            cast 'comfort' %1;
                            augment off;
                            sleep;
                    };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 2375 for a4 comfort! Request a lower one.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.};
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---comf5---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 3562} {
                            stand;
                            augment 5;
                            cast 'comfort' %1;
                            augment off;
                            sleep;
                    };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 3562 for a5 comfort! Request a lower one.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.};
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---fren%2---|$} {
            #if {$mana >= 9} {
                    stand;
                    cast 'frenzy' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 9 for frenzy. Try again later.;}
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---heal%2---|$} {
            #if {$mana >= 47} {
                    stand;
                    cast 'heal' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 47 for heal. Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---invig%2---|$} {
            #if {$mana >= 256} {
                    stand;
                    augment 3;
                    cast 'invig' %1;
                    augment off;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 256 for a3 invigorate! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---fly%2---|$} {
            #if {$mana >= 9} {
                    stand;
                    cast 'fly' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 9 for fly! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---light%2---|$} {
            #if {$mana >= 6} {
                    stand;
                    cast 'magic light' %1;
                    tell %1 Careful now, take care.;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 6 for magic light! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---cp%2---|$} {
            #if {$mana >= 4} {
                   stand;
                   cast 'cure poison' %1;
                   sleep;
           };
           #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 4 for cure poison! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---regen%2---|$} {
            #if {$mana >= 142} {
                   stand;
                   cast 'regeneration' %1;
                   sleep;
           };
           #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 142 for regeneration! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---cd%2---|$} {
            #if {$mana >= 9} {
                    stand;
                    cast 'cure disease' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 9 for cure disease! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---rc%2---|$} {
            #if {$mana >= 4} {
                    stand;
                    cast 'remove curse' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 4 for remove curse! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---sanc%2---|$} {
            #if {$mana >= 71} {
                    stand;
                    cast 'sanctuary' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 71 for sanc! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---iron%2---|$} {
            #if {$mana >= 19} {
                    stand;
                    cast 'iron skin' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 19 for iron skin! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---steel%2---|$} {
            tell %1 You start by mixing iron with carbon, at approximately 98.2 to 1.8 ratio...;
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---invis%2---|$} {
            #if {$mana >= 4} {
                    stand;
                    cast 'invis' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 4 for invisibility! Try again later.;};
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---gs%2---|$} {
            #if {$mana >= 18} {
                    stand;
                    cast 'giant strength' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 18 for giant strength! Try again later.;};
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---thren %2---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 118} {
                            stand;
                            tell %1 Casting thren for %2.;
                            cast 'threnody' %2;
                            sleep;
                    };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 118 for threnody! Try again later.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.}
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---send %2 %3---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 237} {
                            stand;
                            gt %1 - I'm sending %2 to %3. Get ready.;
                            cast 'send' %2 %3;
                            gt %1 - following self regardless of success. Good hunting!;
                            follow self;
                            sleep;
                    };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 237 for send! Try again later.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.}
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---shri%2---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 2375} {
                            stand;
                            cast 'create shrine';
                            sleep;
                    };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 2375 for shrine! Try again later.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.}
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---req %2---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 475} {
                            stand;
                            tell %1 Casting requiem for %2.;
                            cast 'requiem' %2;
                            sleep;
                   };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 475 for send! Try again later.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.}
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---salv %2---|$} {
            #if {$manafull > 10000} {
                    #if {$mana >= 237} {
                            stand;
                            tell %1 Casting salvation for %2.;
                            cast 'salvation' %2;
                            sleep;
                    };
                    #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 237 for salvation! Try again later.;};
            };
            #else {tell %1 I'm on a wrong plane for that. Call me to Thorngate with |BW|tell Teacup thorn|N|.};
    };

    #action {^%1 beckons for you to follow %2.$} {
            #if {"$spellbot" == "1"} {
                    #if {$manafull > 10000} {
                            wake;
                            follow %1;
                            sleep;
                            gt Sending help has arrived. Usage: |P|tell Teacup send Pros Noct|N|.;
                    };
                    #else {tell %1 I do not follow people unless at Thorngate.;};
            };
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---thorn%2---|$} {
            #if {"$spellbot" == "1"} {
                    #if {$manafull < 10000} {
                            stand;
                            cast homeshift;
                            e;
                            e;
                            sleep;
                            tell %1 I have returned.;
                    };
                    #else {tell %1 Am I not at 2e? Did I get lost? Tell me |BW|reset|N| or |BW|mid|N| to go to infirmary and then |BW|home|N| to return to 2e.;};
            };
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---home%2---|$} {
            #if {"$spellbot" == "1"} {
                    #if {$manafull < 10000} {
                            stand;
                            cast homeshift;
                            e;
                            e;
                            sleep;
                            tell %1 I have returned.;
                    };
                    #else {tell %1 Am I not at 2e? Did I get lost? Tell me |BW|reset|N| or |BW|mid|N| to go to infirmary and then |BW|home|N| to return to 2e.;};
            };
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---mid%2---|$} {
            #variable summoner %1;
            #if {$manafull > 10000} {
                    #if {$mana >= 475} {
                            stand;
                            cast 'planeshift' mid;
                            sanc;
                    };
                    #else {tell $summoner Not enough mana - $mana/$manafull. Need to be at 475 for midshift! Try again later.;};
            };
            #else {tell $summoner But I AM at Midgaardia. If I'm not at the infirmary, tell me |BW|reset|N|.;};
    };
    #action {^The Sanctum$} {
            #if {"$spellbot" == "1"} {
                    stand;
                    d;
                    w;
                    sleep;
                    tell $summoner I have shifted to infirmary.;
            };
    };

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---full%2---|$} {
            #if {$mana >= 1374} {
                    tell %1 Seek barkskin and steel elsewhere. Cast concentration. While waiting, why not go and vote: <insert vote url here, cut due to wiki issues>;
                    stand;
                    cast 'holy sight' %1;
                    cast 'foci' %1;
                    cast 'fortitude' %1;
                    cast 'water breathing' %1;
                    cast 'iron skin' %1;
                    cast 'invincibility' %1;
                    cast 'dark embrace' %1;
                    tell %1 $spellupdone[$var_spellupdone];
                    cast 'awen' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 1374 for full spellup! Try again later.;};
    }

    #action {^%iINCOMING TELL. TELLER:---%1---|ORDER:---split%2---|$} {
            #if {$mana >= 1044} {
                    tell %1 Seek barkskin and steel elsewhere. Cast concentration. In the meantime go vote: <insert vote url here, cut due to wiki issues>;
                    stand;
                    cast 'holy sight' %1;
                    cast 'foci' %1;
                    cast 'fortitude' %1;
                    cast 'water breathing' %1;
                    cast 'iron skin' %1;
                    cast 'invincibility' %1;
                    cast 'armor' %1;
                    cast 'holy armor' %1;
                    cast 'holy aura' %1;
                    cast 'bless' %1;
                    cast 'dark embrace' %1;
                    tell %1 $spellupdone[$var_spellupdone];
                    cast 'sanctuary' %1;
                    sleep;
            };
            #else {tell %1 Not enough mana - $mana/$manafull. Need to be at 1044 for split spells! Try again later.;};
    }

## Usage

Usage is trivial:

Use **spellboton** and **spellbotoff** to toggle.

[Category: TinTin++ Scripting](Category:_TinTin++_Scripting "wikilink")
