# Übungsprotokoll Labor SYT - IndInf

*ALLE KURSIVEN Inhalte in dieser Vorlage müssen entweder entsprechend angepasst werden (z.B. Name des Projektes) oder vor Abgabe gelöscht werden (alle Hilfetexte)*

**Datum**: 26.03.2026
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
Zuerst haben wir uns das Förderband angeschaut und vom WAGO 750-354/000-001 die Dokumentation gesucht und gefunden. Die wichtigsten Klemmen dabei sind 750-601 (für die Stromversorgung), 750-614 (2-Kanal Analog-Eingang) und 750-430 (8-Kanal Digital-Eingang 24V). Dann haben wir die Ethernet-Kabel angeschlossen, eines von unserem Laptop in den Master (Port X100) und eines vom Master (Port X101) in den WAGO Slave (EtherCAT IN). Am Ende haben wir es geschafft, die Hardware in TwinCAT zu scannen, auch wenn wir am Anfang Verbindungsprobleme hatten.

Wir haben den Beckhoff CX9240 als Master mit dem WAGO-Koppler als Slave verbunden. Ein kleiner Fehler war am Anfang, dass wir das Kabel beim WAGO in "OUT" statt in "IN" gesteckt haben, weshalb der Scan nicht funktioniert hat. Am Laptop mussten wir eine feste IP-Adresse vergeben, damit wir den Beckhoff-Master überhaupt im Netzwerk finden konnten. Danach haben wir ein neues Projekt in der TwinCAT XAE Shell erstellt und das "Target System" auf den Beckhoff CX9240 umgestellt. 
Anschließend haben nach den EtherCAT-Geräten gesucht. Ein Problem war hier die fehlende orangefarbene Endklemme am WAGO-Block, weshalb der Bus erst eine Fehlermeldung ausgespuckt hat. Nachdem wir sie drangesteckt haben, wurde alles erkannt.


## Meine heutigen Tätigkeiten (detailliert)
Ich habe mithilfe meines Kolleges folgende Dinge erfüllt:
- *Physikalischer Aufbau & Verkabelung:* Anschluss des Laptops an den Master (Port X100) und Verbindung des Masters (Port X101) mit dem WAGO-Koppler (Port IN). Fehler den wir hatten war, dass wir zuerst das LAN-Kabel beim WAGO in den "OUT"-Port gesteckt. TwinCAT konnte den Slave so nicht finden. Erst nach dem Umstecken auf "IN" war der Koppler im Scan sichtbar.
- *Netzwerk-Konfiguration & Routing:* Einrichtung einer statischen IP-Adresse am Laptop (192.168.1.50), um mit dem Beckhoff CX zu kommunizieren. Über den TwinCAT Router haben wir per Broadcast Search die Route zum Master hinzugefügt.
- *TwinCat Erstellung, I/O-Konfiguration und Scan:* Anlegen eines neuen TwinCAT XAE Projekts. Wir hatten einen großen Fehelr, dass wir nicht Konfigurationsmodus starten konnten. Wir haben mithilfe KI es probiert aber vergebens.

Ohne Konfiguration: Durchführung eines Scans der E/A-Geräte. Hierbei wurden alle WAGO-Klemmen automatisch erkannt und im Projektbaum aufgelistet.

Nächstes mal Freerun.

## Meine Learnings aus dieser Laboreinheit
Ich habe gelernt, dass die Reihenfolge und der physische Anschluss bei EtherCAT entscheidend sind. Ein Kabel im falschen Port führt dazu, dass der gesamte Strang für den Master unsichtbar bleibt. Außerdem habe ich mich mit TwinCat beschäftigt (letztes Semester war ich dafür nicht verantwortlich). Ein wichtiges Learning war auch, dass der Feldbus physisch abgeschlossen sein muss. Ohne die orangefarbene Endklemme am letzten Slot des Slaves kann kein stabiler Datenfluss zustande kommen, und das System verweigert den Wechsel in den op state.

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
