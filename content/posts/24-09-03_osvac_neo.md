---
title: "OsVac (Neo) Staubsaugeradapter"
date: 2024-09-03T12:34:11+02:00
draft: false
---

![osvac_logo](/images/osvac_logo.png)

Fast jeder (Hobby)-Schreiner kennt das Problem: man hat verschiedene Elektrowerkzeuge zur Bearbeitung von Holz, sei es eine Handkreissäge, Schleifgerät oder Dickenhobel. Es fallen immer Späne, mal größer, mal kleiner an. Meist wird man nicht alle Geräte von einem Hersteller gekauft haben, somit passt der Absaugschlauch vom Staubsauger nicht an alle Geräte.

Man kann jetzt stumpf den Schlauch ohne Adapter versuchen auf die einzelnen Öffnungen mit Gewalt drüber zu stülpen, daran wird man aber nicht sehr lange Spaß haben. Der Schlauch leiert aus, es hält nicht sauber an der Maschine und der Staub und Dreck kommen doch wieder in die Werkstatt anstatt in den Staubsauger. Schritt zwei wird dann meist die brachiale Methode mit [Gewebeklebeband](https://www.amazon.de/3M-Universelles-Gewebeklebeband-2903-Schwarz/dp/B077VNDNJ8/) sein, das mag ein paar Tage halten, danach klebt dann aber nur der Schlauch und die Maschine, der Staub wird weiterhin nicht sauber abgesaugt.

Alternativ gibt es [Universalschlauchadapter](https://www.amazon.de/St%C3%BCck-Schlauchadapter-universal-Staubsaugeradapter-Staubsaugerschlauch/dp/B08D3Y9WGM/), bzw. Reduzierstücke auf die gewünschten Durchmesser. Leider passen diese nicht auf alle Geräte und halten auch nur mäßig gut an der jeweiligen Maschine (sie fallen meist nach 5 min. Nutzung ab). Zudem müssen die Stücke zurechtgeschnitten werden, bei ca. 5€ pro Reduzierstück also nicht nachhaltig.
## Die Alternative: osVac

Genau dieses Problem haben die Hobby-Heimwerker im [Hobbyhimmel](https://osvacneo.de/) in Stuttgart erkannt und [osVac](https://osvacneo.de/) bzw. osVac [Neo](https://osvacneo.de/) entwickelt. osVac ist ein Open-Source Verbindungssystem für elektrische Handwerkzeuge. Die 3D Modelle sind [frei erhältlich](https://osvacneo.de/downloads/) und können remixed werden, somit kann man sich für jedes nur denkbare Gerät, vorrausgesetzt man ist einer 3D CAD Anwendung mächtig und besitzt einen 3D Drucker, den passenden Adapter designen und anschließend drucken. Ich gehe hier absichtlich nicht auf duzende "Webshops" von "DIY YouTubern" ein, für einen simplen Adapter > 15€ aufzurufen ist m.M.n. einfach nur frech und widerspricht der Open Source Idee.

Es gibt auf [Thingiverse](https://www.thingiverse.com/search?q=osvac&page=1) duzende Modelle von Nutzern für alle möglichen Maschinen. Einfach herunterladen und drucken. Sollte das passende Modell nicht vorhanden sein, können die osVac Neo Dateien in Fusion360 geladen und nach den eigenen Bedürfnissen angepasst werden.

Das Grundprinzip besteht aus einem Female Stück (F), welches mit einem Innengewinde kommt, welches auf den Saugschlauch geschraubt wird. Die meisten Staubsaugerschläuche kommen mit einem Gewinde von 32mm, daher ist der F32 Adapter der gängigste. Selbstverständlich gibt es auch F-Adapter mit 32, 36, 40 oder 50mm - sollte das nicht ausreichen kann man sich mit den Vorlagen die Adapter auch an den eigenen Schlauchdurchmesser anpassen.

Das Gegenstück zu Female stellt das Male (M) Stück dar. Beide Teile formen zusammen einen simplen Baynonettverschluss - aufstecken, reindrücken und drehen. Damit sind beide Teile, Female (F) und Male (M) fest miteinander verbunden. In der Praxis wird man sich meist einen Female (F) Adapter drucken und dann für jede Maschine den passenden Male (M) Adapter. Die Adapter verbleiben in der Maschine, den Saugschlauch mit dem Female (F) Stück kann man dann nach Belieben an die jeweilige Maschien via dem Bayonettverschluss arretieren.

## Meine Erfahrungen mit osVac

OsVac per se war nicht neu für mich. In den Untiefen meiner Notizen hatte ich mir das System bereits notiert, kam aber bisher nicht dazu, die Adapter zu drucken und das System selber auszuprobieren.

Das Drucken selbst ist sehr einfach, wichtig ist nur, dass man keinen Support im Slicer verwendet - das mag am Anfang irritierend sein, macht aber Sinn. Die Teile sind relativ grob, das trifft auch auf die Gewinde zu (in meinem Fall ein F32mm Gewinde). Bei solchen Auflösungen benötigt man kein Support Material. Ich habe bei meinen ersten Druckversuchen leider den Fehler begangen und habe mit Support gedruckt. Das Gewinde vom Support zu befreien war kein Spaß und kann auch zu Ausriss am Gewinde selbst führen, zudem verlängert sich die Druckzeit mit Support bei der Größe der Adapter erheblich.

Die OsVac Rohdateien selbst sind super designed und einfach verständlich, auch die Remixe, welche man auf [Thingiverse](https://www.thingiverse.com/search?q=osvac&page=1) findet sind sehr hilfreich - entweder weil man noch nicht gut mit [Fusion360](https://www.autodesk.com/de/products/fusion-360/) o.Ä. zu Recht kommt, oder weil der passende Adapter bereits von einem anderen User erstellt wurde.
## OsVac Adapter 

Hier die Adapter, welche ich als Remixe heruntergeladen und gedruckt habe.

1. F32 to M32 [(Thingiverse)](<[Thingiverse](https://www.thingiverse.com/thing:4562762)>)
2. F32 Mirka Deros Orbitsander [(Thingiverse)](<[Thingiverse](https://www.thingiverse.com/thing:4678233)>)
3. F32 Dewalt 7492 Tischkreissäge [(Thingiverse)](https://www.thingiverse.com/thing:4967925)
4. F32 Festool Adapter (Kappex KS60) [(Thingiverse)](https://www.thingiverse.com/thing:4822318)
## Eigene OsVac Adapter Designs

Da ich für die [Scheppach BTS800](https://shop.scheppach.com/band-und-tellerschleifer-bts800-scheppach-diy) Band- und Tellerschleifemaschine, sowie für den [Vevor Dickenhobel](https://eur.vevor.com/benchtop-planer-c_10104/vevor-thickness-planer-13-inch-two-speed-three-blade-15-amp-for-woodworking-p_010145887280) bisher keine passenden Adapter finden konnte, habe ich diese fix selber in Fusion360 designed. Bei Bedarf könnt ihr diese auf Thingiverse herunterladen und nachdrucken.

1. F32 Scheppach BTS800 [(Thingiverse)](https://www.thingiverse.com/thing:6753270)
2. F32 Vevor Dickenhobel [(Thingiverse)](https://www.thingiverse.com/thing:6753279)

## Fusion 360 Tipps für OsVac Adapter

Da ich selbst etwa eine knappe Stunde in das Thema investiert habe und bisher keine passenden Anleitungen dafür auf YouTube o. Ä. gefunden habe, werdet ihr oft vor der Herausforderung stehen, eine Verbindung von einem hohlen Zylinder zu einem anderen hohlen Zylinder herstellen zu wollen – von 32 mm auf X mm (Euer anzupassender Durchmesser für die Absaugung an der Maschine).

Erstellt zuerst zwei Sketches mit Kreisen, d.h. einmal 37.8mm und 50mm (exemplarischer Wert). Und definiert danach die Höhe der beiden, z.B. 20mm Abstand.

Erstellt jetzt einen Body aus den zwei Kreisen mit dem Befehl Create -> Loft. Danach solltet ihr einen konisch zulaufenden Zylinder haben, dieser hat bisher jedoch noch kein Loch in der MItte.

Erstellt dafür auf dem schmaleren Ende (37.8mm) einen weiteren Kreis Sketch mit dem Durchmesser von 32mm - somit bleibt ein Rand von 2.9mm stehen. Auf der anderen Seite (50mm) erstellt ihr den gewünschten Innendurchmesser mit 44.2mm - hier bleiben also auch 2.9mm als Wandstärke stehen.

Jetzt verbindet ihr beide Sketches wieder mit dem Create -> Loft Befehl und wählt Cut als Option. Wenn alles geklappt hat, solltet ihr einen konische zulaufenden Zylinder mit einer durchgehenden Wandstärke von 2.9mm erhalten haben.

Man könnte meinen, dass sich der gleiche Effekt mit Modify -> Shell erzielen ließe. Dem ist jedoch (leider) nicht so. Shell höhlt den Körper zwar aus, dabei bleiben jedoch immer der Deckel und der Boden des Körpers bestehen. Versucht man nun, den Deckel negativ zu extrudieren, bleibt die Wandstärke logischerweise nicht gleich – auf einer Seite wird die Wandstärke nicht passen.

Je nach Bedarf könnt ihr jetzt noch mit Modify -> Fillet oder Modify -> Chamfer die Ecken abrunden. Meiner Erfahrung nach ist das jedoch nicht notwendig. Es könnte bei sehr großen Adaptern relevant an der Innenseite werden, sodass ein gleichbleibender Luftstrom ohne Verwirbelungen erreicht werden kann.
