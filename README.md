<h1 align="center">Design of a Highly Configurable Cordic IP Core</h1>

<h3 align="left">Introduction:</h3>
<p align="left">
This project mainly focuses on the implementation of CORDIC Algorithm. The word CORDIC is the abbreviation of Coordinate Rotation Digital Computer, which is an algorithm that is commonly used to calculate various mathematical functions, including trigonometric, logarithmic, and exponential functions. A CORDIC IP Core is a specialized hardware component that is designed to implement the CORDIC algorithm. The method can also be easily extended to compute square roots as well as hyperbolic functions.</p>
<p align="left">
The algorithm works by reducing the calculation into a number of micro-rotations for which the arctangent value is precomputed and loaded in a table. This method reduces the computation to addition, subtraction, compares, and shifts. All functions easily performed by FPGAs.
CORDIC cores are commonly used in digital signal processing (DSP) applications, such as in the design of filters, modulators, and demodulators. They can also be used in other applications that require high-speed and low-power processing of complex mathematical functions, such as in the fields of image and video processing, telecommunications, and cryptography.
</p>

<h3 align="left">CORDIC Theory:</h3>

- Let (x<sub>i</sub>, y<sub>i</sub>) and (x<sub>j</sub>, y<sub>j</sub>) are the coordinates and to rotate these coordinates I have performed the following matrix calculations. 

<img width="308" alt="image" src="https://user-images.githubusercontent.com/99958597/235726611-e9b34d2d-3c18-40cc-9ba1-3b61b129c968.png">

- The above equation can be modified by factoring a cosθ, then we will get the following, 
<img width="375" alt="image" src="https://user-images.githubusercontent.com/99958597/235728271-002f313f-44cc-4ccd-a40f-650093c7baab.png">

- So from the above calculation, we can say that any angle θ in the first quadrant can be represented with the help of the following equation. 

<img width="375" alt="image" src="https://user-images.githubusercontent.com/99958597/235731062-e7c69f1b-4497-40ab-a178-be49f4f2d5a1.png">

Where, S<sub>i</sub> = 0 or 1

- For instance,  the angle 0 is computed by letting S_i in equation (3) be simply S<sub>i</sub>=0,0,0,0... . The angle θ = pi/2 is computed by letting S<sub>i</sub> = 1,1,1,1. .. . pi/4 would be 1,0,0,0... , and so on.

- In practice, due to the accuracy limits of the digital representation, the summation can be limited to the number of bits representing the value, or fewer. The arctan (1/2<sup>n</sup>) quickly approaches to 0.

- **Note** that equation 3 can also be constructed such that S_i = -1 or 1. For example, the angle pi/4 would be computed with the sequence 1,1,−1,−1,−1,... . The advantage of this method will become apparent.

- This results in the following equation for tan θ<sub>n</sub>
<img width="375" alt="image" src="https://user-images.githubusercontent.com/99958597/235733187-4865c2e0-0dd4-4569-9cea-dd258a9b750a.png">

- Lets go back to the equation 2, , and given that the angle θ can be represented as a weighted sum (3). We can represent the rotation from (x<sub>i</sub> to  y<sub>i</sub>) as a sequence of smaller rotations where the n+1 rotational coordinate is derived from the n<sup>th</sup> coordinate using the following equation: 

<img width="410" alt="image" src="https://user-images.githubusercontent.com/99958597/235734096-7f3bf2c0-96f6-47b2-9c34-4b96e79851df.png">
- By replacing the equation 4 into equation 5 we are replacing the tangent function divided by 2<sup>n</sup>, which is a simple shift operatopn. 
<img width="422" alt="image" src="https://user-images.githubusercontent.com/99958597/235734960-d0534396-daa3-4cf8-a8c8-4c0473d256f1.png">

- By implementing the algorithm by letting S<sub>n</sub> with the values -1 and 1, and by doing that the cos θ will become, 
          
          cos (θ) = cos (-θ)      (7)
          
- The cos (θ) term is reduced to a constant K, which is given by the following equation, 
<img width="399" alt="image" src="https://user-images.githubusercontent.com/99958597/235736229-8f11d695-b3a4-4a9b-b8e9-2fe235e60e4a.png">

**As a result, the constant K indicates that the CORDIC function results in an overall gain of the factor 1/K, also known as the CORDIC gain. This may be considered in other areas of the system because it is a constant.**

<h3 align="left">IP CORE Discription:</h3>

- A first quadrant coordinate rotational digital computer technique for computing transcendental functions are realised by the highly customizable CORDIC IP Core. The core can be implemented in RTL in one of three designs using definitions in the source:
<br>
<b>1. Combinatorial: </b>

- The combinatorial realisation sacrifices several levels of logic in order to solve equations in one clock cycle.
-  This option gives a result in a single clock cycle at the expense of very deep logic levels.
-  The combinatorial implementation runs at about 10 mhz while the iterative ones run at about 125 in a Lattice ECP2 device.
<br>
<b>2. Iterative: </b> 

- The problem is divided into a number of iterations using the iterative technique. Using this approach results in less logic, but it requires more clock cycles to finish. This option builds a single ROTATOR. 
- The user provides the arguments and gives the core ITERATIONS clock cycles to get the result.  A signal named init is instantiated to load the input values.  It uses the least amount of LUTs
<br>
<b>3. Pipelined: </b>

- Finally, the pipeline realization takes several clock cycles to complete, but the core can produce a new result on every clock cycle.
-  This option can take a new input on every clock and gives results ITERATIONS clock cycles later. It uses the most amount of LUTS.
<br>

<h3 align="left">Modes of Operation:</h3>

This CORDIC IP CORE operates in one of the two modes: 
<br>

<b>1. Rotate: </b>

- The user provides the core an angle in the rotate mode, and the core computes the sin and cos. Sin/Cos inputs are used. 
- The core enables computing angles in either radian or degree format in addition to core architecture. Additionally, there are provisions for dealing with variable bit lengths and iterations. The source has a thorough explanation of each of the many "defines."
-  In this mode the user supplies a X and Y cartesian vector and an angle. The CORDIC rotator seeks to reduce the angle to zero by rotating the vector.
-  To compute the cos and sin of the angle, set the inputs as follows:<br>

             y_in = 0;
             x_in = `CORDIC_1
             theta_in = the input angle 
             
             on completion: 

             y_out = sin
             x_out = cos
             
Where, The `CORDIC_1 above is the inverse of the cordic gain. 

<br>
<b>2. Vector: </b>

- In vector mode, and the computer outputs the arctangent. 
- In this mode the user supplies the tangent value in x and y and the rotator seeks to minimize the y value, thus computing the angle.
- To compute the arctan set the inputs as follows:
-  y_in and x_in  such that y/x = the tangent value for which you wish to find the angle 

             theta_in = 0;
             
             on completion
             
             theta_out = the angle
             
<h3 align="left">CORDIC IP CORE Implementation:</h3>
 



















