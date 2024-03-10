# «Разработка веб-приложения для обнаружения и диагностики заболеваний легких»
## Порождающие шаблоны
### ***Factory***
``` Java
// Интерфейс для создания объектов
interface DiagnosticTool {
    void diagnose();
}

// Конкретная реализация диагностического инструмента
class LungDiagnosticTool implements DiagnosticTool {
    @Override
    public void diagnose() {
        System.out.println("Диагностика легких");
    }
}

// Создание объектов
class DiagnosticToolFactory {
    public DiagnosticTool createDiagnosticTool() {
        return new LungDiagnosticTool();
    }
}

// Пример использования фабрики
public class Main {
    public static void main(String[] args) {
        DiagnosticToolFactory factory = new DiagnosticToolFactory();
        DiagnosticTool diagnosticTool = factory.createDiagnosticTool();
        diagnosticTool.diagnose();
    }
}
```
В данном примере создается DiagnosticToolFactory, которая создает объекты типа LungDiagnosticTool через метод createDiagnosticTool(). При вызове этого метода, будет создан объект LungDiagnosticTool, который в свою очередь реализует метод diagnose() для диагностики легких.

Uml-диаграмма:
``` sql
-----------------------------------------
|              DiagnosticTool            |
-----------------------------------------
| + diagnose() : void                    |
-----------------------------------------

                 ^
                 |
                 |
-----------------------------------------
|        LungDiagnosticTool             |
-----------------------------------------
| + diagnose() : void                    |
-----------------------------------------

-----------------------------------------
|        DiagnosticToolFactory          |
-----------------------------------------
| + createDiagnosticTool() : DiagnosticTool |
-----------------------------------------

```
### ***Builder***
``` Java
// Класс, который представляет объект "Пациент"
class Patient {
    private String name;
    private int age;
    private String symptoms;

    public Patient(String name, int age, String symptoms) {
        this.name = name;
        this.age = age;
        this.symptoms = symptoms;
    }

    // Геттеры для получения данных о пациенте
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getSymptoms() {
        return symptoms;
    }

    @Override
    public String toString() {
        return "Patient{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", symptoms='" + symptoms + '\'' +
                '}';
    }
}

// Класс Builder для пошагового создания объекта Patient
class PatientBuilder {
    private String name;
    private int age;
    private String symptoms;

    public PatientBuilder setName(String name) {
        this.name = name;
        return this;
    }

    public PatientBuilder setAge(int age) {
        this.age = age;
        return this;
    }

    public PatientBuilder setSymptoms(String symptoms) {
        this.symptoms = symptoms;
        return this;
    }

    public Patient build() {
        return new Patient(name, age, symptoms);
    }
}

// Использование Builder
public class Main {
    public static void main(String[] args) {
        Patient patient = new PatientBuilder()
                .setName("Иванов Иван")
                .setAge(35)
                .setSymptoms("Кашель, затрудненное дыхание")
                .build();

        System.out.println(patient);
    }
}
```
В данном примере создается класс Patient, представляющий пациента с полями name, age и symptoms. Затем создается класс PatientBuilder, который позволяет пошагово задавать значения для этих полей и создавать объект Patient с помощью метода build(). В методе main иллюстрируется использование Builder для создания объекта Patient.

Uml-диаграмма:
``` sql
-----------------------------------------
|                Patient                |
-----------------------------------------
| - name: String                        |
| - age: int                            |
| - symptoms: String                    |
-----------------------------------------
| + getName() : String                  |
| + getAge() : int                      |
| + getSymptoms() : String              |
-----------------------------------------

                 ^
                 |
                 |
-----------------------------------------
|             PatientBuilder            |
-----------------------------------------
| - name: String                        |
| - age: int                            |
| - symptoms: String                    |
-----------------------------------------
| + setName(name: String) : void        |
| + setAge(age: int) : void             |
| + setSymptoms(symptoms: String) : void|
| + build() : Patient                   |
-----------------------------------------
```

