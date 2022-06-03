# HALLO killer whale detector




## Prerequisites

You will need a Python environment with [ketos](https://docs.meridian.cs.dal.ca/ketos/) v2.6.1 
or newer and [ketos-scripts](https://gitlab.meridian.cs.dal.ca/public_projects/ketos_scripts).


## Installation

Ketos may be installed with `pip install ketos` whereas ketos-scripts must be 
obtained from [this GitLab repository](https://gitlab.meridian.cs.dal.ca/public_projects/ketos_scripts) 
and compiled from source with 
```
python setup sdist
pip install dist/ketos-scripts-0.0.1.tar.gz 
```


## Configuration files

[db.yaml](db.yaml) contains the specifications for creating a 
database file with sound samples for training and testing a deep learning 
model at detecting killer-whale vocalisations.

[spec.json](spec.json) specifies the form in which the sound 
samples will be stored. In the present case, the sound samples are 
transformed to spectrograms of 5 second duration.

[train.yaml](train.yaml) contains the specifications for training 
the deep learning model.

Finally, [resnet_recipe.json](resnet_recipe.json) specifies the architecture 
of the neural network, in this case a fairly standard ResNet.


## Usage

To create the database file, run the following command from within the `hallo-models` 
directory,
```
kt-db --config db.yaml --output_path db.h5
```
This will create a database file called `db.h5` in the same directory.

Next, to train the deep learning model, run the command
```
kt-train --config train.yaml
```
When the training is complete, the model will automatically saved to a 
directory called `output`.


## References

Preliminary results from this work have been presented at the 
[181st Meeting of the Acoustical Society of America](https://asa.scitation.org/doi/abs/10.1121/10.0008312), 
held in December 2021 in Seattle, USA.
