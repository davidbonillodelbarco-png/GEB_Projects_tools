# **REPORT Practice 1: GEB Projects tools**
## ***Group 3: David Bonillo, Adrià Francès, Francisca García***

The first part of the practice consisted on getting familiarized with github and Visual Studio Code, which would syncronize the changes made on the files with the repository. Then, a simple exercise was performed: to blink a LED using an ESP32 board programmed with PlatformIO and the Arduino framework inside Visual Studio Code. A crucial step for this activity was to verify that the pin of the LED was correctly specified in the source code. Once familiarized with VSC, GitHub and the basic devices, we proceeded with a practical case. 

### 3D orientation in space
----
The case example consisted in using an ESP32 (IP:192.168.1.32) based PCB board with an IMU (Inertial Mass Unit) sensor to obtain 3D orientation in a 3D space. The IMU integrates three sensors (a gyroscope to measure the angular speed, an accelerometer to measure the linear accelaration and a compass to have a reference with the Earth's magnetic North Pole) and a DMP microcontroller to process data an obtain its proper RPY (Roll-Pitch-Yaw) 3-D orientation.   

The next step was to build up the Hardware-Software setup. The **hardware setup** was based on a "Robotics_UB" router to assign a fixed IP address to each module and the hardware modules (the endo-module board with an ESP32 and the PC control with roboDK program and python scripts). On the other hand, the **software setup** was based on an Arduino program for the endo-module to obtain the 3D orientation, a RoboDK virtual environment to visualize a 3D object orientation in that space, and Python script programs to read data. Once the system was build, we were asked to complete a series of tasks:

  1. Firstly, we connected the Harware setup
  2. Then, we uploaded the Arduino program (`Endowrist_IMU`) to the endo-module using PlatformIO, being aware to write the proper IP address of endo-module and PC of our group. Otherwise, we would not recieve any data.
  3. At this step we ran the RoboDK virtual environemt (`3D_Orientation.rdk`) to visualize two virtual objects: a plane and a surgical needle. In order to visualize one object or the other, we had to make a modification to the file `Receive_data_RPY_IMU_world.py` by naming the variable `object_NAME` either *plane* or *surgical_needle*. 
  4. Finally, we executed the `Receive_data_RPY_IMU_world.py` python program, which read the data from the endo-module and send it to the 3D-orientation object simulated in the virtual environment. Rotating the endo-module through roll, pitch and yaw movements we realized that the objects were not moving properly. Specifically, the **Yaw** movement around the *Z-axis* was correct but nevertheless the **Pitch** (around *Y-axis*) and **Roll** movements (around the *X-axis*) seemed to be inverted. 

