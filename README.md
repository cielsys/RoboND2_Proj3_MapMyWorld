# RoboND2_Proj3_MapMyWorld 
ChrisL 2019-02-05

This repo is for personal coursework.<br/>
It is for the the "Map My World" ROS Slam Project, the third project of Udacity Robotic Engineer ND, Term2.

## Submission notes
NOTE: This repo contains more than just the Catkin source directory!
The ROS source directory is <br>
$REPO/catkin_ws/src

The project report is [report.pdf](./report/report.pdf)<br/>

Main launch invocation:
choose one of
```commandline
roslaunch ciel_bot world_main.launch world_name:=kitchen_dining.world
roslaunch ciel_bot world_main.launch world_name:=custom.world
roslaunch ciel_bot world_main.launch world_name:=jackal_race.world
```

To issue the goal command:
```commandline
rosrun ciel_bot navigation_goal
```

## Links
Submission Requirements: [Rubric](https://review.udacity.com/#!/rubrics/1441/view) 