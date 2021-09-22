##### For CS Students,
Computer Science is an extremely complicated but rewarding field. Some of the concepts when talking about computer hardware or computing philosophy, can be quite difficult for those with no background. Persevere through it, it is worth it.\
In this teaching series we use the language C and C++ for most of it. I personally believe that due to the exposure to hardware that these languages allow, it makes the learner have a better understanding of how the hardware works.\

#### PART 0 DATA TYPES

In all programming languages they have data types. These data types are able to be used to initialize variables depending on what type of information they want to store. Here we will list the primitive types.

    -------------------------(C/C++)-------------------------
    short:     short integer   (usually 2 bytes)
    int:       integer         (usually 4 bytes)
    long:      long integer    (usually 8 bytes, sometimes 4)
    long long: long integer    (usually 8 bytes)
    float:     single precision floating point 
    double:    double precision floating point
    char:      character       (byte)
    --------------------------(C++)--------------------------
    bool:      boolean
    
The list of these types seem quite arbirtrary, After all why are four integer types? Why are there two floating point types? What is a floating point? What is a Integer? What is a boolean? 
##### PART 0.1 Integers
Integers are values that can be represented as whole numbers in math. IE {-1,0,1}. Seems simple enough. However in all computers, because of the limited size of the memory available, and how hardware is designed, we put sizes on these integer types. These sizes determine how big of a range each integer type has. That is why there are four integer types. You can actually calculate the range of the integer type quite easily. \

    lower_bound = -2^(bits-1), upper_bound = 2^(bits-1)-1

 The reason why this equation works is due to the fact how computer hardware stores these integers and why we do it the way we do.\
###### Binary Numbers
So integers are stored in hardware as binary data. Binary data is represented as on (1) or off (0). The individual data slices (singular ones or zeros) are called bits, and a group of eight are called a byte. How integer data is held in memory are in groups of bytes, composed of bits, which stores the binary form of that decimal number. Now to understand how to convert binary numbers to normal arabic numbers, there is a easy to understand method.\
Lets say I have the binary number 0b 0101 0110 (0b is just a prefix to identify the following digits are in binary) that I want to convert to decimal. What I do is I multiply the bit by 2^(its position). Then I sum all the numbers. For example

    0b | 0   1   0   1   -   0   1   1   0
     *   2^7 2^6 2^5 2^4     2^3 2^2 2^1 2^0
     = | 0 + 64+ 0+ 16+      0+  4+  2+  0
     = | 64 + 16 + 4 + 2 =   86  
How do we convert decimal to binary though?\
We divide the number from the largest 2^n that resolves in 1 and we continue down from 2^(n-1) to 2^0 on the remainder of each. For example

    decimal 100
    100 / 2^6 = 1, r36         :   0b| 1...
	  36 / 2^5 = 1, r4         :   0b| 1 1...
	    4 / 2^4 = 0, r4        :   0b| 1 1 0...
	      4 / 2^3 = 0, r4      :   0b| 1 1 0 0...
	        4 / 2^2 = 1, r0    :   0b| 1 1 0 0 1...
	          0 / 2^1 = 0, r0  :   0b| 1 1 0 0 1 0...
	            0 / 2^0 = 0, r0:   0b| 1 1 0 0 1 0 0
	thus 0b| 0 1 1 0 - 0 1 0 0 = (100)
	

