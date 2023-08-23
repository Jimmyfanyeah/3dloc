# 3D Localization of Point Sources
This branch shows a demo of 3D localization of point sources using rotating PSF.

## Installation
Set up the environment for running codes using ANACONDA.

First clone the  repository
```
git clone -b code_share https://github.com/Jimmyfanyeah/3DLocalization.git
cd "code_share"
```

Then start a virtual environment with new environment variables
```
conda env create -f environment.yml
conda activate environment
```

## Executing program
The whole framework contains three parts: 
1. Training dataset generation
2. Model training
3. (Optional) Infer on mock set and select hard sample subset
4. Infer on test dataset with different densities of point sources

### Part 1 Training dataset generation
Use the shell file to generate training dataset. A number of 10,000 observed and noiseless images are saved in '../../data_train/train' and '../../data_train/clean' respectively.
```
bash 1_run_matlab_gen_train.sh
```

### Part 2 Model training
For training, run shell file `4_run_training.sh`, which calls `main.py` with given parameters. Parameters are defined in lines 166 to 194 in `main.py`.

### Part 3 (Optional) Infer on mock set and select hard sample subset
During one iteration of hard sample strategy, a mock set is first generated by running `3_run_matlab_gen_more_test.sh`. By running `5_run_testing.sh`, the mock set is then infered based on trained model, followed by postprocessing together with evaluation. During the evaluation, the index of hard samples, whose recall rate is lower than the threshold, will be record. By running `6_add_hardsamples.sh`, hard samples and corresponding labels are copied to the training data path to update the training dataset.

### Part 4 Infer on test dataset with different densities of point sources
To compare with KL-NC algorithm, first run `2_run_matlab_var_gen_test.sh` to generate the test dataset and the results from KL-NC algorithm.
Running `8_testing.sh` to infer and evaluate on the same test set as KL-NC algorithm used. It also calls `main.py` with the argument `--train_or_test=test`. Predicted coordinates will be saved in csv file



