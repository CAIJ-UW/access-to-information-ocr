# Access to Information and Optical Character Recognition (OCR): Rendering Image Format Public Records Machine Readable Using Tesseract OCR
 - [Alex Luscombe](https://criminology.utoronto.ca/facultyandstaff/graduate-students/alex-luscombe/), University of Toronto
 - [Kevin Dick](https://www.linkedin.com/in/dickkevin/), Carleton University
 - [Kevin Walby](https://www.uwinnipeg.ca/criminal-justice/faculty-staff/kevin-walby.html), University of Winnipeg

## Overview
It is perennial problem in Canada that municipal, provincial, and federal government agencies disclose records under Access to Information (ATI)/Freedom of Information (FOI) law by default in a non-machine readable (image) format. Excel files, for instance, are often printed and scanned by access coordinators before they are released to the requester. In some cases, coordinators may be willing to release the data in a "raw" format, however, this is not always the case. Historical researchers face a similar limitation when accessing public records via archives. To create copies of public records held in archives, researchers will typically photograph or scan them. In both cases the result is the same: a large dossier of non-machine readable, image format records stored on one's personal computer. 

The inability to machine read these texts limits the kinds of analytic techniques we can use. With respect to FOI/ATI requests in particular, it also creates issues of access. It is a well known technique of obfuscation for government agencies to "over produce" when processing a request by including mounds of irrelevant text as part of one's disclosure package. Manually sifting through thousands of pages of image format documents disclosed under ATI/FOI in search of one or two lines or key words becomes the equivalent of finding a needle in a haystack.

Fortunately, there currently exist a number of free and open-source solutions to this problem. Within the field of computer science, the rendering of scanned imagery into machine readable text is widely considered to be a "solved" problem. One such state-of-the-art solution is the *Tesseract Optical Character Recognition* (OCR) engine, considered to be one of the best OCR engines currently available.

The goal of this project is to teach you how to use Tesseract OCR. It is part of a larger series of projects we intend to launch aimed at promoting both computer literacy and algorithmic thinking in non computer scientists, enabling the use of many additional open-source tools that promise to eliminate many of the barriers to access of information. The ability to convert a document into a format wherein it might be searched for keywords, phrases, and possibly studied using natural language processing (NLP) methods alongside more traditional qualitative ones promises to revolutionize analytical methods within the social sciences that are typically limited to leveraging human-based readers/transcribers.

## Tesseract is powerful, but not perfect
While the Tesseract OCR engine is highly effective, it is not perfect. ATI/FOI disclosures and the record copies we obtain from public archives are typically photographed or scanned copies of records and depending on their quality, not all characters may be properly recognized. Moreover, ATI/FOI disclosures generally contain redactions, usually in the form of white, grey, or black boxes covering undisclosed portions of the text. Finally, complex character layouts, such as with scanned tables with varying layouts highlight the challenges in correctly identifying characters and presenting them in relation to others in the document. As a result, depending on the quality, layout, and format of the ATI/FOI records, it is likely that some degree of document "cleaning" will be required after processing them. Cleaning can be effectively streamlined by following what computer scientists call the "[human-in-the-loop](https://en.wikipedia.org/wiki/Human-in-the-loop)" paradigm, though we cannot get into this here.

## Open-source vs proprietary OCR
Tesseract OCR is completely free. Proprietary OCR software by contrast is not, and can be quite expensive. There is also no restriction to how many documents you can process using tesseract OCR, whereas most proprietary software will limit the number of pages you can process (usually until you pay more). Depending on the software you purchase, it may also be less effective than tesseract (tesseract, as noted above, is one of the best OCR engines currently available). Finally, by using tesseract, we generate transparent, reproducible code that can be shared with others or adapted for future purposes. 

## Let's get started

### Tutorial (compatible with MacOS & Linux):
The goal of this tutorial is to walk you through how to render your scanned, image-based format documents into a machine-readable, text-based format. The following steps are designed to build up an intuition for the use of open-source software to facilitate access to information. In the following, we will cover the high-level steps required to convert a large scanned document into a machine-readable and searchable format. For learning purposes, we recommend following the steps using one of two sample ATI/FOI disclosures provided in the repository. 

#### Step one: download the repository
Download the repository, unzip the folder, and save it locally on your computer's hard drive. 

![](Screen_Shots/download_image.png)

#### Step two: open your computer's command prompt
Open your computer's command prompt. On MacOS, this is called the Terminal. To open the Terminal, simply press Command + Space and enter the word "Terminal" in the search bar. Double click the Terminal application listed under Top Hit to open it.

#### Step three: install Homebrew
*If you already have Homebrew installed on your computer, skip this step.*

If you unsure about whether you have homebrew installed, type the following line into your computer's Terminal:
```
brew help
```
If it returns "command not found", you do not have homebrew installed on your computer.

You can download Homebrew by entering the following command into your computer's Terminal:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

#### Step four: install Ghostscript using Homebrew
*If you already have Ghostscript installed on your computer, skip this step.*

If you unsure about whether you have Ghostscript installed, type the following line into your computer's Terminal:
```
ghostscript help
```
If it returns "command not found", you do not have Ghostscript installed on your computer.

You can download Ghostscript by entering the following command into your computer's Terminal:
```
brew install ghostscript
```

#### Step five: install Tesseract using Homebrew
*If you already have tesseract installed on your computer, skip this step.*

If you unsure about whether you have Tesseract installed, type the following line into your computer's Terminal:
```
tesseract help
```
If it returns "command not found", you do not have tesseract installed on your computer.

You can download Tesseract by entering the following command into your computer's Terminal:
```
brew install tesseract
```

#### Step six: copy the pathname of the folder you downloaded, unzipped, and saved in step one
We need to obtain the full directory pathname of the folder that you downloaded, unzipped, and saved in step one. This is a copy of the repository and contains the sample ATI/FOI disclosures and scripts we will need to run the Tesseract OCR engine. To obtain the exact pathname to this folder, simply open the folder on your computer, click the gear wheel at the top of the window, and select the "copy as Pathname" option from the list. This will copy the full pathname to your clipboard.

![](Screen_Shots/Pathname.png)
	
#### Step seven: change your working directory in the Terminal
We will now change our working directory using the pathname you just copied to your clipboard. To do this, we will return to the Terminal, first writing "cd" followed by the pathname we just copied (pasted into the Terminal using command + V). The end result will look something like this:
```
cd path/name
```
To be more exact, if your name was Jane, and you saved the folder on your desktop, it should look something like this:
```
cd /Users/jane/Desktop/ATIP-OCR-master
```
To double check that you are in the correct directory, you can enter the following into your Terminal, which will tell you the name of your current directory and print off the name of the files contained in it:
```
ls
```

#### Step eight: create an output subfolder
To process our sample ATI/FOI disclosure file rendering it machine readable, we are going to be first parsing the file into individual page elements, running each of these page elements through the Tesseract OCR engine, and recompiling the .txt files generated from each individual page item into a single .txt file that we can then clean, search, and analyze. Throughout these processing stages, a lot individual .png and .txt files are going to be generated, and these need to be stored somewhere (ideally not on your desktop!).

Inside the ATIP-OCR-master folder, our working directory, create a new subfolder. You can call this folder whatever you like. If you are using one of the sample ATI disclosures in the Sample-ATI-Disclosures folder, you might name the subfolder after the sample record you are processing, for example "A-2017-00078". 

To create this subfolder, enter the following into your computer's Terminal:
```
mkdir subfolder-name
```
So, if you are going to call this subfolder "A-2017-00078", you would write:
```
mkdir A-2017-00078
```

To verify that this has worked, return to your Terminal and enter the following command (which we learned earlier):
```
ls
```
You should now see the name of your new subfolder listed with the other files in your working directory.

#### step nine: load in the requirements.txt file
Before we can run our python script, we need to install two key python libraries. To do this, enter the following into your computer's Terminal:
```
pip3 install -r requirements.txt
```

#### Step ten: run the script
We are now ready to process your file using ghostscript (to parse the file into individual page elements) and tesseract (OCR engine)! This stage can take several minutes (or hours) depending on the size of the file.

To do this, we are going to be running a simple Python script (in you working directory, this is the OCR-converter.py file). Same as the previous steps, we will run this script in the Terminal. Python is available on all MacOS computers by default, but depending on the age of your computer, you may need to update it to version 3. To do this, use homebrew to install python3 by entering the follow into your Terminal:
```
bew install python3
```

To run the Python script, all we need do is obtain the correct pathname for our input file (if you are following along with the example, one of the sample ATI/FOI disclosure files) and the correct pathname for our output file (the subfolder we created in step eight) and we are ready to go.

The basic formula is:
```
python3 OCR-converter.py -i input/file/pathname -o output/folder/pathname
```
So, let's say we are going to run the script on the A-2017-00078.pdf file in the Sample-ATI-Disclosures subfolder, and we are going to store the results in the subfolder we created in step eight called A-2017-00078. The result would look like this:
```
python3 OCR-converter.py -i Sample-ATI-Disclosures/A-2017-00078.pdf -o A-2017-00078
```
Enter this into your Terminal, sit back, and relax! (But don't mess with anything in the master folder until it's completely finished!). The end result will be a single .txt file in the output subfolder you created by the same name as the pdf file you processed (e.g., A-2017-00078.txt). 
