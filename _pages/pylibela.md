---
permalink: /pyLiBELa/
title: "pyLiBELa"
excerpt: "My current undergraduate reaserch program, the adaptation of the LiBELa software to python."
toc: true
---



# **Weekly Reports**


## Week 1

### Adapting WRITER class to pyLiBELa

- read the pyPARSER and pyMOL2 classes to understand how it works
- boost WRITER.cpp and WRITER.h 
- upload it to github and test in Google Collab

How to generate a shared library with BOOST:
```yaml
g++ -Wall -Wextra -fPIC -shared / - example.cpp -o example.so -lboost_python37 -lboost_numpy37
```

Steps I followed to adapt it with boost:
- Know the class funcions and variables by looking at the .h file
- Write the BOOST appendage by the end of the code writing 
      for variables .def_readwrite("old_variable_name", & CLASS NAME::"new variable name"), we kept the same name for simplicity
      for functions .def_write("old_variable_name", & CLASS NAME::"new variable name")
      
Notes:
- The local machine uses python3.10.6 while Collab uses python3.9.16
- If the function has more than one possible input, adapt it as [TNG](https://github.com/TNG/boost-python-examples/blob/main/09-Overloading/overload.cpp)
- [pyLiBELa on github](https://github.com/alessandronascimento/pyLiBELa)
Results:
- The WRITER class is now adapted to pyLiBELa


### Tried to Adapt new Mol2 class to pyLiBELa

- see the history log in [Source code](https://github.com/alessandronascimento/LiBELa/blob/master/trunk/src/LiBELa/Mol2.cpp)
- boost new functions and variables

Results:
- The Mol2 class doesn't work in Google Collab showing the following error message:

```yaml
In file included from pyMol2.cpp:8:
pyMol2.h:21:10: fatal error: openbabel/obconversion.h: No such file or directory
   21 | #include <openbabel/obconversion.h>
      |          ^~~~~~~~~~~~~~~~~~~~~~~~~~
compilation terminated.
```

 - In the local computer, it still shows the message
 
 ```yaml
 In file included from /usr/include/boost/smart_ptr/detail/sp_thread_sleep.hpp:22,
                 from /usr/include/boost/smart_ptr/detail/yield_k.hpp:23,
                 from /usr/include/boost/smart_ptr/detail/spinlock_gcc_atomic.hpp:14,
                 from /usr/include/boost/smart_ptr/detail/spinlock.hpp:42,
                 from /usr/include/boost/smart_ptr/detail/spinlock_pool.hpp:25,
                 from /usr/include/boost/smart_ptr/shared_ptr.hpp:29,
                 from /usr/include/boost/shared_ptr.hpp:17,
                 from /usr/include/boost/python/converter/shared_ptr_to_python.hpp:12,
                 from /usr/include/boost/python/converter/arg_to_python.hpp:15,
                 from /usr/include/boost/python/call.hpp:15,
                 from /usr/include/boost/python/object_core.hpp:14,
                 from /usr/include/boost/python/args.hpp:22,
                 from /usr/include/boost/python.hpp:11,
                 from pyWRITER.cpp:712:
/usr/include/boost/bind.hpp:36:1: note: ‘#pragma message: The practice of declaring the Bind placeholders (_1, _2, ...) in the global namespace is deprecated. Please use <boost/bind/bind.hpp> + using namespace boost::placeholders, or define BOOST_BIND_GLOBAL_PLACEHOLDERS to retain the current behavior.’
   36 | BOOST_PRAGMA_MESSAGE(
      | ^~~~~~~~~~~~~~~~~~~~
/usr/include/boost/detail/iterator.hpp:13:1: note: ‘#pragma message: This header is deprecated. Use <iterator> instead.’
   13 | BOOST_HEADER_DEPRECATED("<iterator>")
   
```

but as the .so archives are compiled, we ignore it.


## Week 2

### Adapting new Mol2 class to pyLiBELa

- fixing the fatal error adding the g++ flags: -DHAVE_EIGEN; -I/usr/include/openbabel3; -I/usr/include/eigen3
- fixing an error by defining the str variable differently

How to generate the libraries for each class so far:
<p align="center">
PARSER
</p>
```yaml
g++ -fPIC -shared -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyPARSER.cpp -o pyPARSER.so -L/usr/lib/x86_64-linux-gnu -lboost_python310
```
<p align="center">
WRITER
</p>
```yaml
g++ -fPIC -shared -DBUILD=0 -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyWRITER.cpp -o pyWRITER.so -L/usr/lib/x86_64-linux-gnu -lboost_python310 -lz
```

<p align="center">
Mol2
</p>
```yaml
g++ -fPIC -shared -DHAVE_EIGEN -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyMol2.cpp -o pyMol2.so -L/usr/lib/x86_64-linux-gnu -lboost_python310 -lz
```
 
### Adapting Grid class to pyLiBELa
 
- boost the variables and functions of the class
- adapt the variable info as the variable str of the Mol2 class 
 
Results:
- The Grid class can be turned into a dinamic library in Google Collab and in the local machine through the following command line:

<p align="center">
Grid
</p>

```yaml
g++ -fPIC -shared -DBUILD=0 -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyGrid.cpp pyWRITER.cpp -o pyGrid.so -L/usr/lib/x86_64-linux-gnu -lboost_python310 -lz
```



## Week 3

### Adapting COORD_MC class to pyLiBELa
- Downloaded the RAND.h from LiBELa's source code and renamed it to pyRAND.h
- Downloaded the gsl library to local machine

Results:
- The COORD_MC class can be turned into a dinamic library in Google Collab and in the local machine through the following command line:

<p align="center">
COORD_MC
</p>
```yaml
g++ -fPIC -shared -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyCOORD_MC.cpp -o pyCOORD_MC.so -L/usr/lib/x86_64-linux-gnu -lboost_python310
```


### Adapting FindHB class to pyLiBELa

Results:
- The FindHB class can be turned into a dinamic library in the local machine

<p align="center">
FindHB
</p>
```yaml
g++ -fPIC -shared -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyCOORD_MC.cpp -o pyCOORD_MC.so -L/usr/lib/x86_64-linux-gnu -lboost_python310
```

- In Google Collab, the code is compiled. But, as the library is imported on python, it shows the following error message appears
![image](https://user-images.githubusercontent.com/84737515/227987066-1f2b9750-d897-4143-8cf4-edf03d3a873c.png)
- The Collab error was fixed by adding the -lopenbabel at the end


### Creating a makefile
 - Learning how to create a makefile
 - Creating a makefile to compile all the .so libraries
 - Create another one to compile on Google Colab
 
 Results:
 - Created a makefile that compiles every dinamic library with by tiping only 'make'
 - The BOOST error appears for every line of code compiled -> fix that in the future
 - The makefile to the [Google Colab version](https://colab.research.google.com/github/alessandronascimento/pyLiBELa/blob/main/Colabs/makefile_test.ipynb) works, but the libraries pyGrid and pyFindHB can't be imported.
 - The pyGrid library can be imported if you add the following lines to the pyGrid.cpp file:
 ```yaml
 #include "pyWRITER.h"
 #include "pyWRITER.cpp"
 ```
and do the same with the pyFindHB file, but include the pyCOORD_MC.* files instead.
 
  
 
 Useful links:
 - [Introdução ao makefile](https://embarcados.com.br/introducao-ao-makefile/)
 - [LiBELa's original makefile](https://github.com/alessandronascimento/LiBELa/blob/master/trunk/src/LiBELa/Makefile)
 - [GNU make documentation](https://www.gnu.org/software/make/manual/make.html)
 
## Week 4

### Adapting Energy2 class to pyLiBELa

Now that we have a makefile, testing needs to be done in four steps:
- individually in the local machine
```yaml
g++ -fPIC -shared -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyCLASS.cpp -o pyCLASS.so -L/usr/lib/x86_64-linux-gnu -lboost_python310
```
- with the other classes through the Makefile
```yaml
make
```
in the pyLiBELa folder. The files will be transformed in .so dinamnic libraries that will be found in the obj/ directory. To delete everything, you just need to type
```yaml
make clean
```
- individually in Google Colab
- with the other classes through the [Colab Makefile](https://colab.research.google.com/github/alessandronascimento/pyLiBELa/blob/main/Colabs/Working_Example.ipynb) 

In this class we had a "subclass" called GridInterpol, written in the .h file as
```yaml
class Energy2 {
public:
    struct GridInterpol{
        double vdwA;
        double vdwB;
        double elec;
        double pbsa;
        double delphi;
        double solv_gauss;
        double rec_solv_gauss;
        double hb_donor;
        double hb_acceptor;
    };
```
so we had to define it in a separate space in the boost appendage

```yaml
    class_<Energy2::GridInterpol>("GridInterpol")
            .def_readwrite("vdwA", & Energy2::GridInterpol::vdwA)
            .def_readwrite("vdwB", & Energy2::GridInterpol::vdwB)
            .def_readwrite("elec", & Energy2::GridInterpol::elec)
            .def_readwrite("pbsa", & Energy2::GridInterpol::pbsa)
            .def_readwrite("delphi", & Energy2::GridInterpol::delphi)
            .def_readwrite("solv_gauss", & Energy2::GridInterpol::solv_gauss)
            .def_readwrite("rec_solv_gauss", & Energy2::GridInterpol::rec_solv_gauss)
            .def_readwrite("hb_donor", & Energy2::GridInterpol::hb_donor)
            .def_readwrite("hb_acceptor", & Energy2::GridInterpol::hb_acceptor)
    ;
```
and where it appeared as an argument for trilinear_interpolation(), we put it as Energy2::GridInterpol.


In Colab, if the following error appears
```yaml
ImportError: /content/pyEnergy2.so: undefined symbol: _ZTI4Grid
```
just add the corresponding class to the .cpp file. In this case, the Energy2 class depends on the Grid class, so we add to the pyEnergy.cpp file the file
```yaml
#include "pyGrid.cpp"
```
and it works.

Results:
- The .so library can be compiled locally
- If we add the new files to the Makefile on the Colab repository, it can be imported in Google Colab.

Notes:
- Shift+Ctrl+V pastes in the terminal
- Ctrl+C interrupts the command in the terminal



Useful links:
- To search for specific parts of the source code go to this [link](https://github.com/alessandronascimento/LiBELa/search?l=C%2B%2B&q=)
- .md files [tutorial](https://www.markdowntutorial.com) 

### Adapting Conformer class to pyLiBELa

Notes:
- The following message appeared
```yaml
pyConformer.cpp: In member function ‘bool Conformer::generate_conformers_confab(PARSER*, Mol2*, std::string)’:
pyConformer.cpp:92:11: error: ‘class OpenBabel::OBForceField’ has no member named ‘DiverseConfGen’
   92 |     OBff->DiverseConfGen(0.5, Input->conf_search_trials, 50.0, false);
      |           ^~~~~~~~~~~~~~
```
and the error was fixed by adding to the terminal line a -DHAVE_EIGEN.
 
Results:
- The Conformer class is ready to be used both in the local machine and in the Google Collab

### Updating Makefiles
- Creating an external Makefile for testing in the local machine so that there is only one Makefile, the one from Google Colab.
- Updating the Colab Makefile to be able to compile one function at a time.

### Adapting RAND and Gaussian classes to LiBELa
- RAND and Gaussian can be compiled in the local machine and in Google Colab


### Listed the dependencies of pyLiBELa library
- The dependencies of each class can be now viewed on the following tables
![image](https://user-images.githubusercontent.com/84737515/230207467-f9f2b71f-b0e4-4d38-8263-071dd60d66a4.png)
![image](https://user-images.githubusercontent.com/84737515/230207513-a4ccae75-61e3-4cc9-9a8b-de1eb5333998.png)

## Updated the local and Colab makefiles
-     now we can make one class at a time
-     
### Adapting MC and classes to LiBELa
- The classes work on the local machine

- In the MC class, the following errors appeared
      changed the info variable as in Grid

Notes:
- In the MC class, the following error appeared
```yaml
pyLiBELa/src/pyMC.cpp: In member function ‘void MC::run(Mol2*, Mol2*, Mol2*, std::vector<std::vector<double> >, PARSER*, double)’:
pyLiBELa/src/pyMC.cpp:1015:23: warning: format ‘%f’ expects argument of type ‘double’, but argument 3 has type ‘long double’ [-Wformat=]
 1015 |         sprintf(info, "Boltzmann-weighted average energy: %10.3f kcal/mol @ %7.2f K", this->Boltzmann_weighted_average_energy, T);
      |                       ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      |                                                                                             |
      |                                                                                             long double
```
and was fixed by changing in the line 1015 the %10.3f to a %10.3Lf, so that is has type long double (Lf).

This also appeared
```yaml
pyLiBELa/src/pyMC.cpp: In constructor ‘MC::MC(Mol2*, PARSER*, WRITER*)’:
pyLiBELa/src/pyMC.cpp:73:19: warning: format ‘%d’ expects argument of type ‘int’, but argument 3 has type ‘size_t’ {aka ‘long unsigned int’} [-Wformat=]
   73 |     sprintf(info, "Found %d rotatable bonds in ligand %s.", RotorList.Size(), Lig->molname.c_str());
      |                   ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  ~~~~~~~~~~~~~~~~
      |                                                                           |
      |                                                                           size_t {aka long unsigned int}
```
and was fixed by changing in the line 73 the %d to a %lu, so that is has type long unsigned int.








