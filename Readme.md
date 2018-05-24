# HybridLWEAttack

## Estimator

Our "estimator_hybrid.py" sage module estimates the bit-security of the hybrid MITM attack, given LWE parameters.
We uses functions in "estimator.py" made by Albrecht et al [[APS15]](https://eprint.iacr.org/2015/046)
and the high-level structure of our estimator "estimator_hybrid.py" is from "estimator.py".

### Note 
The current version is somewhat unstable for estimating small parameters such as n < 1000 and log(q) < 30.

### How to Run

We recommend to use our **MITM_estimator** function in "estimator_hybrid.py". This function estimates the cost of ring operations, given a bounded memory capacity 2^80.

EXAMPLE :\
    * sage: from estimator_hybrid import *\
    * sage: n = 8192\
    * sage: q = next_prime(2^125)\
    * sage: alpha = 8 / q\
    * sage: h = 64 # default 64\
    * sage: MITM_estimator(n, alpha, q, h)
    
    
    

    Chosen Parameters :
             n =  8192, log(q) = 125.0, stddev =  3.19, HW(s) =   64
     
    Start Estimation . . .

    Optimizing with beta =  240 . . .
    Optimizing with beta =  247 . . .
    Optimizing with beta =  243 . . .
    Optimizing with beta =  244 . . .

    == Bit-security : 129.9 with optimal parameters
         k =  4861, h1 =  7, h2 =  9, beta =  240, mem =  80.8
                 (For simplicity, we set k1 = k2 = k/2)

    == Details
             rop:  2^129.9
               m:   2^12.8
             red:  2^102.3
         delta_0: 1.005603
            beta:      240
          repeat:  170.419
               d:   2^12.8
               c:   23.025
            post:  2^97
        prob_inv:   2^27.6
               k:   2^12.2
              h1:        7
              h2:       9
             mem:   2^80.8
             
## Attack Implementation

"mitm.py" is an attack code for Sage.
This is just for a proof-of-concept of the hybrid attack, and hence it is definitely not optimized.

### How to run

The most high-level function is **hybrid_mitm**.
It takes the following input

    n, q, alpha (=8/q default) 		: LWE parameters
    h 					: the hamming weight of secret vector
    beta 					: BKZ blocksize
    k					: LWE dim after Dim-Error tradeoff
    tau (= 30 default)			: LWE sample number after Dim-Error tradeoff
    ell (= h default)			: MITM parameter (hamming weight bound)

and generate LWE sample (A, **c**) and Uniform vector **u**,
and perform the hybrid MITM attack on (A, **c**), and then on (A, **u**).
  
EXAMPLE :\
    * sage: from Mitm import *\
    * sage: n = 50; q = next_prime(2^18); h = 8; beta = 20; k = 30;\
    * sage: hybrid_mitm(n, q, h, beta, k)
    
    Performing Dimension-error trade-off . . .

    New k-dim samples have error bound = 405.96, where q = 16411

    Start MITM on the k-dim samples . . .

    Table size = 451
    ** Mitm with (YA_2, Yc) ** 

    Number of noisy searches = 48
     - Input is LWE

    ** Mitm with (YA_2, Yu) **

    Number of noisy searches = 451
     - Input is uniform    



