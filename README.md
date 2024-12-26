# SivlerSK
Projet de scripts réaliser avec le plugin minecraft [skript](https://skript-mc.fr/) !
---
# Projet en dévelopement | maquettes :
## inspect.sk :
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

---
Copyright (c) 2024 SilverCore | Tous droits réservés.<br>
Vous n'êtes pas autorisé à copier, modifier, distribuer, ou vendre ce code sans l'autorisation explicite de l'auteur.
