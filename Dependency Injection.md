# Lektion 0: Dependency Injection - Grunden för TDD
## Förstå beroendeinjektion innan du börjar med Test-Driven Development

Välkommen till den viktigaste grundlektionen! Innan vi börjar med TDD och Calculator-implementationen måste du förstå **Dependency Injection (DI)**. Detta designmönster är absolut kritiskt för framgångsrik TDD och kommer att användas i alla kommande lektioner.

## 🎯 Varför denna lektion kommer först

- **Dependency Injection** är grunden för testbar kod
- **TDD blir nästan omöjligt** utan bra DI
- **Alla kommande lektioner** bygger på dessa principer
- **Professionell utveckling** kräver förståelse för DI

## 🔍 Vad är Dependency Injection?

**Dependency Injection** är ett designmönster där objekt **får sina beroenden utifrån** istället för att **skapa dem internt**. Det handlar om att **"ge objekt vad de behöver"** istället för att låta dem **"skapa vad de behöver"**.

### 🤔 Tänk på det som ett verkligt exempel

**Utan DI (Dåligt):**
```java
// Som att bygga en bil som tillverkar sina egna däck
public class Car {
    private Engine engine;
    private Wheels wheels;
    
    public Car() {
        this.engine = new V8Engine();     // Hårdkodat! Kan inte ändras
        this.wheels = new SportWheels();  // Hårdkodat! Kan inte testas
    }
}
```

**Med DI (Bra):**
```java
// Som att bygga en bil som får däck och motor levererade
public class Car {
    private final Engine engine;
    private final Wheels wheels;
    
    public Car(Engine engine, Wheels wheels) {  // Flexibelt! Kan växlas
        this.engine = engine;
        this.wheels = wheels;
    }
}
```
**Denna kod demonstrerar konstruktorbaserad beroendeinjektion. Istället för att skapa sina egna Engine och Wheels tar Car-klassen emot dem som konstruktorparametrar. Detta gör klassen flexibel, testbar och löst kopplad, eftersom du kan tillhandahålla vilken implementering som helst av Engine och Wheels när du skapar ett Car-objekt. Användningen av final säkerställer att dessa beroenden inte kan ändras efter konstruktionen.**

## 💻 Kodexempel: Från dålig till bra design

### ❌ **EXEMPEL 1: Utan Dependency Injection (Dåligt)**

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    public double divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Division by zero");
        }
        return (double) a / b;
    }
}

public class TextAnalyzer {
    private Calculator calculator;        // Beroende
    private FileLogger logger;           // Beroende
    private DatabaseConnection database; // Beroende
    
    public TextAnalyzer() {
        // PROBLEM: Skapar alla beroenden internt
        this.calculator = new Calculator();
        this.logger = new FileLogger("/tmp/log.txt");
        this.database = new DatabaseConnection("localhost:5432");
    }
    
    public double calculateAverageWordLength(String text) {
        logger.log("Analyzing text: " + text);
        
        String[] words = text.split(" ");
        int totalChars = 0;
        
        for (String word : words) {
            totalChars = calculator.add(totalChars, word.length());
        }
        
        double average = calculator.divide(totalChars, words.length);
        database.save("average_length", average);
        
        return average;
    }
}
```

**Problem med denna design:**
- 🚨 **Omöjligt att testa** - kan inte styra vad Calculator, Logger eller Database gör
- 🚨 **Hårt kopplat** - TextAnalyzer är låst till specifika implementationer
- 🚨 **Dolda beroenden** - inte uppenbart vad klassen behöver
- 🚨 **Inflexibelt** - kan inte använda olika Calculator-typer
- 🚨 **Produktionsproblem** - vad händer om databasen är nere?

### ✅ **EXEMPEL 2: Med Dependency Injection (Bra)**

```java
// Samma Calculator som tidigare
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    public double divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Division by zero");
        }
        return (double) a / b;
    }
}

// Gränssnitt för flexibilitet
public interface Logger {
    void log(String message);
}

public interface Database {
    void save(String key, double value);
}

public class TextAnalyzer {
    private final Calculator calculator;  // Injicerade beroenden
    private final Logger logger;
    private final Database database;
    
    // Konstruktor som tar alla beroenden
    public TextAnalyzer(Calculator calculator, Logger logger, Database database) {
        this.calculator = calculator;
        this.logger = logger;
        this.database = database;
    }
    
