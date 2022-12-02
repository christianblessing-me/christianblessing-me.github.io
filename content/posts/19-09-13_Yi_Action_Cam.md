---
title: "Yi Action Cam: SD Karte formatieren"
date: 2019-09-13T15:52:14+01:00
draft: false
---

Die Yi Action Cam ist eine kostengünstige (ca. 50EUR im Retail, Stand 03/2018) Action Cam von Yi Technologies, leider "meckert" sie, wenn man eine SD Karte mit einer Clustersize > 32kB einlegt. Reguläre SD Karten sind nicht mit einer Clustersize von 32kB formatiert, das lässt sich jedoch unter OS X einfach beheben:

Zuerst lassen wir uns alle Drives anzeigen:

{{<highlight Bash>}}
diskutil list
{{</highlight>}}

.. wir merken uns die disk-set-no der eingelegten SD Karte und unmounten die Disk:

{{<highlight Bash>}}
diskutil unmount /dev/{disk-set-number}
{{</highlight>}}

.. und formatieren die SD Karte mit einer Clustersize von 32kB:

{{<highlight Bash>}}
sudo newfs_msdos -F 32 -c 64 -v {volumeid} /dev/{disk}
{{</highlight>}}

Die Yi Action Cam sollte die SD Karte nun problemlos annehmen.