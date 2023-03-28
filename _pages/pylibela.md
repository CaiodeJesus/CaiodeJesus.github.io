---
permalink: /pyLiBELa/
title: "pyLiBELa"
---



# **Reports**

## Weekly Reports 

### Week 1

Adapting WRITER class to pyLiBELa:

- read the pyPARSER and pyMOL2 classes to understand how it works
- boost WRITER.cpp and WRITER.h 
- upload it to github and test in Google Collab

How to generate a shared library with BOOST:
 g++ -Wall -Wextra -fPIC -shared / - example.cpp -o example.so -lboost_python37 -lboost_numpy37

Steps I followed to adapt it with boost:
- Know the class funcions and variables by looking at the .h file
- Write the BOOST appendage by the end of the code writing 
      for variables .def_readwrite("old_variable_name", & CLASS NAME::"new variable name"), we kept the same name for simplicity
      for functions .def_write("old_variable_name", & CLASS NAME::"new variable name")
      
Notes:
- The local machine uses python3.10.6 while Collab uses python3.9.16
- If the function has more than one possible input, adapt it as [TNG](https://github.com/TNG/boost-python-examples/blob/main/09-Overloading/overload.cpp)

Results:
- The WRITER class is now adapted to pyLiBELa


Tried to Adapt new Mol2 class to pyLiBELa:

- see the history log in [Source code](https://github.com/alessandronascimento/LiBELa/blob/master/trunk/src/LiBELa/Mol2.cpp)
- boost new functions and variables

Results:
- The Mol2 class doesn't work in Google Collab showing the following error message:

In file included from pyMol2.cpp:8:
pyMol2.h:21:10: fatal error: openbabel/obconversion.h: No such file or directory
   21 | #include <openbabel/obconversion.h>
      |          ^~~~~~~~~~~~~~~~~~~~~~~~~~
compilation terminated.

 - In the local computer, it still shows the message
 
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
   
but as the .so archives are compiled, we ignore it.


### Week 2

Adapting new Mol2 class to pyLiBELa:

- fixing the fatal error adding the g++ flags: -DHAVE_EIGEN; -I/usr/include/openbabel3; -I/usr/include/eigen3
- fixing an error by defining the str variable differently

How to generate the libraries for each class:

PARSER
g++ -fPIC -shared -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyPARSER.cpp -o pyPARSER.so -L/usr/lib/x86_64-linux-gnu -lboost_python310

WRITER
g++ -fPIC -shared -DBUILD=0 -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyWRITER.cpp -o pyWRITER.so -L/usr/lib/x86_64-linux-gnu -lboost_python310 -lz
 
Mol2
g++ -fPIC -shared -DHAVE_EIGEN -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyMol2.cpp -o pyMol2.so -L/usr/lib/x86_64-linux-gnu -lboost_python310 -lz

 
Adapting Grid class to pyLiBELa:
 
- boost the variables and functions of the class
- adapt the variable info as the variable str of the Mol2 class 
 
Results:
- The Grid class can be turned into a dinamic library in Google Collab and in the local machine through the following command line:

Grid
g++ -fPIC -shared -DBUILD=0 -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyGrid.cpp pyWRITER.cpp -o pyGrid.so -L/usr/lib/x86_64-linux-gnu -lboost_python310 -lz




### Week 3

Adapting COORD_MC class to pyLiBELa:
- Downloaded the RAND.h from LiBELa's source code and renamed it to pyRAND.h
- Downloaded the gsl library to local machine

Results:
- The COORD_MC class can be turned into a dinamic library in Google Collab and in the local machine through the following command line:

COORD_MC
g++ -fPIC -shared -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyCOORD_MC.cpp -o pyCOORD_MC.so -L/usr/lib/x86_64-linux-gnu -lboost_python310



Adapting FindHB class to pyLiBELa

Results:
- The FindHB class can be turned into a dinamic library in the local machine

FindHB
g++ -fPIC -shared -I/usr/include/python3.10 -I/usr/include/openbabel3 -I/usr/include/eigen3 pyCOORD_MC.cpp -o pyCOORD_MC.so -L/usr/lib/x86_64-linux-gnu -lboost_python310

- In Google Collab, the code is compiled. But, as the library is imported on python, it shows the following error message appears
![image](https://user-images.githubusercontent.com/84737515/227987066-1f2b9750-d897-4143-8cf4-edf03d3a873c.png)
- The Collab error was fixed by adding the -lopenbabel at the end


Creating a makefile:
 - Learning how to create a makefile
 - Creating a makefile to compile all the .so libraries
 - Create another one to compile on Google Collab
 
 Results:
 - Created a makefile that compiles every dinamic library with by tiping only 'make'
 - The BOOST error appears for every line of code compiled -> fix that in the future
 
 
 
 
 Useful links:
 https://embarcados.com.br/introducao-ao-makefile/
 https://github.com/alessandronascimento/LiBELa/blob/master/trunk/src/LiBELa/Makefile
 








