# Mechanism

It's required that the target spatial point is not too far to reach, namely

<img src="http://latex.codecogs.com/svg.latex?\sqrt{x^2&plus;y^2&plus;z^2}&space;\le&space;l_1&plus;l_2&plus;l_3" title="\sqrt{x^2+y^2+z^2} \le l_1+l_2+l_3" />

This is checked before running the algorithm.

Also, it is assumed that z &ge; 0 as it is not expected that the arm could pick up something below the surface on which it is placed. The assumption z &ge; 0 is required for the analytical solution of inequalities, but as per my testing, deliberately setting z as negative does not lead to failure in the algorithm.

To reach a point D(x,y,z) in space, the robotic arm first needs to rotate <img src="http://latex.codecogs.com/svg.latex?\inline&space;&space;\varphi" title=" \varphi" /> degrees so that the linkages of the arm and the target point are coplanar (i.e. they all line on the blue plane).

<img src="http://latex.codecogs.com/svg.latex?&space;\varphi&space;=&space;\arctan{\frac{y}{x}}" title=" \varphi = \arctan{\frac{y}{x}}" />

<img src="demo/three-to-two.png" width="600px">

The spatial coordinate of the target point D is mapped to a coordinate on the blue plane, which could be calculated as follow

<img src="http://latex.codecogs.com/svg.latex?&space;&space;\begin{array}{l}&space;f:&space;\mathbb{R}^3&space;\rightarrow&space;\mathbb{R}^2&space;\\&space;f(x,&space;y,&space;z)&space;=&space;(\sqrt{x^2&space;&plus;&space;y^2},\&space;z)&space;\end{array}" title="  \begin{array}{l} f: \mathbb{R}^3 \rightarrow \mathbb{R}^2 \\ f(x, y, z) = (\sqrt{x^2 + y^2},\ z) \end{array}" />

Consequently, this problem is simplied to a two dimensional route planning problem. 

<img src="demo/2d.svg" width="500px">

As shown in the diagram above, the solution for this problem is not unique. A particular position of the first segment (l<sub>1</sub>) corresponds to a particular solution. However, not all positions of point B are suitable. We require:

<img src="http://latex.codecogs.com/svg.latex?&space;\left\{&space;\begin{array}{ll}&space;&l_2&space;&plus;&space;l_3&space;>&space;BD&space;\\&space;\\&space;&\sqrt{l_2^2&space;&plus;&space;l_3^2}&space;<&space;BD&space;\end{array}&space;\right." title=" \left\{ \begin{array}{ll} &l_2 + l_3 > BD \\ \\ &\sqrt{l_2^2 + l_3^2} < BD \end{array} \right." />

The first condition ensures that point B won't be so far away from D that the arm couldn't reach D. The second condition ensures that B won't be so close to point D that the angle BCD becomes acute (because the servo cannot rotate more than 90 degrees to the right).

B is on a circle whose center is the origin and radius is l<sub>1</sub>, so its coordinates can be expressed as follow:

<img src="http://latex.codecogs.com/svg.latex?&space;\begin{array}{l}&space;m&space;\in&space;[-l_1,&space;l_1]&space;\\\\&space;n&space;=&space;\sqrt{l_1^2&space;-&space;m^2}&space;\end{array}" title=" \begin{array}{l} m \in [-l_1, l_1] \\\\ n = \sqrt{l_1^2 - m^2} \end{array}" />

So the inequality could be formulated:

<img src="http://latex.codecogs.com/svg.latex?&space;\sqrt{l_2^2&space;&plus;&space;l_3^2}&space;<&space;\sqrt{(m&space;-&space;a)^2&space;&plus;&space;\left&space;(&space;\sqrt{l_1^2&space;-&space;m^2}&space;-&space;b&space;\right&space;)^2}&space;<&space;l_2&space;&plus;&space;l_3" title=" \sqrt{l_2^2 + l_3^2} < \sqrt{(m - a)^2 + \left ( \sqrt{l_1^2 - m^2} - b \right )^2} < l_2 + l_3" />

This inequality could be solved analytically. Because all terms are positive, we could take the square of both sides of the left inequality and obtain

