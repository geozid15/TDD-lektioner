# Lektion 2: Avancerade TDD-mönster med StringProcessor
## Från grundläggande TDD till professionella teststrategier

I denna lektion bygger vi en StringProcessor-klass som hanterar komplex strängmanipulation.

## 🎯 Vad kommer du att lära dig i Lektion 2?

- **Nästlade testklasser** för bättre organisation
- **Parametriserade tester** för att testa många scenarier effektivt
- **Undantagshantering** i TDD
- **Edge case-testning** med null, tomma strängar och speciella tecken
- **Komplexa algoritmer** genom TDD (palindromer, komprimering)
- **Valideringslogik** med regex-mönster

## 📚 Förutsättningar

Du bör ha slutfört **Lektion 0 & 1** och förstå:
- ✅ RED-GREEN-REFACTOR cykeln
- ✅ Arrange-Act-Assert mönster
- ✅ Grundläggande test-skrivande med JUnit 5

## 🏗️ Projektstruktur för Lektion 2

```
TDD/
├── src/
│   ├── main/java/com/example/tdd/
│   │   ├── Calculator.java          # Från Lektion 1
│   │   └── StringProcessor.java     # Ny klass vi bygger
│   └── test/java/com/example/tdd/
│       ├── CalculatorTest.java      # Från Lektion 1  
│       └── StringProcessorTest.java # Ny testklass
└── pom.xml
```

## 📝 Kom igång: Initiala uppgifter

Innan du börjar koda, här är en översikt över uppgifterna du kommer att genomföra i denna lektion. Följ RED-GREEN-REFACTOR cykeln för varje steg!

### ✅ Uppgift 1: Skapa StringProcessorTest med första testet (RED)
**Mål:** Starta med TDD - skriv testet INNAN implementationen  
**Fil:** `src/test/java/com/example/tdd/StringProcessorTest.java`

Skapa testet med:
1. `@DisplayName("StringProcessor TDD Demo")` på klassnivå
2. `@BeforeEach` setup med `StringProcessor`
3. Nästlad klass `@DisplayName("String Reversal Tests")`
4. Första testet:
   - Metod: `shouldReverseSimpleString()`
   - `@DisplayName("Should reverse a simple string")`
5. **Kör testet** - det ska MISSLYCKAS (RED fas) ✗

### ✅ Uppgift 2: Skapa StringProcessor klass (GREEN)
**Mål:** Minimal implementation för att få testet att lyckas  
**Fil:** `src/main/java/com/example/tdd/StringProcessor.java`

Implementera:
- Grundläggande klasstruktur
- `reverse(String input)` metod
- Hårdkoda först för att få testet grönt
- **Kör testet igen** - det ska NU LYCKAS (GREEN fas) ✓

### ✅ Uppgift 3: Lägg till fler reverse-tester (RED → GREEN)
**Mål:** Driva fram riktig implementation

Lägg till i `StringReversalTests`:
1. **Test 2:** `shouldHandleSingleCharacter()`
   - `@DisplayName("Should handle single character")`
2. **Test 3:** `shouldHandleNullAndEmptyStrings(String input)`
   - `@DisplayName("Should handle null and empty strings")`
   - `@ParameterizedTest` med `@NullAndEmptySource`
3. **Test 4:** `shouldPreserveSpacesInReversal()`
   - `@DisplayName("Should preserve spaces in reversal")`

**Implementera sedan riktig `reverse()` metod med StringBuilder** ✓

### ✅ Uppgift 4: Palindromdetektering (RED → GREEN → REFACTOR)
**Mål:** Implementera komplex algoritm genom TDD

**Skapa nästlad klass:** `@DisplayName("Palindrome Detection Tests")`

Implementera tester:
1. `shouldDetectSimplePalindrome()` - "Should detect simple palindrome"
2. `shouldDetectNonPalindrome()` - "Should detect non-palindrome"
3. `shouldHandleCaseInsensitivePalindromes()` - "Should handle case-insensitive palindromes"
4. `shouldHandlePalindromesWithSpaces()` - "Should handle palindromes with spaces"
5. `shouldHandleSingleCharacterAsPalindrome()` - "Should handle single character as palindrome"
6. `shouldHandleNullAndEmptyAsPalindromes(String)` - "Should handle null and empty as palindromes" (parametriserad)

