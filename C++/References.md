consider them just as a disguise pointer. 

```
int var = 9 ; 
int& ref = var ; 
```

Do you what happen here , nothing we just gave another name to variable block var. That is ref. we can use both var and ref to access that data block. 
No memory is created here. This ref  ==pseudo==Variable doesn't exist , It just exist in our source code. 

Nothing But syntax sugar.

### Pass By reference and Pass by pointer 
actually does the same thing. 
just pass by reference makes it slightly easier and reduce the over head of creating an extra variable that stores the address of our passed variable. 

##### also we need to initialize the ref var every time we create a new one. we can't change the value it refer to once declared.

`int& ref` --> This will through a syntax error as it is not initialized . 

```
int a = 9; 
int &ref = a ; 
int b = 3; 
ref = b ; 
```

it does similar to what would happen if we wrote `a = b `; value of a changes not that ref variable now refer b . so yea pretty basic stuff. 

```
for(auto it : adj[node]){
cout<<it<<" ";
}
or 
for(auto &it : adj[node]){
cout<<it<<" ";
}
```

so remember the second block of code is much efficient as you are not coping the data in the it variable for every iteration which is done in 1st block 


