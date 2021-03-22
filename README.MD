# **Conditional Generative Adversarial Networks for Speed Control in TrajectorySimulation**

Conditioned Speed GAN (CSG): A generative system that can be conditioned on agent speed and semantic classes of agents, to simulate multimodal and realistic trajectories based on user defined control. 

**To reproduce the project, run the following command:**

Initially, clone the repository

To install all dependencies, run:
````
pip install -r requirements.txt
````

### Datasets:
| Datasets |      Single-Agent      |      Multi-Agent      |
|:-------------:|:-------------:|:-------------:|
| Obs len, Pred len |  3.2 sec (8 frames) and 4.8 sec (12 frames) | 2 sec (20 frames) and 3 sec (30 frames) |
| Path |    In Project: single_condition_dataset  |  Argoverse dataset version v1.1 |
| Approach | Hold-one out |    5126, 1678 files for train and test respectively https://www.argoverse.org/data.html#forecasting-link |
| Training | Trained on GPU (USE_GPU flag = 1) |    Trained on CPU (USE_GPU flag = 0) |


## Pretrained Models:
The code was developed and tested using Python 3.7.2.
- For single-agent, `USE_GPU=1`:
    - `'NoAgg' folder`: contains 5 pretrained models for CSG.
    - `'PM' folder`: contains 5 pretrained models for CSG-P.
    - `'Concat' folder`: contains 5 pretrained models for CSG-C.
    - `'Attention' folder`: contains 5 pretrained models for CSG-A.
    - `'Extrapolation' folder`: contains 3 pretrained models for CSG-C (for minimum, medium and maximum speed folds).
- For multi-agent, `USE_GPU=0`:
    - `'MultiAgent' folder`: contains pretrained model for CSG for Argoverse dataset.
    

For `simulation` environment, use `TEST_METRIC = 2` and for `prediction` environment, use `TEST_METRIC = 1` To evaluate the model, run:
````
python evaluate_model.py
````

**Note:** The speed value should be 0 < speed > 1

To train the model from scratch, 

Change the CHECKPOINT_NAME (provide a new filename), change the required aggregation flags and run:
````
python train.py
````

# Flag descriptions:

- `H_DIM_GENERATOR_SINGLE_CONDITION`, `H_DIM_GENERATOR_MULTI_CONDITION`: hidden dimensions used for both encoder and decoder LSTM in G for single and multi agent respectively
- `H_DIM_GENERATOR_SINGLE_CONDITION`, `H_DIM_GENERATOR_MULTI_CONDITION`: hidden dimensions used in D for single and multi agent respectively.
- `EMBEDDING_DIM`: embedding dimension
- `MLP_INPUT_DIM_SINGLE_CONDITION`, `MLP_INPUT_DIM_MULTI_CONDITION`: input shape of MLP for single and multi agent respectively.
- `NOISE_DIM`: Gaussian noise dimension
- `G_LEARNING_RATE`, `D_LEARNING_RATE`: Learning rate of G and D
- `NUM_LAYERS`: Number of LSTM layers
- `DROPOUT`: Dropout to be used in the model
- `NUM_EPOCHS`: Number of epochs the model is to be trained. 
- `OBS_LEN`, `PRED_LEN`: Observed and predicted length of the trajectories
- `CHECKPOINT_NAME`: Path of the checkpoint at which the weights needs to be stored.
- `BATCH`: Batch size
- `BATCH_NORM`: Batch normalization option
- `ACTIVATION`: We use leakyrelu activation function.
- `G_STEPS`, `D_STEPS`: The number of feed-forward and backward passes for generator and discriminator respectively.
- `NUM_SAMPLES`: Number of samples to test the model
- `NOISE`: Noise to be added.

## Aggregation Module Flags
- `AGGREGATION_TYPE`: 'pooling' for CSG-P, 'attention' for CSG-A, 'concat' for CSG-C, 'None' for 'CSG'

Note: In ETH/UCY, as each dataset has different max speed value, we implement a common threshold for max_value across all datasets and consider the speeds above the threshold as max_speeds.
In argoverse dataset, we set the ratio of low_speed:high_speed as 100:1.


## SPEED CONTROL FLAGS
- `TRAIN_METRIC`: We utilize the next speed information from the ground truth to train the model. This value always holds 0. 
- `TEST_METRIC`: For prediction environment, use 1 and for Simulation environment use 2.  
- The following flags are applicable only if the `TEST_METRIC` = 2
    - `STOP_PED`: This flag stops all the agents. This implies that a speed value of 0 is imposed on all the agents in ETH/UCY.
    - `CONSTANT_SPEED_FOR_ALL_PED`: This flags imposes a constant speed to all pedestrians in ETH/UCY. If set True, enter a value between 0 and 1 in CS_SINGLE_CONDITION variable
    - `DIFFERENT_SPEED_MULTI_CONDITION`:  Used during Multi-agent simulation. If set True, enter a value between 0 and 1 to AV_SPEED, OTHER_SPEED, AGENT_SPEED.