**Implementera `isPalindrome(String)` metod** ✓

### ✅ Uppgift 5: Strängkomprimering (RED → GREEN)
**Mål:** Algoritmisk komplexitet genom TDD

**Skapa nästlad klass:** `@DisplayName("String Compression Tests")`

Implementera tester:
1. `shouldCompressRepeatedCharacters()` - "Should compress repeated characters"
2. `shouldReturnOriginalIfCompressionDoesntReduceLength()` - "Should return original if compression doesn't reduce length"
3. `shouldHandleSingleCharacterCompression()` - "Should handle single character"
4. `shouldHandleNullAndEmptyStringsInCompression(String)` - "Should handle null and empty strings in compression" (parametriserad)

**Implementera `compress(String)` metod** ✓

### ✅ Uppgift 6: Ordräkning (RED → GREEN)
**Mål:** Grundläggande stränganalys

**Skapa nästlad klass:** `@DisplayName("Word Count Tests")`

Implementera tester:
1. `shouldCountWordsInSimpleSentence()` - "Should count words in simple sentence"
2. `shouldHandleMultipleSpacesBetweenWords()` - "Should handle multiple spaces between words"
3. `shouldHandleLeadingAndTrailingSpaces()` - "Should handle leading and trailing spaces"
4. `shouldCountSingleWord()` - "Should count single word"
5. `shouldReturnZeroForNullEmptyOrWhitespaceOnlyStrings(String)` - "Should return zero for null, empty, or whitespace-only strings" (parametriserad med `@NullAndEmptySource` och `@ValueSource`)

**Implementera `countWords(String)` metod** ✓

### ✅ Uppgift 7: Input-validering (RED → GREEN)
**Mål:** Regex-baserad validering

**Skapa nästlad klass:** `@DisplayName("Input Validation Tests")`

Implementera tester:
1. `shouldValidateEmailFormat()` - "Should validate email format"
2. `shouldRejectInvalidEmailFormats()` - "Should reject invalid email formats"
3. `shouldRejectNullAndEmptyEmails(String)` - "Should reject null and empty emails" (parametriserad)

**Implementera `isValidEmail(String)` med regex Pattern** ✓

### ✅ Uppgift 8: Strängmanipulering (RED → GREEN)
**Mål:** Avancerade strängoperationer

**Skapa nästlad klass:** `@DisplayName("String Manipulation Tests")`

Implementera tester för:
- **Kapitalisering:** `capitalizeWords(String)`
- **Substring-sökning:** `containsSubstring(String, String)`
- **Dubblettborttagning:** `removeDuplicates(String)`

Totalt 11 tester i denna klass.

### ✅ Uppgift 9: Avancerad validering (RED → GREEN)
**Mål:** Flera valideringsmönster

**Skapa nästlad klass:** `@DisplayName("Advanced Validation Tests")`

Implementera:
- **Telefonnummer:** `isValidPhoneNumber(String)`
- **URL:** `isValidURL(String)`

### ✅ Uppgift 10: Prestanda och edge cases (RED → GREEN)
**Mål:** Säkerställ robusthet

**Skapa nästlad klass:** `@DisplayName("Performance and Edge Case Tests")`

Testa:
- Långa strängar (10,000 tecken)
- Specialtecken och Unicode
- Olika whitespace-typer
- Performance för stora palindromer

---

### 📋 Komplett teststruktur med DisplayNames

