Basic run counter I've used for years. It doesn't track damage verbs
etc, but could be extended to do that.

`;; Initial value, 0 in this case`  
`;;/set xp_total=0`  
`;;/set xp_gain=0`  
`;;/set xp_flee_loss=0`  
`;;/set xp_death_loss=0`  
`;;/set xp_kill_counter=0`  
`;;/set xp_flee_counter=0`  
`;;/set xp_death_counter=0`  
`;;/set gold_total=0`  
`;;/set gold_counter=0`  
  
`;; Reset macros`  
`/def run_reset = /xp_reset %; /gold_reset`  
`/def -i xp_reset = /set xp_total=0 %; /set xp_kill_counter=0 %; /set xp_gain=0 %; /set xp_death_counter=0 %; /set xp_death_loss=0 %; /set xp_flee_counter=0 %; /set xp_flee_loss=0`  
`/def -i gold_reset = /set gold_total=0 %; /set gold_counter=0`  
`/def runon = /set run_on=true`  
`/def runoff = /set run_on=false`  
`/if (run_on == true)`  
`;; Experience counter`  
`/def -i -t'You receive * experience points.' xp_counter_macro = \`  
` /set xp_gain=$[xp_gain + {3}] %;\`  
` /set xp_kill_counter=$[xp_kill_counter + 1] %; \`

`;; Death counter`  
`/def -i -t'Death sucks * experience.' xp_death_macro = \`  
` /set xp_death_loss=$[xp_death_loss - {3}] %;\`  
` /set xp_death_counter=$[xp_death_counter + 1] %; \`

`;; Flee checker`  
`/def -i -t'You lose * experience points.' flee_loss_macro = \`  
` /set xp_flee_loss=$[xp_flee_loss - {3}] %; \`  
` /set xp_flee_counter=$[xp_flee_counter + 1] %; \`

`;; Manually subtract XP.`  
`/def -i sub_xp = /set xp_total=$[xp_total - {1}] %; \`  
`       /echo XP Total set to $[xp_total]`

`;; Manually add XP.`  
`/def -i add_xp = /set xp_total=$[xp_total + {1}] %; \`  
`       /echo XP Total set to $[xp_total]`

`;; Gold counter`  
`/def -i -mregexp -t'Your share is *' coin_counter_macro = \`  
` /set gold_total=$[gold_total + {9}] %;\`  
` /set gold_counter=$[gold_counter + 1]`

`/endif`  
  
`/def -i run_stats = /set xp_total=$[xp_gain - xp_death_loss - xp_flee_loss] %; \`  
`        /echo *********************** %; \`  
`        /echo * Run totals %; \`  
`                /echo *********************** %; \`  
`                /echo * Total XP Gained:  $[xp_total] %; \`  
`                /echo * Kill XP Gained:   $[xp_gain] %; \`  
`                /echo * Death Losses:     $[xp_death_loss] %; \`  
`                /echo * Flee Losses:      $[xp_flee_loss] %; \`  
`                /echo * Mobs killed:      $[xp_kill_counter] %; \`  
`                /echo * Avg XP per kill:  $[xp_total/xp_kill_counter] %; \`  
`                /echo * ----- %; \`  
`                /echo * Gold collected:   $[gold_total] %; \`  
`                /echo ***********************`

`/def -i gt_stats = /set xp_total=$[xp_gain - xp_death_loss - xp_flee_loss] %;\`  
` gtell Run XP: $[xp_total], Gained XP: $[xp_gain], Kills: $[xp_kill_counter], Avg: $[xp_total/xp_kill_counter].`

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
