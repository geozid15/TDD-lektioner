# Lektion 1: Test-Driven Development (TDD) Grundprinciper
## Din första introduktion till TDD med Calculator-klassen

I denna lektion kommer du att lära dig grundprinciperna för Test-Driven Development genom att bygga en enkel Calculator-klass från grunden.

## 🎯 Vad kommer du att lära dig?

- Förstå **RED-GREEN-REFACTOR** cykeln
- Skriva ditt första misslyckande test
- Implementera minimal kod för att få testet att lyckas
- Refaktorera kod medan testerna förblir gröna
- Förstå varför vi skriver tester **före** produktionskoden

## 🔄 TDD-cykeln: RED-GREEN-REFACTOR

TDD följer en enkel men kraftfull trestegs-cykel:

### 🔴 RED - Skriv ett misslyckande test
- Skriv ett test för den funktionalitet du vill skapa
- Kör testet - det **måste** misslyckas (eftersom koden inte finns än)
- Detta bekräftar att testet faktiskt testar något

### 🟢 GREEN - Få testet att lyckas  
- Skriv den **minsta** mängden kod som behövs
- Fokusera inte på "perfekt" kod - bara få testet att lyckas
- Kör testet igen - nu ska det lyckas

### 🔵 REFACTOR - Förbättra koden
- Städa upp kod utan att ändra beteende
- Ta bort duplicering
- Förbättra design och läsbarhet
- Alla tester ska fortfarande lyckas

## 📚 Förutsättningar

**Du bör ha slutfört Lektion 0** och förstå:
- ✅ Vad Dependency Injection är och varför det är viktigt
- ✅ Skillnaden mellan bra och dålig koddesign  
- ✅ Hur DI gör testning möjlig och enkel
- ✅ Constructor Injection vs andra DI-typer

## 📚 Praktisk övning: Bygga Calculator steg för steg

### Steg 1: Förberedelser

Först, säkerställ att du har rätt projektstruktur:

```
TDD/
├── src/
│   ├── main/java/com/example/tdd/
│   │   └── (Tom - vi skapar Calculator.java senare)
│   └── test/java/com/example/tdd/
│       └── CalculatorTest.java  # Vi börjar här!
└── pom.xml
```

## 📝 Kom igång: Initiala uppgifter

Innan du börjar koda, här är en översikt över uppgifterna du kommer att genomföra i denna lektion. Följ RED-GREEN-REFACTOR cykeln för varje steg!

### ✅ Uppgift 1: Skapa CalculatorTest med första testet (RED)
**Mål:** TDD börjar här! Skriv testet INNAN Calculator-klassen finns  
**Fil:** `src/test/java/com/example/tdd/CalculatorTest.java`

Skapa testet med:
1. `@DisplayName("Calculator TDD Demo")` på klassnivå
2. `@BeforeEach` setup med `Calculator`
3. Första testet:
   - Metod: `shouldAddTwoPositiveNumbers()`
   - `@DisplayName("Should add two positive numbers")`
4. **Kör testet** - det ska MISSLYCKAS (RED fas) ✗

### ✅ Uppgift 2: Skapa Calculator klass (GREEN)
**Mål:** Minimal implementation för att få testet att lyckas  
**Fil:** `src/main/java/com/example/tdd/Calculator.java`

Implementera:
- Grundläggande klasstruktur
- `add(int a, int b)` metod
- **Hårdkoda först** (returnera 8) för att förstå konceptet
- **Kör testet igen** - det ska NU LYCKAS (GREEN fas) ✓

### ✅ Uppgift 3: Lägg till fler additions-tester (RED → GREEN)
**Mål:** Driva fram riktig implementation

Lägg till tester (PHASE 1: Basic Addition):
1. **Test 2:** `shouldAddZeroToNumber()`
   - `@DisplayName("Should add zero to a number")`
2. **Test 3:** `shouldAddNegativeNumbers()`
   - `@DisplayName("Should add negative numbers")`

