---
title: "433MHz Funksteckdosen mit dem Raspberry Pi fernsteuern"
date: 2017-12-15T15:52:14+01:00
draft: false
---

![RaspberryPi2 mit 433MHz Modul](/images/433mhz_steckdose_raspi.jpg)

Funksteckdosen gibt es mittlerweile schon für kleines Geld beim Baumarkt um die Ecke. Damit lassen sich elektrische Geräte mit Hilfe einer Fernbedienung fernsteuern, besser gesagt, die Geräte lassen sich damit an- und ausschalten. Die Funktion der Fernbedienung kann mit einem passenden Modul auch vom Raspberry Pi übernommen werden, damit lassen sich günstige 433MHz Steckdosen in die eigene Home-Automation integrieren.

# Benötigte Komponenten

Voraussetzung  ist natürlich ein bereits konfigurierter Raspberry Pi. Um die Funksteckdosen steuern zu  können benötigen wir noch folgende Zusatzkomponenten:

* Funksteckdosen (ich benutze hier welche der Firma “ELRO”) ca. 15€ ([Amazon](http://www.amazon.de/gp/product/B002QXN7X6/ref=as_li_ss_tl?ie=UTF8&camp=1638&creative=19454&creativeASIN=B002QXN7X6&linkCode=as2&tag=picturaluceor-21))
* 433MHz Sende-Modul ca. 9€ ([Amazon](http://www.amazon.de/gp/product/B00ATZV5EQ/ref=as_li_ss_tl?ie=UTF8&camp=1638&creative=19454&creativeASIN=B00ATZV5EQ&linkCode=as2&tag=picturaluceor-21))
* Jumperkabel (weiblich auf beiden Seiten) ca. 6€ ([Amazon](http://www.amazon.de/gp/product/B00CWN8PQE/ref=as_li_ss_tl?ie=UTF8&camp=1638&creative=19454&creativeASIN=B00CWN8PQE&linkCode=as2&tag=picturaluceor-21) 40Stk.)

# Installation der Komponenten

Als Erstes trennen wir sämtliche Verbindungen mit unserem Raspberry Pi, vor allem das Netzkabel! Jetzt öffnen wir den Deckel des Gehäuses, dass wir an die GPIO-Anschlüsse kommen. Die GPIOs sind die 26 kleinen Pins auf dem Board des PIs. Mit drei Jumperkabeln verbinden wir das 433MHz Sende-Modul wie folgt mit dem Pi:

![RaspberryPi2 Jumper Kabel](/images/433mhz_steckdose_jumper.jpg)

Nachfolgend eine schematische Darstellung. Wichtig ist, dass die Perspektive so gewählt wurde, dass das 433MHz Sende-Modul mit der Spule auf dem Kopf liegt.

![RaspberryPi2 Jumper Schema](/images/433mhz_steckdose_schema.jpg)

Sind alle drei Jumperkabel korrekt verbunden, so können wir die restliche Peripherie wieder an den RaspberryPi anschließen und ihn mit Strom versorgen.

# Installation der Software

Um das 433MhZ Sende-Modul dazu zu bewegen, dass wir es mittels der GPIOs ansprechen können, müssen wir noch die passende Software (wiringPi und raspberry-remote) installieren. Im ersten Schritt aktualisieren wir die Paketliste via:

{{<highlight Bash>}}
sudo apt-get update
{{</highlight>}}

Danach installieren wir die Updates aus der Liste:

{{<highlight Bash>}}
sudo apt-get upgrade
{{</highlight>}}

Um wiringPi und raspberry-remote installieren zu können, benötigen wir git auf unserem RaspberryPi, solltet ihr git bereits für ein vorhergehendes Projekt installiert haben, könnt ihr den nächsten Schritt überspringen:

{{<highlight Bash>}}
sudo apt-get install git-core
{{</highlight>}}

Nach der Installation von git können wir das Repository von wiringPi auf unsere lokale Maschine klonen:

{{<highlight Bash>}}
git clone git://git.drogon.net/wiringPi
{{</highlight>}}

Danach wechseln wir in das neu erstelle Verzeichnis:

{{<highlight Bash>}}
cd wiringPi
{{</highlight>}}

Jetzt muss wiringPi aus dem Source Code kompiliert werden, das übernimmt build:

{{<highlight Bash>}}
./build
{{</highlight>}}

Anschließend wechseln wir wieder in das übergeordnete home directory:

{{<highlight Bash>}}
cd ..
{{</highlight>}}

Nun können wir raspberry-remote installieren, dazu klonen wir das Repository:

{{<highlight Bash>}}
git clone git://github.com/xkonni/raspberry-remote.git
{{</highlight>}}

Im letzten Schritt kompilieren wir auch raspberry-remote aus dem Source Code, dazu wechseln wir in das neue Verzeichnis und kompilieren das Tool via make:

{{<highlight Bash>}}
make send
{{</highlight>}}

Die Software ist jetzt installiert und können uns dem Konfigurieren der Funksteckdosen widmen.

# Konfiguration der Funksteckdosen

Die Funksteckdosen von ELRO haben hinten eine kleine Klappe. Schraubt man sie auf, so sieht man acht kleine Schalter:

![Elro Steckdose](/images/433mhz_steckdose_elro.jpg)

Die ersten fünf Zahlen geben den sogenannten “Hauscode” an, auf einem “Hauscode” können vier verschiedene Geräte geschaltet werden. Die hinteren vier Buchstaben geben die Geräte an. Bei der gezeigten Steckdose ist der  Hauscode folglich “11111” und die angesteuerte Steckdose heißt “A”, also “1”. Will man eine weitere Steckdose mit dem gleichen Hauscode  ansteuern, so müsste man den Schalter bei “B” auf 1 stellen, bei A, C, D und E auf 0.

Mit diesem System könnt ihr folglich mehrere Funksteckdosen in Euer Netzwerk aufnehmen, es ist auch möglich den gleichen Code an unterschiedliche Steckdosen zu vergeben, diese werden dann gleichzeitig geschaltet, sobald sie den Befehl empfangen.

# Ansteuern der Funksteckdosen

Um die Funksteckdose(n) jetzt anzusteuern, wechselt man in das Verzeichnis von raspberry-remote. Hier können jetzt die Funksteckdose mittels des Hauscodes und der passenden Steckdosen-Kennung angesteuert werden. In meinem Beispiel schalte ich die oben beschrieben Steckdose mittels dem folgenden Befehl:

{{<highlight Bash>}}
sudo ./send 11111 1 1
{{</highlight>}}

Ändert man die Befehlskette am Ende auf eine 0, so schaltet man die entsprechende Steckdose wieder aus:

{{<highlight Bash>}}
sudo ./send 11111 1 0
{{</highlight>}}

Hätte man beispielsweise eine weitere Steckdose mit dem gleichen Hauscode auf Kanal B, so könnte man sie mit folgendem Befehl anschalten:

{{<highlight Bash>}}
sudo ./send 11111 2 1
{{</highlight>}}

Wichtig ist, dass man sich im Verzeichnis von raspberry-remote befinden muss, um den Steuerungsbefehl abzusetzen. Wenn man nicht immer extra in  das Verzeichnis von raspberry-remote wechseln möchte, so kann mit folgendem Befehl die Steuerung auslöst werden (hier schalten wir wieder Steckdose A an):

{{<highlight Bash>}}
sudo /home/pi/raspberry-remote/send 11111 1 1
{{</highlight>}}

# Anmerkungen

433MHz Steckdosen sind sehr günstig erhältlich, bergen jedoch auch einen erheblichen Nachteil: sie sind selbst [stateless](https://nordicapis.com/defining-stateful-vs-stateless-web-services/). Es kann nicht festgestellt werden, ob eine Steckdose gerade ON oder OFF ist. In unserem Beispiel haben wir keinen Rückkanal, da der Client (die Steckdose) nur einen Empfänger besitzt und somit weder zurückgeben kann, ob die entsprechende Steckdose gerade ON oder OFF ist, noch ob sie den Schaltbefehl wirklich erhalten hat. Es ist zudem ratsam, den Befehl mehrmals kurz hintereinander zu senden, um sicherzustellen, dass der Befehl wirklich bei der Steckdose ankam.

Möchte man ein stateful Environment aufbauen, lohnt es sich, sich mit [WLAN Steckdosen](https://www.christianblessing.me/gosund-sp111-mit-tasmota-flashen/) auseinanderzusetzen. Diese sind mittlerweile auch relativ günstig erhältlich und können, wenn auch mit etwas Mehraufwand verbunden, einfach in ein Home-Automation Setup eingebunden werden.

