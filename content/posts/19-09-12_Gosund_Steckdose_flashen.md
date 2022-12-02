---
title: "Gosund SP111 mit Tasmota flashen"
date: 2019-09-12T15:52:14+01:00
draft: false
---

{{< image src="/images/tasmota.svg" alt="Tasmota Logo" >}}

Mit Hilfe eines [FTDI to USB Adapter](https://www.amazon.de/keyestudio-Adapter-FT232RL-Arduino-Starter/dp/B07H3VH68F/ref=sr_1_1?ie=UTF8&qid=1547594858&sr=8-1&keywords=keyestudio+FTDI+Basic+USB+Adapter) lassen sich günstige WiFi Steckdosen, z.B. die [Gosund SP111](https://www.amazon.de/gp/product/B07FVT15L5/ref=ppx_yo_dt_b_asin_title_o02__o00_s01?ie=UTF8&psc=1) mit einer neuen Firmware flashen. Damit können wir sie via [MQTT](https://de.wikipedia.org/wiki/MQTT) oder HTTP in unsere Heimautomatisierungslösungen, wie z.B. [OpenHab2](https://www.openhab.org/docs/) oder [Homeassistant (HASS)](https://www.home-assistant.io/) einbinden.

# Vorab: USBasp != FTDI to USB

Da ich es schmerzlich selbst herausfinden musste, ein [USBasp](https://www.fischl.de/usbasp/) ist etwas anderes als ein [FTDI to USB Adapter](https://www.amazon.de/keyestudio-Adapter-FT232RL-Arduino-Starter/dp/B07H3VH68F/ref=sr_1_1?ie=UTF8&qid=1547594858&sr=8-1&keywords=keyestudio+FTDI+Basic+USB+Adapter). Die meisten Fernost WiFi Steckdosen haben einen [ESP8266](https://www.mikrocontroller.net/articles/ESP8266) verbaut, dieser lässt sich nicht via USBasp oder AVR flashen - hierfür wird ein FTDI to USB Adapter benötigt.

# Vorab: Gosund SP111 != Gosund EP2

Da mir es leider selber passiert ist: die [Gosund EP2 Steckdosen](https://www.amazon.de/Gosund-Steckdosen-erforderlich-Stromverbrauch-Fernsteurung/dp/B085RFKVW4/), welche gerade vermehrt auf Amazon und anderen Plattformen verkauft werden, können nicht verwendet werden. Diese Variante der Steckdose hat keine offen zugängliche Schraube auf der Rückseite, auch der interne Chip ist kein ESP8266.

> Update 2021: Mittlerweile lassen sich bestimmte Steckdosen auch OTA (Over the Air) mit Tasmota flashen; damit muss man Steckdoesen nicht mehr aufschreiben und Brücken löten.

# Öffnen des Gehäuses

Auf der Rückseite des Gehäuses befindet sich eine nach innen versetzte Kreuzschlitzschraube, diese öffnen und das Gehäuse sanft auseinanderziehen.

Wir benötigen eine Verbindung zu 3.3V, TXD, RXD, GND, sowie eine Brücke zwischen GND und IO1, um den ESP8266 in den Flash-Modus zu versetzen. Ich empfehle die Brücke nicht einzulöten, 2 sek. Überbrücken beim Bootvorgang reichen aus.

![Gosund Steckdose](/images/gosund_steckdose.jpeg)

# Verbindung zum FTDI

Wir verbinden jetzt 3.3v mit 3.3v/5v, GND mit GND, TXD mit RXD, RXD mit TXD. Wichtig ist, dass beim Bootvorgang IO1 mit GND für ca. 2 Sek. verbunden ist, nur dann wird der Chip in den Flash-Modus versetzt. Am besten also IO1 mit GND brücken, dann erst den FTDI via USB verbinden und die Brücke nach 2 Sek. wieder lösen.

![Gosund FTDI](/images/gosund_flasher.jpeg)

# Flashvorgang

Hierzu verwenden wir den [ESP-EasyFlasher](https://github.com/Grovkillen/ESP_Easy_Flasher/releases), sowie das aktuellste [Tasmota Binary](https://github.com/arendst/Sonoff-Tasmota/releases). Das Binary entpacken und in den Ordner des ESP-EasyFlasher legen. Danach den ESP-EasyFlasher starten, den COM-Port und das Binary auswählen und den Flashvorgang starten.

Nach dem Flashvorgang einmal den FTDI von USB trennen und neu verbinden (jetzt müsst ihr die Verbindung nicht mehr überbrücken).

# Tasmota Firmware via WiFi einrichten

Nach dem erfolgreichen Flashvorgang + Reboot sollte ein neues WiFi Netzwerk im AP-Modus verfügbar sein, verbindet Euch hiermit und konfiguriert SSID, sowie das zugehörige Passwort.

Wählt nach dem erneuten Reboot (automatisch, nach dem auswählen des WiFi Netzwerks) die Blitzwolf SHP (45) Firmware unter "Einstellungen" --> "Gerät konfigurieren aus". Nach einem erneuten Reboot solltet ihr bereits via Webinterface die Steckdose schalten, sowie den aktuellen Stromverbrauch sehen können.

{{< image src="/images/tasmota_screenshot.png" alt="Tasmota Screenshot" >}}

# Einbindungsmöglichkeiten

Die Tasmota Steckdosen könnt ihr jetzt entweder via HTTP Request oder MQTT in Eure Openhab2/Homeautomation Lösung einbinden.