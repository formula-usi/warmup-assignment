# Formula USI 2021 Warm up Assignment

Assignment in preparation for the FORMULA USI 2021 Challenge, 5--7 November 2021, Campus Est, Lugano, Switzerland. This task is preparatory to what the Challenge would require you to do, thus do it at the best of your possibilities.

## Testbed

At FORMULA USI 2021 you will build a real small-scale Donkey Car driving on a real track. For this assignment, we require you to build a virtual Donkey Car driving on a virtual road in a simulator. Below is the virtual racing track.

*Virtual Environment (Unity)*

<img src="images/sim1.png" height="200" width="200" />
<img src="images/sim_env.png" height="200" width="200" />
<img src="images/sim2.png" height="200" width="200" />
<img src="images/sim.gif" height="200" width="200" />

### Task

Build a deep neural network model that performs lane-keeping and that is able to drive our little training track (see picture below). The vehicle should be able to drive up to **10 laps** in autonomous mode with the least number of failures (i.e., crashes or out of track episodes).

## Requisites
Python 3.7, git 64 bit, miniconda 3.7 64 bit.

**Software setup:** We encourage using the PyCharm IDE by JetBrains. We used PyCharm  Professional 2020.3 and Python 3.7.

**Hardware setup:** Training the DNN models (self-driving cars) is computationally expensive on large datasets. Therefore, we recommend using a machine with a GPU. However, given the limited size of the track, you should be able to complete the task successfully also on more resources-constrained hardware.

## Donkey Car Installation

We use Donkey Car v3.1.5. Two Python packages are needed to run this repository (1) *donkeycar* and (2) *gym-donkeycar*, which need to be installed on your machine.


To install the *donkeycar* package, perform the following commands

```
* git clone https://github.com/autorope/donkeycar.git
* git checkout a91f88d
* conda env remove -n donkey
* conda env create -f install/envs/mac.yml
* conda activate donkey
* pip install -e .\[pc\] (ZSH version)
* pip install -e .[pc] (Bash version)
```

To install the *gym-donkeycar* package, perform the following commands

```
* git clone https://github.com/tawnkramer/gym-donkeycar
* cd gym-donkeycar
* conda activate donkey
* pip install -e .\[gym-donkeycar\] (ZSH version)
* pip install -e .[gym-donkeycar] (Bash version)
```

To create the a new Donkey Car project, perform the following commands

```
* conda activate donkey
* donkey createcar --path mycar/
```

In case of issues, you can also refer to the original [documentation](http://docs.donkeycar.com/guide/install_software/). 

**Important:** Make sure your code run with Tensorflow 1.13.1. No Tensorflow2 models will be accepted.

## Donkey Car Simulator

You need to donwload and use [our simulator](https://drive.google.com/drive/folders/1iZP2LKnRvib6T6yFSuGrhwtPUpeL9pXC?usp=sharing) (macOS/Windows/Linux).

During autonomous (but also manual) driving, the simulator automatically records the driving behaviour and stores it into CSV files in a `Testing` directory (on macOS it is accessbile by right clicking the `donkey-sim-mac` file --> `Show Package Contents` --> `Contents`). The generated files are two:

* `Simulation-<timestamp>.csv` records the telemetry for an entire simulation
* `Laps - <timestamp>.csv` records the aggregates on a per lap basisDuring autonomous (but also manual) driving, the simulator automatically records the driving behaviour and stores it into CSV files in a `Testing` directory (on macOS it is accessbile by right clicking the `donkey-sim-mac` file --> `Show Package Contents` --> `Contents`). The generated files are two:

* `Simulation-<timestamp>.csv` records the telemetry for an entire simulation
* `Laps - <timestamp>.csv` records the aggregates on a per lap basis

## Building the Self-Driving Car

### Data Collection

1. Create a directory where you want to save your training data to
2. Open the simulator
3. Click `Log dir`
4. Select the newly created folder
5. Select the scene `Sandbox Track`
6. Select the button `Joystick/Keyboard w Rec`
7. Drive manually trying to stay as much as possible at the center of track. For a decent model, 10 to 30 laps may be sufficient.

### Training

Train a [default model](https://docs.donkeycar.com/parts/keras/) using the collected data. You can do it using the following command

```
python train.py --model <modelname>.h5 --tub <data> --type <modeltype> --aug
```

Eventually, you can modify the trainig hyperparameter in the `myconfig.py` file.

### Testing

Set the following configurations in the `myconfig.py` file

1. `DONKEY_GYM = True`
2. `DONKEY_SIM_PATH = "path to the simulator"`
3. `DONKEY_GYM_ENV_NAME = "donkey-warehouse-v0"`

For testing the behaviour of your lane keeping model, enable autonomus driving using the following command

```
python manage.py drive --model [models/<model-name.h5>]
```
Go to: http://localhost:8887/drive
Select “Local Pilot (d)”



### Submission Format & Details

Submit a `zip` file named `<name-surname>-formula-usi-warmup-assignment.zip` to [formulausi@usi.ch](mailto:formulausi@usi.ch). The file must contain

1. the model in H5 format compatible with Tensorflow 1.13.1
2. your `Simulation-<timestamp>.csv` 
* your `Laps - <timestamp>.csv` 
* a text file with team information and everything you believe it is necessary to report us


### Evaluation

We will run your model on the simulator locally, and compare the results obtained with those we receive from you. The models will be evaluated automatically based on the lap time, avg CTE and failure scores, all to be minimized. This means that the car should drive on road, following the central trajectory as fast as possible with no failures.


### Contacts

For issues and miscellaneous, contact [Andrea Stocco](mailto:andrea.stocco@usi.ch) and CC [Brian Pulfer](mailto:brian.pulfer@usi.ch).



