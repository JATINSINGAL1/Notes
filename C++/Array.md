`int var[5] ; ` This is the raw way of creating an array. 
yea just created an array of size 5 . 
```
cout<<var<<endl; 
```
print the memory address of the first block as var is pointer. 

**Fun fact : The error for accessing out of bound index is received only in debug mode.**

```
// creating an array on heap
int* another = new int[5]; 
// deleting the array 
delete[] another 
```