**Implementera sedan riktig `add()` metod** (`return a + b;`) ✓

### ✅ Uppgift 4: Subtraktion (RED → GREEN → REFACTOR)
**Mål:** Fortsätt TDD-cykeln

**PHASE 2: Subtraction** - Lägg till tester:
1. `shouldSubtractTwoNumbers()` - "Should subtract two numbers"
2. `shouldHandleNegativeResultInSubtraction()` - "Should handle negative result in subtraction"

**Implementera `subtract(int a, int b)` metod** ✓

### ✅ Uppgift 5: Multiplikation (RED → GREEN)
**Mål:** Bygg på funktionalitet

**PHASE 3: Multiplication** - Lägg till tester:
1. `shouldMultiplyTwoNumbers()` - "Should multiply two numbers"
2. `shouldReturnZeroWhenMultiplyingByZero()` - "Should return zero when multiplying by zero"

**Implementera `multiply(int a, int b)` metod** ✓

### ✅ Uppgift 6: Division med felhantering (RED → GREEN)
**Mål:** Lär dig undantagshantering i TDD

**PHASE 4: Division with Error Handling** - Lägg till tester:
1. `shouldDivideTwoNumbers()` - "Should divide two numbers"
2. `shouldHandleDecimalDivision()` - "Should handle decimal division"
3. `shouldThrowExceptionWhenDividingByZero()` - "Should throw exception when dividing by zero"
   - Använd `assertThatThrownBy()` för att testa undantag

**Implementera `divide(int a, int b)` metod som returnerar `double`** ✓  
**Kasta `IllegalArgumentException` vid division med noll**

### ✅ Uppgift 7: Komplexa operationer (RED → GREEN)
**Mål:** Matematiska funktioner

**PHASE 5: Complex Operations** - Lägg till tester:
1. `shouldCalculatePowerOfNumber()` - "Should calculate power of a number"
2. `shouldHandlePowerOfZero()` - "Should handle power of zero"
3. `shouldCalculateSquareRoot()` - "Should calculate square root"
4. `shouldThrowExceptionForNegativeSquareRoot()` - "Should throw exception for negative square root"

**Implementera:**
- `power(double base, int exponent)` 
- `sqrt(double number)`

### ✅ Uppgift 8: Edge cases och gränsvärden (RED → GREEN)
**Mål:** Testa extremfall

**PHASE 6: Edge Cases and Boundary Values** - Lägg till tester:
1. `shouldHandleMaxIntegerValuesInAddition()` - "Should handle maximum integer values in addition"
2. `shouldHandleNegativeExponents()` - "Should handle negative exponents"
3. `shouldHandleZeroBaseWithPositiveExponent()` - "Should handle zero base with positive exponent"
4. `shouldHandleZeroSquared()` - "Should handle zero squared"

**Verifiera att edge cases hanteras korrekt** ✓

### ✅ Uppgift 9: Nya matematiska operationer (RED → GREEN)
**Mål:** Utöka Calculator med fler funktioner

**PHASE 7: New Mathematical Operations** - Lägg till tester:

**Fakultet:**
1. `shouldCalculateFactorialOfPositiveNumber()` - "Should calculate factorial of positive number"
2. `shouldReturn1ForFactorialOfZero()` - "Should return 1 for factorial of 0"
3. `shouldThrowExceptionForNegativeFactorial()` - "Should throw exception for negative factorial"

**Absolut värde:**
4. `shouldCalculateAbsoluteValueOfNegativeNumber()` - "Should calculate absolute value of negative number"
5. `shouldReturnSameValueForPositiveAbsoluteValue()` - "Should return same value for positive absolute value"
6. `shouldHandleAbsoluteValueOfZero()` - "Should handle absolute value of zero"

**Modulo:**
7. `shouldCalculateModuloOfTwoNumbers()` - "Should calculate modulo of two numbers"
8. `shouldThrowExceptionForModuloByZero()` - "Should throw exception for modulo by zero"

