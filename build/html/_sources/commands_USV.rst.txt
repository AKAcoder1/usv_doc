Operating the Marine X
=======================

This guide provides a detailed overview of the terminal commands required to operate the Marine X Unmanned Surface Vessel (USV).


Connecting to the Raspberry and TX2: 
+++++++++++++++++++++++++++++++++++++
    1. Ensure the USV is powered on.
    2. Connect to the ASUS router (KU_pool_c00058):
        - Access the router administration page: http://router.asus.com/Main_Login.asp
        - Input the following credentials: username: admin & password: KU_Marine_X
    3. You can now view the IP addresses of both the Raspberry Pi and TX2 by clicking on the view list.
    4. Open a terminal and type ``roscore``

.. note::
       The IPs for both are statics however check the client IP just in case  

.. image :: images/client_ip.png
.. note::
     The TX2 is labeled as ``mbzirc-humais`` while the Raspberry Pi with the Navio2 is named ``navio``.    

Access Remote Control 
+++++++++++++++++++++
1. SSH to the Navio ``password: raspberry``:
--------------------------------------------

.. code-block:: sh

   ssh pi@192.168.50.145

2. Modify the ROS master and input your IP address:
---------------------------------------------------

.. code-block:: sh

   sudo nano .bashrc

.. image :: images/ROS_Master.png

.. note::
       Only change the ROS_MASTER_URI while keeping the port address (.11311).

.. code-block:: sh

   source .bashrc

.. note::
     At this stage, you should already have a terminal running ``roscore``


3. Run the USV firmware 
--------------------------------------------
 - Turn on futaba RC controller on and make sure the left knob is inialized by being all the way down
 - source the package then run the firmware:

  .. code-block:: sh

   source catkin_ws/devel/setup.bash 

Run the firmware:

 .. code-block:: sh

   rosrun usv_firmware usv_node

.. image :: images/RC_firmware.png

4. Calibrate the RC controller
------------------------------
 - Open new terminal

.. code-block:: sh

    rosservice call /calibrate_rc "data: true" 

.. note::
       Move the right knob in a circule 

.. code-block:: sh

    rosservice call /calibrate_rc "data: false"

.. note::
       The USV_firmware terminal should display a "calibration successful" message.

.. image :: images/calibration.png

5. Activate the arm_usv
-----------------------
.. code-block:: sh

    rosservice call /arm_usv "data: true" 

.. note::
    The USV_firmware terminal should display an "arm called" message, allowing you to control the thrusters with the RC.


Access the camera through TX2
+++++++++++++++++++++++++++++

1. SSH to the TX2 ``password: mbzirc``:
---------------------------------------
.. code-block:: sh

   ssh mbzirc-usv@192.168.50.38

2. Modify the ROS master and input your IP address:
---------------------------------------------------

.. code-block:: sh

   sudo nano .bashrc

.. image :: images/ROS_MASTER_TX2.png


.. code-block:: sh

   source .bashrc

3. Lanuch the camera: 
---------------------

.. code-block:: sh

    roslaunch zed_wrapper zedm.launch

Open a new terminal and execute the following command:

.. code-block:: sh

    rqt

.. image :: images/camera_output.png


Detection and autonomous navigation
+++++++++++++++++++++++++++++++++++

1. Run the detection commands in TX2:
-------------------------------------
- You must have two TX2 terminals (so SHH twice)

Lanuch the camera node:

.. code-block:: sh

    roslaunch zed_wrapper zed_camera_nodelet.launch node_name:=zed_node

Launch the detection node:

.. code-block:: sh

    roslaunch mbz_vessel_det mbz_detection.launch camera_model:=zedm

The following warnings indicate that the nodes are working proparly: 

.. image :: images/detection_works.png


Open new terminal and check the output:

.. code-block:: sh

    rqt

.. image :: images/boat_detection.png    


2. Autonomous maneuvering
-------------------------
- ssh to the Navio and execute the following command

.. code-block:: sh

    rosrun usv_nav usv_nav_node

- In another terminal you have to enable the Autonomous service:

.. code-block:: sh

    rosservice call /enable_auto "data: true" 

- Initiate the autonomous system:

.. code-block:: sh

    rosservice call /reset_manual_override "{}"

.. warning::

   Before initiating the autonomous service you have to access to the RC controller. This precaution ensures that you can swiftly switch to manual control if necessary, in order to prevent potential collisions or damage.
