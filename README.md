# Optical-Disk-Image-Comparison
Im Rahmen des Moduls "Conservation of Software-based Art" wurden verschiedene Werkzeuge zum Erstellen von Disk-Images von optischen Datenträgern getestet. Im Vordergrund steht der Vergleich,  bereits in der Medienrestaurierung etablierten Werkzeuge wie "ddrescue", "dvdisaster" oder "guymager" mit dem relativ neuen Werkzeug "aaru" durchzuführen. 

Für die Versuche wurde folgendes Laufwerk verwendet: **ASUS SDRW-08D2S-U, Rev: B912**,  Verbunden via USB. Aussagen, wie akkurat das Laufwerk die optischen Datenträger ausliest können keine gemacht werden. Jedoch wurden alle Programme mit demselben Laufwerk geprüft. Eine Übersicht über die Laufwerke, welche am akkurastesten sind finden sich im folgendem Forum: https://forum.dbpoweramp.com/showthread.php?43786-CD-Drive-Accuracy-2019

Als Test-CD wurde ein Computerspiel (Pitch n Putt) gewählt, da sich diese Art von CDs für ein Disk-Image anbieten. Anders sieht es bei reinen Audio-CDs aus. Dafür gibt es andere Programme, mit denen die Audio-Informationen besser und exakter ausgelesen werden können. Bspw. mit dem Programm "Exact Audio Copy" https://www.exactaudiocopy.de

Die Versuche wurden mit dem Betriebssystem: **Debian GNU/Linux 11** (bullseye), **Linux 5.10.0.19 (64-bit)** durchgeführt. 

## Arbeitsablauf

### Informationen zum optischen Datenträger und zum Laufwerk

Zunächst wurden Informationen zum optischen Datenträger sowie zum Laufwerk zusammengetragen. Diese Informationen finden sich in Text-Dateien im Ordern "Infofiles". 

Insgesamt wurden drei Info-Dateien mit den folgenden Programmen und Befehlen erstellt:

**cd-info** (Version:) (für weitere Infos: `cd-info -h`) 
Zeig Informationen sowohl zum verwendeten Laufwerk als auch zum optischen Datenträger an. 

`cd-info /dev/sr0`



**cdrdao** (Version: ) https://cdrdao.sourceforge.net/ 

Zeigt ebenfalls Informationen zum Laufwerk an, jedoch deutlicher weniger Informationen zum optischen Datenträger als dies bei `cd-info` der Fall ist.

`cdrdao disk-info /dev/sr0`



**disktype** (Version: ) https://disktype.sourceforge.net/

Zeigt nur Informationen zum optischen Datenträger an, jedoch keine Mehrinformationen im Vergleich zu `cd-info`. 

`disktype /dev/sr0`



#### Auswertung

Wie bereits oben erwähnt wurde das Laufwerk als ASUS SDRW-08D2S-U, Rev: B912 erkannt. 
Auf der CD befindet sich **1 Track** im Dateisystem **ISO9660**. Die Grösse beträgt gemäss disktype 108.2 MiB (113430528 bytes).

​	

### Vergleich verschiedener Programme zum Erstellen eines Diks-Images eines optischen Datenträgers

Verglichen wurde folgende Programme, welche alle unter Linux laufen: aaru, dd, ddrescue, dvdisaster und guymager. Nachfolgende wird auf jedes Programm hinsichtlich Funktionsumfang sowie Vor- und Nachteile eingegangen.

#### aaru

Webseite: https://www.aaru.app/#/

Mit aaru lassen sich sowohl ISO-Dateien als auch eine aaru eigenes Format "aaruf" erzeugen. Vorteil von aaru ist, dass sehr viele Speichermedien ausgelesen werden können. Zudem können mit dem aaruf-Format Abbilder komprimiert gespeichert werden, was im Falle von guymager bspw. nur mit dem proprietären Format Advanced Forensic Format (AFF) möglich ist. aaru erstellt eine Log-Datei in der viele nützliche Metadaten, wie Informationen zum verwendeten Laufwerk, Programminformationen und den Erstellungsprozesses des Abbildes enthalten sind. 

Weiter lassen sich mit aaru auch zwei Abbilder vergleichen, unabhängig davon, ob diese mit aar erstellt worden sind. Auch können mit aaru Abbilder zu ISO-Dateien konvertiert werden. 

##### Verwendete Befehle:

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

