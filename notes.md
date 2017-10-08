## 9/5 - Introduction: What is AI? Foundations, History, State of the Art

#### What is the AI approach to tic tac toe?
* Instead of calculated every single board config, write a heuristic function that measures "goodness" of confid and pick best one
* This is important because you can't enumerate all possibilities of complex probs efficiently
  - Chess has 10^43 combinations

### Characteristics of AI Problems

##### Fully observable v. Partially observable
* __Fully observable__ - know exact state of opponent ex) Chess
* __Partially observable__ - ex) Battleship, Poker

#### Single Agent v. Multi Agent v. Adverserial Agent
* __Single Agent__ - One system acting to complete a task.
* __Multi Agent__ - Multiple systems communication and acting together.
  - ex) Self-driving car broadcasts to other cars
* __Adverserial Agent__ - Opponent is adversary. Must beat/defeat.
  - ex) Games (Chess, Go)

#### Deterministic v. Stochastic
* __Deterministic__ - Any game that doesn't depend on dice. Opponent makes move. No ambiguity/uncetainty.
* __Stochastic__ - Throwing die determines how you'll move. Move is dependent on roll of dice.

#### Static v. Dynamic
* __Static__ - World never changes. Robot navs around chairs.
* __Dynamic__ - ex) Self-driving car

#### Discrete v. Continuous
* __Discrete__ - Separate, distinct units.
  - ex) Chess has grid. Grid can be imposed on a room
* __Continuous__ - No grid.

#### What is the AI Effect (Tesleris Theorem)?
* "AI is whatever hasn't been done yet"
* Constant movement towards the next problem such that past/current probs are forgotten

## 9/5 & 9/7 - Lisp

#### Why Lisp? (3)
* Great for symbolic computation
* Great for functional programming
* Learning Lisp will make you a better programmer in other
languages

#### Characteristics of functional programming langs. Why beneficial?
* Everything is a function that returns a value, even for loops
* First-class and higher-order functions
* Purely functional languages have immutable data structures,
which eliminates side effects
  - No side effects means computation can be __distributed__
* Pure functional: Haskell
* “Impure” functional: Common Lisp, Clojure
* Languages have different degrees of purity.

#### What is Lisp? 
* Specified by John McCarthy in 1958
* LISP ≡ LISt Processing
* Functional programming language
* Great for symbolic computing; important for AI tasks of the
day
* Interpreted, but it can be compiled
  - __Interpreted lang__ - a language for which most of its implementations execute instructions directly, without previously compiling a program into machine-language instructions
* Great for rapid prototyping

#### Characteristics of Lisp
* Most everything in Lisp is a list (a linked list)
* Lists can store elements of different types
* Most everything is a function that returns a value
* Functions, function calls, and control structures are also linked
lists
* Consequently, there is no distinction between code and data
* And then there are the parentheses

#### Lisp characteristics and notation
* Optimized for symbolic computation tasks
* Prefix notation
  - `+ 2 2` instead of `2 + 1` (infix)
* Wrapped in `()` makes it both a LL and a function call
* `()` empty list == nil
* No literal `False`
* Built in garbage collection. After return statement executed, the original list is destroyed.
* Functions executed recursively

#### Everything in Lisp is a list, specifically a (2)
* Linked list
* Function that returns a value

#### Quotes 
* Use single quote to prevent evaluation
* `(quote(...))` passes list as arg to funct `quote`. It declares that the arg is an atom, specifically a symbol
* `'(...)` is a shorthand
  - `'` single quote turns off evaluation for entire list, even if there are lists within the list
  - `'((mark marc) tom sally)`
  - can't have a function call within a single quoted list
* __Funtion form__: causes arg to be interpreted as a function rather than being evaluated 
  - `> (setq + 3)` assigns symbol `x` with value 3. 
  - Useful when you want to pass a function as an arg to another function.

#### How is Lisp represented internally?
![Linked List Vis](images/linked-list.jpg)