### ***Prototype***
``` Java
// Интерфейс для клонирования объектов
interface Prototype {
    Prototype clone();
}

// Класс, представляющий данные о пациенте
class Patient implements Prototype {
    private String name;
    private int age;
    private String symptoms;

    public Patient(String name, int age, String symptoms) {
        this.name = name;
        this.age = age;
        this.symptoms = symptoms;
    }

    // Геттеры для получения данных о пациенте

    @Override
    public Prototype clone() {
        return new Patient(this.name, this.age, this.symptoms);
    }

    @Override
    public String toString() {
        return "Patient{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", symptoms='" + symptoms + '\'' +
                '}';
    }
}

// Использование шаблона Prototype
public class Main {
    public static void main(String[] args) {
        Patient originalPatient = new Patient("Иванов Иван", 35, "Кашель, затрудненное дыхание");
        
        // Создаем копию пациента с помощью клонирования
        Patient clonedPatient = (Patient) originalPatient.clone();

        System.out.println("Оригинальный пациент: " + originalPatient);
        System.out.println("Клонированный пациент: " + clonedPatient);
    }
}
```
В данном примере создается интерфейс Prototype, который содержит метод clone() для клонирования объектов. Затем создается класс Patient, реализующий интерфейс Prototype, и имеющий метод clone(), который создает копию объекта Patient.

В методе main создается оригинальный объект originalPatient, после чего создается его копия clonedPatient с помощью метода clone(). После этого выводятся данные об оригинальном и клонированном пациентах.

Uml-диаграмма:
``` sql
-----------------------------------------
|               Prototype               |
-----------------------------------------
| + clone() : Prototype                 |
-----------------------------------------
                   ^
                   |
                   |
-----------------------------------------
|               Patient                 |
-----------------------------------------
| - name: String                        |
| - age: int                            |
| - symptoms: String                    |
-----------------------------------------
| + getName() : String                  |
| + getAge() : int                      |
| + getSymptoms() : String              |
| + clone() : Prototype                 |
-----------------------------------------
```

### ***Singleton***
``` Java
// Класс Singleton для управления базой данных пациентов
public class PatientDatabase {
    private static PatientDatabase instance;
    
    // Приватный конструктор, чтобы предотвратить создание экземпляров класса извне
    private PatientDatabase() {
        // Инициализация базы данных пациентов
    }
    
    // Глобальная точка доступа к единственному экземпляру класса
    public static PatientDatabase getInstance() {
        if (instance == null) {
            instance = new PatientDatabase();
        }
        return instance;
    }
    
    // Другие методы для работы с базой данных пациентов
}

// Пример использования шаблона Singleton
public class Main {
    public static void main(String[] args) {
        // Получаем единственный экземпляр базы данных пациентов
        PatientDatabase database = PatientDatabase.getInstance();
        
        // Попытка создать новый экземпляр класса будет вызывать ошибку
        // PatientDatabase anotherDatabase = new PatientDatabase(); // Ошибка
        
        // Используем базу данных для обработки данных о пациентах
        // ...
    }
}
```
В данном примере класс PatientDatabase реализует шаблон Singleton. У класса есть статическое поле instance, которое хранит единственный экземпляр класса. Конструктор класса приватный, чтобы предотвратить создание объектов извне, а метод getInstance() возвращает этот единственный экземпляр класса.

В методе main создается объект database с помощью метода getInstance(), и попытка создать новый экземпляр класса вызовет ошибку. Таким образом, с помощью шаблона Singleton можно утверждать, что в приложении будет только один экземпляр базы данных пациентов.

Uml-диаграмма:
``` sql
-----------------------------------------
|           PatientDatabase             |
-----------------------------------------
| - instance: PatientDatabase           |
-----------------------------------------
| + getInstance() : PatientDatabase     |
-----------------------------------------
                   ^
                   |
                   |
-----------------------------------------
|              Singleton                |
-----------------------------------------
|                                      |
-----------------------------------------
```

