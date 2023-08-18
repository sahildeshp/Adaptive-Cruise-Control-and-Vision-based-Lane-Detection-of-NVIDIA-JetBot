ECE 536 Team JetBot - 1

There are 3 Python Notebooks submitted.


1. Data Collection -->

With this notebook, we collect an image regression dataset that will enable JetBot to follow a path.
We will teach the JetBot to detect a target x, y image coordinate that the JetBot will chase. As JetBot gets closer to the point, it moves further along the path. We used a gamepad joystick to move the marker 
for the target x,y position on the JetBot live camera feed and stored those images. The target position
coordinated were stored in the file name for each image so that they can be accessed later while training 
the network.


2. Model Training -->

In this notebook we  train a neural network to take an input image, and output a set of x, y values corresponding to a target.
We will be using PyTorch deep learning framework to train ResNet18 neural network architecture model for road follower application.
Data collected in the Data Collection notebook is split on train and test sets.
We use ResNet-18 model available on PyTorch TorchVision.
We also use transfer learning, with which we can repurpose a pre-trained model (trained on millions of images) for a new task that has possibly much less data available.
We train for 50 epochs and save best model if the loss is reduced.
Once the model is trained, it will generate best_steering_model_xy.pth file which  is used for inferencing in the live demo notebook.


3. Live Demo -->

With the best_steering_model_xy.pth generated in the last step, we now create a pre-processing function to do the following tasks:
a. Convert from HWC layout to CHW layout
b. Normalize using same parameters as we did during training (our camera provides values in [0, 255] range and training loaded images in [0, 1] range so we need to scale by 255.0
c. Transfer the data from CPU memory to GPU memory
d. Add a batch dimension

We also defined some sliders in this notebook to control the JetBot. We created Speed control, Steering Gain Control (Kp), Steering I control (Ki), Steering D control (Kd) and Steering Bias Control sliders to implement the best PID Controller.

The execute function calculates an error angle by looking at the trained model with the target values and the actual camera feed. The goal of the controller is to minimize this error angle with our control action.