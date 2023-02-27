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
   * [Add Adjacent Vertex Against City](#add-adjacent-vertex-against-city)
   * [Remove Adjacent Vertex Against City](#remove-adjacent-vertex-against-city)
   
   
    
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
   
   
   
   
   
   
   ![add](https://user-images.githubusercontent.com/109301830/221589013-923a8347-74bf-4c1b-b769-c04567413cf5.jpg)

   
   
   
   
   
   
   
   
   

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
   
   
   
   
   
   
   
   ![edit](https://user-images.githubusercontent.com/109301830/221589730-bcd9e778-dd61-4c52-ba9c-a10029114634.jpg)

   
   
   
   
   
   
   
   
   
   
   
    
      
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
   
   
   ![search](https://user-images.githubusercontent.com/109301830/221590097-534eb35f-b5a9-447a-ab4b-8bad741b3406.jpg)

   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
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
   
   
   

![delete](https://user-images.githubusercontent.com/109301830/221590431-700763fa-cf8d-4c1a-baf9-5efe79eada39.jpg)



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



![display](https://user-images.githubusercontent.com/109301830/221591225-901edbec-2240-4923-b2d0-98afe6ee2eec.jpg)












## Add Adjacent Vertex Against City  
***
    
*
 ```c
    void addAdjacentVertex()
    {
    AVLTreeInOrderIterator graphIterator,advIterator;
    FILE *graphFile;
    char m,name[52],advName[52];
    int shouldAppend,succ,weight,*edgeWeight,*wgt;
    City *gct,c,*city,*advCity,*act;
    Pair *graphPair,*advPair,*gp,*ap,aPair,gPair;
    AVLTree *advt,*advTree;

    printf("Enter City against which you wanna add adjacent city : ");
    fgets(name,52,stdin);
    fflush(stdin);
    name[strlen(name)-1]='\0';
    strcpy(c.name,name);
    city=(City *)getfromAVLTree(citiesByName,&c,&succ);
    if(!succ)
    {
    printf("This City doesn't exist\n");
    printf("Press ENTER to continue...");
    getchar();
    fflush(stdin);
    return;
    }
    shouldAppend=0;
    gPair.first=(void *)city;
    graphPair=(Pair *)getfromAVLTree(graph,&gPair,&succ);
    if(!succ)
    {
    shouldAppend=1;
    graphPair=NULL;
    }
    if(graphPair!=NULL)advTree=(AVLTree *)graphPair->second;

    while(1)
    { 
    printf("Enter adjacent  City : ");
    fgets(advName,52,stdin);
    fflush(stdin);
    advName[strlen(advName)-1]='\0';
    strcpy(c.name,advName);
    advCity=(City *)getfromAVLTree(citiesByName,&c,&succ);
    if(!succ)
    {
    printf("City %s doesn't exist\n",advName);
    printf("Wanna add another city as adjacent to %s(Y/N) : ",city->name);
    m=getchar();
    fflush(stdin);
    if(m!='y' && m!='Y')break;
    continue;
    }
    aPair.first=(void *)advCity;
    if(graphPair!=NULL)
    {
    advPair=(Pair *)getfromAVLTree(advTree,&aPair,&succ);
    if(succ)
    {
    printf("%s is already adjacent to %s\n",advCity->name,city->name);
    printf("Wanna add another city (Y/N) : ");
    m=getchar();
    fflush(stdin);
    if(m!='y' && m!='Y')break;
    continue;
    }
    }
    
    printf("Enter weight from %s to %s : ",city->name,advCity->name);
    scanf("%d",&weight);
    fflush(stdin);
    if(weight<=0)
    {
    printf("Invalid Entry\nWanna add another adjacent city (Y/N) : ");
    m=getchar();
    fflush(stdin);
    if(m!='y' && m!='Y');break;
    continue;
    }
    if(graphPair!=NULL)advTree=(AVLTree *)graphPair->second;
    advPair=(Pair *)malloc(sizeof(Pair));
    advPair->first=(void *)advCity;
    edgeWeight=(int *)malloc(sizeof(int));
    *edgeWeight=weight;
    advPair->second=(void *)edgeWeight;
    if(graphPair==NULL)
    {
    graphPair=(Pair *)malloc(sizeof(Pair));
    graphPair->first=(void *)city;
    advTree=createAVLTree(&succ,adjacentVertexComparator);
    graphPair->second=(void *)advTree;
    insertintoAVLTree(advTree,advPair,&succ);
    insertintoAVLTree(graph,graphPair,&succ);
    graphFile=fopen("graph.dat","a");
    fprintf(graphFile,"%d,%d,%d#",city->code,advCity->code,weight);
    fclose(graphFile);
    }
    else if(shouldAppend==1)
    {
    insertintoAVLTree(advTree,advPair,&succ);
    graphFile=fopen("graph.dat","r+");
    fseek(graphFile,-1,SEEK_END);
    fprintf(graphFile,",%d,%d#",advCity->code,weight);
    fclose(graphFile);
    }
    else
    {
    insertintoAVLTree(advTree,advPair,&succ);
    createGraphFile();
    }
    printf("Information saved\n");
    printf("Wanna add more adjacent city to %s(Y/N) : ",city->name);
    m=getchar();
    fflush(stdin);
    if(m!='y' && m!='Y')break;
    continue;
    }
    }  
  ```  





![add adjacent city](https://user-images.githubusercontent.com/109301830/221591479-fd01834e-8594-4a84-a24b-5bc48521673b.jpg)








  
## Remove Adjacent Vertex Against City   
***
    
* 

   ```c
     void removeAdjacentVertex()
     {
     AVLTreeInOrderIterator graphIterator,advIterator;
     FILE *graphFile;
     char m,name[52],advName[52];
     int shouldAppend,succ,weight,*edgeWeight,*wgt;  
     City *gct,c,*city,*advCity,*act;
     Pair *graphPair,*advPair,*gp,*ap,aPair,gPair;
     AVLTree *advt,*advTree;

     printf("Enter City against which you wanna remove adjacent city : ");
     fgets(name,52,stdin);
     fflush(stdin);
     name[strlen(name)-1]='\0';
     strcpy(c.name,name);
     city=(City *)getfromAVLTree(citiesByName,&c,&succ);
     if(!succ)
     {
     printf("This City doesn't exist\n");
     printf("Press ENTER to continue...");
     getchar();
     fflush(stdin);
     return;
     }
     gPair.first=(void *)city;
     graphPair=(Pair *)getfromAVLTree(graph,&gPair,&succ); 
     if(graphPair==NULL)
     {
     printf("There is no Adjacent City of this City\nPress ENTER to continue...");
     getchar();
     fflush(stdin);
     return;
     }
     advTree=(AVLTree *)graphPair->second;
     printf("Enter Adjacent City : ");
     fgets(advName,52,stdin);
     fflush(stdin);
     advName[strlen(advName)-1]='\0';
     strcpy(c.name,advName);
     advCity=(City *)getfromAVLTree(citiesByName,&c,&succ);
     if(!succ)
     {
     printf("City doesn't exist\nPress ENTER to continue...");
     getchar();
     fflush(stdin);
     return;
     }
     aPair.first=(void *)advCity;
     advPair=(Pair *)getfromAVLTree(advTree,&aPair,&succ);
     if(!succ)
     {
     printf("City doesn't exist\nPress ENTER to continue...");
     getchar();
     fflush(stdin);
     return;
     }
     removefromAVLTree(advTree,advPair,&succ);
     createGraphFile();
     printf("Information saved\nPress ENTER to continue..."); 
     getchar();
     fflush(stdin);
     }

    
    
     void createGraphFile()
     {
     FILE *graphFile;
     AVLTreeInOrderIterator graphIterator,advIterator;
     Pair *gp,*ap;
     City *gct,*act;
     AVLTree *advt;
     int *wgt,succ;
     graphFile=fopen("graph.dat","w");
     graphIterator=getAVLTreeInOrderIterator(graph,&succ);
     while(HasNextInorderelementAVLTree(&graphIterator))
     {
     gp=(Pair *)getnextinorderelementfromAVLTree(&graphIterator,&succ);
     gct=(City *)gp->first;
     advt=(AVLTree *)gp->second;
     fprintf(graphFile,"%d,",gct->code);
     advIterator=getAVLTreeInOrderIterator(advt,&succ);
     while(HasNextInorderelementAVLTree(&advIterator))
     {
     ap=(Pair *)getnextinorderelementfromAVLTree(&advIterator,&succ);
     act=(City *)ap->first;
     wgt=(int *)ap->second;
     if(HasNextInorderelementAVLTree(&advIterator))
     {
     fprintf(graphFile,"%d,%d,",act->code,*wgt);
     }
     else 
     {
     fprintf(graphFile,"%d,%d#",act->code,*wgt);
     }
     }
     }
     fclose(graphFile);
     }

  ```





![remove adjacent city](https://user-images.githubusercontent.com/109301830/221600459-2b8219ac-df01-40a0-9ae6-13c015074083.jpg)





## Get Route   
***
    
 ```c
 
   void searchRoute()
   {
   int distanceComparator(void *left,void *right)
   {
   Pair *leftPair,*rightPair;
   int *leftDistance,*rightDistance;
   leftPair=(Pair *)left;
   rightPair=(Pair *)right;
   leftDistance=(int *)leftPair->second;
   rightDistance=(int *)rightPair->second;
   return *leftDistance-*rightDistance;
   }
   typedef struct _path
   {
   City *city;
   City *arrivedFromCity;
   int totalDistanceFromStarting;
   }Path;
   int pathComparator(void *left,void *right)
   {
   Path *leftPath,*rightPath;
   leftPath=(Path *)left;
   rightPath=(Path *)right;
   return leftPath->city->code-rightPath->city->code;;
   }

   AVLTreeInOrderIterator iterator;
   int i,*td,*totalDistance,*totalDistanceFromSourceCity,*edgeWeight,sum,found,routeExists;
   int succ;
   Path *pathStructurePointer,pathStructure;
   Pair *graphPair,gPair,*advPair;
   AVLTree *pathTree,*advTree;
   PQueue *pqueue;
   Stack *stack;
   Pair *tmpPair,*PQueuePair;
   char sourceCityName[52],targetCityName[52];
   City c,*city,*sourceCity,*targetCity,*advCity,*tmpCity;

   
   printf("Enter SOURCE city : ");
   fgets(sourceCityName,52,stdin);
   fflush(stdin);
   sourceCityName[strlen(sourceCityName)-1]='\0';
   strcpy(c.name,sourceCityName);
   sourceCity=(City *)getfromAVLTree(citiesByName,&c,&succ);
   if(!succ)
   {
   printf("City DOESN'T exists\n");
   return;
   }

   printf("Enter TARGET city : ");
   fgets(targetCityName,52,stdin);
   fflush(stdin);
   targetCityName[strlen(targetCityName)-1]='\0';
   strcpy(c.name,targetCityName);
   targetCity=(City *)getfromAVLTree(citiesByName,&c,&succ);
   if(!succ)
   {
   printf("City DOESN'T exists\n");
   return;
   }


   pathTree=createAVLTree(&succ,pathComparator);
   pqueue=createPQueue(distanceComparator,&succ);
   PQueuePair=(Pair *)malloc(sizeof(Pair));
   PQueuePair->first=sourceCity;
   totalDistanceFromSourceCity=(int *)malloc(sizeof(int));
   *totalDistanceFromSourceCity=0;
   PQueuePair->second=totalDistanceFromSourceCity;
   AddtoPQueue(pqueue,PQueuePair,&succ);
   pathStructurePointer=(Path *)malloc(sizeof(Path));
   pathStructurePointer->city=sourceCity;
   pathStructurePointer->arrivedFromCity=NULL;
   pathStructurePointer->totalDistanceFromStarting=0;
   insertintoAVLTree(pathTree,pathStructurePointer,&succ);
   routeExists=0;
   while(!isPQueueEmpty(pqueue))
   {
   PQueuePair=(Pair *)RemovefromPQueue(pqueue,&succ);
   city=(City *)PQueuePair->first;
   if(city==targetCity)
   {
   routeExists=1;
   break;
   }
   totalDistance=PQueuePair->second;
   gPair.first=city; 
   graphPair=(Pair *)getfromAVLTree(graph,&gPair,&succ);
   if(!succ)continue;
   if(graphPair->second==NULL)continue;
   advTree=(AVLTree *)graphPair->second;
   iterator=getAVLTreeInOrderIterator(advTree,&succ);
   while(HasNextInorderelementAVLTree(&iterator))
   {
   advPair=getnextinorderelementfromAVLTree(&iterator,&succ);
   advCity=(City *)advPair->first;
   edgeWeight=(int *)advPair->second;
   sum=*edgeWeight+*totalDistance;
   pathStructure.city=advCity;
   pathStructurePointer=getfromAVLTree(pathTree,&pathStructure,&succ);
   if(succ)
   {
   if(sum>pathStructurePointer->totalDistanceFromStarting)continue;
   pathStructurePointer->totalDistanceFromStarting=sum;
   pathStructurePointer->arrivedFromCity=city;
   found=false;
   for(i=0;i<getsizeofPQueue(pqueue);i++)
   {
   tmpPair=(Pair *)getElementfromPQueue(pqueue,i,&succ);
   tmpCity=tmpPair->first;
   if(tmpCity->code==advCity->code)
   {
   found=true;
   break;
   }
   }
   if(found)
   {
   *td=sum;
   tmpPair->second=td;
   UpdateElementInPQueue(pqueue,i,tmpPair,&succ);
   continue;
   }
   PQueuePair=(Pair *)malloc(sizeof(Pair));
   PQueuePair->first=advCity;
   totalDistanceFromSourceCity=(int *)malloc(sizeof(int));
   *totalDistanceFromSourceCity=sum;
   PQueuePair->second=totalDistanceFromSourceCity;
   AddtoPQueue(pqueue,PQueuePair,&succ);
   }
   else
   {
   PQueuePair=(Pair *)malloc(sizeof(Pair));
   PQueuePair->first=advCity;
   td=(int *)malloc(sizeof(int));
   *td=sum;
   PQueuePair->second=td;
   AddtoPQueue(pqueue,PQueuePair,&succ);

   pathStructurePointer=(Path *)malloc(sizeof(Path));
   pathStructurePointer->city=advCity;
   pathStructurePointer->arrivedFromCity=city;
   pathStructurePointer->totalDistanceFromStarting=sum;
   insertintoAVLTree(pathTree,pathStructurePointer,&succ);
   }
   }
   }
   if(!routeExists)
   {
   printf("NO ROUTE EXISTS from %s to %s\n",sourceCityName,targetCityName);
   return;
   }
   stack=createStack(&succ);
   pathStructure.city=targetCity;
   while(1)
   {
   pathStructurePointer=getfromAVLTree(pathTree,&pathStructure,&succ);
   pushonStack(stack,pathStructurePointer,&succ);
   if(pathStructurePointer->city==sourceCity)break;
   pathStructure.city=pathStructurePointer->arrivedFromCity;
   }
   while(!isStackempty(stack))
   {
   pathStructurePointer=(Path *)popfromStack(stack,&succ);
   printf("%s (%d)\n",pathStructurePointer->city->name,pathStructurePointer->totalDistanceFromStarting);
   }
   }

 
 
 
   ```
   
   
   
   
   ![get route](https://user-images.githubusercontent.com/109301830/221600617-d9596dd6-ba68-46a1-b432-04c013e3b4ad.jpg)