## Структурные шаблоны
### ***Decorator***
``` Java
// Интерфейс, представляющий компонент (основной объект)
interface LungDiagnostic {
    void diagnose();
}

// Конкретный компонент, представляющий базовую диагностику легких
class BasicLungDiagnostic implements LungDiagnostic {
    @Override
    public void diagnose() {
        System.out.println("Выполняется базовая диагностика легких...");
    }
}

// Декоратор, добавляющий функциональность к компоненту
abstract class LungDiagnosticDecorator implements LungDiagnostic {
    protected LungDiagnostic lungDiagnostic;

    public LungDiagnosticDecorator(LungDiagnostic lungDiagnostic) {
        this.lungDiagnostic = lungDiagnostic;
    }

    public void diagnose() {
        lungDiagnostic.diagnose();
    }
}

// Конкретные декораторы, добавляющие дополнительную функциональность
class XRayDecorator extends LungDiagnosticDecorator {
    public XRayDecorator(LungDiagnostic lungDiagnostic) {
        super(lungDiagnostic);
    }

    @Override
    public void diagnose() {
        super.diagnose();
        System.out.println("Добавлена рентгенография легких.");
    }
}

class BloodTestDecorator extends LungDiagnosticDecorator {
    public BloodTestDecorator(LungDiagnostic lungDiagnostic) {
        super(lungDiagnostic);
    }

    @Override
    public void diagnose() {
        super.diagnose();
        System.out.println("Добавлен анализ крови.");
    }
}

// Пример использования шаблона Decorator
public class Main {
    public static void main(String[] args) {
        // Создаем базовый компонент - базовую диагностику легких
        LungDiagnostic basicDiagnostic = new BasicLungDiagnostic();

        // Добавляем к нему декораторы - рентгенографию и анализ крови
        LungDiagnostic diagnosticWithXRay = new XRayDecorator(basicDiagnostic);
        LungDiagnostic diagnosticWithBloodTest = new BloodTestDecorator(diagnosticWithXRay);

        // Выполняем диагностику с добавленными декораторами
        diagnosticWithBloodTest.diagnose();
    }
}
```
В данном примере созданы интерфейс LungDiagnostic, представляющий основной компонент, и класс BasicLungDiagnostic, который реализует этот компонент. Затем создан абстрактный класс LungDiagnosticDecorator, который расширяет интерфейс LungDiagnostic и содержит ссылку на объект типа LungDiagnostic. Классы XRayDecorator и BloodTestDecorator являются конкретными декораторами, добавляющими функциональность рентгенографии и анализа крови соответственно.

В методе main создается базовый компонент basicDiagnostic, к которому последовательно добавляются декораторы. При вызове метода diagnose() у объекта diagnosticWithBloodTest будет выполнена базовая диагностика легких с добавленной рентгенографией и анализом крови.

Uml-диаграмма:
``` sql
-----------------------------------------
|           LungDiagnostic              |
-----------------------------------------
| + diagnose() : void                   |
-----------------------------------------
                    ^
                    |
                    |
-----------------------------------------
|         BasicLungDiagnostic           |
-----------------------------------------
|                                      |
-----------------------------------------

-----------------------------------------
|      LungDiagnosticDecorator         |
-----------------------------------------
| - component: LungDiagnostic           |
| + diagnose() : void                   |
-----------------------------------------
                    ^
                    |
        ------------------------
        |                      |
-------------------   -------------------
| XRayDecorator   |   | BloodTestDecorator|
-------------------   -------------------
| + diagnose() : void | | + diagnose() : void|
-------------------   -------------------
```

### ***Facade***
``` Java
// Подсистема для диагностики легких
class LungDiagnosticSubsystem {
    public void performXRayScan() {
        System.out.println("Выполняется рентгенография легких...");
    }

    public void performBloodTest() {
        System.out.println("Выполняется анализ крови...");
    }

    public void performSpirometryTest() {
        System.out.println("Выполняется спирометрия...");
    }
}

// Фасад, предоставляющий упрощенный интерфейс к подсистеме диагностики легких
class LungDiagnosticFacade {
    private LungDiagnosticSubsystem subsystem;

    public LungDiagnosticFacade() {
        this.subsystem = new LungDiagnosticSubsystem();
    }

    public void performFullDiagnostic() {
        subsystem.performXRayScan();
        subsystem.performBloodTest();
        subsystem.performSpirometryTest();
    }
}

// Пример использования шаблона Facade
public class Main {
    public static void main(String[] args) {
        LungDiagnosticFacade diagnosticFacade = new LungDiagnosticFacade();
        diagnosticFacade.performFullDiagnostic();
    }
}
```
В данном примере создана подсистема LungDiagnosticSubsystem, которая содержит методы для выполнения рентгенографии, анализа крови и спирометрии. Затем создан класс LungDiagnosticFacade, который предоставляет упрощенный интерфейс к этой подсистеме путем объединения вызовов методов подсистемы в один метод performFullDiagnostic().

