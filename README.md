# rl-with-videos

This repository is the official implementation of the following paper:

**Reinforcement Learning with Videos:Combining Offline Observations with Interaction**<br/>
Karl Schmeckpeper, Oleh Rybkin, Kostas Daniilidis, Sergey Levine, and Chelsea Finn <br/>
[Conference on Robot Learning](https://www.robot-learning.org/) 2020 <br/>
[Website](https://sites.google.com/view/rl-with-videos) | [Video](https://www.youtube.com/watch?v=aIWr4fhzPFA) | [arXiv](https://arxiv.org/abs/2011.06507)



This implementation is based on [softlearning](https://github.com/rail-berkeley/softlearning).


# Installation

We recommend making a new virtual environment to install the dependencies.

```
git clone https://github.com/kschmeckpeper/rl_with_videos.git
cd rl_with_videos

pip install -r requirements.txt
python setup.py develop
```


# Examples
We provide the commands to replicate experiments from the paper.

## Acrobot
We wrap the Acrobot environment in the AcrobotContinuous environment, which takes a continuous action and discretizes it before passing it to the original Acrobot environment.

To run the SAC baseline, run the following commands.
```
cd examples/run_rl
python3 -u main.py --task=AcrobotContinuous-v1 --algorithm SAC --n_epochs 1000 --checkpoint-at-end True --checkpoint-replay-pool True --checkpoint-frequency 25 --exp-name EXP_NAME --gpus=1 --trial-gpus=1 --temp-dir=/PATH/TO/TMP/DIR
```


To run RLV, first download a replay pool containing the desired observations.  You may also use a replay pool generated during the training of SAC.

| Avg. Reward | Link |
| -99 | [here](https://drive.google.com/file/d/16Je5LcjTM_7VJ4oEjxNrAyoXhFdzHrwT/view?usp=sharing) |
| -79 | [here](https://drive.google.com/file/d/16Je5LcjTM_7VJ4oEjxNrAyoXhFdzHrwT/view?usp=sharing) |
| -63 | [here](https://drive.google.com/file/d/15pqxuLvD-PjkWsdZl2FRyekpOhC88LPE/view?usp=sharing) |

Then, run the following commands.

```
cd examples/run_rl
python3 -u main.py --task=AcrobotContinuous-v1 --algorithm RLV --n_epochs 1000 --checkpoint-at-end True --checkpoint-replay-pool True --checkpoint-frequency 100 --exp-name EXP_NAME --replace_rewards_bottom=-1.0 --replace_rewards_scale=10.0 --gpus=1 --trial-gpus=1 --replay_pool_load_path PATH/TO/REPLAY/POOL --temp-dir=PATH/TO/TMP/DIR
```

## Pushing with Human Observations

To run the SAC baseline, run the following command.

```
cd examples/run_rl
python3 -u main.py --task=Image48HumanLikeSawyerPushForwardEnv-v0 --domain mujoco --algorithm SAC --n_epochs 1000 --checkpoint-at-end True --checkpoint-replay-pool True --checkpoint-frequency 100 --exp-name EXP_NAME --gpus=1 --trial-gpus=1  --video-save-frequency=50 --temp-dir=/PATH/TO/TMP/DIR
```

To run RLV, first download the human observations from [here](https://drive.google.com/file/d/1H8p3RsimZjZMhbpf1AP2xEOPWBBQgpIu/view?usp=sharing) and the human paired data from [here](https://drive.google.com/file/d/1qK2EoHMaOPAmACIxLbxI0C34gyH_UiWB/view?usp=sharing).

Run the following commands:

```
cd examples/run_rl
python3 -u main.py --task=Image48HumanLikeSawyerPushForwardEnv-v0 --domain mujoco --algorithm RLV --n_epochs 1000 --exp-name EXP_NAME --gpus=1 --trial-gpus=1 --replay_pool_load_path /home/karls/partially_action_conditioned_rl/softlearning-pac2/scripts/replay_pools/hand_july_21_26_keep_all_fixed.pkl --paired_data_path /home/karls/partially_action_conditioned_rl/softlearning-pac2/scripts/replay_pools/human_paired_oct_10.pkl --paired_loss_scale 1e-06 --preprocessor_for_inverse --shared_preprocessor --remove_rewards=True --replace_rewards_scale=10.0 --replace_rewards_bottom=0.0 --inverse_domain_shift --inv_model_ds_generator_weight 0.001 --inv_model_ds_discriminator_weight 1e-08 --temp-dir=/home/karls/partially_action_conditioned_rl/tmp 
```


## Drawer opening with Human Observations

To run the SAC baseline, run the following command.

```
cd examples/run_rl
python3 -u main.py --task=Image48MetaworldDrawerOpenSparse2D-v0 --domain Metaworld --algorithm SAC --num-samples 1 --n_epochs 1000 --exp-name EXP_NAME --gpus=1 --trial-gpus=1 --video-save-frequency=50 --temp-dir=/PATH/TO/TMP/DIR
```


Download the human observations from [here](https://drive.google.com/file/d/1LhJ5LE8FkiBI9i7KRtv-wFmsN5xvLkzu/view?usp=sharing) and the human paired data from [here](https://drive.google.com/file/d/1z-4XJevn-S2yHf8usk2Cv_NfzlT-pTYQ/view?usp=sharing).

Run the following commands:
```
cd examples/run_rl
python3 -u main.py --task=Image48MetaworldDrawerOpenSparse2D-v0 --domain Metaworld --algorithm PAC  --n_epochs 3000 --exp-name EXP_NAME --gpus=1 --trial-gpus=1 --replay_pool_load_path /home/karls/partially_action_conditioned_rl/softlearning-pac2/scripts/replay_pools/drawer_2d/drawer_kitchen_aug28_nov2.pkl --paired_data_path /home/karls/partially_action_conditioned_rl/softlearning-pac2/scripts/replay_pools/drawer_2d/kitchen_paired_frames.pkl --paired_loss_scale 1e-08 --preprocessor_for_inverse --shared_preprocessor --remove_rewards=True --replace_rewards_scale=10.0 --replace_rewards_bottom=0.0 --inverse_domain_shift --inv_model_ds_generator_weight 0.001 --inv_model_ds_discriminator_weight 1e-08 --video-save-frequency=50 --temp-dir=/home/karls/partially_action_conditioned_rl/tmp
```




## Citation
If this codebase helps you in your academic research, you are encouraged to cite our paper. Here is an example bibtex:
```
@article{schmeckpeper2020rlv,
  title={Reinforcement Learning with Videos: Combining Offline Observations with Interaction},
  author={Schmeckpeper, Karl and Rybkin, Oleh and Daniilidis, Kostas and Levine, Sergey and Finn, Chelsea},
  journal={Conference on Robot Learning},
  year={2020}
}
```


