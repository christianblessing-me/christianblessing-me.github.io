---
title: "Openhab2 Backup via Bash-Script und Dropbox"
date: 2020-06-12T15:52:14+01:00
draft: false
---

![Openhab2 Header](/images/Openhab2_Header.png)

Jeder der eine offene Home-Automation Lösung wie Openhab im Einatz hat, hat viele Stunden mit der aufwändigen Konfiguration verbracht - zudem liegen viele Messwerte in einer SQL Datenbank, welche man später via [Grafana](https://grafana.com/) o.Ä. visualisieren und auswerten möchte. Datenverlust wäre sehr ärgerlich. Alle bereits gesammelten Datenpunkte wären verloren und die Konfiguration des geliebten Setups würde wieder Tage und Wochen verschlingen. Ich habe das Problem bei mir mit einem simplen Setup, bestehend aus einem Bash-Script, sowie Dropbox gelöst.

# Dropbox Uploader

Um unser Backup später auf Dropbox speichern zu können, verwenden wir das Dropbox Uploader Repository von [Andrea Fabrizi](https://github.com/andreafabrizi/). Zuerst benötigt ihr Git auf Eurem Raspberry Pi, dazu Git via Command Line installieren:
{{<highlight Bash>}}
sudo apt-get install git
{{</highlight>}}
Nach der erfolgreichen Installation von Git klont ihr Euch das Repository via:
{{<highlight Bash>}}
git clone https://github.com/andreafabrizi/Dropbox-Uploader.git
{{</highlight>}}
Jetzt wechselt ihr in das Dropbox-Uploader Directory:
{{<highlight Bash>}}
cd ~/Dropbox-Uploader
{{</highlight>}}
Von hier aus könnt ihr jetzt die Konfiguration des Dropbox Uploader Scripts starten:
{{<highlight Bash>}}
./dropbox_uploader.sh
{{</highlight>}}
Das Script wird Euch Schritt für Schritt durch die Installation führen, nach der Installation wechselt ihr auf die Dropbox Website und erstellt eine API Application, den zugehörigen API Access Token gebt ihr dann wiederum im Terminal ein, um den Dropbox Uploader zu authentifizieren. Um Dropbox verwenden zu können benötigt ihr selbstverständlich einen Dropbox Account, hierbei reicht ein kostenloser Account mit max. 2GB Speicherplatz für die erste Zeit aus.
# Backup Bash Script
![Bash Logo](/images/Bash_logo.png)
Openhab2 bietet von Haus aus eine [Backup Funktionalität](https://community.openhab.org/t/recommended-way-to-backup-restore-oh2-configurations-and-things/7193/81) an, diese machen wir uns zu Nutzen, um die Konfiguration von Openhab zu sichern. Im Standard werden jedoch keine Datenbanken gesichert, um diese auch mit in das Backup aufzunehmen, [dumpen](https://dev.mysql.com/doc/refman/5.7/en/mysqldump-sql-format.html) wir die komplette SQL Datenbank.

Zuerst müssen wir ein Bash-Script erstellen, ihr könnte dafür einen Texteditor Eurer Wahl verwenden, ich verwende hierfür [Nano](https://en.wikipedia.org/wiki/GNU_nano), ihr könnt natürlich auch [vi](https://en.wikipedia.org/wiki/Vi), [vim](https://en.wikipedia.org/wiki/Vim_(text_editor)) oder jeden anderen Editor verwenden:
{{<highlight Bash>}}
nano openhab_backup.sh
{{</highlight>}}

Zuerst definieren wir den Script-Type:
{{<highlight Bash>}}
#!/bin/bash
{{</highlight>}}

Wir ziehen ein Backup einer kompletten MYSQL Datenbank und speichern es in einem .sql File, ihr müsst hier Euren Username, Password und die entsprechende Datenbank hinterlegen. Achtung: Ihr hinterlegt hier das Passwort des SQL Users im Klartext, ihr solltet Euch über das Sicherheitsrisko im Klaren sein.
{{<highlight SQL>}}
mysqldump -u USER --password="PASSWORD" DATABASE > backup.sql
{{</highlight>}}


Im nächsten Schritt zippen wir das erstellte Datenbankbackup:
{{<highlight Bash>}}
gzip backup.sql
{{</highlight>}}

Jetzt verschieben wir das backup.sql.gz File in das Directory des Dropbox-Uploaders:
{{<highlight Bash>}}
mv backup.sql.gz /home/openhabian/Dropbox-Uploader
{{</highlight>}}


Mit dem nachfolgenden Befehl laden wir das File auf Dropbox hoch, die Date section am Ende des Filenames definiert die Formatierung des Files, in unserem Fall wird es mit der Endung year_month_day abgespeichert:
{{<highlight Bash>}}
(exec /home/openhabian/Dropbox-Uploader/dropbox_uploader.sh upload /home/openhabian/Dropbox-Uploader/backup.sql.gz sql_backup_date +%Y_%m_%d.gz)
{{</highlight>}}

Anschließend können wir das File lokal löschen:
{{<highlight Bash>}}
rm /home/openhabian/Dropbox-Uploader/backup.sql.gz
{{</highlight>}}

Jetzt ist das Config Backup von Openhab2 an der Reihe, dazu rufen wir das Backup Script von Openhab2 auf. Achtung: auch hier hinterlegen wir das Passwort des ROOT Users im Klartext.
{{<highlight Bash>}}
echo ROOT-USER-PASSWORD | sudo -S openhab-cli backup backup_config.zip
{{</highlight>}}

Anschließend verschieben wir das erstelle Config Backup in das Dropbox-Uploader Directory:
{{<highlight Bash>}}
mv backup_config.zip /home/openhabian/Dropbox-Uploader
{{</highlight>}}


Mit dem nachfolgenden Befehl wir das File hochgeladen, die Notation des Files ist die selbe wie bereits beim oben beschriebenen Datenbank Backup:
{{<highlight Bash>}}
(exec /home/openhabian/Dropbox-Uploader/dropbox_uploader.sh upload /home/openhabian/Dropbox-Uploader/backup_config.zip backup_config_date +%Y_%m_%d.zip)
{{</highlight>}}

Zum Schluss löschen wir auch das Config Backup lokal, auch hier muss wieder mit Root Rechten gearbeitet werden, die Sicherheitsrisiken, welche durch das Hinterlegen des Root Passwortes im Klartext entstehen können, habe ich weiter oben beschrieben:
{{<highlight Bash>}}
echo ROOT-USER-PASSWORD | sudo -S rm /home/openhabian/Dropbox-Uploader/backup_config.zip
{{</highlight>}}

# Automatisierung des Backups via Cronjob

Ihr könnt Euer Backup jetzt jederzeit mit dem nachfolgenden Befehl ausführen.
{{<highlight Bash>}}
sh openhab_backup.sh
{{</highlight>}}

Besser wäre es jedoch, wenn das Backup automatisiert, z.B. jeden Tag zu einer bestimmten Uhrzeit, ausgeführt werden würde. Dazu können wir uns das Crontab zu Nutzen machen. Dieses rufen wir mit folgendem Befehl auf:
{{<highlight Bash>}}
crontab -e
{{</highlight>}}

Am Ende des Files könnt ihr weitere Cronjobs hinzufügen. Mein Setup führt den Backup Job jede Nacht um 05:00 Uhr durch, dazu habe ich mir folgende Zeile hinzugefügt:
{{<highlight Bash>}}
0 5 * * * sh /home/openhabian/openhab_backup.sh
{{</highlight>}}

Ihr könnt selbstverständlich auch einen anderen Intervall wählen, der [Cronjob Generator](https://crontab-generator.org/) kann hierfür sehr hilfreich sein.

# Abschließend

Mit der oben beschriebenen Methode lässt sich einfach ein rudimentäres Backup von Openhab2 realisieren, als Ziel muss nicht unbedingt Dropbox gewählt werden. Man könnte die Files auch auf eine NAS im lokalen Netzwerk, nach AWS, Azure, Google Drive etc. schieben.

Bitte bedenkt, dass wir nur die Datenbank und die Openhab2 Config gesichert haben, andere Scripts/Applications, welche ihr für Euer Setup benötigt sind nicht gesichert, das Bash Script lässt sich aber endlos erweitern und an Eure Wünsche anpassen.

Zudem steht Euer ROOT Password aktuell noch im Backup Script, das ließe sich über User-Rechte besser regeln, vor allem wenn man es produktiv einsetzen möchte.

