<!-- 

# Design patterns:

  # Creational: different ways to create objects
  # structural: relation between objects
  # behavioral: interaction or communication between objects

# Behavioral:
==

# Momento pattern:
==

# undo functionality

The Memento Pattern allows an object to save its state and restore it later without exposing its internal details. It involves three main parts: the Originator, which creates and restores its own states; the Memento, which holds the state; and the Caretaker, which manages saved states for undo/redo. This pattern is particularly useful for implementing features like undo in applications. By storing previous states, the pattern enables restoring objects to their previous conditions seamlessly.

# Momento =: State
# Originator =: Editor 
# CareTaker =: History
  # takes care of a History stack, with State objects

// Momento
public class State {
  private readonly string _content;
  
  // state metadata
  # date that state was stored
  private readonly DateTime _stateCreated;

  public Editor(string content) {
    _content = content;
    _stateCreatedAt = DateTime.Now;
  }

  // getter content
  public string GetContent() => _content;

  // get date
  public DateTime GetDate() => _stateCreatedAt;

  // get info
  public string GetInfo() => $"{_stateCreatedAt} : {_content}";
}

# CareTaker
public class History {
  private Stack<State> _states = new List<State>();
  private Editor _editor = new Editor();

  // inject an editor
  public History(Editor editor) {
    _editor = editor;
  }

  // push 
  public void Save() {
    # save a new state
    _states.Push(_editor.SaveState()); 
  }

  // pop
  public void Undo() {
    if (_states.Count == 0) return;
    State prevState = states.Pop();

    # Editor restore
    _editor.Restore(prevState);
  }

  // list of history
  public void ShowHistory() {
  
    Console.WriteLine("the list of momentos: ");
    foreach (var _state in _states) {
      Console.WriteLine(state.GetName());
    }
  }
}

// Originator
public class Editor {
  public string Content {get; set;}

  // save state
  public State SaveState() {
    return new State(this.Content);
  }

  // restore state
  public void Restore(State state) {
    this.Content = state.GetContent();
  }
}

var editor = new Editor();
var history = new History(editor);
editor.Content = "first1";
history.Save();

history.Undo();

# ***************** End Momento pattern

# State pattern



# ***************** End State pattern

-->