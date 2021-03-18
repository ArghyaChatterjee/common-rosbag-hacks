# common-rosbag-hacks
It's a personal repository to quick hack rosbag files for analysis.
## Rename a topic
1. In built feature, no chance of message loss and the data will be synced.
```
rosrun rosbag topic_renamer.py <in topic> <in bag> <out topic> <out bag>
```
2. Possible chance of 1 or 2 message loss and the messages will be slightly off sync. Inside tmux file, in one terminal:
```
roscore  
```
In another terminal:
```
rosbag play <bag name> -s$DELAY -r$RATE --clock --topics --pause clock:=/clock <topic name>
```
In another terminal: 
```
rosrun topic_tools relay <topic name> <new topic name>
```
In another terminal:
```
rosbag record -O <new bag name> <new topic name>
```
3. Possible chance of off sync as well as huge offset in time stamp. In one terminal:
```
roscore
```
In another terminal:
```
rosbag record -O <new bag name> <new topic name> 
```
In another terminal:
```
rosbag play <bag name> <topic name>:=<new topic name>
```
## Rename a frame id
Install [bag-tools](http://wiki.ros.org/bag_tools) from `srv_tools` ros repo.
```
cd ~/catkin_ws/src
git clone https://github.com/srv/srv_tools
cd ..
catkin build
```
In the terminal:
```
rosrun bag_tools change_frame_id.py -o <out bag> -i <in bag> -f <frame_id> -t <topic>
```
