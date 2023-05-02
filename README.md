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

**Therefore, the constant K means that the CORDIC function as an overall 











