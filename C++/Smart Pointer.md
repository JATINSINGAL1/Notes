
To access these smart pointers library used is memory `#include <memory>`
## Unique Pointer 
it is a scoped pointer, as it get out of scope get destroyed and will call delete. You can't copy unique pointer for the simple purpose if anyone dies it frees the resource creating the other a dangling pointer and as consequence unexpected results. 

```
int main(){
	{ 
	std:: unique_ptr<Player> player(new Player()); 
	// prefered way to create 
	std:: unique_ptr<Player> player = std:: make_unique<Player>(); 
	player->any_fun(); 
	}
}
```
more about it later like: "Exception safety" 

## Shared Pointer 
The way they are implemented depends upon the compiler and the standard library we use with our compiler. 
IT majorly works on reference counting principle. It is about counting the no of reference we have of our pointer. 

```
std:: shared_ptr<Player> sharedPlayer = std:: make_shared<Player>(); 
std:: shared_ptr<Player> sharedPlayer = new Player();
```
shared pointer allocates another block of memory called control block for storing the reference count

the issue with second line is first object is created inside the heap than control block is allocated creating 2 allocation. 
And when we use make_shared it allocates the memory for the object and control block together. 


## Weak Pointer 
Basically it doesn't take accountability like when a shared ptr is assigned to weak ptr it doesn't increase the reference count so when the scope of shared ptr ends the ptr dies making the weak pointer similar to dangling pointer but we can ask it are you expired or not this help us out with errors. 