    public double calculateAverageWordLength(String text) {
        logger.log("Analyzing text: " + text);
        
        String[] words = text.split(" ");
        int totalChars = 0;
        
        for (String word : words) {
            totalChars = calculator.add(totalChars, word.length());
        }
        
        double average = calculator.divide(totalChars, words.length);
        database.save("average_length", average);
        
        return average;
    }
}
```

**Fördelar med denna design:**
- ✅ **Lätt att testa** - kan kontrollera alla beroenden
- ✅ **Löst kopplat** - kan växla implementationer
- ✅ **Tydliga beroenden** - konstruktorn visar vad som behövs
- ✅ **Flexibelt** - kan använda olika Calculator-typer
- ✅ **Säkert** - kan hantera fel genom test-dubletter

## 🧪 Varför DI är kritiskt för TDD

### 1. **Testbar kod**

**Utan DI - Omöjligt att testa:**
```java
@Test
void shouldCalculateAverageWordLength() {
    TextAnalyzer analyzer = new TextAnalyzer(); // Skapar FileLogger, DatabaseConnection etc.
    
    // Hur testar vi detta?
    // - FileLogger skriver till verklig fil
    // - DatabaseConnection försöker ansluta till riktig databas
    // - Testet är inte isolerat
    // - Kan misslyckas av miljöskäl
}
```

**Med DI - Lätt att testa:**
```java
// Enkla testimplementationer
class TestLogger implements Logger {
    public void log(String message) {
        // Gör ingenting i test, eller logga till minne
    }
}

class TestDatabase implements Database {
    private Map<String, Double> data = new HashMap<>();
    
    public void save(String key, double value) {
        data.put(key, value);
    }
    
    public double getValue(String key) {
        return data.get(key);
    }
}

@Test
void shouldCalculateAverageWordLength() {
    // Arrange - Vi kontrollerar alla beroenden
    Calculator calculator = new Calculator();
    Logger logger = new TestLogger();
    TestDatabase database = new TestDatabase();
    TextAnalyzer analyzer = new TextAnalyzer(calculator, logger, database);
    
    // Act
    double result = analyzer.calculateAverageWordLength("hello world");
    
    // Assert
    assertThat(result).isEqualTo(5.0); // (5+5)/2 = 5.0
    assertThat(database.getValue("average_length")).isEqualTo(5.0);
}
```

### 2. **Isolerad testning**

Med DI kan vi testa en klass utan att bekymra oss om dess beroenden:

```java
@Test
void shouldHandleDivisionByZero() {
    // Arrange
    Calculator calculator = new Calculator();
    Logger logger = new TestLogger();
    Database database = new TestDatabase();
    TextAnalyzer analyzer = new TextAnalyzer(calculator, logger, database);
    
    // Act & Assert
    assertThatThrownBy(() -> analyzer.calculateAverageWordLength(""))
        .isInstanceOf(IllegalArgumentException.class);
}
```

### 3. **Kontrollerade testmiljöer**

```java
@Test
void shouldLogCorrectMessage() {
    // Arrange - Logger som fångar meddelanden
    class CapturingLogger implements Logger {
        private String lastMessage;
        
        public void log(String message) {
            this.lastMessage = message;
        }
        
        public String getLastMessage() {
            return lastMessage;
        }
    }
    
    Calculator calculator = new Calculator();
    CapturingLogger logger = new CapturingLogger();
    Database database = new TestDatabase();
    TextAnalyzer analyzer = new TextAnalyzer(calculator, logger, database);
    
    // Act
    analyzer.calculateAverageWordLength("test");
    
    // Assert
    assertThat(logger.getLastMessage()).isEqualTo("Analyzing text: test");
}
```

## 🔧 Typer av Dependency Injection

### 1. **Constructor Injection (Bäst för TDD)**

```java
public class TextAnalyzer {
    private final Calculator calculator;  // final = oföränderlig
    
    public TextAnalyzer(Calculator calculator) {  // ← Injiceras här
        this.calculator = calculator;
    }
}
```

**Fördelar:**
- ✅ Oföränderliga objekt (`final`)
- ✅ Alla beroenden måste ges vid skapande
- ✅ Lätt att se vad som behövs
- ✅ Fungerar perfekt med TDD

### 2. **Setter Injection**

```java
public class TextAnalyzer {
    private Calculator calculator;
    
