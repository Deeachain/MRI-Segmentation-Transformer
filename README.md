# Med Seg DL

Med Seg DL provides a concise framework to perform segmentation on medical images. It establishes all functionality needed to operate on 3D images with a patch-based architecture.

## Functionality

Note: medio may be required to parse your dataset

Currently medseg_dl has been established with the following aspects in mind:

* general structure follows the excellent example of the Stanford course CS230: https://github.com/cs230-stanford/cs230-code-examples/tree/master/tensorflow/vision
* clean separation of input pipeline, overall model and training/evaluation scripts
* separation of training and evaluation;
    * following TFs suggested model, training and evaluation are no longer interleaved. Instead they are two separate processes that can be performed independently of each other. Going further all their wrapper functions (like TFEstimators will work like that)
    * you’ll still be able to evaluate your model during training by running a parallel evaluation process
    * a further benefit is, that the model takes less ram on the GPU and if servers are occupied, evaluation can be suspended
    * separate scripts managing tf.Sessions for both cases (last few ckpts for training and best ckpts for evaluation are saved)
* three input pipelines using the tfrecords data.Dataset api allowing parallel reading, processing, batching and more:
    * Training pipeline with image (mirroring, rotating, scaling, warping (elastic transforms), and patch augmentation (noise), patch cropping, class-balanced sampling
    * Augmentation is implemented as conditional execution based on simple random numbers, so fine-grained adjustments can be made
    * Evaluation pipeline performing automated conversion from images to patches to batches, processing by network and recombination to whole images
    * Evaluation pipeline for metrics passing whole images, labels for comparing recombined predictions to ground truth
    * Pipeline support a flexible amount of input channels
* clean separation between model, loss, metrics as separate files to prevent students from being confused:
    * losses: soft dice and soft jaccard
    * metrics:  general tf.metrics fn’s as well as (hard dice) of mean/single classes
    * layers: a grouping of higher level building blocks for easier model building
    * model_fn: combining losses, metrics and a function building the actual architecture
* parameter object, easing parametrization;
    * generating a model folder keeping your graphs, logs, ckpts, parameters and more
    * alleviating using multiple paths/subpaths
    * access to parameters via dict
    * adjusting and saving your parameters to a yaml file.
    * restoring of training by simply passing the path to this yaml file
* utilities;
    * showing 3D images, labels, predictions via nibabel (during training/evaluation)
    * calculating dsc, assd metrics (more of the old framework could be migrated in the future)
    * Simple overlay plotting
    * Parsing and fetching subject folders containing tfrecords
    * Generating training/test sets; saving filelists as json within model folder
*	implementation of MedPatchNet containing dynamic convolutions learning to process a passed position and efficient spatial pyramid (ESP) blocks providing a lightweight dilated pyramid architecture 
*	setup file so medseg_dl scripts can be executed conveniently (pip3 install -e .)

## Installation

use pip3 (with a venv)

    pip3 install -e .

if it fails consider

    pip3 install -e . --user

or the new pipenv (not recommended atm)

    cd <project_dir>
    pipenv shell
    pipenv install -e .
    
## Usage

For training use

    nohup python3 -u train.py > file_out 2> file_err &
    
For prediction use

    nohup python3 -u evaluate.py > file_out 2> file_err &