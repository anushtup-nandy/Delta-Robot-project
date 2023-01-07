# Delta-Robot-project

This project is being done by me *individually* under the Guidance of Dr. Arshad Javed from the department of Mechanical Engineering in BITS-Pilani, Hyderabad Campus.

Materials I've used:
- Neema 17 stepper motors (3)
- 3D printed Lower arms (3)
- Carbon fibre upper arms (6)
- 3D printed end-effector
- L298N motor driver shields (3)
- Ball joints (12)
- Arduino Uno
- Jumper wires
- Bread-board

![Delta](https://github.com/anushtup-nandy/Delta-Robot-project/blob/master/Delta_img.jpg)


## Theory:

Delta robot is a type of parallel robot. Parallel robots have certain advantages over serial ones (like a robotic arm):
1. higher stiffness
2. strong bearing capacities
3. high precesion
4. fast pick-ups

Delta robots are usually ==3-DOF parallel robots==. They have the following parts:
1. 3 equal arms
2. 1 base (fixed)
3. 1 end-effector (moving)
The arms are usually attatched at 120 degrees of each other. The end-effector is attatched to the arms witha revolute joint. Due to it's parallelogram like nature, the delta robot is unable to rotate, only translate.

Delta robots are used for:
1. pick-and-place operations.

### FORWARD KINEMATICS:
We need to find the 3D coordinate: $P[x y z]$ by knowing the $\theta_{i}$'s of the arms (*typical FK*)
The 3D positional coordinates of B:

$$
B_i=\begin{bmatrix}R+L_1cos\theta_{i} \\ 0 \\ L_1sin \ theta_{i} \end{bmatrix}----(1)
$$

the rotation matrix (transformation matrix)-- **about Z**

$$
R=\begin{bmatrix}cos\alpha & -sin\alpha & 0\\ sin\alpha & cos\alpha & 0\\ 0&0&1\end{bmatrix}
$$

The installation angles of the 3 arms are:

$$
\alpha_{1}=0, \alpha_{2}=\frac{2\pi}{3}, \alpha_{2}=\frac{4\pi}{3} 
$$

using pythagoras theorem we can write the following:

$$
\begin{gather*}
(x-cos\alpha_1[(R-r)+L_{1}cos\theta_1])^2+(y-sin\alpha_1[(R-r)+L_{1}sin\theta_1])^2+(z-L_1sin\theta_1)^2=L_2^2----(2.1)\\
and\\
(x-cos\alpha_2[(R-r)+L_{1}cos\theta_2])^2+(y-sin\alpha_2[(R-r)+L_{1}sin\theta_2])^2+(z-L_1sin\theta_2)^2=L_2^2----(2.2)\\
and\\
(x-cos\alpha_3[(R-r)+L_{1}cos\theta_3])^2+(y-sin\alpha_3[(R-r)+L_{1}sin\theta_3])^2+(z-L_1sin\theta_3)^2=L_2^2----(2.3)\\
\end{gather*}
$$

in a simpler manner we can write something of the sorts of:

$$
(x-k_{ii})+(y-k_{ij})+(z-k_{ik})=L_2^2----(3)
$$

where $k$ will be a matrix containing the simplified terms of the equations (2.1-2.3)

$$
k=\begin{bmatrix}
(R-r)+L_1cos\theta_{1}&0&-L_1sin\theta_1\\
0.5[(R-r)+L_1cos\theta_{2}]&{\frac{-\sqrt{3}}{2}[(R-r)+L_1cos\theta_{2}]}&-L_1sin\theta_2\\
0.5[(R-r)+L_1cos\theta_{3}]&{\frac{\sqrt{3}}{2}[(R-r)+L_1cos\theta_{3}]}&-L_1sin\theta_3
\end{bmatrix}
$$

Eqn (1) is substracted from eqns (2) and (3) (==why??==) and then merged and [[MATRIX DECOMPOSITION]].

$$
\begin{gather*}
a_1x+b_1y+c_1z=d_1\\
a_2x+b_2y+c_2z=d_2----(4)
\end{gather*}
$$

This in turn can also be written as:

$$
\begin{bmatrix} a_{1}&b_{1} \\ a_{2}&b_2 \end{bmatrix} \begin{bmatrix} x\\ y \end{bmatrix} = \begin{bmatrix}d_1-c_1z \\ d_2-c_2z \end{bmatrix}----(5)
$$

Solve this linear equation for x and y. 

$$
\begin{gather*}
x=f_1+f_xz \\
y=f2+f_yz----(6)
\end{gather*}
$$

### Inverse Kinematics analysis:
For a given delta robot, the vector loop closure equations are:

$$
{B^B_i}+L^B_i+I^B_i=P^B_P+[R^B_P]P^P_i=P^B_P+P^B_i
$$

where:

$$
\begin{gather*}
R=I_3 \\
B_i={\text{the revolute joints of the fixed base}} \\
P_i={\text{connection joints (vertices of the equilateral triangle of the base and end-effector)}}
\end{gather*}

$$

The basic IPK solution for the 3 legs are modelled as:

$$
E_{i}{cos\theta_i}+F_{i}sin\theta_{i}+G_i=0
$$

where:

$$
\begin{gather*}
E_1=2L(y+a) \\
F_1=2zL \\
G_1=x^2+y^2+z^2+a^2+L^2+2ya-l^2 \\
E_2=-L(\sqrt3(x+b)+y+c) \\
F_2=2zL \\
G_2=x^2+y^2+z^2+b^2+c^2+L^2+2(xb+yc)-l^2 \\
E_3=L(\sqrt3(x+b)-y-c) \\
F_3=2zL \\
G_3=x^2+y^2+z^2+b^2+c^2+L^2+2(-xb+yc)-l^2 \\
\end{gather*}
$$

then we put these individually in the equation and then finally solve using *tangent half angle substitution.*


## Papers Read:

[1] O. Kalayci, I. Pehlivan, A. Akgul, S. Coskun, and E. Kurt, “A New Chaotic Mixer Design Based on the Delta Robot and Its Experimental Studies,” Mathematical Problems in Engineering, vol. 2021, p. e6615856, Mar. 2021, doi: 10.1155/2021/6615856.

[2] C. E. Boudjedir and D. Boukhetala, “Adaptive robust iterative learning control with application to a Delta robot,” Proceedings of the Institution of Mechanical Engineers, Part I: Journal of Systems and Control Engineering, vol. 235, no. 2, pp. 207–221, Feb. 2021, doi: 10.1177/0959651820938531.

[3] E. Rodriguez, A. J. Alvares, and C. I. Jaimes, “Conceptual design and dimensional optimization of the linear delta robot with single legs for additive manufacturing,” Proceedings of the Institution of Mechanical Engineers, Part I: Journal of Systems and Control Engineering, vol. 233, no. 7, pp. 855–869, Aug. 2019, doi: 10.1177/0959651819836915.

[4] M. López, E. Castillo, G. García, and A. Bashir, “Delta robot: Inverse, direct, and intermediate Jacobians,” Proceedings of the Institution of Mechanical Engineers, Part C: Journal of Mechanical Engineering Science, vol. 220, no. 1, pp. 103–109, Jan. 2006, doi: 10.1243/095440606X78263.

[5] A. V. Zrazhevskiy, A. V. Mikhailov, A. S. Zalomskii, V. I. Kononenko, and D. A. Sukmanov, “Improving the accuracy of designing a delta robot for 3D printing,” J. Phys.: Conf. Ser., vol. 2094, no. 4, p. 042058, Nov. 2021, doi: 10.1088/1742-6596/2094/4/042058.

[6] A. Gholami, T. Homayouni, R. Ehsani, and J.-Q. Sun, “Inverse Kinematic Control of a Delta Robot Using Neural Networks in Real-Time,” Robotics, vol. 10, no. 4, Art. no. 4, Dec. 2021, doi: 10.3390/robotics10040115.

[7] C. E. Boudjedir, M. Bouri, and D. Boukhetala, “Iterative learning control for trajectory tracking of a parallel Delta robot,” at - Automatisierungstechnik, vol. 67, no. 2, pp. 145–156, Feb. 2019, doi: 10.1515/auto-2018-0086.

[8] H. Shen, Q. Meng, J. Li, J. Deng, and G. Wu, “Kinematic sensitivity, parameter identification and calibration of a non-fully symmetric parallel Delta robot,” Mechanism and Machine Theory, vol. 161, p. 104311, Jul. 2021, doi: 10.1016/j.mechmachtheory.2021.104311.

[9] Q. Meng, J. Li, H. Shen, J. Deng, and G. Wu, “Kinetostatic design and development of a non-fully symmetric parallel Delta robot with one structural simplified kinematic linkage,” Mechanics Based Design of Structures and Machines, vol. 0, no. 0, pp. 1–21, Jun. 2021, doi: 10.1080/15397734.2021.1937213.
[10] C. E. Boudjedir, M. Bouri, and D. Boukhetala, “Model-Free Iterative Learning Control With Nonrepetitive Trajectories for Second-Order MIMO Nonlinear Systems—Application to a Delta Robot,” IEEE Transactions on Industrial Electronics, vol. 68, no. 8, pp. 7433–7443, Aug. 2021, doi: 10.1109/TIE.2020.3007091.

[11] M. Elshami, M. Shehata, Q. Bai, and X. Zhao, “Multibody Dynamics Modeling of Delta Robot with Experimental Validation,” in Multibody Mechatronic Systems, Cham, 2022, pp. 94–102. doi: 10.1007/978-3-030-88751-3_10.

[12] E. McCormick, Y. Wang, and H. Lang, “Optimization of a 3-RRR Delta Robot for a Desired Workspace with Real-Time Simulation in MATLAB,” in 2019 14th International Conference on Computer Science & Education (ICCSE), Toronto, ON, Canada, Aug. 2019, pp. 935–941. doi: 10.1109/ICCSE.2019.8845388.
[13] K. Fathi, H. W. van de Venn, and M. Honegger, “Predictive Maintenance: An Autoencoder Anomaly-Based Approach for a 3 DoF Delta Robot,” Sensors (Basel), vol. 21, no. 21, p. 6979, Oct. 2021, doi: 10.3390/s21216979.

[14] H. Wu, “Research on Control System of Delta Robot Based on Visual Tracking Technology,” J. Phys.: Conf. Ser., vol. 1883, no. 1, p. 012110, Apr. 2021, doi: 10.1088/1742-6596/1883/1/012110.

[15] K. Zheng, “Research on intelligent vibration suppression control of high-speed lightweight Delta robot,” Journal of Vibration and Control, p. 10775463211024888, Jun. 2021, doi: 10.1177/10775463211024888.

[16] Z. Iklima, M. I. Muthahhar, A. Khan, and A. Zody, “SELF-LEARNING OF DELTA ROBOT USING INVERSE KINEMATICS AND ARTIFICIAL NEURAL NETWORKS,” Sinergi, vol. 25, no. 3, p. 237, Jul. 2021, doi: 10.22441/sinergi.2021.3.001.

[17] C. M. A. Vasques and F. A. V. Figueiredo, “The 3D-Printed Low-Cost Delta Robot Óscar: Technology Overview and Benchmarking,” Engineering Proceedings, vol. 11, no. 1, Art. no. 1, 2021, doi: 10.3390/ASEC2021-11173.

[18] Susanto, M. Rohim, and R. Analia, “The Implementation of a Modest Kinematic Solving for Delta Robot,” in 2019 2nd International Conference on Applied Engineering (ICAE), Oct. 2019, pp. 1–5. doi: 10.1109/ICAE47758.2019.9221720.

[19] C. Liu, G.-H. Cao, and Y.-Y. Qu, “Workspace Analysis of Delta Robot Based on Forward Kinematics Solution,” in 2019 3rd International Conference on Robotics and Automation Sciences (ICRAS), Jun. 2019, pp. 1–5. doi: 10.1109/ICRAS.2019.8808987.
