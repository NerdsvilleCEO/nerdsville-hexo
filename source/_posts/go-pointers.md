---
title: Go Pointers
date: 2016-02-15 16:00:54
tags:
---
In Go, we have pointers, just as there are in C. Pointers can be a difficult concept to grasp, so I am writing this blogpost in hopes that it will demystify what we all know as pointers :)

A pointer, simply is a reference to an address in memory. 

If we have a pointer such as:
    
    var *p1 string
    implicitString := "Test"
    p1 = &implicitString
    
By prepending the variable name with the & symbol, we are essentially saying "give us the address of the variable" and then we are taking that address and setting it as the reference of the pointer.

If we want to use the pointer, we could use it as any other variable with one exception:
  
    fmt.Println(*p1);
  
We have to use the * prefix in order to retrieve the actual contents, because remember, earlier by using the & prefix on implicitString, we stored the address inside of the pointer. The * prefix de-references the pointer in order to get the value at that address.

We need to be careful when using pointers, though, because we can accidentally alter memory which we did not intend to modify!

	*p1 = "Test2"
	fmt.Println(implicitString)
	=> "Test2"
	
If we modify the contents of a pointer without copying, we actually end up modifying the contents of that address in memory, and therefore anything that points to it will be modified!

    
