
# ViewModelBase Klasse

## SetProperty

```C#
public class ViewModelBase : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;
        
        protected void SetProperty<T>(ref T storage, T value, [CallerMemberName] string property = null)
        {
            if (Object.Equals(storage, value)) return;
            storage = value;
            if (PropertyChanged != null)
            {
                PropertyChanged(this, new PropertyChangedEventArgs(property));
            }
        }       
    }
```


## Command

```C#
 public class ActionCommand : ICommand
    {
        private readonly Action<object> _execute;
        private readonly Func<object, bool> _canExecute;
        public ActionCommand(Action<object> execute,
        Func<object, bool> canExecute)
        {
            _execute = execute ?? throw new ArgumentNullException(nameof(execute));
            _canExecute = canExecute;
        }
        public event EventHandler CanExecuteChanged
        {
            add { CommandManager.RequerySuggested += value; }
            remove { CommandManager.RequerySuggested -= value; }
        }
        public void Execute(object parameter)
        {
            _execute(parameter);
        }
        public bool CanExecute(object parameter)
        {
            if (_canExecute == null)
                return true;
            return _canExecute(parameter);
        }
    }
```

## MainViewModel

```C#
 public class MainViewModel : ViewModelBase
    {
        private string _besipiel1;

		private int _beispiel2;

	    private DateTime _besipiel3;

        public MainViewModel()
        {
            Items = new ObservableCollection<Components>();
          
            InputNewItem = new ActionCommand(OnInputExcecuted, OnInputCanExcecuted);
          
        }

		// Observable Collection -> List of Class Components 
        public ObservableCollection<Components> Items { get; }

		// Commands
		void OnInputExcecuted(object parameter)
        {
            // DO Something
        }
        
		bool OnInputCanExcecuted(object parameter)
        {
            return true;           
        }
        
		public ICommand InputNewItem { get; }

		// Properties
        public string Besipiel1
        { 
            get { return _besipiel1; }
            set { SetProperty<string>(ref _besipiel1, value); }
        }

		public int Besipiel2
        { 
            get { return _besipiel2; }
            set { SetProperty<int>(ref _besipiel2, value); }
        }
        
		public DateTime Besipiel3
        { 
            get { return _besipiel3; }
            set { SetProperty<DateTime>(ref _besipiel3, value); }
        }               
    }

	// Class Components -> Every Property raises INotify Property Changed
	public class Components : ViewModelBase
    {
        private int _quantity;
                       
        public int Quantity
        {
            get { return _quantity; }
            set { SetProperty<int>(ref _quantity, value); }
        }

        private string _item;

        public string Item
        {
            get { return _item; }
            set { SetProperty<string>(ref _item, value); }
        }
    }    
```

## Call MainViewModel

App.xaml
Clear Startup 

```XAML
<Application x:Class="InventoryHelper.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:InventoryHelper">
             
// No Startup here !!!

    <Application.Resources>
         
    </Application.Resources>
</Application>
```

App.xaml.cs
Call MainWindow CTOR with Parameter mainViewModel

```C#
public partial class App : Application
    {
        protected override void OnStartup(StartupEventArgs e)
        {
            base.OnStartup(e);
            var mainViewModel = new MainViewModel();
            MainWindow = new MainWindow(mainViewModel);
            MainWindow.Show();
        }
    }
```

MainWindow.xaml.cs
Add an Input Parameter -> from Class MainViewModel

```C#
public partial class MainWindow : Window
    {
       
        public MainWindow(MainViewModel mainViewModel)
        {
            InitializeComponent();
            // Set DataContext -> MainViewModel
            DataContext = mainViewModel;           
        }
    }
```