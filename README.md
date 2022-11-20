# Optical Disk-Image Comparison

Im Rahmen des Moduls "Conservation of Software-based Art" wurden verschiedene Werkzeuge zum Erstellen von Disk-Images von optischen Datenträgern getestet. Im Vordergrund steht der Vergleich,  bereits in der Medienrestaurierung etablierten Werkzeuge wie "ddrescue", "dvdisaster" oder "guymager" mit dem relativ neuen Werkzeug "aaru" durchzuführen. 

Für die Versuche wurde folgendes USB- Laufwerk verwendet: **ASUS SDRW-08D2S-U, Rev: B912**,  Aussagen, wie akkurat das Laufwerk die optischen Datenträger ausliest können keine gemacht werden. Jedoch wurden alle Programme mit demselben Laufwerk geprüft und die CD mehrmals mit demselben Programm ausgelesen. Die Ergebnisse innerhalb eines Programmes waren immer identisch. Eine Übersicht über die Laufwerke, welche am akkuratesten sind, finden sich im folgendem Forum: https://forum.dbpoweramp.com/showthread.php?43786-CD-Drive-Accuracy-2019

Als Test-CD wurde ein Computerspiel (Pitch n Putt) gewählt, da sich diese Art von CDs für ein Disk-Image anbieten. Dies aus dem Grund, weil die CD nicht nur Audio- und Videodaten enthält sondern auch Software, welche eine Vielzahl von unterschiedlichen Dateien enthält und diese jeweils in Abhängigkeit zu anderen Dateien  stehen. Anders sieht es bei reinen Audio-CDs aus. Dafür gibt es andere Programme, mit denen die Audio-Informationen besser und exakter ausgelesen werden können. Bspw. mit dem Programm "Exact Audio Copy" https://www.exactaudiocopy.de

Die Versuche wurden mit dem Betriebssystem: **Debian GNU/Linux 11** (bullseye), **Linux 5.10.0.19 (64-bit)** durchgeführt. 

## Arbeitsablauf

### Informationen zum optischen Datenträger und zum Laufwerk

Zunächst wurden Informationen zum optischen Datenträger sowie zum Laufwerk zusammengetragen. Diese Informationen finden sich in Text-Dateien im Ordern "Infofiles". 

Insgesamt wurden drei Info-Dateien mit den folgenden Programmen und Befehlen erstellt:

**cd-info** (Version: 2.1.0) (für weitere Infos: `cd-info -h`) 
Zeig Informationen sowohl zum verwendeten Laufwerk als auch zum optischen Datenträger an. 

`cd-info /dev/sr0`

**cdrdao** (Version: 1.2.4) https://cdrdao.sourceforge.net/ 

Zeigt ebenfalls Informationen zum Laufwerk an, jedoch deutlicher weniger Informationen zum optischen Datenträger als dies bei `cd-info` der Fall ist.

`cdrdao disk-info /dev/sr0`

**disktype** (Version: 9-11 ) https://disktype.sourceforge.net/

Zeigt nur Informationen zum optischen Datenträger an, jedoch keine Mehrinformationen im Vergleich zu cd-info.

`disktype /dev/sr0`

#### Auswertung

Wie bereits oben erwähnt wurde das Laufwerk als ASUS SDRW-08D2S-U, Rev: B912 erkannt. 
Auf der CD befindet sich **1 Track** im Dateisystem **ISO9660**. Die Grösse beträgt gemäss disktype 108.2 MiB (113430528 bytes).



### Vergleich verschiedener Programme zum Erstellen eines Disk-Images, eines optischen Datenträgers

Verglichen wurde folgende Programme, welche alle unter Linux funktionieren: aaru, dd, ddrescue, dvdisaster und guymager. Nachfolgende wird auf jedes Programm hinsichtlich Funktionsumfang sowie Vor- und Nachteile kurz eingegangen.

#### aaru

Webseite: https://www.aaru.app/#/

