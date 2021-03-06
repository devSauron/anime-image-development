# anime-image-development

#### Introduction 
In this project we are trying to create a model using Generative Adversarial Networks which would be able to help in the image development of new anime images based on the training it receives. Our project is broadly classified into 3 parts – 
* Image Creation – creation of 32x32 sized images out of random noise.
* Super Resolution – enhancing the resolution of the image by 4 times to get the picture in 128x128 format.
#### Dataset
The dataset was found on Kaggle. It consists of 14,310 images of 173 distinct anime characters. The images are centered and have a corresponding entry in a csv file containing the hair and skin classification value. 12,000 images are used for training and the rest for testing.
#### Data Preprocessing
All the images from each character folder are read into the system. The images are resized as per the requirements and stored into an array, ready to be fed to the network. A similar exercise is done for classification along with reading the csv files and storing them into an array.
#### Methodology and Analysis
Since, our project incorporates 2 distinct neural networks, each one of them is a class in its own – 
* Image Creation using DCGAN
  * Deep Convolutional GAN (DCGAN) is a sequential model built up entirely of convolution layers.
  * The model is designed to accept and generate 32x32 sized colored images.
  * In order to create a cross-tabbing free image which has less pixelization, we use De-convolutional layers instead
  * The generator is made up of kernel size 5, De-convolutional layers (aka Conv2D Transpose layers) with regular Batch Normalization of momentum 0.5, PReLU and Dropout value of 0.2. Adam optimizer is used with a learning rate of 0.00001 and a decay of 0.5. Binary Cross-entropy is used for calculating the loss.
  * The discriminator on the other hand is made of convolution layers with stride 2 (padded), PReLU and Dropout value of 0.2. Optimizer and loss function along with the hyper-parameters are same as the generator.
  * Combined model is trained with a batch size 32 and 75,000 steps.
* Super Resolution using SRGAN
  * Super Resolution GAN (SRGAN) is a neural model built using Model API approach with convolutional layers.
  * The model is designed to accept 32x32 sized colored images and generate the enhanced version of size 128x128.
  * Residual blocks (similar to the ResNet model) are used to create a deep network and prevent the vanishing gradient problem.
  * In our Generator, we have used 16 residual blocks along with Batch Normalization for faster convergence. We use PReLU for activation and two up-sampling layers as well. The optimizer used is Adam (learning rate = 0.00009 and decay = 0.5) along with the loss function as MSE.
  * For Discriminator, it is a Convolutional network with last two fully connected dense layers. We use LeakyReLu(α=0.2) for activation along with Batch Normalization (momentum = 0.5) for faster convergence, Adam (learning rate = 0.00009 and decay = 0.5) as optimizer and Binary Cross-entropy as the loss function.
  * Model trained with batch size 8 and 1,17,000 steps.
#### Results – 
##### Image Creation using DCGAN
These models can be used in generating images on their own. If the same model is trained on other datasets like faces, then it has the capability to generate different faces as well. Also, since these images are being generated by the Generator which never sees an original image, therefore we can assume that the images created by it are unique.

###### Discriminator
![model details](https://github.com/Team7-Code/anime-image-development/blob/dev/Image%20Generator/sample_outputs/dis%20-%201.png)
![model details](https://github.com/Team7-Code/anime-image-development/blob/dev/Image%20Generator/sample_outputs/dis%20-%202.png)

###### Generator
![model details](https://github.com/Team7-Code/anime-image-development/blob/dev/Image%20Generator/sample_outputs/gen1.png)
![model details](https://github.com/Team7-Code/anime-image-development/blob/dev/Image%20Generator/sample_outputs/gen2.png)

###### Images Obtained
![model details](https://github.com/Team7-Code/anime-image-development/blob/dev/Image%20Generator/sample_outputs/img1.png)
![model details](https://github.com/Team7-Code/anime-image-development/blob/dev/Image%20Generator/sample_outputs/img2.png)


##### Super Resolution using SRGAN
The process of estimating a high resolution image using a low resolution image is known as Super resolution. For our project we up-sample a 32x32 picture by four times. The PSNR (Peak Signal to Noise Ratio) metric is used for evaluation for which we managed to obtain values ranging between 16-19 dB.

###### Results
![model_output](https://github.com/Team7-Code/anime-image-development/blob/dev/Super%20Resolution/Sample%20Outputs/img1.png)

#### Execution and Scalability
* Image Generator – It took close to four days to train the model on the NVIDIA GTX 1050 GPU to obtain the images shown above. The code is capable of being scaled up to include huge datasets and has the functionality to visualize gradients, losses and callbacks to Tensor-board for further visualizations
* Super Resolution –  It took close to five days to train the model on the NVIDIA GTX 1050 Ti GPU. This model can be scaled to up sample images of any dimension by four times if trained upon them. The upscaling times can also be parametrized. Although, the above mentioned points are heavily limited by the available infrastructure. 

#### Limitations
* Image Generator – The model is yet not able to create images equivalent to the ones it was trained upon, so further training is required.
* Super Resolution – The model is only able to accept images of dimension 32x32 and scale them up to four times of the original resolution. Also, since the models fails to recognize some features like spectacles, it needs to be trained more on a larger dataset with a variety of images.

