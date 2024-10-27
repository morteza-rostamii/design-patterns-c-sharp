<!--  

# Solid principles

  # Single responsibility principle
  # Open-closed principle
  # Liskov Substitution principle
  # interface segregation principle
  # Dependency inversion principle

#******** single responsibility ********:

# class should have only one thing to do!!

# basically have a model, route/controller, service/business-logic

#******** END single responsibility ********:

#******** open-closed ********:

# class, function, etc...! should be open for extension but closed for modification

so basically: you should create class and methods that:
  # they do one thing
  # and generally when you make some change in another part of the app you don't have to change these class or methods too!

# ex: instead of having a method that calculate shape area. create a object for each shape and have a calcArea() method on them

# don't do this:

# this way anytime you want to add a new shape, you have to change Shape_class
public class Shape {
  public double CalcArea(string shape) {
    switch (shape) {
      case shape == "circle":
        something
      case shape == "rectangle":
        some other formula
    
    }
  }
}

# do this:
public abstract class Shape {

  public abstract void CalcArea();
}

public class Rectangle : Shape {
  public double Length;
  public double Width;

  public override double CalculateArea() {
    return Length * Width;
  }

}

#******** End open-closed ********:

#******** Liskov substitution ********:

# in inheritance:

# 

# wrong !!
public abstract class Shape {
  public abstract double Area {get; set;}
}

public class Rectangle: Shape {
  public virtual double {get; set;}
  public virtual double {get; set;}

  // override Area prop
  public override double Area => Width * Height;
}

public class Square : Rectangle {

  public override double Width {
    get => base.Width;
    set => {
      // square has the same width, height
      base.Width = value;
      base.Height = value;
    }
  }

  public override double Height {
  
    get => base.Height;
    set => {
      base.Height = value;
      base.Width = value;
    }
  }
}

// Main

# now: basically if you replace the object of baseClass Rectangle with sub-class Square you calculate a different Area

Rectangle rect = new Rectangle();
or
Rectangle rect = new Square();

rect.Height = 10;
rect.Width = 5;
// different 
rect.Area;

# Correct way:

# so in this case we should be able to pass around any object of a child of Shape and our code works just fine!

public class Rectangle : Shape {
  public double Width {get; set;}
  public double Height {get; set;}

  public override double Area => Width * Height;
}

public class Square : Shape {

  public double SideLength {get; set;}

  public override double Area => SideLength * SideLength;
}

# basically Rectangle-base Square-child =: you square not a Rectangle really! => 
  # cause: Rectangle.Area = 50
  # and Square.Area = 25 
# but: Shape-base and Square-child =: both square and rectangle are a Shape.
Shape rect = new Rectangle {Width = 5, Height = 4}
Shape rect = new Rectangle {Width = 5, Height = 4}

# GPT:
==
Here’s a summary of the key points that align with LSP in your example:

Incorrect Approach:

The initial design violates LSP by having Square inherit from Rectangle. Since a Square isn’t a specialized Rectangle (it requires that Width and Height remain equal), substituting a Square wherever a Rectangle is used can lead to incorrect behavior. For example, setting the Width independently should not be allowed for a Square but is allowed for a Rectangle.
When you substituted Square for Rectangle, the Area calculation broke LSP because it produced different results than expected.
Correct Approach:

By making both Square and Rectangle inherit directly from Shape, you create independent classes that both meet the contract defined by Shape (in this case, having an Area property).
Square has its own specific implementation of Area (SideLength * SideLength), while Rectangle has Width * Height.
Now, you can substitute Square or Rectangle wherever a Shape is expected, ensuring consistent behavior without needing to consider unique constraints.
Core Principle:

The corrected approach adheres to LSP by ensuring that any instance of Shape (either Square or Rectangle) behaves correctly when used polymorphically. This is because Square and Rectangle are treated as distinct shapes rather than forcing Square to be a specialized type of Rectangle.

# another way of explaining this is:
  # Square in are case not a Rectangle 
    # cause: new Rectangle {Width = 10, Height = 5}
    and: new Square {SideLength = 10}

#******** End Liskov substitution ********:

#******** Interface Segregation ********:

# basically! our interface should not enforce implementing a method that a derived class does not need! 

# Wrong!

public interface IShape {
  double Area();
  double Volume();
}

# and this works for 3d shapes but 2d shapes don't have volume!!

# Correct:

# so you segregate interface into to separate ones.
public interface IShape3D {
  double Area();
  double Volume();
}

public interface IShape2D {
  double Area();
}

#******** End Interface Segregation ********:

#******** Dependency Inversion ********:

Dependency Inversion is a design principle that suggests high-level modules should not depend on low-level modules; both should depend on abstractions. This reduces coupling by letting abstractions (interfaces) define the interactions, making systems more flexible and maintainable.

# Car is the hight level module and Engine Low-level
  # Car has Engine and other things

# wrong!!

public class Engine {
  public void Start() {
    Console.WriteLine("car started!);
  }

}

public class Car {

  private Engine _engine;

  public Car() {
    // Car is coupled with Engine, imagine if we want to use a different engine for our cart =: we have to change Car class !! NOT GOOD!!
    this._engine = new Engine(); 
  }

  public void StartCar() {
  
    engine.Start();
    Console.WriteLine("Cart started!");
  }
}

# Correct ways is to use Dependency-Injection:
==

public interface IEngine {
  public void Start() {
    Console.WriteLine("Engine started!");
  }
}

public class Engine : IEngine {
  public void Start() {
    Console.WriteLine("car started!);
  }
}

public class Car {

  private IEngine _engine;

  // Dependency injection =: now we can pass any Engine to our Car
  public Car(IEngine engine) {
    this._engine = engine; 
  }

  public void StartCar() {
    engine.Start();
    Console.WriteLine("Cart started!");
  }
}

#******** End Dependency Inversion ********:

-->