#  Exp 7 - HOUGH TRANSFORMATION
## Name: Ashwath M
## Register number: 212223230023

##  Aim

To implement a basic lane detection pipeline using OpenCV by completing missing code segments at specified locations.
---

##  Software Used

* Anaconda – Python 3.7
* Jupyter Notebook / VS Code
* OpenCV (cv2)
* NumPy
* Matplotlib

---

##  Algorithm & Explanation

Step1:

Import all the necessary modules for the program.

Step2:

Load a image using imread() from cv2 module.

Step3:

Convert the image to grayscale.

Step4:

Using Canny operator from cv2,detect the edges of the image.

Step5:

Using the HoughLinesP(),detect line co-ordinates for every points in the images.Using For loop,draw the lines on the found co-ordinates.Display the image.

Output


---

###  Step 1: Import Libraries

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

---

###  Step 2: Read the Image

```python
image = cv2.imread('lan_img1.jpg') 
image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
```

---

###  Step 3: Convert to Grayscale

```python
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
```

---

###  Step 4: Display Images

```python
plt.figure(figsize=(10,20))
plt.subplot(121);plt.imshow(image);plt.title("Input Image")
plt.subplot(122);plt.imshow(gray_image,cmap='gray');plt.title("Gray Image")
plt.show()
```

---

###  Step 5: Thresholding

```python
threshold = cv2.inRange(gray_image, 150, 255)

plt.figure(figsize = (20, 10))
plt.subplot(1,1,1);plt.imshow(threshold,cmap = 'gray'); plt.title('Threshold');plt.show()
```

---

###  Step 6: Region of Interest (ROI)

```python
roi_vertices = np.array([[[100, 540],
                          [900, 540],
                          [515, 320],
                          [450, 320]]])
mask = np.zeros_like(threshold)   

if len(threshold.shape) > 2:
    channel_count = threshold.shape[2] 
    ignore_mask_color = (255,) * channel_count
else:
    ignore_mask_color = 255

cv2.fillPoly(mask, roi_vertices, ignore_mask_color)
roi = cv2.bitwise_and(threshold, mask)

plt.figure(figsize = (20, 10))
plt.subplot(1,3,1); plt.imshow(threshold, cmap = 'gray'); plt.title('Initial threshold')
plt.subplot(1,3,2); plt.imshow(mask, cmap = 'gray');      plt.title('Polyfill mask')
plt.subplot(1,3,3); plt.imshow(roi, cmap = 'gray');       plt.title('Isolated roi');
```

---

### Step 7: Edge Detection & Gaussian Blur

```python
low_threshold = 50
high_threshold = 100
edges = cv2.Canny(roi, low_threshold, high_threshold)

kernel_size = 3
canny_blur = cv2.GaussianBlur(edges, (kernel_size, kernel_size), 0)

plt.figure(figsize = (20, 10))
plt.subplot(1,2,1); plt.imshow(edges, cmap = 'gray'); plt.title('Edge detection')
plt.subplot(1,2,2); plt.imshow(canny_blur, cmap = 'gray'); plt.title('Blurred edges');
```

---

###  Step 8: Hough Transform

```python
def draw_lines(img, lines, color = [255, 0, 0], thickness = 2):
    if lines is not None:
        for line in lines:
            for x1,y1,x2,y2 in line:
                cv2.line(img, (x1, y1), (x2, y2), color, thickness)


rho = 1
theta = np.pi / 180
threshold = 50
min_line_len = 10
max_line_gap = 20

lines = cv2.HoughLinesP(
    canny_blur, rho, theta, threshold, minLineLength = min_line_len, maxLineGap = max_line_gap)

hough = np.zeros((image.shape[0], image.shape[1], 3), dtype = np.uint8)
draw_lines(hough, lines)

print("Found {} lines, including: {}".format(len(lines), lines[0]))
plt.figure(figsize = (15, 10)); plt.imshow(hough);
```

---

##  Expected Output

### Original image:

 <img width="499" height="288" alt="image" src="https://github.com/user-attachments/assets/afdfe4ca-8ae9-45c6-9690-b16c71b323c6" />

### Grayscale image:
  <img width="518" height="306" alt="image" src="https://github.com/user-attachments/assets/03224aef-8951-4cc3-85f2-46cbfaaa7f2b" />

### Thresholded image:
  <img width="1239" height="741" alt="image" src="https://github.com/user-attachments/assets/c044c3f5-cf6a-4384-9db0-0684ee3cb635" />

### ROI masked image:
  <img width="1254" height="265" alt="image" src="https://github.com/user-attachments/assets/2c6ef110-d2c5-4048-a347-ee74b2205eea" />

### Edge detected image:
  <img width="631" height="355" alt="image" src="https://github.com/user-attachments/assets/bd65719c-87db-4344-a96f-2ba9c22506bc" />

### Smoothed image:
  <img width="607" height="361" alt="image" src="https://github.com/user-attachments/assets/4378f25e-6852-4c58-a6d7-d36534078aa5" />

### Detected lines:
  <img width="1250" height="721" alt="image" src="https://github.com/user-attachments/assets/0fa2d041-87da-412d-b1f1-10c6b7b2b275" />


---


## Result

Thus, the lane detection pipeline is successfully implemented by completing the missing code sections. The system detects and highlights lane lines effectively.

---