```
@DisplayName("StringProcessor TDD Demo")
class StringProcessorTest {
    
    @Nested
    @DisplayName("String Reversal Tests")
    class StringReversalTests {
        ├─ @DisplayName("Should reverse a simple string")
        ├─ @DisplayName("Should handle single character")
        ├─ @DisplayName("Should handle null and empty strings")
        └─ @DisplayName("Should preserve spaces in reversal")
    }
    
    @Nested
    @DisplayName("Palindrome Detection Tests")
    class PalindromeDetectionTests {
        ├─ @DisplayName("Should detect simple palindrome")
        ├─ @DisplayName("Should detect non-palindrome")
        ├─ @DisplayName("Should handle case-insensitive palindromes")
        ├─ @DisplayName("Should handle palindromes with spaces")
        ├─ @DisplayName("Should handle single character as palindrome")
        └─ @DisplayName("Should handle null and empty as palindromes")
    }
    
    @Nested
    @DisplayName("String Compression Tests")
    class StringCompressionTests {
        ├─ @DisplayName("Should compress repeated characters")
        ├─ @DisplayName("Should return original if compression doesn't reduce length")
        ├─ @DisplayName("Should handle single character")
        └─ @DisplayName("Should handle null and empty strings in compression")
    }
    
    @Nested
    @DisplayName("Word Count Tests")
    class WordCountTests {
        ├─ @DisplayName("Should count words in simple sentence")
        ├─ @DisplayName("Should handle multiple spaces between words")
        ├─ @DisplayName("Should handle leading and trailing spaces")
        ├─ @DisplayName("Should count single word")
        └─ @DisplayName("Should return zero for null, empty, or whitespace-only strings")
    }
    
    @Nested
    @DisplayName("Input Validation Tests")
    class InputValidationTests {
        ├─ @DisplayName("Should validate email format")
        ├─ @DisplayName("Should reject invalid email formats")
        └─ @DisplayName("Should reject null and empty emails")
    }
    
    @Nested
    @DisplayName("String Manipulation Tests")
    class StringManipulationTests {
        ├─ @DisplayName("Should capitalize first letter of each word")
        ├─ @DisplayName("Should handle single word capitalization")
        ├─ @DisplayName("Should handle mixed case input in capitalization")
        ├─ @DisplayName("Should handle null and empty strings in capitalization")
        ├─ @DisplayName("Should check if string contains substring")
        ├─ @DisplayName("Should check case-insensitive substring")
        ├─ @DisplayName("Should return false for non-existent substring")
        ├─ @DisplayName("Should remove duplicate characters")
        ├─ @DisplayName("Should handle string with no duplicates")
        ├─ @DisplayName("Should preserve order when removing duplicates")
        └─ @DisplayName("Should handle null and empty strings in duplicate removal")
    }
    
    @Nested
    @DisplayName("Advanced Validation Tests")
    class AdvancedValidationTests {
        ├─ @DisplayName("Should validate US phone number format")
        ├─ @DisplayName("Should reject invalid phone number formats")
        ├─ @DisplayName("Should validate URL format")
        ├─ @DisplayName("Should reject invalid URL formats")
        └─ @DisplayName("Should reject null and empty validation inputs")
    }
    
    @Nested
    @DisplayName("Performance and Edge Case Tests")
    class PerformanceAndEdgeCaseTests {
        ├─ @DisplayName("Should handle very long strings in reversal")
        ├─ @DisplayName("Should handle strings with special characters")
        ├─ @DisplayName("Should handle unicode characters")
        ├─ @DisplayName("Should handle very long palindrome check")
        └─ @DisplayName("Should handle word count with various whitespace")
    }
}
```

### 🎯 Checklista för Lektion 2

#### Teststruktur (följer verklig implementation)
- [ ] **StringReversalTests** - 4 tester gröna
- [ ] **PalindromeDetectionTests** - 6 tester gröna
- [ ] **StringCompressionTests** - 4 tester gröna
- [ ] **WordCountTests** - 5 tester gröna
- [ ] **InputValidationTests** - 3 tester gröna
- [ ] **StringManipulationTests** - 11 tester gröna
- [ ] **AdvancedValidationTests** - 5 tester gröna
- [ ] **PerformanceAndEdgeCaseTests** - 5 tester gröna

