# CNN brain mask script usage
This guide explains how to use the CNN brain masking script in this package directory.

## Data directory tree setup
Please refer to the README.md file in the project root for data directory tree setup.

## Required files
This algorithm requires the following image contrasts coregistered to the same space:
* T1 pre-contrast
* T1 post-contrast
* T2
* T2/FLAIR
* DWI

## Image filenames
The image files should follow the naming convention specified in the data directory tree setup. The suffixes for each individual contrast can be specified in the parameter file for this algorithm "mask.json".

The defaut naming convention using patient ID 123456, for example, is as follows (note: the "_w" suffix refers to "warped" i.e. co-registered image data):
* T1 pre-contrast - "123456_T1gad_w"
* T1 post-contrast - "123456_T1_w"
* T2 - "123456_T2_w"
* T2/FLAIR - "123456_FLAIR_w"
* DWI - "123456_DWI_w"

## Usage
To create a brain mask for a single directory (in this case patient ID 123456):
```json
python cnn_brain_mask.py -p mask.json -i data_dir/123456/
```
By default, the output mask will be saved in a subdirectory "predictions". The output directory can also be specified manually using:
```json
python cnn_brain_mask.py -p mask.json -i data_dir/123456/ -o mask_outputs/
```