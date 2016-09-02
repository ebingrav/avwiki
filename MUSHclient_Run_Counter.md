---
title: MUSHclient Run Counter
permalink: wiki/MUSHclient_Run_Counter/
layout: wiki
tags:
 -  MUSHclient Scripting
---

Below is a rough damage/exp counter for MUSHclient. It requires some
triggers, and Lua script to work.

How to Use It
-------------

To install simply copy the aliases/triggers below and paste them into
the proper place in your world (aliases in the "alias" section, triggers
in the "trigger"). Then, add the script to the world's script file.  
  
To populate the damage counter: **counter populate**  
To reset the damage counter: **counter reset**  
To show the counter (but not report it to the group): **counter show**  
To report the counter stats to the group: **counter report**  

Aliases
-------

    <aliases>
      <alias
       match="counter *"
       enabled="y"
       send_to="12"
       sequence="100"
      >
      <send>counterCommand("%1")</send>
      </alias>
    </aliases>

Triggers
--------

    <triggers>
      <trigger
       enabled="y"
       group="Counters"
       keep_evaluating="y"
       match="*"
       send_to="12"
       sequence="1"
      >
      <send>findDamage("%1")</send>
      </trigger>
      <trigger
       enabled="y"
       group="Counters"
       keep_evaluating="y"
       match="You receive * experience points."
       send_to="12"
       sequence="1"
      >
      <send>addExp("%1")</send>
      </trigger>
      <trigger
       group="Counters"
       match="*|*"
       name="GroupGrab"
       send_to="12"
       sequence="100"
      >
      <send>local nCheck = tonumber("%1")
    local p = ""
    local s = " " .. "%2"

    if nCheck == nil then return end
    p = string.sub(s, string.find(s, " ") + 1)
    p = string.sub(p, string.find(p, " ") + 1)
    p = string.sub(p, string.find(p, " ") + 1)
    if string.sub(p, 0, string.find(p, " ") - 1) == "Hero" or string.sub(p, 0, string.find(p, " ") - 1) == "Lord" then
     p = string.sub(p, string.find(p, " ") + 1)
    end
    if tonumber(string.sub(p, 0, string.find(p, " ") - 1)) ~= nil then
    p = string.sub(p, string.find(p, " ") + 1)
    p = string.sub(p, string.find(p, " ") + 1)
    end
    p = string.sub(p, 0, string.find(p, " ") - 1)
    counterCommand("add " .. p)</send>
      </trigger>
      <trigger
       enabled="y"
       group="Counters"
       match="Welcome back to the AVATAR System *, *"
       send_to="12"
       sequence="100"
      >
      <send>world.SetVariable("cPlayer", "%1")
    counterCommand("reset")</send>
      </trigger>
      <trigger
       enabled="y"
       group="Counters"
       match="Welcome back to the AVATAR System, * *."
       send_to="12"
       sequence="100"
      >
      <send>world.SetVariable("cPlayer", "%2")
    counterCommand("reset")</send>
      </trigger>
    </triggers>

World Variables
---------------

    <variables>
      <variable name="cPlayer">PLAYERNAMEGETSET</variable>
    </variables>

The Script
----------

