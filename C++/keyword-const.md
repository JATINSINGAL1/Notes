### Used with Pointer 
`const int* a = new int;` define that content stored inside the address of a is const or  * a is const 

`int * const a = new int; ` data inside variable a is const 

so `const int a = 9; ` and `int const a = 9;` are same. 



### Used with class or struct 
```
class entity{
private : 
int x , y ; 
mutable int var ; 
public: 
int getX() const 
{
var = 2; // this is allowed 
x = 8 ; // this line will through compile error 
return x ; 
}
};


// must be wondering yes if we are not changing anything inside the funtion why do we need const 
reason 

void Print(const entity &e ){
// passed by refrence with const key word making sure nothing is changed for the object

cout<<e.getX(); // this function can only be called if it is ensured that this function will not change the object e. So const keyword after the function is required. 
}

```

using const with method name only works with class and struct we promise not to change the class inside the method. It's just read only method. 

## Mutable 
`mutable int x ; ` it just give access to change x in any function. 

using mutable inside lambda : talk about it later . 