To understand why this works, we have to understand how all numbering systems work. \
Decimal, or Base 10, has ten individual symbols to represent numeric values. These are our digits 0-9. We have the places that each number sits in to represent further numbers past nine. These are our places. Now each place is going to be 10^(position of the number)'s place. This is becauase after 9,  we cant fit a 10 number in the first place, so it 'rolls' over to the next place, and we get the 10s place, then the 100s... All other base N systems follow a simmilar system. Binary, or Base 2 is very similar. We have 2 digits, 0 and 1. To fit 2, we 'roll' to the next spot to get it. Thats why it works, cause each place is going to be a version of 2^n.\ 
There are other numbering systems too. Base 16, or hexadecimal works in a similar way. It has digits 0,1,2,3...9,A,B,C,D,E,F, and has 16s places.\
What about adding?
Again works very similar to decimal, when the number exceeds the base, it rolls the digit over to the next spot to add in. To show what I mean, look at this text box where I add two binary numbers.

    carry| 1 1
       0b| 0 1 1 0 - 0 0 1 0
    +  0b| 0 1 1 0 - 0 0 0 0
    --------------------------
       0b| 1 1 0 0 - 0 0 1 0
Seeing how the two 1s add up, it is quite similar to decimal addition.
What about subtracting? Good question.
###### Two's Compliment
Because of the fixed size of these integers, there is a occurance that happens when you add two high binary numbers, its something called a integer overflow. To show what I mean, I will add two large numbers

        0b| 1 0 0 0 - 0 0 0 0 (256)
      + 0b| 1 0 0 0 - 0 0 0 0 (256)
      -----------------------
        0b| 0 0 0 0 - 0 0 0 0 (0)
These two numbers added overflow the range of the integer and flip back. This is what happens in an integer overflow.\
How we harness this overflow for negative integers, is called Two's Compliment. The process is as such. We flip all bits on the binary number, and add one to it. This is how we get the negative of a binary number.

     invert 0b| 0 0 1 0 - 0 1 1 1 (49)
     =      0b| 1 1 0 1 - 1 0 0 0
     +      0b| 0 0 0 0 - 0 0 0 1
            ---------------------
            0b| 1 1 0 1 - 1 0 0 1 (-49)
    
       
      
To prove that its the negative, we will take the original and add it to its negative. This should result in zero.

     carry | 1 1 1 1   1 1 1
         0b| 0 0 1 0 - 0 1 1 1 (49)
       + 0b| 1 1 0 1 - 1 0 0 1 (-49)
       -----------------------
         0b| 0 0 0 0 - 0 0 0 0 (0)
The addition of these two numbers resulted in zero, meaning that the it is the negative of the original number.
What about the method we use to find the decimal value of a binary number? Wont this mess it up? Yes, however one of the properties of a negative two's compliment binary number is that the left most digit is going to be one if its negative, zero if its positive. If its negative, you could just get the two's compliment of that number, then find the decimal value from that.
##### PART 0.2 Floating Points
This section is going to be more of a brief overview of floating points, as it is too complicated to show how it works under the hood.\
Floating points are decimal numbers that support numbers with decimal points. This means you could store numbers such as 3.14 and others. Precision is how far in the decimal point you can accurately store. Single precision floating points can hold all short numbers safely, and all double precision floating points can hold all int numbers safely.
##### PART 0.3 Characters
Characters are individual letters and other symbols. char stores such characters. Under the hood the language we are using uses ASCII or (American Standard Code For Information Interchange). This is a set of numbers that represent individual characters, not just including letters. This is how text is displayed.

##### PART 0.4 Boolean
Booleans are very simple. They are true or false values. 
##### PART 0.5 Variables
Variables are named positions in memory which have a specific type. To instantiate a variable we do such:

    type_goes_here name_goes_here;
For example if we wanted a int with the name computers we could do this:

    int computers;
All instatiated variables are filled with garbage values (random values) unless assigned a value. To assign a value we use the equals operator. To assign a value to an already instantiated variable we could do this:

    variable_name = value_goes_here;
You can also combine an instantiation and an assignment. How this is done is as such

    type_goes_here name_goes_here = value_goes_here;
For example if we wanted a int variable computers with the value equal to 32 we do this:

    int computers = 32;
For characters the value assigned needs to be in single quotes. For example if I wanted a variable first_initial with type char, and assigned to J I would need to do

    char first_initial = 'J';
