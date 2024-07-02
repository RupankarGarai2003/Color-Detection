# Color Detection Application
This Python script allows you to detect colors in an image by double-clicking on the image. It uses the OpenCV library for image processing and the pandas library to handle color data from a CSV file.

## Prerequisites
Ensure you have Python installed on your system. You can download Python from the [ official website](https://www.python.org/)

## Installation
To run this script, you need to install the following libraries:

1. OpenCV: This library is used for image processing.
2. pandas: This library is used for data manipulation and analysis.
```bash
pip install opencv-python pandas
```

## Setup
1. Image File: Place the image you want to analyze in the same directory as your script. In this example, the image file is named colorpic.jpg.

2. CSV File: Ensure you have a CSV file named colors.csv that contains color names and their respective RGB values. The CSV file should be in the following format:

```bash
color,color_name,hex,R,G,B
```
## Usage
1. Run the script in your Python environment.
2. A window will open displaying the image.
3. Double-click on any part of the image to get the color name and RGB values of that point.
4. The color name and RGB values will be displayed on the image.
5. Press the Esc key to close the window.

## Script Explanation
### Import Libraries
```bash
import cv2
import pandas as pd
```
### Read Image and CSV File
```bash
img_path = r'colorpic.jpg'
img = cv2.imread(img_path)

# Reading csv file with pandas and giving names to each column
index = ["color", "color_name", "hex", "R", "G", "B"]
csv = pd.read_csv('colors.csv', names=index, header=None)
```
### Global Variables
These variables are used to store the color values and the coordinates of the mouse click.
```bash
clicked = False
r = g = b = x_pos = y_pos = 0
```
### Function to Get Color Name
This function calculates the minimum distance from all colors and gets the most matching color name.
```bash
def get_color_name(R, G, B):
    minimum = 10000
    for i in range(len(csv)):
        d = abs(R - int(csv.loc[i, "R"])) + abs(G - int(csv.loc[i, "G"])) + abs(B - int(csv.loc[i, "B"]))
        if d <= minimum:
            minimum = d
            cname = csv.loc[i, "color_name"]
    return cname
```
### Function to Get Mouse Click Coordinates
This function captures the x and y coordinates of the mouse double-click and extracts the color values at that point.
```bash
def draw_function(event, x, y, flags, param):
    if event == cv2.EVENT_LBUTTONDBLCLK:
        global b, g, r, x_pos, y_pos, clicked
        clicked = True
        x_pos = x
        y_pos = y
        b, g, r = img[y, x]
        b = int(b)
        g = int(g)
        r = int(r)
```
### Main Loop
This loop continuously displays the image and updates the color information when the user double-clicks on the image.
```bash
cv2.namedWindow('image')
cv2.setMouseCallback('image', draw_function)

while True:
    cv2.imshow("image", img)
    if clicked:
        cv2.rectangle(img, (20, 20), (750, 60), (b, g, r), -1)
        text = get_color_name(r, g, b) + ' R=' + str(r) + ' G=' + str(g) + ' B=' + str(b)
        cv2.putText(img, text, (50, 50), 2, 0.8, (255, 255, 255), 2, cv2.LINE_AA)
        
        if r + g + b >= 600:
            cv2.putText(img, text, (50, 50), 2, 0.8, (0, 0, 0), 2, cv2.LINE_AA)
        
        clicked = False

    if cv2.waitKey(20) & 0xFF == 27:
        break

cv2.destroyAllWindows()
```
### Example Output
When you double-click on the image, a rectangle will appear at the top left corner displaying the color name and RGB values of the clicked point.

## Output Screenshots:
![Example Output](https://github.com/RupankarGarai2003/Color-Detection/blob/main/Output%20imgs/Output%201.png)
![Example Output](https://github.com/RupankarGarai2003/Color-Detection/blob/main/Output%20imgs/Output%203.png)

You can customize the script to suit your needs, such as changing the input image or modifying the CSV file with different color data.

# Contributed By :
https://github.com/RupankarGarai2003