**GCD (Största gemensamma delare):**
9. `shouldCalculateGCDOfTwoPositiveNumbers()` - "Should calculate GCD of two positive numbers"
10. `shouldHandleGCDWhenOneNumberIsZero()` - "Should handle GCD when one number is zero"
11. `shouldCalculateGCDOfIdenticalNumbers()` - "Should calculate GCD of identical numbers"

**Implementera:**
- `factorial(int n)` - returnerar `long`
- `abs(int n)` - absolut värde
- `modulo(int a, int b)` - modulo operation
- `gcd(int a, int b)` - Euklides algoritm

### ✅ Uppgift 10: Refaktorera och dokumentera (REFACTOR)
**Mål:** Förbättra kodkvalitet

Fokus på:
- Lägg till JavaDoc-kommentarer för alla metoder
- Förbättra variabelnamn om nödvändigt
- Kontrollera att alla undantag har beskrivande meddelanden
- **Kör ALLA tester** efter refaktorering ✓

---

### 📋 Komplett teststruktur med DisplayNames

```
@DisplayName("Calculator TDD Demo")
class CalculatorTest {
    
    // PHASE 1: Basic Addition (RED -> GREEN -> REFACTOR)
    ├─ @DisplayName("Should add two positive numbers")
    ├─ @DisplayName("Should add zero to a number")
    └─ @DisplayName("Should add negative numbers")
    
    // PHASE 2: Subtraction (RED -> GREEN -> REFACTOR)
    ├─ @DisplayName("Should subtract two numbers")
    └─ @DisplayName("Should handle negative result in subtraction")
    
    // PHASE 3: Multiplication (RED -> GREEN -> REFACTOR)
    ├─ @DisplayName("Should multiply two numbers")
    └─ @DisplayName("Should return zero when multiplying by zero")
    
    // PHASE 4: Division with Error Handling (RED -> GREEN -> REFACTOR)
    ├─ @DisplayName("Should divide two numbers")
    ├─ @DisplayName("Should handle decimal division")
    └─ @DisplayName("Should throw exception when dividing by zero")
    
    // PHASE 5: Complex Operations (RED -> GREEN -> REFACTOR)
    ├─ @DisplayName("Should calculate power of a number")
    ├─ @DisplayName("Should handle power of zero")
    ├─ @DisplayName("Should calculate square root")
    └─ @DisplayName("Should throw exception for negative square root")
    
    // PHASE 6: Edge Cases and Boundary Values
    ├─ @DisplayName("Should handle maximum integer values in addition")
    ├─ @DisplayName("Should handle negative exponents")
    ├─ @DisplayName("Should handle zero base with positive exponent")
    └─ @DisplayName("Should handle zero squared")
    
    // PHASE 7: New Mathematical Operations (Following TDD)
    ├─ @DisplayName("Should calculate factorial of positive number")
    ├─ @DisplayName("Should return 1 for factorial of 0")
    ├─ @DisplayName("Should throw exception for negative factorial")
    ├─ @DisplayName("Should calculate absolute value of negative number")
    ├─ @DisplayName("Should return same value for positive absolute value")
    ├─ @DisplayName("Should handle absolute value of zero")
    ├─ @DisplayName("Should calculate modulo of two numbers")
    ├─ @DisplayName("Should throw exception for modulo by zero")
    ├─ @DisplayName("Should calculate GCD of two positive numbers")
    ├─ @DisplayName("Should handle GCD when one number is zero")
    └─ @DisplayName("Should calculate GCD of identical numbers")
}
```

### 🎯 Checklista för Lektion 1

#### Teststruktur (följer verklig implementation)
- [ ] **PHASE 1: Basic Addition** - 3 tester gröna
- [ ] **PHASE 2: Subtraction** - 2 tester gröna
- [ ] **PHASE 3: Multiplication** - 2 tester gröna
- [ ] **PHASE 4: Division with Error Handling** - 3 tester gröna (inklusive undantagstest)
- [ ] **PHASE 5: Complex Operations** - 4 tester gröna (power, sqrt)
- [ ] **PHASE 6: Edge Cases** - 4 tester gröna
- [ ] **PHASE 7: New Mathematical Operations** - 11 tester gröna

