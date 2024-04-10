# Sign-Language-Recognition-Using-MediaPipe-and-OpenCV

<br> This is a model that recognizes hand signs and finger gestures using the detected key points.
<br>
This repository contains the following contents.
* The main program
* Sign Language recognition model and its learning data
* Finger Gesture recognition model and its learning data
<br>

# Requirements
* Check the requirements.txt 
# Running the Model
Here's how to run the model using your webcam.
```bash
python app.py
```

Here is how to run the model using Docker and a webcam.
```bash
docker build -t hand_gesture .

xhost +local: && \
docker run --rm -it \
--device /dev/video0:/dev/video0 \
-v `pwd`:/home/user/workdir \
-v /tmp/.X11-unix/:/tmp/.X11-unix:rw \
-e DISPLAY=$DISPLAY \
hand_gesture:latest

python app.py
```


# Directory
<pre>
│  app.py
│  keypoint_classification.ipynb
│  point_history_classification.ipynb
│
├─model
│  ├─keypoint_classifier
│  │  │  keypoint.csv
│  │  │  keypoint_classifier.hdf5
│  │  │  keypoint_classifier.py
│  │  │  keypoint_classifier.tflite
│  │  └─ keypoint_classifier_label.csv
│  │
│  └─point_history_classifier
│      │  point_history.csv
│      │  point_history_classifier.hdf5
│      │  point_history_classifier.py
│      │  point_history_classifier.tflite
│      └─ point_history_classifier_label.csv
│
└─utils
    └─cvfpscalc.py
</pre>
### app.py
The main Program for inference and collecting training data

### keypoint_classification.ipynb
The main model training script for Sign language Recognition.

### point_history_classification.ipynb
The main model training script  for finger gesture recognition.

### model/keypoint_classifier
Stores key files needed to train the model for Sign language Recognition.<br>
These files are:
* Training data (keypoint.csv)
Used to store Training data
* Trained model(keypoint_classifier.tflite)
* Label data(keypoint_classifier_label.csv)
* Inference module(keypoint_classifier.py)

### model/point_history_classifier
Stores key files needed to train the model for finger gesture recognition.<br>
The following files are stored.
* Training data (point_history.csv)
* Trained model (point_history_classifier.tflite)
* Label data (point_history_classifier_label.csv)
* Inference module (point_history_classifier.py)

### utils/cvfpscalc.py
A module for FPS measurement.

# Training
Hand sign recognition and finger gesture recognition can add and change training data and re-train the model.

### Hand sign recognition training
#### 1.Learning data collection![P_thumb_up](https://github.com/MohamedMostafa21/Sign-Language-Recognition-Using-MediaPipe-and-OpenCV/assets/115514135/c22cc624-7e1e-4b14-a128-6469c20ad8c5)

Press "k" to enter "Logging Key Point" mode to save key points within a frame<br>
<img src="https://github.com/MohamedMostafa21/Sign-Language-Recognition-Using-MediaPipe-and-OpenCV/assets/115514135/c22cc624-7e1e-4b14-a128-6469c20ad8c5" width="60%"><br><br>
Press keys 0 to 9 to save the keypoints to a file called "keypoint.csv" located in the "model/keypoint_classifier" folder as shown below<br>
1st column represents the pressed number (used as class ID), the rest are the Key point coordinates<br>
<img src="https://user-images.githubusercontent.com/37477845/102345725-28d26280-3fe1-11eb-9eeb-8c938e3f625b.png" width="80%"><br><br>
These are the hand landmarks on each sign.<br>
<img src="https://user-images.githubusercontent.com/37477845/102242918-ed328c80-3f3d-11eb-907c-61ba05678d54.png" width="80%">
<img src="https://user-images.githubusercontent.com/37477845/102244114-418a3c00-3f3f-11eb-8eef-f658e5aa2d0d.png" width="80%"><br><br>
Right now, the model is able to recoginse and classify 10 signs, which are:.<br>
Peace sign, Thumps up, Thumbs down, Yes, No, I love you, Excellent, Really?, Hello, You.<br>
If necessary, add 3 or later, or delete the existing data of csv to prepare your own training data.<br>
<img src="https://user-images.githubusercontent.com/37477845/102348846-d0519400-3fe5-11eb-8789-2e7daec65751.jpg" width="25%">　<img src="https://user-images.githubusercontent.com/37477845/102348855-d2b3ee00-3fe5-11eb-9c6d-b8924092a6d8.jpg" width="25%">　<img src="https://user-images.githubusercontent.com/37477845/102348861-d3e51b00-3fe5-11eb-8b07-adc08a48a760.jpg" width="25%">