#### Are lists in lisp are heterogeneous?
* `'(a-symbol 5 3.14 "Mark" (abc))`
* `> (setf mylist '(a-symbol 5 3.14 "Mark" (abc)))` sets function
  * `my-list` is head ptr
  * the `'` in `'(a-symbol...` declares that `a-symbol` is not a function.
  * returns `(a-symbol 5 3.14 "Mark" (abc))`
  * `> mylist` returns `(a-symbol 5 3.14 "Mark" (abc))`
  * vs. in `(setf x (+ 5 6))`, 5 + 6 executed and returned then assigned to x

### Manipulating Lists

#### Setting functions
* `> (setf friends '(tom dick sally)))` returns `(tom dick sally)`
* `> friends` returns `(tom dick sally)`
* ` >'friends` returns `friends`

#### Returning first elem of list
* `(car <list>) || (first <list>)`
  * `(car friends)` returns `tom`
  * `(car hoya saxa)` returns `hoya`
  * `(car '((mark marc) tom sally))` returns `(mark marc)`
  * `(cdr(car'((mark marc) tom sally)))` returns `(marc)`
  * Doesnt change list itself

#### Returning returns rest of list sans the first elem but doesnt change list itself
* `(cdr <list>)`
  *  `(cdr friends)` returns `(dick sally)`
* `(setf friends (cdr friends))`
  * returns `(dick sally)`

#### Returning elems at a certain pos in list
* `(nth pos list)`
  * `(nth 1 friends)` returns `dick`
  * this is a linked list traversal
  * also `first, second, third, ... tenth` functions for getting elems

#### `member` & keywords
* Every element of a list is a member of the set it represents. 
* When `member` returns true, it returns the part of the list beginning with the object it was looking for. 
  - `> (member 'mark friends)` returns nil
  - `> (member 'dick friends)` returns `(dick sally)`
    - this is a __ptr__ to elem in LL
  - `> (member 'sally friends)` returns `(sally)`
* By default, `member` compares objects using `eql`. This will not work if we're comparing lists if the element you're looking for is the first element in the list. 
  - `[11]> (member '(1 3) '(((1 1) east 45) ((1 3) north 25)))` returns NIL
* To make this work, we need to (1) specify the correct comparison function and (2) specify a function to retrieve the key for making comparisons
* __Keyword__ : a symbol preceded by a colon. 2 argument types:
  - `:test`
    + If you pass some function as `:test` arg in a `member` call, then that funct will be used to test for equality instead of `eql`
    + `(member '((1 3) north 25) '(((1 1) east 45) ((1 3) north 25)) :test #'equal)` returns `(((1 3) NORTH 25))`.
    + `(member '(a) '((a) (z) :test #'equal)` returns `((A) (Z))`
  - `:key`
    + This argument specifies a function to be applued to each element before comparison 
    + `(member '(1 3) '(((1 1) east 45) ((1 3) north 25)) :test #'equal :key #'car)` returns `(((1 3) NORTH 25))`
    + `(member 'a '((a b) (c d) :key #'car)` returns `((A B) (C D))`

#### Putting Lists Together `format`
* `(format t "~d:~f This is a ~s! ~a ~a ~%" 5 4.2 "test" 'mark friends)`
  - ~d = decimals
  - ~f = floats
  - ~s = strings
  - ~a = symbols
  - ~% = newline 
  - returns  `5:4.2 This is a "test"! mark (tom dick sally)`
  - then format returns `nil`

#### `prog1` Blocks
Syntax:
```
(prog1    //returns result of <form_1>
  <form_1>
  ...
  <form_n> )

(prog1 'a 'b 'c)
A

( progn   //returns result of <form_n>
   <f_1>
   ...
   <f_n> )

(progn 'a 'b 'c)
C
```
Semantics:
* Evaluates forms 1–n
* prog1 evaluates to the valuation of <form1>

### 9/12 Lisp 

#### Defining Functions
Syntax:
```
(defun <name> (<params>)
  <form>)
```
Semantics: 
* Defines a function with the name _funct-name_ with the params _param-list_ and the body _form_
* Evals to the function _function-name_

