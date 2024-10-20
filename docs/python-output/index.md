---
title: "Python Output"
date: "2018-07-17"
categories: 
  - "devops-tools"
---

This blog will help you about Python Output, or what the user sees from your program.  In  our sample program, we used the print() function to output your program’s inputs.

As you may have guessed by now, print() is the primary function used for displaying your output.

You will also learn about newlines and separators, characters which act as formatting tools for your screen output. With the help of these characters, you can show exactly how you want your program output to look on screen.

## print() function

 

1. Run IDLE
2. Create a new file
3. Enter the following on the command line:
4.  print "Nidhi"
5. The shell displays a syntax error regarding the missing parenthesis in the command. This is because you will need to enclose anything you want to output in parentheses.If you revise your program to include the parentheses:print(“Nidhi”)the error disappears.
    
    Next, enter the following:
    
    print(“NIdhi Gupta”, ”instructor”)
    
    This displays the following to the screen:
    
    NIdhi Gupta instructor
    
    This example shows that if you want to output different values in a single line, you may also enter them separately, each enclosed in quotation marks, with a comma separating each value. This improves the readability.
    
    Next, enter the following commands:
    
    course=”Python for Beginners
    
    print(“course”)
    
    Here, you define the string variable course as having a value = Python for Beginners, then you output the value of the variable course to the screen. Thus, the following string is printed to the screen:
    
    Python for Beginners
    
    Now, combine the variable course with another string, using a comma to separate the values.
    
    print(course, “Nidhi”)
    
    This outputs the following:
    
    Python for Beginners Nidhi
    
    Let’s try integers next. Entering:
    
    print(17)
    
    gives you the following:
    
    17
    
    If you enter a floating-point number and then press Enter:
    
    print(255.6987)
    
    Python will spew out
    
    255.6987
    
    to the screen.
    
    So far, you have seen how Python handles strings, numbers, and floating-point numbers. Let’s try a mathematical expression next.
    
    Enter
    
    print(255/76\*2+9-18)
    
    In the case of mathematical expressions, Python evaluates the expression, then prints out the results.
    
    \-2.2894736842105274
    
    If you look closely at how these values are displayed on your screen, you will notice a difference in how Python treats strings and numbers when using the print() function. Can you tell what it is?
    
    Numbers, regardless of whether they are integers or floating-points, can be entered without quotation marks. In contrast, you can only use the print() function on a string without quotation marks, if the string is a variable. That is if you have previously assigned a value to the string.
    
     
    
    You have now learned how to use the print() function with strings, variables, integers, floating-point numbers, and mathematical expressions. In the next section, you will learn how to use newlines and separators with the print() function to format the output shown on your screen.