В методе main создается объект LungDiagnosticFacade и вызывается метод performFullDiagnostic(), который в свою очередь выполняет все необходимые шаги для полной диагностики легких. Пользователь веб-приложения может использовать этот упрощенный интерфейс для запуска всей процедуры диагностики без необходимости знать о деталях каждого шага.

Uml-диаграмма:
``` sql
-----------------------------------------
|       LungDiagnosticSubsystem         |
-----------------------------------------
| + xRay() : void                       |
| + bloodTest() : void                  |
| + spirometry() : void                 |
-----------------------------------------

-----------------------------------------
|      LungDiagnosticFacade             |
-----------------------------------------
| - subsystem: LungDiagnosticSubsystem  |
-----------------------------------------
| + performFullDiagnostic() : void      |
-----------------------------------------
```

### ***Adapter***
``` Java
// Интерфейс для диагностики легких
interface LungDiagnostic {
    void performDiagnostic();
}

// Класс, реализующий диагностику легких через рентгенографию
class XRayDiagnostic implements LungDiagnostic {
    @Override
    public void performDiagnostic() {
        System.out.println("Выполняется диагностика легких через рентгенографию...");
    }
}

// Адаптер для выполнения диагностики легких через анализ крови
class BloodAnalysisAdapter implements LungDiagnostic {
    private BloodAnalyzer bloodAnalyzer;

    public BloodAnalysisAdapter(BloodAnalyzer bloodAnalyzer) {
        this.bloodAnalyzer = bloodAnalyzer;
    }

    @Override
    public void performDiagnostic() {
        bloodAnalyzer.performBloodAnalysis();
    }
}

// Класс, реализующий анализ крови
class BloodAnalyzer {
    public void performBloodAnalysis() {
        System.out.println("Выполняется анализ крови для диагностики легких...");
    }
}

// Пример использования шаблона Adapter
public class Main {
    public static void main(String[] args) {
        LungDiagnostic xRayDiagnostic = new XRayDiagnostic();
        xRayDiagnostic.performDiagnostic();

        BloodAnalyzer bloodAnalyzer = new BloodAnalyzer();
        LungDiagnostic bloodAnalysisAdapter = new BloodAnalysisAdapter(bloodAnalyzer);
        bloodAnalysisAdapter.performDiagnostic();
    }
}
```
В данном примере создан интерфейс LungDiagnostic, который определяет метод performDiagnostic() для выполнения диагностики легких. Затем создан класс XRayDiagnostic, реализующий этот интерфейс для выполнения диагностики через рентгенографию.

Для выполнения диагностики через анализ крови создан адаптер BloodAnalysisAdapter, который принимает объект BloodAnalyzer и вызывает его метод performBloodAnalysis() в своей реализации метода performDiagnostic().

В методе main создаются объекты XRayDiagnostic и BloodAnalyzer, а затем через адаптер BloodAnalysisAdapter вызывается метод performDiagnostic(), что позволяет выполнить диагностику легких через анализ крови с использованием уже существующего класса BloodAnalyzer.

Uml-диаграмма:
``` sql
-----------------------------------------
|             LungDiagnostic             |
-----------------------------------------
| + performDiagnostic() : void          |
-----------------------------------------
                   ^
                   |
-----------------------------------------
|           XRayDiagnostic              |
-----------------------------------------
| + performDiagnostic() : void          |
-----------------------------------------

-----------------------------------------
|          BloodAnalyzer               |
-----------------------------------------
| + performBloodAnalysis() : void      |
-----------------------------------------

-----------------------------------------
|     BloodAnalysisAdapter             |
-----------------------------------------
| - bloodAnalyzer: BloodAnalyzer       |
-----------------------------------------
| + performDiagnostic() : void         |
-----------------------------------------
```
### ***Proxy***
``` Java
// Интерфейс для диагностики легких
interface LungDiagnostic {
    void performDiagnostic();
}

// Реальный сервис для выполнения диагностики легких
class RealLungDiagnostic implements LungDiagnostic {
    @Override
    public void performDiagnostic() {
        System.out.println("Выполняется диагностика легких...");
    }
}

// Прокси-класс для контроля доступа к реальному сервису
class LungDiagnosticProxy implements LungDiagnostic {
    private RealLungDiagnostic realLungDiagnostic;

    @Override
    public void performDiagnostic() {
        if (realLungDiagnostic == null) {
            realLungDiagnostic = new RealLungDiagnostic();
        }
        // Дополнительная логика, например, проверка прав доступа
        System.out.println("Проверка прав доступа перед выполнением диагностики...");
        realLungDiagnostic.performDiagnostic();
    }
}

// Пример использования шаблона Proxy
public class Main {
    public static void main(String[] args) {
        LungDiagnosticProxy lungDiagnosticProxy = new LungDiagnosticProxy();
        lungDiagnosticProxy.performDiagnostic();
    }
}
```
В данном примере создан интерфейс LungDiagnostic, который определяет метод performDiagnostic() для выполнения диагностики легких. Затем создан класс RealLungDiagnostic, реализующий этот интерфейс для реального выполнения диагностики.