#### 2.Model training
Open "[keypoint_classification.ipynb](keypoint_classification.ipynb)" in Jupyter Notebook and execute from top to bottom.<br>
To change the number of training data classes, change the value of "NUM_CLASSES = 10" <br>and modify the label of "model/keypoint_classifier/keypoint_classifier_label.csv" as appropriate.<br><br>

#### X.Model structure
The image of the model prepared in "[keypoint_classification.ipynb](keypoint_classification.ipynb)" is as follows.
<img src="https://user-images.githubusercontent.com/37477845/102246723-69c76a00-3f42-11eb-8a4b-7c6b032b7e71.png" width="50%"><br><br>

### Finger gesture recognition training
#### 1.Learning data collection
Press "h" to enter the "Logging Point History" mode to save the history of fingertip coordinates .<br>
<img src="https://user-images.githubusercontent.com/37477845/102249074-4d78fc80-3f45-11eb-9c1b-3eb975798871.jpg" width="60%"><br><br>
IPress keys 0 to 9 to save the keypoints to a file called "point_history.csv" located in the "model/point_history_classifier" folder as shown below.<br>
1st column represents the pressed number (used as class ID), the rest are the Coordinates History<br>
<img src="https://user-images.githubusercontent.com/37477845/102345850-54ede380-3fe1-11eb-8d04-88e351445898.png" width="80%"><br><br>
The key point coordinates are the ones that have undergone the following preprocessing up to ④.<br>
<img src="https://user-images.githubusercontent.com/37477845/102244148-49e27700-3f3f-11eb-82e2-fc7de42b30fc.png" width="80%"><br><br>
In the initial state, 4 types of learning data are included: stationary, clockwise, counterclockwise, and moving. <br>
If necessary, add 5 or later, or delete the existing data of csv to prepare the training data.<br>
<img src="https://user-images.githubusercontent.com/37477845/102350939-02b0c080-3fe9-11eb-94d8-54a3decdeebc.jpg" width="20%">　<img src="https://user-images.githubusercontent.com/37477845/102350945-05131a80-3fe9-11eb-904c-a1ec573a5c7d.jpg" width="20%">　<img src="https://user-images.githubusercontent.com/37477845/102350951-06444780-3fe9-11eb-98cc-91e352edc23c.jpg" width="20%">　<img src="https://user-images.githubusercontent.com/37477845/102350942-047a8400-3fe9-11eb-9103-dbf383e67bf5.jpg" width="20%">

#### 2.Model training
Open "[point_history_classification.ipynb](point_history_classification.ipynb)" in Jupyter Notebook and execute from top to bottom.<br>
To change the number of training data classes, change the value of "NUM_CLASSES = 4" and <br>modify the label of "model/point_history_classifier/point_history_classifier_label.csv" as appropriate. <br><br>

#### X.Model structure
The image of the model prepared in "[point_history_classification.ipynb](point_history_classification.ipynb)" is as follows.
<img src="https://user-images.githubusercontent.com/37477845/102246771-7481ff00-3f42-11eb-8ddf-9e3cc30c5816.png" width="50%"><br>


# Reference
* [MediaPipe](https://mediapipe.dev/)
* [Kazuhito00/mediapipe-python-sample](https://github.com/Kazuhito00/mediapipe-python-sample)


