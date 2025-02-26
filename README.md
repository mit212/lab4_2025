# Lab 4: UR5 and Motor Control

2.12/2.120 Intro to Robotics  
Spring 2025[^1]

- [Lab 4: UR5 and Motor Control](#lab-4-ur5-and-motor-control)
  - [0 (Prelab) Software Setup](#0-prelab-software-setup)
    - [0.1 Python](#01-python)
    - [0.2 UR5 RTDE](#02-ur5-rtde)
  - [1 Motor Modelling and Controller Design](#1-motor-modelling-and-controller-design)
  - [2 Validate Hardware Setup](#2-validate-hardware-setup)
    - [2.1 Validate Microcontroller](#21-validate-microcontroller)
    - [2.2 Validate Motors](#22-validate-motors)
    - [2.3 Validate Encoders](#23-validate-encoders)
    - [2.4 Validate Joystick](#24-validate-joystick)
  - [3 Tune Controller](#3-tune-controller)
  - [4 Drawing Revisited](#4-drawing-revisited)
  - [5 Feedback Form](#5-feedback-form)
  - [X Optional](#x-optional)

## 0 (Prelab) Software Setup

Estimated time of completion: 15 min

Before coming in to lab, please download and install the following software.

### 0.1 Python 

1. Download Python here: https://www.python.org/downloads/.
    <details>
    <summary><i> What version of Python do I need? </i></summary>

    We recommend at least 3.8 to ensure compatibility with the packages we will use in this class. If you already have Python, you should be able to check its version by entering the command `python --version` in your terminal. 

    </details>

    <details>
    <summary><i> Can I get Python via conda? </i></summary>

    We recommend getting vanilla Python. The staff may not be able to help troubleshoot issues relating to `conda`.

    </details>

2. Make sure to check "Add python.exe to PATH".
  
    <p align="center">
    <img src="./.images/Python_install.png" alt="ik" width="400"/>
    </p>

3. Click "Install Now" and finish the installation.
4. Check that you installed Python correctly by entering the command `python` in your terminal. It should return `Python X.X (tags...`. 
    <details>
    <summary><i> It's returning something else? </i></summary>

    If it instead returns `python is not recognized as an internal or external command, operable program, or batch file` or `python: command not found`, you may have forgotten to add Python to PATH during installation. You can fix this using the following instructions: [How to Add Python to PATH](https://realpython.com/add-python-to-path/).

    </details>
5. Install pip:

    **Windows/Mac**
    - Click on this link: https://bootstrap.pypa.io/get-pip.py
    - Save the file (CTRL + s) to your computer
    - Open a terminal/command prompt and navigate to the same directory as the saved script (e.g. via `cd "C:\Users\*your username*\Downloads"` if you saved it in your downloads folder)
    - Run `python get-pip.py`
    
    **Linux**
    - Enter the following sequentailly:
        ```
        sudo apt update
        sudo apt upgrade
        sudo apt install python3-pip
        ```

6. Install the RTDE library by followiing the steps here: [ur_rtde Installation](https://sdurobotics.gitlab.io/ur_rtde/installation/installation.html). It might work for some Mac or Windows machines to simply run `pip install ur_rtde`, but if not follow the instructions from the link. If the Windows installation feels complex for you, look at section 0.2 below.

### 0.2 (Recommended for Windows Machines) Windows Subsystem for Linux (WSL)

**If you have a Windows machine, it is much easier to install ur_rtde through the Linux virtual environment for Windows (WSL). This is OPTIONAL.**

For Windows: 
1. Open PowerShell in administrator mode by right-clicking and selecting "Run as administrator".
2. Enter `wsl --install`. This installs Windows Subsystem for Linux (WSL) with Ubuntu as the default distribution.
   
    <details>
    <summary><i> Not working? </i></summary>

    Please refer to this tutorial: [How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install.).
    </details>

3. Restart your machine.
4. Open Powershell and enter `wsl` to open a WSL terminal window. If Ubuntu hasn't been installed already, Windows will install it in another terminal window. Wait until after this is completed and type `wsl` into PowerShell again. You might be asked to create a username and password. This can be whatever you want as long as you remember it.
5. Enter the following sequentially:
    ```
    sudo apt update
    sudo apt upgrade
    sudo apt install python3-pip
    pip3 install ur_rtde --break-system-packages
    ```
    Tip: You can paste text from your clipboard by right-clicking in Powershell.
   
Additional references:
- [Github (Example Codes)](https://github.com/mit212/ur_2024)
- [RTDE API Doc](https://sdurobotics.gitlab.io/ur_rtde/)

In lab, we will perform a supervised run-through of controlling the UR5.

## 1 Motor Modelling and Controller Design

Estimated time of completion: 20 min

Clone this repository. Open `src/matlab/MotorModel.mlx` in MATLAB and follow along the guided tutorial to find the nominal controller gains for the motors.

You can find information about the motor here: https://www.gobilda.com/modern-robotics-12vdc-motor/

## 2 Validate Hardware Setup
Estimated time of completion: 10 min

Like in the previous labs, we first want to make sure the parts work individually! Faulty wiring or hardware can be very difficult to debug in complex systems.

### 2.1 Validate Microcontroller

**Make sure that motor power is turned off any time you are uploading code to your microcontroller.** The arm has a tendency to spin around and hit itself if motor power is on during upload. As a reminder, motor power should only be on when you expect the motors to move. Otherwise, please keep motor power off. 

<details> <summary> <i> How to know if the motor power is on? </i> </summary>

The green LED on the motor driver indicates whether motor power is on or off.
</details>

Clone this repository and run `robot/blink_test.cpp`. You should see the onboard LED change colors! 

<details> <summary> <i> Forget how to clone? </i> </summary>

Please refer to the [instructions from Lab 1](
https://github.com/mit212/lab1_2025?tab=readme-ov-file#31-git-clone).

</details>

### 2.2 Validate Motors

Orient the arm straight up, in the default starting position. Run `test_code/motor_drive_test.cpp` to validate your motor setup. Set the power supply to 12 volts (also for the rest of the lab). You should see both motors oscillating back and forth. Remember, motor 1 is attached to the base and motor 2 is attached to the second link.

**Perturb the arm and base links by hand to check if all screws are tight.** If you notice any excessive slop in the system (e.g. a link can be moved by hand more than 2-3 degrees without engaging the motors), notify the TAs, and we will provide you with a hex key to tighten the bolts.

### 2.3 Validate Encoders

Run `test_code/encoder_test.cpp` to validate your encoder setup. Open the Serial Monitor to see the output and confirm that both the direction and the magnitude make sense!

### 2.4 Validate Joystick

Run `test_code/joystick_test.cpp` to validate your joystick setup. You should be able to see joystick readings within the range `[-1, 1]`.

## 3 Tune Controller
Estimated time of completion: 20 min

We will be using `lab_code/sinusoidal_input.cpp` to tune our controllers. We will tune the two motors separately.

1. Uncomment `#define MOTOR1` and comment out `#define MOTOR2`.
2. Update the parameters under the `#ifdef MOTOR1` section to be the nominal parameters computed from MATLAB.
3. Run `lab_code/sinusoidal_input.cpp`.
4. Open `matlab/TuningPlot.m` in MATLAB. Update the COM port.
5. Run `matlab/TuningPlot.m` in MATLAB to visualize the performance of your controller. 
6. Tune your controller by incrementally changing `Ti1`, `Td1`, `Kp1`, and `alpha1` and seeing how that affects performance. Ideally, we want minimal overshoot, oscillations, and steady state error.

We are using a lead-lag compensator as opposed to a PID controller. Remember: for a transfer function, the frequency response for an angular frequency `w` can be found by substituting `s` with `iw`. You can think of the lag component as implementing integral control, since it places a pole at the origin. Similarly, the lead component acts as a derivative term, but when the frequency is very high (`w = ∞`), the gain approaches `alpha` instead of beoming infinite.

Looking at the transfer functions for each, how do the standard PID gains `Kp`, `Ki`, and `Kd` reflect to the lead-lag compensator gains `Kp`, `Ti`, `Td`, and `alpha`? Specifically, if we want to increase the integral gain `Ki`, do we increase or decrease `Ti`? How about for the derivative gain `Kd` - how does it affect `Td`?

<p align="center">
    <img src="./.images/lead_lag_compensator.png" alt="ik" width="300"/>
    </p>

<p align="center">
    <img src="./.images/PID_controller.png" alt="ik" width="300"/>
    </p>

7. Once you are satisfied with the performance, repeat the process for `MOTOR2`. 

## 4 Drawing Revisited
Estimated time of completion: 10 min

We will now use your tuned controllers to improve the drawing from last lab!

1. Open `lab_code/drawing.cpp` and update the parameters.
2. Set `trajectoryType = HORIZONTAL_LINE`.
3. Run `lab_code/drawing.cpp` and observe whether the arm performs better than last lab.
4. Open `src/matlab/TrajectoryPlot.m` in MATLAB. Update the COM port.
5. Run `src/matlab/TrajectoryPlot.m` to visualize the target and actual trajectories.
6. After putting the arm in the home position, set `trajectoryType = JOYSTICK` and run `lab_code/drawing.cpp` again.
7. Draw something awesome using your joystick!

| :white_check_mark: CHECKOFF 1 :white_check_mark:   |
|:---------------------------------------------------|
| Show your work of art to a TA or LA so we can pin it up on the board! |

## X Optional (eXtra credit!)

Here are some optional challenges you can try if you finish lab early!
1. Run `lab_code/drawing.cpp` with `trajectoryType = VERTICAL_LINE`.
2. Run `lab_code/drawing.cpp` with `trajectoryType = CIRCLE`.
3. Explain to the staff how a lead-lag compensator works, why we use it instead of a PID controller, and the what the gain/phase response is for each component (a sketched Bode plot would be nice).

[^1]: Version 1 - 2020: Dr. Harrison Chin  
  Version 2 - 2021: Phillip Daniel  
  Version 3 - 2023: Ravi Tejwani and Kentaro Barhydt  
  Version 4 - 2024: Joseph Ntaimo, Jinger Chong, Josh Sohn  
  Version 5 - 2025: Roberto Bolli Jr., Kaleb Blake