<img src="http://latex.codecogs.com/svg.latex?&space;\begin{array}{l}&space;l_2^2&plus;l_3^2&space;<&space;m^2&space;-&space;2ma&space;&plus;&space;a^2&space;&plus;&space;(l_1^2&space;-&space;m^2)&space;-2b\sqrt{l_1^2-m^2}&space;&plus;&space;b^2&space;\\\\&space;l_2^2&plus;l_3^2&space;&plus;&space;2ma&space;-&space;l_1^2&space;-&space;a^2&space;-&space;b^2&space;<&space;-2b\sqrt{l_1^2-m^2}&space;\\\\&space;let\&space;A&space;=&space;l_2^2&plus;l_3^2&space;-&space;l_1^2&space;-&space;a^2&space;-&space;b^2&space;\\\\&space;A&space;&plus;&space;2ma&space;<&space;-2b\sqrt{l_1^2&space;-&space;m^2}\end{array}" title=" \begin{array}{l} l_2^2+l_3^2 < m^2 - 2ma + a^2 + (l_1^2 - m^2) -2b\sqrt{l_1^2-m^2} + b^2 \\\\ l_2^2+l_3^2 + 2ma - l_1^2 - a^2 - b^2 < -2b\sqrt{l_1^2-m^2} \\\\ let\ A = l_2^2+l_3^2 - l_1^2 - a^2 - b^2 \\\\ A + 2ma < -2b\sqrt{l_1^2 - m^2}\end{array}" />

We know that both sides are negative, so squaring both sides will flip the inequality

<img src="http://latex.codecogs.com/svg.latex?&space;\begin{array}{l}A^2&space;&plus;&space;4Aam&space;&plus;&space;4a^2m^2&space;>&space;4b^2(l_1^2-m^2)\\\\&space;4(a^2&plus;b^2)m^2&space;&plus;&space;4Aam&space;-&space;4b^2l_1^2&space;&plus;&space;A^2&space;>&space;0&space;\\\\&space;m&space;=&space;\frac{-4Aa\pm&space;\sqrt{16A^2a^2&space;-&space;16(a^2&plus;b^2)(A^2-&space;4b^2l_1^2)}}{2*4(a^2&plus;b^2)}&space;=&space;\frac{-Aa\pm&space;b\sqrt{4l_1^2(a^2&space;&plus;&space;b^2)-A^2}}{2(a^2&plus;b^2)}&space;\end{array}" title=" \begin{array}{l}A^2 + 4Aam + 4a^2m^2 > 4b^2(l_1^2-m^2)\\\\ 4(a^2+b^2)m^2 + 4Aam - 4b^2l_1^2 + A^2 > 0 \\\\ m = \frac{-4Aa\pm \sqrt{16A^2a^2 - 16(a^2+b^2)(A^2- 4b^2l_1^2)}}{2*4(a^2+b^2)} = \frac{-Aa\pm b\sqrt{4l_1^2(a^2 + b^2)-A^2}}{2(a^2+b^2)} \end{array}" />

<img src="http://latex.codecogs.com/svg.latex?&space;\begin{array}{l}&space;m_1&space;=&space;\frac{-Aa&space;-&space;b\sqrt{4l_1^2(a^2&space;&plus;&space;b^2)-A^2}}{2(a^2&plus;b^2)}\\\\&space;m_2&space;=&space;\frac{-Aa&space;&plus;&space;b\sqrt{4l_1^2(a^2&space;&plus;&space;b^2)-A^2}}{2(a^2&plus;b^2)}&space;\\\\&space;m\in&space;[-l_1,&space;m_1]\cup[m_2,l_1]\&space;\textup{is\&space;the\&space;solution\&space;set}&space;\end{array}" title=" \begin{array}{l} m_1 = \frac{-Aa - b\sqrt{4l_1^2(a^2 + b^2)-A^2}}{2(a^2+b^2)}\\\\ m_2 = \frac{-Aa + b\sqrt{4l_1^2(a^2 + b^2)-A^2}}{2(a^2+b^2)} \\\\ m\in [-l_1, m_1]\cup[m_2,l_1]\ \textup{is\ the\ solution\ set} \end{array}" />

Similarly,