#### Implementerade metoder
- [ ] `reverse(String)` - Vänder en sträng
- [ ] `isPalindrome(String)` - Detekterar palindrom
- [ ] `compress(String)` - Komprimerar repeterade tecken
- [ ] `countWords(String)` - Räknar ord
- [ ] `isValidEmail(String)` - Validerar email
- [ ] `capitalizeWords(String)` - Kapitaliserar ord
- [ ] `containsSubstring(String, String)` - Söker substring
- [ ] `removeDuplicates(String)` - Tar bort dubbletter
- [ ] `isValidPhoneNumber(String)` - Validerar telefonnummer
- [ ] `isValidURL(String)` - Validerar URL

#### Kodkvalitet
- [ ] Alla metoder har tydliga namn och `@DisplayName`
- [ ] Parametriserade tester används för edge cases
- [ ] Null/Empty-hantering i alla metoder
- [ ] Regex-patterns är korrekt implementerade
- [ ] Ingen duplicerad kod
- [ ] Edge cases (unicode, långa strängar) hanteras

**Kör alla tester:**
```bash
mvn test -Dtest=StringProcessorTest
```

**Förväntat resultat:** 43+ tester ska lyckas! ✅

---

## 🚀 Del 1: Nästlade testklasser för organisation

När vi arbetar med komplexa klasser med många metoder, blir våra testklasser stora. Nästlade klasser hjälper oss att organisera!

### Steg 1: Grundläggande struktur (RED)

Skapa `StringProcessorTest.java`:

```java
package com.example.tdd;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import static org.assertj.core.api.Assertions.*;

@DisplayName("StringProcessor Advanced TDD Patterns")
class StringProcessorTest {
    
    private StringProcessor processor;
    
    @BeforeEach
    void setUp() {
        processor = new StringProcessor();
    }
    
    @Nested
    @DisplayName("String Reversal Tests")
    class StringReversalTests {
        
        @Test
        @DisplayName("Should reverse a simple string")
        void shouldReverseSimpleString() {
            // Arrange
            String input = "hello";
            
            // Act
            String result = processor.reverse(input);
            
            // Assert
            assertThat(result).isEqualTo("olleh");
        }
    }
}
```

**Kör testet - det ska misslyckas (RED fas):**
```bash
mvn test -Dtest=StringProcessorTest
```

### Steg 2: Minimal implementation (GREEN)

Skapa `StringProcessor.java`:

```java
package com.example.tdd;

public class StringProcessor {
    
    public String reverse(String input) {
        return "olleh";  // Hårdkodat för att få testet att lyckas
    }
}
```

**Testet ska nu lyckas!**

### Steg 3: Lägg till fler tester för reverse (RED igen)

```java
@Nested
@DisplayName("String Reversal Tests")
class StringReversalTests {
    
    @Test
    @DisplayName("Should reverse a simple string")
    void shouldReverseSimpleString() {
        String result = processor.reverse("hello");
        assertThat(result).isEqualTo("olleh");
    }
    
    @Test
    @DisplayName("Should reverse a different string")
    void shouldReverseADifferentString() {
        String result = processor.reverse("world");
        assertThat(result).isEqualTo("dlrow");
    }
    
    @Test
    @DisplayName("Should handle single character")
    void shouldHandleSingleCharacter() {
        String result = processor.reverse("a");
        assertThat(result).isEqualTo("a");
    }
}
```

### Steg 4: Riktig implementation (GREEN)

```java
public String reverse(String input) {
    if (input == null || input.isEmpty()) {
        return "";
    }
    return new StringBuilder(input).reverse().toString();
}
```

## 🧪 Del 2: Parametriserade tester

Parametriserade tester låter oss köra samma testlogik med olika data - perfekt för edge cases!

### Lägg till parametriserade tester

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;
import org.junit.jupiter.params.provider.NullAndEmptySource;

@Nested
@DisplayName("String Reversal Tests")
class StringReversalTests {
    
    // ... tidigare tester ...
    
    @ParameterizedTest
    @NullAndEmptySource
    @DisplayName("Should handle null and empty strings")
    void shouldHandleNullAndEmptyStrings(String input) {
        String result = processor.reverse(input);
        assertThat(result).isEmpty();
    }
    
