# Optical Disk-Image Comparison

As part of the module "Conservation of Software-based Art", various tools for creating disk images of optical media were tested. The main focus was to compare tools already established in media restoration like "ddrescue", "dvdisaster" or "guymager" with the relatively new tool "aaru". 

The following USB drive was used for the tests: **ASUS SDRW-08D2S-U, Rev: B912**, statements how accurate the drive reads the optical media cannot be made. However, all programs were tested with the same drive and the CD was read several times with the same program. The results within a program were always identical. An overview of which drives are the most accurate can be found in the following forum: https://forum.dbpoweramp.com/showthread.php?43786-CD-Drive-Accuracy-2019.

As a test CD a computer game (Pitch n Putt) was chosen, because this kind of CDs are suitable for a disk image. This for the reason, because the CD contains not only audio and video data but also software, which contains a multiplicity of different files and these in each case in dependence to other files stand. The situation is different for pure audio CDs. For this there are other programs, with which the audio information can be read better and more exactly. For example with the program "Exact Audio Copy" https://www.exactaudiocopy.de.

The tests were made with the operating system: **Debian GNU/Linux 11** (bullseye), **Linux 5.10.0.19 (64-bit)**. 



## Workflow

### Information about the optical disk and the drive

First, information about the optical data carrier and the drive was collected. This information can be found in text files in the "Infofiles" folder. 

A total of three info files were created with the following programs and commands:

**cd-info** (Version: 2.1.0, for more info: `cd-info -h`)
Show information about both the drive used and the optical disk. 

`cd-info /dev/sr0`

**cdrdao** (version: 1.2.4) https://cdrdao.sourceforge.net/ 

Also displays information about the drive, but much less information about the optical disk than cd-info.

`cdrdao disk-info /dev/sr0`

**disktype** (version: 9-11 ) https://disktype.sourceforge.net/

Displays only information about the optical disk, but no additional information compared to cd-info.

`disktype /dev/sr0`



#### Evaluation

As mentioned above, the drive was recognized as ASUS SDRW-08D2S-U, Rev: B912. 
On the CD there is **1 track** in the file system **ISO9660**. The size is 108.2 MiB (113430528 bytes) according to disktype.



### Comparison of different programs for creating a disk-image of an optical disk.

The following programs were compared, all of which work under Linux: aaru, dd, ddrescue, dvdisaster and guymager. In the following, each program is briefly described in terms of its functionality and advantages and disadvantages.

#### aaru

Website: https://www.aaru.app/#/

With aaru it is possible to create ISO files as well as aaru's own format "aaruf". Advantage of aaru is that very many storage media can be read. In addition with the aaruf format images can be stored compressed, which is possible in the case of guymager for example only with the proprietary format Advanced Forensic format (AFF). aaru creates a log file in that many useful metadatas, like information to the used drive, program information and the creation process of the image are contained. 

Furthermore, aaru can also be used to compare two images, regardless of whether they were created with aaru. Also, with aaru, images can be converted to ISO files. 

##### Commands used

Creating the ISO file (with Debian GNU/Linux 11, aaru version: 5.3.1+a175d01f)

```bash
/usr/share/aaru/aaru.dll media dump /dev/sr0 Image.iso -f
```

Creating the aaruf file (with Debian GNU/Linux 11, aaru version: 5.3.1+a175d01f)

```bash
/usr/share/aaru/aaru.dll media dump /dev/sr0 Image.aaruf
```

Conversion of aaruf file to ISO file (with macOS Ventura 13.0.1, aaru version: 5.3.1)

```bash
./aaru image convert -c 32 --comments "Converted Aaru Image" --creator "Ralph Michel" --drive-manufacturer "ASUS" --drive-model "SDRW-08D2S-U" --drive-revision "B912" --drive-serial "KCD0AP011680"   --media-title "Pitch n Putt Game" -f -O "deduplicate=true,nocompress=false" -r Image.resume.xml -x Image.cicm.xml aaru_Image.aaruf dd_dump.iso
```



#### dd

Debian mangpage: https://manpages.debian.org/testing/manpages-de/dd.1.de.html

dd (disk dump) is a simple but very powerful program for bit-precise copying of storage media. With dd you can create ISO files.

##### Used command

With Debian GNU/Linux 11, dd version: 4.10.0-1

```bash
sudo dd if=/dev/sr0 of=/media/ralph/test.iso
```



#### GNU ddrescue

Website: https://www.gnu.org/software/ddrescue/

GNU ddrescue is a program for data recovery from defective storage media such as hard disks, CD-ROMS, USB sticks, etc. The advantage of this program is that the number of attempts to read the storage medium can be specified in the command. The defective sectors are read out several times, which means a higher probability of recovering the defective data. ISO files can be created using ddrescue. 

##### Used command

With Debian GNU/Linux 11, GNU ddrescue version 1.23

```bash
ddrescue -v -r 5 /dev/sr0 /home/ralph/Dokumente/Modul_Disk_Imaging/FOLLOW_UP/ASUS/ddrescue/ddrescue_PitchnPutt.iso /home/ralph/Dokumente/Modul_Disk_Imaging/FOLLOW_UP/ASUS/ddrescue/ddrescue_mapfile_PitchnPutt.txt
```



#### dvdisaster

Website: https://dvdisaster.jcea.es/