    public void setCalculator(Calculator calculator) {  // ← Injiceras här
        this.calculator = calculator;
    }
}
```

**Nackdelar för TDD:**
- ❌ Kan användas innan alla beroenden är satta
- ❌ Mutable state (kan ändras efter skapande)
- ❌ Svårare att se obligatoriska beroenden

### 3. **Interface Injection (Sällan använd)**

```java
public interface CalculatorAware {
    void injectCalculator(Calculator calculator);
}

public class TextAnalyzer implements CalculatorAware {
    private Calculator calculator;
    
    public void injectCalculator(Calculator calculator) {
        this.calculator = calculator;
    }
}
```

## 🎯 Praktisk övning: Bygg med DI från början

Låt oss bygga en enkel `MathProcessor` med korrekt DI:

### Steg 1: Definiera interface (om behövs)

```java
public interface Calculator {
    int add(int a, int b);
    int subtract(int a, int b);
    int multiply(int a, int b);
    double divide(int a, int b);
}
```

### Steg 2: Implementera Calculator

```java
public class BasicCalculator implements Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    public int subtract(int a, int b) {
        return a - b;
    }
    
    public int multiply(int a, int b) {
        return a * b;
    }
    
    public double divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Division by zero");
        }
        return (double) a / b;
    }
}
```

### Steg 3: Bygg MathProcessor med DI

```java
public class MathProcessor {
    private final Calculator calculator;
    
    // Constructor injection
    public MathProcessor(Calculator calculator) {
        this.calculator = calculator;
    }
    
    public int calculateSum(int[] numbers) {
        int sum = 0;
        for (int number : numbers) {
            sum = calculator.add(sum, number);
        }
        return sum;
    }
    
    public double calculateAverage(int[] numbers) {
        if (numbers.length == 0) {
            throw new IllegalArgumentException("Cannot calculate average of empty array");
        }
        
        int sum = calculateSum(numbers);
        return calculator.divide(sum, numbers.length);
    }
}
```

### Steg 4: Skriv tester med DI

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.*;

class MathProcessorTest {
    
    private MathProcessor processor;
    private Calculator calculator;
    
    @BeforeEach
    void setUp() {
        // Dependency injection i testet
        calculator = new BasicCalculator();
        processor = new MathProcessor(calculator);
    }
    
    @Test
    void shouldCalculateSumCorrectly() {
        // Arrange
        int[] numbers = {1, 2, 3, 4, 5};
        
        // Act
        int result = processor.calculateSum(numbers);
        
        // Assert
        assertThat(result).isEqualTo(15);
    }
    
    @Test
    void shouldCalculateAverageCorrectly() {
        // Arrange
        int[] numbers = {2, 4, 6, 8};
        
        // Act
        double result = processor.calculateAverage(numbers);
        
        // Assert
        assertThat(result).isEqualTo(5.0);
    }
    
    @Test
    void shouldThrowExceptionForEmptyArray() {
        // Arrange
        int[] emptyArray = {};
        
        // Act & Assert
        assertThatThrownBy(() -> processor.calculateAverage(emptyArray))
            .isInstanceOf(IllegalArgumentException.class)
            .hasMessage("Cannot calculate average of empty array");
    }
}
```

### Steg 5: Testa med olika Calculator-implementationer

```java
// Skapar en "mock" Calculator för specifik testning
class MockCalculator implements Calculator {
    public int add(int a, int b) {
        return 999; // Alltid returnera 999 för test
    }
    
    public int subtract(int a, int b) { return 0; }
    public int multiply(int a, int b) { return 0; }
    public double divide(int a, int b) { return 0.0; }
}

@Test
void shouldUseCalculatorForAddition() {
    // Arrange
    Calculator mockCalculator = new MockCalculator();
    MathProcessor processor = new MathProcessor(mockCalculator);
    
    // Act
    int result = processor.calculateSum(new int[]{1, 2}); // add() called twice
    
    // Assert
    assertThat(result).isEqualTo(999); // Bevisar att vår Calculator användes
}
```

## 🎭 Testdubbler (Test Doubles) - Förstå skillnaderna

När vi använder Dependency Injection kan vi ersätta riktiga beroenden med **testdubbler** under testning. Det finns fem huvudtyper, var och en med sitt specifika syfte.

### 🔍 Vad är en testdublett?

En **testdublett** (test double) är en generisk term för alla typer av objekt som ersätter riktiga beroenden i tester. Termen kommer från filmvärlden där "stunt double" används istället för den riktiga skådespelaren.

> **"Test Double är paraplytermen - Mock, Stub, Fake, Spy och Dummy är specifika typer"**

---

