# TrainingData

## Urdu OCR Training Data

For Recognition: 24K lines of ground truth for Urdu print (1860-1940) as Image-ALTO pair.

For Layout Analysis and Line Detection: pages with line location as baseline are available in `baseline`, while `centerline` has pages annotated with centerline as line location.

### Training
A binary dataset was compiled from the data provided here and OpenITI Urdu nasta’līq data with the following prompt:
`cat xml.xmllist | xargs -d '\n' ketos compile --ignore-splits -f alto -o ur.arrow --workers 10`

### Recognition Training prompt:
`ketos train -d cuda:0 -f binary --base-dir R --normalization NFD --min-epochs 30 -w 0 -s '[1,120,0,1 Cr3,13,32 Do0.1,2 Mp2,2 Cr3,13,32 Do0.1,2 Mp2,2 Cr3,9,64 Do0.1,2 Mp2,2 Cr3,9,64 Do0.1,2 S1(1x0)1,3 Lbx200 Do0.1,2 Lbx200 Do.1,2 Lbx200 Do]' -r 0.0001 ur.arrow`

### Urdu Models
The automatic text recognition (ATR) model `urATR_best_17-2-23.mlmodel` was trained with the data in this repository and OpenITI Urdu Nastaliq data (total 36K lines). It's at 95.5% CAR or less than 5% CER.

The segmentation model `urSEG_best.mlmodel` was trained from pages in the `baseline` directory.

Models trained with `kraken==4.3.3`

### Recognition Test Report

The file named `urATR_best_17-2-23_report.txt` provides a report of recognition errors for the model `urATR_best_17-2-23.mlmodel`.

A vast majority of errors for the recognition model are word boundary errors, which leads to either run-on errors or split errors.
