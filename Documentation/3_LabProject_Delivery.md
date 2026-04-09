# **REPORT Practice 1: GEB Projects tools**
## ***Group 3: David Bonillo, Adrià Francès, Francisca García***

The first part of the practice consisted on getting familiarized with GitHub and Visual Studio Code, which would syncronize the changes made on the files with the repository. Then, a simple exercise was performed: to blink a LED using an ESP32 board programmed with PlatformIO and the Arduino framework inside Visual Studio Code. A crucial step for this activity was to verify that the pin of the LED was correctly specified in the source code. Once familiarised with VSC, GitHub and the basic devices, we proceeded with a practical case. 

### 3D orientation in space
----
#### HARDWARE SET-UP
The case example consisted of using an ESP32 (IP:192.168.1.32) based PCB board with an IMU (Inertial Mass Unit) sensor to obtain 3D orientation in a 3D space. The IMU integrates three sensors (a gyroscope to measure the angular speed, an accelerometer to measure the linear accelaration and a compass to have a reference with the Earth's magnetic North Pole) and a DMP microcontroller to process data and obtain its proper RPY (Roll-Pitch-Yaw) 3-D orientation.   

The next step was to build up the Hardware-Software setup. The **hardware setup** was based on a "Robotics_UB" router to assign a fixed IP address to each module and the hardware modules (the endo-module board with an ESP32 and the PC control with roboDK program and python scripts). 

#### SOFTWARE SETUP AND 3D VISUALIZATION 
On the other hand, the **software setup** was based on an Arduino program for the endo-module to obtain the 3D orientation, a RoboDK virtual environment to visualise a 3D object orientation in that space, and Python script programs to read data. Once the system was build, we were asked to complete a series of tasks. 

Firstly, we connected the Hardware setup. Then, we uploaded the Arduino program (`Endowrist_IMU`) to the endo-module using PlatformIO, being aware to write the proper IP address of the endo-module and PC of our group. Since our equipment was number 3, we had to change some numbers in the code, such as `deviceId = "G**3**_Endo"` or `IPAddress receiverComputerIP(192, 168, 1, **3**5)`; otherwise, we would not recieve any data. At this step we ran the RoboDK virtual environment (`3D_Orientation.rdk`) to visualize two virtual objects: a plane and a surgical needle. In order to visualise one object or the other, we had to make a modification to the file `Receive_data_RPY_IMU_world.py` by naming the variable `object_NAME` either *plane* or *surgical_needle*. 

#### CORRECTION OF THE ORIENTATION BUG
Finally, we executed the `Receive_data_RPY_IMU_world.py` python program, which read the data from the endo-module and send it to the 3D-orientation object simulated in the virtual environment. When rotating the endo-module through roll, pitch and yaw movements, we realised that the objects were not moving properly. Specifically, the **Yaw** movement around the *Z-axis* was correct but nevertheless, the **Pitch** (around *Y-axis*) and **Roll** movements (around the *X-axis*) seemed to be inverted. 

Analysing the code in the `Receive_data_RPY_IMU_world.py` file, we found an error in line **126**. Here, the extrinsic ZYX axes registered from the IMU are defined. In other words, the changes in the 3D orientation measured by the IMU are translated into a rotation matrix that can then be applied to the position vector of the needle, making it rotate accordingly. Originally, line 126 looks:

`R_imu = rotz(y)*roty(p)*rotx(r)`

[▶️ Watch Short: 3D orientation in space of an IMU](https://www.youtube.com/shorts/GLxBDzUgLlo)

As we can see in the video, by physically rotating the IMU and ESP32 around one axis, we can check if the visual representation is accurate and moves along the same axis too. However, we observe that the x-axis in the simulation is facing the y-axis of the sensor, while the y-axis of the simulation faces the negative part of the sensor's x-axis (they are rotated 90 degrees clockwise, so the airplane faces its right). In the previous formula, the last two terms of the multiplication above are incorrect, given that "p" (*pitch*) and "r" (*roll*) are swapped. This is due to the fact that *Pitch, Roll and Yaw* movements are defined as:
+ **Pitch:** Rotation around the x axis. 
+ **Roll:** Rotation arounf the y axis. 
+ **Yaw:** Rotation around the z axis.


By rewriting this line 126 as: `R_imu = rotz(y)*roty(r)*rotx(p)`, we correct this mistake. 

### CONCLUSIONS
----
In this session, we have deepened our knowledge of GitHub by visualising the various documents within the repository and utilising them on the Visual Studio Code platform. By making the necessary changes, we have successfully integrated hardware and software to achieve 3D visualisation in the RoboDK tool for multiple objects. The IMU successfully managed to transfer the physical changes suffered by the sensor due to our mechanical manipulation to the visualization program, although initially the axes were not well aligned. In addition, we can observe that, although the needle could be considered a simpler geometric object, the movements in the simulation were seen as more abrupt than in the plane, making 90-degree turns. This is probably due to the symmetry of the object in the simulation and the gimbal lock effect, but we think a more realistic representation of the movement could be achieved with certain corrections to the code.


In addition to using GitHub to carry out the above tasks, we believe that GitHub is a powerful tool for future academic projects, such as a TFG. It helps to facilitate the organisation between the members of a team or with the tutor, since the permissions of each one can be modified, the changes are shown in an organised way and can be easily reviewed and suggested modifications. Repositories can be made public, thus helping other researchers learn about new projects, promoting collaboration and learning in the global community. It can also help reduce costs since it integrates different types of tools that perhaps otherwise would have to be used separately. Finally, we want to highlight the effectiveness of GitHub in our field, biomedical engineering, in which working with different types of files, such as literature, code, or 3D representations, is very common and crucial for satisfactory project development. However, the difficulty that new users have to familiarise themselves with the platform, merge conflicts and the wide range of teamwork tools (such as Microsoft Teams or the Google Workspace ecosystem), make us think that they are the reason why GitHub is not yet so well known and implemented among students and professionals in the field of biomedical engineering.
