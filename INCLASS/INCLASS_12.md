### IN-CLASS 12: Univariate distributions

Produce R-code to obtain the following probabilities

**1)** X follows a Normal distribution with mean 10 and variance 4. Evaluate the following probabilities:
   - P(X<8)
   - P(X>11)
   - P(8<X<11)
   


**2)** For the same RV X, we produce a linear transformaton Z=(X-10)/2. Compute the following probabilities
   - P(Z< -1)
   - P(Z> 1/2)
   - P( -1 < X < 1/2)



**3)** Let Z1, Z2,...,Zp be IID Bernoulli random variables with success probability 0.07. Now let X=Z1+Z2+...+Zn. Compute and report the following probabilities for `n=[10,20,30]`

  - P(X=3)
  - P(X>3)
  - P(X<3)
  - P(X<=3)
  - Verify that P(X<=3)+P(X>3)=1



**4)** The Poisson Distribution is used for count variables (i.e., variables that can take values 0, 1, 2,...) and can be viewed as an 
approximation to the Binomial distribution when the success probability of each of the Bernoulli trials is small and the number of trials is large. 
The distribution has a signle parameter (labmda) which is both the expected value and the variance of the RV. 

Use R to generate 10,000 draws from Binomial and Poisson, for a RV X which is the sum of 50 Bernoulli trials, each with success probability 0.05. Hint: for the Poisson simulation set lambda to be the expected value of the Binomial RV.
Compare the results of both simulations using table(X).


**5)** The estimated regression coefficient for the effect of education in wages in a linear model (b) that included an intercept and 5 other predictors was 0.83 and the SE was 0.45. Compute the p-value for testing the following hypothes

   - H0: b=0 Vs Ha: b different than 0
   - H0: b=0.5 Vs Ha: b>0.5
   
Counduct the test assuming that b follows a normal distribution first, and then using a t-distribution for (b-b0)/SE using df=20.


**6)** In a meeting involving 100 people there are three people infected with COVID-19. You are suceptible, you participate in the meeting, and have close contacts with 4 individuals. What is the probability that you contract  COVID-19 assuming that the probability of contracting the disease from a close contact between an infected and a suceptible individual is 1?

Hint 1: P(Infection|4 contacts)=1-P(No-infection|4 contacts)=1-P(No-infection in 1st contact)P(No-infection in 2nd contact| no infection in 1st contact)P(No-infection in 3rd contact| no infection in 1st 2 contacts)P(No-infection in 4th contact| no infection in 1st 3 contacts)

Hint 2: Study the hypergeometric distribution.



