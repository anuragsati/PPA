### Class
class = collection of related data and functions
every instance of class is called object of that class and data / functions is held by instances not class
classes methods may behave differenly for diff objects bcz diff objects have diff. data

- data members are not part of class they are part of object i.e diff objects will have diff. data member values

- data members of class does not exist in memory until objects are created
    ```c++
        class A{
            int var = 10;   
            //this doesnot exist yet and whatever object is created that object will have var = 10 as default
        }
    ```

- always hide instance variables







### Access Modifier
[default:private]

- Private :
    can only be accessed from inside class
    we keep data members private to add security and so that others might not accidently change it
    we might want user to not access some features of our class so we keep it private 
    we can make a function public in class that changes private variable

    ```c++
        class{
            private:
                a = 5;
            public:
                changea(int x){
                    this.a = x;
                }
        }
    ```

- Public : 
    can be accessed everywhere

- Protected :
    can be accessed from within class 
    can be accessed from classes which are inheriting current class (child classes)








### static variables
instance variables(data members) are part of object
but 
static variables are part of class 
means for one class there will be only one such variable in memory and all objects of this class will be using this variable
static is not different for all objects rather it is common across all objects

static variables are only cleated once in memory

    Q. create a program that tracks the number of objects a class have

    class A{
        static int num;

        A(){
            num++;
        }
    }

create static variables when you want something to be at the level of class and not for each object

drawbacks : 
    in multithreaded systems if multiple objects are using one static variable then it might lead to race condition
    we can use lock or semaphore to solve










### static method

    Q. how to prevent user from creating object of certain class ?
        make the constructor of that class private


    Q. can we access non static members from static method ?
        a big `NO`

        static members are global and are created only once 
        static method cant acess non static method or variable

        ```c++
        class A{
            int local = 4;

            static int getThing(){
                this.local = 3;          //this method will not know which local you are talking about bcz this is static 
                return 1;
            }
        }
        ```

we can create static method when it has nothing to do with instance of objects

- static variables are ininitalized during class loading step (before object creation)










### constructors
- no return type
- used to initialize the instance variables of new object

- default constructor exists if and only if we dont mention any constructor of class
    as soon as we mention any constructor, the default one vanishes

- constructor overloading : we can have multiple constructors with diff. arguments

- when you want certain objects to have compulsory default value then we create a constructor for it


copy constructor : 
    copies value of one object into other object
    basically if we want one object to be same as other object

    ```c++
    class A{
        A(){
            this.x = 10;
            this.y = 20;
        }

        A(A oldobject){
            this.x = A.x;
            this.y = A.y;
        }
    }

    A obj1;     //10, 20
    A obj2(obj1)    //10, 20 (copied from obj1)
    ```


### [This] keyword
used for self referencing purpose
this means get the object which has been used to invoke this function

- this can be used on functions as well as variables
- this can't be used on static things as they are local to object

- using this we can call constructors (also called constructor chaining)

    class file{
    public:
        file(){
            cout << "default" << endl;
        }

        file(int x){
            file();
            cout << "hey " << x << endl;
        }
    };

- we can send or return current object using this







# ================================= PRINCIPLES OF OOP =================================

- Abstraction
- Encapsulation
- Polymorphism
- Inheritance




# Abstraction
the art of showing only that code which is useful and abstracting out other
making data / functions into private / public  ...

ex : we use private to hide all the functions that are not necessary for client and using public we only show useful functions
ex : making something complex into functions such as black box (ex sqrt function)




# Encapsulation (data hiding)
when an object expresses only selected info
- bundling of data with methods that operate on that data
i.e you cant access private data ... every private data is bundled with its own class and getter and setter 


- data members must be private so that others will not be able to directly access them
- we can only access private things by public getters and setters with right set of rules