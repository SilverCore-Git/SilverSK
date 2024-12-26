# SivlerSK
Projet de scripts réaliser avec le plugin minecraft [skript](https://skript-mc.fr/) et l'addon [SkQuery](https://www.spigotmc.org/resources/skquery-1-13-1-19.36631/) !

---
# Projet en dévelopement voici des maquettes :
## Table des matieres :
- [config.sk](#config-sk)
- [onjoin.sk](#onjoin-sk)
- [inspect.sk](#inspect-sk)
- [topluck.sk](#topluck-sk)
- [anticheat.sk](anticheat-sk)


## config sk :
```
#
# @copiryght = 2024 SilverCore
# @autor = Silverdium
# @autor = MisterPapaye
#

on load:
    send "Loading config with ./plugins/skript/scripts/config.sk" to console

    set {permmessage} to "%{prefix::dium}% &cVous n'avez pas accès à cette commande... (dommage dommage...)"
    set {used} to "Utilisation :"

    set {preprefix} to "&8[&l&bSilver"
    set {prefix::chat} to "%{preprefix}%&8-&6chat&8]» &f"
    set {prefix::dium} to "&8[&6Silverdium&8]» &f"
    set {prefix::cheat} to "&8[&cSilver-AntiCheat&8]> &f"
    set {prefix::inspect} to "&8[&4Silver-Inspect&8] &7"
    set {prefix::join} to "&8[&6Silver-join&8]> &7"
    set {prefix::staff} to "&8[&6Silver-stafftools&8] &7"


command /config [<text>]:
    permission: skript.staff.config
    permission message: %{permmessage}%
    trigger:
        if arg-1 is "reload":
            send "&8[&6Silverdium&8]» &fRechargement de la config..." to player

            set {permmessage} to "%{prefix::dium}% &cVous n'avez pas accès à cette commande... (dommage dommage...)"
            set {used} to "Utilisation :"

            set {preprefix} to "&8[&l&bSilver"
            set {prefix::chat} to "%{preprefix}%&8-&6chat&8]» &f"
            set {prefix::dium} to "&8[&6Silverdium&8]» &f"
            set {prefix::cheat} to "&8[&cSilver-AntiCheat&8]> &f"
            set {prefix::inspect} to "&8[&4Silver-Inspect&8] &7"
            set {prefix::join} to "&8[&6Silver-join&8]> &7"
            set {prefix::staff} to "&8[&6Silver-stafftools&8] &7"

            send "&8[&6Silverdium&8]» &fConfig rechargée !!" to player
        else:
            send "{@prefix}&cUsage : /config <target:reload>" to player

#end
```
## onjoin sk :
```
#
# @copiryght = 2024 SilverCore
# @autor = Silverdium
# @autor = MisterPapaye
#

options:
    prefix: %{prefix::join}%

on load:
    send "Loading skript ./plugins/skript/scripts/onjoin.sk" to console

on first join:
    send "{@prefix} Bienvenue sur silverdium !!" to player


on join:
    set {player_luckinfo::%player%} to 0
    set {diamond_mined::%player%} to 0 
    set {blocks_mined::%player%} to 0
    set {_percent::%player%} to 0
    set {vanish::%player%} to false
    
    if player's name is "MisterLyle" or "MisterPapaye":
        broadcast "{@prefix} &eLe fondateur &b%player% &evient de se connecter !"
    else:
        send "{@prefix} Salut, &b%player%! &7Bonne game sur Silverdium !" to player
        broadcast "{@prefix} &e%player% vient de se connecter !"

#end
```
## inspect sk :
```
#
# @copiryght = 2024 SilverCore
# @autor = Silverdium
# @autor = MisterPapaye
#

options:
    prefix: %{prefix::inspect}%

on load:
    send "Loading skript ./plugins/skript/scripts/inspect.sk" to console

#   Get Data
every 1 minute:
    loop all players:
        add 1 to {timeingamemn::%loop-player%}
        if {timeingamemn::%loop-player%} = 60:
            add 1 to {timeingameh::%loop-player%}
            set {timeingamemn::%loop-player%} to 0


on first join:
    set {join-date::%player%} to now

on join:
    set {join-time::%player%} to time

on chunk load:
    loop all players:
        if loop-player is online:
            add 1 to {chunkloaded::%loop-player%}

on chat:
    add 1 to {chatnumber::%player%}

on kick:
    add 1 to {kickcount::%player%}

on death of player:
    add 1 to {deathcount::%victim%}

# /inspect commande
command /inspect [<player>] [<text>]:
    permission: skript.staff.inspect
    permission message: %{permmessage}%
    trigger:
        set {player} to arg-1
        set {s?} to arg-2
        if arg-1 is set:
            if arg-2 is "s":
                send "{@prefix}&l||&l=========={ &4%player%&7 &l}==========" to player
                if {join-date::%arg-1%} is set:
                    send "{@prefix}&l|| - &bFirst join &f: &e%{join-date::%arg-1%}%" to player
                else:
                    send "{@prefix}&l|| - &bFirst join &f: &cAucune donnée disponible." to player

                set {_percent::%player%} to ({diamond_mined::%player%} * 100) / {blocks_mined::%player%}
                send "{@prefix}&l|| - &bTopLuck &f: &c%{_percent::%player%}%&7 /100." to player

                if {kickcount::%player%} is set:
                    send "{@prefix}&l|| - &bKick &f: &c%{kickcount::%player%}%&7." to player
                else:
                    send "{@prefix}&l|| - &bKick &f: &aAucun kick pour le moment (gg)." to player

                if {suspect::%player%} is set:
                    send "{@prefix}&l|| - &bSuspicion &f: &c%{suspect::%player%}%&7 points." to player
                else:
                    send "{@prefix}&l|| - &bSuspicion  &f: &aAucun point de suspicion." to player
                send "{@prefix}&l||&l================================" to player
            else:
                set {_join-time} to {join-time::%player%}
                set {_connection-duration} to difference between now and {_join-time}
                set {_hours} to {_connection-duration} / 3600
                set {_minutes} to ({_connection-duration} / 3600) / 60
                set {_seconds} to {_connection-duration} / 60
                send "{@prefix}&l||&l=========={ &4%player%&7 &l}==========" to player
                if {join-date::%arg-1%} is set:
                    send "{@prefix}&l|| - &bFirst join &f: &e%{join-date::%arg-1%}%" to player
                else:
                    send "{@prefix}&l|| - &bFirst join &f: &cAucune donnée disponible." to player

                send "{@prefix}&l|| - &bJoin time &f: &e%{_hours}%:%{_minutes}%:%{_seconds}%." to player

                if {timeingameh::%player%} is set:
                    send "{@prefix}&l|| - &bTime in game &f: &c%{timeingameh::%player%}%:%{timeingamemn::%player%}%&7." to player
                else:
                    set {timeingameh::%player%} to 0
                    send "{@prefix}&l|| - &bTime in game &f: &c%{timeingameh::%player%}%:%{timeingamemn::%player%}%&7." to player

                if {chunkloaded::%player%} is set:
                    send "{@prefix}&l|| - &bChunk loaded &f: &c%{chunkloaded::%player%}%&7." to player
                else:
                    send "{@prefix}&l|| - &bChunk loaded &f: &aAucun Chunk charger." to player
                
                if {chatnumber::%player%} is set:
                    send "{@prefix}&l|| - &bPosted message &f: &c%{chatnumber::%player%}%&7." to player
                else:
                    send "{@prefix}&l|| - &bPosted message &f: &aAucun message envoyer." to player

                set {_percent::%player%} to ({diamond_mined::%player%} * 100) / {blocks_mined::%player%}
                send "{@prefix}&l|| - &bTopLuck &f: &c%{_percent::%player%}%&7 /100." to player

                if {deathcount::%player%} is set:
                    send "{@prefix}&l|| - &bDeath &f: &c%{deathcount::%player%}%&7." to player
                else:
                    send "{@prefix}&l|| - &bDeath &f: &aAucune morts pour le moment (quelle géni)." to player

                if {kickcount::%player%} is set:
                    send "{@prefix}&l|| - &bKick &f: &c%{kickcount::%player%}%&7." to player
                else:
                    send "{@prefix}&l|| - &bKick &f: &aAucun kick pour le moment (gg)." to player

                if {suspect::%player%} is set:
                    send "{@prefix}&l|| - &bSuspicion &f: &c%{suspect::%player%}%&7 points." to player
                else:
                    send "{@prefix}&l|| - &bSuspicion  &f: &aAucun point de suspicion." to player
                send "{@prefix}&l||&l================================" to player
        else:
            send "{@prefix}&cUsage : /inspect <player>" to player

#end
```
## topluck sk :
```
#
# @copiryght = 2024 SilverCore
# @autor = Silverdium
# @autor = MisterPapaye
#

on load:
    send "Loading skript ./plugins/skript/scripts/topluck.sk" to console

on break:
    add 1 to {blocks_mined::%player%}
    if event-block is diamond ore:
        add 1 to {diamond_mined::%player%} 
    if event-block is deepslate diamond ore:
        add 1 to {diamond_mined::%player%} 


command /topluck [<player>]:
    permission: skript.staff.topluck
    permission message: %{permmessage}%
    trigger:
        if arg-1 is set:
            if {blocks_mined::%arg-1%} > 0:
                set {_percent::%arg-1%} to ({diamond_mined::%arg-1%} * 100) / {blocks_mined::%arg-1%}
                send "%{prefix::staff}% TopLuck | %arg-1% = &5%{_percent::%arg-1%}% &7/100" to player
            else:
                send "%{prefix::staff}% TopLuck | %arg-1% = &50 &7/100" to player
        else:
            wait 3 ticks
            open chest with 6 rows named "&8[&4Top Luck&8]" to player
            set {_slot} to 0
            loop all players:
                set {_head} to skull of loop-player
                set name of {_head} to loop-player's name  
                if {blocks_mined::%loop-player%} > 0: 
                    set {_percent::%loop-player%} to ({diamond_mined::%loop-player%} * 100) / {blocks_mined::%loop-player%}
                    set lore of {_head} to "&5%{_percent::%loop-player%}% /100" 
                else:
                    set lore of {_head} to "&40 /100" 
                format slot {_slot} of player with {_head} to be unstealable 
                add 1 to {_slot}



#end

```
## anticheat sk :
```
#
# @copiryght = 2024 SilverCore
# @autor = Silverdium
# @autor = MisterPapaye
#


options:
    prefix: "&8[&cSilver-AntiCheat&8]> &7"

# Création de la blacklist
on load:
    send "Loading skript ./plugins/skript/scripts/anticheat.sk" to console

    send "Black list player : %{blacklist::*}%" to console

# Détection de connexion avec blacklist
on join:
    if player's name is {blacklist::*}:
        send "&8[&cSilver-AntiCheat&8]> &7&e%player% &cTu es inscrit sur la BlackList, le Staff vient d'être averti de ta connexion !" to player
        loop all players:
            if loop-player has permission "skript.anticheat.alert":
                send "&8[&cSilver-AntiCheat&8]> &7&e%player% &cest connecté et est inscrit sur la blacklist !" to loop-player


# Détection de clics trop rapides (AutoClicker)
on click:
    if difference between {lastclick::%player%} and now < 55 millisecond:
        cancel event
        send "&8[&cSilver-AntiCheat&8]> &7&cDis donc, tu as un sacré CPS !" to player
        loop all players:
            if loop-player has permission "skript.anticheat.alert":
                send "&8[&cSilver-AntiCheat&8]> &7&e%player% &cpourrait utiliser un AutoClicker !" to loop-player
        add 1 to {suspect::%player%}
    set {lastclick::%player%} to now

# Détection de casse de blocs trop rapide
on break:
    if difference between {lastbreak::%player%} and now < 55 millisecond:
        cancel event
        send "&8[&cSilver-AntiCheat&8]> &7&cLes blocs se cassent si rapidement avec toi..." to player
        loop all players:
            if loop-player has permission "skript.anticheat.alert":
                send "&8[&cSilver-AntiCheat&8]> &7&e%player% &cpourrait utiliser un AutoMine !" to loop-player
        add 1 to {suspect::%player%}
    set {lastbreak::%player%} to now

# Surveillance des joueurs suspects
every 1 minutes:
    loop all players:
        if {suspect::%loop-player%} >= 5:
            loop all players:
            execute console command "tempban %loop-player% 1d Salut, je suis le SilverAntiCheat et je crois que tu fais des choses pas très légales... Pour de l'aide rejoins le Discord https://silverdium.fr !"
            if loop-player has permission "skript.anticheat.alert":
                send "&8[&cSilver-AntiCheat&8]> &7&e%loop-player% &ca vien de se faire bannir par l'anticheat !" to loop-player
            delete {suspect::%loop-player%}
        


#end
```
## chatmanager sk :
```
#
# @copiryght = 2024 SilverCore
# @autor = Silverdium
# @autor = MisterPapaye
#

on load:
    send "Loading skript ./plugins/skript/scripts/chatmanager.sk" to console

command /chatmanager:
	aliases: cm
	permission: skript.staff.chatm
	permission message: %{permmessage}%
	trigger:
		open chest with 1 rows named "&6SilverChat &f- &cOptions" to player
		wait 1 tick
		format slot 0 of player with green wool named "&a&lActiver le Chat !" to close then run [make player execute command "chat enable"]
		format slot 8 of player with red wool named "&c&lDésactiver le chat !" to close then run [make player execute command "chat disable"]
		format slot 4 of player with paper named "&7&lClear le chat" to close then run [make player execute command "chat clear"]


command /chat <text>:
	aliases: c
	permission: skript.staff.chatm
	permission message: %{permmessage}%
	trigger:
		if arg 1 is "clear":
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast ""
			broadcast "%{prefix::chat}% &fLe chat a été nettoyé par &c%player%"
		if arg 1 is "enable":
			set {chat.serveur} to true
			broadcast "%{prefix::chat}% &cLe Chat a été activé par &c%player%"
		if arg 1 is "disable":
			set {chat.serveur} to false
			broadcast "%{prefix::chat}% &cLe Chat a été désactivé par &c%player%"
		if arg-1 is not set:
			send "%{prefix::chat}% %{used}% /chat <target:clear|enable|disable>"
			
on chat:	
	if {chat.serveur} is false:
		cancel event
		send "%{prefix::chat}% &cLe chat est désactivé, tu ne peux pas envoyer de messages !" to player
	else:
		stop


#end 
```

---
Copyright (c) 2024 SilverCore | Tous droits réservés.<br>
Vous n'êtes pas autorisé à vendre ce code sans l'autorisation explicite de l'auteur.
