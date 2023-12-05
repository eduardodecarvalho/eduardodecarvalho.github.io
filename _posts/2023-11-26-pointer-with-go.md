---
title: Pointers with Golang
date: 2023-11-26 12:00:00
categories: [engineering]
tags: [golang]
---


I was not used to working with pointers before, as I started my life as a developer working with Java I never had to think about it, but now I’m entering the world of Golang, and is a subject I have to pay with. So in this article, I will try to cover everything I know about Pointers in Golang.

<h1>But first, what is a pointer?</h1>

A pointer is an int 8-byte (for 64-bit machines) long variable that stores the memory address of another variable. In other words, a pointer contains the location in memory where a specific value is stored. Which benefits you can take advantage of? You can use advanced operations, like dynamic memory allocation.

Now, let’s do some code:


```go
package main

import "fmt"

type Person struct {
	age int
}

func celebrateBirthday(person Person) {
	fmt.Println("Congratulations, today is your birthday!")
	person.age++
}

func main() {
	person := Person{
		age: 20,
	}

	fmt.Println("Age before birthday:", person.age)
	celebrateBirthday(person)
	fmt.Println("Age after birthday:", person.age)
}
```

So, this Go program defines a simple program that simulates a birthday celebration for a person.

The `main` function is the entry point of the program. It creates a `Person` struct with an initial age of 20, prints the age before calling `makeBirthday`, calls `makeBirthday` to simulate a birthday celebration, and then prints the age again after the birthday function is called.

What do you think will be printed on line 21 and line 23? When we call the function `makeBirthday` the value of the variable `age` inside `Person` should add `1`, right? Let’s see the result on the terminal:

![Passing struct gif](/assets/2023-11-25-pointer-with-go/gif/1_Pointers.gif)

Whaaaaaaaaaaaat??? But, I mean, you see we changed the value, right?

What happened we created a copy of the `person` variable and we changed the value **for the copy**. After the function run, we lost that data, so nothing happened. But how we can solve this problem?

Instead of passing the `Person` structure, we can point to the address in memory of the `Person`, so we can modify in memory the state of the variable, but how we can do it?

```go
package main

import "fmt"

type Person struct {
	age int
}

func makeBirthday(person *Person) {
	fmt.Println("Congratulations, today is your bithday!")
	person.age++
}

func main() {
	person := Person {
	age: 20,
}

fmt.Println("Age before birthday ", person.age)
makeBirthday(&person)
fmt.Println("Age after birthday ", person.age)
}
```

So, what we did? Instead of receiving the copy, we gonna receive a pointer to a player. It means, that instead of the structure of the player, we are receiving the 8-bit that points to the location in memory where the displayed structure is allocated. So we are modifying in memory the state of the variable `person`. So every function pointed to the address in the memory of the `person` will be updated by the function now. To do it, we used the `*` in the function and `&` in the variable we are passing on the function call.

Or we can just initialize our `person` pointing to the memory as a reference:

```go
package main

import "fmt"

type Person struct {
	age int
}

func makeBirthday(person *Person) {
	fmt.Println("Congratulations, today is your bithday!")
	person.age++
}

func main() {
	person := &Person {
	age: 20,
}

fmt.Println("Age before birthday ", person.age)
makeBirthday(person)
fmt.Println("Age after birthday ", person.age)
}
```

And we can se the result:

![Pointer working](/assets/2023-11-25-pointer-with-go/gif/2_Pointers_working.gif)

<h1>Pointer delivers to the developer great power, and we all know… </h1>

![Uncle Ben](/assets/2023-11-25-pointer-with-go/imges/Uncle-Ben.jpg)

In this case, the biggest threat is inconsistent data, especially in a multi-go routine environment, where we can update this pointer everywhere, with data risk conditions. Another risk is the `nil pointer`, which means a pointer that does not point to any memory address. It is the zero value for pointers in Go. When a pointer is declared but not assigned any value (or explicitly set to **`nil`**), it is considered a `nil pointer`, like in this example below:

