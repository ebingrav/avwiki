This simplistic autofletcher script will hold new fletching kits when it
runs out, and supports lord fletching (mana regenning). Complicated
fletchings operating with fatigue are not supported.

Be sure to update the prompt section to suit your own prompt.

## Code

*Put the following into a file in your tintin++ folder and use **\#read
<filename>** while in tintin to load them up. Though you probably want
them to autoload with your character.*

    #variable fletch 0;
    #variable ftype explosive;
    #variable fammo bolts;
    #variable fail 0;

    #alias {fon}{#var fletch 1;#showme Autofletcher is on;stand;autofletch;};
    #alias {foff}{#var fletch 0;#showme autofletcher is off;};
    #alias {ftype}{#var ftype %1;#showme Type set to %1;};
    #alias {fammo}{#var fammo %1;#showme Ammo set to %1;};
    #alias {finfo}{#showme Fletch set to $ftype $fammo. Change with ftype <splinter> and fammo <bolts>.;};

    #function gettime {#format result %t %H:%M};

    #action {(%1/%2)(%3/%4 s:%5 q:%6)} {
            #if {"$fletch" == "1"}{
                    #variable mana %3;
                    #variable manafull %4;
                    #showme @gettime{}, $mana/$manafull mana.;
            }
    }

    #alias {autofletch}{
            #if {"$fletch" == "1"} {
                    fletch $fammo $ftype;
            };
            #else {#showme Autofletch is off. Doing nothing.;};
    }

    #action {^Your efforts produced}{#var fail 0;i;autofletch;};

    #action {^You fail to produce anything worth shooting.$}{
            #math {fail}{$fail+1};
            #showme Failed $fail times in a row.;
            #if {$fail == 3}{
                    #showme going to sleepy mode;
                    #ticker retrymuchlater {wake;#var fail 0;#unticker retrymuchlater;autofletch;} 1800;
                    sleep;
            };
            #else {autofletch;};
    };
    #action {^You lack the proper tools.$}{hold fle;autofletch;};
    #action {^You are not carrying a fle.}{#showme Not carrying tools.;foff;};

    #action {^You don't have enough mana to make %1 arrows.$}{
            #if {"$fletch" == "1"}{
                    sleep;
                    lag;
            };
    };

    #action {^Lagged as per your wishes!$}{
            #if {"$fletch" == "1"}{
                    #if {$mana == $manafull}{
                            stand;
                            fletch $fammo $ftype;
                    };
                    #else {look;lag;};
            };
    };

## Usage

Use **fon** and **foff** to toggle, **ftype** to set ammo style,
**fammo** to set ammo type, and **finfo** to remind yourself what you
were doing when you forget.

[Category: TinTin++ Scripting](Category:_TinTin++_Scripting "wikilink")
