
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