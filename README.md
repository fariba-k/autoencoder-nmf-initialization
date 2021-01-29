# autoencoder-nmf-initialization
This is a project about using non-negative matrix factorization for weight initialization in autoencoders. 


# Background
Autoencoders are a specific form of neural networks build by two parts. First part is used to encode the input data to a space with lower dimension and the second part decodes the abstracted data to reconstruct the input.

The networks is trained in a way that the reocnstructed data is very similar to the original input. This way we can make sure that we are transferring enough information to a small encoded space so that we are enable to reconstruct the entire image from it.

Bellow is a simple figure illustrating the gist of and autoencoder:

<a href="url"><img src="https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/AE.png" align="middle" alt="autoencoder architecture" width="600"></a>

# Proposed Methodology
In a deep neural network, each layer is calculated by multiplying the previous layer by the weight matrix and then adding a bias term and finally passing all the values through an activation function:

<img src="https://render.githubusercontent.com/render/math?math=h^{(i)} = \sigma( W^{(i)} * h^{(i-1)} %2B b^{(i)})">


where 
<img src="https://render.githubusercontent.com/render/math?math=W"> is the weight matrix between 
<img src="https://render.githubusercontent.com/render/math?math=h^{(i)}">
and 
<img src="https://render.githubusercontent.com/render/math?math=h^{(i-1)}">, 
<img src="https://render.githubusercontent.com/render/math?math=\sigma">
is the activation function(for an overview of famous activation function read [this article](https://missinglink.ai/guides/neural-network-concepts/7-types-neural-network-activation-functions-right/)), and 
<img src="https://render.githubusercontent.com/render/math?math=b^{(i)}"> 
is the bias for 
<img src="https://render.githubusercontent.com/render/math?math=i">th 
hidden layer. And for the simplification let's assume 
<img src="https://render.githubusercontent.com/render/math?math=h^{(0)}"> 
is the input layer and 
<img src="https://render.githubusercontent.com/render/math?math=h^{(N)}"> 
is the output layer which is the input reconstruction in case of autoencoders.

Now, let's write this down for an entire autoencoder with N layers:

<img src="https://render.githubusercontent.com/render/math?math=h^{(N)} = \sigma( W^{(i)} * (\sigma( W^{(i-1)} * (...\sigma( W^{(1)} * h^{(0)} %2B b^{(1)})...) %2B b^{(i-1)}) %2B b^{(i)})">

Further, let's consider linear activation function which maps each value to itself. Therefore for one single layer we will have:

<img src="https://render.githubusercontent.com/render/math?math=h^{(i)} = \sigma( W^{(i)} * h^{(i-1)} %2B b^{(i)}) = W^{(i)} * h^{(i-1)} %2B b^{(i)}">

And for the whole network:

<img src="https://render.githubusercontent.com/render/math?math=h^{(N)} = \sigma( W^{(i)} * (\sigma( W^{(i-1)} * (...\sigma( W^{(1)} * h^{(0)} %2B b^{(1)})...) %2B b^{(i-1)}) %2B b^{(i)}) = W^{(i)} * ( W^{(i-1)} * (...( W^{(1)} * h^{(0)} %2B b^{(1)})..) %2B b^{(i-1)}) %2B b^{(i)}">

Let's simplify things. We can rewrite the above formula in a much simpler form:

<img src="https://render.githubusercontent.com/render/math?math=h^{(N)} = (W^{(i)} * W^{(i-1)} * ... W^{(1)}) * h^{(0)} %2B B ">

<img src="https://render.githubusercontent.com/render/math?math=h^{(N)} = W * h^{(0)} %2B B">

where 
<img src="https://render.githubusercontent.com/render/math?math=W"> is the mutlplication of all weight matrices that transforms the input to its reconstruction (note that this only holds if we have a linear activation function) and 
<img src="https://render.githubusercontent.com/render/math?math=B">
is the gerneral bias term that can be calculated from the previous equations.

Remember that it was mentioned before, the network is trained to produce a very similar data to the input. There for we can consider the identity matrix 
<img src="https://render.githubusercontent.com/render/math?math=I">
with the same dimention as the input to be the perfect choice for 
<img src="https://render.githubusercontent.com/render/math?math=W"> and if we can find all the individual weight matrices in a way that their multiplication is close to 
<img src="https://render.githubusercontent.com/render/math?math=I"> we can use them as initial weights of our network, and of course use a better activation function.

We can do this, by using [Non Negative Matrix Factorization](https://en.wikipedia.org/wiki/Non-negative_matrix_factorization) to factorize the identity matrix into as many matrices as we desire with arbitrary dimensions.

One might say, this is a learning process by itself and can be computationaly expensive. However, for a certain network architecture we only need to calculate these initial matrices one and reuse it for any type of data. Moreover, this good initialization will later help the neural network to reach its goal in much fewer epochs and even with less data.

# Experiments
I compared the proposed weight initialization with 5 other [famous approaches that are used in keras ](https://keras.io/api/layers/initializers/) including Random Normal, Glorot Normal, He Normal, VarianceScaling, and Orthogonal. 

* Data: MNIST digit and MNIST fasion data.
* Number of epochs: 10 vs 50
* Number of training samples: 3000 vs 60000

Which led to 8 total result sets to compare the 6 initialization approaches. In order to compare the performance of each method, I used loss drop graph during each epoch and the reconstructed input compared with the original input:


## MNIST digit Data
### Loss drop comparison
![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/loss_drop_legend.png)
|                          | Epochs = 10              |  Epochs = 50 |
:-------------------------:|:------------------------:|:-------------------------:
Training Samples = 3000    |![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_digit_loss_3000_10.png) |  ![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_digit_loss_3000_50.png)
Training Samples = 60000   |![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_digit_loss_all_10.png) |  ![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_digit_loss_all_50.png)


### Reconstruction comparison
|                          | Epochs = 10              |  Epochs = 50 |
:-------------------------:|:------------------------:|:-------------------------:
Training Samples = 3000    |![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_digit_reconstruction_3000_10.png) |  ![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_digit_reconstruction_3000_50.png)
Training Samples = 60000   |![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_digit_reconstruction_all_10.png) |  ![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_digit_reconstruction_all_50.png)


## MNIST fashion Data
### Loss drop
![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/loss_drop_legend.png)
|                          | Epochs = 10              |  Epochs = 50 |
:-------------------------:|:------------------------:|:-------------------------:
Training Samples = 3000    |![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_fashion_loss_3000_10.png) |  ![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_fashion_loss_3000_50.png)
Training Samples = 60000   |![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_fashion_loss_all_10.png) |  ![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_fashion_loss_all_50.png)

### Reconstruction comparison
|                          | Epochs = 10              |  Epochs = 50 |
:-------------------------:|:------------------------:|:-------------------------:
Training Samples = 3000    |![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_fashion_reconstruction_3000_10.png) |  ![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_fashion_reconstruction_3000_50.png)
Training Samples = 60000   |![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_fashion_reconstruction_all_10.png) |  ![](https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/mnist_fashion_reconstruction_all_50.png)


# Conclusion
As you can see in the previous section, while there is no significant difference between different initialization methods if you use a large amount of training samples or more epochs, it is obvious that NNMF initialization technique gets to a better reconstructed image after fewer epochs even with much lower number of samples.