#### Implementerade metoder
- [ ] `add(int, int)` - Addition
- [ ] `subtract(int, int)` - Subtraktion
- [ ] `multiply(int, int)` - Multiplikation
- [ ] `divide(int, int)` - Division (returnerar double)
- [ ] `power(double, int)` - Upphöjt till
- [ ] `sqrt(double)` - Kvadratrot
- [ ] `factorial(int)` - Fakultet (returnerar long)
- [ ] `abs(int)` - Absolut värde
- [ ] `modulo(int, int)` - Modulo operation
- [ ] `gcd(int, int)` - Största gemensamma delare

#### Kodkvalitet
- [ ] Alla metoder har `@DisplayName` annotations
- [ ] Undantagshantering implementerad (IllegalArgumentException)
- [ ] Edge cases hanteras (noll, negativa tal, överskridning)
- [ ] JavaDoc-dokumentation på alla publika metoder
- [ ] Arrange-Act-Assert mönster följs i alla tester
- [ ] Beskrivande testnamn som börjar med "should"

**Kör alla tester:**
```bash
mvn test -Dtest=CalculatorTest
```

**Förväntat resultat:** 33 tester ska lyckas! ✅

---

### Steg 2: Ditt första TDD-test (RED fas)

Skapa `CalculatorTest.java` och skriv ditt första test:

```java
package com.example.tdd;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.DisplayName;
import static org.assertj.core.api.Assertions.*;

@DisplayName("Calculator TDD Lesson 1")
class CalculatorTest {
    
    private Calculator calculator;
    
    @BeforeEach
    void setUp() {
        calculator = new Calculator();  // Detta kommer att misslyckas först!
    }
    
    @Test
    @DisplayName("Should add two positive numbers")
    void shouldAddTwoPositiveNumbers() {
        // Arrange - Förbered testdata
        int a = 5;
        int b = 3;
        
        // Act - Utför handlingen som testas
        int result = calculator.add(a, b);  // Detta kommer också att misslyckas!
        
        // Assert - Kontrollera resultatet
        assertThat(result).isEqualTo(8);
    }
}
```

**Kör testet nu:**
```bash
mvn test -Dtest=CalculatorTest
```

**Resultat:** Testet misslyckas! ❌ Detta är förväntat och korrekt!

**Felen du ser:**
- `Calculator` klassen existerar inte
- `add` metoden existerar inte

**🎉 Grattis! Du är nu i RED-fasen!**

### Steg 3: Minimal implementation (GREEN fas)

Nu skapar vi `Calculator.java` med minimal kod för att få testet att lyckas:

```java
package com.example.tdd;

public class Calculator {
    
    public int add(int a, int b) {
        return 8;  // Hårdkodad! Men testet lyckas nu
    }
}
```

**Kör testet igen:**
```bash
mvn test -Dtest=CalculatorTest
```

**Resultat:** Testet lyckas! ✅

**🎉 Du är nu i GREEN-fasen!**

### Steg 4: Lägg till fler tester (tillbaka till RED)

Låt oss lägga till ett test som tvingar oss att skriva riktig logik:

```java
@Test
@DisplayName("Should add two different positive numbers")
void shouldAddTwoDifferentPositiveNumbers() {
    // Arrange
    int a = 10;
    int b = 7;
    
    // Act
    int result = calculator.add(a, b);
    
    // Assert
    assertThat(result).isEqualTo(17);
}
```

**Kör testet:**
```bash
mvn test -Dtest=CalculatorTest
```

**Resultat:** Ett test lyckas, ett misslyckas! ❌

**🔴 Vi är tillbaka i RED-fasen!**

