## NAME : Ayisha Rinsi k
## REG.NO: 212223040022
# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.
## program
```
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Load images
faceImage = cv2.imread('profile.jpg')
plt.imshow(faceImage[:, :, ::-1]); plt.title("Face")

glassPNG = cv2.imread('pro.png', -1)
plt.imshow(glassPNG[:, :, ::-1]); plt.title("glassPNG")

# Resize sunglasses to fit better (width=180, height=140)
glassPNG = cv2.resize(glassPNG, (180, 140))
print("image Dimension ={}".format(glassPNG.shape))

glassBGR = glassPNG[:, :, 0:3]    # Color channels
glassMask1 = glassPNG[:, :, 3]    # Alpha channel

plt.figure(figsize=[15, 15])
plt.subplot(121); plt.imshow(glassBGR[:, :, ::-1]); plt.title('Sunglass Color channels')
plt.subplot(122); plt.imshow(glassMask1, cmap='gray'); plt.title('Sunglass Alpha channel')

# Overlay coordinates - adjust for smaller glasses
y_offset = 295
x_offset = 310
height, width = glassPNG.shape[:2]

faceWithGlassesNaive = faceImage.copy()
faceWithGlassesNaive[y_offset:y_offset+height, x_offset:x_offset+width] = glassBGR
plt.imshow(faceWithGlassesNaive[..., ::-1])

# Prepare the mask for blending
glassMask = cv2.merge((glassMask1, glassMask1, glassMask1))
glassMask = np.uint8(glassMask / 255)

faceWithGlassesArithmetic = faceImage.copy()
eyeROI = faceWithGlassesArithmetic[y_offset:y_offset+height, x_offset:x_offset+width]

# Blend glasses and face region using mask and inverse mask
maskedEye = cv2.multiply(eyeROI, (1 - glassMask))
maskedGlass = cv2.multiply(glassBGR, glassMask)
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

plt.figure(figsize=[20, 20])
plt.subplot(131); plt.imshow(maskedEye[..., ::-1]); plt.title("Masked Eye Region")
plt.subplot(132); plt.imshow(maskedGlass[..., ::-1]); plt.title("Masked Sunglass Region")
plt.subplot(133); plt.imshow(eyeRoiFinal[..., ::-1]); plt.title("Augmented Eye and Sunglass")

# Place blended region into face image
faceWithGlassesArithmetic[y_offset:y_offset+height, x_offset:x_offset+width] = eyeRoiFinal

# Show original and final image with glasses
plt.figure(figsize=[20, 20])
plt.subplot(121); plt.imshow(faceImage[:, :, ::-1]); plt.title("Original Image")
plt.subplot(122); plt.imshow(faceWithGlassesArithmetic[:, :, ::-1]); plt.title("With Sunglasses")
plt.show()
```
## Output:
<img width="860" height="695" alt="image" src="https://github.com/user-attachments/assets/32ec0039-5324-4e08-af8f-eb7a8b5fadb6" />
<img width="751" height="347" alt="image" src="https://github.com/user-attachments/assets/fa4fb2e9-ae99-40d0-a7cd-d40bacf794c3" />

<img width="352" height="443" alt="image" src="https://github.com/user-attachments/assets/cc417228-c257-4892-9150-e366acba7a0e" />
