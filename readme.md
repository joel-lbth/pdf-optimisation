# pdf optimisation for planners

1. remove the decision notice (first three pages) of the pdf, to just leave the S106 legal agreement (split PDF).
2. then reduce the file size to under 5MB so that it will fit into our document management system on Exacom (compress PDF). 
3. plus OCR the PDF so that we can copy the text from the document into our system (legal clause text).


0. to install these packages, first install [scoop](https://scoop.sh/)
1. Split ([pdftk](https://scoop.sh/#/apps?q=pdftk&id=1baf4b75f78a347f787b20f45816e894392675bc))
2. Compress ([ghostscript](https://scoop.sh/#/apps?q=ghostscript&id=b7c42fc707108f38755f0d1296307889a602edcf))
3. Ocr ([ocrmypdf](https://ocrmypdf.readthedocs.io/en/latest/) requires [conda](https://scoop.sh/#/apps?q=anaconda&id=39e2245fc94826061b94becb2290e1e8a186f196) and [tesseract](https://scoop.sh/#/apps?q=tesseract&id=689900d11e96be93908d2234b27e6e7a1f23baf6))

# 1
> remove the decision notice (first three pages) of the pdf, to just leave the S106 legal agreement (split PDF).



```python
!pdftk "data/raw/PA_99_00121 - DN and S106 agreement.pdf" cat 3-end output "data/interim/split.pdf"
```

# 2
> reduce the file size to under 5MB so that it will fit into our document management system on Exacom (compress PDF). 



```python
!gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/screen -dNOPAUSE -dBATCH -dColorImageResolution=150 -sOutputFile=data/interim/compress.pdf data/interim/split.pdf
```

    GPL Ghostscript 10.02.0 (2023-09-13)
    Copyright (C) 2023 Artifex Software, Inc.  All rights reserved.
    This software is supplied under the GNU AGPLv3 and comes with NO WARRANTY:
    see the file COPYING for details.
    Processing pages 1 through 15.
    Page 1
    Loading font Helvetica (or substitute) from %rom%Resource/Font/NimbusSans-Regular
    Page 2
    Page 3
    Page 4
    Page 5
    Page 6
    Page 7
    Page 8
    Page 9
    Page 10
    Page 11
    Page 12
    Page 13
    Page 14
    Page 15
    

# 3
> OCR the PDF so that we can copy the text from the document into our system (legal clause text).



```python
!ocrmypdf --output-type pdfa --pdfa-image-compression jpeg data/interim/compress.pdf "data/processed/PA_99_00121 - DN and S106 agreement.pdf" --skip-text

```

    
    Start processing 8 pages concurrently
        1 skipping all processing on this page
        2 skipping all processing on this page
        3 skipping all processing on this page
        4 skipping all processing on this page
        5 skipping all processing on this page
        6 skipping all processing on this page
        8 skipping all processing on this page
        9 skipping all processing on this page
        7 skipping all processing on this page
       10 skipping all processing on this page
       11 skipping all processing on this page
       12 skipping all processing on this page
       13 skipping all processing on this page
       14 skipping all processing on this page
       15 skipping all processing on this page
    
    Postprocessing...
    
    
    
    [WinError 2] The system cannot find the file specified
    [WinError 2] The system cannot find the file specified
    [WinError 2] The system cannot find the file specified
    [WinError 2] The system cannot find the file specified
    Image optimization ratio: 1.03 savings: 3.1%
    Total file size ratio: 1.03 savings: 2.8%
    Output file is a PDF/A-2B (as expected)
    


```python
!ls -lh data/raw/ && ls -lh data/processed
```

    total 17M
    -rw-r--r-- 1 Joseph.Leach 1049089 17M Nov 16 13:57 PA_99_00121 - DN and S106 agreement.pdf
    total 1.2M
    -rw-r--r-- 1 Joseph.Leach 1049089 1.2M Nov 19 13:09 PA_99_00121 - DN and S106 agreement.pdf
    
