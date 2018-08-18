# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   
### Reflections.

Following the walkthrough in the beginning we don't have any data for the previous waypoints the car took, so we create 2 points to use with the current car position and the calculated previous car position. 
`main.c [345:355]`

If we have previous points we use the last 2 to make the path tangent so it is smoother. 
`main.c [356:370]`

We then generate 3 additional points 30 m each ahead of the car reference point. 
`main.c [374:384]`

To easy the calculations we then recalculate the points in car reference angle to 0, so the points are in a line that goes strait ahead of the front of the car.
`main.c [386:393]`

We now use spline to generate the path and then determine what should the spacing between the points be in order to achieve the desired velocity. 
`main.c [395:413]`

Finally the generate the new points to send to the simulator by adding the previous points and then fill the rest to 50 from the spline.
`main.c [415:439]`
 

#### Sensor fusion, changing lanes and collision avoidance.  

Going in a loop of each object in the sensor_fussion vector we consider the once that are in our lane or a lane next to ours. If the object is in our lane and the gap is less than 30 meters we trigger flags for changing lanes and object too close. 
Since we are already looping through the objects we also calculate a cost for the left and right lanes next to ours. 
`main.c [268:313]`

There is also safety feature that prevent a lane change if there is an object in that lane 15 meters ahead or behind us.
`main.c [302:304],[307:309]` 

There is also a safety feature that avoids changing lane outside of the 3 lanes in our direction.
`main.c [260:265]`

Based on the flags raised and left and right cost values we decide if we need to slow down or speed up. `main.c [317:312]` or change lanes `main.c [324:330]`.


## Improvments

There are lots of improvements that can be made, but this code is enough to pass the project. 
Here are some examples. 

1. The speed of the objects should also be considered. For example the deceleration of the car if there is a car in front should be proportional to the delta of the speeds of both objects. That way the change would be gradual and if it is not safe to change lane ideally we should start driving with the speed of the car ahead. 
2. When calculating the costs the speed also should be taken into consideration. For example if we are in the middle lane and there are cars left and write we should choose to get behind the faster car, since this would lower the chance of making another lane change soon. 
3. The check for objects behind us could be lower than 15 m again with the speed taken into account so we could slide safely in front of another car after we passed it with greater speed. 

... 
There are also much more improvements to be made if this was a real world project.
