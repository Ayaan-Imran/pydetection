# PyDetection
"pydetection" is a python library that allows users to find and locate various objects
such as hands and fingers. It contains many functions that simplify the tedious works and
worries to detect real life objects.

## How does it work?
- To detect objects, it uses a library called "mediapipe".  
  **Note**: Mediapipe requires a python version below or equal to 3.7!
- It also uses the "opencv-python" library to process and display images.

**Mediapipe**: `pip install mediapipe`  
**Opencv**: `pip install opencv-python`

## Installation and setup
- INSTALLATION: Open your terminal and type:
`pip install pydetection`
- SETUP: Open up a python file and type:
`import pydetection`

# PyDetection - Code examples
### Find hands in an image
```python
from pydetection import hand # The pydection module
import cv2 # Used for displaying images (Comes installed with pydetection

detector = hand.HandRecogniser() 
image_dir = "./images/hands_image.jpg" # Our image

# The variable "image": This contains the image with the hands outlined (Image can be shown with the cv2 module)
# The variable "hand_landmark_position": This contains the coordinates for each joint in each hand in the image (Can be used with the "Hand()" class
image, hand_landmark_position = detector.findHands(image_dir, draw=True) # If draw is false, the returned image will not have the hands outlined

cv2.imshow(image) # Use the cv2 module to display the image
cv2.waitKey(5000) # Wait for 5 seconds (5000 milli seconds) so that we can see the image.
```

### Get the name of each joint in the hand
```python
from pydetection import hand # The pydetection module

detector = hand.HandRecogniser() 
image_dir = "./images/hands_image.jpg" # The image

image, hand_landmark_position = detector.findHands(image_dir, draw=False) # We set draw to False because we don't need the image and it uses a lot of CPU.

# The "Hand()" class only excepts the coordinates of one hand
handObject = hand.Hand([hand_landmark_position[0]]) # The first hand

# Let's say that we need the coordinates of the thumb tip
id = handObject.thumb_tip["id"] # Every landmark (joint) has a specific ID
x = handObject.thumb_tip["x axis"] # The x axis of the coordinate
y = handObject.thumb_tip["y axis"] # The y axis of the coordinate
```

### Detect hand in real time webcam footage
```python
from pydetection import hand # The pydetection module
import cv2 # We will use this for reading and displaying webcam frames

detector = hand.HandRecogniser()
webcam = cv2.VideoCapture(0) # Change the number for a different camera. 0 is the default one.

while True:
    _, frame = webcam.read() # Gets a frane of the current webcam feed
  
    processed_image, hand_landmark_position = detector.findHands(frame, draw=True)
    
    cv2.imshow(processed_image) # Show the image with the hands outlined
    
    # Add a delay of 1 milli second and check for the keystroke "q"
    key = cv2.waitKey(1)
    if key == ord("q"):
      break
```

### Check if the thumb is closed
```python
from pydetection import hand # The pydetection module

detector = hand.HandRecogniser(max_num_hands=1) # Detect only 1 hand
image_dir = "./images/hand_image.jpg" # The image

# Fingers available: ["thumb", "index", "middle", "ring", "thumb"]
# Parameter "hand_orientation": You need to provide the function with whether the hand you want is a right or left hand.
# Parameter "hand_num": If you have multiple hands in this image, you need to specify which hand you are targetting
thumb_state = detector.getFingerState(image_dir, "thumb", hand_orientation="right", hand_num=1)

print(thumb_state) # It will print "True" if the hand is opened, and "False" if not
```
