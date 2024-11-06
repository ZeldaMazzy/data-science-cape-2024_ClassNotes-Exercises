## 0. Terminology
- **Python** — a beginner-friendly programming language built for many things, but excels in tasks related to data science due to extensive community support and swathes of extensions
- **Variable** — a placeholder for a value, such as a number, string of letters, or true/false. It can be set and reused for many different things.
- **Function** — a subset of code that performs one specific task. It sometimes takes an input and returns a single output, which you can use to run calculations and more.
- **Conditionals** — also known as if/else statements, these are diverging paths in your code that you can program to run in specific conditions. 
## 1. Digging In
Let's first start by going to python.org and installing Python. Most of this class is going to take place in Jupyter Notebooks, but I'm also going to show you how to run these from a text editor. I'll be using Sublime — however, you can use your IDE of choice. Personally, I really like VS Code too.
#### Our first program
Open up your text editor and create a new file called `helloworld.py`. We gotta type out Hello World. It's the law.

```python
print("Hello, World")
```

To run your program, open up your terminal or command prompt, then navigate to the folder containing the file with `ls` and `cd` to find the folder. Then, you can just run `helloworld.py` and it'll print to the terminal.
## 2. Variables
Variables are pieces of data we name that can be used to hold different values. There are several different data types in python, where the three most common are *numbers*, *strings*, and *booleans*. 

Numbers are uhh numbers, and can be either *integers* or *floats*. Integers are the default and floats are determined by adding a decimal place.

Strings hold letters together (also numbers, but aren't treated as such since you can't do arithmetic with them). Strings are made by wrapping the letters together with apostrophes or quotation marks.

Booleans are simply a true or false binary value. Let's take a look at some examples.

```python
age = 31 #number type
name = "Zelda"
height = 6.1
is_teacher = True
   
print(f"Hi, I'm {name} and I'm {age} years old") #the 'f' inserts the variables when you put them in curly brackets
```
#### Working with Numbers
There are a few different functions you can do with these different datatypes. For instance, basic arithmetic is possible with a few different operators. 

```python
+  # addition
-  # subtraction
/  # division
*  # multiplication
** # exponent
%  # modulus (remainder)
```

These all work how you would expect, but I imagine some of you have never seen the percent sign % operator before. It returns the remainder after dividing two numbers. For instance, 4 % 2 = 0, because the quotient of $4 / 2$ is an integer. However, 3 % 2  = 1, because there is a remainder of 1 when you divide 3 by 2.

```python
# you can use this to find even numbers, because their modulus by 2 is always 0

even = 10 % 2  # 0
odd = 11 % 2   # 1, so not even since there's a remainder
```

You can use operators when printing out data, for instance, or you can use it to create new variables entirely.

```python
example_sum = 2 + 2     # 4
example_product = 2 * 3 # 6

print(5 ** 2)           # prints 25
```

You can increment a variable by adding a number to itself. You can do this with all operators.

```python
x = 2
x = x + 2
print(x)   # 4
```
#### Working with Strings
We'll get into more of this later, but strings have lots of different functions too. There is a non-exhaustive list of all string operations you can find about Python all over the Internet, so I'll start with simple ones like .lower().

`.lower()`

```python
hello = "Hello World"
print(hello.lower())  # hello world
```

`.upper()`

```python
hello = "Hello World"
print(hello.upper())  # HELLO WORLD
```

`.replace()`

```python
hellow = "Hello World"
print(hello.replace("World", "Students")) # Hello Students
```

There are many more, but the last thing I want to show you is *indexing*. You can index a string in Python using square brackets []. Starting at 0, it gets the letter in that place. For instance, "Hello" is five letters. "Hello"[0] = "H", since indexing begins at zero. In this case, the last letter would technically be in the 4th place, not the 5th place, so "Hello"[4] = "o", and "Hello"[5] gives you an error, since that doesn't exist.

```python
hello = "Hello World"

# H e l l o   W o r l d
# 0 1 2 3 4 5 6 7 8 9 10 (empty spaces count too)

dub = hello[6]
print(dub) # prints "W"
```
## 3. Functions
It's time to get into functions! These are reusable bits of code that perform specific tasks. They usually take an input and generate an output. For our case, that's exactly what we're going to do.

We're going to get lots of practice with functions, so don't be discouraged if you don't entirely grasp this today.

