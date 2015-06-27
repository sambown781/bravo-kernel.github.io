title: "Handige Minecraft weetjes (Dutch)"
date: 2015-06-27 10:38:10
tags:
categories:
  - Minecraft
---


## Multiverse

- [Multiverse website](http://dev.bukkit.org/bukkit-plugins/multiverse-core/)
- [Alle Multiverse commando's](https://github.com/Multiverse/Multiverse-Core/wiki/Command-Reference)
- [Alle Multiverse per-wereld commando's](https://github.com/Multiverse/Multiverse-Core/wiki/World-properties)

## Multiverse spawns

Om als speler naar de spawn te gaan van de wereld waar je nu in zit:

```bash
/mv spawn
```

Om als speler naar de spawn van een andere wereld dan degene waar je nu in zit te gaan:

```bash
/mv tp <naam-van-je-wereld>
```

Om de standaard spawn wereld voor **nieuwe** spelers (die voor het eerst op je server inloggen)
in te stellen voer je deze commando's als admin uit:

```bash
/mv conf firstspawnoverride true
/mv conf firstspawnworld <naam-van-je-wereld>
```

## Multiverse werelden

### Namen
Omdat je de namen van je werelden veel zal moeten gebruiken (voor portals, aliassen, etc) is het niet
verstandig om "zo maar namen" te gebruiken. Probeer een slimme naamgeving te gebruiken die je leven
als admin een stuk makkelijker zal maken. Goede tips zijn:

- gebruik alleen kleine letters
- gebruik volgnummers voor je werelden (bijv. creative1, creative2, kingdom1)

### Seeds

Minecraft werelden worden gemaakt met een ``seed``. Seeds zijn combinaties van letters en/of cijfers
die het uiterlijk van je wereld bepalen. Je kunt zelf een seed bedenken of er één pikken van de
vele sites die seeds bijhouden zoals http://www.minecraftseeds.co/.

### Nieuwe wereld maken

Om een nieuwe unieke Multiverse wereld te maken met je eigen seed voer je dit commando uit:

```bash
mv create <naam-van-je-wereld> NORMAL -s "Mijn eigen unieke seed!"
```

**Let op:** vergeet je nieuwe wereld niet toe te voegen aan het lijstje met ``PerWorldInventory``
werelden!

### Flat wereld maken

Soms wil je liever een platte wereld, voer dan dit commando uit:

```bash
/mv create flatworld1 NORMAL -t FLAT
```

**Let op:** vergeet je nieuwe wereld niet toe te voegen aan het lijstje met ``PerWorldInventory``
werelden!

### Gamemode instellen

Je kunt de gamemode voor je wereld instellen op ``creative`` of ``survival`` door
één van deze commando's te kiezen:

```bash
/mvm set gamemode creative <naam-van-je-wereld>
/mvm set gamemode survival <naam-van-je-wereld>
```

### Moeilijkheid instellen

Je kunt de moeilijkheid (difficulty) voor je wereld instellen door één van deze commando's te kiezen:

```bash
/mvm set diff peaceful <naam-van-je-wereld>
/mvm set diff easy <naam-van-je-wereld>
/mvm set diff normal <naam-van-je-wereld>
/mvm set diff hard <naam-van-je-wereld>
```

### Monsters uitschakelen

Met het volgende commando kun je monsters uitschakelen voor je wereld (bijvoorbeeld handig als je je
wereld als hub wilt gebruiken):

```bash
/mvm set monsters false <naam-van-je-wereld>
```

### Weer instellen

Met het volgende commando kun je het weer voor je wereld uitschakelen:

```bash
/mvm set weather false <naam-van-je-wereld>
```

### Wereld verwijderen

Denk altijd goed na voordat je een wereld weggooit en maak het liefst eerst een backup zodat je
nooit per ongeluk al het werk van je spelers weggooit!

Om een wereld te verwijderen voer je eerst dit commando uit:

```bash
/mv delete <naam-van-je-wereld>
```

Daarna moet je binnen 15 seconden bevestigen met:

```bash
/mvconfirm
```

## Multiverse Portals

- [Multiverse-Portals plugin](http://dev.bukkit.org/bukkit-plugins/multiverse-portals/)
- [Multiverse-Portal voorbeeld](https://github.com/Multiverse/Multiverse-Portals/wiki/Basic-portal-tutorial)

### Nieuw portaal maken

**Let op:** je kunt geen portals maken als de ``Worldedit`` plugin aanstaat dus zorg er eerst voor
dat je de Worledit plugin uitschakelt. Nadat je het portal gemaakt hebt kun je Worldedit plugin
weer inschakelen.

Om een nieuw portal te maken bouw je eerst een structuur zoals in het voorbeeld hieronder:


Zet daarna de portal wand aan met dit commando:

```bash
/mvp wand
```

Stel nu het 1e coordinaat in door met je wand linksonderin je structuur te klikken, klik daarna
rechtsboven om het 2e coordinaat in stellen.

Als het goed is kun je nu je portal naar een andere wereld aanmaken met het volgende commando:

```bash
/mvp create <naam-van-je-portal> w:<naam-van-de-wereld-waar-het-portal-uitkomt>
```

**Let op:** je kunt je portal vullen met alles behalve nether, nether zorgt er voor dat je portal
niet zal werken. Een goed alternatief is lava of water.

### Portal vrijgeven (rechten)

Standaard kunnen alleen OPs je nieuwe portal gebruiken en dat is meestal niet waar je ze voor
gebouwd hebt. Om er voor te zorgen dat ook normale spelers de portals kunnen gebruiken voer je
deze twee commando's uit:

```bash
/mv conf enforceaccess false
/mvp conf enforceportalaccess false
```

## Multiverse tweaks

### Chat prefix uitschakelen

Multiverse zet de naam van de wereld van waaruit gechat wordt voor het chatbericht en dat is....
gewoon irritant. Schakel het uit met het volgende commando:

```bash
/mv conf prefixchat false
```

### Monsters verwijderen

Als je alle monsters wilt verwijderen die al in je wereld zijn gespawned gebruik je 
het volgende commando:

```bash
/mv purge <naam-van-je-wereld> monsters
```

## OPs

Om te zien wie er allemaal OP zijn op je server:

1. log in op de website
2. Ga naar ``Files`` > ``FTP File Access``
3. open het bestand ``ops.json``

## HomeSpawnPlus

Gebruik de [HomeSpawnPlus](http://dev.bukkit.org/bukkit-plugins/homespawnplus/)
plugin om in te stellen dat iedere speler bij het inloggen (joinen)
in een bepaalde Multiverse wereld terecht komt. Bijvoorbeeld handig als je wilt
dat iedereen na het inloggen in je hub wereld terecht komt.


Stel eerst de naam van de wereld in die je als standaard wereld witl gebruiken.
Open hiervoor ``core.yml`` en pas de ``defaultWorld`` aan. Hieronder zie je
een voorbeeld waarbij de wereld ``hub`` als standaard wereld wordt gebruikt.

```yml
# The default/main world. Strategies reference this as the default
# world, you can change that here if you like.
defaultWorld: hub
```

Open daarna ``events.yml`` en stel bij het ``onJoin`` event in dat de wereld
die je hierboven als standaard wereld hebt ingesteld (hub) moet bij het joinen
van je server als spawn wereld (spawnDefaultWorld) moet worden gebruikt:

```yaml
events:
  # strategies to use when player is joining the game
  onJoin:
    - spawnNewPlayer
    - spawnDefaultWorld
```

Reload je server en iedereen (ook OPs) landen voortaan netjes in je hub wereld.

## SuperLogger

De [SuperLogger](https://github.com/zack6849/SuperLogger) plugin houdt bij
wie allemaal inlogt op je server en welke commando's men wanneer uitgevoerd heeft. Handig als je
bijvoorbeeld wilt weten wie iemand OP heeft gegeven.

1. log in op de website
2. Ga naar ``Files`` > ``FTP File Access``
3. Open de map ``Plugins`` > ``SuperLogger``
4. De logs kun je per dag vinden in de map ``logs``

Instellingen kun je aanpassen in ``config.yml``:

```yaml
auto-update: true
update-notify: true
debug: false
log:
  commands: true
  #do we check if a command is even a real command before we log it?
  check-command-exists: false
  chat: false
  join: true
  quit: true
  kick: false
  death: false
  #do we log disallowed connections? (banned, not whitelisted, full server, etc)
  failed-connections: true
  player-ip: true
  player-uuid: true
  coordinates: true
#a list of commands ignored by superlogger, for privacy reasons.
blacklist:
  - 'login'
  - 'register'
```

## BetterAlias

Met de [BetterAlias](http://dev.bukkit.org/bukkit-plugins/betteralias/) plugin kun je aliassen
aanmaken zodat je met één woord een moeilijk commando kunt laten uitvoeren. Bijvoorbeeld handig
als je spelers naar een wereld wilt laten tp'en met 1 commando.

1. log in op de website
2. Ga naar ``Files`` > ``FTP File Access``
3. Open de map ``Plugins`` > ``BetterAlias``
4. Voeg een alias toe aan het bestand ``aliases.yml``

Hieronder zie je een paar voorbeelden. Als iemand wil pvp'en hoeft hij alleen nog maar het
alias ``/pvp`` in te tikken, het commando ``/mv tp topsy-vale`` wordt dan automatisch voor hem
uitgevoerd.

```yaml
hub:
    0: /mv tp hub

creative:
    0: /mv tp plotworld

pvp:
    0: /mv tp topsy-vale

factions:
    0: /mv tp factions1

kingdom:
    0: /mv tp kingdom1
```

## Per World Inventory

De [Per World Inventory](http://www.spigotmc.org/resources/per-world-inventory.4482/) plugin
maakt het mogelijk om voor iedere Multiverse wereld een aparte inventory te maken.

Hiervoor moet de plugin wel vertellen welke werelden er allemaal op je server bestaan.

1. log in op de website
2. Ga naar ``Files`` > ``FTP File Access``
3. Open de map ``Plugins`` > ``PerWorldInventory``
4. Open het bestand ``worlds.yml``

Maak groepen aan voor **alle** werelden op je server die een eigen inventory moeten hebben (of delen).

```yaml
groups:
  default:
    worlds:
    - hub
  kitbattle:
    worlds:
    - topsy-vale
    - topsy-vale_nether
    - topsy-vale_the_end
  kingdom:
    worlds:
    - kingdom1
  creative:
    worlds:
    - plotworld
  factions:
    worlds:
    - factions1
```

## PlotMe

Met de [PlotMe](http://dev.bukkit.org/bukkit-plugins/plotme/) plugin kun je eenvoudig een
coole creative wereld maken met professionele plot features. **Let op**: de plugin zit niet
in Spigot, je moet 'm handmatig installeren.

Eerst moet de wereld aangemaakt worden met dit Minecraft commando:

``
/mv create plotworld normal -g PlotMe-DefaultGenerator
``

De wereld is nu aangemaakt maar voordat normale spelers er gebruik van
kunnen maken moet je ze eerst ``plotme.use`` rechten geven in PermissionsEx.

1. log in op de website
2. Ga naar ``Files`` > ``FTP File Access``
3. Open de map ``Plugins`` > ``PermissionsEx``
4. Open het bestand ``permissions.yml``

```yaml
groups:
  default:
    options:
      default: true
    permissions:
    - modifyworld.*
    - herochat.join.*
    - herochat.speak.*
    - herochat.leave.*
    - plotme.use
```

Nu alleen nog je server reloaden en je spelers kunnen aan de slag met hun plots.

**Let op:** vergeet je nieuwe wereld niet toe te voegen aan het lijstje met ``PerWorldInventory``
werelden!

## Coole MOTD

De Message Of The Day (MOTD) van je server is wat spelers zien in de lijst met servers als ze een
potje online willen spelen. Standaard zie je daar een saaie grijze tekst maar die kun je pimpen door
verschillende lettertypes (dik, dun, doorgestreept) s en een combinatie van kleuren te gebruiken.

Hou er wel rekening mee dat het niet eenvoudig is om een coole MOTD te maken en dat je eerst zult
moeten begrijpen hoe de codes voor kleuren en lettertypes werken. Maar als je dat eenmaal doorhebt
maak je al snel de coolste MOTDs.

### Kleurcodes

Voor het voorbeeld dat straks komt hoef je alleen de vier kleuren te kennen die hieronder staan.
Als je andere kleuren wilt gebruiken bezoek dan [deze link](http://minecraft.gamepedia.com/Classic_server_protocol).

| Kleur       | code |
| ----------- |---------|
| rood        | \u00A7c |
| geel        | \u00A7e |
| roze        | \u00A7d |
| donkergrijs | \u00A78 |

### Lettertypes

Met deze lettertypes kun je nog meer variatie aanbrengen in je MOTD:

| Lettertype  | code |
| ----------- |---------|
| dik         | \u00A7l |
| doorgehaald | \u00A7m |
| reset       | \u00A7r |

### Je eigen MOTD maken

Hieronder zie je een paar voorbeelden van een gepimpte MOTD. Als je goed kijkt zie je dat er
codes voor de tekst staan, die code codes bepalen hoe de tekst wordt weergegeven.

**Let op** zorg er voor dat je altijd eerst de kleurcode gebruikt en dan pas de lettercode (anders
snapt Minecraft het niet)

Een voorbeeld van een MOTD met maar één regel:

```
\u00A7c\u00A7m------\u00A7r\u00A7c\u00A7l[\u00A7e\u00A7lMijnServer\u00A7c\u00A7l]\u00A7c\u00A7m------
```

Een voorbeeld van een MOTD met twee regels (de \n zorgt dat de rest op regel 2 terecht komt):

```
\u00A7c\u00A7m------\u00A7r\u00A7c\u00A7l[\u00A7e\u00A7lMijnServer\u00A7c\u00A7l]\u00A7c\u00A7m------\n\u00A7r\u00A78PVP | FACTIONS | KINGDOM | CREA | \u00A7d\u00A7lALTIJD GEZELLIG
```

### Je MOTD bijwerken

Om je nieuwe MOTD zichtbaar te maken in de serverlijst:

1. log in op de website
2. Ga naar ``Files`` >  ``Config files``
3. Open ``Server Settings``
4. Kopieer je eigen MOTD en plak deze in het veld naast ``Server Message``
5. Druk op ``Save`` onderaan de pagina om op te slaan
6. Reload je server om de nieuwe MOTD te activeren

# Handige links

- [Klassieke Minecraft kleurcodes](http://minecraft.gamepedia.com/Classic_server_protocol)