Класс LungDiagnosticProxy является прокси-классом, который контролирует доступ к реальному сервису RealLungDiagnostic. В методе performDiagnostic() прокси-класс выполняет дополнительную логику, например, проверку прав доступа, перед вызовом метода реального сервиса.

В методе main создается объект LungDiagnosticProxy и вызывается метод performDiagnostic(), что позволяет выполнить диагностику легких через прокси-класс с контролем доступа к реальному сервису.

Uml-диаграмма
``` sql
-----------------------------------------
|             LungDiagnostic             |
-----------------------------------------
| + performDiagnostic() : void          |
-----------------------------------------
                   ^
                   |
-----------------------------------------
|         RealLungDiagnostic            |
-----------------------------------------
| + performDiagnostic() : void          |
-----------------------------------------

-----------------------------------------
|        LungDiagnosticProxy            |
-----------------------------------------
| - realDiagnostic: RealLungDiagnostic  |
-----------------------------------------
| + performDiagnostic() : void         |
-----------------------------------------
```
## Поведенческие шаблоны
### ***Iterator***
``` Java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

// Класс, представляющий результаты диагностики заболеваний легких
class LungDiagnosticResult {
    private String diagnosis;

    public LungDiagnosticResult(String diagnosis) {
        this.diagnosis = diagnosis;
    }

    public String getDiagnosis() {
        return diagnosis;
    }
}

// Класс, представляющий коллекцию результатов диагностики
class LungDiagnosticResults implements Iterable<LungDiagnosticResult> {
    private List<LungDiagnosticResult> results;

    public LungDiagnosticResults() {
        results = new ArrayList<>();
    }

    public void addResult(LungDiagnosticResult result) {
        results.add(result);
    }

    @Override
    public Iterator<LungDiagnosticResult> iterator() {
        return results.iterator();
    }
}

// Пример использования шаблона Iterator
public class Main {
    public static void main(String[] args) {
        LungDiagnosticResults diagnosticResults = new LungDiagnosticResults();
        diagnosticResults.addResult(new LungDiagnosticResult("Пневмония"));
        diagnosticResults.addResult(new LungDiagnosticResult("Бронхит"));

        System.out.println("Результаты диагностики заболеваний легких:");
        for (LungDiagnosticResult result : diagnosticResults) {
            System.out.println(result.getDiagnosis());
        }
    }
}
```
В данном примере создан класс LungDiagnosticResult, представляющий результаты диагностики заболеваний легких, и класс LungDiagnosticResults, представляющий коллекцию этих результатов. Класс LungDiagnosticResults реализует интерфейс Iterable, что позволяет использовать его в цикле for-each.

В методе main создается объект LungDiagnosticResults, добавляются результаты диагностики, а затем с помощью цикла for-each происходит итерация по результатам и выводится информация о диагнозах.

Паттерн Iterator позволяет упростить работу с коллекциями, обеспечивая удобный и последовательный доступ к их элементам.