##### PART 0.6 Casting
Casting can be both implicit and explicit. Casting is when you change one data type to another data type. Casting can be implicit when its safe (looses no precision / values). Casting to unsafe types requires to use a cast operator.
Explicit casting is done as such:

    int number = 32;
    short result_cast = (short)number;
This casts number to a short and assigns it to result_cast. Below I put a tree with the order of safe casting. You can cast from any position to a position below it.

    bool -> char -> short -\---> int ---\---> long ---> long long
                            \            \
                             \-> float ---\---> double

Safe casts can be casted implicitly. This means that if you assign a value from a short to a int it will automatically cast safely.
##### PART 0 Questions
1. What is the decimal of 0b 0010 1110?
2. What is the negative of 30 (byte size integer)?
3. What is 40 + 0b 0001 1010?
4. What is 40 - 0b 0001 1010?
5. Are these variable assignments correct? Which ones are incorrect and why?
###### 
    int total = 42;
    char value = 'd';
    float number = 32.0;
    short thing = 'g';
    char alpha = 32.2;
    int zdepth = 33.1;
    float intial = 'D';
 
 #### PART 1 OPERATORS
 Operators are a essential component of programming languages. They provide us with the ability to manipulate the data itself. There are many operators in C. We are going to start with comparison operators, as they are simple.
 ##### PART 1.1 Comparison operators.
 There are a few comparison operators in C. Comparision operators are operators that compare two values and return a value that is true or false (one or zero). The operators are as such
 

    a == b: a equal to b
    a != b: a not equal to b
    a <  b: a less than b
    a >  b: a greater than b
    a <= b: a less than or equal to b
    a >= b: a greater than or equal to b
   These operators return true (one) if the statement is correct, and false (zero) if the statement is incorrect
   ##### PART 1.2 Boolean Operators
   Boolean operators are operators that take one or two boolean values (true or false) and return a boolean value. The operators are as such:
   

    !a:     not a
    a || b: a or b
    a && b: a and b
   The not operator returns the opposite of the boolean value. If true it returns false, and false returns true. The or operator returns false only if a and b are both false, otherwise true. The and operator return true only if a and b are both true, false otherwise.
##### PART 1.3  Other Operators
Many of these mathmatical operators you probably already know of, however C has a few bitwise operators. The example of all C's other operators are as such:

    a + b:  a plus b
    a - b:  a minus b
    -a:     negative of a
    a / b:  a divided by b
    a * b:  a multiplied by b
    a++:    increment a
    a--:    decrement a
    ~a:     bitwise invert of a
    a | b:  bitwise or of a and b
    a & b:  bitwise and of a and b
    a >> b: a bitwise left shift by b bits
    a << b: a bitwise right shift by b bits
    a = b: a assigned b
The bitwise operators apply to the individual bits. So for bitwise invert would flip every bit on that variable to the opposite value for example. For bitwise or it every bit that is one on a or b that is active. Bitwise and is only every bit active on both a and b. The shift operators rotate the bits by n bits in either direction, dropping the bits off the end.\
To understand better, I will provide examples here:

    0b|1010 0101 bitwise invert = 
    0b|0101 1010
    0b|1010 1010 bitwise or with
    0b|1100 0011 = 
    0b|1110 1011
    0b|1010 1010 bitwise and with
    0b|1100 0011 = 
    0b|1000 0010
    0b|1010 1010 left shift by 2 =
    0b|1010 1000
    0b|1010 1010 right shift by 2 =
    0b|0010 1010
    
##### PART 1.4 Order of Operations
Like with math, operations on progams have an order. This applys to all operators. The order of operation is as such from top down:

    a * b, a / b
    a + b, a - b, -a
    a++, a--,
    a & b, a | b, a~
    a >> b, a << b
    a == b, a != b, a < b, a > b, a <= b, a >= b
    a && b, a || b, !a
    a = b 
The program will compute the output following the order of operations.

##### PART 1 Questions
For all following questions assume as follows:

    a = 10
    b = 3
    c = 49
    d = 2
    e = 1
    
