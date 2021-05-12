# Deep learning with self-supervision and uncertainty regularization to count fish in underwater images

A pre-print version of this paper is available <a href="https://arxiv.org/abs/2104.14964">here</a>.

### Authors

Penny Tarling, Mauricio Cantor, Albert Clapés and Sergio Escalera

### Overview

Effective conservation actions require effective population monitoring. However, accurately counting animals in the wild to inform conservation decision-making is difficult. Monitoring populations through image sampling has made data collection cheaper, wide-reaching and less intrusive but created a need to process and analyse this data efficiently. Counting animals from such data is challenging, particularly when densely packed in noisy images. Attempting this manually is slow and expensive, while traditional computer vision methods are limited in their generalisability. Deep learning is the state-of-the-art method for many computer vision tasks, but it has yet to be properly explored to count animals. To this end, we employ deep learning, with a density-based regression approach, to count fish in low-resolution sonar images. We introduce a large dataset of sonar videos, deployed to record wild mullet schools <i>(Mugil liza)</i>, with a subset of 500 labelled images. We utilise abundant unlabelled data in a self-supervised task to improve the supervised counting task. For the first time in this context, by introducing uncertainty quantification, we improve model training and provide an accompanying measure of prediction uncertainty for more informed biological decision-making. Finally, we demonstrate the generalisability of our proposed counting framework through testing it on a recent benchmark dataset of high-resolution annotated underwater images from varying habitats (DeepFish). From experiments on both contrasting datasets, we demonstrate our network outperforms the few other deep learning models implemented for solving this task. By providing an open-source framework along with training data, our study puts forth an efficient deep learning template for crowd counting aquatic animals thereby contributing effective methods to assess natural populations from the ever-increasing visual data.  




<p align="center"><img src="https://github.com/ptarling/DeepLearningFishCounting/blob/main/Figures/FIGURE2.png" align=middle width=645.87435pt height=348.58725pt/>
</p>
<p align="left">
Figure 1: A multi-task deep convolutional network with aleatoric uncertainty regularization to count (dense schools of) fish in underwater images: one branch learns the supervised task of regressing an input image to an estimated fish count via a density mapping and simultaneously, parallel Siamese branches learn a self-supervised task of ranking pairs of images according to the number of fish - leveraging unlabelled data. (Inspired by an <a href="https://arxiv.org/pdf/1803.03095.pdf">algorithm</a> proved for counting people in crowds - these pairs have been generated by taking an original image P<sub>1</sub>, and comparing it to a cropped sample of this image P<sub>2</sub>. We added an additional layer to the output so our model learns to predict the noise variance within each labelled sample - producing a measure of "uncertainty" alongside each fish count estimate. The backbone of each branch is a <a href="https://arxiv.org/pdf/1512.03385.pdf">ResNet-50</a> followed by a 1x1 2D Convolutional layer. A non-learnable Global Average Pooling layer is added to each branch of the Siamese network so the resulting scaler count of image P<sub>1</sub> can be subtracted from image P<sub>2</sub>. The model is trained end-to-end and all parameters are shared (represented by the orange arrows) thus the self-supervised task can boost the performance of the core supervised task.
</p>

<p align="center"><img src="https://github.com/ptarling/DeepLearningFishCounting/blob/main/Figures/FIGURE1.png" align=middle width=645.87435pt height=348.58725pt/>
</p>
<p align="left">
Figure 2: Novel dataset: 105 hours of sonar video footage were recorded at the estuarine canal in Laguna, southern Brazil. Here a rare cooporative foraging system has been established between wild dolphins and artisanal net-casting fishers to catch migrating mullet fish. A subset of this data has been labelled with the aim of training deep learning models to count the number of mullet fish in the captured footage. This subset contains 500 images with point annotations to mark the locations of mullet, bounding boxes to mark non-mullet noisy objects and an image bounding box to mark the geographical sample area. (a) Raw frame depicting dolphins and a large mullet school. (b) Contrast enhancement and background removal. (c) Manual annotation of individual mullets (point), dolphins - noise (bounding box) and geographical sample area (bounding box).
</p>


### Requirements

  ```sh
  Python >= 3.6
  Tensorflow >= 2.0 
  Numpy
  Matplotlib
  PIL
  Random
  ```
### Data 

Labelled and unlabelled data can be downloaded <a href="https://zenodo.org/record/4717411">here</a>.

* Labelled data: 
  * 500 original images: 350 train, 70 validation, 80 test

* Unlablled data: 
  * 126 video files which equates to > 100k image frames


### Weights
Our optimal pre-trained model weights can be downloaded from <a href="https://zenodo.org/record/4717411">here</a>.

### Scripts

* <a href="https://github.com/ptarling/DeepLearningFishCounting/blob/main/Unlabelled_data_augmentation/unlabelled_data_augmentation.py">Unlabelled_data_augmentation/unlabelled_data_augmentation.py</a>: Run this script to generate new pairs of images from unlabelled data (for training the self-supervised task) - see unlablled_data_augmentation directory for detailed instructions.
* <a href="https://github.com/ptarling/DeepLearningFishCounting/blob/main/model.py">model.py</a>: The architecture of our multi-task network with aleatoric uncertainty regularization
* <a href="https://github.com/ptarling/DeepLearningFishCounting/blob/main/lossfunctions_metrics.py">lossfunctions_metrics.py</a>: L<sub>1</sub> absolute loss adapted for aleatoric uncertainty regularization for training supervised counting task and pairwise ranking hinge loss used for training self-supervised ranking task.
* <a href="https://github.com/ptarling/DeepLearningFishCounting/blob/main/train.py">train.py</a>: Run this script to train and validate the model 
* <a href="https://github.com/ptarling/DeepLearningFishCounting/blob/main/test.py">test.py</a>: Run this script to produce results of trained weights on test set

### Variables
* <a href="https://github.com/ptarling/DeepLearningFishCounting/blob/main/train.py">train.py</a>:
 1.  <em>Load data</em>: script default loads the original training set. Change to train on different data.
 2.  multitask_au(input_shape = (576,320,3)): default = (576,320,3). Change to match input image data size.
 3.  model.load_weights(): uncomment to upload our pre-trained optimal weights
 4.  batch_size: script default = 10
 5.  epochs: script default = 200
* <a href="https://github.com/ptarling/DeepLearningFishCounting/blob/main/test.py">test.py</a>: 
 1.  multitask_au(input_shape = ): default = (576,320,3). Change to match input image data size. 
 2.  model.load_weights(): script default loads our pre-trained optimal weights. Change to test different weights. 

### Citation

Please cite our paper if you use our code or ideas in your research:

  ```sh
@misc{tarling2021deep,
      title={Deep learning with self-supervision and uncertainty regularization to count fish in underwater images}, 
      author={Penny Tarling and Mauricio Cantor and Albert Clapés and Sergio Escalera},
      year={2021},
      eprint={2104.14964},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
  ```
