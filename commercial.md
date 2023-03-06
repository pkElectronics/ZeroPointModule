# WIP Dokumentation und Anwendungsbeispiele (English version below)

Wenn Sie ein ZeroPointModule bei IB-Wistinghausen oder einem anderen autorisierten Reseller erworben haben, finden Sie hier die notwendige Dokumentation für die Inbetriebnahme sowie einige Anwendungsbeispiele

### Zusammenbau
Der Lieferumfang umfasst jeweils zwei M2.5 Schrauben, Muttern und Abstandsbolzen, montieren Sie den Raspberry Pi wie abgebildet.

Die mitgelieferten 4pin Stecker können entweder auf der ober- oder auf der unterseite des Platine angebracht werden. Auf der unterseite entspricht die Pinbelegung der, handelsüblicher PWM Lüfter.


### Konfiguration

Die Konfiguration der Kernel Treiber wird über das vorprogrammierte Autokonfigurationseeprom realisiert. Dieses aktiviert folgende Funktionen:
- MCP2515 CAN Controller
- PWM Channel 0 auf GPIO12
- PWM Channel 1 auf GPIO13

Weitere Kernelmodule können einfach über die config.txt nachgeladen werden.

Soll das Board als CAN Controller für Klipper verwendet werden, muss folgende Einstellung zusätzlich vorgenommen werden:

Erstellen Sie die Datei `/etc/network/interfaces.d/can0` mit folgendem Inhalt

```
auto can0
iface can0 can static
    bitrate 500000
    up ifconfig $IFACE txqueuelen 128
```


### Hinweise

Wichtiger Hinweis für alle Klipper Nutzer: Der Port SPI1 ist aufgrund eines Bugs im Broadcom Prozessor des Raspberry Pi Zero 2W nicht für den Betrieb eines ADXL345 geeignet


### Anwendungsbeispiele


