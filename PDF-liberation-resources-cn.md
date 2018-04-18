# PDF Extraction Resources

A list of tools and resources for working with PDF files.


## output format

text
json
xml
excel
csv
html

##  extract tabular data from PDF


### Open source technologies


- [Tabula-Java](https://github.com/tabulapdf/tabula-java): the scriptable engine behind Tabula, via OpenCollective.
- [pdfplumber](https://github.com/jsvine/pdfplumber): a high-level Python interface to pdfminer, created by Jerm-Sing-Vine
- [ZamZar](http://www.zamzar.com/): Provides pdf-to-table functionality at the level of Google Drive, which is to say, not much at all. Might include it just as a baseline.
- [pdftotext](https://poppler.freedesktop.org/): part of the Poppler suite. Its `-layout` option provides a conversion that attempts to visually emulate the original PDF, but the result is not delimited data.

### commercial technologies

- [PDFTables.com](https://pdftables.com/) - a cloud service from the Sensible Code Company
- [CometDocs](https://www.cometdocs.com/) - a cloud service with PDF-to-XLS functionality
- [ABBYY FineReader Pro for Mac](https://www.abbyy.com/en-us/finereader/pro-for-mac/) - What I personally use. Costs money. No idea if it differs via cross-platform. Has no automatable-interface. But very accurate and dependable.




## extract regular text and images data from PDF

## Open source  technologies


- [pdf2json](https://github.com/modesty/pdf2json) - A PDF file parser that converts PDF binaries to text based JSON, powered by a fork of PDF.JS

- [PDF Reader in JavaScript](https://github.com/mozilla/pdf.js/) -You can get raw text  and word position of pdf  :getTextContent(),
> Retrieve bounding box of text on a page  https://github.com/mozilla/pdf.js/issues/5643

- [PDFPlumber]((https://github.com/jsvine/pdfplumber)
PDFPlumber is a Python library and command-line tool for extracting information from PDFs. Both tools provide granular information about each character, rectangle, and line. The library also has Tabula-style features for extracting tables and text.

>- [Homepage](https://github.com/jsvine/pdfplumber)
>- Requirements: `python` and [`pip`](https://pip.pypa.io/en/stable/installing/#do-i-need-to-install-pip)
>- Installation: `pip install pdfplumber`


- "A set of tools for extracting tables from PDF files helping to do data mining on (OCR-processed) scanned documents." https://github.com/WZBSocialScienceCenter/pdftabextract

>related: https://github.com/WZBSocialScienceCenter/pdf2xml-viewer

- [content-extractor](https://github.com/Micka33/content-extractor) - 
Extract meaningful content from pdf and psd file, such as texts and images both linked into a common JSON string


- [Apache PDFBox](http://pdfbox.apache.org/) - General purpose PDF library written in Java.

- [Tabula](http://tabula.nerdpower.org) - Open source PDF table extraction tool written in Java and Ruby by Manuel Aristarán.  Makes calls to PDFBox. Table extraction powered by [http://github.com/jazzido/tabula-extractor]().

- [PDF Extraction Toolkit](https://github.com/tamirhassan/pdfxtk/) - Java framework built on PDFBox by Tamir Hassan for performing document analysis of PDF files and creating custom conversion methods to HTML and other formats.

- [PDFExtract](https://github.com/elacin/PDFExtract) - Text extraction library that extends both PDFBox and Poppler. Written in Java by Øyvind Berg, the tool is no longer under active development but may contain code that can be reused by hackathon participants. Download Page: http://elacin.github.io/PDFExtract/.

- [PDF2SVG](https://bitbucket.org/petermr/pdf2svg-dev/wiki/Home) - Java tool developed by Peter Murray-Rust that converts PDFs to Scalable Vector Graphics (SVG) files that can be rendered by most modern browsers. PDF2SVG, which is based on PDFBox, is a component of the larger AMI suite of open source tools created for the purpose of liberating scientific documents. Another component, SVG2XML converts the SVG files to HTML and is currently under heavy development.

- [Poppler](http://poppler.freedesktop.org/) (pdftotext, pdfinfo, pdfimages) - Command line tools to extract text, metadata, and bitmap images from PDF files, written in C++, forked from Xpdf.

- [Ashima PDF Table Extractor](https://github.com/ashima/pdf-table-extract) - Table extraction tool built in Python and based on Poppler.

- [Coolwanglu-pdf2htmlEX](http://coolwanglu.github.io/pdf2htmlEX/) - PDF to HMTL converter based on Poppler.

- [PDF2XML](http://sourceforge.net/projects/pdf2xml/) - Open source converter based on XPDF library developed by Hervé Déjean.

- [Xpdf](http://www.foolabs.com/xpdf/) (pdftotext, pdfinfo, pdfimages) - Command line tools to extract text, metadata, and bitmap images from PDF files.  Also includes a page rasterizer (pdftoppm).

- [MuPDF](http://www.mupdf.com/) - General purpose, open source PDF toolkit written in C by Artifex, the developers of GhostScript. The mudraw component has a basic text extraction utility.

- [PDFMiner](https://github.com/euske/pdfminer/) - Open source PDF extraction library written in Python.

- [PDFTables](https://github.com/okfn/pdftables) - Table extraction tool based on PDFMiner and also written in Python.

- [Doc⚡split](https://documentcloud.github.io/docsplit) - A command-line utility and Ruby library for splitting apart documents into their component parts: searchable UTF-8 plain text via OCR if necessary, page images or thumbnails in any format, PDFs, single pages, and document metadata (title, author, number of pages...)

- [DocHive](https://github.com/raleighpublicrecord/dochive) - Open source tool based on Tesseract and ImageMagick that extracts data from scanned PDFs.

- [Node PDF Extract](https://github.com/nisaacson/pdf-extract) - Javascript library that reads PDFs with embedded text as well as scanned PDFs. Built on both Poppler and Tesseract.

- [Ocrad](http://www.gnu.org/software/ocrad/) - "GNU Ocrad is an OCR (Optical Character Recognition) program based on a feature extraction method. It reads images in pbm (bitmap), pgm (greyscale) or ppm (color) formats and produces text in byte (8-bit) or UTF-8 formats."

- [GOCR](http://jocr.sourceforge.net/) - "GOCR is an OCR (Optical Character Recognition) program, developed under the GNU Public License. It converts scanned images of text back to text files."


- [pdfsandwich](http://www.tobias-elze.de/pdfsandwich/)-

>pdfsandwich generates "sandwich" OCR pdf files, i.e. pdf files which contain only images (no text) will be processed by optical character recognition (OCR) and the text will be added to each page invisibly "behind" the images.

>pdfsandwich is a command line tool which is supposed to be useful to OCR scanned books or journals. It is able to recognize the page layout even for multicolumn text.

>Essentially, pdfsandwich is a wrapper script which calls the following binaries: unpaper (since version 0.0.9), convert, gs, hocr2pdf (for tesseract prior to version 3.03), and tesseract. It is known to run on Unix systems and has been tested on Linux and MacOS X. It supports parallel processing on multiprocessor systems.

>While pdfsandwich works with any version of tesseract from version 3.0 on, tesseract 3.03 or later is recommended for best performance. By default, pdfsandwich runs unpaper to enhance the readability of scanned pages and to improve OCR. For instance, slightly rotated pages are automatically straightened and dark edges removed. For optimally scanned pdf files, this can be switched off by option -nopreproc to speed up processing.



## OCR Technologies:



### GUI (Free)

- [DocumentCloud](https://www.documentcloud.org/home) (if you're willing to wait in queue)
- [Overview](https://blog.overviewdocs.com/author/overview/) will now give you searchable pdfs back.
- [CometDocs](https://www.cometdocs.com/user/subscriptions) (web-based, [freemium](https://www.cometdocs.com/user/subscriptions))
- [pdfconvertme](https://github.com/wanghaisheng/pdfconvertme-public)


### GUI (Paid)

- [ABBYY Fine Reader](http://www.abbyy.com/finereader/) ($120 Mac / $170 Windows). Here's a [scanned doc](examples/klamath_elex/2010NovPrecinct.pdf) released by an Oregon county gov; it's [much better](examples/klamath_elex/2010NovPrecinct_scanned.pdf) with abby. 
- [Adobe Acrobat DC](https://acrobat.adobe.com/us/en/how-to/ocr-software-convert-pdf-to-text.html) (free trial / $15/mo / $450 purchase)
- [Cogniview PDF2XL](https://www.cogniview.com/) Windows only. Free trial. OCR edition: $299; "Enterprise":$399.Extracts PDFs to Excel.
- [Datawatch Monarch](http://www.datawatch.com/monarch13/) Specialty tool for structured data extraction; dunno if OCR is included. Free "personal" edition with 'limited imports and exports'; 30-day free trial for enterprise edition. They [claim](http://finance.yahoo.com/news/datawatch-makes-everyone-big-data-131101026.html)  Classic Edition ($895 per user per year) and Complete Edition ($1,595 per user per year).  

- [Abby Flexicapture](http://www.abbyy.com/flexicapture/).  Believe that Abby charges less in other countries, and there's an industry of document conversion services in other countries. Price varies by country. (~ $24K) 



- [Tesseract](https://code.google.com/p/tesseract-ocr/) - Open source OCR library. This tool does not work directly with PDFs, but a shell script or package can be used to convert a PDF to a TIFF which can be analyzed with Tesseract.
    - A [Java interface](http://sourceforge.net/projects/tess4j/) to Tesseract is available.
    - [Tesserwrap](https://tesserwrap.readthedocs.org/) is a Python ctypes wrapper for tesseract.

- [ABBYY FineReader](http://finereader.abbyy.com/) - Commercial OCR tool which works directly with PDFs. ABBYY also offers a cloud [OCR API](http://ocrsdk.com/)

- [Nuance OmniPage](http://www.nuance.com/for-individuals/by-product/omnipage/index.htm) - Commercial OCR tool which works directly with PDFs.

- [Captricity](http://captricity.com/captricity-at-a-glance/) - Web based service that uses a mixture of technology and human labor to convert uploaded documents into structured data.



-  [Able2Extract](http://www.investintech.com/order_main.htm): $99.95 for non-OCR/ $129.95 for OCR. Intuitive and reasonably accurate. It has some annoyances, but saving as a text file often overcomes them (rather than going directly to Excel).


- [Google vision API](https://cloud.google.com/vision/?utm_source=google&utm_medium=cpc&utm_campaign=2015-q1-cloud-na-gcp-skws-freetrial-en&gclid=CNnhpLWNu9ICFVFahgodbpYF1Q) . Note that 'in-situ' extraction is a little different (lots of it is based on stroke-width transform, maybe?)


- [Adobe Acrobat XI Pro](http://www.adobe.com/products/acrobat/) - The original general purpose GUI-based PDF tool that can convert to PDFs to Excel, Word, Powerpoint and HTML.


- [BCL Technologies](http://www.pdfonline.com/pdf-to-word-converter/) - Free, online PDF to Word and PDF to HTML converters.


- [Docudesk deskUnPDF Converter](http://www.docudesk.com/pdf-downloads) - Converts PDFs to Excel, Word, XML and other formats. Trial download available.

- [Microsoft Word 2013](http://office.microsoft.com/en-us/word/) - The most recent version of this MS Office component supports [direct opening of PDFs](http://office.microsoft.com/en-us/word-help/edit-pdf-content-in-word-HA102903948.aspx?CTT=5&origin=HA102809597). The contents can then be saved in DOCX or other Word-supported formats.

- [NitroPDF](http://www.nitropdf.com/) - General purpose GUI-based PDF tool that can extract to spreadsheets and documents.

- [Nuance PDF Reader](http://www.nuance.com/products/pdf-reader/index.htm) - Free PDF reader with a web service that converts PDFs to spreadsheets and documents.

- [Nuance PDF Converter](http://www.nuance.com/for-business/document-imaging-and-scanning/pdf-converter/index.htm)

- [PDFLib Text Extraction Tool](http://www.pdflib.com/products/tet/) - Function library that  makes available the text contents of a PDF as Unicode strings, plus detailed glyph and font information as well as the position on the page.

- [PDFTron](http://www.pdftron.com/) - General purpose PDF manipulation library that includes text extraction capabilities. [Sample Code Page](http://www.pdftron.com/pdfnet/samplecode.html)

- [ScraperWiki Table XTract](https://scraperwiki.com/tools/tablextract) - Web based solution that returns tables extracted from uploaded PDFs.

- [Simx Text Converter](http://www.simx.com/simx/Products.stp?prm=tc) - Extract, Transform and Load (ETL) solution that enables users to create custom routines for converting PDFs and other unstructured formats to database records.

- [Snow Tide PDF](http://www.snowtide.com/) TextStream. Commercial PDF text extraction component that can be embedded in Java or .Net applications. Single threaded version is free.

- [Xpdf Commercial Libraries from Glyph and Cog](http://glyphandcog.com/products.html) - Including:
    - [XpdfText](http://glyphandcog.com/XpdfText.html), a PDF text extraction library
    - [XpdfInfo](http://glyphandcog.com/XpdfInfo.html), a PDF metadata extraction library
    - XpdfImageExtract, a PDF image extraction library (contact info@glyphandcog.com for details)
    - [XpdfRasterizer](http://glyphandcog.com/XpdfRasterizer.html), a library which converts PDF pages to images.

- [Aspose.Pdf for Java](http://www.aspose.com/docs/display/pdfjava/) - How to [Extract Text From All the Pages of a PDF Document](http://www.aspose.com/docs/display/pdfjava/Extract+Text+From+All+the+Pages+of+a+PDF+Document)

- [Big Faceless Java Library](http://bfo.com/products/pdf/) - [PDF Text Extraction in Java](http://bfo.com/blog/2011/11/16/pdf_text_extraction_in_java.html)

- [IText, a Java PDF Library](http://sourceforge.net/projects/itext/)


## Enterprise-Level ETL Solutions

Enterprise-Level (Cost > $1000) Extract Transfer Load (ETL) Solutions that Directly Read PDFs

- [Datawatch Modeler (Formerly Known as Monarch)](http://www.datawatch.com/form-page)

- [IDR Solutions](http://www.idrsolutions.com) - Online PDF to SVG and PDF to HTML5 conversions. This vendor also maintained the open source [JPedal library](http://sourceforge.net/projects/jpedal/) until last year.

- [Informatica B2B Data Transformation](http://www.informatica.com/us/products/b2b-data-exchange/b2b-data-transformation/)

- [Pradea](http://www.praedea.com/)

## Reviews, Listings and Comparisons:

- Duke University's Reporters Lab contains [reviews of many of the tools listed above](http://reviews.reporterslab.org/search?q=&type=products&category=pdf-tools-2011-11-09)

- PDFJailbreak provides [a list of tools](http://pdfjailbreak.com/tools) for extracting data from scientific papers in PDF format.

- [Peter Murray-Rust's Blog Post](http://blogs.ch.cam.ac.uk/pmr/2013/05/28/jailbreaking-the-pdf-a-wonderful-hackathon-and-a-community-leap-forward-for-freedom-1/) discussing software resources used at a May 2013 PDF Hackathon in Europe.

- [Comparison of iText, PDFBox and PDFTextExtractor](http://www.e-zest.net/blog/extracting-text-from-a-pdf-file/) by Madhura Oak



## Challenge descriptions

- [Comprehensive Annual Financial Reports](challenges/cafr-challenge.md)
- [House of Representatives Financial Disclosures (OpenSecrets.org)](challenges/house-financial-disclosures.md)
- [IRS Form 990 – Not-for-Profit Organization Reports](challenges/irs-form-990.md)
- [Amnesty International Annual Reports – Torture Incident Database](challenges/amnesty-challenge.md)
- [Unlocking U.S. Foreign Aid Reports Database](challenges/usaid-challenge.md)
- [Federal Government Federal Communications Commission Challenge](challenges/fcc-daily-releases.md)
- [NYCpedia challenge(s)](challenges/NYCpedia-challenges.md)




## What we would like to capture from each team

1. The source PDF file(s) itself
2. What type of PDF is it (tabular data, images, structured text, etc)
3. What the extraction goal is (what are we trying to get out of that and into what format)
4. Each tool that was attempted and how it was attempted
5. What the results were with that tool/particular attempt
6. What would have to be changed/added to the tool or process to achieve success (#3)...or if it actually worked, and where the results are
7. Contact info (names/email addresses/twitter handles/organizations) for each challenge group.




## The One Weird Thing You Need To Know About PDFs

It's a print format--the point is to tell a printer what things to print where. There's not much more order than that. But for our purposes we'll talk about three types of PDFs:

* "Text-based PDFs": The document has characters, fonts and font sizes, and information about where to place them stored in the document in a format that (a program) can read. Created by programs that can save to pdf directly. Dragging the mouse will highlight text.

* "Image-based PDFs": The document has images of pages, but not words themselves. Typically the result of scanning. You can't highlight text.

* "Embedded-text PDFs": The document has images of pages, but there's invisible text 'attached' to the document, so you can select text. Typically created by scanners that also run OCR. Sticky wicket: should you assume the attached text is better than what you'd get by running tesseract? Not necessarily (but it probably is...)



## Learn More About the PDF Format

- [Official Adobe PDF reference](http://www.adobe.com/devnet/pdf/pdf_reference.html)

- [PDF Explained](http://shop.oreilly.com/product/0636920021483.do) (O'Reilly)


- [A really basic overview of PDF is described here](http://partners.adobe.com/public/developer/en/livecycle/lc_pdf_overview_format.pdf)

- [Summary of the PDF file format:](http://www.planetpdf.com/developer/article.asp?ContentID=navigating_the_internal_struct)      

- [The PDF specification itself-1:](http://wwwimages.adobe.com/www.adobe.com/content/dam/Adobe/en/devnet/pdf/pdfs/pdf_reference_1-7.pdf) 

- [The PDF specification itself-2:](http://www.adobe.com/devnet/acrobat/pdfs/PDF32000_2008.pdf)
    

- [Presentations:](http://labs.appligent.com/files/2013/03/recognizing_malformed_pdf_f.pdf)




## reference 

* http://okfnlabs.org/blog/2016/04/19/pdf-tools-extract-text-and-data-from-pdfs.html

* https://pdfliberation.wordpress.com/

* https://github.com/jsfenfen/pdf17    

* https://github.com/jpswade/jpswade.github.io/blob/799035b3a6073f84cdb4f915b46155cfb33ba048/_posts/2009-10-24-scan-to-excel.md

* https://github.com/sarahcnyt/data-journalism/tree/master/class4