<img src="http://latex.codecogs.com/svg.latex?&space;\begin{array}{l}&space;m^2&space;-&space;2am&space;&plus;&space;a^2&space;&plus;&space;(l_1^2&space;-&space;m^2)&space;-2b\sqrt{l_1^2-m^2}&space;&plus;&space;b^2&space;<&space;l_2^2&plus;l_3^2&space;&plus;&space;2l_2l_3&space;\\\\&space;-2am&space;&plus;&space;l_1^2&space;&plus;&space;a^2&space;&plus;&space;b^2&space;-&space;l_2^2&space;-&space;l_3^2&space;-&space;2l_2l_3<&space;2b\sqrt{l_1^2-m^2}\\\\&space;let\&space;B&space;=&space;a^2&space;&plus;&space;b^2&space;&plus;&space;l_1^2&space;-&space;l_2^2&space;-&space;l_3^2&space;-&space;2l_2l_3&space;\\\\&space;B&space;-&space;2am&space;<&space;2b\sqrt{l_1^2&space;-&space;m^2}&space;\end{array}" title=" \begin{array}{l} m^2 - 2am + a^2 + (l_1^2 - m^2) -2b\sqrt{l_1^2-m^2} + b^2 < l_2^2+l_3^2 + 2l_2l_3 \\\\ -2am + l_1^2 + a^2 + b^2 - l_2^2 - l_3^2 - 2l_2l_3< 2b\sqrt{l_1^2-m^2}\\\\ let\ B = a^2 + b^2 + l_1^2 - l_2^2 - l_3^2 - 2l_2l_3 \\\\ B - 2am < 2b\sqrt{l_1^2 - m^2} \end{array}" />

If B-2am > 0, we will have

<img src="http://latex.codecogs.com/svg.latex?&space;\begin{array}{l}&space;B^2&space;-&space;4Bam&space;&plus;&space;4a^2m^2&space;<&space;4b^2(l_1^2-m^2)\\\\&space;4(a^2&plus;b^2)m^2&space;-&space;4Bam&space;-&space;4b^2l_1^2&space;&plus;&space;B^2&space;<&space;0\\\\&space;m&space;=&space;\frac{4Ba\pm&space;\sqrt{16B^2a^2&space;-&space;16(a^2&plus;b^2)(B^2-&space;4b^2l_1^2)}}{2*4(a^2&plus;b^2)}&space;=&space;\frac{Ba\pm&space;b\sqrt{4l_1^2(a^2&space;&plus;&space;b^2)-B^2}}{2(a^2&plus;b^2)}&space;\end{array}" title=" \begin{array}{l} B^2 - 4Bam + 4a^2m^2 < 4b^2(l_1^2-m^2)\\\\ 4(a^2+b^2)m^2 - 4Bam - 4b^2l_1^2 + B^2 < 0\\\\ m = \frac{4Ba\pm \sqrt{16B^2a^2 - 16(a^2+b^2)(B^2- 4b^2l_1^2)}}{2*4(a^2+b^2)} = \frac{Ba\pm b\sqrt{4l_1^2(a^2 + b^2)-B^2}}{2(a^2+b^2)} \end{array}" />

If B-2am < 0, then this inequality is trivially satisfied. Therefore,

<img src="http://latex.codecogs.com/svg.latex?&space;\begin{array}{l}&space;m_3&space;=&space;\frac{Ba&space;-&space;b\sqrt{4l_1^2(a^2&space;&plus;&space;b^2)-B^2}}{2(a^2&plus;b^2)}\\\\&space;m_4&space;=&space;\frac{Ba&space;&plus;&space;b\sqrt{4l_1^2(a^2&space;&plus;&space;b^2)-B^2}}{2(a^2&plus;b^2)}\\\\&space;m\in[m_3,m_4]\cup[\frac{B}{2a},l_1]\&space;\textup{is&space;the&space;solution&space;set}&space;\end{array}" title=" \begin{array}{l} m_3 = \frac{Ba - b\sqrt{4l_1^2(a^2 + b^2)-B^2}}{2(a^2+b^2)}\\\\ m_4 = \frac{Ba + b\sqrt{4l_1^2(a^2 + b^2)-B^2}}{2(a^2+b^2)}\\\\ m\in[m_3,m_4]\cup[\frac{B}{2a},l_1]\ \textup{is the solution set} \end{array}" />

