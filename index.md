Welcome to our UAV Blog!
# Einleitung

Die folgende Arbeit soll eine Möglichkeit zur Volumenberechnung von komplexen dreidimensionalen Strukturen mit Hilfe von low budget UAV’s aufzeigen.

Genutzt werden könnte das aufgezeigte Verfahren für eine Changedetection von historischen Gebäuden, oder aber zur prozentualen Bestimmung einer Gesteinszusammensetzung eines historischen Objekts.

Zunächst wurde die Frauenberg Ruine mit einer DJI Mini 2 händisch überflogen. Ziel war es, ein schemenhaftes Modell der Ruine anzufertigen, um mit Agisoft einen reproduzierbaren Flugplan für Detailaufnahmen zu erstellen. Einen Flugplan auf Basis des Modells zu erstellen, ermöglicht ein relativ umfassendes dreidimensionales Abfliegen von komplexen Strukturen und Objekten.

# Optimierte Flugplanung via Agisoft und Litchi

Hierfür wurde die Ruine, wie in dem Screenshot zu sehen, als *zone of interest* kategorisiert. Dies ist mit der Funktion möglich:

<img src="/images/anleitung_volume/image1.png" style="width:6.3in;height:3.43284in" />

Im nächsten Schritt kann über den Reiter *Tools \>\> Plan Misson* ein Flugplan erstellt werden.

Bei der Einstellung *Focus on interesting Zone* sollte das zuvor markierte und zu untersuchende Objekt ausgewählt werden. Die Kameraeinstellungen erkennt Agisoft automatisch auf Basis der vorhandenen Bilder. Die Einstellungen können hier übernommen werden, sofern für den nächsten Flug die gleiche UAV verwendet wird.  
Es ist jedoch wichtig, je nach UAV, einen Sicherheitsabstand zum zu befliegenden Objekt einzustellen. Ohne RTK oder eine 360° Antikollisionssensorik findet sich bei der Recherche immer wieder die Empfehlung, aufgrund von GPS Ungenauigkeiten, einen Sicherheitsabstand von 10 Metern nicht zu unterschreiten. Auch das Relief, der Bewuchs, Überwuchs, Hochspannungsleitungen und Ähnliches sollten berücksichtigt werden, wobei die Limits an die jeweilige Situation angepasst werden, um Kollisionen beim automatisierten Flug zu vermeiden.

Je nach Bedarf können auch NoGo-Areale mit Hilfe der *Draw Polygon* Funktion deklariert werden.

<img src="/images/anleitung_volume/image2.png" style="width:6.3in;height:3.4403in" />

Via Rechtsklick auf Mission plan kann der Flugplan als CSV-Datei exportiert und in Litchi hochgeladen werden.

<img src="/images/anleitung_volume/image3.png" style="width:6.3in;height:3.39605in" />

In Litchi selbst können die Waypoints noch modifiziert werden. Vor allem der Home-Punkt sollte angepasst werden. Mit der unmodifizerten CSV-Datei lässt sich ansonsten bereits ein relativ effizienter Flugplan durchführen.

Im Vergleich zu einer Flugplanung mit qGroundcontrol scheint Agisoft hier leicht überlegen, sobald es darum geht Flugplanungen an komplexen Objekten für 3D Modelle durchzuführen.

Die Waypoints haben, auf Basis der trigonometrisch berechneten optimalen Position, für die Kamera bereits mehrere Actions wie: „*Take Photo, Rotate Aircraft, Tilt Camera, Take Photo“,* bevor der nächste Waypoint angeflogen wird.

<img src="/images/anleitung_volume/image4.png" style="width:6.3in;height:3.11998in" />

Die Detailaufnahmen werden wieder in Agisoft aligned und es wird eine Dense Cloud erstellt. Je nach Rechenleistung und Verarbeitungsmöglichkeiten sollte hier die entsprechende Qualität gewählt werden.

<img src="/images/anleitung_volume/image5.png" style="width:6.06047in;height:3.30232in" />

