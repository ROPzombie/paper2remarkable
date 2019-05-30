# arxiv2remarkable.py

This script makes it as easy to get a PDF on your reMarkable from any of the 
following sources:

- an arXiv url (either ``arxiv.org/abs/...`` or ``arxiv.org/pdf/...``)
- a PubMed Central url (either to the HTML or the PDF)
- an ACM citation page url (``https://dl.acm.org/citation.cfm?id=...``)
- a url to a PDF file
- a local file.

The script takes the source and:

1. Downloads the pdf if necessary
2. Removes the arXiv timestamp
3. Crops the pdf to remove unnecessary borders
4. Shrinks the pdf file to reduce the filesize
5. Generates a nice filename based on author/title/year of the paper (arXiv 
   only)
6. Uploads it to your reMarkable using ``rMapi``.

Optionally, you can download a paper but not have it uploaded to the 
reMarkable using the ``-n`` switch. Also, the ``--filename`` parameter to the 
script can be used to provide an explicit filename for on the reMarkable.

Here's the full help of the script:

```bash
usage: arxiv2remarkable.py [-h] [-v] [-n] [-d] [--filename FILENAME]
                           [-p REMARKABLE_DIR] [--rmapi RMAPI]
                           [--pdfcrop PDFCROP] [--pdftk PDFTK] [--gs GS]
                           input

positional arguments:
  input                 url to an arxiv paper, url to pdf, or existing pdf
                        file

optional arguments:
  -h, --help            show this help message and exit
  -v, --verbose         be verbose (default: False)
  -n, --no-upload       don't upload to the reMarkable, save the output in
                        current working dir (default: False)
  -d, --debug           debug mode, doesn't upload to reMarkable (default:
                        False)
  --filename FILENAME   Filename to use for the file on reMarkable (default:
                        None)
  -p REMARKABLE_DIR, --remarkable-path REMARKABLE_DIR
                        directory on reMarkable to put the file (created if
                        missing) (default: /)
  --rmapi RMAPI         path to rmapi executable (default: rmapi)
  --pdfcrop PDFCROP     path to pdfcrop executable (default: pdfcrop)
  --pdftk PDFTK         path to pdftk executable (default: pdftk)
  --gs GS               path to gs executable (default: gs)
```

And here's an example with verbose mode enabled that shows everything the 
script does:
```bash
$ python arxiv2remarkable.py -v https://arxiv.org/abs/1811.11242
2019-05-30 00:38:27 - INFO - Starting ArxivProvider
2019-05-30 00:38:27 - INFO - Getting paper info from arXiv
2019-05-30 00:38:27 - INFO - Downloading url: https://arxiv.org/abs/1811.11242
2019-05-30 00:38:27 - INFO - Generating output filename
2019-05-30 00:38:27 - INFO - Created filename: Burg_Nazabal_Sutton_-_Wrangling_Messy_CSV_Files_by_Detecting_Row_and_Type_Patterns_2018.pdf
2019-05-30 00:38:27 - INFO - Downloading file at url: https://arxiv.org/pdf/1811.11242.pdf
2019-05-30 00:38:32 - INFO - Downloading url: https://arxiv.org/pdf/1811.11242.pdf
2019-05-30 00:38:32 - INFO - Removing arXiv timestamp
2019-05-30 00:38:34 - INFO - Cropping pdf file
2019-05-30 00:38:37 - INFO - Shrinking pdf file
2019-05-30 00:38:38 - INFO - Starting upload to reMarkable
2019-05-30 00:38:42 - INFO - Upload successful.
```

## Dependencies

The script requires the following external programs to be available:

- [pdftk](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/)
- [pdfcrop](https://ctan.org/pkg/pdfcrop?lang=en): usually included with a 
  LaTeX installation.
- [GhostScript](https://www.ghostscript.com/)
- [rMAPI](https://github.com/juruen/rmapi)

If these scripts are not available on the ``PATH`` variable, you can supply them 
with the relevant options to the script.

The script also needs the following Python packages:

- [BeautifulSoup4](https://pypi.org/project/beautifulsoup4/): parsing HTML
- [requests](https://pypi.org/project/requests/): getting HTML
- [PyPDF2](https://github.com/mstamy2/PyPDF2): verifying urls point to PDF
- [titlecase](https://pypi.org/project/titlecase/): fancy titles

You can use this line:

```bash
pip install --user bs4 requests PyPDF2 titlecase
```

# Notes

License: MIT

Author: G.J.J. van den Burg