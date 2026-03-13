
Have you ever wondered how vector's work?
DE Shaw Problem of one of my friend. 
```
vector<int> v; 
v.push_back(1); 
int *ptr = &v[0]; 
v.push_back(2); aa
cout<<*ptr; 
```
what will be the result 
The answer is unexpected result , yes it's not 1. 

Let's see how vectors work? 
when them vector is created some memory gets allocated in the heap but as the vector resize itself it doesn't extend from there itself it finds the bigger memory slot and copy all the data from the previous memory to the new memory freeing up the old memory. 
So in our case the ptr is a dangling pointer leading to unexpected behavior. 

The amount of size allocated is usually twice than the previous size. 

#### Optimization in Vector

using `std::vector::reserve()` to pre-allocate enough memory if you know the number of elements you'll be adding, preventing multiple reallocations

When using `push_back()`, a temporary object is often created in the current function's stack and then copied into the vector's memory. To avoid this extra copy, the video recommends using `emplace_back()`, which constructs the object directly "in place" within the vector's allocated memory, eliminating the need for a copy