vehicle_counter

Detects and counts vehicles moving from left to right or vice versa. Also for raspberry pi. Includes speed approximation.

## Procedure

### Process the frame
- Convert original frame to grayscale and remove noise
- Create an average frame, which is basically the static background. Each new frame impacts this average frame to some extent
- Compute the difference between average and current frame to get the foreground
- Apply a threshold to this difference frame to get clean shapes of the moving objects in the foregroun



### Get the contours
- Get the contours and filter out small ones
- Reduce the amount of points each contour consists of and sort the contours by size
- Find contours which are close to each other and merge these. For instance, if a is close to b and b is close to c, then a is also close to c via b. Consequently, the new merged contour consists of all three contours a, b and c



### Get the centroids and add these to vehicles or create new vehicles
- Vehicles are stored as dictionaries with the attributes: id, fist time seen, last time seen, direction, a boolean 'found' and a list of 'tracked' centroids (blue dots)
- If the distance isn't too great and the direction is correct, the centroid will be added to this vehicle. An added centroid will be removed from the list of candidates
- If there are still centroids left in the candidates list, check if a new vehicle can be created
- If a centroid from the current frame is close to one of the last frame, a new vehicle will be created

###  Detect whether vehicles crossed center line
- Vehicles which haven't been seen for some time will be removed and its data saved to a .csv-file
- If a vehicle has not been found yet and has crossed the center line, the vehicle counter increments
- If a vehicle has crossed the left and right barrier, its speed will be calculated


## Materials
- <a href="https://github.com/iandees/speedtrack">Speedtrack</a> repo by @iandees
- <a href="https://docs.opencv.org/3.3.1/d3/d05/tutorial_py_table_of_contents_contours.html">Tutorial</a> about contours
- <a href="https://www.pyimagesearch.com/2015/12/28/increasing-raspberry-pi-fps-with-python-and-opencv/">Blog post</a> about raspberry pi threading on pyimagesearch by Adrian Rosebrock
