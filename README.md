# Optical-Disk-Image-Comparison
m Rahmen des Moduls "Conservation of Software-based Art" wurden verschiedene Werkzeuge zum Erstellen von Disk-Images von optischen Datenträgern getestet. Im Vordergrund steht der Vergleich,  bereits in der Medienrestaurierung etablierten Werkzeuge wie "ddrescue", "dvdisaster" oder "guymager" mit dem relativ neuen Werkzeug "aaru" durchzuführen. 

Für die Versuche wurde folgendes Laufwerk verwendet: **ASUS SDRW-08D2S-U, Rev: B912**,  Verbunden via USB.
Aussagen, wie akkurat das Laufwerk die optischen Datenträger ausliest können keine gemacht werden. Jedoch wurden alle Programme mit demselben Laufwerk geprüft. Eine Übersicht über die Laufwerke, welche am akkurastesten sind finden sich im folgendem Forum: https://forum.dbpoweramp.com/showthread.php?43786-CD-Drive-Accuracy-2019

 Die Versuche wurden mit dem Betriebssystem: **Debian GNU/Linux 11** (bullseye) durchgeführt.

Als Test-CD wurde ein Computerspiel (Pitch n Putt) gewählt, da sich diese Art von CDs für ein Disk-Image anbieten. Anders sieht es bei reinen Audio-CDs aus. Dafür gibt es andere Programme, mit denen die Audio-Informationen besser und exakter ausgelesen werden können. Bspw. mit dem Programm "Exact Audio Copy" https://www.exactaudiocopy.de

## Arbeitsablauf

Zunächst wurden Informationen zum optischen Datenträger zusammengetragen. Diese Informationen finden sich in Text-Dateien im Ordern "Infofiles". 

## Infofiles

Zunächst wurden 

Programme welche getestet wurden:

- ddrescue
- dvdisaster
- guymager
- aaru