Uml-диаграммма
``` sql
-----------------------------------------
|           LungDiagnosticResult         |
-----------------------------------------
| - diagnosis: String                   |
| - severity: String                    |
-----------------------------------------

-----------------------------------------
|          LungDiagnosticResults         |
-----------------------------------------
| - results: List<LungDiagnosticResult> |
-----------------------------------------
| + addResult(result: LungDiagnosticResult) : void |
| + iterator() : Iterator<LungDiagnosticResult>    |
-----------------------------------------
                    ^
                    |
-----------------------------------------
|             Iterator                   |
-----------------------------------------
| + hasNext() : boolean                  |
| + next() : E                           |
-----------------------------------------
```
### ***Template Method***
``` Java
// Абстрактный класс, представляющий шаблон для диагностики заболеваний легких
abstract class LungDiagnosticsTemplate {
    
    // Шаблонный метод, определяющий последовательность выполнения диагностики
    public void diagnoseLungDisease() {
        collectPatientData();
        performPhysicalExamination();
        analyzeSymptoms();
        makeDiagnosis();
    }
    
    // Абстрактные методы, которые должны быть реализованы в подклассах
    protected abstract void collectPatientData();
    protected abstract void performPhysicalExamination();
    protected abstract void analyzeSymptoms();
    
    // Метод для установки диагноза
    protected void makeDiagnosis() {
        System.out.println("Диагноз установлен.");
    }
}

// Класс, реализующий конкретную диагностику заболеваний легких
class PneumoniaDiagnostics extends LungDiagnosticsTemplate {

    @Override
    protected void collectPatientData() {
        System.out.println("Сбор данных о пациенте для диагностики пневмонии.");
    }

    @Override
    protected void performPhysicalExamination() {
        System.out.println("Проведение физического осмотра для диагностики пневмонии.");
    }

    @Override
    protected void analyzeSymptoms() {
        System.out.println("Анализ симптомов для диагностики пневмонии.");
    }
}

// Пример использования шаблона Template Method
public class Main {
    public static void main(String[] args) {
        LungDiagnosticsTemplate diagnostics = new PneumoniaDiagnostics();
        diagnostics.diagnoseLungDisease();
    }
}
```
В данном примере создан абстрактный класс LungDiagnosticsTemplate, который представляет шаблон для диагностики заболеваний легких. В этом классе определен шаблонный метод diagnoseLungDisease(), который определяет последовательность выполнения диагностики, используя абстрактные методы для сбора данных о пациенте, проведения физического осмотра и анализа симптомов.

Класс PneumoniaDiagnostics реализует конкретную диагностику пневмонии, переопределяя абстрактные методы для сбора данных о пациенте, физического осмотра и анализа симптомов.

В методе main создается объект PneumoniaDiagnostics, который представляет конкретную диагностику пневмонии, и вызывается метод diagnoseLungDisease(), который запускает последовательность выполнения диагностики согласно шаблону Template Method.

Паттерн Template Method позволяет определить основные шаги алгоритма в абстрактном классе, оставляя реализацию конкретных шагов подклассам.

Uml-диаграмма:
``` sql
-----------------------------------------
|       LungDiagnosticsTemplate          |
-----------------------------------------
| + diagnoseLungDisease() : void         |
| # collectPatientData() : void          |
| # conductPhysicalExamination() : void  |
| # analyzeSymptoms() : void             |
-----------------------------------------
                    ^
                    |
-----------------------------------------
|      PneumoniaDiagnostics               |
-----------------------------------------
| + collectPatientData() : void          |
| + conductPhysicalExamination() : void  |
| + analyzeSymptoms() : void             |
-----------------------------------------
```
### ***Strategy***
``` Java
// Интерфейс стратегии для диагностики заболеваний легких
interface LungDiagnosticsStrategy {
    void diagnoseLungDisease();
}

// Конкретная стратегия для диагностики пневмонии
class PneumoniaDiagnosticsStrategy implements LungDiagnosticsStrategy {
    @Override
    public void diagnoseLungDisease() {
        System.out.println("Диагностика пневмонии.");
        // Здесь можно реализовать конкретные шаги для диагностики пневмонии
    }
}

// Конкретная стратегия для диагностики туберкулеза
class TuberculosisDiagnosticsStrategy implements LungDiagnosticsStrategy {
    @Override
    public void diagnoseLungDisease() {
        System.out.println("Диагностика туберкулеза.");
        // Здесь можно реализовать конкретные шаги для диагностики туберкулеза
    }
}

// Контекст, использующий стратегию для диагностики заболеваний легких
class LungDiagnosticsContext {
    private LungDiagnosticsStrategy strategy;

    public LungDiagnosticsContext(LungDiagnosticsStrategy strategy) {
        this.strategy = strategy;
    }

    public void setStrategy(LungDiagnosticsStrategy strategy) {
        this.strategy = strategy;
    }

    public void performDiagnostics() {
        strategy.diagnoseLungDisease();
    }
}

// Пример использования паттерна Strategy
public class Main {
    public static void main(String[] args) {
        LungDiagnosticsContext context = new LungDiagnosticsContext(new PneumoniaDiagnosticsStrategy());
        context.performDiagnostics();

        context.setStrategy(new TuberculosisDiagnosticsStrategy());
        context.performDiagnostics();
    }
}
```
В данном примере создан интерфейс LungDiagnosticsStrategy, который определяет метод diagnoseLungDisease() для диагностики заболеваний легких. Затем созданы две конкретные стратегии: PneumoniaDiagnosticsStrategy и TuberculosisDiagnosticsStrategy, которые реализуют этот метод соответственно для диагностики пневмонии и туберкулеза.

