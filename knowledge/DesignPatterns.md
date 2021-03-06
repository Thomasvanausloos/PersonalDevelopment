# Design Patterns

## 1) Factory

### Factory method

Create a static method within a class that is responsible of creating new instances of that class. We can use such methods to validate info before we make an actual object for example. To make sure this validation is performed it is typpical to make the classes' constructor private.



```java

public class Person(){

	

    private String firstName;

    private String lastName;

    

	//Private constructor

	private Person(String firstName, String lastName){

	

    	this.firstName = firstName;

        this.lastName = lastName;

	}

    

    //Factory method

    public static Person createPerson(String firstName, String lastName){

    

    	if(isValid(firstName,lastName)){

    	

        	return new Person(firstName,lastName);

        }

        throw new ValidationException("invalid name");

    }

}

```



### Simple Factory



Create a factory class with the purpose of creating new objects.



```java

public class PersonFactory{



	public Person createPerson(String firstName, String lastName){

    	return new Person(firstName, lastName);

    }

}

```



```java

public class TestClass{

	private PersonFactory personFactory;

    

    public static void main(String ... args[]){

    	personFactory = new PersonFactory();

        Person testPerson = personFactory.createPerson("aFirstName","aLastName");

    }



}

```

## 2) Strategy Pattern



 The strategy pattern is about letting objects decide over their own implementation of a certain **behaviour**. The general behaviour will be abstracted in an interface. 

A simple design rule of strategy pattern is: 



Seperate what stays the same from what differs. The behaviour that differs can be extracted into an interface. All different types can then implement that interface and decide over their own behaviour.



#### Example:



An example of an interface that decides the behaviour of a certain hand (in the game of rock-paper-scissors). 



```java

public interface InterfaceHand{



public boolean winsFrom(HandEnum hand);



public boolean loosesFrom(HandEnum hand);



}



```



Each kind of hand (rock, paper or scissors) will implement the interface above and implement its methods on its own way. Each object will decide its own behaviour! Example:



```java

public class Rock implements Interfacehand{

	

	@Override

	public boolean winsFrom(HandEnum hand){

	

		if(hand == HandEnum.SCISSORS){

			return true;

		}

		return false;

	}

	

	@Override

	public boolean loosesFrom(HandEnum hand){

	

		if(hand == HandEnum.PAPER){

			return true;

		}

		return false;

	}

}

```

In the example above we see that the Rock class defines its own winsFrom and loosesFrom behaviour. We can make sure the class has that certain behaviour by letting it implement the InterfaceHand interface. 



##3)Decorator Pattern



The decorator pattern is a structural pattern. It has two different ways of using it:



1. The first way of using the decorator patters is to add functionality to an existing object without altering its structure. The already existing object will be wrapped by a new class, adding the new functionallity. 

2. A second way of using the decorator pattern is by wrapping an object into different layers where each layer adds for example a piece of a price or description. This follows the same logic as the builder pattern.



#### Example of case 1:



Note that using the interface here is a usage of the strategy pattern...



```java

public interface Shape{



	public void draw();



}

```



```java

public class Rectangle implements Shape{



	public void draw(){

	

		sout("Shape: Rectangle");

	}

}



```



```java

public class ShapeDecorator implements Shape{

	

	private Shape shapeToDecorate;

	

	public ShapeDecorator(Shape shape){

	

		this.shapeToDecorate = shape;

	}

	

	

	public void draw(){

	

		shapeToDecorate.draw();

		sout("Extra decoration");

	}

}



```



The shapeDecorator will wrap itself around the shape object and store it in its instance field. It will then add functionallity to it's draw() function by calling the draw function of the shape and then adding its own functionallity.



#### Example of case 2:



In the second example we can have many different and independant layers and one base-layer. this will be the base that we will wrap in the other layers. 

Each layer class will implement the same interface, have a field to store an object of type of that interface (the previous layer that will be encapsulated). The current class's methods will call the methods of the previous object (stored in its field at the moment of creation) and add some of its own info.



##### The interface:



```java

public interface PricePackage{



	public int getPrice();

}

```

##### The base layer:

```java

public class BasePackage implements PricePackage{

	

	// Implement the method

	public int getPrice(){

	

		return 1000;

	}

}

```

Note that the class above is the base layer and thus has no field to store a previous pricePackage! This class will be wrapped by other layers but it wont wrap anything itself.



##### An example of another layer:



```java

public class WrapperLayer(){



	// Field to hold the previous layers

	private PricePackage previousPackage;

	

	//Constructor stores the previous layer into its field

	public WrapperLayer(PrivePackage pack){

	

		this.previousPackage = pack;

	}

	

	//Implement the method by adding info to the previous layer(s)

	punlic int getPrice(){

	

		return previousPackage.getPrince() + 50;

	}

}



```

##### Using the example above:

```java

public class TestClass{

	public static void Main(String ... args[]){

	

	PricePackage package = new WrapperLayer(new BasePackage());

	// Will return 1050

	package.getPrice();g

	}

}



```

