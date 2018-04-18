## Gory details of pdftotext

There are really two versions of pdftotext running around. One is from foolabs; one is based on the code from foolabs but distributed by poppler. The poppler version has more features.

Specifically, the poppler version includes a -bbox option for pdftotext that outputs word-level bounding boxes.

The poppler distribution of pdf tools is [here](https://poppler.freedesktop.org/) - also see the [wiki](https://freedesktop.org/wiki/Software/poppler/) and can be downloaded with  `brew install poppler` (OSX) / `sudo apt-get install poppler-utils` (Ubuntu)

The foolabs version is [here](http://www.foolabs.com/xpdf/download.html). 

You can tell it's the poppler by looking at the output of the command with no inputs, which will print the usage. It'll say poppler, and it'll give you a version number less than 1.

	pdftotext version 0.41.0
	Copyright 2005-2016 The Poppler Developers - http://poppler.freedesktop.org
	Copyright 1996-2011 Glyph & Cog, LLC

The usage has these two options

	  -bbox                : output bounding box for each word and page size to html.  Sets -htmlmeta
	  -bbox-layout         : like -bbox but with extra layout bounding box data.  Sets -htmlmeta
	  

Note that the version number for the foolabs version that doesn't have the -bbox option is 3.X (the one shown below is version 3.04)

	pdftotext version 3.04
	Copyright 1996-2014 Glyph & Cog, LLC
	
