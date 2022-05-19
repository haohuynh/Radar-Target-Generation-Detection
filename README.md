# Radar-Target-Generation-Detection

## Code Starter

See https://video.udacity-data.com/topher/2019/May/5ce36479_radar-target-generation-and-detection/radar-target-generation-and-detection.m

## 2D CFAR Process

### 1.Determine the number of Training & Guard cells for each dimension.

### 2.Slide the cell under test across the complete matrix. Make sure the CUT has margin for Training and Guard cells from the edges.

### 3.For every iteration sum the signal level within all the training cells. To sum convert the value from logarithmic to linear using db2pow function.

### 4.Average the summed values for all of the training cells used. After averaging convert it back to logarithmic using pow2db.

### 5.Further add the offset to it to determine the threshold.

### 6.Next, compare the signal under CUT against this threshold.

### 7.If the CUT level > threshold assign it a value of 1, else equate it to 0.

### 8.To keep the map size same as it was before CFAR, equate all the non-thresholded cells to 0.
