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
| Path |    In Project: use single_condition_dataset  |  Argoverse dataset version v1.1 |
| Approach | Hold-one out |    5126, 1678 files for train and test respectively |
| Training | Trained on GPU (USE_GPU flag = 1) |    Trained on CPU (USE_GPU flag = 0) |
    
Argoverse dataset version v1.1: https://www.argoverse.org/data.html#forecasting-link


## Pretrained Models:

- `'NoAgg' folder`: contains 5 pretrained models for CSG.
- `'PM' folder`: contains 5 pretrained models for CSG-P.
- `'Concat' folder`: contains 5 pretrained models for CSG-C.
- `'Attention' folder`: contains 5 pretrained models for CSG-A.

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