# Exportieren der PointCloud aus Agisoft in CloudCompare

Als nächsten Arbeitsschritt exportieren wir die DenseCloud aus Agisoft in CloudCompare. Dazu speichert man die DenseCloud ganz einfach als eine taugliche CloudCompare Datei, z.B. mit der Endung „*.laz*“.  
*Rechtsklick auf* *dense cloud \>\> export dense cloud \>\> speichern als .laz*

<img src="/images/anleitung_volume/image6.png" style="width:6.3in;height:3.54375in" alt="C:\Users\roman\Documents\Uni\Geo\SoSe22\Projekt 2 Mikrofernerkundung\screens\first.png" />

# Einladen in CloudCompare und Preprocessing

Die .laz-Datei öffnet man dann in CloudCompare über *File \>\> open \>\> Datei auswählen.* Das sich anschließend öffnende Fenster kann zweimal mit *apply* bestätigt werden.

<img src="/images/anleitung_volume/image7.png" style="width:4.20135in;height:3.08664in" />

<img src="/images/anleitung_volume/image8.png" style="width:4.12304in;height:2.91685in" />

Nun sollte die Datei erfolgreich eingeladen sein. Im nächsten Schritt kann man nun das jeweilige Objekt etwas genauer ausschneiden – dadurch minimiert man potentielle Fehler bei der späteren Klassifizierung. Dieser Schritt funktioniert theoretisch auch mit Agisoft, ist jedoch vom Handling angenehmer in CloudCompare. Für uns war dieser Schritt ohnehin unausweichlich, da die Pixelfarbe des Bodens um die Ruine herum der Ruine selbst stark ähnelte. Eine Klassifizierung der Ruine beinhaltete also stets den umliegenden Boden, so dass wir diesen grob entfernten. Dazu wählt man die Schere in der Konsole aus und wählt mit Linksklick ein Polygon aus. Beendet wird das Polygon mit einem Rechtsklick und einem Klick auf Segment in.

*Schere \>\> Polygon mit Linksklick erstellen \>\> Polygon mit Rechtsklick abschließen \>\> Segment in*

<img src="/images/anleitung_volume/image9.png" style="width:6.3in;height:0.49375in" />

<img src="/images/anleitung_volume/image10.png" style="width:6.3in;height:3.02014in" />

Die Auswahl mit dem *Grünen Haken* bestätigen. Nun hat man zwei Punktwolken – *remaining* und *segmented*. *Segmented* ist der Teil den man im Vorfeld ausgeschnitten hat – der Teil der also von Interesse ist.

<img src="/images/anleitung_volume/image11.png" style="width:3.53739in;height:1.67272in" />

# Canupo Trainer – trainieren und klassifizieren

Nach der ersten groben Bereinigung des Objektes geht es nun an die automatische Klassifizierung der einzelnen Bildelemente. Dazu weisen wir zunächst einzelnen Bildbereichen bestimmte Werte zu. Das machen wir wieder über das Scherensymbol und dem Zeichnen von Polygonen. Danach klicken wir auf die *Rautetaste* und geben einen Wert ein, z.B. 1.

*Schere \>\> Polygon mit Linksklick erstellen \>\> Polygon mit Rechtsklick abschließen \>\> Raute*

<img src="/images/anleitung_volume/image9.png" style="width:6.3in;height:0.49375in" />

<img src="/images/anleitung_volume/image12.png" style="width:6.3in;height:4.07905in" alt="C:\Users\roman\Documents\Uni\Geo\SoSe22\Projekt 2 Mikrofernerkundung\screens\class_3.png" />

Nun haben wir diesem Bereich und den jeweils darin liegenden Bildpunkten den Wert 1 zugeschrieben. Für den Rest der Mauer verfährt man genauso. Dabei achtet man darauf, dass auch individuelle Erscheinungen der Mauer, wie z.B. ein besonders heller Stein oder ein Stein im Schatten miterfasst wird.

