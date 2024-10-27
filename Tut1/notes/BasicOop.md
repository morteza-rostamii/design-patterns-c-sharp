<!--  

# Learning design patterns C#:
==

# Encapsulation
  # basically: make class data private and either use it only through the methods inside the class or create getter & setter

# Abstraction:
  # hide implementation of a certain functionality inside the class.

# Inheritance
  # is a relation => Bear is an Animal
  # a Circle is a Shape

# Polymorphism *******************************

# Polymorphism
  # same name method is override in different sub-classes

public class Animal {
  private string Name;
  public virtual void Eats() {
    Console.WriteLine("${Name} eats!"); // animal eats
  }
}

public class Bear: Animal {
  public override void Eats() {
    Console.writeLine($"{Name} eats!"); // bear eats!
  }
}

// now we can have a list of animals that all have .Eats()

List<Animals> animals = new List<Animals>();

animals.Add(new Bear {Name = "beary!"});
animals.Add(new Rat {Name = "Ratty!"});

foreach (var animal in animals) {
  animal.Eats();
}

#======================================= End polymorphism

#=== Start: Coupling *******************

# tight coupling vs tight coupling
  # degree of dependency between classes

public interface IMessageService {
  void SendMessage(string message);
}

public class SmsSender : IMessageService {
  public void SendMessage(string message) {
    Console.WriteLine($"SMS: {message}");
  }
}

public class EmailSender : IMessageService {
  public SendMessage(string message) {
    Console.WriteLine(message);
  }
}

// Order wants to send sms or email
// this way we never have to change Order class! in case we wanted to change the messageService to sendEmail() or sendSms()
** hence (decoupling)
public class Order {
  private readonly IMessageService _messageService;

  // we can send any class(object) that implements IMessageService
  public Order(IMessageService messageService) {
    this._messageService = messageService;
  }

  // place order
  public void PlaceOrder() {
    // order logic

    // could be email or sms
    this._messageService.sendMessage();
  }
}

// Main
Order order = new Order(new SmsSender());
order.PlaceOrder();

or
Order order = new Order(new EmailSender());
order.PlaceOrder(); // this one sends an email

//# how to have the options of multiple message on one Order and still keep it de-coupled !!

public Order {

  // a list of objects that all implement =: IMessageService
  private readonly List<IMessageService> _messageServices;

  public Order(List<IMessageService> messageService) {
    this._messageServices = messageServices;
  }

  public void PlaceOrder() {
    // order logic

    foreach (var messageService in _messageServices) {
      messageService.SendMessage();
    }
  }
}

// Main

var messageServices = new List<IMessageService> {
  new SmsSender();
  new EmailSender();
};

Order order = new Order(messageServices);
order.PlaceOrder();

#============================ End Coupling

#============= composition **************

# object-a has a object-b =: each object has it's own state and functionality.

public class Weapon {

  private string Name;

  public class Weapon() {}

  public void Shoot() {
    Console.WriteLine($"{this.Name} is shooting!!PO!PO!");
  }
}

public class Movement {

  public void Run(Hero hero) {
    Console.WriteLine($"{hero.Name} Runs and ...");
  }
  public void Walk(Hero hero) {} 
}

public class Hero {
  private string Name;

  public Hero(string name) {
    this.Name = name;
  }

  // the problem with creating an object of Weapon here, is we might want to use a different sub-class of Weapon later!!
  private Weapon weapon = new Weapon();
  private Movement movement = new Movement();

  public RunAndShoot() {
    movement.Run(this);
    weapon.Shoot();
  }
}

#== use Dependency-injection and interfaces =: to improve the above.

// the reason we create interfaces: is that later we can create a different class that implements the same interface with some additional stuff , without braking the code! cause our code uses the interface as the type all over the place!

public interface IWeapon {
  void Shoot();
}

public interface IMovement {
  void Run(Hero hero);
  void Walk(Hero hero);
}

// implementation
public class Weapon : IWeapon {
  private string Name;

  public Weapon(string Name) {
    this.Name = name;
  }

  public void Shoot() {
    Console.Write($"{this.Name} is shooting!");
  }
}

public class Movement : IMovement {
  public void Run(Hero hero) {
    Console.WriteLine(hero.Name);
  }

  public void Walk(Hero hero) {
    Console.WriteLine(hero.Name);
  }
}

public class Hero {
  // now we don't have to change the Hero class in case i want to pass any class object that implement IWeapon in _weapon
  private readonly IWeapon _weapon;
  private readonly IMovement _movement;

  public string Name {get; set;}

  // Dependency injection
  // we can pass any object that is going to implement IWeapon
  public Hero(string name, IWeapon weapon, IMovement movement) {
    this.Name = name;
    this._weapon = weapon;
    this._movement = movement;
  }

  public void RunAndShoot() {
    _movement.Run(this);
    _weapon.Shoot();
  }
}

// Main
var hero = new Hero(
  "Jack",
  
  // dependency injection
  new Weapon(),
  new Movement()
);

#============= End composition **************

#============= problems with inheritance **************
# problem with inheritance =: Fragile Base class Problem
==

#============= End problems with inheritance **************

#============= UML **************

# 

#============= End UML **************
-->