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
## The code

### Glue code
An assembler program is provided to call a C function (CTOREXX).
The rexx interface has R1 -> 4 unused parameters, the function parameters, the evalblock.   R0 points to the address of the environment block, which has an array of rexx function points, for example get symbol.
The glue code creates a parameter list so you can use CTOREXX(parameter, evalblock, environment block).
### Example Rexx code

CPREXX is a Rexx program which calls the COLIN function, sets up varibles for the function.

### C high level function CTOREXX
This is a small program which does the following
<ol>
  <li>Displays the parameters passed to the function
  </li>
  <li>Demonstrates the use of the "Next" function to iterate over the variables.  The example   compares with a parameters passed, and highlights those variables names which start with the passed parameter.  For example <b> = CRexxNext(pEnv,&name[0],lName,&buffer[0],lBuffer);</b>
  </li>
  <li>Demonstrates the use of the drop function to remove a variable.  For example <b> rc = CRexxDrop(pEnv,"ZDROP");</b>
  </li>
  <li>Gets a value from a symbol and displays it.  For example <b>rc = CRexxGet(pEnv,"ZZZZZ",&buffer[0],&lBuffer);</b>
  </li>
  <li>Creates a variable - and reports it it was added or replaced. For example  <b>rc = CRexxPut(pEnv,var,"Colinsv",0);</b>
  </li>
  <li>Returns a string of data.
</ol>

When data is returned, it is null terminated like normal C strings.
### C worker functions

<ul>  
<li>CREXPUT  include file which implements CRexxPut       
</li>
<li>CREXDROP include file which implements CRexxDrop             
</li>
<li>CREXNEXT include file which implements CRexxNext for iterating through variables
</li>
<li>CREXGET  include file which implements CRexxGet
</li>
</ul>

### Rexx header files

<ul>
<li>SHVBLOCK Shared variable access     
</li>
<li>REXPARMS a structure with the address/length definitions        
</li>
<li>IRXEXTE  A structure with the rexx function entry points.   I've only used IRXEXCOM       
</li>
<li>IRXENVB  A structure with the environment block.  This is passed into the C program. I contains a pointer to the list of Rexx functions such as get symbol. 
</li>
<li>EVALBLOC This is used to pass back data to Rexx at the end of the function.
</li>
</ul>

### JCL

<ul>
<li>CC is some JCL to do the work.  it uses COLIN.C.REXX.OBJ to hold the intermediate Object code, and COLIN.C.REXX.LOAD to hold the COLIN module, and CPARMS for the C compiler parmameters.
<ul>
<li>it assembles the glue code, 
</li>
<li>it compile the CTOREXX C program. 
</li>
<li>it binds the C program and the glue code to create the COLIN modules 
</li>
<li>it runs a sample REXX exec to demonstrate the operation
</li>
</ul> 


## Usage notes
<ol>
<li> Although my variables were called x. ... the Next service returned them as X., in upper case.
</ul>
