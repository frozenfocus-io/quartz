---
title: ESC Working Principle
tags: []
date: 2026-03-13
---

## 1. Was macht ein ESC?

Ein ESC (Electronic Speed Controller) ist sozusagen der Mittelmann zwischen dem Controller und den Motoren. Der Controller liefert die leistungsarmen Steuersignale. Diese Signale sind aber zu schwach um etwas antreiben zu können. Das ESC nimmt diese Signale und verstärkt diese um den Motoren die Große Leistung zu geben die sie brauchen um zu funktionieren. 

Es gibt viele Protokolle die verwendet werden können um die Throttle Information von der Steuerung zu dem ESC zu bringen. Die häufigsten Protokolle sind PWM (Pulsweitenmodulation), Oneshot, Multishot und Dshot. 
Der größte Unterscheid zwischen den Protokollen die Frequenz mit denen die Signale gesendet werden. Kürzere Frequenzen erlauben ein schnelleres Signal und damit eine schnellere Reaktion der Motoren. Das Dshot Protokoll ist da anders. Dieses Protokoll verwendet keine analogen Signale sondern digitale. Das macht das Signal stabiler da digitale Signale nicht so anfällig für elektrische Störungen sind. Außerdem ist Dshot präziser mit höherer Auflösung.

## 2. Komponenten eines ESCs

#### 1. MCU (Microcontroller)
Die Aufgaben des Mikrocontrollers können in drei Überpunkte unterteilt werden:
	1. Firmware
		Auf dem MC ist die Firmware gespeichert die die Signale vom Controller interpretiert und diese in einen Regelkreis einpflegt.
	2. Positionsüberwachung
		Der MC überwacht ständig die Positionen der Motoren um eine glatte Beschleunigung zu gewährleisten.
	3. MOSFET Treiber steuern
		Der MC sendet die Steuersignale an den MOSFET Treiber der wiederum die Signale zum schalten der MOSFETs sendet.

Die Firmware in ESCs ist oft vom Hersteller vorinstalliert. Es können auch open-source Firmwares gefunden werden. Üblicherweise werden ESCs im Drohnenbereich mit einer Iteration von BLHeli (BLHeli_S, BLHeli_32) versendet. Es gibt auch andere Firmwares wie SimonK und KISS. **Die gewählte Firmware muss jedoch mit der verbauten Hardware zusammenpassen.**

Der Mikrocontroller überwacht ständig die Position von den Rotoren der Motoren. Bei Anwendungen mit niedrigen Drehzahlen (Bodenfahrzeuge) werden dafür Sensoren (Hall-Effect) verwendet. Im Bereich der Drohnen wird aber eine Technologie namens *back EMF* verwendet. Diese Technologie funktioniert sehr gut bei hohen Geschwindigkeiten.

#### 2. Gate Treiber
Der Gate Treiber hat die Aufgabe innen, die schwachen Signale zur steuerung der MOSFETs des MCU zu verstärken um die MOSFETs sauber schalten zu können. Er ist sozusagen die MIttelfrau zwischen MCU und MOSFET. Der Treiber hat einen geringeren Innenwiderstand und kann somit schneller hohe Spannungen schalten als der MCU. Das hämmt außerdem die Wärmeentwicklung.

#### 3. MOSFET
MOSFETs (Metal Oxide Semiconductor Field Effect Transistors) sind die Schalter welche geplant den Motor mit Strom versorgen. Ein ESC für einen motor besteht aus 6 MOSFETs. Je zwei für eine Phase des Motors. Die MOSFETs bekommen die Signale vom MCU verstärkt über den Gate Treiber. Der MOSFET muss drei Phasen Schalten, diese sind: hohe Spannung, niedrige Spannung, keine Spannung (ground). 

So sich der Motor dreht müssen die MOSFETs die drei Phasen des Motors konstant schalten. Das ESC verwendet DC und mithilfe der Schalterkonfiguration von 6 MOSFETs kann eine 3-phasige Wechselspannung erzeugt werden. (siehe Bild)

![[anordung_mosfet.jpg]]

Umso höher das Gas gedrückt wird umso schneller wird die Schaltfrequenz der MOSFETs, das führt zu einer höheren Drehzahl der Motoren. 
Es gibt verschiedenste Signalübertragungsprotokolle, alle mit anderen Leisungen und anderen Signalfrequenzen. 

#### 4. Battery Eliminator Circuit (BEC)
ESCs haben meistens auch eine Schaltung um die Akkuspannung auf etwa 5V zu reduzieren. Diese Spannungsebene wird dann genutzt um den MCU, Gate Treiber, ... zu speisen. 

#### 5. Device Manager Adapter (DMA)
Dies ist eigentlich ein externes Gerät das verwendet wird um das ESC zu programmieren oder die Firmware upzudaten. 

## Was ist PWM?