Mit aaru lassen sich sowohl ISO-Dateien als auch eine aaru eigenes Format "aaruf" erzeugen. Vorteil von aaru ist, dass sehr viele Speichermedien ausgelesen werden können. Zudem können mit dem aaruf-Format Abbilder komprimiert gespeichert werden, was im Falle von guymager bspw. nur mit dem proprietären Format Advanced Forensic Format (AFF) möglich ist. aaru erstellt eine Log-Datei in der viele nützliche Metadaten, wie Informationen zum verwendeten Laufwerk, Programminformationen und den Erstellungsprozesses des Abbildes enthalten sind. 

Weiter lassen sich mit aaru auch zwei Abbilder vergleichen, unabhängig davon, ob diese mit aaru erstellt worden sind. Auch können mit aaru, Abbilder zu ISO-Dateien konvertiert werden. 

##### Verwendete Befehle

Erzeugen der ISO-Datei (mit Debian GNU/Linux 11, aaru Version: 5.3.1+a175d01f)

```bash
/usr/share/aaru/aaru.dll media dump /dev/sr0 Image.iso -f
```

Erzeugen der aaruf-Datei (mit Debian GNU/Linux 11, aaru Version: 5.3.1+a175d01f)

```bash
/usr/share/aaru/aaru.dll media dump /dev/sr0 Image.aaruf
```

Konvertierung der aaruf-Datei in eine ISO-Datei (mit macOS Ventura 13.0.1, aaru Version: 5.3.1)

```bash
./aaru image convert -c 32 --comments "Converted Aaru Image" --creator "Ralph Michel" --drive-manufacturer "ASUS" --drive-model "SDRW-08D2S-U" --drive-revision "B912" --drive-serial "KCD0AP011680"   --media-title "Pitch n Putt Game" -f -O "deduplicate=true,nocompress=false" -r Image.resume.xml -x Image.cicm.xml aaru_Image.aaruf dd_dump.iso
```



#### dd

Debian mangpage: https://manpages.debian.org/testing/manpages-de/dd.1.de.html

dd (disk dump) ist ein einfaches aber sehr mächtiges Programm für bit-genaues Kopieren von Speichermedien. Mittels dd lassen sich ISO-Dateien erstellen.

##### Verwendeter Befehl

Mit Debian GNU/Linux 11, dd Version: 4.10.0-1

```bash
sudo dd if=/dev/sr0 of=/media/ralph/test.iso
```



#### GNU ddrescue

Webseite: https://www.gnu.org/software/ddrescue/

GNU ddrescue ist ein Programm zur Datenrettung von defekten Datenträgern wie Festplatten, CD-ROMS, USB-Sticks etc. Vorteil dieses Programmes ist, dass im Befehl die Anzahl Versuche beim Auslesen des Speichermediums angegeben werden können. Dabei werden die fehlerhaften Sektoren mehrfach ausgelesen, was eine höhere Wahrscheinlichkeit mitsichbringt, die defekten Daten wiederherzustellen. Mittels ddrescue lassen sich ISO-Dateien erzeugen. 

##### Verwendeter Befehl

Mit Debian GNU/Linux 11, GNU ddrescue version 1.23

```bash
ddrescue -v -r 5 /dev/sr0 /home/ralph/Dokumente/Modul_Disk_Imaging/FOLLOW_UP/ASUS/ddrescue/ddrescue_PitchnPutt.iso /home/ralph/Dokumente/Modul_Disk_Imaging/FOLLOW_UP/ASUS/ddrescue/ddrescue_mapfile_PitchnPutt.txt
```



#### dvdisaster

Webseite: https://dvdisaster.jcea.es/

