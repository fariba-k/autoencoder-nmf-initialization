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

<img src="https://render.githubusercontent.com/render/math?math=h^{(N)} = \sigma( W^{(i)} * (\sigma( W^{(i-1)} * (...\sigma( W^{(1)} * h^{(0)} %2B b^{(1)})...) %2B b^{(i-1)}) %2B b^{(i)}) =  W^{(i)} * ( W^{(i-1)} * (...( W^{(1)} * h^{(0)} %2B b^{(1)})..) %2B b^{(i-1)}) 2B b^{(i)}">

\t <img src="https://render.githubusercontent.com/render/math?math=%&#61;W^{(i)} * ( W^{(i-1)} * (...( W^{(1)} * h^{(0)} %2B b^{(1)})..) %2B b^{(i-1)}) 2B b^{(i)}">
