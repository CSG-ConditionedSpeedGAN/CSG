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

