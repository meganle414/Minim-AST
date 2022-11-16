# Minim-AST

Type Checking
The type checker will determine the type of every expression represented in the abstract-syntax tree and will use that information to identify type errors. In the minim language we have the following types:

int, bool, void (as function return types only), strings, struct types, and function types

A struct type includes the name of the struct (i.e., when it was declared/defined). A function type includes the types of the parameters and the return type.

The operators in the minim language are divided into the following categories:

logical: not, and, or
arithmetic: plus, minus, times, divide, unary minus
equality: equals, not equals
relational: less than (<), greater than (>), less then or equals (<=), greater than or equals (>=)
assignment: assign
The type rules of the minim language are as follows:

logical operators and conditions:
Only boolean expressions can be used as operands of logical operators or in the condition of an if or while statement. The result of applying a logical operator to bool operands is bool.

arithmetic and relational operators:
Only integer expressions can be used as operands of these operators. The result of applying an arithmetic operator to int operand(s) is int. The result of applying a relational operator to int operands is bool.

equality operators:
Only integer expressions, boolean expressions, or string literals can be used as operands of these operators. Furthermore, the types of both operands must be the same. The result of applying an equality operator is bool.

assignment operator:
Only integer or boolean expressions can be used as operands of an assignment operator. Furthermore, the types of the left-hand side and right-hand side must be the same. The type of the result of applying the assignment operator is the type of the right-hand side.

disp and input:
Only an int or bool expression or a string literal can be printed by disp. Only an int or bool identifer can be read by input. Note that the identifier can be a field of a struct type (accessed using . ) as long as the field is an int or a bool.

function calls:
A function call can be made only using an identifier with function type (i.e., an identifier that is the name of a function). The number of actuals must match the number of formals. The type of each actual must match the type of the corresponding formal.

function returns:
A void function may not return a value.
A non-void function may not have a return statement without a value.
A function whose return type is int may only return an int; a function whose return type is bool may only return a bool.

Note: some compilers give error messages for non-void functions that have paths from function start to function end with no return statement. For example, this code would cause such an error:

int f() {
    disp << "hello";
}

However, finding such paths is beyond the capabilities of our minim compiler, so don't worry about this kind of error.

You must implement your type checker by writing appropriate member methods for the different subclasses of ASTnode. Your type checker should find all of the type errors described in the following table; it must report the specified position of the error and it must give exactly the specified error message. (Each message should appear on a single line, rather than how it is formatted in the following table.)

Type of Error	Error Message	Position to Report
Writing a function; e.g., "disp << f", where f is a function name.	Write attempt of function	1st character of the function name.
Writing a struct name; e.g., "disp << P", where P is the name of a struct type.	Write attempt of struct name	1st character of the struct name.
Writing a struct variable; e.g., "disp << p", where p is a variable declared to be of a struct type.	Write attempt of struct variable	1st character of the struct variable.
Writing a void value (note: this can only happen if there is an attempt to write the return value from a void function); e.g., "disp << f()", where f is a void function.	Write attempt of void	1st character of the function name.
Reading a function; e.g., "input >> f", where f is a function name.	Read attempt of function	1st character of the function name.
Reading a struct name; e.g., "input >> P", where P is the name of a struct type.	Read attempt of struct name	1st character of the struct name.
Reading a struct variable; e.g., "input >> p", where p is a variable declared to be of a struct type.	Read attempt of struct variable	1st character of the struct variable.
Calling something other than a function; e.g., "x();", where x is not a function name. Note: In this case, you should not type-check the actual parameters.	Call attempt on non-function	1st character of the variable name.
Calling a function with the wrong number of arguments. Note: In this case, you should not type-check the actual parameters.	Function call with wrong # of args	1st character of the function name.
Calling a function with an argument of the wrong type. Note: you should only check for this error if the number of arguments is correct. If there are several arguments with the wrong type, you must give an error message for each such argument.	Actual type and formal type do not match	1st character of the first identifier or literal in the actual parameter.
Returning from a non-void function with a plain return statement (i.e., one that does not return a value).	Return value missing	0,0
Returning a value from a void function.	Return with value in void function	1st character of the first identifier or literal in the returned expression.
Returning a value of the wrong type from a non-void function.	Bad return value	1st character of the first identifier or literal in the returned expression.
Applying an arithmetic operator (+, -, *, /) to an operand with type other than int. Note: this includes the ++ and -- operators.	Arithmetic operator with non-numeric operand	1st character of the first identifier or literal in an operand that is an expression of the wrong type.
Applying a relational operator (<, >, <=, >=) to an operand with type other than int.	Relational operator with non-numeric operand	1st character of the first identifier or literal in an operand that is an expression of the wrong type.
Applying a logical operator (!, &&, ||) to an operand with type other than bool.	Logical operator with non-bool operand	1st character of the first identifier or literal in an operand that is an expression of the wrong type.
Using a non-bool expression as the condition of an if.	Non-bool expression in condition of if	1st character of the first identifier or literal in the condition.
Using a non-bool expression as the condition of a while.	Non-bool expression in condition of while	1st character of the first identifier or literal in the condition.
Applying an equality operator (==, !=) to operands of two different types (e.g., "j == true", where j is of type int), or assigning a value of one type to a variable of another type (e.g., "j = true", where j is of type int).	Type mismatch	1st character of the first identifier or literal in the left-hand operand.
Applying an equality operator (==, !=) to void function operands (e.g., "f() == g()", where f and g are functions whose return type is void).	Equality operator used with void functions	1st character of the first function name.
Comparing two functions for equality, e.g., "f == g" or "f != g", where f and g are function names.	Equality operator used with functions	1st character of the first function name.
Comparing two struct names for equality; e.g., "A == B" or "A != B", where A and B are the names of struct types.	Equality operator used with struct names	1st character of the first struct name.
Comparing two struct variables for equality; e.g., "a == b" or "a != b", where a and a are variables declared to be of struct types.	Equality operator used with struct variables	1st character of the first struct variable.
Assigning a function to a function; e.g., "f = g;", where f and g are function names.	Function assignment	1st character of the first function name.
Assigning a struct name to a struct name; e.g., "A = B;", where A and B are the names of struct types.	Struct name assignment	1st character of the first struct name.
Assigning a struct variable to a struct variable; e.g., "a = b;", where a and b are variables declared to be of struct types.	Struct variable assignment	1st character of the first struct variable.
Preventing Cascading Errors
A single type error in an expression or statement should not trigger multiple error messages. For example, assume that P is the name of a struct type, p is a variable declared to be of struct type P, and f is a function that has one integer parameter and returns a bool. Each of the following should cause only one error message:

disp << P + 1          //* P + 1 is an error; the write is OK
(true + 3) * 4         //* true + 3 is an error; the * is OK
true && (false || 3)   //* false || 3 is an error; the && is OK
f("a" * 4);            //* "a" * 4 is an error; the call is OK
1 + p();               //* p() is an error; the + is OK
(true + 3) == x        //* true + 3 is an error; the == is OK
                       //* regardless of the type of x
One way to accomplish this is to use a special ErrorType for expressions that contain type errors. In the second example above, the type given to (true + 3) should be ErrorType, and the type-check method for the multiplication node should not report "Arithmetic operator with non-numeric operand" for the first operand. But note that the following should each cause two error messages (assuming the same declarations of f as above):

true + "hello"    //* one error for each of the non-int operands of the +
1 + f(true)       //* one for the bad arg type and one for the 2nd operand of the +
1 + f(1, 2)       //* one for the wrong number of args and one for the 2nd operand of the +
return 3+true;    //* in a void function: one error for the 2nd operand to +
                  //* and one for returning a value
