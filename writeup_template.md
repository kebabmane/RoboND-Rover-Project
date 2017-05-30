Project Submission

Steps to complete the project:

Download the simulator appropriate for your OS (macOS, Linux, Windows)

Get setup with Python here

Fork, download or clone the project repository and have a look at the README.

Experiment with the simulator and take some data (explained here)

Run through the Jupyter notebook and fill in the process_image() function (see notebook for details).

Run drive_rover.py and experiment with autonomous mapping (details here).

Fill in the perception_step() and decision_step() functions and map the environment!

Note: running the simulator with different choices of resolution and graphics quality may produce different results, 
particularly on different machines! Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by drive_rover.py) in your writeup when you submit the project so your reviewer can reproduce your results.

## Rubic Requirements

### Write Up

Well this doc should suffice!

### Notebook Analysis

I have included two additional data sets in my notebook, specifically the last one I used was the test2 data set which focused heavily on finding rocks/obstacles so I could enhance my detection function. Early on when I ran the simulator and grabbed a bunch of data I really hadn’t considered or focused on how the rock obstacles would disrupt my rover as it drove around - once I moved these functions over to perception.py I had a few issues with the rover becoming stuck quite frequently.

Once I zoned in on this I lowered my RGB threshold values across the board which produced a much more fine grain view of the obstacles as shown below

![ipython notebook output - obstacles](/misc/detail_image.jpeg)

This helped the rover stay away from more obstacles however wasn’t full proof, because the rover had a base offset I found small rocks tended to be missed and the rover could be stuck on these and the perception.py functions could not see them - a 'unstuck' function was introduced to help clear these issues

Next I had to detect the rock samples to be collected - I was struggling to find an exact RGB threshold value that would suit, after asking around on slack it was suggested a single value wouldn't work and I should be exploring a range of yellows, the function its ‘self was posted on slack I merely updated with the RGB values I believe produced the most consistent results as shown below

![ipython notebook output - rock samples](/misc/rock_image.jpeg)

This has worked well in the simulator as it has never failed to detect a rock sample!

Based on the lessons I updated the process_image() function with the components required to produce a video that detects rock samples (if in the raw data), obstacles and terrain data - you can see the video which is embedded into the iPython notebook in this GitHub repo.

### Autonomous Navigation and Mapping

First step was to migrate the functions developed in the iPython notebook over to the perception() python file, the only difference is I made was to include an if statement to not update the worldmap if roll or pitch was above 1 (I plucked this number out of the air) this has ensured that the fidelity has stayed high whilst mapping (when the rover was rolling or pitching the image transform would have been incorrect and overall lowered the fidelity).

I have struggled with the decision.py file/function and have had little success in updating to be able to pick up the rocks - although I’m successfully finding them I’m not sure how to navigate the rover close enough to pickup the rocks. I modified a few of the variables as part of the drive_rover.py file making the acceleration a little slower and slowing down further out from obstacles.

I have three issues with the rover I’m attempting to resolve (post project submission), on occasion the rover will go into a loop where the only terrain it believes it can navigate to causes it to go into an endless loop - my idea to resolve this is potentially a check to see how long the rover has been 'steering' for and to a break. Secondly on the small obstacles (rock formations) due to it being so low to the ground the rover perception function does not detect and attempts to roll over (sometimes unsuccessfully), I believe this is due to the bottom offset in the transform however I have no ideas at this stage on how to resolve.

My last issue is the occasional 'ping pong' effect where moving down more narrow areas of the map the rover will ping pong between the walls, I'm thinking of adding a smoothing mechanism or slight damper when it attempts to change angle

#### Notes

My rover sim was run at a res of 800 x 600 on Good Quality