    @ParameterizedTest
    @ValueSource(strings = {"a", "ab", "abc", "racecar"})
    @DisplayName("Should reverse various string lengths")
    void shouldReverseVariousStringLengths(String input) {
        String result = processor.reverse(input);
        String doubleReversed = processor.reverse(result);
        
        // Dubbel reversering ska ge ursprunglig sträng
        assertThat(doubleReversed).isEqualTo(input);
    }
}
```

## 🔍 Del 3: Komplex algoritm - Palindromdetektering

Nu implementerar vi en komplicerad algoritm genom TDD!

### Steg 1: Palindrom-tester (RED)

```java
@Nested
@DisplayName("Palindrome Detection Tests")
class PalindromeDetectionTests {
    
    @Test
    @DisplayName("Should detect simple palindrome")
    void shouldDetectSimplePalindrome() {
        boolean result = processor.isPalindrome("racecar");
        assertThat(result).isTrue();
    }
    
    @Test
    @DisplayName("Should detect non-palindrome")
    void shouldDetectNonPalindrome() {
        boolean result = processor.isPalindrome("hello");
        assertThat(result).isFalse();
    }
    
    @Test
    @DisplayName("Should handle case-insensitive palindromes")
    void shouldHandleCaseInsensitivePalindromes() {
        boolean result = processor.isPalindrome("RaceCar");
        assertThat(result).isTrue();
    }
    
    @Test
    @DisplayName("Should handle palindromes with spaces")
    void shouldHandlePalindromesWithSpaces() {
        boolean result = processor.isPalindrome("A man a plan a canal Panama");
        assertThat(result).isTrue();
    }
    
    @ParameterizedTest
    @NullAndEmptySource
    @DisplayName("Should handle null and empty as palindromes")
    void shouldHandleNullAndEmptyAsPalindromes(String input) {
        boolean result = processor.isPalindrome(input);
        assertThat(result).isTrue();
    }
}
```

### Steg 2: Palindrom-implementation (GREEN)

```java
public boolean isPalindrome(String input) {
    if (input == null || input.isEmpty()) {
        return true;
    }
    
    // Ta bort mellanslag och konvertera till små bokstäver
    String cleaned = input.replaceAll("\\s+", "").toLowerCase();
    String reversed = new StringBuilder(cleaned).reverse().toString();
    
    return cleaned.equals(reversed);
}
```

## 🗜️ Del 4: Algoritmisk komplexitet - Strängkomprimering

Låt oss implementera en algoritm som komprimerar strängar!

### Steg 1: Komprimeringstester (RED)

```java
@Nested
@DisplayName("String Compression Tests")
class StringCompressionTests {
    
    @Test
    @DisplayName("Should compress repeated characters")
    void shouldCompressRepeatedCharacters() {
        String result = processor.compress("aabcccccaaa");
        assertThat(result).isEqualTo("a2b1c5a3");
    }
    
    @Test
    @DisplayName("Should return original if compression doesn't reduce length")
    void shouldReturnOriginalIfCompressionDoesntReduceLength() {
        String result = processor.compress("abcdef");
        assertThat(result).isEqualTo("abcdef");
    }
    
    @Test
    @DisplayName("Should handle single character")
    void shouldHandleSingleCharacterCompression() {
        String result = processor.compress("a");
        assertThat(result).isEqualTo("a");
    }
    
    @ParameterizedTest
    @NullAndEmptySource
    @DisplayName("Should handle null and empty strings in compression")
    void shouldHandleNullAndEmptyStringsInCompression(String input) {
        String result = processor.compress(input);
        assertThat(result).isEqualTo(input == null ? "" : input);
    }
}
```

### Steg 2: Komprimerings-implementation (GREEN)

```java
public String compress(String input) {
    if (input == null) {
        return "";
    }
    
    if (input.isEmpty() || input.length() <= 1) {
        return input;
    }
    
    StringBuilder compressed = new StringBuilder();
    int count = 1;
    char previous = input.charAt(0);
    
    for (int i = 1; i < input.length(); i++) {
        char current = input.charAt(i);
        if (current == previous) {
            count++;
        } else {
            compressed.append(previous).append(count);
            previous = current;
            count = 1;
        }
    }
    
    // Glöm inte sista gruppen
    compressed.append(previous).append(count);
    
    // Returnera original om komprimering inte minskar längden
    return compressed.length() < input.length() ? compressed.toString() : input;
}
```

## 📊 Del 5: Undantagshantering och validering

### Email-validering med undantagshantering

```java
@Nested
@DisplayName("Input Validation Tests")
class InputValidationTests {
    
