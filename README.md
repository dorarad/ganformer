# Mirror of [github.com/dorarad/gansformer](https://github.com/dorarad/gansformer)
![Python 3.7](https://img.shields.io/badge/python-3.7-b0071e.svg?style=plastic)
![PyTorch 1.8](https://img.shields.io/badge/pytorch-1.8-%239e008e.svg?style=plastic)
![TensorFlow 1.14](https://img.shields.io/badge/tensorflow-1.14-blueviolet.svg?style=plastic)
![cuDNN 7.3.1](https://img.shields.io/badge/cuda-10.0-2545e6.svg?style=plastic)
![License CC BY-NC](https://img.shields.io/badge/license-MIT-108a00.svg?style=plastic)


# GANformer: Generative Adversarial Transformers
<p align="center">
  <b><a href="https://cs.stanford.edu/~dorarad/">Drew A. Hudson</a> & <a href="http://larryzitnick.org/">C. Lawrence Zitnick</a></b>
</p>

## Check out our new [PyTorch](pytorch_version) version and the [GANformer2 paper](https://arxiv.org/abs/2111.08960)!

<div align="center">
  <img src="https://cs.stanford.edu/people/dorarad/image1.png" style="float:left" width="340px">
  <img src="https://cs.stanford.edu/people/dorarad/image3.png" style="float:right" width="440px">
</div>
<p></p>

***Update (Feb 21, 2022):*** *We updated the weight initialization of the PyTorch version to the intended scale, leading to a substantial improvement in the model's learning speed!*

This is an implementation of the [GANformer](https://arxiv.org/pdf/2103.01209.pdf) model, a novel and efficient type of transformer, explored for the task of image generation. The network employs a _bipartite structure_ that enables long-range interactions across the image, while maintaining computation of linearly efficiency, that can readily scale to high-resolution synthesis. 
The model iteratively propagates information from a set of latent variables to the evolving visual features and vice versa, to support the refinement of each in light of the other and encourage the emergence of compositional representations of objects and scenes. 
In contrast to the classic transformer architecture, it utilizes multiplicative integration that allows flexible region-based modulation, and can thus be seen as a generalization of the successful StyleGAN network.

<img align="right" src="https://cs.stanford.edu/people/dorarad/img3.png" width="270px">

**1st Paper**: [https://arxiv.org/pdf/2103.01209](https://arxiv.org/pdf/2103.01209)  
**2nd Paper**: [https://arxiv.org/abs/2111.08960](https://arxiv.org/abs/2111.08960)  
**Contact**: dorarad@cs.stanford.edu  
**Implementation**: [`network.py`](training/network.py) ([TF](network.py) / [Pytorch](pytorch_version/training/networks.py))

### We now support both [`PyTorch`](pytorch_version) and TF!
:white_check_mark: Uploading initial code and readme  
:white_check_mark: Image sampling and visualization script  
:white_check_mark: Code clean-up and refacotiring, adding documentation  
:white_check_mark: Training and data-prepreation intructions  
:white_check_mark: Pretrained networks for all datasets  
:white_check_mark: Extra visualizations and evaluations  
:white_check_mark: Providing models trained for longer  
:white_check_mark: Releasing the PyTorch version  
:white_check_mark: Releasing pre-trained models for high-resolutions (up to 1024 x 1024)  
⬜️ Releasing the GANformer2 model (supporting layout generation and conditional layout2image generation) (coming soon!)

If you experience any issues or have suggestions for improvements or extensions, feel free to contact me either thourgh the issues page or at dorarad@stanford.edu. 

## Bibtex
```bibtex
@article{hudson2021ganformer,
  title={Generative Adversarial Transformers},
  author={Hudson, Drew A and Zitnick, C. Lawrence},
  journal={Proceedings of the 38th International Conference on Machine Learning, {ICML} 2021},
  year={2021}
}

@article{hudson2021ganformer2,
  title={Compositional Transformers for Scene Generation},
  author={Hudson, Drew A and Zitnick, C. Lawrence},
  journal={Advances in Neural Information Processing Systems {NeurIPS} 2021},
  year={2021}
}
```

## Sample Images
Using the pre-trained models (generated after training for ***5-7x*** less steps than StyleGAN2 models! Training our models for longer will improve the image quality further):
<div align="center">
  <img src="https://cs.stanford.edu/people/dorarad/samples.png" width="700px">
</div>

## Requirements
<img align="right" src="https://cs.stanford.edu/people/dorarad/dia.png" width="190px">

- Python 3.6 or 3.7 are supported.
- For the TF version: We recommend TensorFlow 1.14 which was used for development, but TensorFlow 1.15 is also supported.
- For the Pytorch version: We support Pytorch >= 1.8.
- The code was tested with CUDA 10.0 toolkit and cuDNN 7.5.
- We have performed experiments on Titan V GPU. We assume 12GB of GPU memory (more memory can expedite training).
- See [`requirements.txt`](requirements.txt) ([TF](requirements.py) / [Pytorch](pytorch_version/requirements.py)) for the required python packages and run `pip install -r requirements.txt` to install them.

## Quickstart & Overview
Our repository supports both **Tensorflow** (at the main directory) and **Pytorch** (at [`pytorch_version`](pytorch_version)). The two implementations follow a close code and files structure, and share the same interface. To switch from the TF to Pytorch, simply enter into [`pytorch_version`](pytorch_version)), and install the [requirements](pytorch_version/requirements.txt).
Please feel free to open an issue or [contact](dorarad@cs.stanford.edu) for any questions or suggestions about the new implementation!

A minimal example of using a pre-trained GANformer can be found at [`generate.py`](generate.py) ([TF](generate.py) / [Pytorch](pytorch_version/generate.py)). When executed, the 10-lines program downloads a pre-trained modle and uses it to generate some images:
```python
python generate.py --gpus 0 --model gdrive:bedrooms-snapshot.pkl --output-dir images --images-num 32
```
**You can use `--truncation-psi` to control the generated images quality/diversity trade-off.  
We recommend trying out different values in the range of `0.6-1.0`.**

### Pretrained models and High resolutions
We provide pretrained models for resolution 256&times;256 for all datasets, as well as 1024&times;1024 for FFHQ and 1024&times;2048 for Cityscapes.

To generate images for the high-resolution models, run the following commands:
(We reduce their batch-size to 1 so that they can load onto a single GPU)

```python
python generate.py --gpus 0 --model gdrive:ffhq-snapshot-1024.pkl --output-dir ffhq_images --images-num 32 --batch-size 1
python generate.py --gpus 0 --model gdrive:cityscapes-snapshot-2048.pkl --output-dir cityscapes_images --images-num 32 --batch-size 1 --ratio 0.5 # 1024 x 2048 cityscapes currently supported in the TF version only
```

We can train and evaluate new or pretrained model both quantitatively and qualitative with [`run_network.py`](run_network.py) ([TF](run_network.py) / [Pytorch](pytorch_version/run_network.py)).  
The model architecutre can be found at [`network.py`](training/network.py) ([TF](training/network.py) / [Pytorch](pytorch_version/training/network.py)). The training procedure is implemented at [`training_loop.py`](training/training_loop.py) ([TF](training/training_loop.py) / [Pytorch](pytorch_version/training/training_loop.py)).

## Data preparation
We explored the GANformer model on 4 datasets for images and scenes: [CLEVR](https://cs.stanford.edu/people/jcjohns/clevr/), [LSUN-Bedrooms](https://www.yf.io/p/lsun), [Cityscapes](https://www.cityscapes-dataset.com/) and [FFHQ](https://github.com/NVlabs/ffhq-dataset). The model can be trained on other datasets as well.
We trained the model on `256x256` resolution. Higher resolutions are supported too. The model will automatically adapt to the resolution of the images in the dataset.

The [`prepare_data.py`](prepare_data.py) ([TF](prepare_data.py) / [Pytorch](pytorch_version/prepare_data.py)) can either prepare the datasets from our catalog or create new datasets.

### Default Datasets 
To prepare the datasets from the catalog, run the following command:
```python
python prepare_data.py --ffhq --cityscapes --clevr --bedrooms --max-images 100000
```

See table below for details about the datasets in the catalog.

**Useful options**:  
* `--data-dir` the output data directory (default: `datasets`)  
* `--shards-num` to select the number of shards for the data (default: adapted to each dataset)  
* `--max-images` to store only a subset of the dataset, in order to reduce the size of the stored `tfrecord`/image files (default: _max_).  
This can be particularly useful to save space in case of large datasets, such as LSUN-bedrooms (originaly contains 3M images)

### Custom Datasets
You can also use the script to create new custom datasets. For instance:
```python
python prepare_data.py --task <dataset-name> --images-dir <source-dir> --format png --ratio 0.7 --shards-num 5
```
The script supports several formats: `png`, `jpg`, `npy`, `hdf5`, `tfds` and `lmdb`.

### Dataset Catalog
| Dataset           | # Images  | Resolution    | Download Size | TFrecords Size   | Gamma | 
| :---------------: | :-------: | :-----------: | :-----------: | :--------------: | :---: |
| **FFHQ**          | 70,000    | 256&times;256 | 13GB          | 13GB             | 10    |
| **CLEVR**         | 100,015   | 256&times;256 | 18GB          | 15.5GB           | 40    |
| **Cityscapes**    | 24,998    | 256&times;256 | 1.8GB         | 8GB              | 20    |
| **LSUN-Bedrooms** | 3,033,042 | 256&times;256 | 42.8GB        | Up to 480GB      | 100   |

Use `--max-images` to reduce the size of the `tfrecord` files.

## Training
Models are trained by using the `--train` option. To fine-tune a pretrained GANformer model:
```python
python run_network.py --train --gpus 0 --ganformer-default --expname clevr-pretrained --dataset clevr \
  --pretrained-pkl gdrive:clevr-snapshot.pkl
```
We provide pretrained models for `bedrooms`, `cityscapes`, `clevr` and `ffhq`.

To train a GANformer in its default configuration form scratch:
```python
python run_network.py --train --gpus 0 --ganformer-default --expname clevr-scratch --dataset clevr --eval-images-num 10000
```

By defualt, models training is resumed from the latest snapshot. Use `--restart` to strat a new experiment, or `--pretrained-pkl` to select a particular snapshot to load.

For comparing to state-of-the-art, we compute metric scores using 50,000 sample imaegs. To expedite training though, we recommend setting `--eval-images-num` to a lower number. Note though that this can impact the precision of the metrics, so we recommend using a lower value during training, and increasing it back up in the final evaluation.

We support a large variety of command-line options to adjust the model, training, and evaluation. Run `python run_network.py -h` for the full list of options!

we recommend exploring different values for `--gamma` when training on new datasets. If you train on resolution >= 512 and observe OOM issues, consider reducing `--batch-gpu` to a lower value.

### Logging
* During training, sample images and attention maps will be generated and stored at `results/<expname>-<run-id>` (`--keep-samples`).
* Metrics will also be regularly commputed and reported in a `metric-<name>.txt` file. `--metrics` can be set to `fid` for FID, `is` for Inception Score and `pr` for Precision/Recall.
* Tensorboard logs are also created (`--summarize`) that track the metrics, loss values for the generator and discriminator, and other useful statistics over the course of training.

### Baseline models
The codebase suppors multiple baselines in addition to the GANformer. For instance, to run a vanilla GAN model:
```python
python run_network.py --train --gpus 0 --baseline GAN --expname clevr-gan --dataset clevr 
```
* **[Vanilla GAN](https://arxiv.org/abs/1406.2661)**: `--baseline GAN`, a standard GAN without style modulation.
* **[StyleGAN2](https://arxiv.org/abs/1912.04958)**: `--baseline StyleGAN2`, with one global latent that modulates the image features.
* **[k-GAN](https://arxiv.org/abs/1810.10340)**: `--baseline kGAN`, which generates multiple image layers independetly and then merge them into one shared image (supported only in the TF version).
* **[SAGAN]()**: `--baseline SAGAN`, which performs self-attention between all image features in low-resolution layer (e.g. `32x32`) (supported only in the TF version).

## Evaluation
To evalute a model, use the `--eval` option:
```python
python run_network.py --eval --gpus 0 --expname clevr-exp --dataset clevr
```
Add `--pretrained-pkl gdrive:<dataset>-snapshot.pkl` to evalute a pretrained model.

Below we provide the FID-50k scores for the GANformer (_using the pretrained checkpoints above_) as well as baseline models.  
Note that these scores are different than the scores reported in the StyleGAN2 paper since they run experiments for up to 7x more training steps (5k-15k kimg-steps in our experiments over all models, which takes about 3-4 days with 4 GPUs, vs 50-70k kimg-steps in their experiments, which take over 90 GPU-days).

**Note regarding Generator/Discriminator**: Following ablation experiments, we observed that incorporating the simplex and duplex attention to the generator (rather than to both the generator and discriminator) improve the models' performance. Accordingly, we are releasing pretrained models that incorporate attention to the generator only. We will update the paper's manuscript to reflect these findings!

| Model          | CLEVR        | LSUN-Bedroom | FFHQ       | Cityscapes |
| :------------: | :----------: | :----------: | :--------: | :--------: |
| **GAN**        | 25.02        | 12.16        | 13.18      | 11.57      |
| **kGAN**       | 28.28        | 69.9         | 61.14      | 51.08      |
| **SAGAN**      | 26.04        | 14.06        | 16.21      | 12.81      |
| **StyleGAN2**  | 16.05        | 11.53        | 16.21      | 8.35       |
| **GANformer** | ***9.24***   | ***6.15***   | ***7.42*** | ***5.23*** |

<div>
  <img src="https://cs.stanford.edu/people/dorarad/plot1.png" width="350px">
  <img src="https://cs.stanford.edu/people/dorarad/plot2.png" width="350px">
</div>

### Model Change-log
Compared to the original GANformer depicted in the paper, this repository make several additional improvments that contributed to the performance:
* Use `--mapping_ltnt2ltnt` so that the latents communicate with each other directly through self-attention inside the mapping network before starting to generate the image.
* Add an additional global latent (`--style`) to the `k` latent components, such that first the global latent modulates all the image features uniformly, and then the `k` latents modulate different regions based on the bipartite transformer's attention.  
The global latent is useful for coordinating holistic aspects of the image such as global lighting conditions, global style properties for e.g. faces, etc.
* After making these changes, we observed no additional benefit from adding the transformer to the discriminator, and therefore for simplicity we disabled that.

## Visualization
The code supports producing qualitative results and visualizations. For instance, to create attention maps for each layer:
```python
python run_network.py --gpus 0 --vis --expname clevr-exp --dataset clevr --vis-layer-maps
```

Below you can see sample images and attention maps produced by the GANformer:

<div align="center">
  <img src="https://cs.stanford.edu/people/dorarad/atts.png" style="float:left" width="831px">
</div>

## Command-line Options
In the following we list some of the most useful model options. 

### Training
* `--gamma`: We recommend exploring different values for the chosen dataset (default: `10`)
* `--truncation-psi`: Controls the image quality/diversity trade-off. (default: `0.7`)
* `--eval-images-num`: Number of images to compute metrics over. We recommend selecting a lower number to expedite training (default: `50,000`)
* `--restart`: To restart training from sracth instead of resuming from the latest snapshot
* `--pretrained-pkl`: To load a pretrained model, either a local one or from drive `gdrive:<dataset>-snapshot.pkl` for the datasets in the catalog.
* `--data-dir` and `--result-dir`: Directory names for the datasets (`tfrecords`) and logging/results.

### Model (most useful)
* `--transformer`: To add transformer layers to the generator (GANformer)
* `--components-num`: Number of latent components, which will attend to the image. We recommend values in the range of `8-16` (default: `1`)
* `--latent-size`: Overall latent size (default: `512`). The size of each latent component will then be `latent_size/components_num`
* `--num-heads`: Number of attention heads (default: `1`)
* `--integration`: Integration of information in the transformer layer, e.g. `add` or `mul` (default: `mul`)

### Model (others)
* `--g-start-res` and `--g-end-res`: Start and end resolution for the transformer layers (default: all layers up to resolution 2<sup>8</sup>) 
* `--kmeans`: Track and update image-to-latents assignment centroids, used in the duplex attention
* `--mapping-ltnt2ltnt`: Perform self-attention over latents in the mapping network
* `--use-pos`: Use trainable positional encodings for the latents.
* `--style False`: To turn-off one-vector global style modulation (StyleGAN2).

### Visualization
* **Sample imaegs**
  * `--vis-images`: Generate image samples 
  * `--vis-latents`: Save source latent vectors
* **Attention maps**
  * `--vis-maps`: Visualize attention maps of last layer and first head
  * `--vis-layer-maps`: Visualize attention maps of all layer and heads
  * `--blending-alpha`: Alpha weight when visualizing a bledning of images and attention maps
* **Image interpolations**
  * `--vis-interpolations`: Generative interplations between pairs of source latents
  * `--interpolation-density`: Number of samples in between two end points of an interpolation (default: `8`)
* **Others**
  * `--vis-noise-var`: Create noise variation visualization
  * `--vis-style-mix`: Create style mixing visualization

Run `python run_network.py -h` for the full options list.

## Sample images (more examples)
<div align="center">
  <img src="https://cs.stanford.edu/people/dorarad/faces.png" style="float:left" width="750px">
  <br>
  <img src="https://cs.stanford.edu/people/dorarad/bedroom.png" style="float:left" width="750px">
  <br>
  <img src="https://cs.stanford.edu/people/dorarad/clevr_new.png" style="float:left" width="750px">
  <br>
  <img src="https://cs.stanford.edu/people/dorarad/cities_small.png" style="float:left" width="750px">
</div>

## CUDA / Installation
The model relies on custom TensorFlow/Pytorch ops that are compiled on the fly using [NVCC](https://docs.nvidia.com/cuda/cuda-compiler-driver-nvcc/index.html). 

To set up the environment e.g. for cuda-10.0:
```python
export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

To test that your NVCC installation is working correctly, run:
```python
nvcc test_nvcc.cu -o test_nvcc -run
| CPU says hello.
| GPU says hello.
```

In the pytorch version, if you get the following repeating message:  
"Failed to build CUDA kernels for upfirdn2d. Falling back to slow reference implementation"  
make sure your cuda and pytorch versions match. If you have multiple CUDA installed, consider
using setting ``CUDA_HOME`` to the matching one. E.g. 
```python
export CUDA_HOME=/usr/local/cuda-10.1
```

## Architecture Overview
The GANformer consists of two networks:

**Generator**: which produces the images (`x`) given randomly sampled latents (`z`). The latent z has a shape `[batch_size, component_num, latent_dim]`, where `component_num = 1` by default (Vanilla GAN, StyleGAN) but is > 1 for the GANformer model. We can define the latent components by splitting `z` along the second dimension to obtain `z_1,...,z_k` latent components. The generator likewise consists of two parts:
* **Mapping network**: converts sampled latents from a normal distribution (`z`) to the intermediate space (`w`). A series of Feed-forward layers. The k latent components either are mapped independently from the `z` space to the `w` space or interact with each other through self-attention (optional flag).
* **Synthesis network**: the intermediate latents w are used to guide the generation of new images. Images features begin from a small constant/sampled grid of `4x4`, and then go through multiple layers of convolution and up-sampling until reaching the desirable resolution (e.g. `256x256`). After each convolution, the image features are modulated (meaning that their variance and bias are controlled) by the intermediate latent vectors `w`. While in the StyleGAN model there is one global w vectors that controls all the features equally. The GANformer uses attention so that the k latent components specialize to control different regions in the image to create it cooperatively, and therefore perform better especially in generating images depicting multi-object scenes.
* **Attention** can be used in several ways
  * **Simplex Attention**: when attention is applied in one direction only from the latents to the image features (**top-down**).
  * **Duplex Attention**: when attention is applied in the two directions: latents to image features (**top-down**) and then image features back to latents (**bottom-up**), so that each representation informs the other iteratively.
  * **Self Attention between latents**: can also be used so to each direct interactions between the latents.
  * **Self Attention between image features** (SAGAN model): prior approaches used attention directly between the image features, but this method does not scale well due to the quadratic number of features which becomes very high for high-resolutions.
     
**Discriminator**: Receives and image and has to predict whether it is real or fake – originating from the dataset or the generator. The model perform multiple layers of convolution and downsampling on the image, reducing the representation's resolution gradually until making final prediction. Optionally, attention can be incorporated into the discriminator as well where it has multiple (k) aggregator variables, that use attention to adaptively collect information from the image while being processed. We observe small improvements in model performance when attention is used in the discriminator, although note that most of the gain in using attention based on our observations arises from the generator.

## Codebase
This codebase builds on top of and extends the great [StyleGAN2](https://github.com/NVlabs/stylegan2) and [StyleGAN2-ADA](https://github.com/NVlabs/stylegan2-ada-pytorch/) repositories by Karras et al.  

The GANformer model can also be seen as a generalization of StyleGAN: while StyleGAN has one global latent vector that control the style of all image features globally, the GANformer has *k* latent vectors, that cooperate through attention to control regions within the image, and thereby better modeling images of multi-object and compositional scenes.

## Acknowledgement
_I wish to thank Christopher D. Manning for the fruitful discussions and constructive feedback in developing the Bipartite Transformer, especially when explored within the language representation area, as well as for providing the kind financial support that allowed this work to happen!_ :sunflower:

If you have questions, comments or feedback, please feel free to contact me at dorarad@cs.stanford.edu, Thank you! :)
