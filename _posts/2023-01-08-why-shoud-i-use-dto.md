---
title: Why Should I use DTO
date: 2023-01-08 12:00:00
categories: [engineering]
tags: [java, security]
---
Data Transfer Objects (DTOs), in Java, are objects with the purpose to aggregate data and reducing the number of calls needed between the applications. furthermore, DTOs can help your application to be more secure by preventing accidental data leaks, but before start talking about DTOs, let’s take a look at other types of objects in Java.

# POJO and Java Bean

A Plain Old Java Object (POJO) is an ordinary Java Object. It can be any class and the only restriction is the ones prescribed by the Java language. The reason for creating POJO is for re-usability and increased readability.

```java
public class CoffePOJO {
	public String name;
	public List<String> ingredients;

	public CoffePojo(String name, List<String> ingredients) {
		this.name = name;
		this.ingredients = ingredients;
	}
}
```

A Java Ben is a POJO with all properties private and will be accessed with getter and setter methods, the Java Bean should have a no-arg constructor.

```java
public class CoffeeBEAN implements Serializable {

   private String name;
   private List<String> ingredients;

   public CoffeeBEAN() {
   }

   public String getName() {
       return name;
   }

   public void setName(String name) {
       this.name = name;
   }

   public List<String> getIngredients() {
       return ingredients;
   }

   public void setIngredients(List<String> ingredients) {
       this.ingredients = ingredients;
   }
}
```

# But why should I use a DTO?

The purpose of a DTO is to carry data between processes, but a good DTO only contains the information needed for that specific part of the system. The general advice to have a good DTO is to keep it as simple, small, and straightforward as possible.

The DTO only carries data, so ab immutable data structure would be a great fit for a DTO, for new Java versions a Record would be a great fit - especially because JSON serialization libraries like Jackson support Java Records.

Decoupling the Domain Model from the Presentation layer with this DTO pattern is a good practice, simple DTO contains the data needed for the subsystem when we expose all the domains we lead to unnecessary data being available outside of the system and potential data leaks.

Say our API has a function to find all customers but does not use a DTO:

```java
@GetMapping("/customers")
public List<Customer> getAllCustomers(){
   return repository.findAll();       
}
```

If our customer object is the same as in our previous example, we are already displaying too much information. Do we actually need the `id` or the list of favorite `coffees`? It gets worse if we decide to attach more information to the customer, like a home address.

```java
public class Customer {
   public Long id;
   public String firstName;
   public String lastName;
   public List<Coffee> coffees;
   public String homeAddress;
}
```

If we don’t filter in our endpoint, we suddenly create a data breach, since providing a full name and home address is considered a privacy breach in many countries. Using a DTO we can choose the information that will be shared with other systems and just send the necessary information:

```java
public class CustomerDTO {
   private String firstName;
   private String lastName;
}
```

For better security, I would recommend making your DTOs specific and limiting the amount of reuse. If you are reusing your DTOs for different functions, you should clearly understand where these DTOs are used before changing them.

*This is a personal summary. You can find the original article here:* [https://snyk.io/blog/how-to-use-java-dtos/](https://snyk.io/blog/how-to-use-java-dtos/)
