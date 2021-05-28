# Re-training DetectNet

I collected 2552 images of the 5 kittens using my iPhone and labeled them using labelImg on my desktop computer and saved them in Pascal-VOC format.

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

To determine which network to retrain, I tested a set of 20 images consisting of a variation of single kittens, multiple kittens, and various objects in the images.

Example code:

<code>cd /jetson-inference/build/aarch64/bin</code>

<code>detectnet --network=<ssd-mobilenet-v1/ssd-mobilenet-v2/ssd-inception-v2> "images/kittens/*.jpg" images/test/kittens/%i_<v1/v2/inception>.jpg</code>

#### Training

I trained the network using the tutorial from jetson-inference on re-training MobileNet-SSD-V1, but performed the training on my desktop computer. 

This computer is running Ubuntu 18.04 LTS, and I used the NVIDIA NGC PyTorch container with the jetson-inference directory mounted in the workspace.

After training for 50, 100, 150, and eventually 200 epochs, I transferred the best path from the model (Epoch 198, Loss=0.7824) to my Jetson Nano via SCP.

#### Export model to ONNX

From the Jetson Nano, I then ran:

<code>python3 export_onnx.py --model-dir=models/kittens_200</code>

  This created a file named <code>mobilenet-ssd.onnx</code> in the <code>models/kittens_200</code> directory. This is the model from which I can launch <code>detectnet</code>.

#### Live camera demo

From the directory <code>jetson-inference/python/training/detection/ssd</code>:

<code>detectnet --model=models/kittens_200/mobilenet-ssd.onnx --labels=models/kittens_200/labels.txt --input-blob=input_0 --output-cvg=scores --output-bbox=boxes --input-flip=rotate-180 csi://0</code>

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
