# Rasperry-pI-data-extraction
This is script was made for as a university project.  

In this section I will explain every step the script takes during the process of extracting the meta data from files. The script is mostly written in bash scripting as it was the simplest way to write the pro-gram, and because of this the program currently only works on Linux systems.
There are many steps that the program takes in order achieve the final result of extracting all possible meta data. The process is divided into four different stages: Execution, Creation, Collection, and Con-clusion.

Execution

In the beginning we have to choose the location where to execute the script. Originally the script was in one specific directory and could only be executed in the directory using the bash command fol-lowed by the name of the script. For example, bash code.sh.
The directory of the script was later added to the $PATH variable, and currently can be executed an-ywhere using the extract command. Arguments can also be added to the command for specific extrac-tion. 
If a directory name is added as the first argument, the meta data of every file on that directory will be extracted and will be added to the directory of origin. If a file is added instead of a directory, only that file will be extracted and added to the directory of origin. If no argument is added to the extract com-mand, the files in the current directory will be extracted.
If the user executing this program does not have read or write permission in the specified directory, then the program will stop and tell the user about their unsatisfactory permissions and urge them to use sudo to run the program as an administrator.
The extraction will NOT execute if:
-	The user does not have permission
-	The directory/file does not exist
-	The directory/file was misspelled 
-	If the directory/file is on a different directory, and the path is entreated incorrectly
-	If the directory is empty
For the second argument a directory location can be added for where to store all these files that will be created. If no argument is added, the files will be sent to a separate directory of the directory of the first argument.


Creation – for directories

When the script is executed a directory, known as Extracted, is created to stores all the files that will be created. I will continue to refer to the Extracted directory simply as Extracted throughout the rest of the paper. 
After Extracted is created, the script loops through all the files in the selected directory. Hidden files in a directory are also part of this process. Each text files, upon created, is immediately moved to Ex-tracted, and is given the extension “.e”, meaning extracted. There are some files that will be excluded in this process in order to reduce confusion:
-	Extracted and each subsequent text file are excluded from the loop, if this wasn’t the case it would eventually cause a system wide storage overload from all the files crated.
-	The parent directory is also excluded to avoid confusion with the documents in the current directory. The extraction is on done on the current or the selected directory.
-	All except direct children of directories. If a file in a directory only the meta data of files inside the directory will part of the extraction process. It is only limited to this to avoid to many files being created. 
Besides the regular document a separate “master file” is created to hold the meta data of all these files in one place, it is simply titled “masterfile.txt”. 

Creation – for an individual file

If the investigator wishes to only to retrieve meta data from one file, the file name should be added as the first argument. If a file is in the current directory, only the filename is enough, otherwise the full path should be written. Extracted will still be created to store the text file that holds the meta data. Because this is an extraction of an individual file, a master file is not necessary because all the meta data will already be in the singular text file. The standards are the same as with the directories, if the user does not have permission, the program will stop and ask the user to use sudo to run the script as an administrator.


Collection

After a text document for the corresponding file has been created, and moved to Extracted, the meta data from the files is added. The simplest way to retrieve the meta data for a file is by using the built-in stat command. Below is an example of the output of the stat command:
  File: file.txt
  Size: 4030      	Blocks: 8          IO Block: 4096   regular file
Device: 301a/3049c	Inode: 13633379    Links: 1
Access: (0644/-rw-r--r--) Uid: ( 1000/   martin)   Gid: ( 1000/   pi)
Access: 2022-04-06 09:52:17.991979701 +0100
Modify: 2022-04-06 09:52:17.971979713 +0100
Change: 2022-04-06 09:52:17.971979713 +0100
 Birth: -

This data is collected and later organized in the corresponding text file. Besides the stat command meta data, other commands are used to extract more meta data specific to a file type, this includes:
Exiftool – As mentioned earlier in the Method section of this paper, the function of Exiftool is simi-lar to this script in that it extracts meta information of a variety of files and even features the option to create a text file. Exiftool is used to extract additional meta data from image, pdf, and video files. In order for it to work it needs to be installed using apt-get install libimage-exiftool-perl command.
ImageMagick – ImageMagick is used for creating and editing bitmap images with support for meta data extraction. With the help of the identify -verbose <file_name> command we can extract very de-tailed meta data from image files. Everything from the number of pixels in the image to the amount of red, green, and blue in each sector. In order for it to work it needs to be installed using apt-get install imagemagick command.
Mediainfo – This tool supplies technical and tag information on video and audio files. It is used to extract the meta information out of the media files in the extraction procees. Some of the results returned from the tool include size of file, length of video/audio, format, settings, bit-rate, frame rate, type comparison, etc. If the media file is a video file there is separate meta information for audio and video. In order for it to work it needs to be installed using apt-get install mediainfo command.
PDFinfo – This tool prints the contents of the ‘Info’ dictionary and additional meta information from PDF files. It gives detailed information on a pdf file such as the Creator of the file, production date, Modification date, Encryption details, number of pages, Pdf version, etc. In order for it to work it needs to be installed using apt-get install pdfinfo command.
If the user does not have the required permissions for meta data collection of a file, the file will be skipped, and the script informs the user that they should use sudo on this file. If the script is already executed using sudo this will not be the case.


Extraction conclusion

When the extraction is concluded it informs the user that the meta data of all the files of the selected directory, or individual file, have been successfully extracted. All the files are available for the investi-gator to see in Extracted along with masterfile.txt containing the meta of all the files in the extrac-tion process.
