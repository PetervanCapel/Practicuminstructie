# Data-acquisitie

## Data-acquisitieystemen en functies

Bij een deel van de opstellingen bij het practicum wordt gebruik gemaakt van data-acquisitiesystemen. Er zijn twee systemen in gebruik: de commerciële Digilent Analog Discovery 2 (AD2) en de door de afdeling Instrumentatie speciaal voor het practicum ontwikkelde Julia-III. De belangrijkste functies:
- Het genereren van spanningen middels digitaal-analoog-conversie (DAC)
- Het meten van spanningen middels analoog-digitaal-conversie (ADC)
- Externe triggering
- Digitale invoer en uitvoer (Digital I/O)
- Het meten van tijdverschillen en frequenties en het tellen van impulsen (alleen op Julia-III).

Eerst volgt een uitleg over analoog-digitaal-conversie. Vervolgens bespreken we de functionaliteit van beide systemen.

## Analoog-digitaal-conversie

Fysische grootheden zijn nagenoeg altijd continue grootheden: zowel continu in de tijd als in de waarde, zie {numref}`fig_andata`. 

```{figure} figuren/andata.svg
:name: fig_andata

Continue waarde van de grootheid spanning als functie van de tijd.
```

Analoge meetapparatuur registreert de waarden ook continu, bijvoorbeeld met een wijzeruitslag op een schaalaanduiding. Praktische beperkingen bij analoge meetapparatuur zijn responstijd (er is enige tijd nodig is om het signaal te verwerken), handmatige aflezing en optekenen, en beperkte gevoeligheid en afleesnauwkeurigheid. Moderne meetprocessen zijn daarom vaak digitaal. Zie {numref}`fig_digitaal_meetproces`.

```{figure} figuren/digitaal_meetproces.svg
:name: fig_digitaal_meetproces

Meten, digitaliseren en opslaan of weergeven van een meting.
```

Vroeg in het proces wordt de waarde van een grootheid (temperatuur, massa, ..) direct of indirect geconverteerd naar een elektrische spanning. Op de vraag waarom juist een spanning zijn tenminste vier belangrijke redenen te bedenken:

1. Er zijn veel fysische processen waarbij de verandering van een grootheid direct of indirect leidt tot een elektrische spanning. Het object dat gevoelig is voor (de verandering van de waarde van) de grootheid noemen we een transducent[^transducent].
1.	Spanningen zijn via koperdraden eenvoudig te transporteren[^signaaltransport]. In het practicum maken we veel gebruik van eenaderige banaanstekkers en twee-aderige coaxkabels met BNC-aansluiting. Zie {numref}`fig_digitaal_meetproces`.
1.	Spanningen zijn via aan de computer verbonden acquisitiesystemen eenvoudig te digitaliseren en daarna digitaal op te slaan.
1.	Spanningen zijn met elektrische componenten eenvoudig te bewerken.

Vandaar dat je zonder overdrijving kunt stellen dat elke fysische meting uiteindelijk een spanningsmeting betreft.

```{figure} figuren/stekkers.svg
:name: fig_stekkers

Links: banaanstekkers. Rechts: coaxkabels met BNC-aansluiting.
```

Later in het proces treedt ook digitalisatie op: een analoge spanning wordt discreet gemaakt, zowel in de tijd - bemonstering of sampling - als in de waarde - kwantisatie, zie {numref}`fig_digdata`. Waarden worden daarna uitgedrukt in een binair getal. 

```{figure} figuren/digdata.svg
:name: fig_digdata

Digitaliseren van een analoog signaal.
```

We spreken van analoog-digitaal conversie, en het apparaat dat deze functie uitvoert heet een analoog-naar-digitaal converter (ADC). Digitale acquisitie en opslag verloopt vaak geautomatiseerd, wat de snelheid van het meetproces enorm verhoogt. 

Belangrijke eigenschappen van een ADC, geïllustreerd met de eigenschappen van de AD2: 
1. Het bereik: de minimale en maximale spanning die de ADC kan meten ($V_{min}$ respectievelijk $V_{max}$). De AD2 heeft twee ADC-bereiken. High_Gain heeft een bereik ±2.75 V, dus totaal 5.5 V, en voldoet voor de meeste doeleinden in dit practicum. Low_Gain heeft heeft een bereik van rond de $\pm$ 30 V (60 V totaal). De precieze waarden worden in het meetprogramma weergegeven; ze verschillen licht per AD2. Het bereik wordt automatisch gekozen in Waveforms.
1. Aantal bits $n$: bij $n$ bits kunnen $2^n$ spanningswaarden worden gemeten. Voor beide ADC-bereiken van de AD2 is het aantal bits gelijk aan 14, wat $2^{14}=16384$ stappen toelaat.
1. De resolutie die hiervan het gevolg is: $\Delta V = (V_{max} - V_{min})/2^n$. Voor High_Gain is dat ongeveer 0.34 mV, voor Low_Gain ongeveer 3.7 mV.

