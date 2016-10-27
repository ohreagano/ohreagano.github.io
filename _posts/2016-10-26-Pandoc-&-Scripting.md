---
layout: post
Title: Pandoc & Scripting
category: inls161
---

# Assignment 3: Text

## Abstract

The original work I used for this assignment is a federalist paper arguing for adoption of a new constitution agreed upon
in my POLI 409 course: Mock Constitutional Convention. It examines the gun rights clause, which makes the right to own and
operate a gun an individual right. By making it an individual right, this Constitution ensures citizens’ right to self-defense
and allows for lawful recreational gun use. It codifies the modern shift away from the current US Constitution’s militia 
interpretation and prevents civilian access to military-style weapons that have no reasonable civilian purpose.


## So how did I turn one file into so many different things?! Pandoc!

For this assignment I wrote a script that uses pandoc to convert a markdown file into four other file types:
PDF, HTML, docx, and odt. There are many other file types that pandoc can convert to, which you can learn about in the
[Pandoc User Guide](http://pandoc.org/MANUAL.html).

The first step is to install pandoc it in your Cloud9 workspace, or wherever you are working, with the following code:

```
wget https://github.com/jgm/pandoc/releases/download/1.15.1/pandoc-1.15.1-1-amd64.deb

sudo dpkg -i pandoc-1.15.1-1-amd64.deb
```
Once pandoc is installed, you're ready to convert some files. The basic format for converting is:

```
pandoc -o output.html input.md
```
The -o signals that the document immediately following will be the output document. 

Markdown is a flexible file format, and can be converted directly to the other types. 
Other file formats, like docx, contain more built in formatting and cannot be directly converted to other types.
So for example, to go from docx to html, the document would have to be converted from docx to md, and then from md to html.
That's what I did to get my source file for the project! My original document was in docx format, because I had typed it in Microsoft Word.
To turn it into my source file, FederalistPaper.md, I used this command:

```
pandoc -o -S FederalistPaper.md SourceFile.docx
```

The -S is another flag that stands for Smart. It produces typographically correct output, like
curly quotes and ellipses. 

Producing a PDF requires some kind of LaTeX engine to be installed. Use this code to install LaTeX:

```
sudo apt-get install texlive
```
It's going to take up a lot of space on the disk, and it will ask if you want to proceed. Enter Y if there is room.
If not, delete some stuff to make room. 
And if that command doesn't work, it's probably because you need to update the software packages. Use this command:

```
sudo apt-get update
```
So those are the basic tools to convert documents easily from one format to many others.
Pandoc can do a ton more, and it's all well-documented in the [guide](http://pandoc.org/MANUAL.html).
But it gets even easier!

## Using a Script to streamline the process

With scripts, you can type out all your Pandoc commands once, and run them over and over and over again
on as many documents as you want! Want to make a tiny revision in the source document, but don't want to 
deal with editing _all_ your different formats? No problem! A script will take care of it for you. 

Setting up a bash shell script requires 3 things:

- Make a Bash shell script file with the __.sh__ extension. 
- Make the file executable with this command:

```
chmod +x ScriptFileName.sh
```
- And put the _shebang_ in the first line of your .sh file:    `#!/bin/bash`

Now think of commands you would execute in the command line, and put them in your script to automate the process! 
Note, it's important to think about the logic and flow of the commands. The script will execute them in order, 
so the script will fail if it isn't logical or later commands conflict with something earlier in the script.

To execute the script, use the following command in the command line:

```
./ScriptFileName.sh 
```

For this assignment, I wrote a script that takes in user input (or an argument) at the time that the script is executed.
This line declares the input argument as a variable with the name INPUT:

```
INPUT=$1
```
When I use "INPUT" later in the script, I call on the variable with the "$" to signify it's a variable. 
As a convention, variables are all-caps. 

Once the script is running, it asks the user to input the desired name for the output documents.
The script accepts the name as a variable: $FILENAME.

My script assumes the user will input a markdown file, so the next lines use 
the variable $INPUT to create the file outputs. Here's the md to pdf command:

```
# Convert markdown to PDF
pandoc -S -o $FILENAME.pdf $INPUT
```
And then once the script is done converting, it prints (using command `echo`) a line
telling the user that it has output four new files of different formats.


## Here are links to the original markdown file and resulting file outputs on GitHub:

- [MD](https://github.com/inls161/assignment-3-ohreagano/blob/master/FederalistPaper.md)
- [PDF](https://github.com/inls161/assignment-3-ohreagano/blob/master/FederalistPaper.pdf)
- [HTML](https://github.com/inls161/assignment-3-ohreagano/blob/master/FederalistPaper.html)
- [DOCX](https://github.com/inls161/assignment-3-ohreagano/blob/master/FederalistPaper.docx)
- [ODT](https://github.com/inls161/assignment-3-ohreagano/blob/master/FederalistPaper.odt)

And here are the links to my script:

- [Script on GitHub](https://github.com/inls161/assignment-3-ohreagano/blob/master/ohreagano-convert-docs.sh)
- [Editor in Cloud9](https://ide.c9.io/recline/assignment3)


## Reflection

Scripting is awesome. It can save so much time. In the context of document conversion, it's especially useful 
because I like to make a lot of small revisions over time. Now with the script, I can revise my soure file and
easily convert the new version into all desired file types. 

But beyond this, any multi-step action I would do regularly on the command line can be automated with BASH!

I looked into some LaTeX templates to swankify my script, but couldn't figure it out in time for the assignment due date. 
I made a file for layouts within my assignment 3 workspace, and then copied a LaTeX template I found online. But when I 
tried to run the script using the template, it kept giving me an error: "! Environment Undefined". I googled it and tried a 
few fixes, but wasn't able to resolve the problem right now. I plan to come back to it later and learn how to use and customize 
templates so I can use markdown and pandoc for school assignments and other writing.

Overall I'm excited about this technology and want to figure out ways to use it to streamline my work. Word and similar programs 
have so many irritating formatting rules, so this enables me to step around those if I can figure out how to further customize
the output.