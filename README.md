Implementation of a Reinforcement Learning Algorithm to find a truss structure matching a target stiffness .
Framework: ray, rllib library;

[]: # Language: python

What files are relevant & up to date:
- train.py: trains the model
- parameters.py: contains the parameters of the model that are frequently changed (e.g. number of iterations, truss size etc.)
- env_class (_2/_3/env_03, env_goal): 
  - the environment for algorithm; 
  - most of the relevant code is in those files, i.e. the step, state, reward definition, termination criterion etc.
- parameters.env_class = 
    - 0: env_goal - changing stiffness target, threshold final state;
    - 1: threshold approach;
    - 2: derivative approach;
    - 3: fixed goal but final state chosen by network - not 3 && custom model!!;
    - 4: mix from 0 and 3: changing stiffness target with final state chosen by network
- fem.py: runs the BeamHomogenization-fromFile2D on the local laptop, in Docker container; pathes needs to be adapted for different computer
- fem_euler.py: runs the BeamHomogenization-fromFile2D on euler, where the BeamHomogenization-fromFile2D is in the same directory as the other .py files
- get_stiffness_goal.py: creates a feasible, random stiffness goal (for env_goal, env_03)
- custom_model.py: creates a custom model for the network, is used for action space masking
- unitCell.py: the unit cell class, contains many functions for different state representations etc. 
- run_pretrained_model: runs a pretrained model once; 
- create_plots_for_thesis.py: this file loads a pre-trained model; if parameters.env_class == 0 or 4, it computes and plots some figures to evaluate the performance of the trained model

Additional files:
- checker.py: this file displays and computes the loss for a structure that is defined in the file; so you can create any structure (for example based on a trained model) and compute it's loss;
- plotDirectionalStiffness.py: creates the plot for the directional stiffness;

for the comments in the enviroments, env_03 is the best commented, since it is the most "advanced" environment
with changing stiffness goal and termination chosen by network;


Folder Structure on euler:
To run the code on euler, the ae-108 BeamHomogenization-fromFile2D must be compiled.
This repo (all of the python files) should be placed in the build/drivers/beamHomogenization folder, where the BeamHomogenization-fromFile2D is located.
Pathes must be adapted in the fem_euler.py file.
If in doubt, use ctrl+f to search for "jaicher" in the code and replace it with your username.
The tensorboard output will be written to home/username/ray_results/

## Usage

Clone the repo and connect into its top level directory.


```
pip install -r requirements.txt
```

To run Ray RLlib to train a policy based on this environment:
- change the pathes in parameters.py if you run it on your laptop
- please try to run the BeamHomogenization-fromFile2D on your local machine first;
- if your cmd for executing the BeamHomogenization-fromFile2D is different from 'docker exec -w /mnt/io/build/drivers/beamHomogenization/ ae108-legacy-dev-1 ' \
          './BeamHomogenization-fromFile2D', please change it in the fem.py
          
```
python train.py
```


## Kudos

h/t:
  - <https://medium.com/@apoddar573/making-your-own-custom-environment-in-gym-c3b65ff8cdaa>
  - <https://github.com/openai/gym/blob/master/docs/creating-environments.md>
  - <https://stackoverflow.com/questions/45068568/how-to-create-a-new-gym-environment-in-openai>