## 📖 De fem typerna av testdubbler

### 1. 🎪 **Dummy** - "Bara för att fylla plats"

**Syfte:** Används när ett objekt krävs men aldrig används i testet.

```java
// Dummy används bara för att kompilera, men anropas aldrig
public interface Logger {
    void log(String message);
}

public class DummyLogger implements Logger {
    @Override
    public void log(String message) {
        // Gör ingenting - används aldrig
    }
}

@Test
void shouldCalculateWithoutLogging() {
    Logger dummy = new DummyLogger();  // Bara för att uppfylla konstruktorn
    Calculator calc = new Calculator(dummy);
    
    int result = calc.add(2, 3);  // Logger används aldrig här
    assertThat(result).isEqualTo(5);
}
```

**När du använder Dummy:**
- Parametern krävs av konstruktorn men används inte i testet
- Du vill fokusera på annan funktionalitet
- Håller testkoden enkel genom att undvika onödiga detaljer

---

### 2. 📝 **Stub** - "Förprogrammerade svar"

**Syfte:** Ger förutbestämda svar på anrop, ignorerar allt annat.

```java
// Stub returnerar hårdkodade värden
public class StubCalculator implements Calculator {
    @Override
    public int add(int a, int b) {
        return 10;  // Alltid returnera 10, oavsett input
    }
    
    @Override
    public int multiply(int a, int b) {
        return 20;  // Alltid returnera 20
    }
    
    // Andra metoder kan kasta exception eller returnera null
}

@Test
void shouldProcessDataWithPredictableCalculations() {
    Calculator stub = new StubCalculator();
    DataProcessor processor = new DataProcessor(stub);
    
    // Vi vet exakt vad Calculator returnerar
    int result = processor.process(new int[]{1, 2, 3});
    
    // Tester baseras på stubbens förutsägbara beteende
    assertThat(result).isEqualTo(30);  // 10 + 20 från stubben
}
```

**När du använder Stub:**
- Du behöver kontrollera vad beroenden returnerar
- Du testar hur din kod **använder** resultaten
- Du vill ha förutsägbara, enkla returvärden

---

### 3. 🎯 **Mock** - "Verifierar beteende"

**Syfte:** Förprogrammerade förväntningar om vilka anrop som ska göras.

```java
import java.util.*;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class MockNotifier implements Notifier {
    List<String> sentMessages = new ArrayList<>();

    @Override
    public void send(String message) {
        sentMessages.add(message);
    }

    void verifyMessageSent(String expected) {
        assertTrue(sentMessages.contains(expected), "Expected message not sent: " + expected);
    }
}

public class BankAccountTest {

    @Test
    void shouldSendNotificationAfterDeposit() {
        MockNotifier mockNotifier = new MockNotifier();
        BankAccount account = new BankAccount(mockNotifier);

        account.deposit(100);

        assertEquals(100, account.getBalance());
        mockNotifier.verifyMessageSent("Deposit successful: 100");
    }
}

```

**När du använder Mock:**
- Du vill verifiera att din kod **anropar** andra objekt korrekt
- Fokus är på **interaktion** mellan objekt
- Du testar **beteende**, inte bara tillstånd

**🔴 Viktigt:** Din kod i `NumberAnalyzerTest.java` använder vad som **tekniskt är en Stub** men kallar den "Mock":

```java
// Detta är egentligen en STUB, inte en Mock
private static class MockCalculator extends Calculator {
    @Override
    public int modulo(int dividend, int divisor) {
        return 1;  // Returnerar bara ett värde - ingen verifiering
    }
}
```

---

### 4. 🏗️ **Fake** - "Enkel fungerande implementation"

**Syfte:** Har fungerande implementation men är förenklad (t.ex. in-memory databas).

```java
// Fake är en riktig implementation, men enklare/snabbare
public class FakeDatabase implements Database {
    private Map<String, String> data = new HashMap<>();
    
    @Override
    public void save(String key, String value) {
        data.put(key, value);  // In-memory istället för disk/nätverk
    }
    
    @Override
    public String load(String key) {
        return data.get(key);
    }
    
    @Override
    public void delete(String key) {
        data.remove(key);
    }
}

@Test
void shouldPersistUserData() {
    Database fake = new FakeDatabase();  // Snabb, ingen riktig databas
    UserService service = new UserService(fake);
    
    // Act
    service.saveUser("user1", "John Doe");
    String result = service.getUser("user1");
    
    // Assert - Fake fungerar som riktig databas
    assertThat(result).isEqualTo("John Doe");
}
```

