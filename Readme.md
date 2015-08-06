### AutoSum: Summarize Publications Automatically

The tool exploits the labor already expended by scholars in summarizing articles. It scrapes words next to citations across all openly available research citing a publication, and collates the output. The result is a very useful summary and data that are in a format that allows easy discovery of potential miscitations. 

#### Table of Contents

* [Get the Data](scripts/scholar.py)
  Scrapes all openly accessible research citing a particular publication using links provided by [Google Scholar](https://scholar.google.com).
  **Note** Google monitors scraping on Google scholar. 

* [Parse the Data](scripts/parse.py)
  Iterates through a directory with all the articles citing a particular research article, and using regular expressions, picks up sentences near a citation.

* Sample Results
  * [Summary](res/summary.csv)
  * [Miscitation](res/results.csv)

### Get the Data

1. Input: URL to Google Scholar Page of an article.
2. What the script does:
   * Goes to 'Cited By..'
   * Downloads a user specified number of publicly available papers (pdfs only for now) that cite the paper to a user specified directory. 
   * Creates a csv that tracks basic characteristics of each of the downloaded paper -- title, url, author names, journal etc. It also dumps relative path to downloaded file.

#### Usage

```
usage: scholar.py [-h] [-u USER] [-p PASSWORD] [-a AUTHOR] [-d DIR]
                  [-o OUTPUT] [-n N_CITES] [-v] [--version]
                  keyword [keyword ...]

positional arguments:
  keyword               Keyword to be searched

optional arguments:
  -h, --help            show this help message and exit
  -u USER, --user USER  Google account e-mail
  -p PASSWORD, --password PASSWORD
                        Google account password
  -a AUTHOR, --author AUTHOR
                        Author to be filtered
  -d DIR, --dir DIR     Output directory for PDF files
  -o OUTPUT, --output OUTPUT
                        CSV output filename
  -n N_CITES, --n-cites N_CITES
                        Number of cites to be download
  -v, --verbose
  --version             show program's version number and exit
```

#### Example

```
python scholar.py -v -d pdfs -o output.csv -n 100 -a "A Einstein" \
"Can quantum-mechanical description of physical reality be considered complete?"
```

### Parse Data 

1. Iterates through the pdfs using the csv generated above. 
2. Based on regex, gets the text and puts it in the same csv. If multiple regex are matched, everything is concatenated with a line space.

```
usage: searchpdf.py [-h] [-i INPUT] [-o OUTPUT] [-v] [--version]
                    regex [regex ...]

positional arguments:
  regex                 Regex to be search

optional arguments:
  -h, --help            show this help message and exit
  -i INPUT, --input INPUT
                        CSV input filename
  -o OUTPUT, --output OUTPUT
                        CSV output filename
  -v, --verbose
  --version             show program's version number and exit
```

#### Example

```
python searchpdf.py -v -i output.csv -o search-output.csv "\.\s(.{5,100}[\[\(]?Einstein.{2,30}\d+[\]\)])"
```

The regular expression matches a sentence (max 100 chars) following by author name "Einstein", any words (max 30 chars) and number with close bracket at the end.

#### Some Results from Social Science

#### Miscitations
Social scientists hold that few truths are self-evident. But some truths become obvious to all social scientists after some years of experience, including: a) [Peer review is a mess](http://gbytes.gsood.com/2015/07/24/reviewing-the-peer-review-with-reviews-as-data/), b) Faculty hiring is idiosyncratic, and c) Research is often miscited. Here we quantify the last portion.  

#### License

Released under the [MIT License](License.md)