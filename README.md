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
   * [Release Data Structures   ](#release-data-structures)
   * [Pair Container In C](#pair-container-in-c)
   * [Populate Data Structures](#populate-data-structures)
   * [Add City ](#add-city)
   * [Edit City](#edit-city)
   * [Search City](#search-city)
   * [Display List Of Cities](#display-list-of-cities)
   * [Remove City](#remove-city)
   
    
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
    
* I have a data structure that contains a size and some data. I want to free the memory when I allocate it.
   ```c
   void releaseData()
   {
   destroyAVLTree(cities);
   destroyAVLTree(citiesByName);
   destroyAVLTree(graph);
   }
   ```
  
## Pair Container In C   
***
    
*  i created a `pair` in C .  pair  is a ( Struct )container that stores two values (like a tuple). These values may or may not be of the same data-type.
*  In Project , Every Pair having one `city *` and one `AVLTree * ` Then this `AVLTree *` against data structures again will be having a `Pair` which is  `city *` and `weight *`. 
   ```c
   #ifndef __MY_PAIR_H
   #define __MY_PAIR_H 123
   typedef struct __$_my_pair
   {
   void *first;
   void *second;
   }Pair;
   #endif

     
   ```  

## Populate Data Structures   
***
    
* 
   ```c
    void populateDataStructure(int *success)
     {
      FILE *cityFile,*graphFile;
      int succ,code,acode;
      City *advCity,*city;
      Pair *p1,*p2;
      char m;
      AVLTree *advTree;
      int weight,*edgeWeight;
      City c;
      if(success)*success=false;
      cities=createAVLTree(&succ,cityCodeComparator);
      if(succ==false)
      {
      printf("Unable to load data..\n");
      return;
      }
      citiesByName=createAVLTree(&succ,cityNameComparator); 
      if(succ==false)
      {
      printf("Unable to load data...\n");
      destroyAVLTree(cities);
      return;
      }

      cityFile=fopen("city.dat","rb+");
      if(cityFile!=NULL)
      {
      fread(&cityHeader,sizeof(CityHeader),1,cityFile);
      if(!feof(cityFile))
      {
      while(1)
      {
      fread(&c,sizeof(City),1,cityFile);
      if(feof(cityFile))break;
      city=(City *)malloc(sizeof(City));
      city->code=c.code;
      strcpy(city->name,c.name);
      insertintoAVLTree(cities,city,&succ);
      insertintoAVLTree(citiesByName,city,&succ);
      }
      fclose(cityFile);
      }
      }
      if(cities->size==0)
      {
      cityHeader.lastGeneratedCode=0;
      cityHeader.recordCount=0;
      }
      graph=createAVLTree(&succ,graphVertexComparator);
      graphFile=fopen("graph.dat","r");
      if(graphFile!=NULL)
      {
      while(1)
      {
      code=0;
      m=fgetc(graphFile);
      if(feof(graphFile))break;
      code=m-48;
      while(1)
      {
      m=fgetc(graphFile);
      if(m==',' || m=='#')break;
      code=(code*10)+m-48;
      }
      c.code=code;
      city=(City *)getfromAVLTree(cities,&c,&succ);
      p1=(Pair *)malloc(sizeof(Pair));
      p1->first=(void *)city;
      advTree=createAVLTree(&succ,adjacentVertexComparator);
      p1->second=(AVLTree *)advTree;
      while(1)
      {
      acode=0;
      while(1)
      {
      m=fgetc(graphFile);
      if(m==',')break;
      acode=(acode*10)+m-48;
      }
      weight=0;
      while(1)
      {
      m=fgetc(graphFile);
      if(m==',' || m=='#' || feof(graphFile))break;
      weight=(weight*10)+m-48;
       }
      c.code=acode;
      advCity=(City *)getfromAVLTree(cities,&c,&succ);
      edgeWeight=(int *)malloc(sizeof(int));
      *edgeWeight=weight;
      p2=(Pair *)malloc(sizeof(Pair));
      p2->first=(void *)advCity;
      p2->second=(void *)edgeWeight;
      
      insertintoAVLTree(advTree,p2,&succ);
      if(m=='#' || feof(graphFile))break;
       }
       
       insertintoAVLTree(graph,p1,&succ);
       }
       fclose(graphFile);
       }
       if(success)*success=true;
        }

   ```

  
## Add City    
***
    
*
  ```c
      void addCity()
      {
      char m;
      int succ,code;
      FILE *file;
      char name[52];
      City c,*city;
      printf("Enter name of the City you want to add : ");
      fgets(name,52,stdin);
      fflush(stdin);
      name[strlen(name)-1]='\0';
      strcpy(c.name,name);
      city=(City *)getfromAVLTree(citiesByName,&c,&succ);
      if(city!=NULL) 
      {
      printf("City already exists\n");
      return;
      }
      printf("Do you wanna add City(Y/N) : ");
      m=getchar();
      fflush(stdin);
      if(m!='Y' && m!='y')
      {
      printf("City not added\n");
      return;
      }
      cityHeader.lastGeneratedCode++;
      cityHeader.recordCount++;
      code=cityHeader.lastGeneratedCode;
      c.code=code;
      file=fopen("city.dat","rb+");
      if(file==NULL)
      {
      file=fopen("city.dat","wb+");
      fwrite(&cityHeader,sizeof(CityHeader),1,file);
       }
      else
      {
      fseek(file,0,SEEK_END);
      }
      fwrite(&c,sizeof(City),1,file);
      fseek(file,0,SEEK_SET);
      fwrite(&cityHeader,sizeof(CityHeader),1,file);
      fclose(file);
      city=(City *)malloc(sizeof(City));
      city->code=c.code;
      strcpy(city->name,c.name);
      insertintoAVLTree(cities,city,&succ);
      insertintoAVLTree(citiesByName,city,&succ);
      printf("City added\n");
      drawLine('-',50);
      printf("Press ENTER to continue..."); 
      getchar();
      fflush(stdin);
      }

     
   ```  

## Edit City   
***
    
* Edit city
   ```c
    void editCity()
    {
    CityHeader cHeader;
    int x,succ,code;
    char name[52],newName[52],m;
    City *city1,*city2,c;
    FILE *file;
    printf("EDIT MODULE\n");
    printf("Enter name of the City to EDIT : ");
    fgets(name,52,stdin);
    fflush(stdin);
    name[strlen(name)-1]='\0';
    strcpy(c.name,name);
    city1=(City *)getfromAVLTree(citiesByName,&c,&succ);
    if(!succ)
    {
    printf("City doesn't exist\n");
    return;
    }
    printf("Enter new Name : ");
    fgets(newName,52,stdin);
    fflush(stdin);
    newName[strlen(newName)-1]='\0';
    if(strcmp(newName,name)==0)
    {
    printf("No need to update\n");
    return;
    }
    strcpy(c.name,newName);
    city2=(City *)getfromAVLTree(citiesByName,&c,&succ);
    if(succ && city1->code!=city2->code)
    {
    printf("This city already exists\n");
    return;
    }
    printf("Do you wanna update(Y/N) : ");
    m=getchar();
    fflush(stdin);
    if(m!='y' && m!='Y')
    {
    printf("City NOT edited\n");
    return;
    }
    file=fopen("city.dat","rb+");
    fread(&cityHeader,sizeof(CityHeader),1,file);
    while(1)
     {
    x=ftell(file);
    fread(&c,sizeof(City),1,file);
    if(strcmp(name,c.name)==0)break;
     }
    fseek(file,x,SEEK_SET);
    strcpy(c.name,newName);
    c.code=city1->code;
    fwrite(&c,sizeof(City),1,file);
     fclose(file);
     strcpy(c.name,name);
     c.code=city1->code;
     strcpy(city1->name,newName);
    removefromAVLTree(citiesByName,&c,&succ);
    removefromAVLTree(cities,&c,&succ);
    insertintoAVLTree(citiesByName,city1,&succ);
    insertintoAVLTree(cities,city1,&succ);
    printf("City EDITED, press ENTER to continue...");
    getchar();
    fflush(stdin);
      }

   
   
   
   ```
    
      
## Search City   
***
    
* Search City
```c
  void searchCity()
  {
  int succ,*advWeight;
  char name[52];
  Pair *graphPair,*advPair,gPair;
  AVLTree *advTree;
  AVLTreeInOrderIterator advIterator;
  City *city,c,*advCity;
  printf("Enter a city name to search : ");
  fgets(name,52,stdin);
  fflush(stdin);
  name[strlen(name)-1]='\0';
  strcpy(c.name,name);
  city=(City *)getfromAVLTree(citiesByName,&c,&succ);
  if(succ)
  {
  printf("Found\n");
  printf("%s\n",city->name);
  }
  else 
  {
  printf("%s doesn't exists\n",name);
  return;
  }
  gPair.first=(void *)city;
  graphPair=(Pair *)getfromAVLTree(graph,&gPair,&succ);
  if(graphPair==NULL)return;
  advTree=(AVLTree *)graphPair->second;
  advIterator=getAVLTreeInOrderIterator(advTree,&succ);
  printf("Following are the cities adjacent to %s\n",city->name);
  while(HasNextInorderelementAVLTree(&advIterator))
  {
  advPair=(Pair *)getnextinorderelementfromAVLTree(&advIterator,&succ);
  advCity=(City *)advPair->first;
  advWeight=(int *)advPair->second;
  printf("City : %-18s | Distance : %d \n",advCity->name,*advWeight);
  }
  }

 
     
   ```  
## Remove City   
***
    
*  
 ```c
  
    void removeCity()
    {
    FILE *file;
    FILE *tmpFile;
    CityHeader cHeader;
    char name[52],m;
    int x,succ;
    City c,*city;
    printf("DELETE MODULE\n");
    printf("Enter name of the City to delete : ");
    fgets(name,52,stdin);
    fflush(stdin);
    name[strlen(name)-1]='\0';
    strcpy(c.name,name);
    city=(City *)getfromAVLTree(citiesByName,&c,&succ);
    if(!succ)
    {
    printf("City doesn't exists\n");
    return;
     }
    printf("City Found\nName : %s\n",city->name);
    printf("Do you wanna delete City(Y/N) : ");
    m=getchar();
    fflush(stdin);
    if(m!='y' && m!='Y')
    {
    printf("City not removed\n");
    return;
     }
    file=fopen("city.dat","rb");
    tmpFile=fopen("tmp.tmp","wb");
    fread(&cHeader,sizeof(CityHeader),1,file);
    cityHeader.recordCount--;
    fwrite(&cityHeader,sizeof(CityHeader),1,tmpFile);
     while(1)
     {
    fread(&c,sizeof(City),1,file);
    if(feof(file))break;
    if(stricmp(city->name,c.name)!=0)
    {
    fwrite(&c,sizeof(City),1,tmpFile);
    }
    }
    fclose(file);
    fclose(tmpFile);
    file=fopen("city.dat","wb");
    tmpFile=fopen("tmp.tmp","rb");
    fread(&cHeader,sizeof(CityHeader),1,tmpFile);
    fwrite(&cHeader,sizeof(CityHeader),1,file);
    while(1)
    {
    fread(&c,sizeof(City),1,tmpFile);
    if(feof(tmpFile))break;
    fwrite(&c,sizeof(City),1,file);
    }
    fclose(file);
    fclose(tmpFile);
    tmpFile=fopen("tmp.tmp","wb");
    fclose(tmpFile);
    c.code=city->code;
    strcpy(c.name,city->name);
    removefromAVLTree(cities,&c,&succ);
    removefromAVLTree(citiesByName,&c,&succ);
    free(city);
    printf("City %s deleted, Press ENTER to continue....",name);
    getchar();
    fflush(stdin);
    }

  
  
     
   ```  




## Display List Of Cities  
***
    

 ```c
    void displayListOfCity()
    {
    City *city;
    int pageNo,sno,succ;
    AVLTreeInOrderIterator iterator;
    sno=0;
    int newPage=5;
    if(cities->size==0)
    {
    drawLine('-',50);
    printf("NO Records added.press ENTER to continue....\n"); 
    drawLine('-',50);
    getchar();  
    fflush(stdin);
    return;
    }
    iterator=getAVLTreeInOrderIterator(citiesByName,&succ);
    if(succ)
    {
    pageNo=0;
    while(HasNextInorderelementAVLTree(&iterator))
    {
    city=(City *)getnextinorderelementfromAVLTree(&iterator,&succ);
    if(sno%newPage==0)
    {
    pageNo++;
    drawLine('-',50);
    printf("S NO.   NAME OF THE CITIES %d\n",pageNo);
    drawLine('-',50);
    }
    sno++;
    printf("%2d       %-52s %d\n",sno,city->name,city->code);
    if(sno%newPage==0)
     {
    drawLine('-',50);
    printf("press ENTER to continue...");
    getchar();
    fflush(stdin);
     }
     }
    if(cities->size==0) 
    {
    drawLine('-',50);
    printf("NO Records added.press ENTER to continue....\n"); 
    drawLine('-',50);
    getchar();
    fflush(stdin);
     return;
     }
     }
     drawLine('-',50);
     }
     
  ```  


  
  
