//Фильтрация данных

![Consol](./img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20(1).png)
![Consol2](./img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20(2).png)

//Поиск,сортировка

using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;
using WpfApp1.Model;

namespace WpfApp1
{
    /// <summary>
    /// Логика взаимодействия для MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window, INotifyPropertyChanged
       
    {
        public List<StudentAge> StudentAgeList { get; set; }
        public StudentAge SelectedAge = null;
        private IEnumerable<Student> _StudentList = null;

        public event PropertyChangedEventHandler PropertyChanged;

        public IEnumerable<Student> StudentList {
            get {
                var res = _StudentList; 
                res=res.Where(c =>(c.Age>=SelectedAge.AgeFrom && c.Age < SelectedAge.AgeTo));

                if (SearchFilter != "")
                    res = res.Where(c => (c.Name.IndexOf(SearchFilter, StringComparison.OrdinalIgnoreCase) >= 0));
               
                if (SortAsc) res = res.OrderBy(c => c.Age);
                else res = res.OrderByDescending(c => c.Age);
    return res;
            }
            set {
                _StudentList = value;
            }
        }
        private void Invalidate()
        {
            if (PropertyChanged != null)
                PropertyChanged(this, new PropertyChangedEventArgs("StudentList"));
        }
        public MainWindow()
        {
            InitializeComponent();
            DataContext = this;
            Globals.dataProvider = new LocalDataProvider();
            StudentList = Globals.dataProvider.GetStudents();
            StudentAgeList = Globals.dataProvider.GetStudentAges().ToList();

        }
        private void ExitButton_Click(object sender, RoutedEventArgs e)
        {
            Application.Current.Shutdown();
        }

        private void AgeFilterComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
        {
            SelectedAge = AgeFilterComboBox.SelectedItem as StudentAge;
            Invalidate();
        }
        
        private string SearchFilter = "";

        private void SearchFilter_KeyUp(object sender, KeyEventArgs e)
        {
            SearchFilter = SearchFilterTextBox.Text;
            Invalidate();
        }
        private bool SortAsc = true;

        private void RadioButton_Checked(object sender, RoutedEventArgs e)
        {
            SortAsc = (sender as RadioButton).Tag.ToString() == "1";
            Invalidate();
        }
    }
}




using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace WpfApp1.Model
{
    class Globals
    {
        public static IDataProvider dataProvider;
    }
    interface IDataProvider
    {
        IEnumerable<Student> GetStudents();
        IEnumerable<StudentAge> GetStudentAges();
    }
    
    public class Student
    {
 
        public string Surname { get; set; }
        public string Name { get; set; }
        public string Patronymic { get; set; }
        public DateTime BirthDay { get; set; }
        public string Adress { get; set; }
        public double Telephone { get; set; }
        public string Faculty { get; set; }
        public int Course { get; set; }
        public int Age { get; set; }
        public Boolean WithDisabilities;
    }

    public class StudentAge
    {
        public string Title { get; set; }
        public int AgeFrom { get; set; }
        public int AgeTo { get; set; }
    }

    public class LocalDataProvider : IDataProvider
    {
        IEnumerable<Student> IDataProvider.GetStudents()
        {
            return new Student[]
                 {
                new Student
                {
                    Surname ="Иванов",
                    Name="Иван",
                    Patronymic="Иванович",
                    Age = 17,
                    BirthDay=new DateTime(2005,3,1),
                    WithDisabilities=false,
                    Adress="г.Санкт-Петербург Невский проспект д.1, кв.1",
                    Telephone= 89778567855,
                    Faculty="Специалист по информационным системам и программированию" ,
                    Course= 2},

                new Student
                {
                Surname = "Сидоров",
                Name = "Максим",
                Patronymic = "Александрович",
                Age = 18,
                BirthDay = new DateTime(2003, 5, 11),
                WithDisabilities = false,
                Adress = "г.Санкт-Петербург Ленинский проспект д.5, кв.257",
                Telephone = 89555674590,
                Faculty = "Специалист по информационным системам и проограммированию",
                Course = 3
            },

                new Student
                {
                Surname = "Пешков",
                Name = "Александр",
                Patronymic = "Максимович",
                Age = 20,
                BirthDay = new DateTime(2001, 10, 24),
                WithDisabilities = false,
                Adress = "г.Санкт-Петербург Озерковский переулок д.22, кв.122",
                Telephone = 89337568848,
                Faculty = "Монументально-декоративное искусство",
                Course = 4
            },

                new Student
                {
                Surname = "Романов",
                Name = "Пётр",
                Patronymic = "Алексеевич",
                Age = 21,
                BirthDay = new DateTime(2003, 5, 30),
                WithDisabilities = false,
                Adress = "г.Санкт-Петербург Верхний переулок д.101, кв.24",
                Telephone = 89250458999,
                Faculty = "Монументально-декоративное искусство",
                Course = 4
            }
            };
        }

        IEnumerable<StudentAge> IDataProvider.GetStudentAges()
        {
            return new StudentAge[]{
                new StudentAge
                 { Title="Все студенты", AgeFrom=0, AgeTo=30 },
                 new StudentAge
                 {Title="Несовершенолетние", AgeFrom=0, AgeTo=18 },
                 new StudentAge
                 { Title="Совершенолетние", AgeFrom=18, AgeTo=30 },
                 new StudentAge
                 { Title="Выпускники", AgeFrom=20, AgeTo=30 }
                
             };
        }

    }
}

![Consol3](./img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20(3).png)
![Consol4](./img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20(4).png)
![Consol5](./img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20(5).png )
![Consol6](./img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20(6).png)
![Consol7](./img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20(7).png)
