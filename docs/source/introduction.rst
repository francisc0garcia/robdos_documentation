Introduction
============

What is Robdos?
^^^^^^^^^^^^^^^

Robdos Team, an underwater robotics association, came up in 2014 from our concern as a
group of students specialized in different fields about putting into practice all the 
skills and knowledge gathered during our university period. Marine, Industrail and Computer
Engineering students from the Technical University of Madrid (Universidad Politecnica de 
Madrid), work hard on the development of an autonomous underwater platform.

Our main goal in the Association is to manufacture and develop an autonomous vehicle in order 
to attend international competitions, which are becoming more and more frequent over the years. 
Such events allow different universities and private teams show the progress made in their 
platforms throughout the years, and thus YUooperating with technological development in underwater 
robotics.


Install ROS and dependencies
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Follow ros installation procedure:

http://wiki.ros.org/kinetic/Installation/Ubuntu

we can summarize the steps:

Open a command windows on ubuntu and run the following commands:

- Prepare ubuntu for installation:

.. code-block:: none

    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
    sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
    sudo apt-get update

- Install ROS

For PC/Laptop we should install full desktop version:

.. code-block:: none

    sudo apt-get install ros-kinetic-desktop-full

For Raspberry PI 2 and Odroid we can install ROS-Base:

.. code-block:: none

    sudo apt-get install ros-kinetic-ros-base

Install rosdep:

.. code-block:: none

    sudo rosdep init
    rosdep update

And finally prepare environment:

.. code-block:: none

    echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
    source ~/.bashrc

Compile project
^^^^^^^^^^^^^^^

First we need to install some dependencies and ROS packages:

.. code-block:: none

    sudo apt-get install libqwt-dev ros-kinetic-teleop-twist-joy  ros-kinetic-rviz-imu-plugin python-smbus ros-kinetic-rqt-multiplot ros-kinetic-mavros*


Finally we create a workspace for project, clone github repository, install dependencies and compile it:

.. code-block:: none

    mkdir -p ~/catkin_ws/src
    cd ~/catkin_ws/src
    git clone https://gitlab.com/SIMULACION_SUBMARINA/robdos_sim -b kinetic
    cd ..
    source devel/setup.bash
    rosdep install robdos_sim
    catkin_make


Test the project
^^^^^^^^^^^^^^^^

Once the project has been compiled successfully,
we have two different alternatives. can run a simulation that includes robot using a simple scenario.

.. code-block:: none

    cd ~/catkin_ws
    source devel/setup.bash
    roslaunch robdos_sim robdos_sim.launch


Launch the project
^^^^^^^^^^^^^^^^
Once we have compiled and tested the project, we can launch the project in our computer or in the robot.

The first thing we must do is to connect a PS3 Sixaxis to out computer.

Then, if we want to launch it in our computer:

.. code-block:: none

    cd ~/catkin_ws
    catkin_make_isolated
    source devel/setup.bash
    roscore
    rosparam set joy_node/dev "/dev/input/js1"
    rosrun joy joy_node
    roslaunch robdos_sim robdos_state_machine.launch

On the other hand, if you want to control the robot, the first thing we must do is to connect our computer 
and the NUC that is on the AUV to the same network.

Secondly, we have to add the next lines into the file ~/.bashrc:

.. code-block:: none
    
    export ROS_IP=192.168.1.2
    export ROS_MASTER_URI=http://192.168.1.101:11311/ 

Then, we must follow the next commands:

.. code-block:: none

    #Our computer
    ping 192.168.1.101
    ssh robdosteam@192.168.1.101
    #Contraseña: ***********

    #NUC computer
    roscore
    cd ~/robdos_ws
    source devel_isolated/setup.bash
    roslaunch robdos_state_machine on_board_architecture.launch

    #Our computer
    robdos_state_machine ground_architecture.launch

    #It is not necessary to do the next commands but it allows us to change some PixHawk values
    rosservice call /mavros/set_stream_rate 0 10 1
    rosservice call /mavros/cmd/arming 1

It is important to do the above steps in the order mentioned.