Класс LungDiagnosticsContext представляет контекст, который использует стратегию для выполнения диагностики. Метод performDiagnostics() вызывает метод diagnoseLungDisease() у текущей стратегии.

В методе main создается объект контекста LungDiagnosticsContext с начальной стратегией для диагностики пневмонии. Затем вызывается метод performDiagnostics(), который выполняет диагностику пневмонии. После этого устанавливается новая стратегия для диагностики туберкулеза, и снова вызывается метод performDiagnostics(), который выполняет диагностику туберкулеза.

Паттерн Strategy позволяет легко добавлять новые стратегии для выполнения различных алгоритмов без изменения контекста.

Uml-диаграмма
``` sql
-----------------------------------------
|       LungDiagnosticsStrategy          |
-----------------------------------------
| + diagnoseLungDisease() : void         |
-----------------------------------------
                    ^
                    |
-----------------------------------------
|  PneumoniaDiagnosticsStrategy          |
-----------------------------------------
| + diagnoseLungDisease() : void         |
-----------------------------------------
                    ^
                    |
-----------------------------------------
| TuberculosisDiagnosticsStrategy        |
-----------------------------------------
| + diagnoseLungDisease() : void         |
-----------------------------------------
                    ^
                    |
-----------------------------------------
|      LungDiagnosticsContext            |
-----------------------------------------
| - strategy: LungDiagnosticsStrategy    |
-----------------------------------------
| + performDiagnostics() : void          |
-----------------------------------------
```
### ***Command***
``` Java
// Интерфейс команды для выполнения действия
interface Command {
    void execute();
}

// Конкретная команда для выполнения диагностики пневмонии
class PneumoniaDiagnosticCommand implements Command {
    private LungDiagnostic lungDiagnostic;

    public PneumoniaDiagnosticCommand(LungDiagnostic lungDiagnostic) {
        this.lungDiagnostic = lungDiagnostic;
    }

    @Override
    public void execute() {
        lungDiagnostic.diagnosePneumonia();
    }
}

// Конкретная команда для выполнения диагностики туберкулеза
class TuberculosisDiagnosticCommand implements Command {
    private LungDiagnostic lungDiagnostic;

    public TuberculosisDiagnosticCommand(LungDiagnostic lungDiagnostic) {
        this.lungDiagnostic = lungDiagnostic;
    }

    @Override
    public void execute() {
        lungDiagnostic.diagnoseTuberculosis();
    }
}

// Получатель команды
class LungDiagnostic {
    public void diagnosePneumonia() {
        System.out.println("Диагностика пневмонии.");
        // Реализация диагностики пневмонии
    }

    public void diagnoseTuberculosis() {
        System.out.println("Диагностика туберкулеза.");
        // Реализация диагностики туберкулеза
    }
}

// Исполнитель команды
class CommandExecutor {
    public void executeCommand(Command command) {
        command.execute();
    }
}

// Пример использования паттерна Command
public class Main {
    public static void main(String[] args) {
        LungDiagnostic lungDiagnostic = new LungDiagnostic();
        CommandExecutor commandExecutor = new CommandExecutor();

        Command pneumoniaCommand = new PneumoniaDiagnosticCommand(lungDiagnostic);
        Command tuberculosisCommand = new TuberculosisDiagnosticCommand(lungDiagnostic);

        commandExecutor.executeCommand(pneumoniaCommand);
        commandExecutor.executeCommand(tuberculosisCommand);
    }
}
```
В данном примере создан интерфейс Command, который определяет метод execute() для выполнения действия. Затем созданы две конкретные команды: PneumoniaDiagnosticCommand и TuberculosisDiagnosticCommand, которые вызывают соответствующие методы диагностики у объекта LungDiagnostic.

