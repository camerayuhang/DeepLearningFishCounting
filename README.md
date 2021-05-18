# Deep learning with self-supervision and uncertainty regularization to count fish in underwater images

A pre-print version of this paper is available <a href="https://arxiv.org/abs/2104.14964">here</a>.

### Authors

Penny Tarling, Mauricio Cantor, Albert Clapés and Sergio Escalera

### Overview

Effective conservation actions require effective population monitoring. However, accurately counting animals in the wild to inform conservation decision-making is difficult. Monitoring populations through image sampling has made data collection cheaper, wide-reaching and less intrusive but created a need to process and analyse this data efficiently. Counting animals from such data is challenging, particularly when densely packed in noisy images. Attempting this manually is slow and expensive, while traditional computer vision methods are limited in their generalisability. Deep learning is the state-of-the-art method for many computer vision tasks, but it has yet to be properly explored to count animals. To this end, we employ deep learning, with a density-based regression approach, to count fish in low-resolution sonar images. We introduce a large dataset of sonar videos, deployed to record wild mullet schools <i>(Mugil liza)</i>, with a subset of 500 labelled images. We utilise abundant unlabelled data in a self-supervised task to improve the supervised counting task. For the first time in this context, by introducing uncertainty quantification, we improve model training and provide an accompanying measure of prediction uncertainty for more informed biological decision-making. Finally, we demonstrate the generalisability of our proposed counting framework through testing it on a recent benchmark dataset of high-resolution annotated underwater images from varying habitats (<a href="https://alzayats.github.io/DeepFish/">DeepFish</a>). From experiments on both contrasting datasets, we demonstrate our network outperforms the few other deep learning models implemented for solving this task. By providing an open-source framework along with training data, our study puts forth an efficient deep learning template for crowd counting aquatic animals thereby contributing effective methods to assess natural populations from the ever-increasing visual data.  




<p align="center"><img src="https://github.com/ptarling/DeepLearningFishCounting/blob/main/Figures/FIGURE1.png" align=middle width = 80% height = 80%/>
</p>
<p align="left">
Figure 1: The multi-task network is trained end-to-end to simultaneously regress labelled images to corresponding density maps and rank unlabelled pairs of images in order of fish abundance. (Inspired by an algorithm proved effective when counting people in crowds (<a href="https://arxiv.org/pdf/1803.03095.pdf"><i>Liu et al.</i></a>), these pairs have been generated by taking an original image <i>I</i>, and comparing it to a cropped sample of this image <i>I'</i>). The backbone of each branch is a <a href="https://arxiv.org/pdf/1512.03385.pdf">ResNet-50</a> followed by a 1 x 1 convolutional layer with 2 output filters. A non-learnable Global Average Pooling layer is added to each branch of the Siamese network so the resulting scaler count of first image in the pair <i>(I,I')</i> can be subtracted from the second image. All parameters are shared (represented by the orange dashed arrows) thus incorporating the self-supervised task adds no parameters to the base model. The inclusion of an additional channel in our output tensor to estimate noise variance only adds a further ~2x parameters in the head, equivalent to 0.01% of the total number (inspired by the work of <a href="https://arxiv.org/abs/1703.04977"><i>Kendall et al.</i></a> and <a href="https://arxiv.org/abs/1903.07427"><i>Oh et al.</i></a>). <i>K</i> is the batch size, where a batch contains <i>K</i> images from the labelled subset of data, and <i>K</i> pairs of images from the larger unlabelled pool of data. <i>H</i> and <i>W</i> are the height and width of some input 3-channel RGB image, whereas <i>H'</i> and <i>W'</i> are the height and width of the output tensors from the backbone and heads.
</p>

<p align="center"><img src="https://github.com/ptarling/DeepLearningFishCounting/blob/main/Figures/FIGURE2.png" align=middle width = 55% height = 55% />
</p>
<p align="left">
Figure 2: Image pre-processing for assessing mullet abundance from sonar images. (a) Raw frame depicting dolphins and a large mullet school. (b) Contrast enhancement and background removal. (c) Manual labelling of a sample: the large bounding box marks where the raw image was cropped so all input samples represent a consistent size geographical area and at a consistent distance from the sonar camera. The smaller bounding boxes mark where noise (here, a dolphin) is present. Each point annotation marks the location of an individual mullet. (d-f) Examples of variation in the sonar images in our dataset, to which the density-based deep learning model needs to be adaptable to. (d) Frame with high mullet abundance: large number of fish, swimming compactly; (e) low abundance: small number of fish, sparsely distributed; (f) noise: 3 dolphins and a fishing net (note the overhead perspective of a rounded casting net).
</p>


### Requirements

  ```plaintext
  Python >= 3.6
  Tensorflow >= 2.0 
  ```
### Data 

Labelled and unlabelled data can be downloaded <a href="https://zenodo.org/record/4717411">here</a>.

* Labelled data: 
  * 500 original images: 350 train, 70 validation, 80 test

* Unlablled data: 
  * 126 video files (named YY-MM-DD_HHMMSS) which equates to > 100k image frames


### Weights
Our optimal pre-trained model weights can be downloaded from <a href="https://zenodo.org/record/4717411">here</a>.

### Scripts

* <a href="https://github.com/ptarling/DeepLearningFishCounting/blob/main/Unlabelled_data_preprocessing/unlabelled_data_preprocessing.py">Unlabelled_data_preprocessing/unlabelled_data_preprocessing.py</a>: Run this script to generate new pairs of images from unlabelled data (for training the self-supervised task) - see unlablled_data_preprocessing directory for detailed instructions.
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