**När du använder Fake:**
- Riktig implementation är för långsam (databas, nätverk, filsystem)
- Du vill testa integrationer utan externa beroenden
- Du behöver fungerande logik, inte bara hårdkodade värden

---

### 5. 🕵️ **Spy** - "Wrapper som spårar anrop"

**Syfte:** Använder riktigt objekt men spårar hur det används.

```java
// Spy omsluter ett riktigt objekt och loggar anrop
public class SpyCalculator extends Calculator {
    private final Calculator realCalculator;
    private int addCallCount = 0;
    private int multiplyCallCount = 0;
    
    public SpyCalculator(Calculator real) {
        this.realCalculator = real;
    }
    
    @Override
    public int add(int a, int b) {
        addCallCount++;
        return realCalculator.add(a, b);  // Använder riktig logik!
    }
    
    @Override
    public int multiply(int a, int b) {
        multiplyCallCount++;
        return realCalculator.multiply(a, b);
    }
    
    public int getAddCallCount() { return addCallCount; }
    public int getMultiplyCallCount() { return multiplyCallCount; }
}

@Test
void shouldCallAddMultipleTimes() {
    Calculator realCalc = new Calculator();
    SpyCalculator spy = new SpyCalculator(realCalc);
    DataProcessor processor = new DataProcessor(spy);
    
    // Act
    processor.processArray(new int[]{1, 2, 3, 4, 5});
    
    // Assert - Verifiera antal anrop OCH korrekt resultat
    assertThat(spy.getAddCallCount()).isEqualTo(4);  // Verifiering
    assertThat(processor.getSum()).isEqualTo(15);     // Korrekt resultat
}
```

**När du använder Spy:**
- Du vill använda **riktig implementation**
- Men också **spåra/verifiera** hur den används
- Balans mellan Mock och riktigt objekt

---

## 📊 Jämförelsetabell

| Typ       | Syfte                                                  | Har riktig logik?             | Verifierar något? |
| --------- | ------------------------------------------------------ | ----------------------------- | ----------------- |
| **Dummy** | Bara för att uppfylla ett metod- eller konstruktorkrav | ❌ Nej                         | ❌ Nej             |
| **Stub**  | Returnerar förbestämda värden                          | ✅ Delvis (minimal)            | ❌ Nej             |
| **Fake**  | Förenklad men fungerande implementation                | ✅ Ja (förenklad logik)        | ❌ Nej             |
| **Mock**  | Används för att verifiera anrop/interaktioner          | ❌ (oftast ingen riktig logik) | ✅ Ja              |
| **Spy**   | Kombination: riktig logik + verifiering                | ✅ Ja                          | ✅ Ja              |


---

## 🧠 När ska du använda vilken?

### Använd **Dummy** när:
- "Jag behöver något här, men det spelar ingen roll vad"
- Konstruktor kräver parameter som inte används i testet

### Använd **Stub** när:
- "Jag vill kontrollera vad beroenden returnerar"
- Du testar hur kod **hanterar** olika returvärden
- **Fokus:** State verification

### Använd **Mock** när:
- "Jag vill verifiera att rätt metoder anropas"
- Du testar **interaktioner** mellan objekt
- **Fokus:** Behavior verification

### Använd **Fake** när:
- "Jag behöver fungerande logik, men riktigt objekt är för långsamt"
- Databas, API, filsystem i tester
- **Fokus:** Realistic behavior, fast execution

### Använd **Spy** när:
- "Jag vill både verifiera anrop OCH använda riktig logik"
- Partial mocking behövs
- **Fokus:** Real behavior + tracking

---

## 🏆 Best Practices

### ✅ Gör:
```java
// Tydliga namn
class StubPaymentGateway      // Returnerar hårdkodade värden
class MockEmailService         // Verifierar att emails skickades
class FakeUserRepository       // In-memory implementation
```

### ❌ Undvik:
```java
// Otydliga namn
class TestPaymentGateway       // Vilken typ?
class MockRepository           // Om det bara returnerar värden, är det en Stub
```

---

## 📚 Ytterligare resurser

- **Martin Fowler's artikel:** ["Mocks Aren't Stubs"](https://martinfowler.com/articles/mocksArentStubs.html)
- **Gerard Meszaros:** "xUnit Test Patterns" - definitioner av test doubles
- **Mockito framework:** Professionellt mock framework för Java (ej i detta projekt)

