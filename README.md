## SoccerNet Camera Callibration 2023

The repository contains the 1st place solution for SoccerNet Camera Calibration 2023, one of the challenges held at CVPR 2023.

Technical details of the approach are available in [Top-1 solution of SoccerNet Camera Calibration Challenge 2023](https://nikolasent.github.io/deeplearning/computervision/2023/06/20/SoccerNet-Camera-Calibration-2023.html). A brief video presentation of the solution is available on [YouTube](https://www.youtube.com/watch?v=bP72jfyecrw).

The solution was developed by Sportlight Technology team: [Nikolay Falaleev](https://github.com/NikolasEnt) and [Ruilong Chen](https://github.com/ruilongml).


## Quick start guide

The environment is provided as a Docker image, built it with `make build`. Use `make run` commamd to start the container.


## Project structure

* `src` - The project's source directory.
* `notebooks` - Jupyter notebooks.
* `data` - The project's storage for required files.
  * `data/experiments/` - Folder with individual experiments results and artifacts (each experiment has its individual folder in this location).
  * `data/dataset/` - Folder with `challenge`, `test`, `train` and `valid` data from the challenge organizers. Use the official [development kit](https://github.com/SoccerNet/sn-calibration) to get the datasets.
* `baseline` - The code of the baseline, adopted from the official [development kit](https://github.com/SoccerNet/sn-calibration). It is used for data handling and evaluation metrics.

## Keypoints model

The HRNet based model code is in [src/models/hrnet/](src/models/hrnet). The model training is configured by Hydra config file [src/models/hrnet/train_config.yaml](src/models/hrnet/train_config.yaml). Run `python src/models/hrnet/train.py` to train the model in the docker container environment.

### Optimize prediction model hyperparameters

In order to run the hyperparameter search with Optuna:
1. Specify the trained model path in [src/models/hrnet/val_config.yaml](src/models/hrnet/val_config.yaml). Make sure the rest of parameters for `model` and `data_params` section are the same to the values used during the model training.
2. Set initial guess and default values for the camera calibration heuristical algorithm in [src/models/hrnet/val_config.yaml](src/models/hrnet/val_config.yaml) `camera` section. Specify parameters seach space in [src/models/hrnet/optimize_valid.yaml](src/models/hrnet/optimize_valid.yaml) in `hydra.sweeper.params` (see Hydra [docs](https://hydra.cc/docs/plugins/optuna_sweeper/) for details on the sweeper configuratiuon). The provided parameters in the files represent the actual final used values during the course of experiments for the Challenge.
3. Run optimization: `cd src/models/hrnet/` and `python validate.py --config-name optimize_valid --multirun`.

## Useful links

* [https://github.com/SoccerNet/sn-calibration](https://github.com/SoccerNet/sn-calibration) Challenge discription and the baseline.
* [https://www.soccer-net.org/tasks/camera-calibration](https://www.soccer-net.org/tasks/camera-calibration) Challenge homepage.
* [https://arxiv.org/abs/2309.06006](https://arxiv.org/abs/2309.06006) Soccernet 2023 results report.
* [Evaluation server](https://eval.ai/web/challenges/challenge-page/1946/overview)