    @Test
    @DisplayName("Should validate correct email format")
    void shouldValidateCorrectEmailFormat() {
        boolean result = processor.isValidEmail("test@example.com");
        assertThat(result).isTrue();
    }
    
    @Test
    @DisplayName("Should reject invalid email formats")
    void shouldRejectInvalidEmailFormats() {
        assertThat(processor.isValidEmail("invalid-email")).isFalse();
        assertThat(processor.isValidEmail("@example.com")).isFalse();
        assertThat(processor.isValidEmail("test@")).isFalse();
        assertThat(processor.isValidEmail("test.example.com")).isFalse();
    }
    
    @ParameterizedTest
    @NullAndEmptySource
    @DisplayName("Should reject null and empty emails")
    void shouldRejectNullAndEmptyEmails(String email) {
        boolean result = processor.isValidEmail(email);
        assertThat(result).isFalse();
    }
}
```

### Implementation med regex

```java
import java.util.regex.Pattern;

public class StringProcessor {
    
    private static final Pattern EMAIL_PATTERN = Pattern.compile(
        "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$"
    );
    
    // ... andra metoder ...
    
    public boolean isValidEmail(String email) {
        if (email == null || email.isEmpty()) {
            return false;
        }
        return EMAIL_PATTERN.matcher(email).matches();
    }
}
```

## 🧮 Del 6: Integration med Calculator

Låt oss använda vår Calculator från Lektion 1 i StringProcessor!

### Ordräkning med matematisk validering

```java
@Nested
@DisplayName("Word Count Tests")
class WordCountTests {
    
    @Test
    @DisplayName("Should count words in simple sentence")
    void shouldCountWordsInSimpleSentence() {
        int count = processor.countWords("hello world java");
        assertThat(count).isEqualTo(3);
    }
    
    @Test
    @DisplayName("Should handle multiple spaces between words")
    void shouldHandleMultipleSpacesBetweenWords() {
        int count = processor.countWords("hello    world     java");
        assertThat(count).isEqualTo(3);
    }
    
    @ParameterizedTest
    @NullAndEmptySource
    @ValueSource(strings = {"   ", "\t", "\n"})
    @DisplayName("Should return zero for null, empty, or whitespace-only strings")
    void shouldReturnZeroForNullEmptyOrWhitespaceOnlyStrings(String input) {
        int count = processor.countWords(input);
        assertThat(count).isZero();
    }
}
```

### Implementation med Calculator-integration

```java
public class StringProcessor {
    
    private final Calculator calculator;
    
    public StringProcessor() {
        this.calculator = new Calculator();
    }
    
    public int countWords(String input) {
        if (input == null || input.trim().isEmpty()) {
            return 0;
        }
        
        String[] words = input.trim().split("\\s+");
        return words.length;
    }
    
    // Använd Calculator för genomsnittlig ordlängd
    public double averageWordLength(String input) {
        if (input == null || input.trim().isEmpty()) {
            return 0.0;
        }
        
        String[] words = input.trim().split("\\s+");
        int totalCharacters = 0;
        
        for (String word : words) {
            totalCharacters = calculator.add(totalCharacters, word.length());
        }
        
        return calculator.divide(totalCharacters, words.length);
    }
}
```

## 📋 Komplett StringProcessorTest.java struktur

```java
package com.example.tdd;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;
import org.junit.jupiter.params.provider.NullAndEmptySource;
import static org.assertj.core.api.Assertions.*;

