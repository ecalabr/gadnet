# Gadnet
Gadnet is a 3D fully convolutional deep neural network designed to synthesize gadolinium enahnced T1 weighted brain MR images from pre-contrast images.

# Installation
The following command will clone a copy of Gadnet to your computer using git:
```bash
git clone https://github.com/ecalabr/gadnet.git
```

**NOTE: It is recommended that you create at python 3 conda environment to run this package. Required modules are listed in the included requirements.txt file. An NVIDIA GPU and a working installation of the CUDA toolkit and cuDNN are strongly recommended.**

# Data directory tree setup
Gadnet expects your image data to be in Nifti format with a specific directory tree. The following example starts with any directory. Here we are using the "example_data" directory included in the git repository.

```bash
example_data/
```
This is an example of the base directory for all of the image data that you want to use. This folder should contain one or more individual patient folders each containing their respective patient image data.

```bash
example_data/example001/
```
This is an example of an individual patient study directory. The directory name is typically a patient ID, but can be any folder name that does not contain the "_" character

```bash
example_data/example001/example001_T1gad.nii.gz
```
This is an example of a single patient image. The image file name must start with the patient ID (must be identical the patient directory name) followed by a "_" character. All Nifti files are expected to be g-zipped. The suffixes of the various contrasts are specified in the params file described below.

**NOTE: Image data must be pre-processed including co-registration, brain masking (AKA skull stripping), and normalization to zero mean unit standard deviation. Coil bias correction prior to normalization is recommended when using inhomogeneous receive coils.**

# Usage
## Train
The following command will train the network using the parameters specified in the param file (-p):
```bash
python train.py -p gadnet/gadnet_params.json
```
For help with parameter files, please refer to a separate markdown file in the "gadnet" subdirectory of this project.

Training outputs will be located in the model directory as specified in the param file.

**NOTE: Training on the "example_data" directory included in the git repository only contains images for 1 patient.**
 
## Predict
The following command will use the trained "full model" weights checkpoint (-c) to predict for a single patient (-s) with ID example001:
```bash
python predict.py -p gadnet/gadnet_full_params.json -s example_data/example001 -c full_model
```
To predict using the "reduced model", use the following command. Note that the specified paramter file (-p) must match the model checkpoint:
```bash
python predict.py -p gadnet/gadnet_reduced_params.json -s example_data/example001 -c reduced_model
```
By default, the predicted output will be placed in the model directory in a subdirectory named "prediction"; however, the user can specify a different output directory using "-o":
```bash
python predict.py -p gadnet/gadnet_full_params.json -s example_data/example001 -c full_model -o outputs/
```

## Evaluate
The following command will evaluate the trained network using the testing portion of the data as specified in the params file (-p) using the full model weights checkpoint (-c):

**NOTE: The "example_data" directory included in the git repository only contains images for 1 patient. If you want to run evaluation on this directory, you will need to set the "train_fract" parameter to 0 in the relevant param file.**

```bash
python evaluate.py -p gadnet/gadnet_full_params.json -c full_model
```
By default, the evaluation output will be placed in the model directory in a subdirectory named "evaluation"; however, the user can specify a different output directory using "-o":
```bash
python evaluate.py -p gadnet/gadnet_full_params.json -c full_model -o eval_outputs/
```
Evaluation metrics can be manually specified using "-t":
```bash
python evaluate.py -p gadnet/gadnet_full_params.json -c full_model -t smape ssim logac
```

## Citation
Please cite the following publication(s) if you use any part of this project for your own work:

1. [manuscript accepted at Radiology: Artificial Intelligence]

Citation(s) will be updated once the associated manuscripts are published.
