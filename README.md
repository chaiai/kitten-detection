# Re-training DetectNet


To determine which network to retrain, I tested a set of 20 images consisting of a variation of single kittens, multiple kittens, and various objects in the images.

Example code:

<code>cd /jetson-inference/build/aarch64/bin</code>

<code>detectnet --network=<ssd-mobilenet-v1/ssd-mobilenet-v2/ssd-inception-v2> "images/kittens/*.jpg" images/test/kittens/%i_<v1/v2/inception>.jpg</code>

I chose to retrain the SSD-Mobilenet-v1 model since the models performed similarly with the base model. The SSD-Mobilenet-v1 model is built into <code>jetson-inference</code>, so I chose to retrain that model for the fastest and most efficient inference.

## Data collection

In 4 seperate occasions, I took hundreds of photos of each of the kittens, making sure that I took an approxmitely equal number of photos of each kitten. I then labeled the images using [labelImg](https://github.com/tzutalin/labelImg). This process took a very long time, and I completed it over the course of 4 days.

I copied the directory structure that is saved using the <code>camera-capture</code> tool in <code>jetson-inference/utils</code> as follows:

<code>jetson-inference/python/training/detection/ssd/data/</code>

- Annotations/ [2552 .XML annotations]
- ImageSets/
    - Main/
        - train.txt
        - test.txt
        - val.txt
        - trainval.txt
- JPEGImages/ [2552 .JPG images]
    
    **[Note:]** The <code>.txt</code> files contain the filenames only of each image set, e.g. "IMG_1040", which has an associated <code>.jpg</code> and <code>.xml</code> file with the same filename

## Training

To save time, I trained the model using my host computer running Ubuntu 18.04 LTS with an NVIDIA GTX 2060 graphics card using the NGC PyTorch Container.

The best path (lowest loss value) was sent to my Jetson Nano via:

<code>scp models/kittens/mb2-mobilenet-Epoch-##-Loss-####.pth chai@<NANO_IP_ADDRESS>:/home/chai/jetson-inference/python/training/detection/ssd/models/kittens_200</code>

## Convert to ONNX

<code>python3 onnx_export.py --model-dir=models/kittens_200</code>

[Note: I do this step on the Nano itself to ensure the ONNX model is created using the correct parameters for the device it will be running on.]
    
This created a file named <code>mobilenet-ssd.onnx</code> in the <code>models/kittens_200</code> directory. This is the model from which I can launch <code>detectnet</code>.

## Live Demo

To use the CSI camera I have attached to my Jetson Nano (which needs to be flipped via <code>--input-flip</code>) in my configuration, I can call:

<code>detectnet --model=models/kittens_200/ssd-mobilenet.onnx --labels=models/kittens_200/labels.txt --input-blob=input_0 --output-cvg=scores --output-bbox=boxes --input-flip=rotate-180 csi://0</code>

video file

## The kittens

The 5 kittens were found very young and named after characters from *Friends*.

### Chandler

![chandler](https://user-images.githubusercontent.com/81446209/118375279-d8126600-b58e-11eb-8e0e-da88b67dd4a4.JPG)

### Gunther

![gunther](https://user-images.githubusercontent.com/81446209/118375292-e8c2dc00-b58e-11eb-8207-2dcbf2585fd0.JPG)

### Joey

![joey](https://user-images.githubusercontent.com/81446209/118375298-efe9ea00-b58e-11eb-8332-b673d9003813.JPG)


### Rachel

![rachel](https://user-images.githubusercontent.com/81446209/118375300-f24c4400-b58e-11eb-843c-06bacdf3862f.JPG)


### Ross

![ross](https://user-images.githubusercontent.com/81446209/118375305-f4ae9e00-b58e-11eb-8946-b33e56259ff6.JPG)
