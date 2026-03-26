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
*detaillierte Auflistung einzelner Tätigkeiten*
- *Item 1*
- *Item 2 mit Hilfe von [1]

## Meine Learnings aus dieser Laboreinheit
*kurze Statements in ganzen Sätzen. Was war neu/schwierig/... . Wie bewerte ich meinen Erfolg*

## Quellen
*Alle verwendeten Quellen, auf die sich bei der Tätigkeitsbeschreibung auch bezogen werden soll, Beispiele siehe unten - diese sind vor der Abgabe zu entfernen*
[1] Dokumentation TwinCAT Echtzeit 
https://infosys.beckhoff.com/content/1031/tc3_grundlagen/17677068555.html?id=2118381968830183681
[2] HandbuchXY.pdf "Sicherung von XY-Geräten", Seite 32
https://xy.com/handbuchxy.pdf
[3] Youtube: Piotr Automaticov "TwinCAT für Dummies"

## KI-Quellen
*Hier könnt ihr verwendete KI-Prompts angeben, wenn sie wirklich nützlich waren - bitte sinnvoll kürzen - unten angegebene Beispiele bitte löschen*
### Claude (Sonnet 4.5):
Prompt: 
``` 
Zeige mir ...
```
Antwort: 
```
Bliblablub
```

Prompt: 
``` 
Ändere ...
```
Antwort: 
```
Blubblabli
```
### Gemini  ...