b = 0 is a special case because the quadratic term m<sup>2</sup> would vanish, leaving a linear inequality. Such case is easy to solve. So finally,

<img src="http://latex.codecogs.com/svg.latex?&space;\left\{&space;\begin{array}{ll}&space;m&space;\in&space;\left&space;(&space;([-l_1,&space;m_1]&space;\cup&space;[m_2,&space;l_1])&space;\cap&space;([m_3,&space;m_4]&space;\cup&space;[\frac{B}{2a},&space;l_1])&space;\right&space;)\&space;b&space;\neq&space;0&space;\\\\&space;m&space;\in&space;[\frac{B}{2a},\frac{-A}{2a}]\&space;b=0&space;\end{array}&space;\right." title=" \left\{ \begin{array}{ll} m \in \left ( ([-l_1, m_1] \cup [m_2, l_1]) \cap ([m_3, m_4] \cup [\frac{B}{2a}, l_1]) \right )\ b \neq 0 \\\\ m \in [\frac{B}{2a},\frac{-A}{2a}]\ b=0 \end{array} \right." />

Any m belongs to this set will be a solution. Given a particular m, the angles of servos could be calculated through a series of geometric derivations.

## Alex's method to obtain the set of solutions (Written by [Alex](https://github.com/Vol0324))

Well, there is another more straightforward way to calculate the angle and the coordinates.

Suppose the angle subtended by the x-axis and DO is &phi; and the distance DO equals k. We can write the above formula into such:

<img src="http://latex.codecogs.com/svg.latex?&space;\begin{array}{l}&space;l_2^2&plus;l_3^2&space;\leq(k\cos{\varphi}-l_1\sin{\theta_1})^2&plus;(k\sin{\varphi}-l_1\cos{\theta_1})^2\leq&space;(l_2&plus;l_3)^2\\\\&space;\frac{k^2&plus;l_1^2-(l_2&plus;l_3)^2}{2kl_1}&space;\leq{\sin(\theta_1&plus;\varphi)\leq{\frac{k^2&plus;l_1^2-l_2^2-l_3^2}{2kl_1}}}&space;\end{array}" title=" \begin{array}{l} l_2^2+l_3^2 \leq(k\cos{\varphi}-l_1\sin{\theta_1})^2+(k\sin{\varphi}-l_1\cos{\theta_1})^2\leq (l_2+l_3)^2\\\\ \frac{k^2+l_1^2-(l_2+l_3)^2}{2kl_1} \leq{\sin(\theta_1+\varphi)\leq{\frac{k^2+l_1^2-l_2^2-l_3^2}{2kl_1}}} \end{array}" />

At this point, naturally one will think of using the arcsin to solve the inequality. However, we have to make sure that the left side of the inequality is no smaller than -1, and the right side of the inequality is no larger than 1. Besides, as:

<img src="http://latex.codecogs.com/svg.latex?&space;\begin{array}&space;{lcr}&space;\theta_1\in[-90\degree,&space;90\degree]&space;&&space;\varphi\in[0\degree,&space;90\degree]&space;&&space;\theta_1&plus;\varphi\in[-90\degree,&space;180\degree]&space;\end{array}" title=" \begin{array} {lcr} \theta_1\in[-90\degree, 90\degree] & \varphi\in[0\degree, 90\degree] & \theta_1+\varphi\in[-90\degree, 180\degree] \end{array}" />

we have to make sure the angles are in the correct intervals.

Let's suppose the above inequality is in the interval [-1, 1]. Then

<img src="http://latex.codecogs.com/svg.latex?&space;\arcsin\left(\frac{k^2&plus;l_1^2-(l_2&plus;l_3)^2}{2kl_1}\right)&space;\leq\theta_1&plus;\varphi&space;\leq\arcsin\left(\frac{k^2&plus;l_1^2-l_2^2-l_3^2}{2kl_1}\right)" title=" \arcsin\left(\frac{k^2+l_1^2-(l_2+l_3)^2}{2kl_1}\right) \leq\theta_1+\varphi \leq\arcsin\left(\frac{k^2+l_1^2-l_2^2-l_3^2}{2kl_1}\right)" />

Let

