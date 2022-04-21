# Sum of terms in a Partial Harmonic Serie by using Stirling Formula
Main reference: https://pt.frwiki.wiki/wiki/Nombre_harmonique#:~:text=Em%20matem%C3%A1tica%20%2C%20o%20n%2D%20%C3%A9simo,frac%20%7B1%7D%20%7Bk%7D%7D%7D

Python code to verify and comparate the method by iterator the sum of terms in a Partial Harmonic Serie and the method with Stirling Formula, wich use Stirling number of primer order.  

By run the code, the follow plot was generated:

![plot_efficiency_stirlingformula](https://user-images.githubusercontent.com/69974562/164373191-0e1a9673-c7f9-4ea7-8d4a-f76b74b91852.png)

## Conclusion:
The plot above shows the seconds expends less than method of iterator the sum of a subsequence of Harmonic Serie by using the Stirling Formula. So it's possible to conclude that the Stirling Formula is computationally more efficiently than the common iterator method. 

Furthermore, by seeing the plot, notice that more the length of the subsequence is, more the difference between the iterator method and stirling Formula increases, so it's a behavior of a polynomial with order two.

---

See full code below:
``` python
#https://pt.frwiki.wiki/wiki/Nombre_harmonique#:~:text=Em%20matem%C3%A1tica%20%2C%20o%20n%2D%20%C3%A9simo,frac%20%7B1%7D%20%7Bk%7D%7D%7D
#https://pt.frwiki.wiki/wiki/Nombre_de_Stirling
#http://clubes.obmep.org.br/blog/sala-de-estudo-permutacoes-circulares-e-os-numeros-de-stirling-do-primeiro-tipo/#:~:text=%5Bnk%5D%3D%5Bn,de%20Stirling%20do%20primeiro%20tipo.
import math
from fractions import Fraction
import time

'''
Código para mostrar a equivalência da soma parcial de N números harmônicos 
(1/k), k=1,...,N, utilizando a fórmula fechada que envolve uso de número de 
Stirling do primeiro tipo (olhe referências acima)
'''


dict_Stirling = {}
def Stirling(n):
    '''
    Calculates Stirling number primer order: [n 2]'
    '''
    if n==3: 
        dict_Stirling[3] = 3
        return 3 #Comb(3,2) = 3
    else:
        try: 
            dict_Stirling[n] = math.factorial(n-2) + (n-1)*dict_Stirling[n-1]
            return dict_Stirling[n]
        except:
            dict_Stirling[n] = math.factorial(n-2) + (n-1)*Stirling(n-1)    
            return dict_Stirling[n]


def n_specify(n = 1, input = False):
    '''
    Function to specify an arbitrary number of length of the sequence of 
    harmonic numbers, and returns the result of this sum using the common formula
    (only iterate the sum of 1/k , k=1 until k=n) and using the formula of 
    Stirling number primer order.
    '''
    if input: 
        n = int(input("Insert the number of harmonics:\t"))
    else: 
        n = n
        print(n)
    print("================================")
    
    t_ini_Xn = time.time()
    Xn=0
    for i in range(1,n+1):
        Xn+=Fraction(1,i)
    print(f"\n\tBy iterator method:\n\t By fraction:\t{Xn}")
    t_end_Xn = time.time()
    time_Xn = t_end_Xn - t_ini_Xn

    try: print(f"\t Decimal result: \t{Xn.numerator/Xn.denominator}")
    except: print("\t float too large")
    print(f"\t Time expend using Iterator Formula: {time_Xn}")
        
    # from itertools import combinations
    # def Comb(k,j):
    #     return len(list(combinations(list(range(k)),j)))
    
    print('\t--------------------------------')
    
    t_ini_Hn = time.time()    
    
    Hn = Fraction(1,math.factorial(n))*Stirling(n+1)

    t_end_Hn = time.time()
    time_Hn = t_end_Hn - t_ini_Hn
    
    print(f"\tBy using Stirling Property:\n\t By fraction:\t{Hn}")
    try: print(f"\t Decimal result: \t{Hn.numerator/Hn.denominator}")
    except: print("\t float too large")
    print(f"\t Time expend using Stirling Formula: {time_Hn}")
    
    
    print(f"\n\n=> Verify equivalent in the two forms to calculate = {str(Hn==Xn)}")
    if time_Hn > time_Xn:
        print(f"=>\t Stirling Formula expends {time_Hn-time_Xn} seconds MORE than the iterator Method")
    else:
        print(f"=>\t Stirling Formula expends {time_Xn-time_Hn} seconds LESS than the iterator Method")

    return (time_Hn, time_Xn, Hn,Xn)
#========================
        
def Prop_TRUE(N):
    '''
    Calculates the accuracy of the Stirling Formula for N subsequences of the
    Harmonic serie (as expected, alwalys returns score = 1)
    '''
    accuracy = 0
    ini = time.time()
    dict_Xn={0:0}
    for j in range(1,N+1):
        n=j
        Xn=0
        
        dict_Xn[j] = dict_Xn[j-1]+Fraction(1,j)
        Xn = dict_Xn[j]
    
        if n==1:Hn=1
        elif n==2: Hn=Fraction(3,2)
        else: Hn = Fraction(1,math.factorial(n))*Stirling(n+1)
        
        if Xn==Hn: accuracy +=1
    score = accuracy/N
    print(f"time:{time.time()-ini}")
    print(f"score: {score} as expected") #Out[]: 1.0
    return score

#=========================

# time_vector is the vector containing the less time expend by using Stirling Formula by comparing with Iterator method in a subsequenve of Harmonic Serie of length i, which i=2,...2000
time_vector = [n_specify(i)[1]-n_specify(i)[0] if n_specify(i)[2]==n_specify(i)[3] else 100 for i in range(2,2000)] #notice that 100 is only to verify any error

import matplotlib.pyplot as plt

plt.plot(time_vector)
plt.title("Stirling Formula performance comparing the Iterator Method")
plt.xlabel("Length of the subsequence of the Harmonic Serie")
plt.ylabel("Time expend with Stirling Formula LESS than Iterotor Method")
plt.show()
```

<hr>

## Some outputs of the code:

### Function `Prop_TRUE()`
The `Prop_TRUE` function calculates the accuracy of the Stirling Formula for N subsequences of the Harmonic serie (as expected, always returns score = 1), and calculates the time expend for run (noticed that depends for each machine).

```
In[]:   Prop_TRUE(2000)

Out[]:  time:0.9969339370727539
        score: 1.0 as expected
```

### Function `n_specify()`
Calculates the sum of 1/i which i goes from 1 to N using two methods than compare them: **by Stirling Formula** _X_ **by Iterator Method**
```
In[]:   n_specify(200)

Out[]:  200
        ================================

          By iterator method:
           By fraction:	73430450139366304745412892037069099001170161275640475032430988199840965762047744114895233/12492355141960232023683917288697829904903495658709527193661000811749408076321384817296000
           Decimal result: 	5.878030948121444
           Time expend using Iterator Formula: 0.00498652458190918
          --------------------------------
          By using Stirling Property:
           By fraction:	73430450139366304745412892037069099001170161275640475032430988199840965762047744114895233/12492355141960232023683917288697829904903495658709527193661000811749408076321384817296000
           Decimal result: 	5.878030948121444
           Time expend using Stirling Formula: 0.0


        => Verify equivalent in the two forms to calculate = True
        =>	 Stirling Formula expends 0.00498652458190918 seconds LESS than the iterator Method
```


