# Image Datasets And Tensorflow



# Images
![Screenshot of my Jupyter Notebook](https://github.com/HanHsunShih/Coding-Two-Submission/blob/main/screenshotOfTenserflow.jpg)

# Code
```
import os
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt

# Load image file from directory
img_path = os.path.join('img_align_celeba', '000001.jpg')

# Load the image and print its shape
img = Image.open(img_path)

# Convert image to numpy array
img_arr = np.array(img)

# Define transformation functions
def brightness(img, value):
    hsv = np.array(Image.fromarray(img).convert('HSV'))
    hsv[:, :, 2] = hsv[:, :, 2] + value
    img = Image.fromarray(np.uint8(hsv), 'HSV').convert('RGB')
    return np.array(img)

def contrast(img, value):
    factor = (259 * (value + 255)) / (255 * (259 - value))
    img = Image.fromarray(np.uint8(np.clip(factor * (img - 128) + 128, 0, 255)))
    return np.array(img)

def saturation(img, value):
    hsv = np.array(Image.fromarray(img).convert('HSV'))
    hsv[:, :, 1] = hsv[:, :, 1] * value
    img = Image.fromarray(np.uint8(hsv), 'HSV').convert('RGB')
    return np.array(img)

def hue(img, value):
    hsv = np.array(Image.fromarray(img).convert('HSV'))
    hsv[:, :, 0] = hsv[:, :, 0] + value
    img = Image.fromarray(np.uint8(hsv), 'HSV').convert('RGB')
    return np.array(img)

def resize(img, size):
    img = Image.fromarray(img).resize(size)
    return np.array(img)

def rotate(img, angle):
    img = Image.fromarray(img).rotate(angle)
    return np.array(img)

# Apply transformations and display images
fig, ax = plt.subplots(2, 3, figsize=(15, 10))
ax[0, 0].imshow(brightness(img_arr, 50))
ax[0, 0].set_title('Brightness +50')
ax[0, 1].imshow(contrast(img_arr, 50))
ax[0, 1].set_title('Contrast +50')
ax[0, 2].imshow(saturation(img_arr, 2))
ax[0, 2].set_title('Saturation x2')
ax[1, 0].imshow(hue(img_arr, 50))
ax[1, 0].set_title('Hue +50')
ax[1, 1].imshow(resize(img_arr, (128, 128)))
ax[1, 1].set_title('Resize to 128x128')
ax[1, 2].imshow(rotate(img_arr, 45))
ax[1, 2].set_title('Rotate 45 degrees')

plt.show()
```
