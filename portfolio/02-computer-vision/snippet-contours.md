# Contour Detection and Radius Tracking per Frame

```python
frame_gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
normalised = frame_gray / np.maximum(frame_gray_0.astype(np.float32), 1e-6)
blurred = cv2.GaussianBlur(frame_gray, (5, 5), 0)

_, mask = cv2.threshold(blurred, threshold, 255, cv2.THRESH_BINARY)
mask = mask.astype(np.uint8)

contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
largest = max(contours, key=cv2.contourArea)

area = cv2.contourArea(largest)
equivalent_radius = np.sqrt(area / np.pi)

radii.append(equivalent_radius)
times.append(frame_id / fps)