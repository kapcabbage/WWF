# Instrukcja do cwiczeń

# Wstęp

Do rozwiązania ćwiczeń używamy Visual Studio wraz z C# 

# Zadania
**1. Stwóż nowy projekt w Visual Studio -> File -> New Project, będzie to projekt Workflow Console Application**
- W pliku Program.cs dodaj poniższy kod do metody `Main`
```cs
 var seq = new Sequence
            {
                Activities =
                {
                    new WriteLine {Text = "Fajny" },
                    new WriteLine {Text = "Workflow" }
                }
            };
```
- Spróbuj wykonać powyższy przepływ za pomocą WorkflowInvoker
```cs
  var invoker = new WorkflowInvoker(seq);
  invoker.Invoke();
```

- Teraz spróbuj wykoniać ten sam przepływ ale za pomocą WorkflowApplication - czy rezultat jest taki sam?
```cs
   var app = new WorkflowApplication(seq);          
   app.Run();
```
**2. Dodaj do projektu nową aktywność (Activity) o nazwie Calculator**
- dodaj do niej nową sekwencje (Sequence)  
- zdefiniuj zmienne (Variables) - `_operand1` oraz `_operand2`
- zdefiniuj argumenty (Arguments) - `Operand1`, `Operand2` oraz `Result`
- dodaj do sekwencji 3 nowe aktywności - `Assign`, a następnie przypisz:
```cs
  _operand1 = Operand1
  _operand2 = Operand2
  Result = _operand1 + _operand2
```
- w metodzie `Main` dodaj poniższy kod i spróbuj wyświetlić wynik:
```cs
  var workflow = new Calculator();
  var invoker = new WorkflowInvoker(workflow);
  var input = new Dictionary<string,object>();
  input.add("Operand1",<twój operand>);
  input.add("Operand2",<twój operand>);
  var result = invoker.Invoke(input);
```
- Spróbuj dorobić do tego przepływu obsługę odejmowania, mnożenia oraz dzielenia. 
> Wskazówka: wykorzystaj do tego aktywność `FlowChart` oraz `FlowSwitch`. Pamiętaj, że nie można dzielić przez 0, aby tego uniknąć wykorzystaj aktywność `TryCatch` lub `FlowDecision`

**3. Dodaj nowy projekt Workflow Service Application do solucji**
- w utworzonym projekcie dodaj klase `Person` oraz `PersonList` 
```cs
 public class Peson{
    public string Name {get;set;}
    public string Surname {get;set;}
 }
 public class PesonList{
    public List<Person> List {get;set;}
    
    public PersonList(){
        <zainicjalizuj liste paroma osobami>
    }
 }
```
- dodaj do projektu nowy WCF Workflow Service 
- zdefiniuj dla tego serwisu zmienne `name`, `surname`, `person`, `list`
- zmień nazwę operacji na `GetPerson`
- zdefiniuj dla aktywności `Receive` parametry `Name` oraz `Surname`
- danymi wejściowymi będą imię oraz nazwisko, przypisz odpowiednio te dane do zmiennych w aktywności `Receive`
- dodaj aktywność `Assign`, która będzie wywoływała konstrukor klasy `PersonList` oraz przypisywała tą listę do zmiennej `list`
- dodaj do projektu nowy Code Activity
- dopisz kod, mający zwracać osobę o zadanym nazwisku oraz imieniu z listy osób
> Wskazówka: `InArgument` to argumenty wejściowe, a `OutArgument` wyjściowe dla tej aktywności. Aby obecna aktywność miała dostęp do wcześniej zdefiniowanych zmiennych należy pożenić je z poziomu Designera, a w metodzie Execute wyłuskać te dane z context'u tej konkretnej aktywności. Aby aktywność była widoczna w Designer, należy wpierw skompilować projekt.
- dodaj napisaną aktywność do sekwencji 
- aktywność `SendReply` będzie zwracała parametr do którego jest przypisana zmienna `person`
- w projekcie Workflow Console Application dodaj usługe (Add Service Reference -> Discovery)
- wywołaj przepływ usługi analogicznie do przepływu Calculator
