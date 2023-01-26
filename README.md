<h1 align="center">
Estimation of Human Base Kinematics using Dynamical Inverse Kinematics and Contact-Aided Lie Group Kalman Filter
</h1>
<div align="center">
<i>
P. Ramadoss, L.Rapetti, Y.Tirupachuri, R.Grieco, G. Milani, E. Valli, S. Dafarra, S. Traversaro and D. Pucci "Estimation of Human Base Kinematics using Dynamical Inverse Kinematics and Contact-Aided Lie Group Kalman Filter" in IEEE 2022 International Conference on Humanoid Robots
</i>
</div>

<p align="center">


[ICHR 2022 - Estimation of Human Base Kinematics](https://user-images.githubusercontent.com/6506093/204252819-0eb9e7fe-abf5-46db-9fb9-9df48aff5a20.mp4)


<div align="center">
2022 International Conference on Humanoid Robotics (Humanoids 2022)
</div>

<div align="center">
  <a href="#reproducing-the-experiments"><b>Installation</b></a> |
  <a href="https://ieeexplore.ieee.org/abstract/document/10000199"><b>Document</b></a> | 
  <a href="https://youtu.be/svX_QNSaElw"><b>YouTube</b></a>
</div>


## Reproducing the experiments

We provide a containerised virtual environment using Docker and Conda in order to launch the software in an isolated, reproducible manner. The dependencies and related versions used to build the environment can be checked in the `deps` folder. However, one can simply pull the pre-built docker image from the GitHub registry, since a `docker.yaml` in the `.github/workflows` folder dispatches a build workflow for the docker image.

1. Pull the docker image:
    ```bash
    docker pull ghcr.io/ami-iit/human-base-estimation-docker:latest
    ```

2. Launch the container:
   ```bash
   xhost +
   docker run -it --net=host --env="DISPLAY=$DISPLAY"  --volume="/tmp/.X11-unix:/tmp/.X11-unix" ghcr.io/ami-iit/human-base-estimation-docker:latest
   ```

   Please be aware that `xhost +` is not a safe way to expose the X Server running on the host machine to the docker container. But in this scenario,  we are not doing anything undesirable, so it is acceptable.

3. The experiment will start by automatically launching a `yarp server`, loading the dataset  and launching the necessary applications along with the visualizer. The experiment will end and everything will close automatically.


For more details on the installation, implementation, and parameters configuration. please check [KinDynFusion repository](https://github.com/ami-iit/kindynfusion).

**Known issues:** Starting the docker daemon using Docker Desktop does not allow to display the visualizer GUI on the screen.  I had to start the Docker daemon using `dockerd` with root privileges and then run the docker container also with root privileges in order to visualize the experiment.

## Citing this work
```
@INPROCEEDINGS{10000199,
  author={Ramadoss, Prashanth and Rapetti, Lorenzo and Tirupachuri, Yeshasvi and Grieco, Riccardo and Milani, Gianluca and Valli, Enrico and Dafarra, Stefano and Traversaro, Silvio and Pucci, Daniele},
  booktitle={2022 IEEE-RAS 21st International Conference on Humanoid Robots (Humanoids)}, 
  title={Estimation of Human Base Kinematics using Dynamical Inverse Kinematics and Contact-Aided Lie Group Kalman Filter}, 
  year={2022},
  volume={},
  number={},
  pages={364-369},
  doi={10.1109/Humanoids53995.2022.10000199}}
```
## Maintainer

This repository is maintained by:

|                                                              |                                                      |
| :----------------------------------------------------------: | :--------------------------------------------------: |
| [<img src="https://github.com/prashanthr05.png" width="40">](https://github.com/prashanthr05) | [@prashanthr05](https://github.com/prashanthr05) |
