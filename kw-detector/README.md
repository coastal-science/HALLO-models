# HALLO killer whale detector

Below are instructions for building a deep-learning model to detect killer-whale calls, following 
the approach described [here]([here](https://docs.google.com/presentation/d/1tWNy7S9j0jy5c0QOkN5mHTWEaUloKU8XcVNlq5qmuFU/edit?usp=sharing).


## Prerequisites

You will need a Python environment with [ketos v2.6.1](https://docs.meridian.cs.dal.ca/ketos/) 
(or newer) and [ketos-scripts](https://gitlab.meridian.cs.dal.ca/public_projects/ketos_scripts).


## Installation

Ketos may be installed with `pip install ketos` whereas ketos-scripts must be 
obtained from [this GitLab repository](https://gitlab.meridian.cs.dal.ca/public_projects/ketos_scripts) 
and compiled from source with 
```
python setup sdist
pip install dist/ketos-scripts-0.0.1.tar.gz 
```


## Configuration files

* [db.yaml](db.yaml): contains the specifications for creating a 
database file with sound samples for training and testing a deep learning 
model at detecting killer-whale vocalisations. 

 * [spec.json](spec.json): specifies the form in which the sound 
samples will be stored. In the present case, the sound samples are 
transformed to spectrograms of 5 second duration. 

 * [train.yaml](train.yaml): contains the specifications for training 
the deep learning model. 

 * [resnet_recipe.json](resnet_recipe.json): specifies the architecture 
of the neural network, in this case a fairly standard ResNet.


## Usage

While the audio data required to reproduce steps 1 and 2 below have yet to be 
made publicly available (we are working on it), the trained models have been included 
in this repository so you can try out step 3 while you wait for us to release the data.


### 1. Prepare the data
To create the database file, run the following command from within `HALLO-models/kw-detector/`,
```
kt-db --config db.yaml --output_path db.h5
```
This will create a database file called `db.h5` in the same directory.

### 2. Train the model
Next, to train the deep learning model, run the command
```
kt-train --config train.yaml
```
When the training is complete, the model will automatically be saved to `output/kw-det_000/kw-det_000-00.kt`.

### 3. Apply the trained model
Finally, to run the model on a set of audio files,
```
kt-run output/kw-det_000/kw-det_000-00.kt <path>
```
where `<path>` is the path to the folder containing the audio files. 
Killer whale calls detected by the model are saved to a csv file 
in RavenPro compatible format.
Use `kt-run -h` to see the full list of command-line arguments available 
for configuring the detector at run time. 

If you want to try out the trained model in PAMGuard, you first have to convert it 
to slightly different format. This is easily accomplished with a few lines of Python code:
```python
from ketos_scripts.utils.nn_utils import import_nn_interface, which_nn_interface
input_path = "output/kw-det_000/kw-det_000-00.kt"
new_path = "kw-det_000-00.ktpb"
name, module_path = which_nn_interface(input_path)
nn_interface = import_nn_interface(name=name, module_path=module_path)
model, audio_repr = nn_interface.load(input_path, load_audio_repr=True)
model.save(new_path, audio_repr=audio_repr)
```


## Trained models

The trained model can be downloaded in two formats:

 * [hallo-kw-det-v1.kt](hallo-kw-det-v1.kt)
 * [hallo-kw-det-v1.ktpb](hallo-kw-det-v1.ktpb) (PAMGuard compatible)

This model was trained on approximately 29,000 killer-whale calls extracted from 
underwater recordings obtained at Roberts Bank and Boundary Pass in the Salish Sea. 
More details can be found 
[here](https://docs.google.com/presentation/d/1tWNy7S9j0jy5c0QOkN5mHTWEaUloKU8XcVNlq5qmuFU/edit?usp=sharing).


## Citation

This work was presented at the [181st Meeting of the Acoustical Society of America](https://asa.scitation.org/doi/abs/10.1121/10.0008312), held in December 2021 in Seattle, USA.





