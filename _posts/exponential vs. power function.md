## dfference between exponentioal and power function
1. power
- plot: When plotting power function, the log(y) versus log(x) plot is linear with the slope being exponent a. y=x^a.  
- relationship: When x2 is two-fold the x1, then y2 is 2^a fold the y1.  
- trend: starts at (0,0)
2. exponentional 
- plot: When plotting exponential function, the log(y) versus x plot is linear with the slope being the k. y=e^(x*k)=(e^k)^x=a^x, if a=e^k
- trend: starts at (0,1), first increase slowly becasue linear x is not fast, then increase very fast
Ln(y1)-ln(y2)=k*(x1-x2)
Ln(y1/y2)=k*(x1-x2)
k=Ln(y1/y2)/(x1-x2)
Y1/y2=e^(k*(x1-x2))=(e^k)^(x1-x2) let a=e^k, then y1/y2=a^(x1-x2)

![alt text](relative%20path/../../figure.png?raw=true)
How to get the initial estimate of k exponential function when we already know the MLE of a in the power function?





From this plot we can see that the two lines are most close at the median of the population. and we already know how much the y increase when the x is 2-fold from the power function.
Then the initial estimate of exponential function k=Ln(y1/y2)/(x1-x2)=ln(2^1.5)/(1250-625)=0.0014


