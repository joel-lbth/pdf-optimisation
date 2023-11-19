# pdf optimisation for planners

## problem

1. remove the decision notice (first three pages) of the pdf, to just leave the S106 legal agreement (split PDF).
2. then reduce the file size to under 5MB so that it will fit into our document management system on Exacom (compress PDF). 
3. plus OCR the PDF so that we can copy the text from the document into our system (legal clause text).

## solution 

_ocr runs before downsampling compression for higher recognition_

0. to install these packages, first install [scoop](https://scoop.sh/) an admin-free package manager
1. Split ([pdftk](https://scoop.sh/#/apps?q=pdftk&id=1baf4b75f78a347f787b20f45816e894392675bc))
2. Ocr ([ocrmypdf](https://ocrmypdf.readthedocs.io/en/latest/) requires [conda](https://scoop.sh/#/apps?q=anaconda&id=39e2245fc94826061b94becb2290e1e8a186f196) and [tesseract](https://scoop.sh/#/apps?q=tesseract&id=689900d11e96be93908d2234b27e6e7a1f23baf6))
3. Compress ([ghostscript](https://scoop.sh/#/apps?q=ghostscript&id=b7c42fc707108f38755f0d1296307889a602edcf))

# split
> remove the decision notice (first three pages) of the pdf, to just leave the S106 legal agreement (split PDF).

```sh
pdftk "data/raw/PA_99_00121 - DN and S106 agreement.pdf" cat 3-end output "data/interim/split.pdf"
```

# ocr
> OCR the PDF so that we can copy the text from the document into our system (legal clause text).

```sh
ocrmypdf --output-type pdfa --pdfa-image-compression jpeg data/interim/split.pdf "data/interim/ocr.pdf" --redo-ocr
```
    
    Start processing 8 pages concurrently
        1 redoing OCR
        ...
       12 redoing OCR
        4 [tesseract] lots of diacritics - possibly poor OCR
       13 redoing OCR
        5 [tesseract] lots of diacritics - possibly poor OCR
    
    Image optimization ratio: 1.13 savings: 11.7%
    Total file size ratio: 1.13 savings: 11.6%
    Output file is a PDF/A-2B (as expected)
    

# compress
> reduce the file size to under 5MB so that it will fit into our document management system on Exacom (compress PDF). 



```sh
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/screen -dNOPAUSE -dBATCH -sProcessColorModel=DeviceGray -sColorConversionStrategy=Gray -dOverrideICC -sOutputFile="data/processed/PA_99_00121 - DN and S106 agreement.pdf" data/interim/ocr.pdf
```

    GPL Ghostscript 10.02.0 (2023-09-13)
    Copyright (C) 2023 Artifex Software, Inc.  All rights reserved.
    This software is supplied under the GNU AGPLv3 and comes with NO WARRANTY:
    see the file COPYING for details.
    Processing pages 1 through 15.
    Page 1
    ...
    Page 15
    
# compare files pre- and post-optimisation


```sh
du -h "data/raw/PA_99_00121 - DN and S106 agreement.pdf" && du -h "data/processed/PA_99_00121 - DN and S106 agreement.pdf"
```

    17M	data/raw/PA_99_00121 - DN and S106 agreement.pdf
    1.5M	data/processed/PA_99_00121 - DN and S106 agreement.pdf
    
