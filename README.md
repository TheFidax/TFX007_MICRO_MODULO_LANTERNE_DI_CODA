# Micro Modulo per Lanterne di Coda FS
Questo micro modulo e' pensato per dotare i modelli in scala delle tipiche *lanterne di coda* usate per segnalamento luminoso di fine convoglio.

**Ultima Revisione HardWare: 1.00**

**Ultima Revisione SoftWare: 1.00**

**Alcune Immagini Dimostrative:**

- Modulo Faccia Superiore

<img src="https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/blob/main/Images/modulo_sopra.jpg" width="1280">

- Modulo Faccia Inferiore

<img src="https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/blob/main/Images/modulo_sotto.jpg" width="1280">

- Modulo Montato su Carrozza Corbellini Hachette e Dimostrazione Lanterne

<img src="https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/blob/main/Images/modulo_montato.jpg" width="1280">
<img src="https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/blob/main/Images/lanterna_sinistra.jpg" width="1280">
<img src="https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/blob/main/Images/lanterna_destra.jpg" width="1280">

------------

## Indice
* [Video Presentazione del Progetto](#video-presentazione-del-progetto)
* [Collegamenti Elettrici](#collegamenti-elettrici)
* [FirmWare](#firmware)
* [HardWare](#hardware)
* [Caratteristiche della Scheda](#caratteristiche-della-scheda)
  - [Micro Dimensioni](#micro-dimensioni)
  - [Ponte di Diodi](#ponte-di-diodi)
  - [Chip Step Down Buck MCP16331](#chip-step-down-buck-mcp16331)
  - [ATtiny10](#attiny10)
  - [Resistori LED incorporati](#resistori-led-incorporati)
  - [Predisposizione Condensatore PowerPack](#predisposizione-condensatore-powerpack)
* [Assemblaggio](#Assemblaggio)
* [Contattami](#Contattami)

------------


## Video Presentazione del Progetto
[![Video Presentazione](https://img.youtube.com/vi/kVVs_6TyThg/0.jpg)](http://www.youtube.com/watch?v=kVVs_6TyThg)

------------

## Collegamenti Elettrici

------------

## FirmWare
Il Firmware dedicato e' presente sotto la cartella [FirmWare](https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/tree/main/FirmWare), per poterlo caricare è consigliata la seguente [Scheda di Prototipizzazione](xxx) e il programmatore [USBasp](https://www.fischl.de/usbasp/).</br>
Le cifre finali del file .HEX identificano la versione del FirmWare.

**NOTA: *E' INDISPENSABILE* CARICARE IL FIRMWARE *PRIMA* DELL'ASSEMBLAGGIO! Pena l'impossibilita' di programmre il Chip ATtiny10**.

------------

## HardWare
Il progetto del Micro Modulo e' disponibile qui: https://workspace.circuitmaker.com/Projects/Details/luca-fidanza/Micro-RedLights-Module .</br>
**Viene rilasciato con la seguente Licenza**: https://creativecommons.org/licenses/by-nc-nd/4.0/ .</br>
I **File Gerber**, il **BOM** e il file **Pick and Place** sono nel file **.Zip** disponibile sotto la cartella [HardWare](https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/tree/main/HardWare).</br> 

**NOTA**: Lo spessore *consigliato* del PCB per questa scheda è di: **0,6mm**

------------

## Caratteristiche della Scheda
Di seguito sono riportate le caratteristiche della scheda, poi spiegate in dettaglio nei vari paragrafi dedicati.
- [Micro Dimensions](#micro-dimensioni): 10 x 10.5mm
- [MCP16331](#chip-step-down-buck-mcp16331) to power module at 3.3v
- Module can be operate with all systems: [Analog CC, Analog AC and Digital](#ponte-di-diodi)
- [Builtin](#resistori-led-incorporati) 1kOhm lamps resistors
- [Slow charge power pack system](#predisposizione-condensatore-powerpack)
- MINIMUM CLEARANCE: 6mil

------------

### Micro Dimensioni
<img src="https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/blob/main/Images/modulo_sopra.jpg" width="1280">

Le dimensioni di questo modulo sono le seguenti (con la scritta TFX in alto):
- Altezza:    10mm
- Larghezza:  10,5mm
- Spessore:   Non misurato

------------

### Ponte di Diodi
<img src="https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/blob/main/Images/graetz.jpg" width="1280">

Il circuito di alimentazione e' realizzato meadiante 4 diodi in configurazione [Ponte di Graetz](https://it.wikipedia.org/wiki/Raddrizzatore#Ponte_di_Graetz).</br>
Tale configurazione permette di *raddrizzare* la tensione captata dalle prese di corrente in Conrente Continua a prescindere del sistema di alimentazione:
- Corrente Continua Analogica (fornisce sempre gli stessi poli in uscita)
- Corrente PWM (raddrizza l'onda quadra fornendo una tensione simil continua)
- Corrente Alternata Analogica (raddrizza l'onda sinusoidale fornendo una tensione simil continua)
- Digitale (raddrizza l'onda quadra fornendo una tensione simil continua)

------------

### Chip Step Down Buck MCP16331
<img src="https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/blob/main/Images/modulo_sotto.jpg" width="1280">

L'alimentazione a 3,3 volt e' fornita dal chip [Microchip MCP16331](https://www.microchip.com/wwwproducts/en/MCP16331), un regolatore di tensione di tipo [Step Down Buck](https://it.wikipedia.org/wiki/Convertitore_buck) in gradi di ricevere in ingresso tensioni fino a 50 volt e di fornire in uscita una tensione stabile a 3,3 volt con sviluppo di calore minimo.</br>

------------

### ATtiny10
<img src="https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/blob/main/Images/attiny10.jpg" width="1280">

Il lampeggio asincrono e' generato dal chip [ATtiny10](https://www.microchip.com/wwwproducts/en/ATtiny10).</br>
Questo chip opera alla tensione di 3,3v e pilota i led mediante resistori incorporati sul modulo.

**NOTA: Va programmato PRIMA di saldarlo sul modulo!**

------------

### Restori LED Incorporati
<img src="https://github.com/TheFidax/TFX007_MICRO_MODULO_LANTERNE_DI_CODA/blob/main/Images/resistori_led.jpg" width="1280">

Per ridurre al minimo i componenti accessori sul modulo **Sono Gia' Presenti** due resistori da **1kOHm** per i led delle lanterne.

------------

### Condensatori PowerPack
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/powerpack.jpg" width="1280">

Per sopperire a problemi di captazione di corrente e' previsto un sistema *powerpack* formato da 4 condensatori al Tantalio da 100uF con tensione **massima** di 25 volt.</br>
I condensatori sono separati dal circuito di alimentazione da un Diodo ed un Resistore che rappresentano *il sistema di ricarica lenta*.
Come ulteriore protezione la scheda **può essere equipaggiata** con un sistema di [protezione dalle sovratensioni](https://github.com/TheFidax/TFX040_ROCO_EUROFIMA_2CLASSE_H0#protezione-sovratensioni-opzionale).

------------

## Assemblaggio
A causa delle ridotte dimensioni sul PCB **non sono presenti i nomi dei componenti**.

*Per conoscere nomi e ordine di Assemblaggio* fare riferimento al Progetto su CircuitMaker: https://workspace.circuitmaker.com/Projects/Details/luca-fidanza/Micro-RedLights-Module

*Nel caso in cui si volesse la scheda **gia' assemblata** e' possibile contattarmi all'indirizzo mail sottoriportato oppure seguire eventuali aste di pezzi in esubero sul mio profilo **Ebay***: https://www.ebay.it/sch/the_fidax/m.html

------------

## Contattami
Per curiosita' o ulteriori informazioni puo contattarmi al seguente indirizzo email:  	TheFidaxContactsAtgmail.com