Answer the questions with the result of the operations:
 1. a - 7 == b
 2. c - a * 4 <= b
 3. a * a == (b + d) * d * a
#### PART 2 BASIC PROGRAM
To make a compilable C program, it requires not much. Below is the begining of a compilable C program.

    #include <stdio.h>
	// comments
	int main(int argc, char** argv){
		return 0;
	}

Much of this you havent seen before, I will explain. `#include <stdio.h>` is what is called a **preprocessor include**. A preprocessor include imports a **library**, or a external codebase into your program. The external codebase contains a **header file**, `stdio.h` in our example. This header file contains all the definitions of functions that library contains. `stdio.h`is called the **Standard Input Output Library**, or **STDIO** for short. This library allows us to request input or output values onto the terminal window through various functions. \
`//` indicates the beggining of a **single line comment**. Comments are to provide clarity for the reader of the code, however they are not compiled.\
`int main(int argc, char** argv){` is a **function definition** called the **program entrypoint**. The program entrypoint is where the program begins executing code at. In C the program entrypoint is at a function called **main**. All functions in C have a **return type**, which is the type of value that the function hands back when completed. In `main`'s case, it is type `int`. For main the values that the function hands back is called a **exit code**. Exit codes tell the operating system about how the program exited, if it exited with an error or not, and what type of error. \
`int argc, char** argv` are called the **function parameters**. For `main`'s case its the **terminal arguments**. `argc` is the **count of the arguments provided**, and `argv` is the **text of the argument values**.\
`return 0;` returns **exit code** of the program and finishes the program.\
What is defined between `main`'s `{` and `}` is called the **body of the function** and is executed when ran.\
Dont worry, this is a lot to take in, and most of it you do not need to remember. Just remember that the begining of the program happens inside main, and the text for the beginning of a program
#### PART 3 WRITING AND COMPILING
From this point on, it is required to use Ubuntu Linux with GCC. If you do not have a machine that runs Ubuntu Linux head to **Adendum** and follow the directions. \
Begin by writing your first C program by using this template in a program called **text editor**.

    #include <stdio.h>
	
	int main(int argc, char** argv[]){
		printf("Hello World!");
		return 0;
	}
and save it in your home directory named as `hello.c`. The `.c` indicates the type of file that you have just wrote. This file is called a **C Source File** and contains the code of your program.
Now open your terminal. You should see something like this:

    user@pc$~:
The first term indicates the **user** of the terminal, and the second terminal indicates what **computer** they are on. The `$` indicates what level of **privileges** the terminal has. In this case it has the privileges of the **group** your user is in. \
Now to **compile** your program. Compiling is when you convert human readable code into **machine readable** code.  The **Compiler** is the program we are going to use to compile our source file into a program. The Compiler we will be using is called **GCC** or **GNU C Compiler**. To compiler our program we will run this command `gcc hello.c -o hello`. the first argument is the source file of our program. `-o` indicates the **outfile**, and immediately following it is the **program executable name**.\
To then run our program which is called `hello`, we will execute it by typing `./hello`. This **executes** the program. \
You wrote, compiled, and executed your first program! Congratulations! From this point on we will be using this method to write programs. 
 #### PART 4 FUNCTIONS
 Functions are going to be the backbone of most of your program. Functions allow you to define **routines** into seperate executable chunks. The point of a function is to make the program more readable and easier to debug.\
 Functions contain at least a **function definition** and may contain a **function declaration**. Function definitions contain the **parameters**, **return type**, **funciton name** and the **body of the function**. Parameters are what is handed to the function to execute for a desired result. These are defined as typed variables. The return type is what the function returns after finishing executing back to the location it was called. The function name is what the function is called by. The body of the function contains all the code for the function. Functions are **called** and execute from where it was called at. \
 **Function declarations** provide the **name**, **the type of the parameters**, and the **return type**\
