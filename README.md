# MOEAC-Solver

[![Build Status](https://travis-ci.org/Project-Platypus/Platypus.svg?branch=master)](https://travis-ci.org/Project-Platypus/Platypus)
[![Documentation Status](https://readthedocs.org/projects/platypus/badge/?version=latest)](http://platypus.readthedocs.org/en/latest/?badge=latest)

### What is MOEAC-Solver?

MOEAC-Solver is a framework for evolutionary computing in Python with a focus on
multiobjective evolutionary algorithms (MOEAs).  It differs from existing
optimization libraries, including PyGMO, Inspyred, DEAP, and Scipy, by providing
optimization algorithms and analysis tools for multiobjective optimization.
It currently supports NSGA-II, NSGA-III, MOEA/D, IBEA, Epsilon-MOEA, SPEA2, GDE3,
OMOPSO, SMPSO, and Epsilon-NSGA-II. 


### Example 2

Objetive distance + material capacity : penalizing material movements to embankments far from the embankment 1.

Data: 

- 2 clearances (Produce 10000 m3)
- 1 embankment (Require 1000 m3)
- 3 Dump zones (Max: 25890)

Results:
 - Resources leveling

```python

#!/usr/bin/python
import sys
print sys.path
from platypus import Problem, Real, NSGAII

class Test(Problem):

    def __init__(self):
        super(Test, self).__init__(8, 1, 6)
        self.types[:] = [Real(0, 25890), Real(0, 25890), Real(0, 25890),Real(0, 25890), Real(0, 25890), Real(0, 25890), Real(0,1000), Real(0,1000)]
        self.constraints[0:3] = ">=0"
        self.constraints[3:] = "==0"

    
    def evaluate(self, solution):
        x = solution.variables[0]
        y = solution.variables[1]
        z = solution.variables[2]
        t = solution.variables[6]

        n = solution.variables[3]
        m = solution.variables[4]
        o = solution.variables[5]
        q = solution.variables[7]

        solution.objectives[:] = [x*2+y*6+z*12+t*3 + n*2+m*6+o*12+q*3]
        solution.constraints[0:3] = [25980-(x+n),25980-(y+m),25980-(z+o)]
        solution.constraints[3:] =[1000-(t+q),10000-(t+(x+y+z)),10000-(q+(n+m+o))]
        
algorithm = NSGAII(Test())
algorithm.run(200000)

for s in algorithm.result:
	print s.variables,s

```

### Installation

To install MOEAC-Solver from source, run the following commands:

```

    git clone https://github.com/manuparra/moeac-solver/moeac-solver.git
    cd moeac-solver
    python setup.py install
```

### License

moeac-solver is released under the GNU General Public License.