---

## 💡 Sammanfattning

1. **Test Double** = Generisk term för alla typer
2. **Dummy** = Bara för att fylla plats
3. **Stub** = Returnerar hårdkodade värden
4. **Mock** = Verifierar att metoder anropades
5. **Fake** = Fungerande men förenklad implementation
6. **Spy** = Riktigt objekt + spårning

**I detta projekt:**
- Vi använder huvudsakligen **Stubs** (även om vi kallar dem "Mocks")
- Vi fokuserar på **handskrivna test doubles** (ingen Mockito)
- Det lär dig **grunderna** innan du använder avancerade framework

## 🌟 DI i kommande lektioner

Nu förstår du varför alla kommande lektioner använder DI:

### **Lektion 1 (Calculator):**
- Calculator har inga beroenden, så DI är enkelt

### **Lektion 2 (StringProcessor):**
- StringProcessor kan använda Calculator
- `StringProcessor(Calculator calculator)`

### **Lektion 3 (TextAnalyzer):**
- TextAnalyzer använder både Calculator och StringProcessor
- `TextAnalyzer(Calculator calculator, StringProcessor stringProcessor)`

### **Lektion 4 (Integration):**
- Testar hur alla komponenter fungerar tillsammans
- Fullständig kontroll över alla beroenden

## 🧪 Praktiska tips för DI och TDD

### 1. **Använd alltid Constructor Injection**
```java
// BRA
public class MyClass {
    private final Dependency dep;
    
    public MyClass(Dependency dep) {
        this.dep = dep;
    }
}
```

### 2. **Gör fält final när möjligt**
```java
private final Calculator calculator;  // Kan inte ändras efter konstruktor
```

### 3. **Använd interfaces för flexibilitet**
```java
// Istället för konkret klass
public MathProcessor(BasicCalculator calc) { }

// Använd interface
public MathProcessor(Calculator calc) { }  // Kan ta vilken Calculator som helst
```

### 4. **Håll konstruktörer enkla**
```java
public TextAnalyzer(Calculator calc, StringProcessor proc) {
    this.calculator = calc;           // Bara tilldelning
    this.stringProcessor = proc;      // Ingen komplex logik
}
```

### 5. **Testa isolerat**
```java
@Test
void shouldTestJustOneThingAtATime() {
    // Använd enkla, kontrollerade beroenden
    Calculator calc = new BasicCalculator();
    MyClass obj = new MyClass(calc);
    
    // Testa bara MyClass, inte Calculator
}
```

## 🎓 Kontrollera din förståelse

Innan du går vidare till Lektion 1, säkerställ att du förstår:

### ✅ **Grundläggande begrepp**
- [ ] Vad är Dependency Injection?
- [ ] Varför är DI viktigt för TDD?
- [ ] Skillnaden mellan Constructor och Setter Injection

### ✅ **Praktisk tillämpning**
- [ ] Kan du identifiera dålig kod utan DI?
- [ ] Kan du omvandla kod för att använda DI?
- [ ] Förstår du hur DI gör testning enklare?

### ✅ **Test-scenarier**
- [ ] Kan du skriva tester med DI?
- [ ] Förstår du hur man kontrollerar beroenden i tester?
- [ ] Kan du testa isolerat från externa system?

## 🚀 Övning: Innan Lektion 1

Försök att implementera denna klass med korrekt DI:

```java
// UPPGIFT: Omvandla denna klass för att använda Dependency Injection
public class NumberAnalyzer {
    
    public NumberAnalyzer() {
        // Inga beroenden här - BRA START!
    }
    
    // Lägg till metoder som använder Calculator genom DI
    public boolean isPrime(int number) {
        // Implementera primtalskontroll
        // Tips: Använd Calculator för modulo-operationer
    }
    
    public int factorial(int number) {
        // Implementera fakultet
        // Tips: Använd Calculator för multiplikation
    }
}
```

**Din uppgift:**
1. Lägg till Calculator som beroende via Constructor Injection
2. Implementera metoderna med Calculator
3. Skriv tester som injicerar Calculator
4. Säkerställ att testerna isolerar NumberAnalyzer från Calculator-implementation

---

## 🎯 Sammanfattning

**Dependency Injection** är **grunden för framgångsrik TDD**. Utan DI:
- ❌ Tester blir komplicerade och opålitliga
- ❌ Kod blir hårt kopplad och svår att ändra
- ❌ Isolerad testning blir omöjlig