@DisplayName("StringProcessor Advanced TDD Patterns")
class StringProcessorTest {
    
    private StringProcessor processor;
    
    @BeforeEach
    void setUp() {
        processor = new StringProcessor();
    }
    
    @Nested
    @DisplayName("String Reversal Tests")
    class StringReversalTests {
        // Alla reversal-tester här
    }
    
    @Nested
    @DisplayName("Palindrome Detection Tests")
    class PalindromeDetectionTests {
        // Alla palindrom-tester här
    }
    
    @Nested
    @DisplayName("String Compression Tests")
    class StringCompressionTests {
        // Alla komprimerings-tester här
    }
    
    @Nested
    @DisplayName("Word Count Tests")
    class WordCountTests {
        // Alla ordräknings-tester här
    }
    
    @Nested
    @DisplayName("Input Validation Tests")
    class InputValidationTests {
        // Alla validerings-tester här
    }
}
```

## 🏃‍♂️ Kör alla tester

```bash
# Kör endast StringProcessor-tester
mvn test -Dtest=StringProcessorTest

# Kör alla tester från båda lektionerna
mvn test

# Kör med detaljerad utdata
mvn test -Dtest=StringProcessorTest -Dsurefire.reportFormat=plain
```

## 🎓 Vad har du lärt dig i Lektion 2?

### ✅ Avancerade testorganisation
- **Nästlade testklasser** (`@Nested`) för logisk gruppering
- **Beskrivande gruppnamn** för bättre förståelse
- **Separation av concerns** - varje grupp fokuserar på en funktionalitet

### ✅ Parametriserade tester
- **`@ParameterizedTest`** för att testa många scenarier
- **`@NullAndEmptySource`** för edge case-testning
- **`@ValueSource`** för att testa olika värden
- **Effektiv testning** av boundary conditions

### ✅ Komplex algoritmutveckling
- **Steg-för-steg implementation** av palindromdetektering
- **Algoritmisk tänkning** genom TDD
- **Edge case-hantering** för komplexa scenarier

### ✅ Regex och validering
- **Pattern-matchning** för email-validering
- **Input sanitization** och validering
- **Professionella valideringsmönster**

### ✅ Integration mellan klasser
- **Dependency injection** med Calculator
- **Composite functionality** - använd en klass i en annan
- **Cross-class testing** strategier

## 🚀 Hemuppgifter

Innan nästa lektion, försök att implementera dessa funktioner med TDD:

### 🎯 Uppgift 1: Kapitalisering
Implementera `capitalizeWords(String input)` som gör första bokstaven i varje ord stor:
- "hello world" → "Hello World"
- "test" → "Test"
- null/empty → ""

### 🎯 Uppgift 2: Substring-sökning
Implementera `containsSubstring(String text, String substring)`:
- Case-insensitive sökning
- Hantera null-värden
- Returnera boolean

### 🎯 Uppgift 3: Dubblettborttagning
Implementera `removeDuplicates(String input)`:
- "programming" → "progamin"
- Bevara ordning
- Hantera edge cases

### 📝 TDD-proces för hemuppgifterna:
1. **RED**: Skriv testet först
2. **GREEN**: Minimal implementation
3. **REFACTOR**: Förbättra koden
4. **Repetera**: Lägg till fler tester och edge cases

## 📚 Nästa lektion förhandsvisning

I **Lektion 3** kommer vi att utforska:
- **Performance-testning** inom TDD
- **Complex data structures** och algoritmer
- **Sentimentanalys** och NLP-algoritmer
- **Statistical analysis** genom TDD
- **Integration testing** strategier

---

**🎉 Utmärkt arbete!** Du behärskar nu avancerade TDD-mönster och kan bygga komplexa algoritmer med confidence. Din testorganisation är professionell och dina algoritmer är robusta!

**Kom ihåg**: TDD handlar inte bara om att testa kod - det handlar om att **designa** kod som är lätt att använda, förstå och underhålla. Genom att skriva tester först tvingar du dig själv att tänka som användare av din kod! 🚀
