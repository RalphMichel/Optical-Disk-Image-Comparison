# Optical-Disk-Image-Comparison
Im Rahmen des Moduls "Conservation of Software-based Art" wurden verschiedene Werkzeuge zum Erstellen von Disk-Images von optischen Datenträgern getestet. Im Vordergrund steht der Vergleich,  bereits in der Medienrestaurierung etablierten Werkzeuge wie "ddrescue", "dvdisaster" oder "guymager" mit dem relativ neuen Werkzeug "aaru" durchzuführen. 

Für die Versuche wurde folgendes Laufwerk verwendet: **ASUS SDRW-08D2S-U, Rev: B912**,  Verbunden via USB. Aussagen, wie akkurat das Laufwerk die optischen Datenträger ausliest können keine gemacht werden. Jedoch wurden alle Programme mit demselben Laufwerk geprüft. Eine Übersicht über die Laufwerke, welche am akkurastesten sind finden sich im folgendem Forum: https://forum.dbpoweramp.com/showthread.php?43786-CD-Drive-Accuracy-2019

Als Test-CD wurde ein Computerspiel (Pitch n Putt) gewählt, da sich diese Art von CDs für ein Disk-Image anbieten. Anders sieht es bei reinen Audio-CDs aus. Dafür gibt es andere Programme, mit denen die Audio-Informationen besser und exakter ausgelesen werden können. Bspw. mit dem Programm "Exact Audio Copy" https://www.exactaudiocopy.de

Die Versuche wurden mit dem Betriebssystem: **Debian GNU/Linux 11** (bullseye) durchgeführt.

## Arbeitsablauf

### Informationen zum optischen Datenträger und zum Laufwerk

Zunächst wurden Informationen zum optischen Datenträger sowie zum Laufwerk zusammengetragen. Diese Informationen finden sich in Text-Dateien im Ordern "Infofiles". 

Insgesamt wurden drei Info-Dateien mit den folgenden Programmen und Befehlen erstellt:

**cd-info** (für weitere Infos: `cd-info -h`)
Zeig Informationen sowohl zum verwendeten Laufwerk als auch zum optischen Datenträger an. 

`cd-info /dev/sr0`

**cdrdao** https://cdrdao.sourceforge.net/

Zeigt ebenfalls Informationen zum Laufwerk an, jedoch deutlicher weniger Informationen zum optischen Datenträger als dies bei `cd-info` der Fall ist.

`cdrdao disk-info /dev/sr0`

**disktype** https://disktype.sourceforge.net/

Zeigt nur Informationen zum optischen Datenträger an, jedoch keine Mehrinformationen im Vergleich zu `cd-info`. 

`disktype /dev/sr0`



#### Auswertung

Wie bereits oben erwähnt wurde das Laufwerk als ASUS SDRW-08D2S-U, Rev: B912 erkannt. 
Auf der CD befindet sich **1 Track** im Dateisystem **ISO9660**. Die Grösse beträgt 108.2 MiB (113430528 bytes).



## Infofiles

Zunächst wurden 

Programme welche getestet wurden:

- dd
- ddrescue
- dvdisaster
- guymager
- aaru

`aaru image compare Image1.iso Image2.iso`



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



**aaruf Format**

- Stores the most amount of data compared to any other image format for supported media.
- Uses c**ompression** to take up the least space possible compared to any other format.
- Can be used to store data from any type of media supported by Aaru.
- Can be converted to many formats supported by Aaru, so you can easily create an ISO (or other image types) for software that doesn’t support the Aaru Image Format.



Bei der Konvertierung von aaruf zu iso resultiert eine identische Datei wie alle anderen ISO-Dateien (ausgenommen diejenige von dvdisaster). Verwendeter Befehl zur Konvertierung



```bash
./aaru image convert -c 32 --comments "Converted Aaru Image" --creator "Ralph Michel" --drive-manufacturer "ASUS" --drive-model "SDRW-08D2S-U" --drive-revision "B912" --drive-serial "KCD0AP011680"   --media-title "Pitch n Putt Game" -f -O "deduplicate=true,nocompress=false" -r Image.resume.xml -x Image.cicm.xml aaru_Image.aaruf dd_dump.iso
```