The following script is Lua and can be set in your world's main script
file. If put into a secondary file, please be sure to require it!  

    local dvalues = {"0","2","4","8","10","14","18","22","26","30","34","38","42","46","49","55","60","65","70","75","80","85","90","95","100","110","120","130","140","150","160","170","180","190","200","225","250","275","300","325","350","375","400","425","450","475","500","540","574","606","675","730","769","810","884","915","1000","1100","1200","1300","1400","1500","1600","1700","1800","1900","2000","2200","2400","2600","2800","3000","3200","3400","3600","3800","4100","4500","5007","5901","5902","6200","6500","7000","7500","7800","8200","8500","9000","9500","10000","11000","12000","13000","14000","15000","16500","18000","19000","20000","21000","22000","23000","24000","25000","26000","27000","28000","29000","30000","31000","32000","33000","34000","35000","36000","37000","38000","39000","40000","41000","42000","43000","44500","47000","48000","50000","51000","53000","55000","57000","59000","61000","65000","70000","75000","80000","100000","0"}
    local dverbs = {"nil","pathetic","weak","punishing","surprising","amazing","astonishing","mauling","MAULING","MAULING*","MAULING**","MAULING***","decimating","DECIMATING","DECIMATING*","DECIMATING**","DECIMATING***","devastating","DEVASTATING","DEVASTATING*","DEVASTATING**","DEVASTATING***","pulverizing","PULVERIZING","PULVERIZING*","PULVERIZING**","PULVERIZING***","maiming","MAIMING","MAIMING*","MAIMING**","MAIMING***","eviscerating","EVISCERATING","EVISCERATING*","EVISCERATING**","EVISCERATING***","mutilating","MUTILATING","MUTILATING*","MUTILATING**","MUTILATING***","disemboweling","DISEMBOWELING","DISEMBOWELING*","DISEMBOWELING**","DISEMBOWELING***","dismembering","DISMEMBERING","DISMEMBERING*","DISMEMBERING**","DISMEMBERING***","massacring","MASSACRING","MASSACRING*","MASSACRING**","MASSACRING***","mangling","MANGLING","MANGLING*","MANGLING**","MANGLING***","demolishing","DEMOLISHING","DEMOLISHING*","DEMOLISHING**","DEMOLISHING***","obliterating","OBLITERATING","OBLITERATING*","OBLITERATING**","OBLITERATING***","annihilating","ANNIHILATING","ANNIHILATING*","ANNIHILATING**","ANNIHILATING***","ANNIHILATING***<","ANNIHILATING***<<","ANNIHILATING***<<<","ANNIHILATING***<<<<","eradicating","ERADICATING","ERADICATING*","ERADICATING**","ERADICATING***","ERADICATING***<","ERADICATING***<<","ERADICATING***<<<","ERADICATING***<<<<","vaporizing","VAPORIZING","VAPORIZING*","VAPORIZING**","VAPORIZING***","VAPORIZING***<","VAPORIZING***<<","VAPORIZING***<<<","VAPORIZING***<<<<","destructive","DESTRUCTIVE","DESTRUCTIVE*","DESTRUCTIVE**","DESTRUCTIVE***","DESTRUCTIVE****","DESTRUCTIVE****<","DESTRUCTIVE****<<","DESTRUCTIVE****<<<","DESTRUCTIVE****<<<<","DESTRUCTIVE***<<<<=","DESTRUCTIVE**<<<<==","DESTRUCTIVE*<<<<===","DESTRUCTIVE<<<<====","extreme","EXTREME","EXTREME*","EXTREME**","EXTREME***","EXTREME****","EXTREME****<","EXTREME****<<","EXTREME****<<<","EXTREME****<<<<","EXTREME***<<<<=","EXTREME**<<<<==","EXTREME*<<<<===","EXTREME<<<<====","porcine","PORCINE","PORCINE*","PORCINE**","PORCINE***","PORCINE***<","PORCINE***<<","PORCINE***<<<","PORCINE***<<<<","divine","daunting","terminal"}

    local damList = {}
    local expEarned = 0
    local expKills = 0

    function setup()
        world.ColourNote("red", "white", "Counters have been reset, and setup!")
        addTable("You")
    end

    function addExp(value)
        expEarned = expEarned + tonumber(value)
        expKills = expKills + 1
    end

    function addTable(name)
        for i = 1, table.getn(damList) do
            if damList[i] == name then
                world.ColourNote("red", "white", damList[i] .. " is already on the list!")
                return
            end
        end
        
        local buf = {name, 0}
        damList[table.getn(damList)+1] = buf
        world.ColourNote("red", "white", name .. " has been added!")    
    end

    function addDamage(person, verb)
        local dam = 0   
        
        for i = 1, table.getn(dverbs) do
            if string.find(verb, dverbs[i]) ~= nil then
                dam = tonumber(dvalues[i])
            end
        end
        
        for i = 1, table.getn(damList) do
            if damList[i][1] == person then
                damList[i][2] = damList[i][2] + dam
            end
        end
    end

    function findDamage(line)
        for i = 1, table.getn(dverbs) do
            if string.find(line, dverbs[i]) ~= nil then
                local fWord = string.sub(line, 0, string.find(line, " ") - 1)
            
                for p = 1, table.getn(damList) do
                    if string.find(string.upper(fWord), string.upper(damList[p][1])) ~= nil then
                        local sFind = string.find(line, dverbs[i])
                        local dVerb = string.sub(line, sFind, string.find(line, " ", sFind))    
                        addDamage(damList[p][1], dVerb)
                        return
                    end
                end
            end
        end
    end

    function counterCommand(args)
        local arg1, arg2 = ""
        if string.find(args, " ") ~= nil then
            arg1 = string.sub(args, 0, string.find(args, " ") - 1)
            arg2 = string.sub(args, string.find(args, " ") + 1)
        else
            arg1 = args
            arg2 = ""
        end
        
        if arg1 == "add" then
            if arg2 == world.GetVariable("cPlayer") then
                addTable("You")
            else
                addTable(arg2)      
            end
        end
        
        if arg1 == "clear" or arg1 == "reset" then
            damList = {}
            expEarned = 0
            expKills = 0        
        end
        
        if arg1 == "show" then
            world.Note("Monitoring:")
            for i = 1, table.getn(damList) do           
                world.Note(damList[i][1] .. " : " .. damList[i][2])
            end
        end
        
        if arg1 == "populate" then      
            world.EnableTrigger("GroupGrab", true)
            world.Send("group")
            DoAfterSpecial (5, 'EnableTrigger ("GroupGrab", 0)', sendto.script)
        end
        
        if arg1 == "report" then
            local tDamage = 0
            local buf = ""
            
            for i = 1, table.getn(damList) do
                tDamage = tDamage + damList[i][2]   
                if damList[i][1] == "You" then
                    buf = buf .. "[|Y|" .. world.GetVariable("cPlayer") .. " |W|- |BR|" .. tostring(damList[i][2]) .. "|N|]"            
                else
                    buf = buf .. "[|Y|" .. damList[i][1] .. " |W|- |BR|" .. tostring(damList[i][2]) .. "|N|]"           
                end
            end
            world.Send("gt [Total Exp |BP|" .. expEarned .. "|N|] [Kills |R|" .. expKills .. "|N|] [Avg XP |P|" .. (expEarned / expKills) .. "|N|]")
            world.Send("gt Damage Recorded: |BW|Total|N| - |BP|" .. tostring(tDamage) .. "|N| " .. buf)
        end
    end

    setup()

Designer comments
-----------------

The **counter populate** has not been tested on low morts or lords. It
should work, in theory.  
Also, sorry for the horrible code. This was never meant to see the light
of day (IE: Wrote it for myself), but was forced to share it by a
friend!
