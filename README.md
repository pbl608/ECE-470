# ECE-470
Project: **Using UR3 to put trash in a bin**

**Assignment 1**

This project allows users to individually rotate the joints of a UR3 robot (manufactured by Universal Robots). The simulator we used is V-REP PRO EDU, and we used a Python API (using Anaconda) to code the robot.

**MOTION DEMO**

**1. Installing the necessary software**

*1.1* V-REP PRO EDU

We used the simulator V-Rep Pro Edu to simulate our robot. We used it in a Windows OS (64-bit). Each user may download it from the page of Coppelia Robotics (www.coppeliarobotics.com). Although we used the Windows x64 version, it is also available for Mac devices and Linux (x64) devices. V-Rep Pro Edu is similar in all the OS, so this guide may easily be adapted to other OS.

Instead of using the newest version, we decided to used V-Rep Pro Edu 3.4.0.

*1.2* Anaconda

Then, we downloaded the Python platform Anaconda. We downloaded the version 5.1, with Python 3.6. We downloaded the 64-bit version.

**2. Setting robot and object V-Rep Pro Edu**

We decided to use Universal Robots UR3 to do our project. To choose it, go to the model browser and then go to robots -> non-mobile -> UR3.ttm

We also used a cuboid, that can be added in Add -> Primitive shape -> Cuboid. 

Finally, we put (although we have not used it yet) a large basket, that can be added using the model browser: household -> largeBasket.ttm.

If the user wants to directly have the scene that we set, there is the possibility of downloading the file my_ur3.ttt, which is attached in this .zip file. Paste this file in C:\Program files\V-REP3\V-REP_PRO_EDU\scenes (if C is the hard drive).

**3. Creating folder vrep_code**

If the user is working on Windows, please note that the folder in which V-Rep Pro Edu is installed is C:\Program files\V-REP3 (if C is the hard drive). Inside that folder, the user must create another folder named vrep_code. The user will copy the next files and paste them in that folder:
_V-REP3\V-REP_PRO_EDU\programming\remoteApiBinding\python\python\vrep.py
_V-REP3\V-REP_PRO_EDU\programming\remoteApiBinding\python\python\vrepConst.py
_V-REP3\V-REP_PRO_EDU\programming\remoteApiBinding\lib\lib\64Bit\remoteApi.dll

Finally, the user will download the assig1.py file that is in this .zip file and will paste it in that folder, too.

**4. Explaining the code**

The code in the .py file moves all the joints of the robot independently, and it also uses a suction pad. The first thing the code does is connecting to V-Rep and setting all the handles for the joints. Then, the simulation starts. To move the joints independently, we used the function time.sleep(), which is used between different movements. The base moves 180º, the shoulder and the elbow move 45º and the wrist 2 moves -90º. These are the movements needed to make the suction pad touch the cuboid (to adjust it, we changed the dimensions of the cuboid to make it touch the suction pad, and also we changed the position in order to get both in touch).

Then, the suction pad is turned on, and the shoulder goes back to its original position. Then, we move the wrists 1 and 3 (because in this assigment is necessary to move all the joints). Then, the elbow also goes to its original position.

This is the end of the simulation.

**5. Running the code**

To run the code, we will use an Anaconda Prompt. Open it, and use the 'cd' command to move to V-REP3\vrep_code. There, you will run the .py file with 'python assig1.py'. Then, the user can look at V-Rep Pro Edu and see the simulation.

** FORWARD KINEMATICS **

For this part of the project, we derived the forward kinematics of the robot. To do so, we took the dimensions of the robot (they are in the lab manual). Knowing them, we derived the six joint axes.

All of them are revolute joints. We assigned the base frame as the one of the base of the robot. This means that the x-axis and y-axis intersect at the center of the base of the robot, while the z-axis, which is the vertical one, intersects with the XY plane 0.043m over the ground (this is, the origin of the frame is not on the ground). This was important because we had to notice that the dimensions are not the same as in the document (because the reference is the ground).

Knowing the direction of the screw and a point of them, we were able to derive the six elements of them.

Then, we wrote a function that takes a 6x1 vector as an input and gives a 4x4 skew-symmetric matrix as an output (this is, with the first three elements of the screw we derive a skew-symmetric matrix, which is inserted in rows 1:3, columns 1:3, and we put the last three elements in the first three elements of the last column. The last row is a row of zeros. With that function, we converted the six screws into matrices.

After that, we used the function scipy.linalg.expm to derive the exponential matrix of the skew-symmetric matrices (each one multiplied by its joint angle). Using the @ operator, we multiplied all six exponential matrices between them, by order, and then, by M, which is the initial pose and was also derived knowing the dimensions of the robot. Doing so, we got T, the final pose.

Now, we need the position and orientation. The position is easy to get: it is given by the first three elements of the last column of T. However, to get the angles, we had to derive the Euler angles (with the sequence 'xyz').