### Steg 5: Riktig implementation (GREEN igen)

Nu måste vi skriva riktig logik:

```java
public class Calculator {
    
    public int add(int a, int b) {
        return a + b;  // Äkta implementation!
    }
}
```

**Kör alla tester:**
```bash
mvn test -Dtest=CalculatorTest
```

**Resultat:** Alla tester lyckas! ✅

**🎉 GREEN-fasen igen!**

### Steg 6: Refaktorering (REFACTOR fas)

Vår kod är redan ganska bra, men låt oss lägga till dokumentation:

```java
package com.example.tdd;

/**
 * En enkel kalkylator för grundläggande aritmetiska operationer.
 * Skapad genom Test-Driven Development.
 */
public class Calculator {
    
    /**
     * Adderar två heltal.
     * @param a första talet
     * @param b andra talet
     * @return summan av a och b
     */
    public int add(int a, int b) {
        return a + b;
    }
}
```

**Kör testerna för att säkerställa att vi inte brutit något:**
```bash
mvn test -Dtest=CalculatorTest
```

**🔵 REFACTOR-fasen klar!**

## 🚀 Fortsätt cykeln: Lägg till subtraktion

Nu fortsätter vi med nästa funktionalitet. **Kom ihåg:** Börja alltid med ett test!

### RED: Skriv subtraktionstestet först

```java
@Test
@DisplayName("Should subtract second number from first")
void shouldSubtractSecondNumberFromFirst() {
    // Arrange
    int a = 10;
    int b = 4;
    
    // Act
    int result = calculator.subtract(a, b);  // Metoden finns inte än!
    
    // Assert
    assertThat(result).isEqualTo(6);
}
```

### GREEN: Implementera subtract-metoden

```java
public int subtract(int a, int b) {
    return a - b;
}
```

### Lägg till fler edge-case tester

```java
@Test
@DisplayName("Should add zero correctly")
void shouldAddZeroCorrectly() {
    assertThat(calculator.add(5, 0)).isEqualTo(5);
    assertThat(calculator.add(0, 5)).isEqualTo(5);
}

@Test
@DisplayName("Should handle negative numbers in addition")
void shouldHandleNegativeNumbersInAddition() {
    assertThat(calculator.add(-5, 3)).isEqualTo(-2);
    assertThat(calculator.add(-5, -3)).isEqualTo(-8);
}

@Test
@DisplayName("Should handle negative result in subtraction")
void shouldHandleNegativeResultInSubtraction() {
    assertThat(calculator.subtract(3, 8)).isEqualTo(-5);
}
```

## 🧪 Komplett första lektion: Calculator med grundoperationer

Här är din kompletta `CalculatorTest.java` 

```java
package com.example.tdd;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.DisplayName;
import static org.assertj.core.api.Assertions.*;

@DisplayName("Calculator TDD Lesson 1")
class CalculatorTest {
    
    private Calculator calculator;
    
    @BeforeEach
    void setUp() {
        calculator = new Calculator();
    }
    
    // ===== ADDITION TESTS =====
    
    @Test
    @DisplayName("Should add two positive numbers")
    void shouldAddTwoPositiveNumbers() {
        // Arrange
        int a = 5;
        int b = 3;
        
        // Act
        int result = calculator.add(a, b);
        
        // Assert
        assertThat(result).isEqualTo(8);
    }
    
    @Test
    @DisplayName("Should add two different positive numbers")
    void shouldAddTwoDifferentPositiveNumbers() {
        assertThat(calculator.add(10, 7)).isEqualTo(17);
    }
    
    @Test
    @DisplayName("Should add zero correctly")
    void shouldAddZeroCorrectly() {
        assertThat(calculator.add(5, 0)).isEqualTo(5);
        assertThat(calculator.add(0, 5)).isEqualTo(5);
    }
    
    @Test
    @DisplayName("Should handle negative numbers in addition")
    void shouldHandleNegativeNumbersInAddition() {
        assertThat(calculator.add(-5, 3)).isEqualTo(-2);
        assertThat(calculator.add(-5, -3)).isEqualTo(-8);
    }
    
    // ===== SUBTRACTION TESTS =====
    
    @Test
    @DisplayName("Should subtract second number from first")
    void shouldSubtractSecondNumberFromFirst() {
        assertThat(calculator.subtract(10, 4)).isEqualTo(6);
    }
    
    @Test
    @DisplayName("Should handle negative result in subtraction")
    void shouldHandleNegativeResultInSubtraction() {
        assertThat(calculator.subtract(3, 8)).isEqualTo(-5);
    }
    
    @Test
    @DisplayName("Should subtract zero correctly")
    void shouldSubtractZeroCorrectly() {
        assertThat(calculator.subtract(10, 0)).isEqualTo(10);
    }
    
    @Test
    @DisplayName("Should subtract negative numbers")
    void shouldSubtractNegativeNumbers() {
        assertThat(calculator.subtract(5, -3)).isEqualTo(8);
        assertThat(calculator.subtract(-5, -3)).isEqualTo(-2);
    }
}
```

