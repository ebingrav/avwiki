---
title: Cmud Lockbox Sub
permalink: wiki/Cmud_Lockbox_Sub/
layout: wiki
tags:
 -  Cmud Scripting
---

This script will make it easy to identify vials without having to cast
identify.

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger priority="3750" copy="yes">
        <pattern>(%*){a|an} %2 vial of healing</pattern>
        <value>#if (%lower( %2)="reddish-brown") {#var vialhealtype "cure crit"} {}
    #if (%lower( %2)="light brown") {#var vialhealtype "cure light"} {}
    #if (%lower( %2)="red-brown") {#var vialhealtype "divinity"} {}
    #if (%lower( %2)="orange-brown") {#var vialhealtype "cure serious"} {}
    #if (%lower( %2)="orange-red") {#var vialhealtype "cure crit+light"} {}
    #if (%lower( %2)="red striped") {#var vialhealtype "heal"} {}
    #if (%lower( %2)="yellow-brown") {#var vialhealtype "heal+cure light"} {}
    #if (%lower( %2)="yellow") {#var vialhealtype "heal+cure serious"} {}
    #if (%lower( %2)="yellow-orange") {#var vialhealtype "heal+cure crit"} {}
    #if (%lower( %2)="orange") {#var vialhealtype "heal+cure light+cure crit"} {}
    #psub {%2~(@vialhealtype~)} %x2
    #ad vialcount 1
    #var vcount %1
    #var vcount %remove((Magical),@vcount)
    #var vcount %remove((Humming),@vcount)
    #var vcount %remove((Glowing),@vcount)
    #var vcount %remove((Demonic),@vcount)
    #var vcount %replace(@vcount,"(","")
    #var vcount %replace(@vcount,")","")
    #var vcount %trim(@vcount)
    #if @vcount&gt;0 {#ad vialcount %eval(@vcount-1)} {}</value>
      </trigger>
    </cmud>
