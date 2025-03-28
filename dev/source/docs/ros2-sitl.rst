.. _ros2-sitl:

===============
ROS 2 with SITL
===============

Once ROS2 is correctly :ref:`installed <ros2>`, and SITL is also :ref:`installed <sitl-simulator-software-in-the-loop>`, source your workspace and launch ArduPilot SITL with ROS 2!

You will need to run this command on every new shell you open to have access to the ROS 2 commands, like so:

.. tabs::

   .. group-tab:: ArduPilot 4.5

      .. code-block:: bash

        source /opt/ros/humble/setup.bash
        cd ~/ardu_ws/
        colcon build --packages-up-to ardupilot_sitl
        source install/setup.bash
        ros2 launch ardupilot_sitl sitl_dds_udp.launch.py transport:=udp4 refs:=$(ros2 pkg prefix ardupilot_sitl)/share/ardupilot_sitl/config/dds_xrce_profile.xml synthetic_clock:=True wipe:=False model:=quad speedup:=1 slave:=0 instance:=0 defaults:=$(ros2 pkg prefix ardupilot_sitl)/share/ardupilot_sitl/config/default_params/copter.parm,$(ros2 pkg prefix ardupilot_sitl)/share/ardupilot_sitl/config/default_params/dds_udp.parm sim_address:=127.0.0.1 master:=tcp:127.0.0.1:5760 sitl:=127.0.0.1:5501


   .. group-tab:: ArduPilot 4.6 and later

      .. code-block:: bash

        source /opt/ros/humble/setup.bash
        cd ~/ardu_ws/
        colcon build --packages-up-to ardupilot_sitl
        source install/setup.bash
        ros2 launch ardupilot_sitl sitl_dds_udp.launch.py transport:=udp4 synthetic_clock:=True wipe:=False model:=quad speedup:=1 slave:=0 instance:=0 defaults:=$(ros2 pkg prefix ardupilot_sitl)/share/ardupilot_sitl/config/default_params/copter.parm,$(ros2 pkg prefix ardupilot_sitl)/share/ardupilot_sitl/config/default_params/dds_udp.parm sim_address:=127.0.0.1 master:=tcp:127.0.0.1:5760 sitl:=127.0.0.1:5501



For more information refer to `ardupilot/Tools/ros2/README.md <https://github.com/ArduPilot/ardupilot/tree/master/Tools/ros2#readme>`__.
There you can find examples of launches using serial connection instead of udp, as well as a step-by-step breakdown of what the launch files are doing.

ROS 2 CLI
=========

Once everything is running, you can now interact with ArduPilot through the ROS 2 CLI.

.. code-block:: bash

    source ~/ardu_ws/install/setup.bash
    # See the node appear in the ROS graph
    ros2 node list
    # See which topics are exposed by the node
    ros2 node info /ap
    # Echo a topic published from ArduPilot
    ros2 topic echo /ap/geopose/filtered

If the ROS 2 topics aren't being published, ensure the ardupilot parameter ref:`DDS_ENABLE<DDS_ENABLE>` is set to 1 and reboot the launch.

.. code-block:: bash

    export PATH=$PATH:~/ardu_ws/src/ardupilot/Tools/autotest
    sim_vehicle.py -w -v ArduPlane --console -DG --enable-dds

    param set DDS_ENABLE 1


Another aspect to check, ensure the ArduPilot parameter ref:`DDS_DOMAIN_ID<DDS_DOMAIN_ID>` matches your enviornment variable ``ROS_DOMAIN_ID``.
The default is ``0`` for ArduPilot, which corresponds to the environment variable being unset.

MAVProxy
========

To test and fly around, you can launch a `mavproxy <https://ardupilot.org/dev/docs/copter-sitl-mavproxy-tutorial.html>`__ instance in yet another terminal:

.. code-block:: bash
    
    mavproxy.py --console --map --aircraft test --master=:14550


Next up
=======

Add Gazebo in :ref:`ROS 2 with Gazebo <ros2-gazebo>`
