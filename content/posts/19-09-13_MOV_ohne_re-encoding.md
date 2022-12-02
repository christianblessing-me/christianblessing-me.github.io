---
title: "MOV Files zusammenfügen ohne re-encoding"
date: 2019-09-13T15:52:14+01:00
draft: false
---

Canon DSLR Kameras können oftmals nur Clips mit einer maximalen Länge von 4GB speichern, ist der eigentliche Clip länger, wird er in 4GB große Clips aufgesplittet. Möchte man später einen gesamten Clip erstellen, muss man meist den Clip zuerst re-encodieren. Mit Hilfe von [ffmpeg](https://ffmpeg.org/) ist dieser Part einfach zu erledigen, ohne Re-Encoding.

# Concat File erstellen
ffmpeg müssen wir zuerst mitteilen, welche Files zusammengefügt werden sollen, dazu erstellen wir ein einfaches **concat.txt** file (in unserem Beispiel führen wir drei *.mov Dateien nacheinander zusammen):
{{<highlight Bash>}}
file ‘PATH/TO/YOUR/FILE/FILE01.MOV’
file ‘PATH/TO/YOUR/FILE/FILE02.MOV’
file ‘PATH/TO/YOUR/FILE/FILE03.MOV’
{{</highlight>}}

# Dateien zusammenführen
Die Dateien mit folgendem ffmpeg Befehl zusammenführen:
{{<highlight Bash>}}
ffmpeg -f concat -i concat.txt -c copy Output_Final.mov
{{</highlight>}}

# Warum man nicht einfach 'cat' verwenden kann
Führt man einen normalen cat befehl aus, so werden die mov Files zwar zusammengefügt, jedoch ist der Header des finalen mov files nicht vorhanden. Die Datei hat dann zwar die angenommene Größe, jedoch kann das File ohne Header nicht von Final Cut Pro x oder Adobe Premiere eingelesen werden.