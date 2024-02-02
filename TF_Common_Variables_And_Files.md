These are some common TF variable settings and library files to load
that are quite useful.

`/set refreshtime=100`  
`/set prompt_usec=100`  
`/set wrapsize=88`  
`/set wrapspace=2`  
`/histsize 100000`

  
;; allow space bar to continue page mode  
/require space-page.tf  
;; keybindings like those in bash (allows you to use up/down arrow keys
to navigate command history, for example)  
/require -q kb-bash.tf ;; let ctrl-D delete word to the right  
/bind ^D = /dokey DWORD  
/load -q alias.tf

`/load -q tools.tf `  
`/load -q speedwalk.tf `  
`/speedwalk `  
`/load -q tick.tf `  
`/ticksize 30`

;; visual mode off  
/visual off

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
