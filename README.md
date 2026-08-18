# Go2 Isaac Lab velocity RL

Isaac Lab / RSL-RL velocity-tracking curricula for the Unitree Go2. This is
the reinforcement-learning track, kept separate from the model-based MuJoCo
controller in [go2-mujoco-control](https://github.com/kairoi-k/go2-mujoco-control)
and the Kine2Go imitation work in
[kine2go-research](https://github.com/kairoi-k/kine2go-research).

![Isaac Lab 0.5 m/s](docs/media/rl_0.5ms.gif)
![Isaac Lab 3.5 m/s](docs/media/rl_3.5ms.gif)

## Scope

The package registers Go2 flat-velocity environments on top of Isaac Lab's
official task. The recorded curriculum reaches commanded lin_vel_x ranges
of ±2.0, ±2.5, ±3.0, and ±3.5 m/s; the released policy is a fast,
short-stride velocity-tracking result.

This is not a natural-gait claim, sim-to-real validation, or a replacement for
the 500 Hz C++ WBC/MPC controller. Coarse error_vel_xy values are tracking
errors, not measured body speed.

## Install

Inside the Isaac Lab Python environment:

~~~
git clone https://github.com/kairoi-k/go2-isaaclab-rl.git
cd go2-isaaclab-rl
pip install -e .
export ISAACLAB_PATH=/path/to/IsaacLab
~~~

The package is gym-registered; do not copy files into isaaclab_tasks.

Registered tasks:

| Task | lin_vel_x command range |
|---|---:|
| Isaac-Velocity-Flat-Unitree-Go2-Fast-v0 | ±2.0 m/s |
| Isaac-Velocity-Flat-Unitree-Go2-Fast25-v0 | ±2.5 m/s |
| Isaac-Velocity-Flat-Unitree-Go2-Fast30-v0 | ±3.0 m/s |
| Isaac-Velocity-Flat-Unitree-Go2-Fast35-v0 | ±3.5 m/s |

Each task also has a -Play-v0 variant.

## Play the recorded policy

~~~
python -m go2_velocity_fast.download -o model_54950.pt
python -m go2_velocity_fast.play --task Isaac-Velocity-Flat-Unitree-Go2-Fast35-v0 --num_envs 16 --checkpoint model_54950.pt
~~~

The recorded checkpoint has SHA-256
c2009f890e5b575a8832021ab717dd2dcc23678a64f423d2f4e793d861ed4b42. It is
published in this repository's [v0.1.0 release](https://github.com/kairoi-k/go2-isaaclab-rl/releases/tag/v0.1.0).

## Train

~~~
python -m go2_velocity_fast.train --task Isaac-Velocity-Flat-Unitree-Go2-Fast35-v0 --headless
~~~

The exact recorded environment is in [ENV_SNAPSHOT.md](ENV_SNAPSHOT.md):
Isaac Sim 6.0.1, Isaac Lab v3.0.0-beta2, rsl_rl 5.4.2, and the Newton /
MuJoCo Warp backend. The local RSL-RL NaN guards used for the recorded
checkpoint are not vendored here.

## Repository boundary

This repository owns only the Isaac Lab velocity-RL extension and its
environment record. Model-based Go2 control is maintained separately in
[go2-mujoco-control](https://github.com/kairoi-k/go2-mujoco-control); motion
imitation, AMP, and seam records are maintained in
[kine2go-research](https://github.com/kairoi-k/kine2go-research).

## License

BSD 3-Clause; see [LICENSE](LICENSE). Upstream and third-party components
retain their own licenses and attribution requirements.
