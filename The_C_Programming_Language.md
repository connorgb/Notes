# The C Programming Language

## CH 5: Pointers and Arrays
### Pointers and Addresses
* A pointer is a variable that contains the address of a variable.
* The operator & gives the address of an object, so the statement `p = &c;` assigns the address of c to the variable p, and p is said to “point to” c.
* The operator * is the indirection or dereferencing operator. When applied to a pointer, it accesses the object the pointer points to.
* If y points to x, then *y can occur in any context where x could.
* The assignment `y = *x +1` takes whatever x points at, adds 1, and assigns the result to y.
* In the case of `(*x)++` the parentheses are necessary, as without them the expression would increment the pointer x instead of what it points to (unary operators like * and ++ associate left to right).
* If x and y are both pointers to int, then `x = y` makes x point to whatever y pointed to.
### Pointers and Function Arguments
* Since C passes arguments to functions by value, there is no direct way for the called function to alter a variable in the calling function. Hence, it is not enough to write `swap(a, b);` where the swap function is defined as:
```c
void swap(int x, int y)     /* wrong */
{
    int temp;
    temp = x;
    x = y;
    y = temp;
}
```
* This is because the swap function will only swap copies of a and b, not affect the arguments in the routine that called it. We must instead pass pointers to the values to be changed: `swap(&a, &b);` This is because the `&` operator produces the address of a variable. The swap function parameters are then declared to be pointers, and the operands are accessed indirectly through them thusly:
```c
void swap(int *x, int *y)    /* right */
{
    int temp;
    temp = *x;
    *x = *y;
    *y = temp;
}
```
* Pointer arguments enable a function to access and change objects in the function that called it. 
### Pointers and Arrays
* Any operation that can be done by array subscripting can also be done with pointers. 
* The declaration `int a[10];` defines a block of consecutive objects.
* If pa is a pointer to an integer, declared as `int *pa;` then the assignment `pa = &a[0];` sets pa to point to the address of `a[0]`. Now the assignment `x = *pa;` will copy the contents of `a[0]` into `x`.
* If pa points to an element in an array, then pa+1 points to the next element. Thus if pa points to `a[0]`, `*(pa+1)` refers to the contents of `a[1]`, `pa+i` is the address of `a[i]`, and `*(pa+i)` is the contents of `a[i]`.
* By definition, the value of a variable or expression of type array is the address of element 0 of the array. Since the name of an array is a synonym for the location of the initial element, the assignment 
`pa = &a[0];` can also be written as `pa = a;`
* The one difference between an array name and a pointer is that pointers are variables but array names are not. Pointers can be assigned to an array, like `pa=a`, or incremented, but arrays cannot be assigned to pointers, like `a=pa`, and they cannot be incremented.
* Because an array name parameter is a pointer (a variable containing an address), an array name passed into a function passes the location of the initial element. Within the called function the argument is a local variable
CONT’D


