**difference between struct and class is none except that the access identifier by default is public.** 
primarily we use struct for storing just data. 
It is datatype which help us to group variables and function together. 
```
class player{
 int a = 0 ; 
 int b = 2 ; 

 int fun(){
	return a + b ; 
	}
}
```

object : It is the instance of that class. `player p` here p is the object 

 ==. and -> both can be used to access the data members of Class==

There comes access identifiers : 
Public : everyone . 
Private : No one except the class itself 
Protected : All subclass , the child classes can access this member 

# Constructor 

The are special type of method used when class is initialized. 
doesn't have a return type not even void and its name matches with that of class name. 

If you don't define a constructor that doesn't mean class doesn't have one it just have a default which looks like 
```
class player{
float X , Y ; 
	player(){
	// default type of constructor 
	// in java int and float are specified to zero that's not the case in cpp. 
		}
		
	player(float x ,  float y ){
	// we define our own constructor 
	X = x ; 
	Y = y ; 
	}
};
```

if we put our constructor inside private access , this would not allow us to create an object of that class. Kind of locking the class. 

```
class Log{
Log()= delete ; 
}
```
this also tell the compiler to delete the default constructor which indeed stop the program to create the instances of the class. 
### Member Initializer list 
```
// example 
player(int x) // can be done for this type as well 
	: X(x) , Y(7) // this is how it is done simple set X = x , Y = 7 also you need to write you variable in order as that you defined in class. 
	{
	}
```

Why in first place we need this to do there is a functional performance overhead.
[start from 4min](https://www.youtube.com/watch?v=1nfuYMXjZsA&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=35)
# Destructor
```
class entity{
~entity(){
// this ~ simiply help in forming the destructor 
}
}
```
would be used when you create something using constructor function which would no longer server purpose , need to be destroyed. To free the memory. 

Destructor can't be overloaded. like that Constructor. 

# Different ways to create an object
stack allocation `Player player(2,3)` most manageable and easy way to create an object. 
heap allocation `Player* player = new Player(2,3)` player is pointer which tell about the address of object of class Player created inside the heap
To free the memory we `delete player`

# This - Keyword
only accessible through member function
and it is pointer to current object instance that method belong to. 
```
class Player{
int x , y ; 
Player(int x , int y){
x = x; // this is meaning less. 
this->x = x ; 
or
(*this).x = x ; // left side x is the member of class and right on is input from the method 
}

}
```
`this` is a special pointer available inside every non-static member function. It points to the **address of the current object** calling the function. 

#### In OOPs it is quite common for us to create a class that consist only of unimplemented methods and then force a subclass to actually implement them. Referred to as Interface. 
Interface is just a class of unimplemented methods acting like some sort of template , 
That's why it is not possible for us to instantiate the interface.  

# Friend Class and Friend Function 
Special function that can access protected and private data both from a class , even though it is not a member of the class. 
Similarly with class it can access the private and protected data members of the class
https://www.perplexity.ai/search/implementation-friend-class-an-NmbjeEQCQhSvF58Uz58Kgg#0 rewamp this stuff . 