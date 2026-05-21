# moving-object-detertor
#A Moving Object Detector is a system or software used to identify objects that are in motion within a video or camera feed. It works by comparing consecutive frames and detecting changes in position or movement.  It is commonly used in:  Security and surveillance systems Traffic monitoring Robotics and automation 
 #code the is require to run 


 
 import cv2
import numpy as np

cap = cv2.VideoCapture(0)

# Check camera
if not cap.isOpened():
    print("Error: Cannot open camera")
    exit()

# Read first frame
ret, frame1 = cap.read()

if not ret:
    print("Error: Cannot read frame")
    exit()

gray1 = cv2.cvtColor(frame1, cv2.COLOR_BGR2GRAY)
gray1 = cv2.GaussianBlur(gray1, (21, 21), 0)

print("Press 'q' to quit")

while True:
    ret, frame2 = cap.read()

    if not ret:
        break

    gray2 = cv2.cvtColor(frame2, cv2.COLOR_BGR2GRAY)
    gray2 = cv2.GaussianBlur(gray2, (21, 21), 0)

    diff = cv2.absdiff(gray1, gray2)

    thresh = cv2.threshold(diff, 25, 255, cv2.THRESH_BINARY)[1]
    thresh = cv2.dilate(thresh, None, iterations=2)

    contours, hierarchy = cv2.findContours(
        thresh,
        cv2.RETR_EXTERNAL,
        cv2.CHAIN_APPROX_SIMPLE
    )

    for contour in contours:

        if cv2.contourArea(contour) < 500:
            continue

        x, y, w, h = cv2.boundingRect(contour)

        cv2.rectangle(
            frame2,
            (x, y),
            (x + w, y + h),
            (0, 255, 0),
            2
        )

    cv2.imshow("Moving Object Detection", frame2)

    gray1 = gray2

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()

# Moving Object Detector

A real-time moving object detection system built with Python and OpenCV.

## What it does
- Detects moving objects in video using background subtraction
- Highlights motion regions frame by frame

## Technologies Used
- Python
- OpenCV

## How to run
1. Install OpenCV: pip install opencv-python
2. Run: python your_filename.py