![Pointers nil pointer gif](/assets/2023-11-25-pointer-with-go/gif/3_Pointers_nil_pointer.gif)

To have a better understanding of what happened above, we need to get familiar with the term **dereference.** Dereferencing a pointer means accessing the value stored at the memory address held by that pointer.

In our example, `person` is a pointer to the structure of `Person`, when we pass to the function using `*Person` we deference the pointer, retrieving the value stored at the memory address it points to, but when we assign `nil` for the pointer we erase the reference to the allocation of memory, leading to a runtime error.

This is why we need to be careful when we are working on manipulating pointers if the same memory address is been manipulated for different threads, for example, we need to work using thread-safe strategies.

<h1>But how we can work thread-safe in Golang?</h1>

In Go, ensuring thread safety often involves synchronization mechanisms to prevent multiple `goroutines` from accessing shared data concurrently. In our example, the `makeBirthday` function modifies the `age` field of a `Person` struct, which is shared across `goroutines`. Therefore, it's important to use synchronization to avoid race conditions.

Here's we have an example using `sync.Mutex`:

In Go, ensuring thread safety often involves synchronization mechanisms to prevent multiple `goroutines` from accessing shared data concurrently. In our example, the `makeBirthday` function modifies the `age` field of a `Person` struct, which is shared across `goroutines`. Therefore, it's important to use synchronization to avoid race conditions.

Here's we have an example using `sync.Mutex`:

```go
package main

import (
	"fmt"
	"sync"
)

type Person struct {
	age int
	mu  sync.Mutex // for synchronization
}

func makeBirthday(person *Person) {
	person.mu.Lock()         // Lock before modifying shared data
	defer person.mu.Unlock() // Ensure that the mutex is unlocked even if an error occurs
	fmt.Println("Congratulations, today is your birthday!")
	person.age++
}

func main() {
	person := &Person{
		age: 20,
	}

	fmt.Println("Age before birthday ", person.age)

	var wg sync.WaitGroup
	wg.Add(2) // Using 2 goroutines

	// First goroutine
	go func() {
    fmt.Println("- Running first goroutine")
		defer wg.Done()
		makeBirthday(person)
	}()

	// Second goroutine
	go func() {
    fmt.Println("- Running second goroutine")
		defer wg.Done()
		makeBirthday(person)
	}()

	wg.Wait() // Espera pelas goroutines terminarem
	fmt.Println("Age after birthday ", person.age)
}
```

In this new code, the `sync.Mutex` is added to the `Person` with the name of `mu`. The `makeBirthday` function now uses `mu.Lock()` to ensure the variable `age` will be modified after we unlock the address in the memory, using `mu.Unlock`, so only one `Goroutine` can modify the field at a time, preventing `race conditions`. At the end of the program, we are using the method `sync.WaitGroup` ensures that the program doesn’t exit before the `goroutines` complete their work.

![Thread safe gif](/assets/2023-11-25-pointer-with-go/gif/4_Pointers_thread_safe.gif)

<h1> So, when I should use pointers in Go?</h1>

There are a few scenarios in which working with pointers is necessary, like reducing **copy overload**.

When working with large data structures like slices or structs, passing them by value can lead to unnecessary copying. Using pointers to reference these structures can be more efficient.

Another scenario is when we need to allocate memory dynamically (using new or make), you get a pointer to the newly allocated memory, we call it **Dynamic Memory Allocation**. When we work using interfaces and we create methods for one specific type, pointers will satisfy our demand.

In general, use pointers when you need to modify the original data inside a function, to reduce copying overhead with large data structures when working with dynamic memory allocation, or when defining methods on types. However, Go encourages simplicity, and you shouldn't use pointers solely for the sake of using pointers; use them when they provide a clear benefit in terms of efficiency or functionality.