Here is the anatomy of a function
1. `def` keyword, short for "define", which signals we're creating a function
2. function name. In the example below, we name it `greet`
3. parentheses, to contain any *arguments*
4. arguments. In the example below, we have `name`
5. function body, which is the logic within. It must be indented, as Python uses white space to determine how logic flow, and where functions begin and end.

```python
def greet(name):
	print(f"Hello, {name}!") # use a tab to indent after the function
	
# Now, we can call the function
greet("Zelda") # prints "Hello, Zelda!"
```

Functions can also *return* values, that we can store in variables for later. Using the return keyword doesn't print the value until we tell it to later.

```python
def calculate_sum(a, b):
   return a + b

# we indent back to the left, and the function is complete
my_sum = calculate_sum(1, 2)
print(my_sum) # prints 3
```
## 4. Control Flow / Conditionals
Conditionals are subsets of code that evaluate to either true or false. They can be used to run certain subsets of code or not when combined with `if`, `elif`, and `else`.

We use a few different operators for these, including equal signs \==, the greater / less than symbols, and the not-equal sign !=. We use two equal signs when comparing values instead of assigning them.

```python
2 == 2 # true
2 > 1  # true
2 < 1  # false
2 != 2 # false
2 >= 2 # true
2 <= 3 # false
```

In our code, we can put conditionals in `if` statements. If that statement is true, then it runs the code indented below it.

```python
x = 5
if x > 2:
	print("X is greater than 2")
```

We can chain conditionals together with `elif` and have a second conditional that attempts to run if the first one fails. In the example below, the first one fails since 5 is not greater than 6. So, it falls to the second one.

```python
x = 5
if x > 6:
	print("X is greater than 6")
elif x < 6:
	print("X is less than 6")
```

If the first one happens to succeed, `elif` won't run (but a second `if` will run, since it restarts the chain). We can add as many `elif` clauses as we want, and they will all attempt in succession. Once one of them passes, the chain stops.

```python
x = 5
if x > 2:
	print("X is greater than 2") # it stops here
elif x > 4:
	print("X is greater than 4") # it never reaches this
```

If all of our `if` and `elif` cases fail, on the other hand, we can fall back to `else`, which captures all other possibilities.

```python
x = 5
if x > 5:
	print("X is greater than 5")
elif x < 5:
	print("X is less than 5")
else:
	print("X must be 5")
```

We can put all of this together in a function to see how it behaves.

```python
def check_temperature(temp):
   if temp > 90:
	   print("hottt")
   elif temp == 72:
	   print("absolute perfection")
   elif temp > 50:
       print("I mean, it's okay out") # this won't trigger if it's 72
   else:
	   print("man it's cold out")

check_temperature(100) # hottt
check_temperature(72)  # absolute perfection
check_temperature(60)  # I mean, it's okay out
check_temperature(20)  # man it's cold out
```

## 5. Lists
Lists in Python hold collections of objects or variables together and are denoted by square brackets [ ]. We'll use these quite a bit when we really dig into the Data Science side of python.

Lists, like strings, can be indexed. There are also another entire list (heh) of different methods we can apply to lists like `.append()` and `.pop()` to add, change, and remove elements.

```python
fruits = ["apple", "banana", "orange"]

print(fruits) # [ "apple", "banana", "orange" ]
```

Indexing lists allows us to print out and change items when needed.

```python
print(fruits[1]) # banana
fruits[0] = "pear"

print(fruits) # [ "pear", "banana", "orange" ]
```

`.append()` adds items to the list

```python
fruits.append("grape")
fruits.appent("watermelon")

print(fruits) # [ "pear", "banana", "orange", "grape", "watermelon" ]
```

`.pop()` removes the last element from the list

```python
fruits.pop()

print(fruits) # [ "pear", "banana", "orange", "grape" ]
```
## 6. Loops
Loops are one of your most powerful tools as a programmer. Imagine needing to write a list of 100 numbers, one by one — loops help make this easy, because otherwise you would have to type `print(1) print(2) ... print(100)` until the cows come home. There are a million different use cases I can list out. In Data Science specifically, we can use loops to access each individual column, row, and cell in a .CSV and access its data.
#### "for" loops
There are two types of loops we'll look at today, but I want to first focus on `for` loops, also known as `definite` loops. These run a specific number of times. As I mentioned earlier, imagine needing to list out a bunch of numbers. We can write two simple lines of code for that.

