
## Reagiere auf einen bestimmte Taste inerhalb einer Textbox.

Mit einem KeyBinding innerhalb der Text Box ist das möglich.
In diesem Fall Key="Return" -> Enter
Command Binding -> in ViewModel ein Command erstellen -> Im Parameter wird der Text mitübergeben -> muss in eine String gecastet werden.

Soll z.B. nach der Verarbeitung der TextBox Inhalt gelöscht werden, kann ein Property auf die TextBox gebunden werden, der auf ein PropertyChanged reagiert. 
`Text="{Binding [Property from ViewModel], UpdateSourceTriger=PropertyChanged}"`


```XAML

<TextBox x:Name="Insert" Width="600" AcceptsReturn="False" Text="{Binding Input, UpdateSourceTrigger=PropertyChanged}">
                <TextBox.InputBindings>
                    <KeyBinding
                        Key="Return"
                        Command="{Binding InputNewItem}"
                        CommandParameter="{Binding Path=Text, ElementName=Insert}"/>                       
                </TextBox.InputBindings>
            </TextBox>
            
```

ViewModel
SetProperty siehe [[WPF - MVVM CSharp#SetProperty]]

```C#

// Field -> TextBox Inhalt
private string _input;

// Property -> TextBox Inhalt
public string Input
{ 
    get { return _input; }
    set { SetProperty<string>(ref _input, value); }
}

// Teil des Command Aufrufs
public ICommand InputNewItem { get; }

void OnInputExcecuted(object parameter)
{
	// Do Smething -> Inhalt TextBox ist im parameter

    // leeren TextBox hier 
    Input = "";
}

bool OnInputCanExcecuted(object parameter)
{
    return true;           
}
```