A example function declaration and definition are provided below:

    int square(int); // declaration
    int square(int x){ // definition
	    return x*x;
	}

C can return any type and one special return type. **void** returns nothing to the point it was called. 
##### PART 4 Questions
Below you will find a imcomplete program with function declaration and a main driver. Complete the code for the desired effect.

    #include <stdio.h>
	#define PI 3.14
	
	double find_area_of_circle(int);
	double find_area_of_circle(int radius){
		// Fill this stub to complete the program
		// This stub has radius as a parameter, and
		// should return the area of the circle
		// for pi, use PI
	}
	
	int main(int argc, char** argv){
		for(int i = 1; i < 101; i++){
			printf("the circle with radius %d has an area of %lf\n",i, find_area_of_circle(i));
		}
		return 0;
	}
#### PART 5 Printing
`stdio` will be a vital part of our programs, and we will need to learn first how to write to the terminal using the functions in this library and how they work.\
Most importantly we need to learn what a **string** is. A **string** is a array of characters. A **null terminated string** is a string that ends with the value zero, also known as a **null character**. `stdio` uses null terminated strings to provide for IO. These strings defines the specification of how to **print** or **read** from the terminal. The strings that define that specification are called **format strings** and are defined by character combinations for replacements. \
The specification for printing to the screen is provided below.\
`printf(format_string, arguments);`  prints the string with the arguments to replace values with to the terminal.\
Format strings are strings where character combos are replaced on screen. The specification for the combos are provided below

 - `%d`: decimal
 - `%f`: float
 - `%ld`: long decimal
 - `%lf`: long float
 
Strings also can contain **escape sequences**. Escape sequences are character combos that trigger the terminal to act differently. Right now all you need to know for escape sequences is the **line feed**. The Line feed escape sequence is `\n`.\

For example if we wanted a program to write a few integers and a double to the terminal, we would use `printf`. Lets say the variables `int total, int age, int acct_id, double total_distance_traveled`are provided. We would write a printf call like this:

    printf("account: %d for %d months has accumulated %lf miles for %d$",acct_id, age, total_distance_traveled, total);

#### PART 6 CONTROL AND FLOW
Programs must be able to act differently based on **state**. **State** is the current condition of the program. Without the ability we would be just defining math statements. We have multiple ways to control the direction a program is going in. The first one is called an **if/if else/else statement**.\
##### PART 6.1 if/if else/else statements
An **if statement** is a conditional **control statement** that executes based off the **condition** provided. . An if statement executes what is inside `{` and `}` whe the condition provided is true. An example if statement is provided here:

    if(age < 21){
	    printf("YOU ARE UNDERAGE!\n");
	}
In this if statement, it will print `YOU ARE UNDERAGE!` if age is less than 21. However if your age is equal to or over 21 nothing happens.\
**else if statements** are a control statement that occurs only if the proceeding if statement fails and it condition is true. We could extend this so that if age is over 49 we could print that its happy hour.

    if(age < 21){
	    printf("YOU ARE UNDERAGE!\n");
	}
	else if(age >= 50){
		printf("happy hour! :)\n");
	}
But what about if your of age, but under 50? We could solve this by using an **else statement**. Else statements are executed if and only if all previous `else if`'s and `if` statements are not executed.  To implement a check for between 21 and 50, we will add an else statement to the existing code.

    if(age < 21){
	    printf("YOU ARE UNDERAGE!\n");
	}
	else if(age >= 50){
		printf("happy hour! :)\n");
	}
	else{
		printf("time for a drink...\n");
	}
