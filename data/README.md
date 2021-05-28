# Kitten Detection Model

I chose to retrain this model after running mobilenet-v1, mobilenet-v2, and inception-v2 on a video that has 4 different kittens appear to see which performed the best. Here are the results:

videos?

## Data collection

In 4 seperate occasions, I took hundreds of photos of each of the kittens, making sure that I took an approxmitely equal number of photos of each kitten. I then labeled the images using [labelImg](https://github.com/tzutalin/labelImg). This process took a very long time, and I completed it over the course of 4 days.

## Training

To save time, I trained the model using my host computer running Ubuntu 18.04 LTS with an NVIDIA GTX 2060 graphics card using the NGC PyTorch Container.

The best path (lowest loss value) was sent to my Jetson Nano via:

<code>scp models/kittens/mb2-mobilenet-Epoch-##-Loss-####.pth chai@<NANO_IP_ADDRESS>:/home/chai/jetson-inference/python/training/detection/ssd/models/kittens</code>

And ran the following command to export the model to ONNX:

<code>python3 onnx_export.py --model-dir=models/kittens</code>

[Note: I do this step on the Nano itself to ensure the ONNX model is created using the correct parameters for the device it will be running on.]

## Live Demo

To use the CSI camera I have attached to my Jetson Nano (which needs to be flipped via <code>--input-flip</code>) in my configuration, I can call:

<code>detectnet --model=models/kittens/ssd-mobilenet.onnx --labels=models/kittens/labels.txt --input-blob=input_0 --output-cvg=scores --output-bbox=boxes --input-flip=rotate-180 csi://0</code>

video file