## 1. Image denoising
In `Python` and especially `cv2`,
- `cv.fastNlMeansDenoising()` - works with a single grayscale images
- `cv.fastNlMeansDenoisingColored()` - works with a color image.
- `cv.fastNlMeansDenoisingMulti()` - works with image sequence captured in short period of time (grayscale images)
- `cv.fastNlMeansDenoisingColoredMulti()` - same as above, but for color images.
#### Parameters
- `h` : parameter deciding filter strength. Higher h value removes noise better, but removes details of image also. (10 is ok)
- `hForColorComponents` : same as h, but for color images only. (normally same as h)
- `templateWindowSize` : should be odd. (recommended 7)
- `searchWindowSize` : should be odd. (recommended 21)

[Reference](https://docs.opencv.org/4.x/d5/d69/tutorial_py_non_local_means.html)

## 2. Image augmentation
Included
- Flipping and Cropping
- Changing Colors
- Combining Multiple Image Augmentation Methods

[Reference](https://d2l.ai/chapter_computer-vision/image-augmentation.html)

## 3. AutoEncoder / Downsampling
### Definition
- The initial convolutional and pooling layers of a CNN progressively reduce the `spatial dimensions (width and height` of the input and intermediate feature maps, a process known as `encoding` or **downsampling**. 
- This produces a compressed representation of the input image that can be efficiently processed, allowing the network to extract key structural features and relationships within the input. 
- The general architecture of an autoencoder includes an encoder, decoder, and bottleneck layer.
### Notes
- Encoder
>- Input layer take raw input data
>- The hidden layers progressively reduce the dimensionality of the input, capturing important features and patterns. These layer compose the encoder.
>- The bottleneck layer (latent space) is the final hidden layer, where the dimensionality is significantly reduced. This layer represents the compressed encoding of the input data.
- Decoder
>- The bottleneck layer takes the encoded representation and expands it back to the dimensionality of the original input.
>- The hidden layers progressively increase the dimensionality and aim to reconstruct the original input.
>- The output layer produces the reconstructed output, which ideally should be as close as possible to the input data.
- The loss function used during training is typically a reconstruction loss, measuring the difference between the input and the reconstructed output. Common choices include mean squared error (MSE) for continuous data or binary cross-entropy for binary data.
- During training, the autoencoder learns to minimize the reconstruction loss, forcing the network to capture the most important features of the input data in the bottleneck layer.

![image](https://github.com/NhanDoV/Lectures_notes-teaching-in-VN-/assets/60571509/79c13591-0e92-4f4f-bf3f-e47c5ff78e1b)

### Types
#### Denoising Autoencoder
Denoising autoencoder works on a partially corrupted input and trains to recover the original undistorted image. As mentioned above, this method is an effective way to constrain the network from simply copying the input and thus learn the underlying structure and important features of the data.
- Advantages
>- This type of autoencoder can extract important features and reduce the noise or the useless features.
>- Denoising autoencoders can be used as a form of data augmentation, the restored images can be used as augmented data thus generating additional training samples. 
- Disadvantages
>- Selecting the right type and level of noise to introduce can be challenging and may require domain knowledge.
>- Denoising process can result into loss of some information that is needed from the original input. This loss can impact accuracy of the output. 

#### Sparse Autoencoder
This type of autoencoder typically contains more hidden units than the input but only a few are allowed to be active at once. This property is called the sparsity of the network. The sparsity of the network can be controlled by either manually zeroing the required hidden units, tuning the activation functions or by adding a loss term to the cost function.
- Advantages
>- The sparsity constraint in sparse autoencoders helps in filtering out noise and irrelevant features during the encoding process.
>- These autoencoders often learn important and meaningful features due to their emphasis on sparse activations.
- Disadvantages
>- The choice of hyperparameters play a significant role in the performance of this autoencoder. Different inputs should result in the activation of different nodes of the network.
>- The application of sparsity constraint increases computational complexity.

#### Variational Autoencoder
Variational autoencoder makes strong assumptions about the distribution of latent variables and uses the Stochastic Gradient Variational Bayes estimator in the training process. It assumes that the data is generated by a Directed Graphical Model and tries to learn an approximation to $q_{\phi}(z|x)$ to the conditional property $q_{\theta}(z|x)$ where $\phi$ and $\theta$ are the parameters of the encoder and the decoder respectively.
- Advantages
>- Variational Autoencoders are used to generate new data points that resemble the original training data. These samples are learned from the latent space.
>- Variational Autoencoder is probabilistic framework that is used to learn a compressed representation of the data that captures its underlying structure and variations, so it is useful in detecting anomalies and data exploration.
- Disadvantages
>- Variational Autoencoder use approximations to estimate the true distribution of the latent variables. This approximation introduces some level of error, which can affect the quality of generated samples.
>- The generated samples may only cover a limited subset of the true data distribution. This can result in a lack of diversity in generated samples.

#### Convolutional Autoencoder
Convolutional autoencoders are a type of autoencoder that use convolutional neural networks (CNNs) as their building blocks. The encoder consists of multiple layers that take a image or a grid as input and pass it through different convolution layers thus forming a compressed representation of the input. The decoder is the mirror image of the encoder it deconvolves the compressed representation and tries to reconstruct the original image.
- Advantages
>- Convolutional autoencoder can compress high-dimensional image data into a lower-dimensional data. This improves storage efficiency and transmission of image data.
>- Convolutional autoencoder can reconstruct missing parts of an image. It can also handle images with slight variations in object position or orientation.
- Disadvantages
>- These autoencoder are prone to overfitting. Proper regularization techniques should be used to tackle this issue.
>- Compression of data can cause data loss which can result in reconstruction of a lower quality image.

![image](https://github.com/NhanDoV/Lectures_notes-teaching-in-VN-/assets/60571509/c012d6e4-83b3-4acc-af20-001b4dcce83f)

## 4. Upsampling / Decoder

### Definition
- For many image processing applications (such as `segmentation`, `co-registration`, `artifact removal`, and `image enhancement`) it may be desirable to reverse the process, gradually restoring the spatial dimensions of the feature maps while reducing the number of channels or features. This process is known as **upsampling** (also called **decoding**, **unpooling**, or **upscaling**).
- `Sequential downsampling and upsampling` is the basis of **encoder-decoder networks**, one of the more commonly used image-processing AI architectures. Because of their shape, such configurations are also called U-networks.

![image](https://github.com/NhanDoV/Lectures_notes-teaching-in-VN-/assets/60571509/d8caa859-7fd9-4f16-bfb2-01645a97a244)

#### Interpolation Methods for Upsampling
- Nearest Neighbor: This method involves duplicating the existing pixels or samples to create new ones. It is the simplest and fastest method but may result in a blocky or pixelated appearance.
Bed of NailsTechnique: Unlike Nearest Neighbor, the Bed-of-Nails only inputs one of the input elements in each region, setting the rest to zero.
- Max Unpooling: This is similar to the Bed-of-Nails but places elements in the larger matrix at locations from where they originally occurred. (This requires memory of their original locations).
- Bilinear Interpolation: This method computes the value of new pixels by taking a weighted average of the nearest four pixels. It results in a smoother image than nearest neighbor interpolation but may introduce artifacts such as blurring and aliasing.
- Bicubic Interpolation: This method uses a more complex interpolation algorithm that takes into account 16 neighboring pixels to compute the value of new pixels. It results in a higher-quality image than bilinear interpolation but requires more computational resources.
- Lanczos Interpolation: This method uses a weighted average of a larger number of neighboring pixels to compute the value of new pixels. It produces sharp and detailed images but requires a larger kernel size and more computational resources.​

![image](https://github.com/NhanDoV/Lectures_notes-teaching-in-VN-/assets/60571509/73bb5719-9af2-4eb1-a966-8a2971f6ec4a)


## 5. Object detection
- Using HSV technique for simpliest case: [reference]()
- In general, we have
>- CNN (Convolutional Neural network)
>- RCNN (Region-Based Convolutional Neural)
>- Fast RCNN
>- Faster RCNN

![image](https://github.com/NhanDoV/Lectures_notes-teaching-in-VN-/assets/60571509/d06f3a0d-c677-405a-a6c3-3c0a294a4093)

[Reference](https://www.analyticsvidhya.com/blog/2018/10/a-step-by-step-introduction-to-the-basic-object-detection-algorithms-part-1/)