In this case it will print time for a drink if we are between 21 and 50. If less than 21, it will print that we are underage. Finally if we are above 50 it will let us know that we are at happy hour.
##### PART 5.2 Switch statements
**Switch statements** allow us to define routines for each **case**. These cases are matched to the parameter of the switch statement. A example would be such:

    // lets say that checking accounts are 0, 
    // savings are 1, and money markets are 2
    double apr;
    switch(acct_type){
	    case 1:
		    apr = 0.0;
		    break;
		case 2:
			apr = 0.0001;
			break;
		case 3:
			apr = 0.015;
			break;
	}
 It allows you to define a set of routines for each case and make the code more readable. Switch statements are strong at differentiating based on a large group of conditions compared to a if else statement.
 ##### PART5.3 Looping
 Looping is vital to our programs. It allows us to deal with more than one value at a time. There are three types of loops. **while loops**, **for loops**, and **do while loops**. \
 While loops are loops that execute only when the condition is present. How while loops work is on entrance to the loop it checks the condition. It then procedes to execute its whole body if its satisfied. It will continue to do this untill its condition fails. This example will double its value until its over 500.

    int value = 1;
    while(value <= 500){
	    value = value * 2;
	}
For loops are similar but provides a spot for **initialization**, a **conditional statement**, and a **increment statement**. This makes it really good for incrementing a number until it reaches a value. For example this example will count untill 100;

    for(int i = 0; i < 100; i++){
	    printf("%d\n",i);
	}
The first statement inside the parenthes is the initializaition statement. We define an int inside of it, int i. *(loops often use the letters  i,j,k for looping. This is called ijk notation)* The next statement is the conditional statement. Finally the incremental statement increments i by 1 before checking the condition again.\
Do while loops are very similar to while loops. The major difference is do while loops will execute once and then check the conditional before executing again. The do while loop looks like such:

    do{
	    body;
    }while(conditional);
everything in the body is executed at least once, until the conditional is satisfied.
##### PART 5 Questions
Write a program that will compute the sum from 0->100 of all sums from 0->n of k where k is the inner sum, and n is the outer sum variable. Display this sum on screen with a `printf` statement.
#### PART 6 ARRAYS AND POINTERS
**Arrays** are a group of values all under one name. Arrays will share like type, but each **element** will vary in value. An element of an array is a singular value from that array. Arrays are allocated at compile time for its size. Array size is equal to the length of the array multiplied by the length of the array.\
**Pointers** are named locations in memory (variable) that stores the location of another variable. To understand this better thing of it this way. Pointers are like the street adress of the friends house. You can go to your friends house to meet your friend there, but you do not own your friend directly.\
Pointers contain a **reference address** which is an **address in memory**. An address in memory is the location where the value is stored. Pointers allow you to look up for that value and modify that value without directly owning it.\
You may ask why is this necessary? Well how would we modify elements in array without owning them without it. Pointers allow us to just go to the next spot and access it. Arrays are defined in the first elements address. To better visualize this, underneath is provided a image of a facetious memory map.

            [          array         ]         [ x ptr]
    address |  0 |  1 |  2 |  3 |  4 |  6 |  7 |    8 |
    value   |  9 | 10 | 22 |  3 | -5 | 92 | 32 |    0 |

If we go to x ptr and look at its value, its 0, which is the begining address of the array. Lets say we wanted to get the third element of the array, we would add 2 to the base address of the array, and we would get the value of the third element.  That is the power of pointers.\
But how do I even use a pointer or declare one?\
The declaration of a pointer is a type with a star following it. For example a int pointer would be declare like `int* a`. To assign a pointer to a location of a variable we can do that by using the **reference** operator. The reference of operator is a ampersand followed by the variable name. lets say we wanted to assign the address of b to `int* a`. we can do that by doing this: `int* a = &b;` How we can retrieve or assign a value to the location that `int* a` stores is by using the **dereference** operator.  The dereference operator is `*` and precedes the pointer name. Lets say we wanted to assign 3 to the location `int* a` stores. We could do that by doing this: `*a = 3;` It is important to note that memory addresses could be invalid, and a special value called **NULL** points to a invalid memory address to indicate an error or no value. To check if a pointer is pointing to `NULL` we can do a simple conditioanl equals check against `NULL`.\
What about arrays?\
Arrays are special and have to be intialized by creating a new array. This is done `type array[num]` where type is the data type, array is the array name and num is the total number of elements. This will allocate enough memory for the array to use. To grab elements from an index we can either add to the array value and dereference, or better, use the **indexing operator**. The indexing operator is  `array[num]` where num is the location of the element in the array. It is important to note in C arrays are **zero indexed**. This means the arrays first element starts at 0 and ends at n - 1 elements.
##### PART 6 Questions
Finish the program's function to calculate all the squares of an array.

	#include <stdio.h>
	// array is the array, and length is the length of the array
	void calculate_array_squares(int* array, int length){
		
	}
	int main(int argc, char** argv){
		int array[100];
		for(int i = 0; i < 100; i++){
			array[i] = 100 + 2*i;
		}
		calculate_array_squares(array,100);
		for(int i = 0; i < 100; i++){
			printf("%d is the result of the function\n",array[i]);
		}
		return 0;
	}
