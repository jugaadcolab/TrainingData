# TrainingData

## Urdu OCR Training Data

For Recognition: 24K lines of ground truth for Urdu print (1860-1940) as Image-ALTO pair.

For Layout Analysis and Line Detection: pages with line location as baseline are available in `baseline`, while `centerline` has pages annotated with centerline as line location.

## Bengali OCR Training Data
The dataset is not public yet, but the models are described below.

### Training
For Urdu recognition models, a binary dataset was compiled from ~28K lines from 24 texts and OpenITI Urdu nasta’līq data (~10K lines from 10 books) with the following command:
`cat xml.xmllist | xargs -d '\n' ketos compile --ignore-splits -f alto --workers 10`

For Bengali recognition model, a binary dataset of 40K lines from 40 books was compiled using the same command as above.
Students of the M.A. Bengali program at the Dept. of MIL & LS, Delhi University {Oishiki Bera, Shilpa Mishra, Sumita Bawali, Sindhu Som} contributed part of the training data for Bengali recognition model by double reading 14 texts. The training dataset also has mix-script text in Bengali and English.

### Recognition Training prompt:
`ketos train -d cuda:0 -f binary --base-dir R --normalization NFD --min-epochs 30 -w 0 -s '[1,120,0,1 Cr3,13,32 Do0.1,2 Mp2,2 Cr3,13,32 Do0.1,2 Mp2,2 Cr3,9,64 Do0.1,2 Mp2,2 Cr3,9,64 Do0.1,2 S1(1x0)1,3 Lbx200 Do0.1,2 Lbx200 Do.1,2 Lbx200 Do]' -r 0.0001`

### Urdu Models
The recognition model `urATR_best_17-2-23.mlmodel` in `models/ur` is at 97.57% CAR or less than 3% CER.

The segmentation model `urSEG_best.mlmodel` in `models/ur` was trained from pages in the `baseline` directory.

### Bengali Models
The recognition model `bnATR_best_14-04-23.mlmodel` in `models/bn` is at 98.82% or a little over 1% CER.

There are two types of segmentation model. `bnSEG_basic.mlmodel` can reliably label baselines for genres such as poetry, drama, and novel, provided the document has a simple layout, and can be used in situations where the default segmentation model in kraken fails, like poetry texts. It cannot infer regions. 

`bnSEG_complex.mlmodel` is more versatile as it is being trained on image and line annotations of documents with complex layouts, such as periodicals and dictionaries.

### Recognition Test Report

The file named `urATR_best_17-2-23_report.txt` in `models/ur` provides a report of recognition errors for the Urdu model `urATR_best_17-2-23.mlmodel`.

In practice, a vast majority of errors in the Urdu recognition model are word boundary errors, which leads to either run-on errors or split errors.


The file named `bnATR_best_14-4-23_report.txt` in `model/bn` provides a report of recognition errors for the Bengali recognition model `bnATR_best_14-04-23.mlmodel`. 

In practice, the Bengali recognizer may get diacritics for both consonants and vowels wrong, particularly in older texts. Rare conjucts, especially in loan words, remains another problem. It also limited in predicting English text, specifically upper-case letters. 


Models trained with [Kraken](https://github.com/mittagessen/kraken) `==4.3.9`