Med DI:
- ✅ Tester blir enkla och pålitliga
- ✅ Kod blir flexibel och underhållbar
- ✅ Isolerad testning blir naturlig

**Nu är du redo för Lektion 1!** Du förstår varför vi bygger klasser med DI och hur det gör TDD möjligt och njutbart.

**Kom ihåg:** *Bra design gör testning enkel. Dålig design gör testning omöjlig.* DI är skillnaden mellan bra och dålig design! 🚀

---

## 💡 **LÖSNING: NumberAnalyzer med Dependency Injection**

Här är den kompletta lösningen till övningsuppgiften, steg för steg:

### **Steg 1: Lägg till Calculator som beroende via Constructor Injection**

```java
package com.example.tdd;

public class NumberAnalyzer {
    
    private final Calculator calculator;  // Injicerat beroende - final för immutability
    
    /**
     * Constructor som tar Calculator som beroende.
     * Detta är Constructor Injection - den bästa formen av DI för TDD.
     */
    public NumberAnalyzer(Calculator calculator) {
        if (calculator == null) {
            throw new IllegalArgumentException("Calculator cannot be null");
        }
        this.calculator = calculator;
    }
    
    // Metoder implementeras nedan...
}
```

**Viktiga DI-principer som följs:**
- ✅ **Final field** - calculator kan inte ändras efter konstruktion
- ✅ **Null-kontroll** - robust mot felaktig användning  
- ✅ **Constructor Injection** - alla beroenden måste ges vid skapande
- ✅ **Tydliga beroenden** - konstruktorn visar exakt vad som behövs

### **Steg 2: Implementera metoderna med Calculator**

```java
/**
 * Kontrollerar om ett tal är ett primtal.
 * Använder Calculator för modulo-operationer genom DI.
 */
public boolean isPrime(int number) {
    if (number < 0) {
        throw new IllegalArgumentException("Cannot check primality of negative numbers");
    }
    
    if (number <= 1) {
        return false;
    }
    
    if (number <= 3) {
        return true;
    }
    
    // Använd Calculator för modulo-operationer - DI in action!
    if (calculator.modulo(number, 2) == 0 || calculator.modulo(number, 3) == 0) {
        return false;
    }
    
    // Kontrollera divisorer från 5 till sqrt(number)
    for (int i = 5; i * i <= number; i += 6) {
        if (calculator.modulo(number, i) == 0 || 
            calculator.modulo(number, calculator.add(i, 2)) == 0) {
            return false;
        }
    }
    
    return true;
}

/**
 * Beräknar fakultet av ett tal.
 * Använder Calculator för multiplikationsoperationer genom DI.
 */
public int factorial(int number) {
    if (number < 0) {
        throw new IllegalArgumentException("Cannot calculate factorial of negative numbers");
    }
    
    if (number == 0 || number == 1) {
        return 1;
    }
    
    int result = 1;
    
    // Använd Calculator för multiplikation - DI in action!
    for (int i = 2; i <= number; i++) {
        // Kontrollera overflow innan multiplikation
        if (result > Integer.MAX_VALUE / i) {
            throw new ArithmeticException("Factorial overflow for number: " + number);
        }
        result = calculator.multiply(result, i);
    }
    
    return result;
}
```

**Så här använder vi Calculator genom DI:**
- 🔧 **calculator.modulo()** istället för direkt % operator
- 🔧 **calculator.multiply()** istället för direkt * operator  
- 🔧 **calculator.add()** för addition via injicerat beroende
- 🔧 **Alla matematiska operationer** går genom Calculator

### **Steg 3: Skriv tester som injicerar Calculator**

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.*;

class NumberAnalyzerTest {
    
    private NumberAnalyzer analyzer;
    private Calculator calculator;
    
    @BeforeEach
    void setUp() {
        // Dependency Injection i testet - vi kontrollerar alla beroenden!
        calculator = new Calculator();
        analyzer = new NumberAnalyzer(calculator);
    }
    
    @Test
    void shouldAcceptCalculatorDependencyViaConstructor() {
        // Arrange
        Calculator testCalculator = new Calculator();
        
        // Act & Assert - Constructor Injection fungerar
        assertThatCode(() -> new NumberAnalyzer(testCalculator))
            .doesNotThrowAnyException();
    }
    
    @Test
    void shouldThrowExceptionWhenCalculatorIsNull() {
        // Act & Assert - Null-kontroll för robusthet
        assertThatThrownBy(() -> new NumberAnalyzer(null))
            .isInstanceOf(IllegalArgumentException.class)
            .hasMessage("Calculator cannot be null");
    }
    
