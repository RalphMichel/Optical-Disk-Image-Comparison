# Optical-Disk-Image-Comparison
m Rahmen des Moduls "Conservation of Software-based Art" wurden verschiedene Werkzeuge zum Erstellen von Disk-Images von optischen Datenträgern getestet. Im Vordergrund steht der Vergleich,  bereits in der Medienrestaurierung etablierten Werkzeuge wie "ddrescue", "dvdisaster" oder "guymager" mit dem relativ neuen Werkzeug "aaru" durchzuführen. 

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

- ddrescue
- dvdisaster
- guymager
- aaru

`aaru image compare Image1.iso Image2.iso`

