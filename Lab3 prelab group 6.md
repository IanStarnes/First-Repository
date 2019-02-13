## Prelab: Lab 3, Group 6
### Ian Starnes and Barbara Oramah


####1.
The ANCs of Cayuga Lake and Wolf Pond before the acid rain are

$$ANC_{Cayuga} = 1.6 \frac{meq}{L}$$

$$ANC_{WolfPond} = 70 \frac{\mu eq}{L}$$

The ANCs after 20% of the lake is replaced with the acid rain are

$$ANC_{Cayuga} = (0.8) \times (1.6 \frac{meq}{L}) + (0.2) \times (-10^-3.5 \frac{eq}{L}) = 1.217 \frac{meq}{L}$$

$$ANC_{WolfPond} = (0.8) \times (70 \frac{\mu eq}{L}) + (0.2) \times (-10^-3.5 \frac{eq}{L}) = -7.245\frac{\mu eq}{L}$$

```python

from aguaclara.research.environmental_processes_analysis import *
from aguaclara.core.units import unit_registry as u
u.define("equivalent = mole = eq")
import aguaclara.research.environmental_processes_analysis as epa
from scipy import optimize
from aguaclara import *
# The u.define function adds the definition for equivalents.

#I created a function that calculates the pH given the ANC for an open system. You can use this code to find the pH of the lake and the pond!

def ANC_zeroed(pHguess, ANC):
  return ((epa.ANC_open(pHguess) - ANC.to(u.eq/u.L)).magnitude)

# Now we use root finding to find the pH that results in the known ANC.
# Our function will call the ANC_zeroed function. The pHguess is the first
# input of the ANC_zeroed function and the range on that is set by the next
# two inputs in the optimize.brentq function. The ANC is passed as an
# additional argument.

def pH_open(ANC):
  return optimize.brentq(ANC_zeroed, 0, 14,args=(ANC))

anc_cayuga = (0.8)*(0.0016 * u.eq/u.L)+(0.2)*(pH_open(3.5))
anc_wolfpond = (0.8)*(0.00007 * u.eq/u.L)+(0.2)*(pH_open(3.5))
# We can test this function to find the pH of pure water in equilibrium
# with the atmosphere
pH_open(anc_cayuga)

print('The pH of pure water equilibrium with the atmosphere is',pH_open(anc_cayuga))

```
####2.
The ANC of a water sample containing only carbonates and a strong acid that is at pH 3.2 is

$$ANC =2[CO_{3}^{2-}]+[HCO_{3}^{-}]+[OH^{-}]-[H^{+}]$$

The $$[CO_{3}^{2-}]$$ and $$[HCO_{3}^{-}]$$ both are approximated to 0 because pH << pK1, pK2.

The $$[OH^{-}]$$ is negligible because the pH is low.

Therefore, the ANC is well approximated as $$ANC= -[H^{-}]= - (10^{-3.2})=-0.000631 \frac {eq}{L}$$

####3.
$$[H^{-}]$$ is not a conserved species because it reacts with the carbonate species in the solution. ANC, on the other hand, is conserved.