dvdisaster ist ein Programm, um optische Datenträger auszulesen. Vorteil dieses Programmes ist die grafische Benutzeroberfläche, welche einerseits die Lesegeschwindigkeit des Laufwerkes anzeigt und somit Rückschlüsse über die Qualität des Laufwerkes machen lassen. Andererseits wird eine Fehlerkorrektur-Datei (Error Correction Code, ECC) erstellt. Mit deren Hilfe lassen sich verlorene Daten aufgrund fehlerhafter Sektoren auf dem Datenträger wieder rekonstruieren. Mit dvdisaster lassen sich ISO-Dateien erzeugen.

Mit Debian GNU/Linux 11, dvdisater version 0.79.5-10



#### Guymager

Webseite: https://guymager.sourceforge.io/

Guymager ist ein Programm, welches in der Forensik zum Einsatz kommt und aus unterschiedlichen Speichermedien, Abbilder erzeugt. Dabei können sowohl dd-Datein (Raw-Image Datei vergleichbar mit einer ISO-Datei) erzeugen, als auch komprimiert Abbilder im proprietären Format Advanced Forensic Format (AFF), welches in der Forensik verbreitet ist. Guymager hat eine eingebaute Prüfsummenfunktion mit der es möglich ist, das Abbild auf dessen Integrität hin zu überprüfen. 

Mit Debian GNU/Linux 11, Guymager version 0.8.12-1 



### Ergebnisse

Die nachfolgende Tabelle liefert eine Übersicht, über die erstellen Abbilder, deren MD5-Prüfsumme sowie deren Dateigrösse und Anzahl der Sektoren. Zum Einen fällt auf, dass die aaruf-Datei kleiner ist alle die restlichen Abbilder. Dies hat damit zu tun, dass es sich beim aaruf-Format, wie oben beschrieben, um ein komprimiertes Abbild-Format handelt. 

Andererseits fällt auf, dass alle Abbilder hinsichtlich deren Prüfsumme und Dateigrösse identisch sind, bis auf das Abbild, welches mit dem Programm dvdisaster erstellt wurde. Diese Abbild ist kleiner und hat somit auch weniger Sektoren und dementsprechend eine andere Prüfsumme. 

| Programm         | Dateiname                 | MD5-Prüfsumme                    | Grösse (bytes) | Sektoren |
| ---------------- | ------------------------- | -------------------------------- | -------------- | -------- |
| aaru (aaruf)     | aaru_Image.aaruf          | 2411f4c1533e28e37418582deba34af5 | 109’301’388    | 55536    |
| aaru (iso)       | aaru_Image.iso            | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728    | 55536    |
| aaru (conv. iso) | dd_dump.iso               | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728    | 55536    |
| dd               | dd_PitchnPutt.iso         | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728    | 55536    |
| ddrescue         | ddrescue_PitchnPutt.iso   | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728    | 55536    |
| dvdisaster       | dvdisaster_PitchnPutt.iso | dba625775ae7e24cbedef278164787f9 | 113’430’528    | 55386    |
| guymager         | guymager_PitchnPutt.dd    | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728    | 55536    |

#### Vergleich mit Infofiles

Informationen zur Dateigrösse und den Anzahl der Sektoren liefert cd-info und disktype. cd-info besagt, dass die CD 55536 Sektoren sowie eine Dateigrösse von 108 MB hat. Weiter unten unter "CD Analysis Report" steht dann "55386 blocks" im ISO 9660 Dateisystem.

disktype  ist bezüglich Dateigrösse genauer und besagt eine Grösse von 113737728 bytes für die CD. Im ISO 9660 Dateisystem beträgt die Dateigrösse 113430528 bytes und 55386 blocks.

Verglichen mit der obigen Tabelle zeigt sich, dass dvdisaster, nur die im ISO 9960 vorliegenden Daten ausliest. Alle anderen Programme lesen den gesamten Track aus. Dies trifft ebenfalls für das komprimierte aaruf Format zu, wenn die aaruf-Datei in eine ISO-Datei konvertiert wird (siehe in Tabelle: aaru conv.iso).

Der Unterschied beträgt 307200 bytes (entspricht 300kB)