<img src="http://latex.codecogs.com/svg.latex?&space;\begin{array}{lr}&space;\delta_1=\arcsin\left(\frac{k^2&plus;l_1^2-(l_2&plus;l_3)^2}{2kl_1}\right)&space;&&space;\delta_2&space;=&space;\arcsin\left(\frac{k^2&plus;l_1^2-l_2^2-l_3^2}{2kl_1}\right)&space;\end{array}" title=" \begin{array}{lr} \delta_1=\arcsin\left(\frac{k^2+l_1^2-(l_2+l_3)^2}{2kl_1}\right) & \delta_2 = \arcsin\left(\frac{k^2+l_1^2-l_2^2-l_3^2}{2kl_1}\right) \end{array}" />

We also need to check whether 180 - &delta;<sub>1</sub> and 180 - &delta;<sub>2</sub> is also an interval for &theta;<sub>1</sub> + &phi;. To make sure that everything is proper, we need to check where 90 + &phi; lies inside the interval. 90 + &phi; is a limit that cannot be exceeded. Nevertheless, using this way we can find the range of &theta;<sub>1</sub>, and from then on we can easily find other angles.
</p>

## From coordinates to angles: A geometric derivation

<img src="demo/angle-solution.svg" width="700px"/>

From the graph, it is easy to see that</p>

<img src="http://latex.codecogs.com/svg.latex?&space;&space;\begin{array}{l}&space;\theta_1&space;=&space;\arcsin{\frac{m}{l_1}}&space;\\&space;\\&space;\theta_2&space;=&space;\frac{\pi}{2}&space;-&space;\theta_1&space;-&space;\alpha&space;-&space;\beta&space;\\&space;\\&space;\theta_3&space;=&space;\pi&space;-&space;B\widehat{C}D&space;\end{array}" title="  \begin{array}{l} \theta_1 = \arcsin{\frac{m}{l_1}} \\ \\ \theta_2 = \frac{\pi}{2} - \theta_1 - \alpha - \beta \\ \\ \theta_3 = \pi - B\widehat{C}D \end{array}" />

&alpha; could be obtained by using the cosine rule

<img src="http://latex.codecogs.com/svg.latex?&space;\alpha&space;=&space;\arccos{\left&space;(&space;\frac{l_2^2&space;&plus;&space;c^2&space;-&space;l_3^2&space;}{2l_2^2c}&space;\right&space;)}" title=" \alpha = \arccos{\left ( \frac{l_2^2 + c^2 - l_3^2 }{2l_2^2c} \right )}" />

where c is the length of BD

<img src="http://latex.codecogs.com/svg.latex?&space;&space;\begin{array}{l}&space;c&space;=&space;\sqrt{(a-m)^2&space;&plus;&space;(b-n)^2}&space;\\\\&space;n&space;=&space;\sqrt{l_1^2&space;-&space;m^2}&space;\end{array}" title="  \begin{array}{l} c = \sqrt{(a-m)^2 + (b-n)^2} \\\\ n = \sqrt{l_1^2 - m^2} \end{array}" />

&beta; can be found by applying arctan to the gradient of BD:

<img src="http://latex.codecogs.com/svg.latex?&space;\beta&space;=&space;\arctan{\frac{b-n}{a-m}}" title=" \beta = \arctan{\frac{b-n}{a-m}}" />

angle BCD can be found by using the cosine rule:

<img src="http://latex.codecogs.com/svg.latex?&space;B\widehat{C}D&space;=&space;\arccos{\left&space;(&space;\frac{l_2^2&plus;l_3^2&space;-&space;c^2}{2l_2l_3}&space;\right&space;)}" title=" B\widehat{C}D = \arccos{\left ( \frac{l_2^2+l_3^2 - c^2}{2l_2l_3} \right )}" />

Finally, combine all equations and using only known values:

