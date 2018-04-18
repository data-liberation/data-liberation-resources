# PDF-to-Tables Table

Making a table to compare how well tools extract tabular data from PDFs.


## Tools

- [Tabula-Java](https://github.com/tabulapdf/tabula-java): the scriptable engine behind Tabula, via OpenCollective.
- [pdfplumber](https://github.com/jsvine/pdfplumber): a high-level Python interface to pdfminer, created by Jerm-Sing-Vine
- [PDFTables.com](https://pdftables.com/) - a cloud service from the Sensible Code Company
- [CometDocs](https://www.cometdocs.com/) - a cloud service with PDF-to-XLS functionality
- [ABBYY FineReader Pro for Mac](https://www.abbyy.com/en-us/finereader/pro-for-mac/) - What I personally use. Costs money. No idea if it differs via cross-platform. Has no automatable-interface. But very accurate and dependable.
- [ZamZar](http://www.zamzar.com/): Provides pdf-to-table functionality at the level of Google Drive, which is to say, not much at all. Might include it just as a baseline.
- [pdftotext](https://poppler.freedesktop.org/): part of the Poppler suite. Its `-layout` option provides a conversion that attempts to visually emulate the original PDF, but the result is not delimited data.

## Benchmark PDFs

Trying to collect publicly-available PDFs that exhibit a wide-range of real-world data-as-PDF characteristics. Feel free to suggest some!

Note: not interested in PDFs that contain _scanned_ documents. Of the tools we're testing, only ABBYY has OCR-to-Excel functionality.


- [pdfs/ca-warn-2013.pdf](pdfs/ca-warn-2013.pdf): [California 2013 WARN Report](http://www.edd.ca.gov/jobs_and_training/warn/eddwarncn13.pdf)
- [pdfs/ca-warn-7-1-2016.pdf](pdfs/ca-warn-7-1-2016.pdf): [California WARN notices processed from July 1, 2016 through September 25, 2016](http://www.edd.ca.gov/jobs_and_training/warn/WARN-Report-for-7-1-2016-to-09-25-2016.pdf)
- [pdfs/hcctransparency-report-2010.pdf](pdfs/hcctransparency-report-2010.pdf): [2010 Jan. to Dec. Janssen Pharmaceuticals](http://www.hcctransparency.com/report/pdf/export/yearly_summary_134.pdf?year=2010&type=hcp) via [ProPublica D4D](https://projects.propublica.org/d4d-archive/companies/johnson-johnson)
- [pdfs/condemnedinmatelistsecure.pdf](pdfs/condemnedinmatelistsecure.pdf):  [California Condemned Inmate List](http://www.cdcr.ca.gov/capital_punishment/docs/condemnedinmatelistsecure.pdf)
- [pdfs/nypd-weekly-stats.pdf](pdfs/nypd-weekly-stats.pdf): [NYPD Weekly Crime Stats](http://www.nyc.gov/html/nypd/downloads/pdf/crime_statistics/cs-en-us-city.pdf)
- [pdfs/menlo-park-sunridge-cad-interface.pdf](pdfs/menlo-park-sunridge-cad-interface.pdf) - A PDF containing one actual table, and a few pages of screenshots
- [pdfs/nics-firearm-background-checks.pdf](pdfs/nics-firearm-background-checks.pdf) - NICS Firearm Background Checks, as copied from [jsvine/pdfplumber](https://github.com/jsvine/pdfplumber/blob/c5bbf497ba80e38650da9bac0f47deeb4ab6105f/examples/pdfs/background-checks.pdf)

## Todos


- [ ] Come up with a methodology (what's the baseline?) to compare the results across tools. Fidelity to table structure is the main goal, but not the only way that tools differ.

- [ ] Makes more sense to split every PDF into separate pages. Easier to do unit testing of each program/service.
- [ ] Think of functionality tests (handling of word-wrap, header/footer text) beyond just cell-by-cell accuracy.
- [ ] Figure way to kind of automate CometDocs and ABBYY and other services

Prospective project tree:

      ├── README.md
      ├── pdfs
      |    └── ca-warn-2013
      |        ├── 001.pdf
      |        ├── 002.pdf
      |        └── 003.pdf
      └── results
          ├── pdfplumber
          |   └── ca-warn-2013
          |       ├── 001.csv
          |       ├── 002.csv
          |       └── 003.csv
          └── tabula-java
              └── ca-warn-2013
                  ├── 001.csv
                  ├── 002.csv
                  └── 003.csv



## Example test suite and results


~~~sh
java -jar \
    bins/tabula-0.9.1-jar-with-dependencies.jar --pages all \
    pdfs/nypd-weekly-stats.pdf \
    > results/tabula-java/nypd-weekly-stats.csv

java -jar \
    bins/tabula-0.9.1-jar-with-dependencies.jar --pages all \
    pdfs/menlo-park-sunridge-cad-interface.pdf \
    > results/tabula-java/menlo-park-sunridge-cad-interface.csv

~~~
