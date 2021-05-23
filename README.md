# kitten-detection
Re-trained MobileNet-SSD-V1 DetectNet to detect 5 kittens.

Trained on an NVIDIA Jetson Nano 4GB Developer Kit using [jetson-inference](https://github.com/dusty-nv/jetson-inference).

## Data directory structure:

<code>jetson-inference/python/training/detection/ssd/data/kittens</code>

- Annotations/ 2552 .XML annotations
- ImageSets/ 
  - Main/ [each .txt file contains the filenames of the images/annotations for each set]
    - train.txt
    - val.txt
    - trainval.txt
    - test.txt
- JPEGImages/ 2552 .JPG photos
- labels.txt

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