Och din kompletta `Calculator.java`:

```java
package com.example.tdd;

/**
 * En enkel kalkylator för grundläggande aritmetiska operationer.
 * Skapad genom Test-Driven Development.
 */
public class Calculator {
    
    /**
     * Adderar två heltal.
     * @param a första talet
     * @param b andra talet  
     * @return summan av a och b
     */
    public int add(int a, int b) {
        return a + b;
    }
    
    /**
     * Subtraherar andra talet från första talet.
     * @param a första talet
     * @param b andra talet att subtrahera
     * @return skillnaden mellan a och b
     */
    public int subtract(int a, int b) {
        return a - b;
    }
}
```

## 🏃‍♂️ Kör dina tester

```bash
# Kör alla Calculator-tester
mvn test -Dtest=CalculatorTest

# Kompilera och kör alla tester
mvn clean test
```

Du bör se alla 8 tester lyckas! ✅

## 🎓 Vad har du lärt dig?

### ✅ TDD-grundprinciper
- **Test först**: Du skrev alltid testet innan koden
- **Minimal implementation**: Du skrev bara tillräckligt för att få testet att lyckas
- **Iterativ utveckling**: Du byggde funktionalitet steg för steg
- **Refaktorering**: Du förbättrade koden medan testerna höll den säker

### ✅ Testskrivande
- **Arrange-Act-Assert** mönster
- **Beskrivande testnamn** med `@DisplayName`
- **Edge case-testning** (noll, negativa tal)
- **Enkel assertion** per test

### ✅ TDD-fördelar du redan ser
- **Förtroende**: Du vet att din kod fungerar
- **Design**: Testerna tvingade fram en ren API
- **Dokumentation**: Testerna visar hur koden ska användas
- **Säkerhet**: Du kan ändra kod utan rädsla

## 🚀 Nästa steg

Grattis! Du har framgångsrikt slutfört din första TDD-lektion. Du har:

- ✅ Lärt dig RED-GREEN-REFACTOR cykeln
- ✅ Skrivit 8 fungerade tester
- ✅ Implementerat en Calculator med addition och subtraktion
- ✅ Förstått varför vi skriver tester först

### 🎯 Hemuppgifter

Innan nästa lektion, försök att lägga till **multiplikation** till din Calculator:

1. **RED**: Skriv ett test för `multiply(int a, int b)`
2. **GREEN**: Implementera metoden för att få testet att lyckas
3. **REFACTOR**: Förbättra koden om nödvändigt
4. Lägg till edge-case tester (multiplikation med noll, negativa tal)

### 📚 Nästa lektion

I **Lektion 2** kommer vi att utforska avancerade TDD-mönster med StringProcessor-klassen, inklusive:
- Nästlade testklasser
- Parametriserade tester  
- Undantagshantering
- Komplexa edge cases

---
