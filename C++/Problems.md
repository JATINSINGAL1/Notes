What will be the output of this code?
```
int* createArray(){
int array[50]; 
return array ; 
}

int main(){fgh
int* arr = createArray(); 
arr[0] = 1 ; 
arr[1] = 2 ; 
cout<<arr[0]<<arr[1]; 
}
```

concept : dangling pointer