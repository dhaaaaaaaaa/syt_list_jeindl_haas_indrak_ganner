# Übungsprotokoll Labor SYT - Haas


**Datum**: 09.04.2026
**Schülername**: Dominik Haas
**Klasse**: 5CHIT
**Meine Gruppenkollegen**: Jakob Jeindl (5DHIT)

## Name des Projekts
Ziel des Projektes ist die Implementierung einer Fernsteuerung für eine industrielle Ampelanlage. Dabei fungiert ein Windows-Laptop als Master-Steuerung, die über das EtherCAT-Protokoll Befehle an einen Beckhoff-Koppler den Slave sendet.
## Verwendete Geräte 
- *Beckhoff CX9240 "Vanguard":* Der zentrale EtherCAT-Koppler, der als Schnittstelle zwischen Netzwerk und Hardware dient.
- *Beckhoff PS1001 Power Supply:* Versorgt die Steuerung und die Ampel mit der notwendigen 24V-Feldspannung.
- *Windows Laptop:* Dient als Programmierumgebung und Master für die Steuerung.
- *Industrielle Ampel:* Signalsäule mit drei Leuchtelementen (Rot, Gelb, Grün).
- *Digitale Ausgangsklemmen (EL-Module):* Die schmalen Module neben der CPU, an denen die Ampeldrähte klemmen.
- *Ethernet-Kabel (RJ45):* Zur physischen Verbindung zwischen Laptop und CX9240.
- *Software:* TwinCAT 3 (Engineering Umgebung) oder alternativ Python mit der Library pysoem.

## Zusammenfassung des heutigen Erfolges
Wir haben den physikalischen Aufbau abgeschlossen und den Laptop direkt mit dem WAGO-Koppler verbunden. Ein großes Problem war die Inkompatibilität der eingebauten Realtek-Netzwerkkarte des Laptops mit den TwinCAT-Echtzeit-Treibern. Auf Anweisung des Lehrers haben wir versucht, dieses Problem mit "Andy’s Package" zu lösen, um die Karte für TwinCAT "sichtbar" zu machen.

Obwohl wir die Installation durchgeführt und verschiedene Konfigurationen getestet haben, blieb der Erfolg aus: Der Laptop konnte keine stabile Echtzeit-Verbindung aufbauen. Wir konnten zwar die Hardware im Projektbaum sehen, aber kein "Free Run" oder "Config Mode" war stabil möglich. Damit haben wir wertvolle Erkenntnisse über die Hardware-Limitierungen von Consumer-Laptops im industriellen Umfeld gewonnen.


## Meine heutigen Tätigkeiten (detailliert)
Ich habe mithilfe meines Kolleges folgende Dinge erfüllt:
- *Physikalischer Aufbau & Verkabelung:* Anschluss des Laptops an den Master (Port X100) und Verbindung des Masters (Port X101) mit dem WAGO-Koppler (Port IN). Fehler den wir hatten war, dass wir zuerst das LAN-Kabel beim WAGO in den "OUT"-Port gesteckt. TwinCAT konnte den Slave so nicht finden. Erst nach dem Umstecken auf "IN" war der Koppler im Scan sichtbar.
- *Netzwerk-Konfiguration & Routing:* Einrichtung einer statischen IP-Adresse am Laptop (192.168.1.50), um mit dem Beckhoff CX zu kommunizieren. Über den TwinCAT Router haben wir per Broadcast Search die Route zum Master hinzugefügt.
- *TwinCat Erstellung, I/O-Konfiguration und Scan:* Anlegen eines neuen TwinCAT XAE Projekts. Wir hatten einen großen Fehelr, dass wir nicht Konfigurationsmodus starten konnten. Wir haben mithilfe KI es probiert aber vergebens.

Ohne Konfiguration: Durchführung eines Scans der E/A-Geräte. Hierbei wurden alle WAGO-Klemmen automatisch erkannt und im Projektbaum aufgelistet.

Nächstes mal Freerun.

