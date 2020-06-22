# HURBA: SLAM tutorial II.

[//]: # (Image References)

[image1]: ./documentation/gazebo.png "Gazebo"
[image2]: ./documentation/map.png "Map"
[image3]: ./documentation/hector.png "Hector SLAM"
[image4]: ./documentation/gmapping.png "GMapping"
[image5]: ./documentation/rtabmap.png "RTAB-Map"
[image6]: ./documentation/rtabmapviz.png "Rtabmapviz"
[image7]: ./documentation/database.png "Database"

### Dependencies:
- [ROS Melodic](http://wiki.ros.org/melodic "ROS Melodic")
- [Gazebo 9](http://wiki.ros.org/gazebo_ros_pkgs "Gazebo ROS package")
- [RViz](http://wiki.ros.org/rviz "RViz")
- [Teleop twist keyboard](http://wiki.ros.org/teleop_twist_keyboard "Teleop twist keyboard")
- [Hector SLAM](http://wiki.ros.org/hector_slam "Hector SLAM")
- [GMapping](http://wiki.ros.org/gmapping "GMapping")
- [RTAB-Map](http://wiki.ros.org/rtabmap_ros "RTAB-Map")
- [AMCL](http://wiki.ros.org/amcl "AMCL")
- [Map server](http://wiki.ros.org/map_server "Map server")
- [PGM Map Creator](https://github.com/dudasdavid/pgm_map_creator/tree/hurba-curriculum "PGM Map Creator")

### Project build instrctions:
1. Clone this repo inside the `src` folder of a catkin workspace with the PGM Map Creator submodule:
`git clone --recurse-submodules https://github.com/hungarianrobot/Project-2-Mapping-Localization`
2. Install `libignition-math2-dev` and `protobuf-compiler` to compile the map creator:
`sudo apt-get install libignition-math2-dev protobuf-compiler`
3. Build workspace: `catkin_make`
4. Source environment: `source devel/setup.bash` 

### Test the simulation
1. Start the Gazebo simulation: `roslaunch hurba_mapping world.launch`
2. Start the teleop package: `roslaunch hurba_mapping teleop.launch`
3. Start RViz and open the `basic_view.rviz` configuration.
4. Drive the robot inside the simulated environment. Note: this time the robot is  omnidirectional instead of the previous diff drive!

![alt text][image1]

### Save the ground truth map
1) Successfully build PGM Map Creator.
2) Copy the `building.world` Gazebo world file into `src/pgm_map_creator/world/` folder.
3) Add `<plugin filename="libcollision_map_creator.so" name="collision_map_creator"/>` before the `</world>` tag.
4) Run Gazebo server with: `gzserver src/pgm_map_creator/world/building.world`.
5) Save the map with PGM Map Creator: `roslaunch pgm_map_creator request_publisher.launch`.
6) Copy the saved `map.pgm` to maps folder of the `my_robot` package and create a `map.yaml` with default values (see `hurba_mapping/maps` folder).

![alt text][image2]

### Hector SLAM
1. Start the Hector SLAM: `roslaunch hurba_mapping hector_slam.launch`
2. Start the teleop package: `roslaunch hurba_mapping teleop.launch`

![alt text][image3]

### GMapping
1. Start the GMapping package: `roslaunch hurba_mapping gmapping_slam.launch`
2. Start the teleop package: `roslaunch hurba_mapping teleop.launch`

![alt text][image4]

### RTAB-Map
1. Start the RTAB-Map package: `roslaunch hurba_mapping rtab_map_slam.launch`
2. Start the teleop package: `roslaunch hurba_mapping teleop.launch`

![alt text][image5]

#### RTAB-Map Viz
Un-comment the `rtabmapviz` node in the `rtab_map_slam.launch` file. In the visualizer tool of RTAB-Map we can see the RGBD pointcloud of the environment:

![alt text][image6]

#### Evaluating the RTAB-Map database
After the mapping is done we can evaluate the database with RTAB-Map's database viewer, that can be started with the following command:

`rtabmap-databaseViewer ~/catkin_ws/src/Project-2-Mapping-Localization/hurba_mapping/maps/database/rtabmap.db`

![alt text][image7]

Once open, we will need to add some windows to get a better view of the relevant information, so:

* Say yes to using the database parameters
* View -> Constraint View
* View -> Graph View

We can check the number of loop closures during the mapping using the bottom left information:

`(283, 0, 29, 0, 0, 0, 0, 0, 0) Links(N, NM, G, LS, LT, U, P, LM, GR)`

Where G means the number of Global Loop Closures. The codes stand for the following: `Neighbor`, `Neighbor Merged`, `Global Loop closure`, `Local loop closure by space`, `Local loop closure by time`, `User loop closure`, and `Prior link`.


### Project structure:
```bash
 tree -L 3

├── README.md
├── documentation
│   ├── database.png
│   ├── frames.png
│   ├── gazebo.png
│   ├── gmapping.png
│   ├── hector.png
│   ├── rosgraph.png
│   ├── rtabmap.png
│   ├── rtabmapviz.png
│   └── rviz.png
├── hurba_mapping
│   ├── CMakeLists.txt
│   ├── launch
│   │   ├── amcl_localization.launch
│   │   ├── gmapping_slam.launch
│   │   ├── hector_slam.launch
│   │   ├── robot_description.launch
│   │   ├── rtab_map_slam.launch
│   │   ├── teleop.launch
│   │   └── world.launch
│   ├── maps
│   │   ├── database
│   │   ├── map_hector.yaml
│   │   ├── map.pgm
│   │   └── map.yaml
│   ├── meshes
│   │   ├── chassis.dae
│   │   ├── chassis.SLDPRT
│   │   ├── chassis.STEP
│   │   ├── hokuyo.dae
│   │   ├── wheel.dae
│   │   ├── wheel.SLDPRT
│   │   └── wheel.STEP
│   ├── package.xml
│   ├── rviz
│   │   ├── amcl.rviz
│   │   ├── basic_view.rviz
│   │   ├── gmapping_slam.rviz
│   │   ├── hector_slam.rviz
│   │   └── rtab_slam.rviz
│   ├── urdf
│   │   ├── hurba_mecanum.gazebo
│   │   └── hurba_mecanum.xacro
│   └── worlds
│       ├── basic_world.world
│       ├── building.model
│       ├── building.world
│       └── empty.world
└── pgm_map_creator
    ├── CMakeLists.txt
    ├── launch
    │   └── request_publisher.launch
    ├── LICENSE
    ├── maps
    │   ├── example_map.pgm
    │   └── map.pgm
    ├── msgs
    │   ├── CMakeLists.txt
    │   └── collision_map_request.proto
    ├── package.xml
    ├── README.md
    ├── src
    │   ├── collision_map_creator.cc
    │   └── request_publisher.cc
    └── world
        ├── building.world
        ├── MyWorld.world
        └── udacity_mtv

```

