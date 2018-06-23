# Gym Environments

In order to use reinforcement learning methods, we must have an environment.
As such, we have created a few robotic environments, using OpenAI's Gym environments.  

(https://github.com/openai/gym)

## Available environments
- Kuka arm
    - KukaButtonGymEnv-v0: Kuka arm with a single button in front.
    - KukaRandButtonGymEnv-v0: Kuka arm with a single button in front, and some randomly positioned objects
    - Kuka2ButtonGymEnv-v0: Kuka arm with 2 buttons next to each others, they must be pressed in the correct order.
    - KukaMovingButtonGymEnv-v0: Kuka arm with a single button in front, slowly moving left to right.
- mobile robot
    - MobileRobotGymEnv-v0: A mobile robot on a 2d terrain where it needs to park on a button.
    - MobileRobot2TargetGymEnv-v0: A mobile robot on a 2d terrain where it needs to park on two buttons, in the correct order.
    - MobileRobot1DGymEnv-v0: A mobile robot on a 1d slider where it can only go up and down, it must park it self on the button.
    - MobileRobotLineTargetGymEnv-v0: A mobile robot on a 2d terrain where it needs to park on a colored band going across the terrain.
- Baxter
    - Baxter-v0: A bridge environment between Gazebo and the tools, in order to test on a real Baxter robot.
    
## Add your own
1. Create a class that inherits ```environments.srl_env.SRLGymEnv``` which implements your environment. 
You will need to define specifically:
    * ```getTargetPos()```: returns the position of the target.
    * ```getGroundTruthDim()```: returns the number of dimensions used to encode the ground truth.
    * ```getGroundTruth()```: returns the ground truth.
    * ```step(action)```: step the environment in simulation with the given action.
    * ```reset()```: re-initialise the environment.
    * ```render(mode='human')```: returns an observation of the environment.
2. Add your class to the ```registered_env``` dictionary in ```environments/registry.py```, 
using this format ```NAME: (CLASS, SUPER_CLASS, PLOT_TYPE)```, where:
    * ```NAME```: is your environment's name.
    * ```CLASS```: is your class that is a subclass of ```SRLGymEnv```.
    * ```SUPER_CLASS```: is the super class of your class, this is for saving all the globals and parameters.
    * ```PLOT_TYPE```: is the type of plotting for ```replay.enjoy_baselines```,
    defined by the enumerator ```PlottingType``` in ```environments/__init__.py```,
    can be ```PLOT_2D``` or ```PLOT_3D``` (use ```PLOT_3D``` if unsure).
3. Add the name of the environment to ```config/srl_models.yaml```, with the location of the saved model for each SRL model (can point to a dummy location, but must be defined).
4. Now you can call your environment using ```--env NAME``` with ```train.py```, ```pipeline.py``` or ```test_env.py```. 