dd (disk dump) ist ein einfaches aber sehr mächtiges Programm für bit-genaues Kopieren von Speichermedien. Mittels dd lassen sich ISO-Dateien erstellen.

##### Verwendeter Befehl:

Mit Debian GNU/Linux 11

```bash
sudo dd if=/dev/sr0 of=/media/ralph/test.iso
```



#### GNU ddrescue

Webseite: https://www.gnu.org/software/ddrescue/

GNU ddrescue ist ein Programm zur Datenrettung von defekten Datenträgern wie Festplatten, Cdroms, USB-Sticks etc. Vorteil dieses Programmes ist, dass im Befehl die Anzahl Versuche beim Auslesen des Speichermediums angegeben werden können. Dabei werden die fehlerhaften Sektoren mehrfach ausgelesen, was eine höhere Wahrscheinlichkeit mitsichbringt, die defekten Daten wiederherzustellen. Mittels ddrescue lassen sich ISO-Dateien erzeugen. 

##### Verwendeter Befehl:

Mit Debian GNU/Linux 11, GNU ddrescue version 1.23

```bash
ddrescue -v -r 5 /dev/sr0 /home/ralph/Dokumente/Modul_Disk_Imaging/FOLLOW_UP/ASUS/ddrescue/ddrescue_PitchnPutt.iso /home/ralph/Dokumente/Modul_Disk_Imaging/FOLLOW_UP/ASUS/ddrescue/ddrescue_mapfile_PitchnPutt.txt
```



#### dvdisaster

Webseite: https://dvdisaster.jcea.es/

dvdisaster ist ein Programm um optische Datenträger auszulesen. Vorteil dieses Programmes ist die grafische Benutzeroberfläche, welche einerseits die Lesegeschwindigkeit des Laufwerkes anzeigt und somit Rückschlüsse über die Qualität des Laufwerkes machen lassen. Andererseits wird eine Fehlerkorrektur-Datei (Error Correction Code, ECC) erstellt. Mit deren Hilfe lassen sich verlorene Daten aufgrund fehlerhafter Sektoren auf dem Datenträger wieder rekonstruieren. Mit dvdisaster lassen sich ISO-Dateien erzeugen.

Mit Debian GNU/Linux 11, dvdisater version ?



#### guymager

Webseite: https://guymager.sourceforge.io/

guymager ist ein Programm, welches in der Forensik zum Einsatz kommt und aus unterschiedlichen Speichermedien, Abbilder erzeugt. Dabei können sowohl dd-Datein (Raw-Image Datei vergleichbar mit einer ISO-Datei) erzeugen, als auch komprimiert Abbilder im proprietären Format Advanced Forensic Format (AFF), welches in der Forensik verbreitet ist. guymager hat eine eingebaute Prüfsummenfunktion mit der es möglich ist, das Abbild auf dessen Integrität hin zu überprüfen. 

Mit Debian GNU/Linux 11, guymager version 0.8.12-1 





### Ergebnisse



| Programm         | Dateiname                 | MD5-Prüfsumme                    | Grösse (bytes) |
| ---------------- | ------------------------- | -------------------------------- | -------------- |
| aaru (aaruf)     | aaru_Image.aaruf          | 2411f4c1533e28e37418582deba34af5 | 109’301’388    |
| aaru (iso)       | aaru_Image.iso            | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728    |
| aaru (conv. iso) | dd_dump.iso               | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728    |
| dd               | dd_PitchnPutt.iso         | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728    |
| ddrescue         | ddrescue_PitchnPutt.iso   | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728    |
| dvdisaster       | dvdisaster_PitchnPutt.iso | dba625775ae7e24cbedef278164787f9 | 113’430’528    |
| guymager         | guymager_PitchnPutt.dd    | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728    |



**Hex Vergleich: ddrescue vs. dvdisaster**

dvdisaster = 307200 bytes weniger.
ddrescue= 307200 bytes mit 0 gefüllt.

**Fiwalk Vergleich** **ddrescue vs. dvdisaster** 

Inhalte identisch, bis auf Angaben zum verwendeten Programm und der Erstellungszeit.



**aaru Abbild-Vergleich**

`aaru image compare Image1.iso Image2.iso`



**aaru Konvertierung**

Bei der Konvertierung von aaruf zu iso resultiert eine identische Datei wie alle anderen ISO-Dateien (ausgenommen diejenige von dvdisaster). Verwendeter Befehl zur Konvertierung



