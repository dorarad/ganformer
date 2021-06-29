# Mirror of [github.com/dorarad/gansformer](https://github.com/dorarad/gansformer)

[![PWC](https://img.shields.io/endpoint.svg?style=plastic&url=https://paperswithcode.com/badge/generative-adversarial-transformers/image-generation-on-clevr)](https://paperswithcode.com/sota/image-generation-on-clevr?p=generative-adversarial-transformers)
[![PWC](https://img.shields.io/endpoint.svg?style=plastic&url=https://paperswithcode.com/badge/generative-adversarial-transformers/image-generation-on-cityscapes)](https://paperswithcode.com/sota/image-generation-on-cityscapes?p=generative-adversarial-transformers)
[![PWC](https://img.shields.io/endpoint.svg?style=plastic&url=https://paperswithcode.com/badge/generative-adversarial-transformers/image-generation-on-lsun-bedroom-256-x-256)](https://paperswithcode.com/sota/image-generation-on-lsun-bedroom-256-x-256?p=generative-adversarial-transformers)

![Python 3.7](https://img.shields.io/badge/python-3.7-blueviolet.svg?style=plastic)
![TensorFlow 1.10](https://img.shields.io/badge/tensorflow-1.14-2545e6.svg?style=plastic)
![cuDNN 7.3.1](https://img.shields.io/badge/cudnn-10.0-b0071e.svg?style=plastic)
![License CC BY-NC](https://img.shields.io/badge/license-MIT-05b502.svg?style=plastic)

# GANformer: Generative Adversarial Transformers
<p align="center">
  <b><a href="https://cs.stanford.edu/~dorarad/">Drew A. Hudson</a>* & <a href="http://larryzitnick.org/">C. Lawrence Zitnick</a></b></span>
</p>

*_I wish to thank [Christopher D. Manning](https://nlp.stanford.edu/~manning/) for the fruitful discussions and constructive feedback in developing the Bipartite Transformer, especially when explored within the language representation area and also in the visual context, as well as for providing the kind financial support that allowed this work to happen!_ :sunflower:

<div align="center">
  <img src="https://cs.stanford.edu/people/dorarad/image1.png" style="float:left" width="340px">
  <img src="https://cs.stanford.edu/people/dorarad/image3.png" style="float:right" width="440px">
</div>
<p></p>

This is an implementation of the [GANformer](https://arxiv.org/pdf/2103.01209.pdf) model, a novel and efficient type of transformer, explored for the task of image generation. The network employs a _bipartite structure_ that enables long-range interactions across the image, while maintaining computation of linearly efficiency, that can readily scale to high-resolution synthesis. 
The model iteratively propagates information from a set of latent variables to the evolving visual features and vice versa, to support the refinement of each in light of the other and encourage the emergence of compositional representations of objects and scenes. 
In contrast to the classic transformer architecture, it utilizes multiplicative integration that allows flexible region-based modulation, and can thus be seen as a generalization of the successful StyleGAN network.

<img align="right" src="https://cs.stanford.edu/people/dorarad/img3.png" width="270px">

**Paper**: [https://arxiv.org/pdf/2103.01209](https://arxiv.org/pdf/2103.01209)  
**Contact**: dorarad@stanford.edu  
**Implementation**: [`network.py`](https://github.com/dorarad/gansformer/tree/main/training/network.py)

### Update: All [code](https://github.com/dorarad/gansformer)! is now ready! 

:white_check_mark: Uploading initial code and readme  
:white_check_mark: Image sampling and visualization script  
:white_check_mark: Code clean-up and refacotiring, adding documentation  
:white_check_mark: Training and data-prepreation intructions  
:white_check_mark: Pretrained networks for all datasets  
:white_check_mark: Extra visualizations and evaluations <!--Extra visualizations/animations and evaluation-->

If you experience any issues or have suggestions for improvements or extensions, feel free to contact me either thourgh the issues page or at dorarad@stanford.edu. 

## Bibtex
```bibtex
@article{hudson2021gansformer,
  title={Generative Adversarial Transformers},
  author={Hudson, Drew A and Zitnick, C. Lawrence},
  journal={arXiv preprint:2103.01209},
  year={2021}
}
```

## Sample Images
Using the pre-trained models (generated after training for ***5-7x*** less steps than StyleGAN2 models! Training our models for longer will improve the image quality further):
<div align="center">
  <img src="https://cs.stanford.edu/people/dorarad/samples.png" width="700px">
</div>

## Code 
Follow the [link](https://github.com/dorarad/gansformer) for the codebase. If you have questions, comments or feedback, please feel free to contact me at dorarad@stanford.edu, Thank you! :)
