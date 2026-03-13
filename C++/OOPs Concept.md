# Inheritance
- Inheritance allows you to create a **hierarchy of classes**, where a **base class** (or parent class) contains common functionality, and **subclasses** (or child classes) can "branch off" from it.
- Its primary benefit is to **avoid code duplication** by putting shared code in the base class.
- When a class inherits from another (e.g., `class Player : public Entity`), the subclass automatically gains all the **members and functions** (unless they are private) of the base class.
- A subclass is considered to be of **both its own type and the base class's type** (e.g., `Player` is also an `Entity`). This allows for polymorphism, meaning a subclass object can be used wherever a base class object is expected.
### Virtual Inheritance 
```
class A { public: int x = 10; };
class B : virtual public A {};  // virtual here
class C : virtual public A {};
class D : public B, public C {};  // D has ONE A::x
```
Prevents duplicate base instances in diamond hierarchies by sharing one copy across.
Virtual keyword ensures B and C share on instance of A.
# Virtual Function 
[watch This](https://www.youtube.com/watch?v=oIV2KchSyGQ&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=27)
Using virtual function we try to override the method. 
```
class Entity { 
public : 
void PrintName(){
cout<<"Entity"; 
}
// virtual void PrintName(){ cout<<"Entity" ; }
};
classs Player : public Entity { 
public : 
void PrintName(){
cout<<"Player";
}
};

int main(){
Player p ; 
Entity* e = &p ; 
e->PrintName();
}

```

result is "Entity" simple reason during runtime our e object is considered to be entity and doesn't look for any type of overriding as we need to define it first that is done with help of keyword 
VIRTUAL 

	the new added line will be telling during runtime that there is a possibility that i can be overridden so check the v table for the function i may be pointing.
[check this also](https://www.perplexity.ai/search/using-virtual-function-we-try-9QAHU3JURZG09hxck8iTrA#0) just the summary. 


Absolute Virtual Functions 
It basically help us to define a function in a base class that doesn't have an implementation and then force subclasses to actually implement that function. 

```
class Entity { 
public : 
virtual void print() = 0 ; // declation of pure virtual function 
}
```

# Abstract Classes 
an abstract class is not same as interface, interface just contains pure virtual functions whereas abstract class can contain pure virtual functions with constructors , concrete methods 
C++ lacks true interfaces, using pure abstract classes (all virtual methods with =0) to mimic them, but abstract classes can still provide partial implementations unlike strict interfaces in Java/C#