Die Pulsweitenmodulation (PWM) ist das ursprüngliche Protokoll zur Ansteuerung von Drehzahlreglern (ESCs) und findet aufgrund ihrer Zuverlässigkeit bis heute Anwendung. Das System steuert die Motorgeschwindigkeit durch präzise getaktete Spannungsimpulse, wobei das Verhältnis zwischen der „An“- und der „Aus“-Zeit – der sogenannte Tastgrad (Duty Cycle) – entscheidend für die Leistungsabgabe ist. Wie in der **beigefügten Abbildung** verdeutlicht wird, führt ein längerer Impuls zu einer höheren Spannung am Motor und damit zu einer schnelleren Rotation des Rotors.

Technisch gesehen variiert die Impulsbreite dabei in einem Bereich von 1000 µs bis 2000 µs. Während die Signale früher in größeren Abständen gesendet wurden, arbeiten moderne Systeme mit einer deutlich höheren Frequenz von etwa 490 Hz, um eine schnellere Reaktionszeit zu ermöglichen. Im ESC selbst nimmt ein Gate-Treiber die Signale des Mikrocontrollers auf und leitet sie an die MOSFETs weiter. Diese fungieren als Schalter für die drei Phasen des Motors: Je mehr Spannung sie erreicht, desto schneller wechseln sie die Phasen, was die Drehzahl des Rotors erhöht.

![[esc_throttle_signal_pulses.jpg]]

#### Control Frequency vs. MOSFET Switching Frequency
Der Flight Controller sagt dem ESC was es machen soll mit der Control Frequency (PWM)
Das ESC sagt dann den MOSFETs wie es es machen soll mit der Switching Frequency 

## ESC Protokolle

Die ESC Protokolle sind praktisch die Sprache in der der Flight Controller mit dem ESC spricht. Sie verwenden spezielle Muster um so die Information über die Gasstellung zu übertragen und sie variieren die Geschwindigkeit des Signals um die Drehzahl des Motors zu steuern.

#### Gewöhnliche ESC Protokolle
Vor 2015 war PWM das einzige ESC Protokoll das kommerziell für Drohnen genutzt wurde. Seit dem wurden einige neue Protokolle entwickelt. Wie z.B.: Oneshot125, Oneshot42, Multishot, and Dshot300, Dshot600 und Dshot1200. Oneshot und Multishot verwenden analoge signale wie PWMs. Dshot verwendet digitale Signale. 

Analoge Protokolle benötigen jedoch Kalibrierung der Oszilatoren. Denn er Oszilator im ESC muss genau den gleichen Takt haben wie jener im Flight Controller. Ohne diese Kalibrierung kann es zu störungen kommen. Digitale Systeme brauchen diesen schritt nicht.

#### Dshot
Dshot1200 ist das schnellste Protokoll. Es liefert 1.200.000 bits Daten pro Sekunde. Dieses Protokoll hat eine fixe Signallänge von 13 µs was fast doppelt so schnell ist wie Multishot mit 25 µs. 

#### Bi-directional DShot
Bi-directional DShot ist eine relativ neue Entwicklung. Es erlaubt es das das ESC auch mit dem FC kommunizieren kann. Und nicht nur wie bei den anderen Protokollen der FC mit dem ESC. Das erlaubt es das Telemetriedaten dem FC mitgeteilt werden können und dieser kann damit dann sachen umsetzen wie fortgeschrittenes RPM Filtering, verringerte signal verspätung und verbesserte Motorperformance. Das alles führt zu einem ruhigeren Flugverhalten.

#### Proshot
Proshot ist ein Protokoll das Digitale Daten in ein PWM Signal packt. So kann es in einem PWM Puls 4-Bit Daten packen. Also 16-Bit in 4 Pulsen. Es ist keine Kalibrierung der Oszilatoren nötig.

## CAN für ESCs

Ähnlich wie moderne Autos benötigen Drohnen eine extrem zuverlässige und effiziente Kommunikation zwischen Flight Controller, ESCs und Sensoren. Hier glänzt der **CAN-Bus** durch seine enorme Störfestigkeit und Echtzeitfähigkeit. Während CAN die physische Transportebene bildet, fungiert **Cyphal** (früher UAVCAN) als spezialisiertes Protokoll darüber, das die Struktur der Datenpakete festlegt. Der größte Vorteil gegenüber veralteten Standards wie PWM oder DShot liegt in der Verkabelung: Statt für jeden Motor ein eigenes Signalkabel zu benötigen, werden alle ESCs und Sensoren über ein gemeinsames Zwei-Draht-Netzwerk verbunden.

Dies ermöglicht nicht nur eine sauberere Hardware-Integration, sondern erlaubt auch den bidirektionalen Datenaustausch. So können neben Steuerbefehlen gleichzeitig wertvolle Telemetriedaten wie Spannung, Stromstärke und Temperatur übertragen werden. Ein entscheidendes Sicherheitsmerkmal ist die **ID-basierte Priorisierung**: Kritische Signale wie die Motordrosselung erhalten niedrigere IDs und damit Vorrang vor weniger zeitkritischen Daten wie GPS-Positions-Updates. Dadurch bleibt die Drohne selbst bei hoher Netzwerkauslastung oder dem Ausfall einzelner Module (z. B. des GNSS-Systems) jederzeit präzise steuerbar und stabil in der Luft.