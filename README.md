# Baby AI Game

[![CircleCI](https://circleci.com/gh/mila-udem/babyai.svg?style=svg&circle-token=ed2191e1bb0206a2f3f2e22f45f1369f7b8115a9)](https://circleci.com/gh/mila-udem/babyai)

Prototype of a game where a reinforcement learning agent is trained through natural language instructions. This is a research project based at [Mila](https://mila.quebec/en/).

## Installation

Requirements:
- Python 3.5+
- OpenAI gym
- NumPy
- PyQT5
- PyTorch 0.4.1+

Start by manually installing PyTorch. See the [PyTorch website](http://pytorch.org/)
for installation instructions specific to your platform.

Then, clone this repository and install the other dependencies with `pip3`:

```
git clone https://github.com/mila-udem/babyai.git
cd babyai
pip3 install --process-dependency-links --editable .
```

### Installation using Conda (Alternative Method)

If you are using conda, you can create a `babyai` environment with all the dependencies by running:

```
conda env create -f environment.yaml
```

Having done that, you can either add `babyai` and `gym-minigrid` in your `$PYTHONPATH` or install them in the development mode as suggested above.

## Structure of the Codebase

In `babyai`:
- The `levels` directory contains all the code relevant to the generation of levels and curriculums. Essentially, this implements the test task for the Baby AI Game. This is an importable module which people can use on its own to perform experiments.
- The `agents` directory contains a default implementation of one or more agents to be evaluated on the baby AI game. This should also be importable as an independent module. Each agent will need to support methods to be provided teaching inputs using pointing and naming, as well as demonstrations.
- The `multienv` directory contains an implementation of the algorithms described in [Matiisen et al., 2017](https://arxiv.org/abs/1707.00183) for automatic execution of curriculums.
- The `utils` directory contains a bunch of useful functions that can be used when training Reinforcement Learning or Imitation Learning agents.
- `model.py` is a script containing the network architectures used when training any type of agent.

In `scripts`:
- `make_human_demos.py` is a helper script to easily make and save human demonstrations that can be helpful for Imitation Learning.
- `train_il.py` is a script used to train an Imitation Learning agent on demonstrations, whether generated by *humans*, or by a Reinforcement Learning agent.
- `train_rl.py` is a script used to train a Reinforcement Learning agent, using the aforementioned `model.py`
- `make_agent_demos.py` takes as input a pre-trained Reinforcement Learning agent (or another type of agent), and generates demonstrations on new instances of the level. These can be used to train an Imitation Learning Agent for example.
- `evaluate.py`, `evaluate_all_demos.py`, and `evaluate_all_models.py` are used to obtain basic statistics on the reward an agent obtains, and the number of steps necessary to complete missions within a level.
- `enjoy.py` helps visualize demonstrations or the behavior of a pre-trained RL agent.

The `gui.py` script implements a template of a user interface for interactive human teaching. The version found in the master branch allows you to control the agent manually with the arrow keys, but it is not currently connected to any model or teaching code. Currently, most experiments are done offline, without a user interface.

## Usage

To run the interactive GUI application:

```
./gui.py
```

The level being run can be selected with the `--env-name` option, eg:

```
./gui.py --env-name BabyAI-UnlockPickup-v0
```

To see the available levels, please read [this](#the-levels).

### Usage at Mila

If you connect to the lab machines by ssh-ing, make sure to use `ssh -X` in order to see the game window. This will work even for a chain of ssh connections, as long as you use `ssh -X` at all intermediate steps. If you use screen, set `$DISPLAY` variable manually inside each of your screen terminals. You can find the right value for `$DISPLAY` by detaching from you screen first (`Ctrl+A+D`) and then running `echo $DISPLAY`.

The code does not work in conda, install everything with `pip install --user`.

### The Levels

Documentation for the ICLR19 levels can be found in
[docs/iclr19_levels.md](docs/iclr19_levels.md).
There are also older levels documented in
[docs/bonus_levels.md](docs/bonus_levels.md).

### Troubleshooting

If you run into error messages relating to OpenAI gym or PyQT, it may be that the version of those libraries that you have installed is incompatible. You can try upgrading specific libraries with pip3, eg: `pip3 install --upgrade gym`. If the problem persists, please [open an issue](https://github.com/maximecb/baby-ai-game/issues) on this repository and paste a *complete* error message, along with some information about your platform (are you running Windows, Mac, Linux? Are you running this on a Mila machine?).

## About this Project

The Baby AI Game is a game in which an agent existing in a simulated world
will be trained to complete task through reinforcement learning as well
as interactions from one or more human teachers. These interactions will take
the form of natural language, and possibly other feedback, such as human
teachers manually giving rewards to the agent, or pointing towards
specific objects in the game using the mouse.

Two of the main goals of the project are to explore ways in which deep learning can take
inspiration from human learning (ie: how human babies learn), and to research AI learning
with humans in the loop. In particular, language learning,
as well as teaching agents to complete actions spanning many (eg: hundreds)
of time steps, or macro-actions composed of multiple micro-actions, are
still open research problems.

Some possible approaches to be explored in this project include meta-learning
and curriculum learning, the use of intrinsic motivation (curiosity), and
the use of pretraining to give agents a small core of built-in knowledge to
allow them to learn from human agents. With respect to built-in knowledge,
Yoshua Bengio believes that the ability for agents to understand pointing
gestures in combination with language may be key.

You can find here a presentation of the project: [Baby AI Summary](https://docs.google.com/document/d/1WXY0HLHizxuZl0GMGY0j3FEqLaK1oX-66v-4PyZIvdU)

A work-in-progress review of related work can be found [here](https://www.overleaf.com/13480997qqsxybgstxhg#/52042269/)

## Instructions for Committers

To contribute to this project, you should first create your own fork, and remember to periodically [sync changes from this repository](https://stackoverflow.com/questions/7244321/how-do-i-update-a-github-forked-repository). You can then create [pull requests](https://yangsu.github.io/pull-request-tutorial/) for modifications you have made. Your changes will be tested and reviewed before they are merged into this repository. If you are not familiar with forks and pull requests, I recommend doing a Google or YouTube search to find many useful tutorials on the issue. Knowing how to use git and GitHub effectively are valuable skills for any programmer.

If you have found a bug, or would like to request a change or improvement
to the grid world environment or user interface, please
[open an issue](https://github.com/maximecb/baby-ai-game/issues)
on this repository. For bug reports, please paste complete error messages and describe your system configuration (are you running on Mac, Linux?).