<img src="http://latex.codecogs.com/svg.latex?&space;&space;\begin{array}{l}&space;\theta_1&space;=&space;\arcsin{\frac{m}{l_1}}\\&space;\theta_2&space;=&space;\frac{\pi}{2}&space;-&space;\arcsin{\frac{m}{l_1}}&space;-&space;\arccos{\left&space;(&space;\frac{l_2^2&space;&plus;&space;(a-m)^2&space;&plus;(b-\sqrt{l_1^2&space;-&space;m^2})^2&space;-&space;l_3^2&space;}{2l_2^2&space;\sqrt{(a-m)^2&space;&plus;&space;(b-\sqrt{l_1^2&space;-&space;m^2})^2}}&space;\right&space;)}&space;-&space;\arctan{\frac{b-\sqrt{l_1^2&space;-&space;m^2}}{a-m}}\\&space;\theta_3&space;=&space;\pi&space;-&space;\arccos{\left&space;(&space;\frac{l_2^2&plus;l_3^2&space;-&space;(a-m)^2-(b-\sqrt{l_1^2&space;-&space;m^2})^2}{2l_2l_3}&space;\right&space;)}&space;\end{array}" title="  \begin{array}{l} \theta_1 = \arcsin{\frac{m}{l_1}}\\ \theta_2 = \frac{\pi}{2} - \arcsin{\frac{m}{l_1}} - \arccos{\left ( \frac{l_2^2 + (a-m)^2 +(b-\sqrt{l_1^2 - m^2})^2 - l_3^2 }{2l_2^2 \sqrt{(a-m)^2 + (b-\sqrt{l_1^2 - m^2})^2}} \right )} - \arctan{\frac{b-\sqrt{l_1^2 - m^2}}{a-m}}\\ \theta_3 = \pi - \arccos{\left ( \frac{l_2^2+l_3^2 - (a-m)^2-(b-\sqrt{l_1^2 - m^2})^2}{2l_2l_3} \right )} \end{array}" />

The spatial coordinate of each joint could be recovered from &phi;, &theta;<sub>1</sub>, &theta;<sub>2</sub> and &theta;<sub>3</sub>:

<img src="http://latex.codecogs.com/svg.latex?&space;&space;\begin{array}&space;{l}&space;\gamma_1&space;=&space;\frac{\pi}{2}&space;-&space;\theta_1&space;\\\\&space;\gamma_2&space;=&space;\frac{\pi}{2}&space;-&space;\theta_1&space;-&space;\theta_2&space;\\\\&space;\gamma_3&space;=&space;\frac{\pi}{2}&space;-&space;\theta_1&space;-&space;\theta_2&space;-&space;\theta_3\\\\\\&space;\end{array}" title="  \begin{array} {l} \gamma_1 = \frac{\pi}{2} - \theta_1 \\\\ \gamma_2 = \frac{\pi}{2} - \theta_1 - \theta_2 \\\\ \gamma_3 = \frac{\pi}{2} - \theta_1 - \theta_2 - \theta_3\\\\\\ \end{array}" />

<img src="http://latex.codecogs.com/svg.latex?&space;&space;\begin{array}{l}&space;A&space;(0,&space;0,&space;0)&space;\\\\&space;B&space;(l_1\cos{\gamma_1}\cos{\varphi},\&space;l_1\cos{\gamma_1}\sin{\varphi},\&space;l_1\sin{\gamma_1})\\\\&space;C&space;(l_2\cos{\gamma_2}\cos{\varphi}&space;&plus;&space;X_B,\&space;l_2\cos{\gamma_2}\sin{\varphi}&space;&plus;&space;Y_B,\&space;l_2\sin{\gamma_2}&space;&plus;&space;Z_B)\\\\&space;D&space;(l_3\cos{\gamma_3}\cos{\varphi}&space;&plus;&space;X_C,\&space;l_3\cos{\gamma_3}\sin{\varphi}&space;&plus;&space;Y_C,\&space;l_3\sin{\gamma_3}&space;&plus;&space;Z_C)&space;\end{array}" title="  \begin{array}{l} A (0, 0, 0) \\\\ B (l_1\cos{\gamma_1}\cos{\varphi},\ l_1\cos{\gamma_1}\sin{\varphi},\ l_1\sin{\gamma_1})\\\\ C (l_2\cos{\gamma_2}\cos{\varphi} + X_B,\ l_2\cos{\gamma_2}\sin{\varphi} + Y_B,\ l_2\sin{\gamma_2} + Z_B)\\\\ D (l_3\cos{\gamma_3}\cos{\varphi} + X_C,\ l_3\cos{\gamma_3}\sin{\varphi} + Y_C,\ l_3\sin{\gamma_3} + Z_C) \end{array}" />