#### Functions: Euclidean Distance Example
$$\sqrt{(x1-x2)^2(y1-y2)^2}$$
```
(defun sqr(x)
  (* x x ))

(defun distance (x1 y1 x2 y2) 
  (sqrt ( + (sqr (- x1 x2)) 
            (sqr (- y1 y2)))))
```
#### Valid function statements
```
clisp
> (defun sqr(x) (x * x))
  H` sqr
> (sqr 5)
  25
> (load "dist.lisp")
  (distance 2 2 4 4)
```

#### `setf`
Uses first arg to define a place in memory, evaluates its second arg, and stores its resulting value in the resulting memory location 
```
(defun sqr(x)
  (setf y ( * x x))   //creates a global variable y
  y)
```
* Don't do very often (??)

#### Binding Local Variables: let and let*

Syntax:
```
(let[*] ((<variable1> <value1>)
        (<variable2> <value2>)
        ...
        (<variablen> <valuen>))
      <form>)
```
Semantics:
* Defines locally scoped variables 1–n
* Binds or assigns values 1–n to variables 1–n, respectively
* Evaluates <form>
* let evaluates to the valuation of <form>
* let* forces the __sequential__ assignment of variables 1–n
  - important if you have dependencies

##### Global Variables
If the symbol x already has a global value, stranger happenings will
result:

```
\> (setq x 7)
7
\>  (let ((x 1)
        (y (+ x 1)))    //adds 1 to global value of x (7)
    y)
8
```

The val1, val2, etc. inside a let cannot reference the variables var1,
var2, etc. that the let is binding. For example,

```
 \> (let ((x 1)
        (y (+ x 1)))
    y)

Error: Attempt to take the value of the unbound symbol X
```
##### Let* special form 
- Allows values to references variables defined earlier in the let. 
```
> (setq x 7)
7
> (let* ((x 1)
         (y (+ x 1)))
    y)
