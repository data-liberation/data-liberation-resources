# This page is [bit.ly/pdf-bagels](http://bit.ly/pdf-bagels)

# Advanced PDF manipulation
Sweet-talking PDFs into giving up the goods?

## Overview

- There's not a single piece of software to demo, so we're gonna whip through a bunch
- format: start simple, become more codey
- not really an advertisment for coding yourself (but who else is really gonna do it for you?)
- "Hands-on" but you can be more hands on after the class with the code, because sometimes stuff works better outside of the hands-on classes? 
- This was added b/c of desire for more pdf manipulation, but we should do a quick poll--what techniques are people using and what do they wanna get? 

## Outline 

- About PDF format
- OCR'ing
- layout analysis 
- more layout analysis
- more layout analysis

## PDF format

The spec is [here](http://www.adobe.com/content/dam/Adobe/en/devnet/acrobat/pdfs/pdf_reference_1-7.pdf), but we'll oversimplify by saying that the important thing is that it pdf is a display language, so one thing it needs to know is what to render and where. We're gonna be concerned about letters, images, rectangles and lines. 

## Three flavors of pdfs
For our purposes, there are three kinds of PDFs:

- All images. You can't select the text, because there isn't any in there.

- Text-based pdfs. Someone generate the file by exporting it to pdf, and the text can be recovered because it's in there.

- Embedded text pdfs. This is a file with both an image of the file, and text "underneath" it. Basically you see this from OCR'ed scans. 

## Note about paths
This document assumes that the data files are in classdata/sample_pdfs/ . Important reminder before we go any further!! 
#### The file paths in the lab are likely slightly different than what's below any you may have to adjust accordingly. That means instead of typing classdata/sample\_pdfs/ you might have to type classdata/this\_class\_name/sample\_pdfs.

## Update: Desktop -> Hands-on Data -> Session name

It's also possible the name has a space in it, which is annoying and my fault. You can either *move* the file or make sure you get the spaces right. 

#### PSA: Spaces in file names are almost always a bad idea! Maybe you should move it with something like `mv sample\ pdfs sample_pdfs`

## Tab completion at the command line
This has nothing to do with PDFs, but folks sometimes don't know that at the command line you can hit tab and it will try to complete the file name. Really helps with typing errors! 

So I can type:
`$ls cl`
the hit tab, and it will fill in the rest of the path, and it will automagically become:

`$ls classdata`

Computers!



## Look at the metadata. It's not too helpful
Poke at the pdf. There's usually not that much info here. It's interesting to see that Stacy Perrus created this on Wed. Sep2 2015 using Excel 2013. Don't think about this too much--anyone who really wants to remove this from the doc can. 

	$ pdfinfo classdata/sample\ pdfs/2015-2016-dma-ranks.pdf
	Author:         Perrus, Stacy
	Creator:        Microsoft® Excel® 2013
	Producer:       Microsoft® Excel® 2013
	CreationDate:   Wed Sep  2 09:32:02 2015
	ModDate:        Wed Sep  2 09:33:26 2015
	Tagged:         yes
	UserProperties: no
	Suspects:       no
	Form:           none
	JavaScript:     no
	Pages:          6
	Encrypted:      yes (print:yes copy:no change:no addNotes:no algorithm:AES)
	Page size:      612 x 792 pts (letter)
	Page rot:       0
	File size:      58627 bytes
	Optimized:      no
	PDF version:    1.6

# OCR: short version

- Overview will give you back searchable pdfs if you upload your files with them. And you probably want to do this anyway, because it rocks. As a side benefit you can just skip ahead to the next section if you do. 

This was talked about a fair amount in the intermediate PDF class and I'm not gonna go into much detail. But as a reference, here's an example. We're not gonna do it. 

#### Preprocessing

This is the simplified version: see full details in the [classdata/sample_pdfs/WFLX](classdata/sample_pdfs/WFLX/README.md) dir.

Tesseract operates on image files, so you'll need to convert pdfs to images first. The simplest way is probably to use imagemagick. For installation see [here](http://www.imagemagick.org/script/binary-releases.php).

Image pre-processing is a dark art and you can spend a lot of time on it. There's actually a script intended for OCR written by the author of imagemagick here: http://www.fmwconcepts.com/imagemagick/textcleaner/index.php. 

#### Make the directory first if it doesn't already exist! : `$mkdir classdata/output` -- or wherever you output your data. 

For [classdata/sample_pdfs/sample\_contract.pdf](classdata/sample_pdfs/WFLX/sample_contract.pdf), convert the first page with: `convert -density 300 ./classdata/sample_pdfs/sample\_contract.pdf ./output/sample_contract_p1.png` ; the result is [here](classdata/output/WFLX/sample\_contract\_p1.png). 

#### Imagemagick has crappy error messages. Instead of saying a file doesn't exist, it says something like this:
	convert: no images defined `./output/sample_contract_p1.png' @ error/convert.c/ConvertImageCommand/3252.

Image pre-processing is a dark art and you can spend a lot of time on it. There's actually a script intended for OCR written by the author of imagemagick here: 
http://www.fmwconcepts.com/imagemagick/textcleaner/index.php

The next step is to turn this into a searchable pdf:

`tesseract  classdata/sample_pdfs/WFLX/sample_contract_p1.png classdata/output/sample_contract_p1_text_added PDF`
 

Read more about quality! [https://github.com/tesseract-ocr/tesseract/wiki/ImproveQuality](https://github.com/tesseract-ocr/tesseract/wiki/ImproveQuality)



## The simple stuff 

Raw file is: [classsata/sample_pdfs/2015-2016-dma-ranks.pdf](classdata/sample_pdfs/2015-2016-dma-ranks.pdf)

This is an advanced class, you probably already know this, right? 

`$pdftotext classdata/sample_pdfs/2015-2016-dma-ranks.pdf classdata/output/2015-2016-dma-ranks-noargs.txt`

No args. Looks like this: 

	$ head classdata/output/2015-2016-dma-ranks-noargs.txt
	
	Local Television Market Universe Estimates
	Estimates as of January 1, 2016 and used throughout the 2015-2016 television season
	Estimates are effective September 26, 2015
	Rank
	1
	2

This is useless. 

## 1 Advanced trick for using command line software
Read the documentation!

Use the -layout flag, with -f (first page = 1) and -l (last page = 1). So it only runs the first page

`$pdftotext -layout -f 1 -l 1 classdata/sample_pdfs/2015-2016-dma-ranks.pdf classdata/output/2015-2016-dma-ranks-preservelayout.txt`

	$ head classdata/output/2015-2016-dma-ranks-preservelayout.txt
	
	       Local Television Market Universe Estimates
	       Estimates as of January 1, 2016 and used throughout the 2015-2016 television season
	       Estimates are effective September 26, 2015
	
	Rank   Designated Market Area (DMA)                                TV Homes          % of US
	  1    New York                                                       7,368,320            6.503
	  2    Los Angeles                                                    5,489,810            4.845
	  3    Chicago                                                        3,475,220            3.067
	  4    Philadelphia                                                   2,917,920            2.575
	  5    Dallas-Ft. Worth                                               2,646,370            2.335

Run the read_dma.py file. The line that matters is this:

`line_re = re.compile("\s+(\d+)\s{2,}(.+?)\s{3,}([\d,]+)\s{2,}([\d\.]+)\n")`

REGEX! 


`$ python read_dma.py`

how it looks:

	$ head classdata/output/2015-2016-dma-ranks-preservelayout.csv
	rank,dma,households,us_percent
	1,New York,"7,368,320",6.503
	2,Los Angeles,"5,489,810",4.845
	3,Chicago,"3,475,220",3.067
	4,Philadelphia,"2,917,920",2.575
	5,Dallas-Ft. Worth,"2,646,370",2.335
	6,San Francisco-Oak-San Jose,"2,484,690",2.193
	7,"Washington, DC (Hagrstwn)","2,443,640",2.156
	8,Boston (Manchester),"2,411,250",2.128
	9,Atlanta,"2,385,730",2.105



There are important differences between different versions of pdftotext, see more about it [here](pdftotext_versionitis.md). 


#### THERE ARE MANY WAYS TO DO THIS! You could use tabula's command line tool as well. 

That was easy! It's harder for weirder files. 

You can parse the heck out of text files, but stuff can get tricky. This is a text-based pdf: [classdata/sample_pdfs/150109DSP-Milw-505-90D.pdf](classdata/sample_pdfs/150109DSP-Milw-505-90D.pdf)


# Plumbing the pdf

### Version in the labs is 0.4.6... Current version is 0.5.3
 
There's a bug in 0.4.6... 

### Running with scissors:

Should we upgrade? Use:

`$ sudo pip install pdfplumber==0.5.3` 

then enter admin password

Default is to spit out

`$ pdfplumber sample_pdfs/150109DSP-Milw-505-90D.pdf --pages 1 > output/150109DSP-Milw-505-90D.csv`

	$ head output/150109DSP-Milw-505-90D.csv
	object_type,page_number,x0,x1,y0,y1,doctop,top,bottom,width,height,adv,fontname,linewidth,points,size,text,upright
	char,1,23.760,29.565,23.067,33.768,758.232,758.232,768.933,5.805,10.701,0.722,ArialMT,,,10.701,D,True
	char,1,29.517,35.322,23.067,33.768,758.232,758.232,768.933,5.805,10.701,0.722,ArialMT,,,10.701,C,True
	char,1,35.273,40.186,23.067,33.768,758.232,758.232,768.933,4.912,10.701,0.611,ArialMT,,,10.701,F,True
	char,1,40.202,42.879,23.067,33.768,758.232,758.232,768.933,2.677,10.701,0.333,ArialMT,,,10.701,-,True

There are chars and rects in this. Let's look at the output a little. 

	$ python
	Python 2.7.11 (default, Jan 22 2016, 08:29:18) 
	[GCC 4.2.1 Compatible Apple LLVM 7.0.2 (clang-700.1.81)] on darwin
	Type "help", "copyright", "credits" or "license" for more information.
	>>> import pdfplumber
	>>> file = "your filepath here!"
	>>> pdfobj = pdfplumber.open(file)
	>>> pdfobj.pages 
	[ output suppressed here]
	>>> firstpage = pdfobj.pages[0]
	
	# You can see other page attributes here too -- 
	
	>>> firstpage.width
		Decimal('612.000')	
	>>> firstpage.height
		Decimal('792.000')
		
#### More docs here: https://github.com/jsvine/pdfplumber

	tables = firstpage.find_tables()
	
#### Read the code, but for weird pdfs, often best to code it yourself

	>>> words = firstpage.extract_words()
	>>>  [i['text'] for i in words]
	>>> import pandas as pd
	>>> df = pd.DataFrame(words)
	
	
By default pdfplumber doesn't pass fonts through, but you might want to write code yourself to do this. 
	
pandas syntax! Show the location of words that start with "Number"

	>>>  df[df.text.str.startswith("Number")]

Filter by position:

	>>>  df[ ( df.x0 > 100 ) & ( df.x0 < 400 ) ]
	
Checkboxes:
	>>> rects = firstpage.rects
	>>> for rect in rects:
	...  print rect['height']

	>>> for f in firstpage.curves:
	>>>  print f



# Other cool stuff I shouldda talked about but probably didn't

- [Google vision API](https://cloud.google.com/vision/?utm_source=google&utm_medium=cpc&utm_campaign=2015-q1-cloud-na-gcp-skws-freetrial-en&gclid=CNnhpLWNu9ICFVFahgodbpYF1Q) . Note that 'in-situ' extraction is a little different (lots of it is based on stroke-width transform, maybe?)

- "A set of tools for extracting tables from PDF files helping to do data mining on (OCR-processed) scanned documents." https://github.com/WZBSocialScienceCenter/pdftabextract

--> related: https://github.com/WZBSocialScienceCenter/pdf2xml-viewer

## Tabula-extractor

https://github.com/tabulapdf/tabula-java

https://github.com/chezou/tabula-py

## PDFTOHTML

`$ pdftohtml classdata/sample\ pdfs/senate_page_sample.pdf -c generated_files/html/senate_page_sample_complex.html`

	$ head -n 100 generated_files/html/senate_page_sample_complex-1.html | tail -n 3
	<p style="position:absolute;top:306px;left:584px;white-space:nowrap" class="ft01">METAIRIE&#160;TO&#160;COVINGTON&#160;AND&#160;RETURN</p>
	<p style="position:absolute;top:314px;left:919px;white-space:nowrap" class="ft01">25.00</p>
	<p style="position:absolute;top:314px;left:584px;white-space:nowrap" class="ft01">STAFF&#160;PER&#160;DIEM</p>

stopping point -- what we have here are words in space. Actually, this is pretty good, because it found the cells for us. well, probably, one page isn't enough to be sure.
