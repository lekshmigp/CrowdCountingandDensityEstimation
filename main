import cv2
import numpy as np

def count_people(image_path):
    # Load image
    img = cv2.imread(image_path)
    if img is None:
        print(f"Error: Could not read image at {image_path}")
        return -1
    
    # Convert to grayscale and enhance
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    gray = cv2.equalizeHist(gray)
    
    # Simple background subtraction
    _, thresh = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY_INV + cv2.THRESH_OTSU)
    
    # Clean up the image
    kernel = np.ones((3,3), np.uint8)
    cleaned = cv2.morphologyEx(thresh, cv2.MORPH_OPEN, kernel, iterations=2)
    
    # Find contours
    contours, _ = cv2.findContours(cleaned, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    
    # Count people (filter by size)
    count = 0
    for cnt in contours:
        area = cv2.contourArea(cnt)
        if 100 < area < 5000:  # Reasonable person size range
            x,y,w,h = cv2.boundingRect(cnt)
            aspect_ratio = w/h
            if 0.3 < aspect_ratio < 3.0:  # Human proportions
                count += 1
    
    return count

# Usage
image_path = "crowd.jpg"  # Replace with your image path
people_count = count_people(image_path)

if people_count >= 0:
    print(f"Estimated people count: {people_count}")
else:
    print("Failed to process image")