Natuurlijk wil je in experimenten dat de meetwaarden binnen het bereik vallen en tegelijk een (veel) grotere variatie kennen dan de resolutie van een ADC-spanningstap. 

## Specificaties van de AD2

Een volledig overzicht van alle eigenschappen van de AD2 is te vinden op 
- [http://nspracticum.science.uu.nl/DATA2026/DATA-E/Handleidingen/AnalogDiscovery2NIDatasheet.pdf](http://nspracticum.science.uu.nl/DATA2026/DATA-E/Handleidingen/AnalogDiscovery2NIDatasheet.pdf)

Een zeer uitgebreide beschrijving van mogelijkheden en interne systemen is te vinden via
- [https://digilent.com/reference/test-and-measurement/analog-discovery-2/reference-manual](https://digilent.com/reference/test-and-measurement/analog-discovery-2/reference-manual)

### Algemeen

```{figure} figuren/AD2.svg
:name: fig_AD2

Bovenaanzicht van de Digilent Analog Discovery 2 (AD2) met uitbreidingsbord (onder).
```

{numref}`fig_AD2` is een bovenaanzicht van de AD2. De AD2 wordt met een micro-usb naar een usb-kabel aangesloten op de computer. De verbinding van de AD2 met de computer fungeert ook als voeding. Voor taken die veel vermogen eisen, is een externe voeding beschikbaar die verbonden wordt met het stopcontact. Het afgedekte deel is de eigenlijke AD2; het daarop aangesloten bordje geeft o.a. toegang tot twee ADC-kanalen (CH1 en CH2) en twee DAC-kanalen (W1 en W2) via BNC-verbindingen. Om de AD2 via een pc te gebruiken moet op de pc de applicatie Waveforms met daarin de benodigde drivers zijn geïnstalleerd.

De AD2 heeft een 30-pins connector (zie {numref}`fig_AD2-pins`) met alle analoge en digitale I/O-kanalen en daarnaast enkele aardes (GND), voedingen (V$+$ en V$-$) en twee triggers (T1 en T2). 

### Analoge input
De analoge ingangen (ADC's) $1+$ en $1-$ resp. $2+$ en $2-$ kunnen via Waveforms gebruikt worden als oscilloscoop-ingangen. Eigenschappen van de ADC's:
- 14-bit, met bereiken ongeveer $\pm 2.5$ V en ongeveer $\pm 30$ V en beveiligd tot $\pm 50$ V. 
- 100 MHz maximale samplefrequentie, waarmee frequenties tot 9 MHz (30 MHz via uitbreidingsbord) kunnen worden gemeten. 
- Ingangsweerstand 1 M$\omega$, parasitaire capaciteit 24 pF. 
- Omdat 1/2$+$ en 1/2$-$ niet gekoppeld zijn aan de aarde is zonder het uitbreidingsbord zogenaamde differentiële meting mogelijk (in tegenstelling tot bij een gewone oscilloscoop). Het wordt wel aangeraden om GND in de te bemeten circuits door te verbinden aan de GND van de AD2.
- Via jumpers op het uitbreidingsbord kan gekozen worden voor DC- of AC-meting (afkapfrequentie 10 Hz); standaard staat deze op DC.

```{figure} figuren/analogdiscovery2-pinout-600.svg
:name: fig_AD2-pins

Definitie van de 30-pins uitgang van de AD2. De ingangen $1+$ en $1-$ resp. $2+$ en $2-$ zijn doorverbonden naar de BNC-connectoren CH1 resp. CH2 op het uitbreidingsbord. De uitgangen W1 en W2 zijn via de BNC-connectoren ook via het uitbreidingsbord beschikbaar.
```

### Analoge output
De analoge uitgangen (DAC's) W1 en W2 kunnen via Waveforms gebruikt worden als toongenerator. Hiervoor is wel een GND-referentie vereist. Eigenschappen van de DAC's:
- Eveneens 14-bit, met een bereik tot $\pm$5 V. 
- 100 MHz maximale samplefrequentie, waarmee frequenties tot 9 MHz kunnen worden gemaakt. Via Waveforms zijn standaardvormen als sinus, driehoek, blok etc. beschikbaar. 
- Verder is het ook mogelijk om door de gebruiker gedefinieerde golfvormen te maken; hiervoor is een buffer van maximaal 65536 waarden beschikbaar en een maximale updatefrequentie van 2 MHz; de beide maxima zijn niet tegelijk van toepassing.   
- Via jumpers op het uitbreidingsbord kan gekozen worden voor uitgangsweerstand 0 $\Omega$ of 50 $\Omega$; standaard staat deze op 50 $\Omega$.

### Digitale I/O functies
Digitale kanalen worden gebruikt voor Er zijn 16 digitale I/O-kanalen, die toegankelijk worden gemaakt via het JI-Extensiebord (zie hierna). Wanneer ze als input gebruikt worden, dan zijn ze bedoeld voor 3.3 V (5 V is toegestaan). Gebruikt als output zijn ze bedoeld voor 3.3 V. De digitale kanalen kunnen ook worden gedefinieerd en aangestuurd via Waveforms, en bijvoorbeeld gebruikt worden voor digitale communicatieprotocollen.  

De digitale Out kan genereren: 
- Een blokvorm-generator instelbaar met 2 opgegeven tijden (lowTime, highTime, in veelvouden van 10 ns)
- Een digitale patroon-generator (maximaal 131072 waarden, maximale herhaalfrequentie 50 MHz) die bijvoorbeeld kan dienen als (digitale) ruisgenerator.

### Overige functies
De AD2 kan extern getriggerd worden op twee kanalen, bijvoorbeeld om de acquisitie via de ADC-kanalen op het juiste moment te starten. Triggeren vindt plaats op een aangeboden spanning van 3.3 V. 

Daarnaast heeft de AD2 twee voedingskanalen V$+$ en V$-$ die tot $\pm$5 V en 500 mW vermogen kunnen voorzien als de AD2 via usb wordt gevoed. 

### JI-Extension Board (JI-EB)

Om meer functies voor de gebruikers beschikbaar te maken, is een extensiebord voor de AD2 ontwikkeld. Dit wordt een groot breadboard waarop alle 30 kanalen van de AD2 aan te sluiten zijn via jumper wires, waar elektrische schakelingen op gebouwd kunnen worden en waar via BNC op in- en uitgekoppeld kan worden. 

## Waveforms

In het practicum wordt meestal wordt gebruik gemaakt van een door de technici van het practicum geschreven acquisitieprogramma. De AD2 kan echter worden aangestuurd en uitgelezen via het meegeleverde programma WaveForms. Dit veelzijdige programma kun je als student gebruiken om zelf acquisitie te plegen of aansturing te ontwikkelen, als alternatief voor werken via oscilloscopen en toongeneratoren. WaveForms staat op de desktops in de practicumzalen en kan naar keuze ook op de laptop geïnstalleerd worden. Er staan installers op Brightspace[^Waveforms]. 

Wanneer de AD2 met een computer verbonden is, staat deze standaard aan. Via de kabel kunnen vanuit de computer 'opdrachten' gegeven worden en (de andere kant op) meetwaarden naar de computer gaan. Via Waveforms kun je metingen direct op de pc of laptop tonen en in databestanden vastleggen. 

Meer informatie over Waveforms:
- [https://digilent.com/reference/software/waveforms/waveforms-3/start](https://digilent.com/reference/software/waveforms/waveforms-3/start)

## Specificaties van Julia-III

### Algemeen
Julia-III moet allereerst op de netspanning zijn aangesloten. Om Julia-III via een pc te gebruiken moet op de pc de benodigde driver zijn geïnstalleerd, en er moet via een USB-kabel een  verbinding zijn. De beschikbare functies zijn:
- 8 langzame ADC's (Slow ADC) om langzaam variërende analoge spanningen te meten, desgewenst extern triggerbaar. 
- 2 snelle ADC's (Fast ADC) om snel variërende analoge spanningen te meten, desgewenst extern triggerbaar.
- 4 DAC's die het mogelijk maken vanuit de pc regelbare spanningen uit te voeren
- Tijdsinterval/frequentie meter, pulsteller (Puls count)
- Blokspanningsgenerator (Blok out)
- Digitale signaal in- en uitvoermogelijkheden.

```{figure} figuren/juliaIII_nieuw.svg
:name: JuliaIII

Front-paneel van Julia-III.
```

In {numref}`JuliaIII` is het frontpaneel van Julia-III weergegeven. Een viertal groene LEDs geeft informatie over de operationele status, n.l. POWER
(Julia-III is voorzien van spanning), USB (USB verbinding met pc is aanwezig), ANALOG STATUS en DIGITAL STATUS (geen situatie van overbelasting). De laatste twee indicatoren zijn belangrijk voor de beveiliging tegen beschadiging. Hoewel de in- en uitgangen van Julia-III (beperkt) zijn beveiligd tegen overspanning respectievelijk kortsluiting moet wanneer een beveiligings-led uit staat zo snel mogelijk de kennelijke overbelasting worden verholpen door de betreffende verbinding te ontkoppelen.

### Analoge functies op ANALOG IO
Een groot deel van de analoge functionaliteit van Julia-III is beschikbaar via de 37-pins ANALOG IO DSUB connector. Deze functies zijn langzame ADC en DAC (de snelle ADC is alleen toegankelijk via de BNC-connectoren op het frontpaneel).


```{figure} figuren/JuliaIII_breakout.svg
:name: JuliaIIIbreakout

BREAKOUT BOX.
```

Julia-III bevat 8 langzame ADC-kanalen met elk een resolutie van 14 bits. De ingangen zijn zowel differentieel als single-ended (d.w.z. t.o.v. aarde) te gebruiken. Simultaan kunnen 8 spanningen worden gemeten. 

Voor de toegankelijkheid kan een Julia-III BREAKOUT BOX op de ANALOG IO worden aangesloten (zie {numref}`JuliaIIIbreakout`). Op de BREAKOUT BOX zijn o.m. vier langzame ADC-kanalen beschikbaar (SADC1, \ldots, SADC4) met triggeringang (TTL, opgaande flank). Per SADC-kanaal is er een programmeerbare versterker opgenomen die de ingangsspanning 1, 10, of 100 keer kan versterken voor digitalisatie. Bij versterking 1 is het spanningsgebied $-10$ V tot $+10$ V. Bij de andere versterkingen wordt dit bereik evenredig kleiner. De toelaatbare samplefrequenties zijn afhankelijk van het aantal kanalen waarop simultaan wordt gemeten met een maximum van ongeveer 250 kHz.

Julia-III bevat 4 onafhankelijk aan te sturen DAC's met een resolutie van 16 bit en een bereik van $-10$ V tot $+10$ V. Deze uitgangspanningen zijn ook beschikbaar via de BNC aansluitingen (DAC1, \ldots, DAC4) op de BREAKOUT BOX.

Julia-III bevat 2 snelle ADC-kanalen met elk een resolutie van 12 bits en een bereik van $-1$ V tot $+1$ V bij een samplefrequentie tot 8 MHz. Het is mogelijk simultaan op beide kanalen te meten. Invoer is via de BNC's (FADC1, FADC2) op het frontpaneel. 

Triggering vindt plaats op de Trigger ingang (TTL, opgaande flank).

### Digitale functies op DIGITAL IO
De digitale IO interface bevat zestien kanalen elk te configureren als TTL-ingang of TTL-uitgang. Deze kanalen zijn beschikbaar op de 25-polige DSUB connector op het
frontpaneel (DIGITAL IO). Een deel van deze functionaliteit (timer/counter functie) is ook op het frontpaneel toegankelijk via BNC-aansluitingen op het frontpaneel.

De BNC-ingang Dig-In op het frontpaneel van Julia dient als gezamenlijke ingang voor meerdere functies:
- het tellen van spanningpulsen (counter functie),
- het meten van tijdsintervallen tussen twee pulsen (timer functie),
- het meten van frequentie.

Voor alle functies is de ingang van het type TTL. De BNC-uitgang Dig-Out op het frontpaneel is een TTL uitgang voor een programmeerbare blokgenerator (bereik van
0.5 mHz tot 2 MHz).

[^transducent] Een transducent is algemeen gedefinieerd als een object dat één vorm van energie omzet in een andere. In de context van dit vak zullen we altijd aannemen dat er sprake is van omzetting in elektrische energie.
[^signaaltransport] Voor transport van signalen worden tegenwoordig ook optische (licht-)signalen gebruikt, denk aan optische fibers voor snelle datanetwerken.
[^Waveforms] Je kunt ook bij Digilent een account aanmaken en daar de installer downloaden. De nieuwste versies van Waveforms zijn echter gebouwd voor de opvolger van de AD2, de AD3, en het kan zijn dat niet alle functies nog goed werken. Gebruik voor een account je UU-mailadres. Zie [https://cloud.digilent.com/login](https://cloud.digilent.com/login)