**Vergleich Sectors**

dvdisaster = 55386
Restliche = 55536

1 sector = 2048 bytes

55536 - 55386 = 150

<u>150 x 2048 = 307200</u>



**Vergleich Dateigrösse**

dvdisaster = 113430528 byte
Restliche = 113737728 byte

<u>113737728 - 113430528 = 307200</u>





#### HEX Vergleich

**ddrescue vs. dvdisaster**

Beyond Compare Version 4.4.4 (https://www.scootersoftware.com/)

Neben dem Vergleich mit den Informationen aus den Infofiles wurden die Abbilder mit einem HEX-Editor verglichen. Wie zu erwarten, waren sind alle Abbilder zueinander identisch bis auf dasjenige von dvdisaster. Die oben berechneten Unterschied von 300kB enthalten jedoch keine Daten sondern sind bei allen Abbildern lediglich mit 0 gefüllt. Möglicherweise handelt es sich dabei um den leadout der CD.

Exemplarisch für die identischen Abbilder, wurde der HEX-Vergleich zwischen dem ddrescue-Abbild und dem dvdisaster-Abbild als HTML-Datei exportiert und als "hex_compare_ddresuce_vs_dvdisaster.html" im Repository eingefügt. 

dvdisaster = 307200 bytes weniger als die restlichen Abbilder
Restliche= 307200 bytes mit 0 gefüllt.



#### **Vergleich mit aaru compare**

Mit dem Programm aaru lassen sich wie eingangs erwähnt, zwei Abbilder miteinander vergleichen, unabhängig davon, ob diese mit aaru erstellt worden sind. Diese Vergleiche sind als Text-Dateien im Ordner aaru_image_compare zu finden. 

Foldender Befehl wurde für die Vergleiche verwendet:

`aaru image compare Image1.iso Image2.iso`

Auch hier zeigt sich ein vergleichbares Bild, wie mit dem HEX Vergleich. Die exportieren Ergebnisse sind im Ordner "aaru_image_compare" zu finden. Bei diesem Vergleich wurden alle Sektoren der jeweiligen Abbilder zueinander verglichen. 



#### **Fiwalk Vergleich** 

**ddrescue vs. dvdisaster** 
Mit Debian GNU/Linux 11, Fiwalk Version 4.10.1

Um Informationen zum Inhalte der jeweiligen Abbilder zu erhalten wurde mit dem Programm Fiwalk ein XML-Export erstellt. Darin enthalten sind alle Dateien, welche im jeweiligen Abbild gespeichert sind. Zudem erstellt Fiwalk eine MD5- sowie eine SHA1-Prüfsumme für jede Datei.

Hierbei wurde ebenfalls exemplarisch für die identischen Abbilder, das Abbild von ddrescue mit demjenigen von dvdisaster verglichen. Die exportierte HTML-Datei, welche die Unterschiede zeigt ist unter "fiwalk_compare_ddrescue_vs_dvdisaster.html" zu finden. 

Inhaltlich sind die beiden Abbilder identisch. Die einzigen Unterschiede zeigen sich bei den Angaben zum verwendeten Programm mit dem das Abbild erstellt wurde sowie der Entstehungszeit des Abbildes. 

## Fazit

Der Vergleich zeigt zwei Haupterkenntnisse. Einerseits lassen sich im aaru-Format aaruf, komprimierte Abbilder in einem Open-Source Format erstellen. Diese Abbilder sind  beim Konvertieren in eine ISO-Datei, identisch mit denjenigen, welche mit den meisten anderen in der Medienrestaurierung verwendeten Programmen erzeugt wird.

Andererseits hat der Vergleich aufgezeigt, dass mit dem Programm dvdisaster "nur" der Datentrack als Abbild gespeichert wird. Inhaltlich sind die Abbilder mit dvdisaster aber identisch mit denjenigen der anderen getesteten Programmen.







