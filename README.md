# CNN-on-CIFAR-10

There are plenty of datasets available online for implementing various algorithms. **CIFAR-10** is one of them. CIFAR(Canadian Institute For Advanced Research)-10 is collection of images with a size of 32x32 with a total of 10 target classes, mainly - airplane, automobile, bird, cat, deer, dog, frog, horse, ship, truck. CIFAR-100 dataset has a total of 100 target classes. It contains 60,000 images (50,000 - training, 10,000- testing).

A litte review about CNN - CNN stands for Convolution Neural Network. Apart from the normal neural network, we have introduced convolutions over it. The main reason to use CNN over normal feed forward network id it's efficiency to extract the features of the image and convert it into lower dimension without losing the context of the image. This helps us to deal with bigger sized images in an appropriate manner. Pooling layers are used along with CNN to reduce the dimensionality, the number of parameters to learn and hence increase the computation.

This CNN model will be build on the PyTorch framework. The flow of code is described in detail.
Import all the necessary libraries which includes matplotlib.pyplot, torch etc. We should set the device on which we want to perform the computation. Check if cuda is available else set the device on cpu. We can import the CIFAR-10 dataset from torchvision as train data and test data. While dowloading the data, we pass on certain parameters, one of them includes- converting or transforming the input into tensors which would be fed to our model. 
Now we divide our big data into small chunks. We choose the batch size randomly (preferred - 32, 64, 100, 128). We define a Dataloader. The job of the *Dataloader* is to divide the incoming data according to the batch size defined. Next we define the parameters for the model, namely - number of epochs, learning rate, num of classes(which are 10 here).  We can define a class for building our model. We pass nn.Module as an argument because it contains all the base classes for building a neural network. Now, we come down on defining our layers:
 * Initially, we set *Sequential*- it will contain the sequence of operations on a particular layer.
 * Talking about our first layer - Conv2D This contains a few parameters - the first one refers to the number of input channels, here we have RGB images so there will            be 3 channels. If the case was for grey images, the input channel size would have been 1. The second parameter is the output channel size. This the the number of              neurons we want to connect to the input channel. The kernel size refers to the filter size,  here it is 3x3. The stride parameter refers to the steps we take the              move the filter window. Here we take stride as 1. Padding means whether want to pad our image giving it extra borders. Here it is 0. 
 * The we apply the BatchNorm2D - this function is responsible for improving the learning rate of the model. As the name suggests, it normalizes the incoming data by            managing the internal covariate shift, i.e. normalizes the mean -0 and standard deviation to 1. 
 * After this we have the ReLu function. 
 * We insert another Conv2D layer over the previous, similar but with a slight difference in the parameters. Here the number of input channels is equal to the number            of output channels of the previous layer, i.e. 16. We again apply BatchNorm2D. 
 * Atlast we apply the MaxPool operation on this layer. The parameters include the kernel size . Here we take it to be 2x2. We take the stride to be 2.

So we have defined our layer 1.
It completely depends upon us how many Conv2D we want to define in our Sequential model. 
After layer 1, we need to calculate the output size of our image. This can be done with the help of a formula: 
 * New height/width = ((Previous Height/width - Filter Size + 2xPadding)/Stride)+1

Just like layer 1, we will define our layer 2 and layer 3. We will manipulate the input and output channels accordingly. After our layer 3, we will define a fully connected layer(A layer in which all the neurons are connected to all the neurons in the previous layer) with nn.Linear. The paramaters for this layer will the dimensions of our final output image times the number of neurons in the previous layer. In this case, we have final image dimension as 4x4 and number of neurons in the previous layer were 512.

After we wre done defining all the layers, we define the forward function for forward propagation. The output after the layer 3 need to be converted into a 1D array in order to feed it to the fully connected layer. So, we reshape it. 
We define loss and optimizer for the model. Since this is a classification problem, we use crossentropy loss and optimizer as Adam. 
To make our model work, we need to define the training loop. We run the loop in increment of batch size. We set the images and labels to device. We pass the images through out model, and calculate loss. For our backward pass(where we optimize the wights in order to reduce the loss), we use optimizer.zero_grad(), so that we don't accumulate the gradient from previous iteration. loss.backward() calculates the loss/gradient. optimizer.step() applies optimizer accordingly so as to change the weights.

Now that our model is trained, we evaluate and test it. We run a loop similar to the one mentioned for the training loop, but the difference here is that after our output is predicted, we need to check whcih label has the maximum probablility. For this purpose, we use torch.max. Then we check the accuracy of correctly predicted variables. With this, we come to the end this small project building.

