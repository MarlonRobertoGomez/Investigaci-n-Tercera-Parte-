Chain Of Responsability
abstract class Handler
{
    protected Handler? NextHandler;
    public void SetNext(Handler next) => NextHandler = next;
    public abstract void HandleRequest(string request);
}

class ConcreteHandlerA : Handler
{
    public override void HandleRequest(string request)
    {
        if (request == "A") Console.WriteLine("Handled by A");
        else NextHandler?.HandleRequest(request);
    }
}

class ConcreteHandlerB : Handler
{
    public override void HandleRequest(string request)
    {
        if (request == "B") Console.WriteLine("Handled by B");
        else NextHandler?.HandleRequest(request);
    }
}

// Uso
var handlerA = new ConcreteHandlerA();
var handlerB = new ConcreteHandlerB();
handlerA.SetNext(handlerB);
handlerA.HandleRequest("B"); // Output: Handled by B

Command
interface ICommand { void Execute(); }

class Light
{
    public void TurnOn() => Console.WriteLine("Luz encendida");
    public void TurnOff() => Console.WriteLine("Luz apagada");
}

class TurnOnCommand : ICommand
{
    private readonly Light _light;
    public TurnOnCommand(Light light) => _light = light;
    public void Execute() => _light.TurnOn();
}

class RemoteControl
{
    private ICommand? _command;
    public void SetCommand(ICommand command) => _command = command;
    public void PressButton() => _command?.Execute();
}

// Uso
var light = new Light();
var command = new TurnOnCommand(light);
var remote = new RemoteControl();
remote.SetCommand(command);
remote.PressButton(); // Output: Luz encendida

Interpretere
interface IExpression { int Interpret(); }

class NumberExpression : IExpression
{
    private readonly int _number;
    public NumberExpression(int number) => _number = number;
    public int Interpret() => _number;
}

class AddExpression : IExpression
{
    private readonly IExpression _left, _right;
    public AddExpression(IExpression left, IExpression right)
    {
        _left = left; _right = right;
    }
    public int Interpret() => _left.Interpret() + _right.Interpret();
}

// Uso
IExpression expression = new AddExpression(new NumberExpression(5), new NumberExpression(3));
Console.WriteLine(expression.Interpret()); // Output: 8

Iterator
class Iterator<T>
{
    private readonly List<T> _items;
    private int _position = 0;

    public Iterator(List<T> items) => _items = items;
    public T? Next() => _position < _items.Count ? _items[_position++] : default;
    public bool HasNext() => _position < _items.Count;
}

// Uso
var items = new List<string> { "A", "B", "C" };
var iterator = new Iterator<string>(items);
while (iterator.HasNext()) Console.WriteLine(iterator.Next()); // Output: A B C

Mediator
class Mediator
{
    public void SendMessage(string message, Colleague colleague)
    {
        Console.WriteLine($"{colleague.Name} recibe: {message}");
    }
}

class Colleague
{
    private readonly Mediator _mediator;
    public string Name { get; }
    public Colleague(string name, Mediator mediator)
    {
        Name = name;
        _mediator = mediator;
    }
    public void Send(string message) => _mediator.SendMessage(message, this);
}

// Uso
var mediator = new Mediator();
var c1 = new Colleague("A", mediator);
var c2 = new Colleague("B", mediator);
c1.Send("Hola B");
c2.Send("Hola A");

Memento
class Memento
{
    public string State { get; }
    public Memento(string state) => State = state;
}

class Originator
{
    public string State { get; set; } = "";
    public Memento Save() => new(State);
    public void Restore(Memento memento) => State = memento.State;
}

// Uso
var originator = new Originator { State = "Estado1" };
var memento = originator.Save();
originator.State = "Estado2";
originator.Restore(memento);
Console.WriteLine(originator.State); // Output: Estado1

Observer
class Subject
{
    private readonly List<Action<string>> _observers = new();
    public void Attach(Action<string> observer) => _observers.Add(observer);
    public void Notify(string message) => _observers.ForEach(o => o(message));
}

// Uso
var subject = new Subject();
subject.Attach(msg => Console.WriteLine("Observador 1: " + msg));
subject.Attach(msg => Console.WriteLine("Observador 2: " + msg));
subject.Notify("Cambio detectado");

State
interface IState { void Handle(); }

class ConcreteStateA : IState { public void Handle() => Console.WriteLine("Estado A"); }
class ConcreteStateB : IState { public void Handle() => Console.WriteLine("Estado B"); }

class Context
{
    private IState _state;
    public Context(IState state) => _state = state;
    public void SetState(IState state) => _state = state;
    public void Request() => _state.Handle();
}

// Uso
var context = new Context(new ConcreteStateA());
context.Request();
context.SetState(new ConcreteStateB());
context.Request();

Strategy
interface IStrategy { void Execute(); }
class ConcreteStrategyA : IStrategy { public void Execute() => Console.WriteLine("Estrategia A"); }
class Context { private IStrategy _strategy; public Context(IStrategy strategy) => _strategy = strategy; public void Execute() => _strategy.Execute(); }

// Uso
var context = new Context(new ConcreteStrategyA());
context.Execute();

Template Method
abstract class Template { public void Execute() { Step1(); Step2(); } protected abstract void Step1(); protected abstract void Step2(); }
class ConcreteClass : Template { protected override void Step1() => Console.WriteLine("Paso 1"); protected override void Step2() => Console.WriteLine("Paso 2"); }

// Uso
new ConcreteClass().Execute();