```python
for i in range(5):
   print(i) # just prints 1 2 3 4 5
```

Let's break this down:
1. `for` is the type of loop: a definite loop
2. `i` is a variable we create. We choose `i` usually because it can be short for `index`, and is a common letter chosen for loops in computer science. 
3. `in range(5):` is what we're looping through. `range(5)` automatically creates a list for us from 1 to 5 and looks like `[1, 2, 3, 4, 5]`. Then, each number takes the place of `i` as it goes through the loop iteration.
4. `print(i)` is the loop body. `i` starts at 1, prints, then goes back through to 2, prints, goes to 3, prints, and so on until 5.
5. The loop ends, and we have our output.

I mentioned that `range(5)` builds a list for us, but we can also loop through pre-made lists ourselves. 

Back by popular demand, it's our fruit list `[ "pear", "banana", "orange", "grape" ]`, and it's back with a vengeance. We can loop through each fruit and print out "Today, I'm having a [fruit name]", without needing to type that out individually. 

```python
fruits = [ "pear", "banana", "orange", "grape" ]

for fruit_name in fruits:
	print(f"Today, I'm having a {fruit_name}")
```

Instead of `i`, I went with a more explicit naming convention so that there wasn't confusion, since I'm not working with numbers.
#### "while" loops
To be honest, I don't use these as much, as the use cases call for much more precise applications. Most of the time, I use "for" loops, and you will probably use "for" loops for most of your career. However, it's necessary to know how "while" loops work as well.

`while` loops are also known as *indefinite loops*, because they don't rifle through a list with a given number of members. `while` loops can theoretically loop forever if you don't give them a good exit condition. Here's what one looks like.

```python
while 5 > 2:
	print("Hello")
```

Don't actually run the code above unless you want your terminal to crash, because it will just print Hello forever until you shut down your computer. The reason that happens is because it doesn't have a good exit condition. But what's *really* going on here?

1. `while` is the keyword, showing we're going into an indefinite loop
2. `5 > 2` is a conditional, like from earlier. It'll evaluate to true or false, and *while* it's true, the loop will continue. So, we need to make sure it breaks by changing something in the loop. Let's try a different approach.

```python
x = 0
while x < 10:
	print(x)
	x = x + 1
```

Now, with each loop, x increases by 1. When it reaches 10, it breaks the loop, and we no longer have to worry about infinite loops.

In general, it's just safer to use definite loops.
## 7. Dictionaries
Also known as Objects in many other programming languages, Dictionaries are a powerful data structure you'll be using quite a bit in Python. Dictionaries hold what are known as *key-value pairs*, where each Key has a corresponding Value.

```python
coach = {
   "name": "Zelda",  # key is Name, value is Zelda
   "age": 31,        # key is Age, value is 31
   "num_students": 5 # key is the Number of Students, the value is 5
}

print(coach["name"]) # Zelda
```

Just like lists and strings, you can index dictionaries too. But instead of accessing them with numbers, you access them with the Key names.

```python
coach["num_students"] = 6
print(coach["num_students"]) # 6
```
## 8. In-Class Exercises
You can do these all on one `.py` file, separated by comments, like the below example:

```python
# Question 1
...
# Question 2
...
# etc
```

1. **Basics**: Create a Hello World program, and insert your name into it.
2. **Variables**: Create two Number variables. Add them together in a third variable called `sum`.
3. **Functions**: Write a function that takes in a temperature and converts it from Fahrenheit to Celsius. Return the result so you can store the calculation in a variable.
4. **Control Flows**: Write a function that takes in a number grade and prints a letter grade: 90-100 is an A, 80-89 is a B, and so on. 59 and below is an F.
5. **Lists**: Create a list of your 5 favorite films. Add another film to it with `.append()`. Change the third film in the list to something else programmatically.
6. **Loops**: Loop through your list of films and print them as such: `Top 1: Return of the King`, `Top 2: Return of the King again`, etc. Use what you've learned so far to increment the numbers. You will have to get creative.
7. **Dictionaries**: Create a dictionary with your name, field of study, and three favorite films. Change your name to just your last name. Loop through the three films (again!).

#### No take-home exercises, enjoy studying Pandas!
