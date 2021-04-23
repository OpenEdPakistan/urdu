# Generating Urdu Audio Files using Google Text-to-Speech
## Why?
Urdu audio files need to be generated from Urdu text so they can be embedded into a Microsoft PowerPoint file. 
## What?
Jupyter Notebook that executes in Google Colab to read Urdu text from a file and generate audio files using the Google Text-to-Speech engine.

![Translate Console App](../files/gTTS-STD.png)
## How?
*The following steps create the audio files.*
### 0. Upload Urdu Text File
The user (or developer) uploads the Urdu text file to the cloud folder.
### 1. Execute Python Notebook
The Python notebook on Google Colab is executed.
### 2. Call Google Text-to-Speech API and serialize output
a. The code calls the text-to-speech API.<br />
b. The API returns the audio output.<br />
c. The audio output is serialized and stored as files in the cloud folder.<br />
### 3. Generate and download zip file
a. The user issues a zip command using Python code.<br />
b. The code creates a zip archive containing the audio files and stores it in a cloud folder.<br />
c. The zip file is downloaded by the user.<br />

The Python code described above is as follow:
```
pip install gTTS pyttsx3 playsound

import gtts
from playsound import playsound

from google.colab import files

inputFile = "/urdu.txt"

with open(inputFile,"r") as f:
        i = 1;
        for line in f:
            tts = gtts.gTTS(line, lang="ur")
            fileName = str(i) + ".mp3"
            tts.save(fileName)
            files.download(fileName)
            i = i + 1;

print('Done.')
```

The following command is executed in the notebook and creates a zip file for the serialized audio files. (NOTE: Do not forget the exclamation mark at the beginning of the command)
```
! zip results.zip *.mp3
```
The code file can be found here: https://github.com/OpenEdPakistan/urdu/blob/main/tts/gTTS.ipynb
<br />
You can convert the mp3 files to wav on your Microsoft Windows 10 machine by downloading the ffmpeg.exe file from [here](https://github.com/BtbN/FFmpeg-Builds/releases) (located in a zip file), adding it to your path, and running the following Python code:
```
import os
import subprocess

path = r"C:\FolderName\SubfolderName"
srcPath = ".mp3"
destPath = ".wav"
fileNameNoExt = ""

for fileName in os.listdir(path):
    if fileName.endswith(srcPath):
        fileNameNoExt = fileName[0:fileName.find(".")]
        srcFile = os.path.join(path, fileName)
        destFile = fileNameNoExt + destPath
        print("Converting " + fileNameNoExt + srcPath + " to " + fileNameNoExt + destPath)
        subprocess.call(['ffmpeg', '-i', srcFile, destFile])
```
