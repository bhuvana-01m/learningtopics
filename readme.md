 A)Why main () is  called first and call without main () is possibe ?
 
Refer https://www.techiedelight.com/c-program-without-main-function/#:~:text=We%20can%20also%20use%20a,reach%20the%20main()%20function.&text=We%20can%20also%20make%20use,constructor%20to%20do%20the%20same
https://cs107e.github.io/guides/gcc/

For ex gcc , _start() is the first function where placed in crct0.s assembly it make call to main . we can override that can exit before jumping to main .
1->gcc -nostartfiles :
// Compile it with gcc -nostartfiles startfun.c
 //startfun.c
#include <stdio.h>
 
void _start()
{
    printf("Inside _start\n");
    exit(0);
}
If we take c program two types of execution environment is there , freestanding other one is hosted .
FREESTANDING : 
              In which C program execution may take place without any benefit of an operating system
              In the standalone world, main can have any type signature and it is configurable whether it is main or some other function that starts the program.
              gcc -ffreestanding -c blink.c
              The -ffreestanding option also directs the compiler to not assume that standard functions have their usual definitions. 

              
2->Using attribute constructor , destructor but without main we cant .
When the attribute is constructor attribute, then it will be executed before main()
when the attribute is destructor type, then it will be executed after main()
The function is __attribute__():
         The syntax __attribute__((constructor)) is used to execute a function when the program starts. 
         The syntax __attribute__((destructor)) is used to execute the function when main() function is completed. 
void before_main() __attribute__((constructor));
         
3-> Using preprocessor macro exapanding :
We can only restrict the direct invocation of main here by using macro expand.Just replace this user defined as main function .
#include <stdio.h>
#include <stdlib.h>
 
#define replace(a,b,c,d) a##b##c##d
#define execute replace(m,a,i,n)
 
void execute()
{
    printf("Hello World");
    exit(0);
}
B)callback called / done by function pointer 

refer :https://www.opensourceforu.com/2012/02/function-pointers-and-callbacks-in-c-an-odyssey/

->passing function ref to another function as param .
->runtime decision 
-> dynamically pass diff func 
//callback 
void display(void (*fptr)(int ,int))
{
(*fptr)(5,4);
}
void sum(int a,int b)
{
printf("sumof a and b ",a+b);
}
int main()
{
display(&sum);
}

