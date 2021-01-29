# autoencoder-nmf-initialization
This is a project about using non-negative matrix factorization for weight initialization in autoencoders.

# Background
Autoencoders are a specific form of neural networks build by two parts. First part is used to encode the input data to a space with lower dimension and the second part decodes the abstracted data to reconstruct the input.

The networks is trained in a way that the reocnstructed data is very similar to the original input. This way we can make sure that we are transferring enough information to a small encoded space so that we are enable to reconstruct the entire image from it.

Bellow is a simple figure illustrating the gist of and autoencoder:

<a href="url"><img src="https://github.com/fariba-k/autoencoder-nmf-initialization/blob/main/images/AE.png" align="middle" alt="autoencoder architecture" width="600"></a>

