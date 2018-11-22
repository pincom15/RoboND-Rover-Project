## Project: Search and Sample Return

---


**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook). 
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands. 
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./misc/rover_image.jpg
[perspective]: ./misc/perspective.jpg
[color_thresh]: ./misc/color_thresh.jpg
[coords]: ./misc/coords.jpg
[result]: ./misc/rover_result.jpg
[result_video]: https://youtu.be/n_YC3YzSePI

---

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.

![alt text][image1]

#### 2. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 

First, we need to read 320x160 pixel images from the rover camera to identify areas where the rover can drive. We can apply perspective transforms to convert the rover camera images into one where we are looking down on the world from above. Transform can be done by OpenCV functions 'cv2.getPerspectiveTransform()' and 'cv2.warpPerspective()'. 

![Perspective Transforms][perspective]

From the top view images, we can identify navigable terrain, obstacles, and samples using color thresholding. Default RGB values(160,160,160) are used since they work great. 

![Color Threshold][color_thresh]

After we obtain a map of the navigable terrain, we need to convert it to rover-centric coordinates, which means the rover camera pixel is located at x = 160 and y = 160 is mapped to (0,0).

![Coordinates][coords]

 Now, we can mark the map of navigable terrain in rover-centric coordinates to world coordinates. To do this, we can use a rotation followed by a translation.

### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.

Based on the above notebook, the `perseption_step()` function is modified and added `find_rocks()` function to find samples. In order to find golden rocks, RGB value(110, 110, 50) performs fine based on several trial.

#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different resuake a note of your simulator settings (resolution and graphics quality set on launch) and frames per second lts, particularly on different machines!  M(FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

I have used the following environment to run this project:
- OS : OSX 10.14.1
- Rover Simulation Screen Resolution : 800 x 600
- Rover Simulation Graphics Quality : Good
- FPS output : 37~41

Result
- Mapped: 42.5%
- Fidelity 60.9%
- Sample collected 3
- Time : 70.3s
- Result video : [LINK][result_video]

![Result][result]

The rover gets stuck sometimes. To avoid this, we can modify `decision_step` function in case that the rover keeps moving forward to obstacles.