Класс LungDiagnostic представляет получателя команды, который содержит методы для диагностики пневмонии и туберкулеза.

Класс CommandExecutor представляет исполнителя команды, который выполняет команду, переданную ему.

В методе main создается объект LungDiagnostic и CommandExecutor, затем создаются команды для диагностики пневмонии и туберкулеза. Далее каждая команда передается на выполнение исполнителю commandExecutor, который вызывает соответствующий метод диагностики у объекта LungDiagnostic.

Паттерн Command позволяет отделить отправителя запроса от получателя, что обеспечивает более гибкую структуру системы и упрощает добавление новых команд.

Uml-диаграмма
``` sql
-----------------------------------------
|               Command                 |
-----------------------------------------
| + execute() : void                    |
-----------------------------------------
                    ^
                    |
-----------------------------------------
|  PneumoniaDiagnosticCommand           |
-----------------------------------------
| + execute() : void                    |
-----------------------------------------
                    ^
                    |
-----------------------------------------
|  TuberculosisDiagnosticCommand        |
-----------------------------------------
| + execute() : void                    |
-----------------------------------------
                    ^
                    |
-----------------------------------------
|          LungDiagnostic                |
-----------------------------------------
| + diagnosePneumonia() : void           |
| + diagnoseTuberculosis() : void        |
-----------------------------------------
                    ^
                    |
-----------------------------------------
|         CommandExecutor                |
-----------------------------------------
| + executeCommand(Command) : void       |
-----------------------------------------
```
# GRASP
1. **Эксперт информации (Expert)**:
   - Классы PneumoniaDiagnosticCommand и TuberculosisDiagnosticCommand выполняют обязанности команды и имеют доступ к объекту LungDiagnostic, который содержит информацию о методах диагностики легких. Таким образом, классы-команды являются экспертами по выполнению соответствующих действий.

2. **Создатель (Creator)**:
   - Класс Main создает экземпляры команд PneumoniaDiagnosticCommand и TuberculosisDiagnosticCommand, что соответствует принципу Creator.

3. **Контроллер (Controller)**:
   - Класс CommandExecutor выполняет роль контроллера, который получает команду и выполняет ее.

4. **Полиморфизм (Polymorphism)**:
   - Использование интерфейса Command позволяет подставлять различные конкретные команды для выполнения действий без изменения кода исполнителя.

5. **Низкая связанность (Low Coupling)**:
   - Классы команд (PneumoniaDiagnosticCommand, TuberculosisDiagnosticCommand) не зависят напрямую от класса LungDiagnostic, что обеспечивает низкую связанность между объектами.

Принципы разработки, реализованные в коде:
1. **Высокая когезия (High Cohesion)**:
   - Каждый класс выполняет свою конкретную задачу: команды инкапсулируют действия, LungDiagnostic содержит методы диагностики, а CommandExecutor выполняет команду.

2. **Принцип единственной ответственности (Single Responsibility Principle)**:
   - Классы имеют четко определенную ответственность: команды выполняют действия, LungDiagnostic проводит диагностику, а CommandExecutor управляет выполнением команд.

3. **Принцип инверсии зависимостей (Dependency Inversion Principle)**:
   - Использование интерфейса Command позволяет избежать прямой зависимости между классами команд и получателем, что соответствует принципу инверсии зависимостей.

Свойство программы:
1. **Гибкость**:
   - Использование паттерна Command обеспечивает гибкость системы, позволяя добавлять новые команды без изменения клиентского кода и управлять выполнением операций.

Необходимости внесения изменений в код для реализации GRASP не обнаружено, так как эти принципы уже применены.
