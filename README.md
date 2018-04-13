# MXBoard Demo

## Visualizing Filters of ConvNets
This section visualizes the filters and outputs of the first convolution layers in three ConvNets:
[Inception-BN](http://data.mxnet.io/models/imagenet/inception-bn/),
[Resnet-152](http://data.mxnet.io/models/imagenet/resnet/152-layers/),
and
[VGG16](http://data.mxnet.io/models/imagenet/vgg/).

All three models are trained on [ImageNet](http://image-net.org/) dataset.
The outputs are generated by applying filters to an image selected from the
[validation dataset](http://data.mxnet.io/data/val_256_q90.rec).
The three pictures in each example are original picture from the validation dataset, the filters of the first
convolution layer, and the outputs of the convolution layer after applying the filters to the original image.
Each filter in the middle picture corresponds to the output image of the same coordinate in the picture on its
right side.
The smooth and nicely-formed patterns in the filters indicate that the models have been well trained.
The gray-scale filters tend to extract the outlines of the object, while the colorful filters
focus on local features of the objects.
Run command
```bash
$ python plot_filter_and_output.py
```
to write filters and outputs to event files for visualization in TensorBoard.

- Inception-BN
![inception_bn_conv_1_weight_output](https://github.com/reminisce/mxboard-demo/blob/master/pic/inception_bn_conv_1_weight_output.png)

- Resnet-152
![resnet_152_conv0_weight_output](https://github.com/reminisce/mxboard-demo/blob/master/pic/resnet_152_conv0_weight_output.png)

- VGG16
![vgg16_conv1_1_weight_output](https://github.com/reminisce/mxboard-demo/blob/master/pic/vgg16_conv1_1_weight_output.png)


## Visualizing ConvNet Codes as Embeddings
Embeddings are the mathematical vector representation of real-world objects in high-dimensional space.
One can quantify the similarity between two objects by calculating the normalized
inner-product of their embedding vectors.
Furthermore, dimension-reduction algorithms, such as
[PCA](https://en.wikipedia.org/wiki/Principal_component_analysis) and
[t-SNE](https://lvdmaaten.github.io/tsne/), can collapse high-dimensional embedding vectors
into 2D or 3D space so that their spatial position can be visualized.

While human beings recognize images by telling the difference of shapes, colors, environments etc. from one to another,
ConvNets classify images via special codes generated by the last fully-connected layer just before the classifier
such as softmax. These codes can be taken as the embeddings of the objects and we can plot them in TensorBoard.
Since t-SNE is known for its excellent capability of preserving local structures, we demonstrate plotting 2,304
objects randomly extracted from the [validation dataset](http://data.mxnet.io/data/val_256_q90.rec)
using t-SNE as followings.
The model used here is [Resnet-152](http://data.mxnet.io/models/imagenet/resnet/152-layers/).
One can use other pre-trained models to achieve the similar results.

![imagenet_resnet_152_embedding](https://github.com/reminisce/mxboard-demo/blob/master/pic/imagenet_resnet_152_embedding.png)

On the top-right corner of the TensorBoard GUI, you can enter the object type to search for the matched
objects in the canvas, and then zoom in to check whether similar objects are clustered. A well trained
ConvNet model should produce the codes that enable t-SNE to place similar objects in the same neighborhood,
and preserve the relative distances of objects in the high-dimensional space. Here we want to search for
images of dogs, and after zooming in, we observed clustering of dog images as follows.
To reproduce the result, type
```bash
$ python get_convnet_codes.py
```
to generate image embeddings, and then type
```bash
$ python plot_convnet_embedding.py
```
to write embeddings to event files for visualization in TensorBoard.

![imagenet_resnet_152_dog_cluster](https://github.com/reminisce/mxboard-demo/blob/master/pic/imagenet_resnet_152_dog_cluster.png)


# References
- https://github.com/awslabs/mxboard
- https://github.com/apache/incubator-mxnet
- [Understanding and Visualizing Convolutional Neural Networks](http://cs231n.github.io/understanding-cnn/)
- [TensorBoard: Embedding Visualization](https://www.tensorflow.org/versions/r1.1/get_started/embedding_viz)