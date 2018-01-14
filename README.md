# Behaviorial Cloning Project

[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

* The outcome of this project was to derive a model using deep learning to drive a car autonomously on a simulated track. Training data is collected from a human driving the simulator. The collected images for every frame and corresponding steering angles are generalized to predict steering angles for new frames. The model is validated on a new track*

**Overview**
---
The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior 
* Design, train and validate a model that predicts a steering angle from image data
* Use the model to drive the vehicle autonomously around the first track in the simulator. The vehicle should remain on the road for an       entire loop around the track.
* Summarize the results with a written report

## 1. Data Recording and Augmentation

Desptite multiple attempts, and multiple laps of driving forwards and backwards using a keyboard I was not able to train a good model. At best I could only reach the bridge. So I switched over to dataset provided by udacity.

Along with the centere image I used left and right camera images as well. The steering output for left and right images had to be adjusted by a small angle to match the steering angle. The final model had an adjustment angle of 0.3.


# Preprocessing

Frames were normalized normalized using using keras's lambda layer to lower losses. Cropping2D layer was used to remove irrelevent parts which sped up training. Images were cropped by 70 px from top (sky, trees) and 25px from bottom (car hood).

![Cropping Example](https://d17h27t6h515a5.cloudfront.net/topher/2017/February/5892a531_cropped-image/cropped-image.jpg)

# Model Architecture

Ater having poor results with LeNet, I used the end to end model described by Nvidia in [this paper](http://images.nvidia.com/content/tegra/automotive/images/2016/solutions/pdf/end-to-end-dl-using-px.pdf). 

I tranied my model on hpc cluster's front node. Training was alot faster than it was on my PC. But it would have been a lot faster, if I had access to its gpu as well.

![Nvidia Architecture](https://devblogs.nvidia.com/parallelforall/wp-content/uploads/2016/08/cnn-architecture-624x890.png)

I added exponential linear units to add non leniarlity and a dropout layer after convolution to prevent overfitting. beAdam optimizer and mean square error as the loss function was used.

|Layer (type)   |
|---|
|Input (160,320,3)|
| Lambda (160,320,3) |
| Cropping  (65,320,3)|
| Convolution with 24 5x5 filters |
|Activation ELU|
| Convolution with 36 5x5 filters  |
|Activation ELU|
| Convolution with 48 5x5 filters  |
|Activation ELU|
| Convolution with 64 3x3 filters  |
|Activation ELU|
| Convolution with 64 3x3 filters  |
|Activation ELU|
|Dropout 0.3|
|Flatten|
|Dense (100 nuerons)|
|Dense (50 nuerons)|
|Dense (10 nuerons)|
|Dense (1 nueron)|


# Testing

I trained my model 20% of validation data and for 30 epochs, 25 epochs, 10 epochs, and 7 epochs. Having more than 10 did not make much of a diffirence.

The training and validation loss was minimal.

The final model trained for 7 epochs was predicting steering angles correctly and drove around the track without crashing.





