# CtoRexxtoC
Sample code to show how you can write an external function in C, and use rexx services from C.

Using C source to create an external function in C is not easy. Rexx passes parameters in R1 and R0, and C programs usually use R1 for passing parameters.   This respository has some glue code to allow C routines to be used from Rexx.

Some C functions to help use Rexx facilities are available; 

    - using the parameters passed to the routine
    - getting a variable
    - putting a variable
    - dropping a variable
    - looping through all available variables
    - returning a parameter back to rexx.


## Glue code
An assembler program is provided to call a C function (CTOREXX).
The rexx interface has R1 -> 4 unused parameters, the function parameters, the evalblock.   R0 points to the address of the environment block, which has an array of rexx function points, for example get symbol.
The glue code creates a parameter list so you can use CTOREXX(parameter, evalblock, environment block).
## Example Rexx code
CPREXX is a Rexx program which calls the COLIN function, sets up varibles for the function.
## C high level function CTOREXX
This is a small program which does the following

    1. Displays the parameters passed to the function
    2. Demonstrates the use of the "Next" function to iterate over the variables.  The example compares with a parameters passed, and highlights those variables names which start with the passed parameter.  For example **rc = CRexxNext(pEnv,&name[0],lName,&buffer[0],lBuffer);**
    3. Demonstrates the use of the drop function to remove a variable.  For example **rc = CRexxDrop(pEnv,"ZDROP");** 
    4. Gets a value from a symbol and displays it.  For example **rc = CRexxGet(pEnv,"ZZZZZ",&buffer[0],&lBuffer);** 
    5. Creates a variable - and reports it it was added or replaced. For example  **rc = CRexxPut(pEnv,var,"Colinsv",0);**
    6. Returns a string of data.

When data is returned, it is null terminated like normal C strings.
## C worker functions

    - CREXPUT  include file which implements CRexxPut       
    - CREXDROP include file which implements CRexxDrop             
    - CREXNEXT include file which implements CRexxNext for iterating through variables       
    - CREXGET  include file which implements CRexxGet

## Rexx header files
    - SHVBLOCK Shared variable access     
    - REXPARMS a structure with the address/length definitions        
    - IRXEXTE  A structure with the rexx function entry points.   I've only used IRXEXCOM       
    - IRXENVB  A structure with the environment block.  This is passed into the C program. I contains a pointer to the list of Rexx functions such as get symbol.     
    - EVALBLOC This is used to pass back data to Rexx at the end of the function.

## JCL

    - CC is some JCL to assemble the glue code, and compile the CTOREXX C program.  It uses COLIN.C.REXX.OBJ to hold the intermediate Object code, and COLIN.C.REXX.LOAD to hold the COLIN module.
    - CPARMS are the C compiler parmameters.






    
           
