---
permalink: /pyLiBELa/
title: "pyLiBELa"
excerpt: "My current undergraduate research program, the adaptation of the LiBELa software to python. The proposal is in this [link](https://drive.google.com/file/d/1kp_QNQqQRvjkVPcNEV61AbLVYqu8VRCy/view?usp=sharing)."
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
![image](https://user-images.githubusercontent.com/84737515/230401109-ed64b0a7-b108-48dc-927b-b17bdbd96059.png)
![image](https://user-images.githubusercontent.com/84737515/230401589-f1f04a9b-c4eb-4825-9430-4a64587cfb84.png)
both available at this [Google Sheets](https://docs.google.com/spreadsheets/d/1O8RrHqa47ECNAhhuMptmBpY7jB9al0Y9AmVeFnsZ9FI/edit?usp=sharing)

And they can be turned into a graph 
![image](https://user-images.githubusercontent.com/84737515/230405794-b8790516-82c5-40d3-97af-440eaf08743b.png)
throught [this script](https://colab.research.google.com/drive/1qgiUBcMtLVN0AS0UV3XsEmcUh1bSfe_p?usp=sharing), 
and cleaned to higher depedencies 
![image](https://user-images.githubusercontent.com/84737515/230448040-c8fac1ab-c879-4ee4-9846-7f23ec989039.png)

### Updated the local and Colab makefiles
-     Now we can make one class at a time
    
### Adapting McEntropy, MC and SA classes to LiBELa
- The classes work on the local machine

Notes:
- In the MC class, I adapted the info variable as in Grid

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



### Adapting Optimizer class to LiBELa
- The following error appeared

```yaml
pyLiBELa/src/pyDocker.cpp: In member function ‘void Docker::run(Mol2*, Mol2*, Mol2*, std::vector<double>, PARSER*, unsigned int)’:
pyLiBELa/src/pyDocker.cpp:129:31: warning: format ‘%d’ expects argument of type ‘int’, but argument 14 has type ‘double’ [-Wformat=]
  129 |                 sprintf(info, "%5d %-12.12s %-4.4s %-10.3e  %-8.3g %-8.3g %-8.3g %-8.2f %-8.3g %3d %2d %2d %.2f", counter, Lig->molname.c_str(), Lig->resnames[0].c_str(), overlay_fmax,
      |                               ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  130 |                         0.0, 0.0, 0.0, 0.0, 0.0, -1, overlay_status, 0.0, si);
      |                                                                      ~~~
      |                                                                      |
      |                                                                      double
pyLiBELa/src/pyDocker.cpp: In member function ‘void Docker::run(Mol2*, Mol2*, Mol2*, std::vector<double>, PARSER*, Grid*, unsigned int)’:
pyLiBELa/src/pyDocker.cpp:266:31: warning: format ‘%d’ expects argument of type ‘int’, but argument 14 has type ‘double’ [-Wformat=]
  266 |                 sprintf(info, "%5d %-12.12s %-4.4s %-10.3e  %-8.3g %-8.3g %-8.3g %-8.2f %-8.3g %3d %2d %2d %.2f", counter, Lig->molname.c_str(), Lig->resnames[0].c_str(), overlay_fmax,
      |                               ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  267 |                         0.0, 0.0, 0.0, 0.0, 0.0, -1, overlay_status, 0.0, si);
      |                                                                      ~~~
      |                                                                      |
      |                                                                      double
```

it was fixed by changing formatted string in line 29 from 
```yaml
%5d %-12.12s %-4.4s %-10.3e  %-8.3g %-8.3g %-8.3g %-8.2f %-8.3g %3d %2d **%2d** %.2f
```
to 
```yaml
"%5d %-12.12s %-4.4s %-10.3e  %-8.3g %-8.3g %-8.3g %-8.2f %-8.3g %3d %2d **%2f** %.2f" and do the same thing in line 266.
```
- The following error still remains
```yaml
pyLiBELa/src/pyOptimizer.cpp: In function ‘void init_module_pyOptimizer()’:
pyLiBELa/src/pyOptimizer.cpp:1516:76: error: no matches converting function ‘evaluate_energy’ to type ‘double (class Optimizer::*)(class Mol2*, class std::vector<std::vector<double> >)’
 1516 |     double (Optimizer::*ee1)(Mol2*, vector<vector<double> >) = &Optimizer::evaluate_energy;
      |                                                                            ^~~~~~~~~~~~~~~
pyLiBELa/src/pyOptimizer.cpp:75:6: note: candidates are: ‘static void Optimizer::evaluate_energy(Mol2*, std::vector<std::vector<double> >, energy_result_t*)’
   75 | void Optimizer::evaluate_energy(Mol2* Lig2, vector<vector<double> > new_xyz, energy_result_t* energy_result){
      |      ^~~~~~~~~
pyLiBELa/src/pyOptimizer.cpp:48:8: note:                 ‘static double Optimizer::evaluate_energy(Mol2*, std::vector<std::vector<double> >)’
   48 | double Optimizer::evaluate_energy(Mol2* Lig2, vector<vector<double> > new_xyz){
      |        ^~~~~~~~~
pyLiBELa/src/pyOptimizer.cpp:1517:94: error: no matches converting function ‘evaluate_energy’ to type ‘void (class Optimizer::*)(class Mol2*, class std::vector<std::vector<double> >, struct energy_result_t*)’
 1517 | izer::*ee2)(Mol2*, vector<vector<double> >, energy_result_t*) = &Optimizer::evaluate_energy;
      |                                                                             ^~~~~~~~~~~~~~~
pyLiBELa/src/pyOptimizer.cpp:75:6: note: candidates are: ‘static void Optimizer::evaluate_energy(Mol2*, std::vector<std::vector<double> >, energy_result_t*)’
   75 | void Optimizer::evaluate_energy(Mol2* Lig2, vector<vector<double> > new_xyz, energy_result_t* energy_result){
      |      ^~~~~~~~~
pyLiBELa/src/pyOptimizer.cpp:48:8: note:                 ‘static double Optimizer::evaluate_energy(Mol2*, std::vector<std::vector<double> >)’
   48 | double Optimizer::evaluate_energy(Mol2* Lig2, vector<vector<double> > new_xyz){
      |        ^~~~~~~~~
make: *** [Makefile:115: Optimizer] Error 1
```

it is probably due to the fact that the functions in this class are defined as static, [this approach found on the internet](https://wiki.python.org/moin/boost.python/FunctionOverloading) didn't work.

Useful links:
- [Static Keyword C++](https://www.geeksforgeeks.org/static-keyword-cpp/)



## Week 4
### Apadting Optimizer class to LiBELa
- In order to solve the static overload error shown in [Week3](https://caiodejesus.github.io/pyLiBELa/#adapting-optimitzer-class-to-libela), I created another function evaluate_energy2 that corresponds to the version with three arguments. So, in pyOptimizer.cpp pyOptimizer.h scripts, the evaluate_energy method with the energy_t parameter was changed to evaluate_energy2.

- An attempt to solve any remaining static function errors, every function had an added .staticmethod() as below

```yaml
.def("name_of_function", & CLASS::name_of_function).staticmethod("name_of_function")
``` 

### Adapting Docker class to LiBELa
- Adapted tha info variable as Grid
- It works in the local machine


### Adapting FullSearch class to LiBELa

- Adapted the info variable as in Grid
- Some format errors were solved in lines 43 and 94, changing from "%d" to "%lu".
- It still shows the following error

```yaml
pyLiBELa/src/pyFullSearch.cpp: In member function ‘double FullSearch::do_search()’:
pyLiBELa/src/pyFullSearch.cpp:211:1: warning: no return statement in function returning non-void [-Wreturn-type]
  211 | }
      | ^
```

## Week 5 

### Updating Makefile
      
- It takes 3 minutes to build the .o files

- Checking the depedencies between the libraries

- Added the Docker class to the github Makefile

- We still have issues with pySA and pyMc. They depend on the Grid class and can't be compiled 

- The Optimizer class shows the following error when imported:
```yaml
 undefined symbol: _ZN9Optimizer3RecE
```

## Week 6

### Updating Makefile

- The pyLiBELa is now constructed through the Makefile in three steps. But when called, the Makefile doesn't make these three targets work one after the other.

- When MC is imported, the following error appears:
```yaml
ImportError: /content/pyMC.so: undefined symbol: _ZN9Optimizer3RecE
```

- When Optimizer is imported, the following error appears:
```yaml
ImportError: /content/pyOptimizer.so: undefined symbol: _ZN9Optimizer3RecE
```
There is a need to reevaluate the way static functions are defined in pyOptimizer.


### Useful Links

- [BOOST python documentation about static members](https://wiki.python.org/moin/boost.python/HowTo#static_class_data_members)



## Week 7

### Updating Makefile 
- Google Colab Updated its Python version to Python 3.10.11 so we have to adapt our code to it.

- Try to replicate [LiBELa's original Makefile]() 



### Static issues Optimizer

- We still have some issues about static members of pyOptimizer: Rec, RefLig, Grids and Parser. There are issues with class linkage. The methods are compiled, but Python can’t identify the static members on Colab.

- [I chatted a bit about these issues with ChatGPT]() to gain some insight into this matter. 

- Tried to adapt some policies suggested by Chat GPT and analyse the errors that appeared. The result is in this [file]().


- We solved the issue by creating a get_Rec function as shown in the Bogo to Bogo link below. We now need to create similar functions with RefLig, Parser and Grids.


### Useful Links

- [Russian site](https://habr-com.translate.goog/ru/articles/168233/?_x_tr_sl=pt&_x_tr_tl=en&_x_tr_hl=pt-BR&_x_tr_pto=wapp&_x_tr_hist=true)

- [Apps Love World](https://www.appsloveworld.com/cplus/100/379/docstrings-for-static-properties-in-boostpython)

- [Sudo Null](https://sudonull.com/post/131366-Combining-C-and-Python-Subtleties-of-BoostPython-Part-two)

- [About Py_Initialize](https://deltastep.blogspot.com/2020/11/embedding-python-in-c-with-boostpython.html)

- [Call Policies - Boost Python](https://boostorg.github.io/python/doc/html/reference/function_invocation_and_creation/models_of_callpolicies.html)

- [Bogo to Bogo](https://www.bogotobogo.com/cplusplus/statics.php)


week 8?
 versão antiga
 !ls /usr/lib/x86_64-linux-gnu/libboost_python*
 	mostra que tenho instalado o libboost_python38.so.1.71.0
 	queremo o libboost_python310.so.1.74.0
na maquina local, com um 
	apt-cache search libboost-python
aparece ja 
	libboost-python1.74.0 - Boost.Python Library
no colab só aparece
	libboost-python1.71.0 - Boost.Python Library

alessandro ja tinha reajustado

pyOptimizer ainda com erro

atualizei makefile - novas classes docker e fullsearch, por mais que ainda nao tenha funcionado o optimizer certinho

usando o qtcreator na maquina local pra compilar
ver como usa o cmake

-fPIC -O3 -I/usr/local/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 -DBUILD=0 -DHAVE_EIGEN -Wunused-value -L/usr/lib/x86_64-linux-gnu -lboost_python310 -lz -lopenbabel -lgsl -lnlopt_cxx -lgsl -lgslcblas -lm 
 
 
 Concluímos a pyOptimizer!!!

Versão finalizada do Colab em https://github.com/alessandronascimento/pyLiBELa/blob/main/Colabs/pyLiBELa.ipynb
Começando testes com a base de dados SB

	baixei pelo site https://ringo.ams.stonybrook.edu/index.php/SB2021.v1
	escrevi um script para organizar melhor (organizer)
		reduzi de 556,9MB para 165,8 MB
	subindo pro drive
	
	
gpu??? https://towardsdatascience.com/downloading-datasets-into-google-drive-via-google-colab-bcb1b30b0166
muita coisa pra baixar pro colab, limitei pra os que começam com 1A*, são 25



agora o desafio é o loop


week 15 maio
flag e linkagem no openmp

	só precisa do -fopemmp: https://stackoverflow.com/questions/12002304/how-to-compile-openmp-using-g
	alterei no makefile
	
	
abri o box.pdb com a função la

consegui fazer a visualização - ver asana

	centrado no ligante
	mostrando aa vizinhos
	ligante destacado
	box.pdb gerado com caixa




alessandro fez isso
	1. calcula as distâncias atomicas de todos os atomos (mol2 e COOR_MC) - definir máxima distância interatomica
		fazer um loop pros 1200
		ai estimar a caixa
		
conferir numero de threads na maquina virtual google
	ele só tem 2 mesmo
	ver com !cat /proc/cpuinfo
	siblings?
		
só falta os aa destacados
(colocar prints)
	tentando concatenar os arquivos pdb
		https://linuxhint.com/sed-command-to-delete-a-line/
		-i ele atualiza o arquivo



como eu juntei os arquivos:


#arquivo original
!tail $name_file_rec #pra mostrar as ultimas linhas

CONECT 2612 2611                                                      
CONECT 2613 2611 2614 2615                                            
CONECT 2614 2613                                                      
CONECT 2615 2609 2609 2613 2616                                       
CONECT 2616 2615                                                      
CONECT 2617 2604 2618 2619                                            
CONECT 2618 2617                                                      
CONECT 2619 2617                                                      
MASTER        0    0    0    0    0    0    0    0 2620    0 2620    0
END



#tira o end e o conect do arquivo do receptor
!sed -i '/END\|CONECT/d' $name_file_rec 
!tail $name_file_rec

ATOM   2612  HE1 HIE A 166      -4.687  14.216   3.620  1.00  0.00           H  
ATOM   2613  NE2 HIE A 166      -2.634  13.840   3.194  1.00  0.00           N  
ATOM   2614  HE2 HIE A 166      -2.471  13.015   3.789  1.00  0.00           H  
ATOM   2615  CD2 HIE A 166      -1.789  14.362   2.290  1.00  0.00           C  
ATOM   2616  HD2 HIE A 166      -0.782  14.028   2.092  1.00  0.00           H  
ATOM   2617  C   HIE A 166      -2.307  14.471  -1.360  1.00  0.00           C  
ATOM   2618  O   HIE A 166      -2.772  14.918  -2.417  1.00  0.00           O1-
ATOM   2619  OXT HIE A 166      -2.671  13.389  -0.874  1.00  0.00           O1-
ATOM   2620 MAG  MAG A 167       4.767  33.735  19.215  1.00  0.00          Mg  
MASTER        0    0    0    0    0    0    0    0 2620    0 2620    0
 



!head $name_file_lig #mostra as primeiras linhas
print(' ')
COMPND    121P.lig 
AUTHOR    GENERATED BY OPEN BABEL 3.0.0
ATOM      1  P1  LIG A   1       5.150  32.173  22.030  1.00  0.00           P  
ATOM      2  O1  LIG A   1       4.768  32.597  23.390  1.00  0.00           O  
ATOM      3  O2  LIG A   1       4.164  32.683  21.069  1.00  0.00           O  
ATOM      4  O3  LIG A   1       4.834  30.641  22.025  1.00  0.00           O  
ATOM      5  C1  LIG A   1       6.782  32.406  21.624  1.00  0.00           C  
ATOM      6  H1  LIG A   1       7.016  33.438  21.679  1.00  0.00           H  
ATOM      7  H2  LIG A   1       7.394  31.869  22.301  1.00  0.00           H  
ATOM      8  P2  LIG A   1       7.419  31.970  20.137  1.00  0.00           P  
 


#troca a cadeia A por B no arquivo do ligante
!sed -i 's/A  /B  /g' $name_file_lig 


!head $name_file_lig
print(' ')
COMPND    121P.lig 
AUTHOR    GENERATED BY OPEN BABEL 3.0.0
ATOM      1  P1  LIG B   1       5.150  32.173  22.030  1.00  0.00           P  
ATOM      2  O1  LIG B   1       4.768  32.597  23.390  1.00  0.00           O  
ATOM      3  O2  LIG B   1       4.164  32.683  21.069  1.00  0.00           O  
ATOM      4  O3  LIG B   1       4.834  30.641  22.025  1.00  0.00           O  
ATOM      5  C1  LIG B   1       6.782  32.406  21.624  1.00  0.00           C  
ATOM      6  H1  LIG B   1       7.016  33.438  21.679  1.00  0.00           H  
ATOM      7  H2  LIG B   1       7.394  31.869  22.301  1.00  0.00           H  
ATOM      8  P2  LIG B   1       7.419  31.970  20.137  1.00  0.00           P  



!tail $name_file_lig
print(' ')
CONECT   39   37   40   41                                            
CONECT   40   39                                                      
CONECT   41   39   42   45   45                                       
CONECT   42   41   43   44                                            
CONECT   43   42                                                      
CONECT   44   42                                                      
CONECT   45   41   41   46                                            
CONECT   46   32   36   36   45                                       
MASTER        0    0    0    0    0    0    0    0   46    0   46    0
END




#tira o conect do arquivo do ligante
!sed -i '/CONECT/d' $name_file_lig 


!tail $name_file_lig
print(' ')
ATOM     39  N3  LIG B   1      17.666  32.006  16.826  1.00  0.00           N  
ATOM     40  H12 LIG B   1      18.472  31.504  16.485  1.00  0.00           H  
ATOM     41  C10 LIG B   1      17.922  32.786  17.906  1.00  0.00           C  
ATOM     42  N4  LIG B   1      19.209  32.864  18.275  1.00  0.00           N  
ATOM     43  H13 LIG B   1      19.926  32.459  17.692  1.00  0.00           H  
ATOM     44  H14 LIG B   1      19.460  33.328  19.135  1.00  0.00           H  
ATOM     45  N5  LIG B   1      16.947  33.303  18.638  1.00  0.00           N  
ATOM     46  C11 LIG B   1      15.731  33.075  18.102  1.00  0.00           C  
MASTER        0    0    0    0    0    0    0    0   46    0   46    0
END



name_file_merge = pdb_code + '.pdb'
!cat $name_file_rec $name_file_lig > $name_file_merge #concatena os arquivos





	
proximos passos
	aumentar grid_spacing e ver como impacta as energias e o tempo
		gráfico pra esse primeiro caso: tempo de execução x 
	testa com uma e duas threads e vê a alteração no tempo de execução


semana 22 maio

visualização no colab
	resolvi o de aparecer o resíduo todo e de aumentar a janela
	falta resolver de n aparecer a cadeia principal
	

preparando o terreno
	me baseei no meu código https://github.com/CIERMag-FFPaivaStudents/Wavelet/blob/master/1D/metrics_1D.py pra organizar os dados
	ecrevi em arquivos .dat e .csv (https://www.pythontutorial.net/python-basics/python-write-csv-file/)
	ver quais valores eu coloco
		rodar!
		
		
vendo se o timeit é em segundos (13s)

	import timeit
	b=0
	start_time = timeit.default_timer()
	for i in range(100000000):
	  b+=i
	time = timeit.default_timer() - start_time
	print(time)

testar o espaçamento dos grids e o erro percentual
	vimos que é 30 A pelo gráfico (colocar gŕafico asana)

diferença perentual (< q 5% é o ideal) - ver a média para todos
	filtros - se a energia é positiva, tirar
		tanto a das coordenadas quanto a dos grids

rodei tudo por 7h - teste sb 25
	analisar dados

time energy dá quase 0
	121P 1.0
	Original energy: -237.910
	Time grids: 11.15
	Points: 30 30 30
	Grid energy: 414.375
	Time energy: 0.00

	121P 0.5
	Original energy: -237.910
	Time grids: 100.20
	Points: 60 60 60
	Grid energy: -61.214
	Time energy: 0.00

rodei pra 10

troquei parametros e adaptei código



cluster: site alessandro

baixar miniconda no cluster
	DEFEITO ASANA

ajeitei inorganicos no py3Dmol
	not lista de aminoácidos

testar script pra máquina local
	criando script


próximas etapas: ver colabs de visuzaliação
rodar no cluster