2
```

#### Example of distance formula using `let`
V1
```
(defun distance (x1 y2 x2 y2)
  (let ((x-square (sqr(-x1 x2)))
        (y-square (sqr(-y1 y2)))
    (sqrt(+ x-square y-square))))
```

V2
```
(defun distance (x1 y2 x2 y2)
  (let * ((x-square (sqr(-x1 x2)))
        (y-square (sqr(-y1 y2)))
    (result(sqrt(+ x-square y-square))))
```
Because `result` depends on values of x-square & y-square, need to use `let *`

#### Branching: `if-then-else, when, unless`
Syntax:
```
(if <test>
  <then-form>
  [<else-form>])
```
Semantics:
* Evaluates <test>
* If <test> is not nil, evaluates <then-form>
* Otherwise, if <else-form> is not nil, evaluates <else-form>
* if evaluates to the form evaluated

Also:
* (when <test> <form>) ≡ (if <test> <then-form>)
* (unless <test> <form>) ≡ (if <test> nil <else-form>)
* (unless (not <test>) <form>) ≡ (if (not <test>)
<then-form>)

#### Relational/Equality Operators and Functions 
* equalp — same expression?
* equal — same expression?
* eql — same symbol or number?
* eq — same symbol?
* = — same number?
* atom — is it an atom?
* numberp — is it a number?
* consp — is it a cons?
* listp — is it a list?
* symbolp — is it a symbol?
* boundp — is it a bound symbol?
* Also member, null, zerop, plusp, minusp, evenp, oddp, <,
<=, >=, and >.
  - important for checking data/user input 

#### Boolean operators
```
(and (< grade 90)
      (> = grade 80) //can have > 2 conditions if you want 
      (....))
(or (eq response 'q)
    (eq response 'Q))
(not (equalp-))
```
#### Nested if-then-else statements: `cond` 
Syntax: 
```
(cond(<test1> <consequent1>)
      (<test2> <consequent2>)
      ...
      (<testn> <consequentn>))
```
Semantics:
* Evaluates tests 1–n
* Evaluates the consequent of the first test that is not `nil`
* `cond` evaluates to the valuation of the consequent or to `nil` if
no test is not nil
* Note that the literal `t` always evaluates to true

#### `cond` example 1
```
(defun num-to-letter (grade)
  (cond ((>=grade 98) 'A)
        ((>= grade 80) 'B)
        ...
        ((>= grade 64) 'D)  
        (f 'F')))
```
* Like a switch statement, once one statement evaluates to true then exits 

#### `cond` example 2: finding atoms in list
```
(defun m-member (atom list)
  (cond ((null list) nil)
        ((eql atom (car list)) t)    //car = get 1st elem. if T, return t
        (t (m-member atom (cdr list)))))  //call m-member recursively 

1. (m-member 'jack '(hoya saxa))
        ^ returns nil 
2. (m-member 'jack 'saxa)
        ^ returns nil 
3. (m-member 'jack '())
```
V2 using counter
```
(defun m-member (atom list & option (counter o))
  (cond ((null list) nil)
        (( eql atom (car list)) counter)    //car = get 1st elem. if T, return t
        (t (m-member atom (cdr list)(1+ counter)))))  //call m-member recursively 
```
#### `cond` example 3
```
> (setq a 3)
3
> (cond
   ((evenp a) a)      ;if a is even return a
   ((> a 7) (/ a 2))  ;else if a is bigger than 7 return a/2
   ((< a 5) (- a 1))  ;else if a is smaller than 5 return a-1
   (t 17))            ;else return 17
2
```
#### Looping through Lists: `doList`
Syntax:
```
(doList (<var> <list-form> [<result-form>])
  <form>)
```
Semantics:
* Iterates once for each element in <list-form>
* Assigns the element to <variable>
* Evaluates <form>
* Upon loop termination, evaluates <result-form>
* dolist evaluates to the valuation of <result-form>

dolist provides straightforward iteration over the elements of a list. First dolist evaluates the form `listform`, which should produce a list. It then executes the body once for each element in the list, in order, with the variable var bound to the element. Then resultform (a single form, not an implicit progn) is evaluated, and the result is the value of the dolist form. (When the resultform is evaluated, the control variable var is still bound and has the value nil.) If resultform is omitted, the result is nil.
[do List Macro](https://www.cs.cmu.edu/Groups/AI/html/cltl/clm/node89.html)

#### `doList` example: print elements
```
> (setf my-list '(a b c d))
> (doList (element my-list 'done') (format t "~a~%" element))

  a
  b
  c
  d
  done 

(dolist (x '(a b c d)) (prin1 x) (princ " "))
 A B C D 
NIL

(defun min-max-i (my-list)
(setf min (car my-list)
  (doList(element my-list)
    (cond ((NIL my-list) NIL)
          (( > min element) (min element))
    )
    (format t min)
  )))


```

#### Count-controlled loops: `dotimes`
Syntax:
```
(dotimes (<var> <upper-bound-form) [ <result-form>])
  <form>) 
```
Semantics:
* Iterates over [0,<upper-bound-form>)
* Assigns the counter value to <variable>
* Evaluates <form>
* Upon loop termination, evaluates <result-form>
* dotimes evaluates to the valuation of <result-form>

#### `dotimes` example
Calcs m^n power. Whatever value stored in result returned to dotimes, returned to let, and returned to funct call
```
(defun m-expt (m n)
  (let ((result 1))
    (dotimes(count n result)
      (setf result ( * m result)))))
```
#### General looping construct: do and do* 
Syntax:
```
(do[*] ((<var1> <init1> <update1>)
      (<var2> <init2> <update2>)
      ...
      (<varn> <initn> <updaten>))
    (<terminiation-test> <result-form>)
  <form>)
```
Semantics 
* Defines locally scoped variables `vari`
* Initializes them to `initi`
* Loops until `termination-test` is not nil by
  - updating `vari` by evaluating `updatei`
  - evaluating `formi`
* If termination-test is true, return result form to do
  - Upon loop termination, evaluates `result-form`
* if false, form executes
* do evaluates to the valuation of `result-formi`
* do* forces the sequential assignment of `vari`

#### `do` example
```
(defun m-expt (m n)
  //set m to result & do m * result n times
  (do ((result m (* m result)   
        //set counter to n - 1. for every loop, decrease counter by 1
        (counter (- n 1) (- counter 1))) 
      //termination test
      ((zerop counter) result)))  

\> (m-expt 2 3)
8
```
#### `dolist` example
Not changing list, but traverse it using an inexplicit current pointer
```
(defun mydolist (list)
  (do * ((result nil)          
        (element (car list) (car list))   
        (list (cdr list)))      //traverse list using cdr
      //termination. when reach end of list, element = nil
      ((null element) result)
    (format + "~a~%" element)))    

(defun mydolist (list)
  (do ((result nil)          
        (element (car list) (car list))   
        (list (cdr list)))      
      ((null element) result)
    (format + "~a~%" element)))      
```

```
(defun mydotimes(n)
  (do ((result n)
    (i 0 (1+ i)))    
    ((> = i n) result)
  (format "~q%" i)))

      //1+ is a defined increment function. could also be (+ i 1)
```
## 9/14 Lisp

```
(setf(second friends) 'richard')
richard
\>friends
\>(tom richard sally)

cons 'tom (cons'richard (cdr (cdr friends)))
\> (tom richard sally)
```
### Arrays
Creation
* `(setf single (make-array '(4)))`
  - Returns `#(NIL NIL NIL NIL)`
* `(setf double (make-array’(2 4)))`
  - Returns `#2A((NIL NIL NIL NIL) (NIL NIL NIL NIL))`
* `(setf double (make-array ’(2 4) :intial-element nil)) ;`i
  - Common lisp req you to specify an init element

Access
* `(format "~a~%"(aref single 1))`
* `(format "~a~%"(aref double 0 0))`

Assignment
* `(setf(aref single 1) 'hoya)`
* `(setf(aref double 0 0 )'saxa)`

### Binary Trees
![binary tree](images/binary-tree.png)
* Nested lists
* The first elem of list is the root 
`(<root> <left-child> <right-child>)`
```
(A (B nil nil)      //implies that B doesnt have children
   (C (D nil nil)
      (E (F nil nil)
         (G nil nil))))
(A (B)              //this works too - don't need to write out the nils
    (C (D)          
    (E (F) 
        (G))))
```



(O(R(E(G)(O))(E(G)(T)))(O(N(W)(H))(A(Y)(S))))

* Edge case: if node A has one right child C, must use nil `(A nil (C))`

### Graphs 
![graph](images/graph.png)
* __Property lists__ really good for representing graphs

```commonlisp
//Encodes graph as a property list
(setf (get 'fortran 'influenced) '(lisp))    
(setf (get 'logic 'influenced) '(lisp prolog))
(setf (get 'lisp 'influenced) '(scheme common-lisp))
(setf (get 'common-lisp 'influenced) '(clos))
;influenced isnt a keyword, issa indicator 

//Can have multiple properties
(setf (get 'fortran 'year) 1950)
//Traverse list
\> (get 'fortran 'influenced)
(lisp)
\> (get 'lisp 'influenced)
(scheme common-lisp)
```
### Compile 
`(compile-file "dist.lisp")` produces dist.o or dist.fasl
`(load "dist.o")`

### Trace
`(trace m-member` shows function calls and return values
```
\> (m-member 'mark friends)
1. TRACE: (m-member 'mark '(tom dick sally))
...
4. TRACE: m-member ==> nil
```
### Funcall
`(funcall <fuction> <arg1> ... <argn>)` uses funcall to set data and also call function. no distinction btwn code and data 

`(funcall #'append '(2 4 6 8) '(a b c)` returns ( 2 4 6 8 a b c) 
* why dont u just call append? well... 

```
                      //set comparison operator to > 
(defun bubble-sort( array & optional (compare-f #'>))   
  let
    dotimes
      dotimes          //compare-f used here 
        (when (funcall compare-f (aref array i) (aref array (1+ j)))
```
### `mapcar` 
`(mapcar <function> <list>)`
* Applies something to all elements of list by picking off all elemts in succession with `car` and passes that to a function 
```
\> (mapcar #'sqr '(2 4 6 8))
(4 16 36 64)

//traverses list then applies binary operator 
\> (mapcar #'= '(2 4 6 8) '(2 2 8 8))   
(T NIL NIL T)
```

### Apply
`(apply # 'append ((....) (..)))`
*  ((...) (..)) is a list of args 
```
(apply #'append ((2 4 6 8) (a b c)))
\> (2 4 6 8 a b c)

(defun average(numbers)
  (/ (apply #'+ numbers) (length numbers)))
```
### Lambda expressions
* Use lambda expression to define a simple, anon/unnamed function inline 
```
\> (mapcar #'sqr '(2 4 6 8))
(4 16 36 64)

\> (mapcar #'(lambda (x) ( * x x)) '(2 4 6 8))
(4 16 36 64)
```
### Project 1 notes
* submit.zip has a main.lisp file with all the function defs and HONOR file with the honor code
* No need to error check
* tic tac toe - make sure you make deepy copy not shallow copy
* Defining global vars
  - Prefixed with asterisks 
  - (defvar *tree* nil)
  - (setf *tree* '(A (B) (C)))
* What does `(cons 'A 'B)` do
  - Creates a dotted list `(A.B)`. Saves memory, but this is neglible irl
         --- ---
        |   |   |--> violet
         --- ---
          |
           --> rose

## 9/19 
### Comments
`;;;;` Header comments 
`;;;` Funtion comments 
`;;` Between line comments
`;;` Check if input is valid
`#| .... |# ` Block comments 
```
;;;;
;;;; P1
;;;; Sabrina Ma
;;;; 9/17/17

;;
;; min-max-i
;; Check if input is valid
(if (...)
... ;valid input
)
```
### Display how funct was interpreted
`#'<function>` 

### Create tree data structure 
```
(setf *tree* '(A (B) (C)))
;; Set root to first node
(setf root (car *tree*))
;; Set left/right child to second/third elem in list
(setf left-child (second *tree*))
(setf right-child (third *tree*))
```
### Creating a main function
```
(defun main()
  (format t "Distance is ~f~%" (distance 3, 4.2, 5, 6.7)))
\>(main)
Distance is 3293843
```
### Recursive functions
```
(defun min-max-r (numbers)   
  ;; Check if list if empty first
  (min-max-r-aux numbers (car list) (car list))

;; Auxillary function that is called recursively by min-max-r
(def min-max-r-aux (numbers current-min current-max)
  ;; Is numbers empty? 
  ;; Check car list against current-min/current-max
  ;; If true, replace current-min with new
  (let ((element (car numbers))))
  ;; Find current min
  (min-max-r-aux (cdr numbers) (if (> current-min element)
                            (car numbers)  
                            (current-min)
  ))
) 

```
### Combining atoms
```
[16]> (setf curr-min 1)
1
[17]> (setf curr-max 3)
3
[18]> (list curr-min curr-max)
(1 3)
```
### Search
* Uninformed methods
  - Depth-first
  - Breadth-first
* Informed methods
  - Doesnt guarantee optimal solution
* Admissible methods 
  - Guarntees optimal solution

* Big diff from data struct b-tree search: in AI, search as grow tree 
* Weighted graphs - trying to max utility. eval function deterines which branch is more favorable 

### Five components of a problem
1. The __initial state__  
  - Ex) Agent in Romania originally in city Arad might be described as `In(A)`
2. Set of __actions__ 
  - State: `In(A)`, Actions: `{Go(B), Go(C), Go(D)}`
3. A __transition model__ that describes what ea action does and its result 
  - __Successor__: any state reachable from a given state by a single action
  - `RESULT(In(A),Go(B)) = In(B)`
4. __Goal-test function__ 
  - Determines whether given state is goal state 
  - `{In(G)}`, `checkmate`
5. __Path-cost function__
  - Assigns a numeric cost to each path and sums the costs of the individ actions along the path 
  - Step cost of taking action `a` in state `s` = `c(s, a, s')`
  - Ex) Length in km. 

These five components of a problem can be gathered in a data structure that is given as input to a problem-solving algorthim. 

### State space
* The set of all states reachable from the initial state by any sequence of actions.
* Defined by the inital state, actions and transition model. 
* A acyclic directed graph in which nodes denote states and links denote transitions btwn states 

### More definitions: path, solution, path cost 
* __Path__: sequence of states/actions from init state to some other state 
* __Solution__: path from the initial state to a goal state. 
  - Solution quality is measured by the path cost function, and an __optimal solution__ has the lowest path cost among all solutions.

### We will never generate the same path twice, so we can assume that the possible action sequences starting at the initial state (search space) form a 
* __Search tree__ with the init state at the root  
* Brances: actions
* Nodes: states 

### General tree-search algorthim 
Essence: following up one option now and putting the others aside for later, in case the first choice does not lead to a solution.

* Start at root node (init state)
* Test whether this is the goal state
  - Edge case: init state == goal state
* Expand current state by applying action to current state and generate new set of states 
  - Ex) Add 3 branches from parent node -> 3 new child nodes 
* Chose which of the new states to consider further 

#### General graph-search algorthim 
* Frontier: Nodes to visit 
* Current: Node I am visiting 
* Explored: Nodes I have visited 

* Initialize frontier and explored
* Set initial to current 
* While frontier is not empty and current != goal state
* Pop node off frontier   
  - If goal state, return 
  - Generate successors and push to frontier 
  - Add node to explored  
  - 
```
function GRAPH-SEARCH(problem) returns a solution, or failure 
  initialize the frontier using the initial state of problem
  initialize the explored set to be empty
loop do
  if the frontier is empty then return failure
  choose a leaf node and remove it from the frontier
  if the node contains a goal state then return the corresponding solution 
  add the node to the explored set
  expand the chosen node, adding the resulting nodes to the frontier
    only if not in the frontier or explored set
```
### Three criteria for evaluating methods for solving problems
1. __Completeness__ - guaranteed to find a solution if it exists? 
2. __Optimality__ - guaranteed to find optimal solution? 
3. __Time/Space complexity__ - What proprotion of the state space must we grow/search to find goal? 

### How is time/space complexity expressed? 
* `b`: branching factor. # of successors of a node
* `d`: depth. number of steps along a path from the root 
* `m`: max length of any path in state space 

Time is often measured in terms of the number of nodes generated during the search, and space in terms of the maximum number of nodes stored in memory.

## Uninformed Search Strategies 

### Breadth-first search 
* All nodes expanded at given depth in tree before nodes in next level are expanded 
* Shallowest unexpanded node is chosen for expansion
* Implemented with a FIFO queue for the frontier 

### Evaluate BFS using the 3 criteria 
* __Complete__: if shallowest goal node is at some depth `d`, if branching factor `b` is finite, will eventually find it 
* __Optimality__: Shallowest goal node not necessaily the optimal one. Only optimal if path cost is a nondecreasing function of the depth of the node (all actions have same cost/path cost increase as depth increase) 
* __Time__: Let tree have `b` successors. Root generates `b` nodes at first, each of those generate `b` more... => O(b^d). not good 
* __Space__: Every node generated remains in memory. O(b^d). 

### Depth-first search
* Expands the deepest node in current frontier 
* Uses LIFO queue / stack 

```
(defun dfs (initial goal)
  (do* ((frontier (list(list initial)))
        (current (pop frontier) (pop frontier)))
        ((or(eq goal (car current)) (null current)) (reverse current)) 
    (doList (successor (get (car current) 'successor))
        (unless (member successor current)
          (push (cons successor current) frontier)))))
```