## Meine Learnings aus dieser Laboreinheit
Verkabelung des Laptops direkt mit dem WAGO-Koppler (Port IN). Wir haben den Umweg über das Labornetzwerk vermieden, um Latenzen zu minimieren.
Da der Standard-TwinCAT-Treiber die Realtek-Karte als "incompatible" markierte, haben wir nach Rücksprache mit dem Lehrer das Modul "Andy’s Package" installiert. Ziel war es, die Hardware-ID der Netzwerkkarte so zu manipulieren, dass TwinCAT sie als echtzeitfähigen Adapter akzeptiert.
Trotz der Installation von "Andy's Package" traten weiterhin Bluescreens und Timeout-Fehler auf. Wir haben folgende Dinge versucht:

- Die Energiesparoptionen der Netzwerkkarte zu deaktivieren.

- Den Treiber manuell über den Gerätemanager zu erzwingen.

- Die Virtualisierung im BIOS (Hyper-V Konflikt) auszuschalten.

- Letztendlich ließ sich der Laptop nicht dazu bewegen, Pakete im erforderlichen Echtzeit-Intervall zu senden. Der Scan fand zwar zeitweise den Koppler, brach aber sofort mit einem "Init to Preop" Fehler ab.



## Quellen
*Alle verwendeten Quellen, auf die sich bei der Tätigkeitsbeschreibung auch bezogen werden soll, Beispiele siehe unten - diese sind vor der Abgabe zu entfernen*
[1] WAGO 750-354 EtherCAT-Koppler (Datenblatt/Manual):
[https://infosys.beckhoff.com/content/1031/tc3_grundlagen/17677068555.html?id=2118381968830183681](https://www.safetycontrol.ind.br/imgs/downloads/m07500354_00000000_0en-pdf-5973f23a0ad49.pdf)
[2] Beckhoff CX9240 Embedded PC (Produktbeschreibung):
[https://xy.com/handbuchxy.pdf](https://automationitems.com/beckhoff/hmis-industrialpcs/embedded-pcs/cx9240)
[3] Beckhoff Netzteil PS1011-2410-0000 (ähnlich PS1001, 24 V):
https://infosys.beckhoff.com/content/1033/ps1011-2410-0000/9071005323.html

## KI-Quellen
### Claude (Sonnet 4.5):
Prompt 1: 
``` 
Ich habe einen Beckhoff CX9240 als Master und einen WAGO 750-354 als Slave. 
Erkläre mir die Slot-Belegung laut Foto und wie ich das Förderband in TwinCAT finde.
```
Antwort: 
```
Basierend auf dem Foto ist Slot 3/5 für Eingänge (Sensoren) und Slot 6/7 für 
Ausgänge (Motor) zuständig. Nutze den 'Free Run' Modus in TwinCAT, um über 
den Reiter 'Online' die Ausgänge von Terminal 6 manuell zu forcen (Wert 1), 
bis sich das Band bewegt.
```

Prompt 2: 
``` 
Mein TwinCAT Scan findet den WAGO-Koppler nicht, obwohl das LAN-Kabel steckt. 
Was könnten typische Hardware-Fehler in einem Industrie-Labor sein?
```
Antwort: 
```
Häufige Fehler sind: 1. Kabel im 'OUT' statt 'IN' Port am Slave. 2. Fehlende 
orangefarbene Endklemme (750-600) am Ende des Bus-Segments. 3. Laptop hat 
keine feste IP im selben Subnetz wie der Beckhoff-Master (z.B. 192.168.1.x).
```
Prompt 3: 
``` 
Fasse mir die wichtigsten Schritte der beigefügten PDF-Anleitung für die 
Inbetriebnahme einer Förderband-Steuerung mit TwinCAT XAE Shell zusammen.
```
Antwort: 
```
1. Echtzeit-Treiber installieren. 2. Neues XAE-Projekt anlegen. 3. Zielsystem 
(Target) auf CX9240 setzen. 4. I/O Scan durchführen und Boxen bestätigen. 
5. Im Free Run Modus die I/O-Zuordnung (Mapping) durch manuelles Schalten 
der Ausgänge verifizieren.
```