    @Test
    void shouldCalculateFactorialCorrectly() {
        // Arrange
        int number = 5;
        
        // Act
        int result = analyzer.factorial(number);
        
        // Assert
        assertThat(result).isEqualTo(120); // 5! = 120
    }
    
    @Test
    void shouldIdentifyPrimeNumbersCorrectly() {
        // Test flera primtal
        assertThat(analyzer.isPrime(2)).isTrue();
        assertThat(analyzer.isPrime(3)).isTrue();
        assertThat(analyzer.isPrime(5)).isTrue();
        assertThat(analyzer.isPrime(7)).isTrue();
        assertThat(analyzer.isPrime(11)).isTrue();
        
        // Test icke-primtal
        assertThat(analyzer.isPrime(4)).isFalse();
        assertThat(analyzer.isPrime(6)).isFalse();
        assertThat(analyzer.isPrime(8)).isFalse();
        assertThat(analyzer.isPrime(9)).isFalse();
    }
}
```

### **Steg 4: Säkerställ isolerad testning**

```java
@Test
void shouldWorkWithCustomCalculatorImplementation() {
    // Skapa en anpassad Calculator för specifik testning
    Calculator customCalculator = new Calculator() {
        @Override
        public int modulo(int dividend, int divisor) {
            return 1; // Alltid returnera 1 för att testa isolering
        }
    };
    
    NumberAnalyzer customAnalyzer = new NumberAnalyzer(customCalculator);
    
    // Med denna anpassade Calculator kommer isPrime att bete sig annorlunda
    boolean result = customAnalyzer.isPrime(4); // Normalt false
    
    // Bevisar att vi kan kontrollera Calculator-beteende i tester
    assertThat(result).isTrue(); // Eftersom modulo alltid returnerar 1 != 0
}

@Test
void shouldIsolateNumberAnalyzerLogicFromCalculatorImplementation() {
    // Test-dublett som gör multiply() = addition istället
    Calculator predictableCalculator = new Calculator() {
        @Override
        public int multiply(int a, int b) {
            return a + b; // Addition istället för multiplikation
        }
    };
    
    NumberAnalyzer testAnalyzer = new NumberAnalyzer(predictableCalculator);
    
    // Nu testar vi NumberAnalyzer-logiken med förutsägbar Calculator
    int result = testAnalyzer.factorial(3);
    
    // Med vår modifierade Calculator: factorial(3) = 1 + 2 + 3 = 6
    assertThat(result).isEqualTo(6);
}
```

## 🎯 **Vad vi lärde oss från denna lösning**

### **1. Constructor Injection i praktiken**
```java
// Före DI (Dåligt)
public NumberAnalyzer() {
    this.calculator = new Calculator(); // Hårdkodat!
}

// Efter DI (Bra) 
public NumberAnalyzer(Calculator calculator) {
    this.calculator = calculator; // Flexibelt!
}
```

### **2. Testbar kod genom DI**
- ✅ **Kan använda riktig Calculator** för normala tester
- ✅ **Kan använda mock Calculator** för isolerad testning
- ✅ **Kan kontrollera alla beroenden** i testmiljön
- ✅ **Inga dolda beroenden** som gör testning svår

### **3. Separation of Concerns**
- 🎯 **NumberAnalyzer** fokuserar på algoritmer (primtal, fakultet)
- 🎯 **Calculator** fokuserar på grundläggande matematik
- 🎯 **Varje klass har ett tydligt ansvar**
- 🎯 **Lätt att ändra utan att påverka andra klasser**

### **4. Flexibilitet genom DI**
```java
// Kan använda olika Calculator-implementationer:
NumberAnalyzer analyzer1 = new NumberAnalyzer(new BasicCalculator());
NumberAnalyzer analyzer2 = new NumberAnalyzer(new ScientificCalculator());
NumberAnalyzer analyzer3 = new NumberAnalyzer(new MockCalculator());
```

**Detta är fundamentet** som alla kommande TDD-lektioner bygger på. Nu förstår du varför varje klass vi bygger framöver kommer att använda Constructor Injection och varför våra tester alltid kommer att ha full kontroll över alla beroenden.

---

**Nästa steg:** Gå till **Lektion 1** och se hur vi tillämpar DI-principerna för att bygga en Calculator-klass genom Test-Driven Development! 📚

