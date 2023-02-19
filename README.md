# DOCUMENTATION

## Overview
 Using `Graph Algorithm` and `Various Data Structures of Generic Library` inspired by `STL` in C++ and  CRUD      Operations. 



## City Management System

  
* Language Used :- `C` Programming.

* I have created a small project using the library of Data Structures, with the help of which all the Data   Structures are used to know how to use them.

* i proposed this system with the help of File Handling for storing data and routes of cities and various Data Structure for traversing various cities and add their adjacent vertex with the help of Graph algorithms. Comparator plays major role in our project.

* With this system we can easily do some operations like add, edit, update, delete information of cities (CRUD Operations) on them . Apart from that user can find the shortest path between the two cities.

* The main technology used in this project is purely C language. Data saves in a file using file handling. Data structures maintain a sequential format and play a key role behind the success of this project.

* The information of all the cities(name) is resulted in a sorted manner and we can also add adjacent vertex  and edges between more than two cities.

* In addition, it also shows the distance between the source and the target destination



## This is how the data structure is maintained




   * [Various Header Files and Struct](#various-header-files-and-struct)
   * [Description Of Comparator](#description-of-comparator) 
    
## Various Header Files and Struct   
***
### Description of Header Files  :
    
* In C language, header files contain the set of predefined standard library functions. You request to use a header file 
    in your program by including it with the C preprocessing directive “#include”.
   ```c
    #include<stdio.h>
    #include<string.h>
    #include<my_pair.h>
    #include<stdlib.h>
    #include<my_pqueue.h>
    #include<my_avl_tree.h>
   ```

 * Structures (also called structs) are a way to group several related variables into one place. Each variable in the structure is known as a member of    the structure.
   Unlike an array, a structure can contain many different data types (int, float, char, etc.).
   ```c
   typedef struct __city
   {
   int code;
   char name [52];
   }City;

   typedef struct _city_header
   {
   int lastGeneratedCode;
   int recordCount;
   }CityHeader;

   CityHeader cityHeader;
   AVLTree *cities;
   AVLTree *citiesByName;
   AVLTree *graph;

   ```

## Description Of Comparator 
***

    
*  The comparator function takes two arguments and contains logic to decide their relative order in sorted output.
   ```c
   int cityCodeComparator(void *p,void *q)
   {
   City *a=(City *)p;
   City *b=(City *)q;
   return a->code-b->code;
   }

   int cityNameComparator(void *p,void *q)
   {
   City *a=(City *)p;
   City *b=(City *)q;
   return stricmp(a->name,b->name);
   }

   int adjacentVertexComparator(void *left,void *right)
   {
   Pair *leftPair,*rightPair;
   City *leftCity,*rightCity;
   leftPair=(Pair *)left;
   rightPair=(Pair *)right;
   leftCity=(City *)leftPair->first;
   rightCity=(City *)rightPair->first;
   stricmp(leftCity->name,rightCity->name);
   }

   int graphVertexComparator(void *left,void *right)
   {
   Pair *leftPair,*rightPair;
   City *leftCity,*rightCity;
   leftPair=(Pair *)left;
   rightPair=(Pair *)right;
   leftCity=(City *)leftPair->first;
   rightCity=(City *)rightPair->first;
   stricmp(leftCity->name,rightCity->name);
    }  
   ```

## Release Data Structures   
***
### Description of Header Files  :
    
* In C language, header files contain the set of predefined standard library functions. You request to use a header file 
    in your program by including it with the C preprocessing directive “#include”.
   ```c
    #include<stdio.h>
    #include<string.h>
    #include<my_pair.h>
    #include<stdlib.h>
    #include<my_pqueue.h>
    #include<my_avl_tree.h>
   ```
  




    
    
  
