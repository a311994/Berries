# Berries
Classification of ripe and unripe berries

## Instructions to run the detection model
1. Clone the repository onto a local system
```
git clone https://github.com/a311994/Berries.git
```
2. Install the necessary dependencies
```
pip3 install -qr Berries/requirements.txt 
```
3. Change directory into the cloned folder
```
cd Berries
```
4. To add images for testing, go to runs/train/test/images and dump the test files in '.jpg' format
5. Go back to base folder 'Berries' and open terminal again
6. To enable detection through the trained model use the following command:
```
python3 detect.py --weights runs/train/yolov5s_results2/weights/last.pt --img 416 --conf 0.4 --source runs/train/test/images
```
7. The resulting images with confidence indeces will be available in runs/detect/exp[largest number]

## Details about the model
The model is based on yolov5 which is a recent pytorch based architecture developed by [Glenn Jocher](https://github.com/ultralytics/yolov5).
Prior to finalising on yolov5, other architectures that were tested were:

1. yolov3
2. Faster R-CNN ResNet50 V1 640x640
3. SSD MobileNet V1 FPN 640x640

YoloV5 was chosen due to its high compatibility with latest cuda and cuDNN drivers (tensorflow 2 had a lot of incompatibility issues which lead to lack of customisation fo the models to better suit the needs for berry detection). YoloV3 was less responsive as compared to YoloV5 as the latter offered three model variants bassed on dataset size and computing prowess. There was also an attempt to create a segmential algorithm to separate the red ripe berries from the green unripe berries, but with the implementation of YoloV5, the process seemed redundant as the the architechture was already capable enough to distinguish these features.

The image dataset consisted of 209 images which were then augmented to create a dataset of 501 images. Majorly cropping, and zoom techniques were implemented to create augmented images. 

The python script [Datasets.py](utils/datasets.py) was configured according to the dataset, listing the number of classes, image size etc. More information about customising the aforementioned script and also further training of the model to a custom dataset can be found in this [Tutorial](tutorial.ipynb). 

After training the model to the custom dataset, a series of graphical results were generated which can be found in the [Results Folder](runs/train/yolov5s_results2/results.png). 
The following is the results of the trained model as of November 15 2020.
![Results](https://github.com/a311994/Berries/blob/main/runs/train/yolov5s_results2/results.png?raw=true)

Post training, a set of weights for the model are also generated which are located in the [Weights Folder](runs/train/yolov5s_results2/weights). Currently the detection is run by utilising the [last set of weights](runs/train/yolov5s_results2/weights/last.pt), but there is also the option to utilise the ['Best' set if weights](runs/train/yolov5s_results2/weights/best.pt). To do so just change the '--weights' parameter in step 6 of the instructions above from 'last.pt' to 'best.pt'. You can also utilise these weights to explicitly train a separate yoloV5 model if necessasary.

