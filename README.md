# PHP-Enum-Class
A simple enum class for PHP

This is a simple PHP class to help people use enums.  It does this by creating a private array where enums are
stored in this fashion:

    $enums[<ENUM>] = <VALUE>;

    Example:
    
       $enums['RED_LIGHT'] = 0;

Since the variable is private, it can not be modified outside of the class.  Further, tests are done to ensure that
once a value has been assigned to an enum that it can not be modified.

Finally, there are several ways to both PUT and GET the enum information.  These include as an array or as a list of
enums, or as a single enum.  Incoming enums that do not have a value assigned to them will be given a value based
upon how many entries there are in the array.  Thus, if you sent over:

   $c->put( array( "Carson"=>7, "RED_LIGHT", "YELLOW_LIGHT", "GREEN_LIGHT" ) );

The program would assign one(1) to RED_LIGHT, two(2) to YELLOW_LIGHT, and three(3) to GREEN_LIGHT and not eight(8),
nine(9), and ten(10) as would happen in other languages.  THIS ACTION CAN BE CHANGED EASILY with this program.  All
that is needed is to modify it so a test is done to see if the last entry in the enums array is numeric:

   is_numeric($this->enums[count($this->enums)-1])

and if the next incoming enum has no value assigned to it - it could then be assigned

   $this->enums[count($this->enums)-1] + 1;

which would make our example above become eight(8), nine(9), and ten(10).

Functions contained with this class:

1. __construct().  This function initializes the enums array.  Further processing can be done by passing in the
list of enums you wish to create.

2. put().  This function "put"s the enums into the array thus making them available to the rest of the class. You
can make your request as either an array, a list, or a single enum.  Enums without a value automatically are assinged
a value based upon how many entries there are in the enums array.  So the sixthed entry, if no value is given,
will become equal to six(6).

3. get(). This function "get"s the enums and their values.  You can make your request of the values as an array,
a list, or a single enum.

4. clear().  An optional way to clear the enums array. There is an associated flag ($clear_flag) which is set to
FALSE to begin with.  Set this flag to TRUE if you want to be able to clear out the enums.  Otherwise, once you
have created the enums - you are stuck with them until the class is removed or the program finished.

5. __call().  A catch-all function just in case someone mistypes one of the other functions.  I just made a
change to this function.  Because it is called whenever a function isn't found - it makes the perfect way to
return an enum.  Just call it as a function.  So the RED_LIGHT enum could be called as:

   $c->RED_LIGHT();

After it has been defined.

6. __destruct().  The function used to shut the class down.
