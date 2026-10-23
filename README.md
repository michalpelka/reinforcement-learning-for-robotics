# Reinforcement Learning for Robotics

This repository holds the development environment and demos used in the Reinforcement Learning for Robotics video series, which can be found [here](https://www.youtube.com/watch?v=zsdceSTRBl4&list=PLYExBrZNJeQg&index=1).

> If you are looking for the workshop/webinar version of this demo, please use this repository: [github.com/ShawnHymel/workshop-reinforcement-learning-for-robotics](https://github.com/ShawnHymel/workshop-reinforcement-learning-for-robotics/)

<a href="https://www.youtube.com/watch?v=zsdceSTRBl4&list=PLYExBrZNJeQg&index=1">
  <img src=".images/rl-for-robotics-thumbnail-play.png" alt="Reinforcement Learning for Robotics" height="500">
</a>

## Installation

Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/). Make sure it is running before continuing to the next step.

Open a terminal and build the Docker image:

```sh
docker build -t rl-robotics -f Dockerfile.cpu .
```

Run the image:

```sh
docker run -it --rm -p 3000:3000 -p 6006:6006 -v "${PWD}/workspace:/workspace" --shm-size=2g rl-robotics
```

Notes:
 * Port 3000 is for the WebTop interface
 * Port 6006 is for TensorBoard
 * VS Code is memory hungry, so we bump the shared memory up to 2 GB

Browse to [http://localhost:3000/](http://localhost:3000/) to interact with WebTop.

## Running locally with uv (no Docker)

Install [uv](https://docs.astral.sh/uv/getting-started/installation/), then from the repository root create the environment. uv fetches Python 3.12 if needed and installs the same library versions as `Dockerfile.cpu`:

```sh
uv sync
```

Register the `Python (RL)` kernel that the notebooks expect (it is installed inside `.venv`, not system-wide):

```sh
uv run python -m ipykernel install --sys-prefix --name rl-env --display-name "Python (RL)"
```

Start JupyterLab:

```sh
uv run jupyter lab workspace
```

Start TensorBoard in a second terminal:

```sh
uv run tensorboard --logdir workspace/software --reload_interval 5
```

Then browse to [http://localhost:6006/](http://localhost:6006/).

Notes:
 * Notebooks use paths relative to their own folder, so open them from JupyterLab (or VS Code with the `.venv` interpreter) rather than running the cells from another directory
 * PyTorch is the CPU-only build on Linux x86_64, the same as in the container
 * TensorFlow is not installed locally because nothing imports it. TensorBoard prints "running with reduced feature set", which is fine for these notebooks
 * The MuJoCo viewer windows open directly on your desktop. On macOS, `mujoco.viewer.launch_passive` only works under `mjpython`, so the interactive viewer cells will not run from Jupyter there

## License

All software in this repository, unless otherwise noted, is licensed under the [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0) license.