#### PART 7 RECURSION
**Recursion** is defined where a problem can be called on a subcase of itself. Recursion is useful for solving problems where the problem cant be identified as easily on a large scale comparatively to a problem in terms of itself. All recursive functions will need a **base case** or you run the risk of having a **stack overflow**. A stack overflow is when all the saved values from calls to the functions overflows the total amount the stack can save. Recursion is done often with calls to a function from itself. A famous example is fibonacci sequence. 

    void fibonacci(int a, int b, int under){
	    if(a >= under){
		    return;
		}
		printf("%d, ",a);
		fibonacci(b, a+b, under);
	}
This will print all elements of a fibonacci sequence by calling itself with the next elements of fibonacci.
##### PART 7 Question
Write a program that will solve the [Collatz conjecture](https://en.wikipedia.org/wiki/Collatz_conjecture) for an arbitrary N
#### PART 8 STRUCT
**Structs** are defined data collection that contains **fields** of any type that you can access using the **member** operator. Structs have to be defined before being delared, and used. To define a struct, we can do this:

    struct Person{
	    char* first_name;
	    char* last_name;
	    int age;
	    double height;
	 };
All variables declared to this struct type must use the type `struct Person`.
To declare a variable of that struct type we can do such `struct Person me;` To assign values to structs we must used the member operator. To change the age of variable `me` we can do such `me.age = 3;`. If the variable is a pointer to a struct we must use **arrow notation** to access a member. Assuming `struct Person* customer` contains a valid memory address, we could change the height value by doing `customer->height = 1.3;`.
##### PART 8 Questions
Declare and define a struct that contains fields for info needed for a bank account. 
#### PART 9 MEMORY ALLOCATION
### Adendum
#### For Windows PC's:
Head to [Oracle's Virtualbox download](https://www.virtualbox.org/wiki/Downloads) and select `Windows hosts`. Install the `VirtualBox-*.*.exe` and accept all driver installs. This will install device drivers for the virtual machine. It will ask to reboot, in which you should.
#### For MacOS PC's:
Head to [Oracle's Virtualbox download](https://www.virtualbox.org/wiki/Downloads) and select `OS-X hosts`.
Install the package and accept all the permissions. 

#### Setting up the Virtual Machine
Head to [Canonical's Ubuntu download site](https://ubuntu.com/download/desktop) and download the LTS version of Ubuntu for `x86-64`. Once the iso is finished downloading, open up VirtualBox and press the blue star indicating New. This will bring up the setup wizard. For the OS, select Ubuntu, when it asks for the install disc, select the location for your .iso file for Ubuntu, Otherwise press next. Finish setting up the Virtual Machine, and click the green arrow. If Virtualbox raises an error stating that the Virtual Machine is not able to start, you will need to turn on virtualization extensions in your BIOS. Search Google for your PC model followed by `enable virtualization in BIOS` and follow the instructions.
Follow the onscreen prompts and setup. You should have a virtual machine setup!