dvdisaster is a program to read optical media. The advantage of this program is the graphical user interface, which on the one hand shows the reading speed of the drive and thus allows conclusions to be drawn about the quality of the drive. On the other hand, an error correction code (ECC) file is created. With the help of this file, lost data due to bad sectors on the medium can be reconstructed. ISO files can be created with dvdisaster.

With Debian GNU/Linux 11, dvdisater version 0.79.5-10



#### Guymager

Website: https://guymager.sourceforge.io/

Guymager is a program that is used in forensics and creates images from different storage media. It can create both dd files (raw image file comparable to an ISO file), as well as compressed images in the proprietary format Advanced Forensic Format (AFF), which is widely used in forensics. Guymager has a built-in checksum function that makes it possible to verify the integrity of the image. 

With Debian GNU/Linux 11, Guymager version 0.8.12-1 



### Results

The following table provides an overview of the images created, their MD5 checksums, and their file size and number of sectors. Firstly, it is noticeable that the aaruf file is smaller than the rest of the images. This is due to the fact that the aaruf format is a compressed image format, as described above. 

On the other hand, all images are identical with respect to their checksum and file size, except for the image created with the dvdisaster program. This image is smaller and therefore has fewer sectors and a different checksum. 

| Program          | Filename                  | MD5-Checksum                     | Size(bytes) | Sectors |
| ---------------- | ------------------------- | -------------------------------- | ----------- | ------- |
| aaru (aaruf)     | aaru_Image.aaruf          | 2411f4c1533e28e37418582deba34af5 | 109’301’388 | 55536   |
| aaru (iso)       | aaru_Image.iso            | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728 | 55536   |
| aaru (conv. iso) | dd_dump.iso               | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728 | 55536   |
| dd               | dd_PitchnPutt.iso         | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728 | 55536   |
| ddrescue         | ddrescue_PitchnPutt.iso   | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728 | 55536   |
| dvdisaster       | dvdisaster_PitchnPutt.iso | dba625775ae7e24cbedef278164787f9 | 113’430’528 | 55386   |
| guymager         | guymager_PitchnPutt.dd    | b8d194d3ff2d9b37f3ec363339ef6d11 | 113’737’728 | 55536   |

#### Comparison with Infofiles

Information about the file size and the number of sectors is provided by cd-info and disktype. cd-info says that the CD has 55536 sectors and a file size of 108 MB. Further down under "CD Analysis Report" it says "55386 blocks" in the ISO 9660 file system.

disktype is more exact concerning file size and says a size of 113737728 bytes for the CD. In the ISO 9660 file system the file size is 113430528 bytes and 55386 blocks.

Compared to the table above it can be seen that dvdisaster, only reads the data present in ISO 9960. All other programs read the entire track. This is also true for the compressed aaruf format when the aaruf file is converted to an ISO file (see in table: aaru conv.iso).

The difference is 307200 bytes (equivalent to 300kB)

**Comparison Sectors**

dvdisaster = 55386
remaining = 55536

1 sector = 2048 bytes

55536 - 55386 = 150

<u>150 x 2048 = 307200</u>



**Comparison file size**

dvdisaster = 113430528 byte
remaining = 113737728 byte

<u>113737728 - 113430528 = 307200</u>



#### HEX Comparison

**ddrescue vs. dvdisaster**

Beyond Compare Version 4.4.4 (https://www.scootersoftware.com/)

Besides the comparison with the information from the infofiles, the images were compared with a HEX editor. As expected, all images were identical to each other except for the one from dvdisaster. The difference of 300kB calculated above does not contain any data but is filled with 0 for all images. Possibly this is the leadout of the CD.

As an example for the identical images, the HEX comparison between the ddrescue image and the dvdisaster image was exported as an HTML file and added to the repository as "hex_compare_ddresuce_vs_dvdisaster.html". 

dvdisaster= 307200 bytes less than the remaining images
Remaining= 307200 bytes filled with 0.



#### **Comparison with aaru compare**

With the program aaru, as mentioned at the beginning, two images can be compared with each other, regardless of whether they were created with aaru. These comparisons can be found as text files in the folder aaru_image_compare. 

The following command was used for the comparisons:

`aaru image compare Image1.iso Image2.iso`.

Again, the picture is similar to the HEX comparison. The exported results can be found in the folder "aaru_image_compare". In this comparison all sectors of the respective images were compared to each other. 



#### **Fiwalk comparison** 

**ddrescue vs. dvdisaster** 
With Debian GNU/Linux 11, Fiwalk version 4.10.1

In order to get information about the content of the respective images, an XML export was created with the program Fiwalk. It contains all files, which are stored in the respective image. In addition, Fiwalk creates an MD5 and an SHA1 checksum for each file.

The image of ddrescue was also compared with the image of dvdisaster as an example for the identical images. The exported HTML file showing the differences can be found under "fiwalk_compare_ddrescue_vs_dvdisaster.html". 

The content of the two images is identical. The only differences are the information about the program used to create the image and the time of creation of the image. 

## Conclusion

The comparison shows two main findings. On the one hand, the aaru format can be used to create aaruf, compressed images in an open-source format. These images, when converted to an ISO file, are identical to those created by most other programs used in media restoration.

On the other hand, the comparison has shown that with the program dvdisaster "only" the data track is saved as an image. However, the contents of the images created with dvdisaster are identical to those of the other programs tested.







