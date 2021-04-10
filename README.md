# Micro Modulo per Lanterne di Coda FS
Questo micro modulo e' pensato per dotare i modelli in scala delle tipiche *lanterne di coda* usate per segnalamento luminoso di fine convoglio.

**Ultima Revisione HardWare: 1.00**

**Ultima Revisione SoftWare: 1.00**

**Alcune Immagini Dimostrative:**

- Carrozza in condizioni luminose *Diurne* con tutti i LED bianchi accesi

<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/diurna_bianchi.jpg" width="1280">

- Carrozza in condizione luminose *Diurne* con *corridoio* illuminato e *compartimenti con luci blu*.

<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/diurna_blu.jpg" width="1280">

- Carrozza in condizioni luminose *Notturne* con tutti i LED bianchi accesi 

<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/notturna_bianchi.jpg" width="1280">

- Carrozza in condizione luminose *Notturne* con *corridoio* illuminato e *compartimenti con luci blu*.

<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/notturna_blu.jpg" width="1280"></br>

- Scheda nella sua posizione appoggiata sopra gli interni:

<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/scheda_posizionata.jpg" width="1280"></br>

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
  - [Predisposizione Condensatore PowerPack](#predisposizione-condensatore-powerpack)
  - [ATtiny10](#attiny10)
  - [Resistori LED incorporati](#resistori-led-incorporati)
* [Assemblaggio](#Assemblaggio)
* [Contattami](#Contattami)

------------


## Video Presentazione del Progetto
[![Video Presentazione](https://img.youtube.com/vi/kVVs_6TyThg/0.jpg)](http://www.youtube.com/watch?v=kVVs_6TyThg)

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
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/mcp16331.jpg" width="1280">

L'alimentazione a 5 volt e' fornita dal chip [Microchip MCP16331](https://www.microchip.com/wwwproducts/en/MCP16331), un regolatore di tensione di tipo [Step Down Buck](https://it.wikipedia.org/wiki/Convertitore_buck) in gradi di ricevere in ingresso tensioni fino a 50 volt e di fornire in uscita una tensione stabile a 5 volt con sviluppo di calore minimo.</br>

------------

### Condensatori PowerPack
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/powerpack.jpg" width="1280">

Per sopperire a problemi di captazione di corrente e' previsto un sistema *powerpack* formato da 4 condensatori al Tantalio da 100uF con tensione **massima** di 25 volt.</br>
I condensatori sono separati dal circuito di alimentazione da un Diodo ed un Resistore che rappresentano *il sistema di ricarica lenta*.
Come ulteriore protezione la scheda **può essere equipaggiata** con un sistema di [protezione dalle sovratensioni](https://github.com/TheFidax/TFX040_ROCO_EUROFIMA_2CLASSE_H0#protezione-sovratensioni-opzionale).

------------

### Protezione Sovratensioni (Opzionale)
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/ltc4367.jpg" width="1280">

Questa protezione **e' opzionale**: la sua presenza **e' segnalata dal Jumper J1 *aperto***. (**N.B.** Nella revisione HardWare 1.00 il Jumper J1 **e' assente**, e' stato introdotto dalla versione 1.01)</br>
Le normative [NMRA](https://www.nmra.org/sites/default/files/standards/sandrp/pdf/s-9.1_electrical_standards_2020.pdf) e [NEM](https://morop.org/downloads/nem/fr/nem670_f.pdf) **impongono** che un decoder digitali *supportino* tensioni fino a 27 volt.</br>
L'utilizzo di condensatori a 25 volt *non e' a norma*. **Tuttavia** le stesse normative impongono che la centrale fornisca tensione *massima* di 22 volt.</br>
Pertanto in condizioni di *funzionamento corretto* i condensatori non riceveranno tensioni che possano danneggiarli.

Gli stessi produttori commerciali *consigliano* condensatori da 25 volt:
<img src="https://images.beneluxspoor.net/bnls/aansluitschema_lokpilot_v3.jpg" width="1280">

**In caso di impiego della Scheda su sistemi a Corrente Alternata Analogica questa protezione E' OBBLIGATORIA!** 

Il sistema di protezione si basa sul chip [LTC4367](https://www.analog.com/en/products/ltc4367.html) che **isola** mediante *MOSFET* i condensatori in caso di tensioni *superiori a 23,7 volt*; quando la tensione scende sotto questa soglia i condensatori torneranno alimentati.</br>

------------

### Microchip ATmega128A
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/atmega.jpg" width="1280">

Il *cervello* della scheda e' un microcontrollore [ATmega128A](https://www.microchip.com/wwwproducts/en/ATmega128A) a 64 pin operante a 5 volt con frequenza di 16MHz tramite cristallo esterno.</br>
Il microcontrollore comanda *in maniera indipendente* tutti (leggere NOTA) i LED, in modo tale da garantire la massima flessibilita' di funzionamento.

*NOTA*: Il corridoio e' progettato con *5 segmenti* da 3 led ognuno: il microcontrollore comanda il segmento e **non** il singolo LED.

Il chip e' programmabile **anche** utilizzando l'**Arduino IDE** (mediante *programmatore ISP*) tramite il [MegaCore](https://github.com/MCUdude/MegaCore).</br>

I valori dei **FUSE** sono i seguenti: 
- LOW: 0x3F
- High: 0xC7
- Extended: 0xFF
- LOCKBITS: 0x3F

Il microcontrollore **non può** essere dotato di *bootloader*: non e' presente una porta Seriale.

------------

### Lettura Segnale Digitale
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/dcc.jpg" width="1280">

Il segnale digitale e' letto mediante [Optoisolatore TLP2168](https://toshiba.semicon-storage.com/eu/semiconductor/product/optoelectronics/detail.TLP2168.html) che fornisce l'[isolamento galvanico](https://it.wikipedia.org/wiki/Isolamento_elettrico) del microcontrollore dalla tensione delle rotaie.</br> 
L'optoisolatore e' protetto dall'inversione di corrente mediante Diodo e da un eccessiva corrente mediante resistore.</br>
Tale chip e' bicanale e permette, inoltre, il rilevamento della presenza di un Decoder Esterno analizzando la linea U+ fornita da esso.</br>
Questo sistema **e' compatibile con il DCC e con il Motorola**.

------------

### Sistema ACK
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/ack.jpg" width="1280">

La scheda e' dotata di sistema per fornire l'ACK nella **programmazione DCC mediante binario di programmazione**.

------------

### Porta di Programmazione ISP
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/isp.jpg" width="1280">

Per programmare il microcontrollore e' presente *una porta di programmazione ISP* con connettore JST SH6 per prevenire connessioni invertite.</br>
Questa porta svolge la doppia funzione di **Porta ISP** e **Porta I2C** mediante il seguente schema:</br>

<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/isp_i2c.jpg" width="1280">

------------

### Illuminazione Compartimenti con Luci Diurne e Notturne
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/luci_compartimenti.jpg" width="1280">

Questa scheda fornisce, per ogni compartimento, la **doppia illuminazione**: *Diurna* con LED Bianco Freddo e *Notturna* con LED Blu.</br>
Ogni LED e' indipendente e puo' essere pilotato dal microcontrollore in maniera indipendente dall'altro.</br>

------------

### Luci di Coda Rosse
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/luci_coda_pad.jpg" width="1280">
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/luci_coda_npn.jpg" width="1280">

Su entrambi i lati sono presenti le connessioni per delle **Luci di Coda Rosse**.</br>
Le connessioni prevedere un *polo positivo* collegato alla linea a **5 volt** e un *polo negativo* pilotato dal micro tramite transistor.

------------

### Interfaccia PluX
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/plux.jpg" width="1280">

E' presente un **connettore PluX** per poter utilizzare decoder commerciali.</br>
Durante il funzionamento con Decoder, la scheda di illuminazione diventa un [Modulo SUSI](https://github.com/TheFidax/Rcn600), questa configurazione diventa necessario se e' richiesto un protocollo *proprietario* non decodifcicabile dalla scheda.</br>
Protocolli come:
- [MFX/M4](https://en.wikipedia.org/wiki/M%C3%A4rklin_Digital)
- [RailCom Plus](http://www.esu.eu/en/support/white-papers/railcomplusr/) 

Il connettore PluX fornisce il collegamento alle rotaie (per portare i comandi al Decoder), il [Bus SUSI](https://dccwiki.com/SUSI) (per utilizzare la scheda come modulo SUSI) e il collegamento per un altoparlante (in caso di decoder sonori: es. [ESU LokSound v5 FX](http://www.esu.eu/en/products/loksound/loksound-5-fx/)).

**ATTENZIONE! Non è previsto un "motore fittizio" pertanto in caso di decoder "per locomotive" la *Lettura/Scrittura* delle CVs e' permessa soltanto tramite *PoM!***

------------

### Porta SUSI
<img src="https://github.com/TheFidax/TFX062_ACME_UICX_GIUBILEO_2CLASSE_H0/blob/main/Images/susi.jpg" width="1280">

Per garantire la massima personalizzazione, e' presente anche **una porta SUSI** per eventuali moduli SUSI esterni.

------------

## Assemblaggio
Il file *BoM* comprende la lista di tutti i pezzi necessario all'assemblaggio in proprio della scheda.</br>
Per esperienza personale consiglio di cominciare l'assemblaggio dal Chip **LTC4367** in quanto il più ostico da saldare a mano; nel caso in cui questo chip *non fosse richiesto* allora è consigliabile cominciare dal *Microcontrollore ATmega128A*.

I successivi pezzi possono essere saldati seguendo un ordine a scelta dell'utente.

*Nel caso in cui si volesse la scheda **gia' assemblata** e' possibile contattarmi all'indirizzo mail sottoriportato oppure seguire eventuali aste di pezzi in esubero sul mio profilo **Ebay***: https://www.ebay.it/sch/the_fidax/m.html

------------

## Contattami
Per curiosita' o ulteriori informazioni puo contattarmi al seguente indirizzo email:  	TheFidaxContactsAtgmail.com
