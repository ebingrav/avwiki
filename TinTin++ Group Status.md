## Basic Info

This is a script that allows you to keep track of group members' hit
points through all the spam that is present when fighting. It is most
useful for Lord runs or EHA as a healer. It takes the output from the
group command and uses netcat (I don't know if it would work with wintin
because of this) to send the information to another tintin++ instance
where it is parsed and color coded. The colour codes are as follows,
white for percent hitpoints above 75%, yellow 50%, pink 25%, red 0%.

## Screen Shot

This script being used in Mac OS X.  
<img src="GroupStat.png" title="GroupStat.png" width="600"
alt="GroupStat.png" />  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  

## Code

*Put this script in a file, i.e. groupstat.tt, and start up a tintin++
program with this loaded, ./tt++ groupstat.tt*

    #var MAXHP 0;
    #var ACTHP 0;
    #var PRCHP 0;
    #run nc_vitals {nc -l 9082};

    #act {%1 %d %d}
    {
        #math PRCHP {100 * %2 / %3};
        #if {$PRCHP >= 75} {#gts #showme <070>%1 %2/%3};
        #elseif {$PRCHP >= 50} {#gts #showme <030>%1 %2/%3};
        #elseif {$PRCHP >= 25} {#gts #showme <050>%1 %2/%3};
        #elseif {$PRCHP >= 0} {#gts #showme <010>%1 %2/%3};
    };
    #act {divider} {#gts #showme -------------------------------------};
    #gts;

*Put this script in your default avatar configuration which you load
when you start the instance of tintin++ you wish to play with: ./tt++
avatar.tt*

    #act {^%1|%2 %3 %w %s%w %s%d/%d %s%d/%d %s%d/%d} {#nc %4 %8 %9}; 
    #alias {gr} {#nc divider;#avatar group};
    #run nc {nc 127.0.0.1 9082};
    #avatar;

## Troubleshooting

You have to run the tintin++ client with groupstat.tt first as its
instance of netcat is the server, if you run your main config first its
instance of netcat will not be able to connect.  
This probably won't work with Wintin as it depends on tintin++ being
able to use netcat.  
It is possible that port 9082 is blocked by a firewall (though unlikely)
if this is the case change the port number in the lines with 9082 in
them.  

[Category:TinTin++ Scripting](Category:TinTin++_Scripting "wikilink")
