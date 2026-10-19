# Prüfstand — 19. Oktober 2026

[English](VERIFICATION.md) · [Projekt](../README.de.md)

Die Ergebnisse stammen aus der lokalen Simulation. Testcode, vollständige Protokolle und maschinenlesbare Laufexporte bleiben privat. Diese Zusammenfassung berichtet über die ausgeführten Prüfungen; sie ist keine öffentlich reproduzierbare Quellcode-Veröffentlichung und keine unabhängige Zertifizierung.

## Automatisierte Prüfungen

**47 Tests bestanden, keiner fehlgeschlagen oder übersprungen**, unter Node 25.9.0 und Node 24.19.0. Typprüfung und Erstellung der gebauten Anwendung waren erfolgreich.

| Bereich | Was geprüft wurde |
| --- | --- |
| Robotergeometrie | Nominale Vorwärtskinematik gegen eine unabhängige UR5e-DH-Berechnung; Zielpositionen und Ausrichtungen; Gelenkgrenzen; Zurückweisung unerreichbarer Ziele |
| Bewegung | Gelenkgeschwindigkeit und Beschleunigung im Modell; gerade Werkzeugbahnen; Zielgenauigkeit; längere Dauer bei reduzierter Bewegungsrate mit unverändertem Ziel |
| Ablaufzustand | Pause hält Zeit, Zuführung und Bewegung an; ein einzelner Befehlsschritt; Bedienerhalt erhält den unterbrochenen Auftrag |
| Bauteilkennung | Eine Ablage pro Kennung; zugehöriges Prüfergebnis; Zuführereignisse bleiben dem neu eintreffenden Bauteil zugeordnet |
| Wiederaufnahme | Kamera-Timeout erhält das Bauteil; volles Fach verhindert Freigabe; nach dem Quittieren bleibt die Anlage bis zum ausdrücklichen Fortsetzen pausiert |
| Geometrieprüfung | Band- und Bauteilhöhe; Entnahme- und Ablageposition; Freiraum des geöffneten Greifers; getrennte Ablageplätze; sichtbare Prüffläche und fünf Sichtlinien |
| Gegenversuche | Zurückweisung einer Fahrt durch das Förderband; nach unten gerichtete Prüffläche besteht die Kameraprüfung nicht; fremdes Prüfergebnis sperrt die Sortierung |
| Wiederholbarkeit | Gleiche Ergebnisse bei unterschiedlichen Bildraten und Wiedergabegeschwindigkeiten; reproduzierbare Testwerte durch festen Seed |

Ein Lauf mit 24 Bauteilen prüfte die vorgesehenen Routen und ihre stichprobenweisen Baugruppenkontrollen. Außerdem wurden vier Szenarien mit jeweils zwölf Bauteilen abgeschlossen: Normalbetrieb, langsame Prüfung, Kamera-Timeout und volles Fach. Die beiden Störszenarien benötigten vor dem Abschluss die erwartete Behebung. Unerwartete Störungen traten in diesen Läufen nicht auf.

## Zeitvergleich

Beide Läufe nutzten zwölf Bauteile, einen Zuführversuch alle vier Sekunden, drei Pufferplätze, 100 Prozent Bewegungsrate und Seed 42. Der Simulationsschritt betrug 20 ms. Unterschiedlich waren nur Kameraverzögerung und zugehöriges Zeitlimit.

| Kennzahl | Normalbetrieb | Langsame Prüfung |
| --- | ---: | ---: |
| Kameraverzögerung / Zeitlimit | 1,1 / 2,5 s | 6,0 / 8,0 s |
| Chargenzeit einschließlich letzter Rückfahrt | 498,74 s | 557,58 s |
| Mittlere Zeit von Entnahme bis Ablage | 33,58 s | 38,48 s |
| Zuführung blockiert | 294,46 s | 333,70 s |
| Durchsatz: abgelegte Teile / Laufzeit | 86,62 Teile/h | 77,48 Teile/h |
| Ergebnis | 8 i. O., 1 Nachprüfung, 3 n. i. O. | 8 i. O., 1 Nachprüfung, 3 n. i. O. |
| Geprüfte Bewegungsbefehle | 144 | 144 |

Die mittlere Zeit von Entnahme bis Ablage enthält weder die leere Rückfahrt noch das Warten auf das nächste Teil. Sie ist daher nicht mit der vollständigen Zyklusdauer gleichzusetzen. Der Durchsatz wird aus der Charge berechnet und ist kein Anlagenkennwert für einen stationären Betrieb.

![Vollständige Browserläufe mit übereinstimmenden Modellergebnissen](../assets/comparison.jpg)

## Prüfung im Browser

Normalbetrieb und langsame Prüfung wurden im Browser mit den gleichen angezeigten Zeiten und Stückzahlen wie in der Ablaufberechnung abgeschlossen. Kamera-Timeout und die Wiederaufnahme nach einem vollen Fach wurden über die Oberfläche geprüft. Kontrolliert wurden außerdem Pause, Befehlsschritt, drei Perspektiven, Bauteilfilter, JSON-Export und eine 45-Sekunden-Videoaufnahme. Die Ansichten wurden bei 390, 768 und 1440 Pixel Breite geprüft. Die Seite hatte keinen horizontalen Überlauf; Navigation und Datentabellen können in ihren eigenen Bereichen scrollen.

## Grenzen

Die offizielle UR5e-Beschreibung liefert nominale Geometrie, keine Kalibrierung eines bestimmten Roboters. Scope Cell verwendet einen eigenen Interpolator und eine eigene inverse Kinematik. URScript-ähnlicher Text in einem Screenshot belegt keine Controller-Kompatibilität.

Die Kollisionsprüfung vergleicht konservative Hüllkörper von Roboter, Greifer und gehaltenem Bauteil an Stichproben entlang der Bahn mit der modellierten Geometrie von Förderband, Kamera, Schaltschrank und Fächern. Sie umfasst keine kontinuierliche Kollisionsprüfung, Eigenkollision des Roboters, sämtliche dekorativen Komponenten, verformbare Objekte, reale Kontaktdynamik oder sicherheitsgerichtetes Stoppen. Die Kameraprüfung kontrolliert Ausrichtung und fünf Sichtlinien, keine optische Kalibrierung oder Fehlererkennung. Die Qualitätswerte sind erzeugte Testdaten.

Es wurden weder ein physischer Roboter noch URSim, eine Produktionslinie oder ein Sicherheitssystem getestet. Bilder und Video belegen die lokale Anwendung, keine industrielle Inbetriebnahme.
