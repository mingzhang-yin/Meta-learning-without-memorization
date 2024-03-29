# Meta-Learning without Memorization
* Implemention of meta-regularizers as described in [Meta-Learning without Memorization](https://openreview.net/pdf?id=BklEFpEYwS) by Mingzhang Yin, George Tucker, Mingyuan Zhou, Sergey Levine, Chelsea Finn.
* Forked from [Google Research Repository](https://github.com/google-research/google-research/tree/master/meta_learning_without_memorization) (2020.01.01), maintained at [Google Research Repository](https://github.com/google-research/google-research/tree/master/meta_learning_without_memorization).
* **Data**: data that are used in experiments can be found at this [link](https://drive.google.com/drive/folders/1iv_MWeLOKQPew5jMyYZpIS8z_ivhf8sg?usp=sharing)

In this paper, we address the memorization problem by designing a meta-regularization objective using information theory that places precedence on data-driven adaptation. This causes the meta-learner to decide what must be learned from the task training data and what should be inferred from the task testing input. By doing so, our algorithm can successfully use data from *non-mutually-exclusive* tasks to efficiently adapt to novel tasks. We demonstrate its applicability to both contextual and gradient-based meta-learning algorithms, and apply it in practical settings where applying standard meta-learning has been difficult. Our approach substantially outperforms standard meta-learning algorithms in these settings.

Below is the paper to cite if you find the algorithms in this repository useful in your own research:
```
@inproceedings{yin2020metalearning,
  title={Meta-Learning without Memorization},
  author={Mingzhang Yin and George Tucker and Mingyuan Zhou and Sergey Levine and Chelsea Finn},
  booktitle={International Conference on Learning Representations},
  year={2020},
}
```

This repository:
* Provides code to generate the pose regression dataset used in the paper.
* Implements Model Agnostic Meta Learning (MAML; Finn et al. 2017) and Neural Processes (NP; Garnelo et al. 2018) with meta-regularization on the weights and activations.
We hope that this code will be a useful starting point for future research in this area.

## Generating pose regression dataset
Requirements:
* TensorFlow (see tensorflow.org for how to install)
* numpy-stl
* gym
* mujoco-py


Step 1: Download CAD models from [Beyond PASCAL: A Benchmark for 3D Object Detection in the Wild](http://cvgl.stanford.edu/projects/pascal3d.html) [ftp://cs.stanford.edu/cs/cvgl/PASCAL3D+_release1.1.zip](ftp://cs.stanford.edu/cs/cvgl/PASCAL3D+_release1.1.zip) and use the CAD folder.

We removed the two classes 'bottle' and 'train' because the objects are symmetric.

Step 2: Convert the CAD models from \*.off to \*.stl.

You can download a converter from [https://www.patrickmin.com/meshconv](https://www.patrickmin.com/meshconv). Then, run

```
chmod 755 meshconv
find ./CAD -maxdepth 2 -mindepth 2 -name "*.off" -exec meshconv -c stl {} \;
```

Step 3: Render the dataset. Using the utilities in pose_data
```
CAD_DIR=
DATA_DIR=
python mujoco_render.py --CAD_dir=${CAD_DIR} --data_dir=${DATA_DIR}
cp -r ${DATA_DIR}/rotate ${DATA_DIR}/rotate_resize
python resize_images.py --data_dir=${DATA_DIR}/rotate_resize
python data_gen.py --data_dir=${DATA_DIR}/rotate_resize
```
This generates two pickle files: train_data.pkl and val_data.pkl.

## Train models on pose regression dataset

```
pip install tensorflow==1.14
pip install tensorflow-probability==0.7
```

See pose_code/run.sh for examples of training the various algorithms.

This is not an officially supported Google product. It is maintained by George Tucker (gjt@google.com, [@georgejtucker](https://twitter.com/georgejtucker), github user: gjtucker).
