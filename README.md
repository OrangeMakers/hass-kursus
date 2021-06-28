# I gang med Home Automation

Når du går hjem idag vil du kunne installere Home Assistant, opsætte Wifi på Home assistant.

Du vil også have integreret Home Assistant med en ESP Chip, vi vil have lavet noget basis automatisering så du kan se hvorfor netop Home Assistant giver værdi til dit hjem.

Du vil have prøvet at integrere ZigBee via en [CC2531 USB Dongle](https://mackabler.dk/sonoff-zigbee-cc2531-usb-dongle-3685.html?utm_source=adtraction&utm_medium=affiliate&utm_campaign=adtraction&at_gd=2687A5538131C3E8D2671FD36774C1014EAF1F91)

**BEMÆRK** - Den dongle du køber skal have rigtig firmware, dem på ovenstående link er flashed med korrekt firmware fra fabrikken. Det er også muligt at købe dem via AliExpress, men pas på du får den rigtige.

# Home Assistant
Home Assistant er bygget i Python og kan køre på alle aktuelle linux distributioner, det er dog ikke anbefalet at installere Home Assistant i hånden med mindre man er udvikler, der findes mange meget bedre løsninger.

Jeg bruger og anbefaler Home Assistant fordi systemet har en tilpas grad af muligheder samt brugervenlighed, og jeg ved projektet er meget fokuseret på at det stadigvæk skal kunne alle de lækre ting, men der er enormt meget fokus på det også skal være brugervenligt. For bare 2 år siden skulle man næsten lave alt i YAML filer, og der var stort set ingen automatisering i Web Interfacet.

## Installations metoder

Der er 4 måder man kan installere Home Assistant på og vi vil følge den 1. som jeg vil anbefale

1. Home Assistant Operating System

   * En pakke der indeholder det hele, som er tilgængelig til Raspberry og Intel NUC
2. Home Assistant Container

   * Et Docker Image som indeholder Home Assistant Core
3. Home Assistant Supervised

   * Den gamle måde at installere det på - der var en komponent der holdt styr på filerne og opdateringer etc.
4. Home Assistant Core
   
   * Installation direkte via Python (Typisk kun til udviklere)

# Installation af HomeAssistant

Når vi skal installere Home Assistant skal vi bruge et Image til vores Raspberry, der findes en [vejledning](https://www.home-assistant.io/installation/raspberrypi) på HomeAssistant's hjemmeside men herunder vil vi gå punkterne igennem

Det image som HomeAssistant stiller til rådighed hedder i daglig tale HassOS (Home Assistant Operating System) som er et speciel image der indeholder alle nødvendige pakker og som er bygget op med best practice.

## Flash tool

Vi kan bruge [balenaEtcher](https://www.balena.io/etcher/) til at flashe din firmware, og du kan enten downloade filen eller indsætte en URL direkte i programmet.

### Flash from URL
![Vælg Flash from URL](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-1.png)

### Indsæt URL til Firmware
![Vælg Flash from URL](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-2.png)

### Vælg Enhed
![Vælg Flash from URL](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-3.png)
![Vælg Flash from URL](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-4.png)

### Tryk Flash
*Det kan tage op til 10 minutter alt efter hvor hurtig din internet forbindelse er*

![Vælg Flash from URL](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-5.png)

### Færdig
Hvis alt virker vil du få denne skærm frem når du er færdig.

![Vælg Flash from URL](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-6.png)

## Første opstart

## Opsætning af Wifi

# Installation af Zigbee

## Installation af Zigbee dongle

## Paring med Ikea pære

## Prøv at styre pære

# Installation af ESPHome

Link til ESP Home / Devices etc

## Installation af integration

## Opret første device - og flash det

## Test integration

# Automatisering

## Tænd lampe når der er over 30 grader på temperatur føler

## Tænd lyset på et bestemt tidspunkt

## Optional: Pir Sensor

### Diode

### PIR Sensor

# Links

* [Home Assistant](https://www.home-assistant.io)

## Firmwares
Aktuel version som passer til dette materiale, du kan altid finde nyeste versioner [her](https://www.home-assistant.io/installation/raspberrypi).

* [haos_rpi3-64-6.1.img.xz](https://github.com/zenturacp/hass-kursus/raw/main/Firmware/haos_rpi3-64-6.1.img.xz)
* [haos_rpi4-64-6.1.img.xz](https://github.com/zenturacp/hass-kursus/raw/main/Firmware/haos_rpi4-64-6.1.img.xz)
