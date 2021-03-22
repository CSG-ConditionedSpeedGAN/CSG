# **Conditional Generative Adversarial Networks for Speed Control in TrajectorySimulation**

**To reproduce the project, run the following command:**

Initially, clone the repository

To install all dependencies, run:
````
pip install -r requirements.txt
````

### Datasets:
- For single condition, use single_condition_dataset (ETH and UCY)
- For multi condition:
    - We utilized the current argoverse dataset version v1.1 (https://www.argoverse.org/data.html#forecasting-link). Select 5126, 1678 files to form the train and test respectively.
- For extrapolation feature, use single_condition_dataset/extrapolation_dataset.
- OBS_LEN and PRED_LEN for single agent is 3.2 seconds (8 frames) and 4.8 seconds (12 frames) respectively 
- OBS_LEN and PRED_LEN for multi agent is 2 seconds (20 frames) and 3 seconds (30 frames) respectively


To use the pre-trained model, follow:
- For Single-agent, turn USE_GPU flag to 1 and: 
    - To run the model without aggregation: change the flag AGGREGATION_TYPE = 'None' and use the checkpoints from the folder 'NoAgg'
    - To run the model with PM: change the flag AGGREGATION_TYPE = 'pooling' and use the checkpoints from the folder 'PM'
    - To run the model with Concat: change the flag AGGREGATION_TYPE = 'concat' and use the checkpoints from the folder 'Concat'
    - To run the model with Attention: change the flag AGGREGATION_TYPE = 'attention' and use the checkpoints from the folder 'Attention'
- For Multi-agent, turn USE_GPU flag to 0 and change the flag AGGREGATION_TYPE = 'None' and use the checkpoints from the folder 'MultiAgent'
- For Extrapolation, activate the flag EXTRAPOLATE_MIN, EXTRAPOLATE_MId or EXTRAPOLATE_MAX (one at time) and change the flag AGGREGATION_TYPE = 'concat' and use the checkpoints from the folder 'Extrapolation'.

* For Simulation: change the TEST_METRIC to 2 and select one of the following options in CONSTANTS.py:
    * For Single-agent simulation, turn USE_GPU flag to 1 and:
        * To maintain constant speeds for all pedestrians: Change the flag CONSTANT_SPEED_SINGLE_CONDITION to True and enter a value between 0 and 1 in CS_SINGLE_CONDITION variable
        * To stop all the pedestrians: Change the flag STOP_PED_SINGLE_CONDITION to True
    * For Multi-agent Simulation, turn USE_GPU flag to 0 and:
        * To impose different speeds to different agents: Change the flag DIFFERENT_SPEED_MULTI_CONDITION to True and enter a value between 0 and 1 to AV_SPEED, OTHER_SPEED, AGENT_SPEED. 

* For Prediction: change the TEST_METRIC to 1.

To evaluate the model with the simulated trajectories, run:
````
python evaluate_model.py
````

**Note:** The speed value should be 0 < speed > 1

To train the model from scratch, 

Change the CHECKPOINT_NAME (provide a new filename), change the required aggregation flags and run    
````
python train.py
````