Vegetation und farblich eindeutig unterscheidbare Bereiche können durch die Zuwahl weiterer Werte erfasst werden. Wichtig ist dabei, dass man darauf achtet, eine möglichst eindeutige Auswahl zu treffen. Vegetation würde z.B. den Wert 2 bekommen. Bei diesem Schritt muss man wirklich sehr sorgfältig und zeitintensiv dranbleiben, da die Erfassung für die spätere automatische Auswertung maßgebend ist.

Sind die jeweils interessanten Bereiche erfasst und mit Werten klassifiziert, wählt man anschließend über:

*Edit \>\> Scalar fields \>\> Split cloud (integer values)*

eine Teilung der Objektwolke in die verschiedenen Werte aus. Dabei kann man nochmal überprüfen wie sauber man die einzelnen Bildelemente ausgeschnitten hat.

<img src="/images/anleitung_volume/image13.png" style="width:6.3in;height:3.07569in" />

Nun hat das Programm beispielhafte Bildpunkte mit bestimmten Werten verknüpft. Anschließend wird das **Plugin Canupo** dafür benutzt, das Bild automatisch nach weiteren Bildpunkten zu durchsuchen, um ihnen, sofern sie vorhanden sind, denselben Wert zuzuschreiben. Zunächst Folgendes auswählen:

*Plugin \>\> CANUPO \>\> Train classifier*

<img src="/images/anleitung_volume/image14.png" style="width:6.3in;height:1.52222in" />

Nun wird eine Filter-Datei erstellt. Hierbei wird nochmals eine Verknüpfung erstellt zwischen den im vorherhigen Schritt durch Eingabe von Werten erstellen Klassifizierungen und einer zu erstellenden Klasse durch das Plugin Canupo. Im Programm sieht das so aus:

<img src="/images/anleitung_volume/image15.png" style="width:6.3in;height:3.74097in" />

Nun weist man den Canupo-Klassen die Werte aus den vorheringen Klassifizierungen zu. Welcher Klasse man welche Werte gibt ist im Prinzip egal, wichtig ist, dass man sich die jeweils interessante Klasse merkt. Dann mit *ok* bestätigen.

Nach einer kurzen Arbeitsphase erscheint ein neues Fenster:

<img src="/images/anleitung_volume/26.png" style="width:4.07861in;height:3.29993in" />

Hier zeigt das Plugin, wie viele der Bildpunkte durch die vorher ausgewählten Bildausschnitte erfasst wurden und wie eindeutig die Trennlinie dabei zwischen den Objekten ist. Unter *statistics* kann man sich weitere Hintergründe dazu anschauen.

Mit Klick auf *Save* wird nun, nach Eingabe eines Dateinamens, eine *.rmp*-Datei gespeichert. Im nächsten Schritt wird diese über *Plugins \>\> CANUPO \>\> Classify* wieder hochgeladen und als Filter auf die Punktwolke eingesetzt.

<img src="/images/anleitung_volume/image17.png" style="width:5.68383in;height:1.35012in" />

Dann öffnet sich folgendes Fenster:

<img src="/images/anleitung_volume/image18.png" style="width:2.04943in;height:3.14247in" />

Wichtig ist hierbei, dass man bei der Aktivierung des Kästchens „*use selected cloud“* die ganze Wolke vorher ausgewählt hat und nicht nur einen Teil, bspw. durch Auswahl eine der vorher erstellten Wertklassen.

Nun wird für alle Bildpunkte der Wert des vorher definierten Filters errechnet. Die Rechnungen dahinter sind sehr komplex und an dieser Stelle in der Gänze von uns nicht reproduzierbar. Das ist auch nicht nötig, sie funktionieren trotzdem. In jedem Fall wird der Bildpunkt nicht nur nach dem RGB-Farbton bestimmt, sondern auch mit den Bildpunkten der nächsten Nachbarn verglichen und eine iterative Interpolation durchgeführt.

