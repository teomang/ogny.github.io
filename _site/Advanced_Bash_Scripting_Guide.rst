=============================
Advanced Bash Scripting Guide
=============================

:date: 2015.01.02

* **motto**: The only way to really learn scripting is to write scripts.

Ne ogrendim? 
============
  
Syntax'a dair; 
--------------

#. COMMENT satiri devam ettiginde basina + koy
#. Degisken adini kucuk harf tanimladiysan $ ile cagiriken kucuk harle, veya
   tersi sekilde cagir.
#. Bulundugun dizindeki betigi cagiriyorken basa ./ ekle, neden?
   for security reasons, the current directory (./) is not by default included
   in a user's $PATH. It is therefore necessary to explicitly invoke the script
   in the current directory with a ./scriptname

Lazim olabilecek Kod parcalari
------------------------------

#. The following script prolog tests whether the script has been invoked with

   the correct number of parameters. 

.. code:: sh


    E_WRONG_ARGS=85
    script_parameters="#.a #.h #.m #.z"
    #                  #.a = all, #.h = help, etc.
    if [ $# #.ne $Number_of_expected_args ]
    then
      echo "Usage: `basename $0` $script_parameters"
      # `basename $0` is the script's filename.
      exit $E_WRONG_ARGS
    fi

Special Characters
------------------

#. ``;;`` Terminator in a case option 

#. ``;;&, ;&`` Terminators in a case option (version 4+ of Bash).

#. The *comma* operator can also concatenate strings

.. code:: sh

    for file in /{,usr/}bin/*calc
    #             ^    Find all executable files ending in "calc"
    #+                 in /bin and /usr/bin directories.
    do
            if [ #.x "$file" ]
            then
              echo $file
            fi
    done

**Not**: comma operatoru degisiklik yapilacak sistem
dosyasinin yedegi alinirken gunluk olarak kullaniliyor 

.. code:: sh

    cp dosya{,.original} 

#. ``,, ,`` Lowercase conversion in parameter substitution 

#. ````` command substitution. The `command` construct makes available the output of
   command for assignment to a variable. This is also known as backquotes or
   backticks.

#. ``:`` null command [colon]. This is the shell equivalent of a "NOP" (no op, a
   do#.nothing operation). It may be considered a synonym for the shell builtin
   true. The ":" command is itself a Bash builtin, and its exit status is true

.. code:: sh

  (0).:echo $?   # 0

#. Variable expansion / substring replacement.In combination with the >
   redirection operator, truncates a file to zero length, without changing its
   permissions. If the file did not previously exist, creates it.

.. code:: sh

    : > data.xxx   # File "data.xxx" now empty.	      
    
    # Same effect as   cat /dev/null >data.xxx
    # However, this does not fork a new process, since ":" is a builtin.

#. The ``?`` character serves as a single#.character "wild card" for
   filename expansion in globbing, as well as representing one character in an
   extended regular expression.

#. ``$`` 

  1. : Variable substitution (contents of a variable).


#. The ``?`` character serves as a single#.character "wild card" for
   filename expansion in globbing, as well as representing one character in an
   extended regular expression.

#. ``$`` 

  1. : Variable substitution (contents of a variable).

  2. : end#.of#.line.

  3. ``${}``        : Parameter substitution.

  4. ``$' ... '``   : Quoted string expansion

  5. ``$*, $@``     : positional parameters.

  6. ``$?``         : exit status variable.

  7. ``$$``         : process ID variable.

#. ``()``           : command group.

.. code:: sh

   (a=hello; echo $a)


   A. Onemli not: parantez icine alinan ifade kendi icinde bir alt shell aciyor
      ve komutlar bu shell icerisinde gecerli. lokal degiskenler alt shell'in
      disina tasinamaz.

#. array initialization.

.. code:: sh

    Array=(element1 element2 element3)

#. ``{xxx,yyy,zzz,...}`` Brace expansion

#. ``{a..z}`` Extended Brace expansion.

#. ``{}`` Block of code [curly brackets]. 
#. ``[[]]`` test
#. ``[]`` 

   A. array element: In the context of an array, brackets set off the numbering
      of each element of that array.

#. ``{}`` Block of code [curly brackets]. 
#. ``[[]]`` test
#. ``[]`` 

   A. array element: In the context of an array, brackets set off the numbering
      of each element of that array.

   .. code:: sh

        Array[1]=slot_1
        echo ${Array[1]}

   B. range of characters (As part of a regular expression)


#. ``|``: A pipe runs as a child process, and therefore cannot alter script
   variables.
   If one of the commands in the pipe aborts, this prematurely terminates
   execution of the pipe. Called a broken pipe, this condition sends a SIGPIPE
   signal.




#. ``||`` OR logical operator: In a test construct, the ``||`` operator causes a
   return of 0 (success) if either of the linked test conditions is true.

#. ``&&`` AND logical operator. In a test construct, the ``&&`` operator causes
   a return of 0 (success) only if both the linked test conditions are true.


#. ``-`` redirection from/to stdin or stdout [dash].

.. code:: sh

   bash$ cat -
   abc
   abc

   ...

   Ctl-D

**Kendime not** : stdout'tan stdin'e bir sey gondermek gerektiginde ``- |``
ikilisini kullanabiliriz. boylece ciktilari bir dosyaya yazmaya gerek kalmaz.

#. Whitespace: functions as a separator between commands and/or variables.

#. ``$`` Variable Substitution: Note that $variable is actually a simplified
   form of ${variable}. 

#. Degisken tanimlarken ifadede bosluk kullanacaksak tirnak icine alabilir,
   veya her bosluk icin escape karakteri kullanabiliriz.

.. code:: sh

   degisken=\ tanimlanacak\ ifadenin\ tumu
   echo "$degisken"


#. komut ciktisinda bos satir birakmak icin echo kulllaniliyor.

#. Bir komut ciktisini degiskene atama yapacaksak, daha modern yol olarak $(..)
   syntax'ini kullanmakta fayda var, bundan once backquotes kullaniliyormus.

.. code:: sh

        # From /etc/rc.d/rc.local
        R=$(cat /etc/redhat-release)
        arch=$(uname -m)



