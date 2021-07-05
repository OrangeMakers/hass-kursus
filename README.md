# Igang med Home Automation

Når du går hjem idag vil du kunne installere Home Assistant, opsætte Wifi på Home assistant.

Du vil også have integreret Home Assistant med en ESP Chip, vi vil have lavet noget basis automatisering så du kan se hvorfor netop Home Assistant giver værdi til dit hjem.

Du vil have prøvet at integrere ZigBee via en [CC2531 USB Dongle](https://mackabler.dk/sonoff-zigbee-cc2531-usb-dongle-3685.html?utm_source=adtraction&utm_medium=affiliate&utm_campaign=adtraction&at_gd=2687A5538131C3E8D2671FD36774C1014EAF1F91)

**BEMÆRK** - Den dongle du køber skal have rigtig firmware, dem på ovenstående link er flashed med korrekt firmware fra fabrikken. Det er også muligt at købe dem via AliExpress, men pas på du får den rigtige.

# Protokoller

## Wifi
Nok den mest kendte protokol, er hurtig og har ok rækkevide, men bruger meget strøm

## Bluetooth
Har ikke særlig god rækkevide og er ikke særlig adopteret som IoT protokol

## ZigBee
Meget brugt blandt andet af Philips Hue, og Ikea trådfri, meget letvægts protokol, bruger meget lidt strøm, og er et "mesh" netværk

Zigbee køre AES128 som anses for sikkert og kommunikere på 2.4ghz hvilket kan give problemer med Wifi

Et Zigbee netværk har en teoretisk maks på 65000 enheder men er aldrig opnået i virkeligheden

## ZWave
Ikke helt så meget brugt og har nogle begrænsinger og fordele, et mesh kan maks sende igennem 4 noder, og der kan maks være 232 enheder i et netværk.

Dog køre ZWave 800-900Mhz ikke 2.4Ghz som Zigbee, dvs. ZWave virker bedre steder hvor der er meget Wifi.

## LORA
LORA er en radio protokol, som er anderledes end ovenstående da den er lavet og optimeret til lang distance meget små beskeder og ikke helt så realtime.

LORA kan som udgangspunkt nemt række 1-2km men LORA beskeder har en begrænsing, og LORA er meget langsomt.

## 866mhz (Radio)
Kendt fra rullegardiner, og de trådløs styret Wall plugs fra Harald Nyborg, er en envejs protokol, uden validering.

# Endnu en gateway eller?
Hvorfor skal jeg skifte min Ikea Trådfri eller Hue Base ud?

Svaret er - det behøver du heller ikke, men det er meget svært at få din Hue til at tænde blinke med lyset i stuen når din vaskemaskine i 5 minutter har trukket 0 Amperer?

Home Assistant kan enten stå alene med diverse radioer på, eller kan forbinde til din eksisterende [Philips Hue](https://www.home-assistant.io/integrations/hue/), [Ikea Trådfri](https://www.home-assistant.io/integrations/tradfri/) eller [Apple HomeKit](https://www.home-assistant.io/integrations/homekit/) ja der er ialt ca. [1800 integrationer](https://www.home-assistant.io/integrations/).

![integrations-featured.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/integrations-featured.png)

# Home Assistant
Home Assistant er bygget i Python og kan køre på alle aktuelle linux distributioner, det er dog ikke anbefalet at installere Home Assistant i hånden med mindre man er udvikler, der findes mange meget bedre løsninger.

Jeg bruger og anbefaler Home Assistant fordi systemet har en tilpas grad af muligheder samt brugervenlighed, og jeg ved projektet er meget fokuseret på at det stadigvæk skal kunne alle de lækre ting, men der er enormt meget fokus på det også skal være brugervenligt. For bare 2 år siden skulle man næsten lave alt i YAML filer, og der var stort set ingen automatisering i Web Interfacet.

## Installations metoder

Der er 4 måder man kan installere Home Assistant på og vi vil følge den 1. som jeg vil anbefale

1. Home Assistant Operating System (**HassOS**)

   * En pakke der indeholder det hele, som er tilgængelig til Raspberry og Intel NUC eller en virtuel maskine
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
![1-etcher-1.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-1.png)

### Indsæt URL til Firmware
![1-etcher-2.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-2.png)

### Vælg Enhed
![1-etcher-3.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-3.png)
![1-etcher-4.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-4.png)

### Tryk Flash
*Det kan tage op til 10 minutter alt efter hvor hurtig din internet forbindelse er*

![1-etcher-5.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-5.png)

### Færdig
Hvis alt virker vil du få denne skærm frem når du er færdig.

![1-etcher-6.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/1-etcher-6.png)

Nu er din Raspberry klar til at starte 1. gang - for at komme til den Wizard som man sætter HomeAssistant op fra skal man forbinde via Ethernet, hvorfra man så kan sætte Wifi op.

*Bemærk der kan godt gå op til 20 minutter før den er klar - der sker en del opsætning 1. gang den starter op*

## Første opstart

Du har flere måder du kan åbne den på enten via IP eller via mDNS (Virker primært på Mac og Linux), jeg anbefaler Windows brugere at bruge IP adressen.

DNS Navn: http://homeassistant.local:8123

IP Adresse: http://(findip fra DHCP Server):8123 f.eks. http://192.168.1.120:8123

### Opstarts Billede
Når du starter den 1. gang vil der blive installeret alt der skal bruges - dette kan tage op til 20 minutter på en langsom Raspberry
![2-hass-1.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/2-hass-1.png)

### Oprette bruger
Du skal oprette dig som bruger når du logger på 1. gang, det er vigtigt at du laver en kode som er komplex hvis du på sigt skal sætte din Home Assistant på Internettet
![2-hass-2.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/2-hass-2.png)

### Placering
Du skal sætte en placering, det er vigtigt denne er korrekt og er der hvor dit device er, dette bruges blandt andet til at beregne hvor solen står i forhold til hvor du er.
![2-hass-3.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/2-hass-3.png)

### Diagnistics Data
Der er helt sikkert en masse holdninger til dette - jeg anbefaler man deler med udviklerne, de levere trods alt et gratis produkt, og jeg er ret sikker på man ikke kommer til at have GDPR data her :-) Men som sagt, så er det helt op til jer selv.
![2-hass-4.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/2-hass-4.png)

### Tryk Afslut
![2-hass-5.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/2-hass-5.png)

### Hjemmeskærm
Her kan du med fordel trykke Ja - så du ikke skal logge ind
![2-hass-6.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/2-hass-6.png)

## Wifi Opstæning
Det anebfales ikke man køre sin Home Assistant via Wifi, f.eks. hvis man vil lave noget automatisering på sigt der kan styre dit wifi, så er det ikke et så smart initiativ, og der er ingen problemer i at sætte sin Home Assistant direkte på din router. Men hvis du alligevel vil denne vej er der en guide her

### Gå ind i Supervisor
![3-hasswifi-1.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/3-hasswifi-1.png)

### Gå over i System
Tryk Ændre under IPV4
![3-hasswifi-2.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/3-hasswifi-2.png)

### Vælg WLAN0
Sæt IPV4 til enten DHCP eller Fast IP

Under Wifi vælg Scan for Adgangspunkter
![3-hasswifi-3.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/3-hasswifi-3.png)

### Vælg det Wifi du vil forbinde til og udfyld kode i WPA-PSK og tryk GEM
**OBS! Du vil få ny IP adresse på WLAN0 interface**
![3-hasswifi-4.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/3-hasswifi-4.png)


# Gennemgang af Interface
* Hjemmeskærm
  * Start siden - med Tabs
* Kort
  * Hvis man har lokations tjenester kan man følge personer / ting på kortet
* Logbog
  * En log bog med handlinger
* Historik
  * Historiske data f.eks. temperatur sensor data historik
* Mediebrowser
  * Ja - bruger den ikke rigtig - Home Assistant er ikke et medie center!
* Udviklerværktøjer
  * Hvis du vil ned i maven på systemet
* Supervisor
  * Opdateringer / Plugins styres herfra
* Indstillinger
  * Hvis du vil ændre opsætningen på din Home Assisiant er det her det foregår
* Notifikationer
  * Info til dig - lidt som et "besked" system fra systemet

# Installation af Zigbee
Zigbee - er den protokol som Philips Hue og Ikea Trådfri bruger, vi har mulighed for at tale direkte med en Trådfri pære via Home Assistant, uden at have en Ikea Gateway.

Det er faktisk også muligt at bibeholde sin eksisteren [Ikea Trådfri](https://www.home-assistant.io/integrations/tradfri/) eller [Philips Hue](https://www.home-assistant.io/integrations/hue/) via Integrations hvis man allerede har en nuværende løsning kørende. Man vil have præcis samme muligheder via de integrationer som at tilføje enhederne direkte via en USB nøgle.

## Installation af Zigbee dongle (ZHA)
Det er muligt at købe en [CC2531 USB Dongle](https://mackabler.dk/sonoff-zigbee-cc2531-usb-dongle-3685.html?utm_source=adtraction&utm_medium=affiliate&utm_campaign=adtraction&at_gd=2687A5538131C3E8D2671FD36774C1014EAF1F91) som er direkte kompatibel med Home Assistants integration der hedder [ZHA](https://www.home-assistant.io/integrations/zha/).

### Åben Indstillinger
Og klik på Integrationer

![4-hasszha-1.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/4-hasszha-1.png)

### Vælg Tilføj Integration

![4-hasszha-2.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/4-hasszha-2.png)

### Søg efter ZHA og tryk på **>**

![4-hasszha-3.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/4-hasszha-3.png)

### Vælg den serial port der indeholder CC2531 (Dette er navn på Chippen fra Texas Instruments)
Og tryk Send og Afslut

![4-hasszha-4.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/4-hasszha-4.png)
![4-hasszha-5.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/4-hasszha-5.png)
![4-hasszha-6.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/4-hasszha-6.png)

### Nu er integrationen på plads
Du kan nu gå igang med at pare dine enheder

![4-hasszha-7.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/4-hasszha-7.png)

## Paring med Ikea pære
Vi skal have vores første enhed sat op på Home Assistant

### Start med at gå ind i Indstillinger og klik på Integrationer

![5-hassikeapair-1.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/5-hassikeapair-1.png)

### På din Zigbee Home Automation klikker du på Konfigurere

![5-hassikeapair-2.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/5-hassikeapair-2.png)

### Klik på tilføj Enhed
Når du klikker på denne knap vil den sætte dit Zigbee netværk i Pair Mode og det vil søge efter nye enheder i 60 sekunder.

Hvis din pære ikke dukker op kan du [nulstille den](#Nulstille-Ikea-Pære)

![5-hassikeapair-3.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/5-hassikeapair-3.png)

### Din controller vil nu søge efter enheder der kan tilføjes

![5-hassikeapair-4.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/5-hassikeapair-4.png)

### Når den finder en enhed vil denne dukke op med informationer om hvad enheden er og hedder
Hvis du har oprettet områder kan du tilføje et område

![5-hassikeapair-5.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/5-hassikeapair-5.png)

### Det er vigtigt at navngive sin pære, da det kan være svært at finde den efterfølgende

![5-hassikeapair-6.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/5-hassikeapair-6.png)

### Når din enhed er klar kan du teste den ved at gå tilbage til oversigten og trænde / slukkee / skifte farve på enheden.

![5-hassikeapair-7.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/5-hassikeapair-7.png)

### Her et et eksempel på at skifte pæren til rød

![5-hassikeapair-8.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/5-hassikeapair-8.png)

# Simpel Automatisering
I dette eksempel vil vi sætte pæren til at slukke og tænde hvert minut.

### Åben indstillinger / Automatisering

![6-hassautomation-1.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/6-hassautomation-1.png)

### Vælg + Tilføj Automatisering

![6-hassautomation-2.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/6-hassautomation-2.png)

### Vælg Start med en tom automatisering

![6-hassautomation-3.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/6-hassautomation-3.png)

### Udfyld formularen
Navn: Tænd Sluk pære hvert minut
Tidsmønster: minut /1 (Betyder hvert minut)

Betingelser: Ingen

![6-hassautomation-4.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/6-hassautomation-4.png)

### Vælg enhed Ikea pære
Under handling vælg Skift ikea pære level, denne funktion laver en "toggle" på pæren så hvis den er tændt slukker den og er den 
slukket tænder den

Tryk nu **Gem**

![6-hassautomation-5.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/6-hassautomation-5.png)

### Du kan teste din automation ved at trykke på Udløs, den vil så køre din automatisering, når du køre den manuelt vil trigger / betingelser blive ignoreret

![6-hassautomation-6.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/6-hassautomation-6.png)

### Du kan også spore din automatisering for at se hvad der sker
![6-hassautomation-7.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/6-hassautomation-7.png)

# Gennemgang af automatisering

* Enheder
* Services
* Events

# Hvad er ESP Home?
ESPHome er et addon du kan installere på Home Assistant, hvor man på en nem måde kan integere en billig ESP Chip, let og elegant ind i Home Assistant.

Som udgangspunkt kommunikere Home Assistant med ESP via HTTP.

Man kan se dokumentation via https://esphome.io som indeholder en liste over supporterede enheder.

## Installation af ESP Home

### Gå ind under Supervisor / Butik for tilføjelsesprogrammer
Søg efter ESPHome og vælg denne

![7-hassesphome-1.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/7-hassesphome-1.png)
![7-hassesphome-2.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/7-hassesphome-2.png)

### Tryk Installer
**Dette kan tage op til 20 minutter**
![7-hassesphome-3.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/7-hassesphome-3.png)

### Når den er installeret skal du starte ESPHome og integrere denne i vores oversigt
Det gøres ved at man åbner Supervisor og under betjeningspanel aktivere ESPHome
![7-hassesphome-4.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/7-hassesphome-4.png)

### Tryk vis i sidepanel og vælg Start
![7-hassesphome-5.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/7-hassesphome-5.png)

### Så er du klar til at tilføje første enhed
![7-hassesphome-6.png](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/7-hassesphome-6.png)

## Opret første device - og flash det
Nu skal vi have tilføjet første enhed

Udfyld med Navn / Wifi SSID og Wifi Password og tryk gem.

Hvis du er forbundet med SSL til din HomeAssistant kan din browser lave Remote Serial, hvis ikke kan du sætte din enhed i din raspberry med et USB kabel og flashe den direkte via USB kabel.

Jeg har lavet en test YAML fil som du kan downloade [her](https://raw.githubusercontent.com/zenturacp/hass-kursus/main/ESPHome/esp8266.yaml).

### Liste over Pins
| PIN | Beskrivelse   |
| --- | ------------- |
| 15  | Ekstern LED   |
| 2   | Intern LED    |
| 13  | Motion Sensor |
| 12  | Binær Switch  |

**PIR Sensor har følgende pins som skal sættes på**
![mini-pir-pinout.jpg](https://github.com/zenturacp/hass-kursus/raw/main/Screenshots/mini-pir-pinout.jpg)

**Switch er sat op til at køre PULLUP dvs. den skal sættes på mellem GND og pin 12.**

Hele koden herunder kan blot sættes direkte ind og erstatte den kode Wizard har lavet, det er blot vigtigt at kalde enheden omkursus-1-10 så vi får unikke ESP'er på netværket og man kan kende sin.


```yaml
esphome:
  name: omkursus-X
  platform: ESP8266
  board: d1_mini

# Enable logging
logger:

# Enable Home Assistant API
api:

ota:
  password: "ea06852cf61501095b87ca77e3d95810"

wifi:
  ssid: "HASS"
  password: "12345678"

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Omkursus Fallback Hotspot"
    password: "9MIIcZQgUMti"

captive_portal:

light:
  - platform: monochromatic
    name: "ESP Intern LED"
    output: 'esp_internal_led'
  - platform: monochromatic
    name: "ESP Extern LED"
    output: 'esp_external_led'

output:
  - platform: esp8266_pwm
    pin: 15
    id: 'esp_external_led'
  - platform: esp8266_pwm
    pin: 2
    id: 'esp_internal_led'
    inverted: true

binary_sensor:
  - platform: gpio
    pin: 13
    name: "Motion Sensor"
    device_class: motion
  - platform: gpio
    pin:
      number: 12
      mode: INPUT_PULLUP
      inverted: True
    name: "ESP Switch"
    
```

## Forbind Home Assistant til ESPHome Enhed

Når din enhed er flashed vil du via Integrationer se der er dukket en ny enhed op - denne enhed skal kobles ind på Home Assistant, dette er integreret via ESPHome Discovery, så de enheder der ligger i ESPHome kan Home Assisistant selv forbinde til.

# Automatisering

Nu kan vi eksperimentere med Automatisering mellem Zigbee og Wifi, når noget sker på ESP8266 kan vi "relay" beskeder over til pæren og få denne til at tænde og slukke.

# Links

* [Home Assistant](https://www.home-assistant.io)

## Firmwares
Aktuel version som passer til dette materiale, du kan altid finde nyeste versioner [her](https://www.home-assistant.io/installation/raspberrypi).

* [haos_rpi3-64-6.1.img.xz](https://github.com/zenturacp/hass-kursus/raw/main/Firmware/haos_rpi3-64-6.1.img.xz)
* [haos_rpi4-64-6.1.img.xz](https://github.com/zenturacp/hass-kursus/raw/main/Firmware/haos_rpi4-64-6.1.img.xz)

## Nulstille Ikea Pære

Hvis du har svært ved at parre din Ikea pære kan du nulstille den, det gøres ved at tænde og slukke pæren hurtigt 6 gange efter hinanden, når det er sket vil pæren stå og skifte farve et kort øjeblik og den vil så være nulstillet.

Video Guide:  
https://youtu.be/iTBh8OBLJSw

## Raspberry Pi oversigt

Der er 4 Raspberry Pi 3'ere som er gjort klar, de har IP adresse 192.168.0.60-63

| Username | Password |
| -------- | -------- |
| om       | 12345678 |

Du skal forbinde via http på port 8123 f.eks. http://192.168.0.60-63:8123

## Wifi

| SSID | WPA-PSK  |
| ---- | -------- |
| HASS | 12345678 |