Das Ergebnis zeigt die automatisch errechneten Bildpunkte mit den zwei Werteklassen. Diese werden zur besseren Sichtbarkeit in kontrastreichen Farben abgebildet. Durch eine abschließende Extraktion der Bildpunkte mit dem vorher festgelegten, bestimmten Wert erhält man die endgültige Punktwolke.

<img src="/images/anleitung_volume/25.png" style="width:3.72407in;height:2.30045in" />

<img src="/images/anleitung_volume/image20.png" style="width:2.3in;height:1.51667in" />

<img src="/images/anleitung_volume/image21.png" style="width:2.52083in;height:1.46875in" />

Diese kann dann zur weiteren Volumenberechnung als *.ply-Datei* gespeichert werden, um sie wieder in Agisoft einzuladen. Die Volumenberechnung ist theoretisch auch in CloudCompare durchführbar, allerdings ist die Erstellung des mesh sehr von empirischen Werten abhängig. Die Erstellung in Agisoft ist nach unserer Ansicht intuitiver. Nichts desto trotz ist der beschriebene Weg ebenfalls in CloudCompare möglich.

# Erstellen eines Mesh und Volumenberechnung Agisoft

Die Arbeitsschritte in CloudCompare waren nötig, damit die Dense Cloud gereinigt und clean ist. Nun sind die störenden Bildpunkte soweit es geht entfernt worden und übrig bleibt die „raw Ruine“. Damit aus der Punktwolke ein geschlossenes Modell wird, dessen Volumen bestimmt werden kann wird zunächst ein Mesh erstellt. Anschließend müssen etwaige vorhandene Löcher mit *close holes* durch Interpolation geschlossen werden. Nun kann als letzter Schritt das Volumen errechnet werden:

<img src="/images/anleitung_volume/22.png" style="width:6.52083in;height:4.46875in" />


Abschließen kann mit unter *Tools >> Measure Area and Volume* das Volumen ausgegeben werden. 


<img src="/images/anleitung_volume/24.png" style="width:6.52083in;height:4.46875in" />

Folgendes Modell stellt die Grundlage der Berechnung dar:

<div class="sketchfab-embed-wrapper"> <iframe title="model" frameborder="0" allowfullscreen mozallowfullscreen="true" webkitallowfullscreen="true" allow="autoplay; fullscreen; xr-spatial-tracking" xr-spatial-tracking execution-while-out-of-viewport execution-while-not-rendered web-share src="https://sketchfab.com/models/97475298de2945a989f3c98c91e146b1/embed"> </iframe> </div>





# Tipps für eine erfolgreiche Durchführung

Besondere Probleme bei der Erstellung einer geeigneten Pointclound sind durch Schattenwurf und windige Verhältnisse aufgetreten. Bei dispersen Lichtverhältnisse und minimalem Wind sollte sohin geflogen werden. Hierdurch wird die Berechnung der Pointcloud deutlich homogener, was die anschließende Volumenberechnung stark vereinfacht.

In Agisoft müssen in einem der letzten Schritte, vor der eigentlichen Berechnung, die Löcher geschlossen werden. Bei hohem Schattenwurf wird das Modell nicht ganz geschlossen und es entstehen offene Stellen, welche mithilfe des vorgestellten Verfahrens ungenau interpoliert werden müssen.



# Verweise

Close holes description for Agisoft:  
<https://agisoft.freshdesk.com/support/solutions/articles/31000166942-mesh-based-volume-measure>

Mission planning with Agisoft (complex structures); detailled description:

<https://agisoft.freshdesk.com/support/solutions/articles/31000157953-mission-planning-for-complex-structures>

CANUPO training – cloud compare

<https://www.cloudcompare.org/doc/wiki/index.php/CANUPO_(plugin)>

<https://www.youtube.com/watch?v=8ryh_QyUqH8>

Create own website:

<https://www.fast.ai/2020/01/16/fast_template/>

<https://www.fast.ai/2020/01/18/gitblog/>

Autoren:

Philip Ring, Roman Welker, Leon Wenz

Bei Fragen und Anregungen meldet euch gerne!
