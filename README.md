# Automatic ECG diagnosis using a deep neural network
Scripts and modules for training and testing deep neural networks for ECG automatic classification.

## Requirements

This code was tested on Python 3 with Tensorflow `2.2`. 

## Model

The model used is a residual neural. The neural network architecture implementation in Keras is available in ``model.py``. To print a summary of the model layers run:
```bash
$ python model.py
```

![resnet](https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs41467-020-15432-4/MediaObjects/41467_2020_15432_Fig3_HTML.png?as=webp)

The model receives an input tensor with dimension `(N, 4096, 12)`, and returns an output tensor with dimension `(N, 6)`,
for which `N` is the batch size.

The model can be trained using the script `train.py`. 
```
- **input**: `shape = (N, 4096, 12)`. The input tensor should contain the  `4096` points of the ECG tracings
sampled at `400Hz` (i.e., a signal of approximately 10 seconds). Both in the training and in the test set, when the
signal was not long enough, we filled the signal with zeros, so 4096 points were attained. The last dimension of the 
tensor contains points of the 12 different leads. The leads are ordered in the following order: 
`{DI, DII, DIII, AVR, AVL, AVF, V1, V2, V3, V4, V5, V6}`. All signal are represented as
32 bits floating point numbers at the scale 1e-4V: so if the signal is in V it should be multiplied by 
1000 before feeding it to the neural network model. 


- **output**: `shape = (N, 6)`. Each entry contains a probability between 0 and 1, and can be understood as the
probability of a given abnormality to be present. The abnormalities it predicts are  **(in that order)**: 1st degree AV block(1dAVb),
 right bundle branch block (RBBB), left bundle branch block (LBBB), sinus bradycardia (SB), atrial fibrillation (AF),
sinus tachycardia (ST).  The abnormalities are not mutually exclusive, so the probabilities do not necessarily
sum to one. 

![abnormalities](https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs41467-020-15432-4/MediaObjects/41467_2020_15432_Fig1_HTML.png?as=webp)

## Datasets

- The testing dataset can be downloaded in:
[doi: 10.5281/zenodo.3625006](https://doi.org/10.5281/zenodo.3625006). 
```
- Part of the training data (the CODE-15\% dataset) is openly available in: [doi: 10.5281/zenodo.4916206 ](https://doi.org/10.5281/zenodo.4916206).

## Scripts

- ``train.py``: Script for training the neural network. To train the neural network run: 
```bash
$ python train.py PATH_TO_HDF5 PATH_TO_CSV
``

- ``predict.py``: Script for generating the neural network predictions on a given dataset.
```bash
$ python predict.py PATH_TO_HDF5_ECG_TRACINGS PATH_TO_MODEL  --ouput_file PATH_TO_OUTPUT_FILE 
```

- ``generate_figures_and_tables.py``: Generate figures and tables from the paper "Automatic Diagnosis to the Short-Duration12-Lead ECG using a Deep Neural Network". Make sure to execute the script from the root folder,
so all relative paths are correct. So first run:
```
$ cd /path/to/automatic-ecg-diagnosis
```
Then the script
 ```bash
$ python generate_figures_and_tables.py
```
It should generate the tables and figure in the folder `outputs/`

- ``model.py``: Auxiliary module that defines the architecture of the deep neural network.
To print a summary of the model  layers run:
```bash
$ python model.py
```

