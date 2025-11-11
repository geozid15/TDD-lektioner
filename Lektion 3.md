# Lektion 3: Komplexa algoritmer och avancerad TDD
## TextAnalyzer - Sentimentanalys, läsbarhet och statistisk analys

Nu när du behärskar grundläggande TDD och avancerade testmönster, är det dags att tackla riktigt komplexa algoritmer. I denna lektion bygger vi en TextAnalyzer med sofistikerade funktioner som sentimentanalys och läsbarhetsmätning.

## 🎯 Vad kommer du att lära dig i Lektion 3?

- **Komplexa algoritmer** genom TDD (Flesch Reading Ease, Jaccard-likhet)
- **Dependency Injection** i testning utan mock-ramverk
- **Statistisk validering** med toleransintervall
- **Prestandatestning** integrerat i TDD-cykeln
- **Multi-step algoritmer** för sentimentanalys
- **Matematiska formler** implementerade test-först
- **Batch-processing** med felresistens

## 📚 Förutsättningar

Du bör ha slutfört **Lektion 0, 1 & 2** och förstå:
- ✅ RED-GREEN-REFACTOR cykeln grundligt
- ✅ Nästlade testklasser och organisation
- ✅ Parametriserade tester och edge cases
- ✅ Integration mellan klasser (Calculator + StringProcessor)

## 🧠 Varför är denna lektion viktig?

TextAnalyzer representerar **verklig mjukvaruutveckling**:
- **Komplexa affärsregler** som kräver noggrann testning
- **Matematiska algoritmer** som är svåra att få rätt
- **Performance-krav** som måste valideras
- **Integration mellan system** som måste fungera tillsammans

## 🏗️ Projektstruktur för Lektion 3

```
TDD/
├── src/
│   ├── main/java/com/example/tdd/
│   │   ├── Calculator.java              # Från Lektion 1
│   │   ├── StringProcessor.java         # Från Lektion 2
│   │   ├── TextAnalyzer.java           # Ny komplex klass ⭐
│   │   ├── SentimentResult.java        # Resultatklasser ⭐
│   │   ├── ReadabilityResult.java      # för komplexa ⭐
│   │   ├── TextStatistics.java         # datastrukturer ⭐
│   │   └── ...fler supportklasser
│   └── test/java/com/example/tdd/
│       ├── CalculatorTest.java          # Från Lektion 1  
│       ├── StringProcessorTest.java     # Från Lektion 2
│       └── TextAnalyzerTest.java        # Ny testklass ⭐
└── pom.xml
```

## 📝 Kom igång: Initiala uppgifter


### ✅ Uppgift 1: Skapa SentimentCategory enum (Setup)
**Mål:** Definiera de möjliga sentimentkategorierna  
**Fil:** `src/main/java/com/example/tdd/SentimentCategory.java`

```java
public enum SentimentCategory {
    POSITIVE,    // Positiv text
    NEGATIVE,    // Negativ text
    NEUTRAL      // Neutral text
}
```

### ✅ Uppgift 2: Skapa SentimentResult klass (Setup)
**Mål:** Datastruktur för att returnera sentimentanalysresultat  
**Fil:** `src/main/java/com/example/tdd/SentimentResult.java`

Klassen ska innehålla:
- `sentimentScore` (double) - numeriskt poäng
- `sentimentCategory` (SentimentCategory) - kategorisering
- `positiveWordCount` (int) - antal positiva ord
- `negativeWordCount` (int) - antal negativa ord
- Alla getters och en konstruktor

### ✅ Uppgift 3: Skapa TextAnalyzerTest med första testet (RED)
**Mål:** TDD börjar här! Skriv testet INNAN implementationen  
**Fil:** `src/test/java/com/example/tdd/TextAnalyzerTest.java`

Testet ska innehålla:
1. `@DisplayName("Complex TextAnalyzer TDD Demo")` på klassnivå
2. `@BeforeEach` setup med Calculator och StringProcessor dependencies
3. Nästlad klass `@DisplayName("Sentiment Analysis Tests")`
4. Första testet:
   - Metodnamn: `shouldAnalyzePositiveSentimentCorrectly()`
   - `@DisplayName("Should analyze positive sentiment correctly")`
5. **Kör testet** - det ska MISSLYCKAS (RED fas) ✗

### ✅ Uppgift 4: Skapa TextAnalyzer klass (GREEN)
**Mål:** Implementera minimal kod för att få testet att lyckas  
**Fil:** `src/main/java/com/example/tdd/TextAnalyzer.java`

Implementera:
- Konstruktor som tar `Calculator` och `StringProcessor` som parametrar
- `analyzeSentiment(String text)` metod
- Hårdkodade ordlistor: `POSITIVE_WORDS` och `NEGATIVE_WORDS` (Set)
- Grundläggande logik för att räkna sentiment
- **Kör testet igen** - det ska NU LYCKAS (GREEN fas) ✓

### ✅ Uppgift 5: Lägg till fler sentimenttester (RED → GREEN → REFACTOR)
**Mål:** Bygg ut sentimentanalys steg för steg

Lägg till tester för (alla i `SentimentAnalysisTests`):

1. **Test 2:** Negativ sentiment
   - Metod: `shouldAnalyzeNegativeSentimentCorrectly()`
   - `@DisplayName("Should analyze negative sentiment correctly")`

2. **Test 3:** Neutral sentiment
   - Metod: `shouldAnalyzeNeutralSentimentCorrectly()`
   - `@DisplayName("Should analyze neutral sentiment correctly")`

3. **Test 4:** Parametriserat test för kategorier
   - Metod: `shouldClassifySentimentCategoriesCorrectly(String text, SentimentCategory expectedCategory)`
   - `@DisplayName("Should classify sentiment categories correctly")`
   - `@ParameterizedTest` med `@CsvSource`

4. **Test 5:** Tom input
   - Metod: `shouldHandleEmptyTextInSentimentAnalysis()`
   - `@DisplayName("Should handle empty text in sentiment analysis")`

**För varje test:** RED → GREEN → REFACTOR

### ✅ Uppgift 6: Skapa läsbarhetsanalys struktur (Setup)
**Mål:** Förbered för Flesch Reading Ease algoritm

Skapa:
1. `ReadingLevel.java` - enum med 7 nivåer (VERY_EASY → VERY_DIFFICULT)
2. `ReadabilityResult.java` - datastruktur med:
   - `fleschScore` (double)
   - `readingLevel` (ReadingLevel)
   - `sentenceCount`, `wordCount`, `syllableCount` (int)
   - `averageWordsPerSentence`, `averageSyllablesPerWord` (double)

### ✅ Uppgift 7: Implementera Flesch Reading Ease (RED → GREEN)
**Mål:** Implementera riktig matematisk formel genom TDD

**Formel:**
```
Flesch Score = 206.835 - (1.015 × ASL) - (84.6 × ASW)
där:
ASL = Average Sentence Length (ord per mening)
ASW = Average Syllables per Word (stavelser per ord)
```

**Skapa nästlad testklass:** `@DisplayName("Readability Analysis Tests")`

Implementera dessa tester i `ReadabilityAnalysisTests`:

1. **Test 1:** Grundläggande Flesch-beräkning
   - Metod: `shouldCalculateFleschReadingEaseCorrectly()`
   - `@DisplayName("Should calculate Flesch Reading Ease correctly")`
   - Skriv testet först ✗

2. **Test 2:** Klassificera läsningsnivåer
   - Metod: `shouldClassifyReadingLevelsCorrectly()`
   - `@DisplayName("Should classify reading levels correctly")`
   - Testa enkel text (hög Flesch-poäng)

3. **Test 3:** Komplexa meningar
   - Metod: `shouldHandleComplexSentencesCorrectly()`
   - `@DisplayName("Should handle complex sentences correctly")`
   - Testa akademisk text (låg Flesch-poäng)

4. **Test 4:** Parametriserat test för konsistens
   - Metod: `shouldProvideConsistentReadabilityMetrics(String text)`
   - `@DisplayName("Should provide consistent readability metrics")`
   - `@ParameterizedTest` med `@ValueSource`

**Implementera sedan:**
- Hjälpmetoder: `countSentences()`, `countSyllables()`, `countSyllablesInWord()`, `determineReadingLevel()`
- Huvudmetod: `analyzeReadability(String text)` ✓
- Refaktorera för läsbarhet och prestanda

### ✅ Uppgift 8: Lägg till prestanda- och statistiktester
**Mål:** Säkerställ att koden är produktionsklar

**A) Nästlad klass:** `@DisplayName("Statistical Analysis Tests")`

Implementera i `StatisticalAnalysisTests`:

1. **Test 1:** Omfattande textstatistik
   - Metod: `shouldGenerateComprehensiveTextStatistics()`
   - `@DisplayName("Should generate comprehensive text statistics")`
   - Verifiera: `TextStatistics` med alla metrics

2. **Test 2:** Grundläggande statistik
   - Metod: `shouldCalculateBasicTextStatistics()`
   - `@DisplayName("Should calculate basic text statistics")`
   - Testa ordräkning och unika ord

3. **Test 3:** Mellanslag och tecken
   - Metod: `shouldCountSpacesInText()`
   - `@DisplayName("Should count spaces in text")`

4. **Test 4:** Komplex vokabulär
   - Metod: `shouldHandleComplexVocabularyText()`
   - `@DisplayName("Should handle complex vocabulary text")`

**B) Nästlad klass:** `@DisplayName("Text Comparison and Similarity Tests")`

Implementera i `TextComparisonTests`:

5. **Test 1:** Textlikhet (Jaccard)
   - Metod: `shouldCalculateTextSimilarityCorrectly()`
   - `@DisplayName("Should calculate text similarity correctly")`

6. **Test 2:** Plagiatdetektering
   - Metod: `shouldDetectPotentialPlagiarism()`
   - `@DisplayName("Should detect potential plagiarism")`

**C) Nästlad klass:** `@DisplayName("Performance and Complex Scenario Tests")`

Implementera i `PerformanceAndComplexScenarioTests`:

7. **Test 1:** Stor texthantering
   - Metod: `shouldHandleLargeTextAnalysisEfficiently()`
   - `@DisplayName("Should handle large text analysis efficiently")`
   - 10,000 ord inom 5 sekunder

8. **Test 2:** Edge cases
   - Metod: `shouldHandleEdgeCasesAndMalformedInputGracefully()`
   - `@DisplayName("Should handle edge cases and malformed input gracefully")`
   - Testa: null, tom sträng, specialtecken

**D) Nästlad klass:** `@DisplayName("Integration with Calculator and StringProcessor Tests")`

Implementera i `IntegrationTests`:

9. **Test 1:** Calculator-integration
   - Metod: `shouldIntegrateWithCalculatorForStatisticalCalculations()`
   - `@DisplayName("Should integrate with Calculator for statistical calculations")`

10. **Test 2:** StringProcessor-integration
    - Metod: `shouldIntegrateWithStringProcessorForTextManipulation()`
    - `@DisplayName("Should integrate with StringProcessor for text manipulation")`

### ✅ Uppgift 9: Refaktorera och optimera (REFACTOR)
**Mål:** Förbättra kodkvalitet utan att ändra beteende

Fokus på:
- Bättre metodnamn
- Eliminera duplicerad kod
- Förbättra prestanda om nödvändigt
- Lägg till kommentarer för komplexa algoritmer
- **Kör ALLA tester** efter varje refaktorering ✓

---

### 🎯 Checklista för Lektion 3

#### Teststruktur (följer verklig implementation)
- [ ] **SentimentAnalysisTests** - 5 tester gröna
  - `shouldAnalyzePositiveSentimentCorrectly`
  - `shouldAnalyzeNegativeSentimentCorrectly`
  - `shouldAnalyzeNeutralSentimentCorrectly`
  - `shouldClassifySentimentCategoriesCorrectly` (parametriserad)
  - `shouldHandleEmptyTextInSentimentAnalysis`

- [ ] **ReadabilityAnalysisTests** - 4 tester gröna
  - `shouldCalculateFleschReadingEaseCorrectly`
  - `shouldClassifyReadingLevelsCorrectly`
  - `shouldHandleComplexSentencesCorrectly`
  - `shouldProvideConsistentReadabilityMetrics` (parametriserad)

- [ ] **StatisticalAnalysisTests** - 4 tester gröna
  - `shouldGenerateComprehensiveTextStatistics`
  - `shouldCalculateBasicTextStatistics`
  - `shouldCountSpacesInText`
  - `shouldHandleComplexVocabularyText`

- [ ] **TextComparisonTests** - 2 tester gröna
  - `shouldCalculateTextSimilarityCorrectly`
  - `shouldDetectPotentialPlagiarism`

- [ ] **PerformanceAndComplexScenarioTests** - 2 tester gröna
  - `shouldHandleLargeTextAnalysisEfficiently` (< 5 sekunder)
  - `shouldHandleEdgeCasesAndMalformedInputGracefully`

- [ ] **IntegrationTests** - 2 tester gröna
  - `shouldIntegrateWithCalculatorForStatisticalCalculations`
  - `shouldIntegrateWithStringProcessorForTextManipulation`

#### Kodkvalitet
- [ ] Ingen duplicerad kod
- [ ] Alla metoder har tydliga namn och `@DisplayName`
- [ ] Edge cases hanteras graciöst (inga exceptions)
- [ ] Dependency Injection korrekt implementerad
- [ ] Alla hjälpklasser skapade (TextStatistics, PlagiarismResult, etc.)

**Kör alla tester:**
```bash
mvn test -Dtest=TextAnalyzerTest
```

**Förväntat resultat:** 19+ tester (fler med parametriserade) ska lyckas! ✅

---

### 📋 Komplett teststruktur med DisplayNames

```
@DisplayName("Complex TextAnalyzer TDD Demo")
class TextAnalyzerTest {
    
    @Nested
    @DisplayName("Sentiment Analysis Tests")
    class SentimentAnalysisTests {
        ├─ @DisplayName("Should analyze positive sentiment correctly")
        ├─ @DisplayName("Should analyze negative sentiment correctly")
        ├─ @DisplayName("Should analyze neutral sentiment correctly")
        ├─ @DisplayName("Should classify sentiment categories correctly")
        └─ @DisplayName("Should handle empty text in sentiment analysis")
    }
    
    @Nested
    @DisplayName("Readability Analysis Tests")
    class ReadabilityAnalysisTests {
        ├─ @DisplayName("Should calculate Flesch Reading Ease correctly")
        ├─ @DisplayName("Should classify reading levels correctly")
        ├─ @DisplayName("Should handle complex sentences correctly")
        └─ @DisplayName("Should provide consistent readability metrics")
    }
    
    @Nested
    @DisplayName("Statistical Analysis Tests")
    class StatisticalAnalysisTests {
        ├─ @DisplayName("Should generate comprehensive text statistics")
        ├─ @DisplayName("Should calculate basic text statistics")
        ├─ @DisplayName("Should count spaces in text")
        └─ @DisplayName("Should handle complex vocabulary text")
    }
    
    @Nested
    @DisplayName("Text Comparison and Similarity Tests")
    class TextComparisonTests {
        ├─ @DisplayName("Should calculate text similarity correctly")
        └─ @DisplayName("Should detect potential plagiarism")
    }
    
    @Nested
    @DisplayName("Performance and Complex Scenario Tests")
    class PerformanceAndComplexScenarioTests {
        ├─ @DisplayName("Should handle large text analysis efficiently")
        └─ @DisplayName("Should handle edge cases and malformed input gracefully")
    }
    
    @Nested
    @DisplayName("Integration with Calculator and StringProcessor Tests")
    class IntegrationTests {
        ├─ @DisplayName("Should integrate with Calculator for statistical calculations")
        └─ @DisplayName("Should integrate with StringProcessor for text manipulation")
    }
}
```

---

## 🚀 Del 1: Dependency Injection och arkitektur

### Steg 1: TextAnalyzer grundstruktur (RED)

Först skapar vi `TextAnalyzerTest.java` med dependency injection:

```java
package com.example.tdd;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import static org.assertj.core.api.Assertions.*;

@DisplayName("TextAnalyzer Complex Algorithms")
class TextAnalyzerTest {
    
    private TextAnalyzer analyzer;
    private Calculator calculator;
    private StringProcessor stringProcessor;
    
    @BeforeEach
    void setUp() {
        calculator = new Calculator();
        stringProcessor = new StringProcessor();
        analyzer = new TextAnalyzer(calculator, stringProcessor);
    }
    
    @Nested
    @DisplayName("Sentiment Analysis Tests")
    class SentimentAnalysisTests {
        
        @Test
        @DisplayName("Should analyze positive sentiment correctly")
        void shouldAnalyzePositiveSentimentCorrectly() {
            // Arrange
            String positiveText = "I love this amazing wonderful fantastic product!";
            
            // Act
            SentimentResult result = analyzer.analyzeSentiment(positiveText);
            
            // Assert
            assertThat(result.getSentimentScore()).isGreaterThan(0.5);
            assertThat(result.getSentimentCategory()).isEqualTo(SentimentCategory.POSITIVE);
            assertThat(result.getPositiveWordCount()).isGreaterThan(0);
        }
    }
}
```

**Kör testet - alla klasser saknas (RED fas)!**

### Steg 2: Skapa nödvändiga datastrukturer (GREEN)

Först skapar vi `SentimentCategory.java`:

```java
package com.example.tdd;

public enum SentimentCategory {
    POSITIVE,
    NEGATIVE,
    NEUTRAL
}
```

Sedan `SentimentResult.java`:

```java
package com.example.tdd;

public class SentimentResult {
    private final double sentimentScore;
    private final SentimentCategory sentimentCategory;
    private final int positiveWordCount;
    private final int negativeWordCount;
    
    public SentimentResult(double sentimentScore, SentimentCategory sentimentCategory, 
                          int positiveWordCount, int negativeWordCount) {
        this.sentimentScore = sentimentScore;
        this.sentimentCategory = sentimentCategory;
        this.positiveWordCount = positiveWordCount;
        this.negativeWordCount = negativeWordCount;
    }
    
    public double getSentimentScore() { return sentimentScore; }
    public SentimentCategory getSentimentCategory() { return sentimentCategory; }
    public int getPositiveWordCount() { return positiveWordCount; }
    public int getNegativeWordCount() { return negativeWordCount; }
}
```

### Steg 3: TextAnalyzer grundimplementation

```java
package com.example.tdd;

import java.util.Set;

public class TextAnalyzer {
    
    private final Calculator calculator;
    private final StringProcessor stringProcessor;
    
    // Enkla sentimentordlistor för demonstration
    private static final Set<String> POSITIVE_WORDS = Set.of(
        "amazing", "awesome", "fantastic", "wonderful", "great", "excellent", 
        "good", "love", "perfect", "brilliant"
    );
    
    private static final Set<String> NEGATIVE_WORDS = Set.of(
        "terrible", "awful", "horrible", "bad", "hate", "disgusting",
        "disappointing", "worst"
    );
    
    public TextAnalyzer(Calculator calculator, StringProcessor stringProcessor) {
        this.calculator = calculator;
        this.stringProcessor = stringProcessor;
    }
    
    public SentimentResult analyzeSentiment(String text) {
        if (text == null || text.trim().isEmpty()) {
            return new SentimentResult(0.0, SentimentCategory.NEUTRAL, 0, 0);
        }
        
        // Förbehandla text
        String cleanText = text.toLowerCase().replaceAll("[^a-z\\s]", "");
        String[] words = cleanText.split("\\s+");
        
        int positiveCount = 0;
        int negativeCount = 0;
        
        // Räkna sentimentord
        for (String word : words) {
            if (POSITIVE_WORDS.contains(word)) {
                positiveCount++;
            } else if (NEGATIVE_WORDS.contains(word)) {
                negativeCount++;
            }
        }
        
        // Beräkna sentimentpoäng med Calculator
        int totalWords = words.length;
        double positiveRatio = totalWords > 0 ? calculator.divide(positiveCount, totalWords) : 0;
        double negativeRatio = totalWords > 0 ? calculator.divide(negativeCount, totalWords) : 0;
        
        // Förstärkt poängsättning
        double sentimentScore = (positiveRatio - negativeRatio) * 3.0;
        
        // Bestäm kategori
        SentimentCategory category;
        if (sentimentScore > 0.3 || positiveCount > 0) {
            category = SentimentCategory.POSITIVE;
        } else if (sentimentScore < -0.3 || negativeCount > 0) {
            category = SentimentCategory.NEGATIVE;
        } else {
            category = SentimentCategory.NEUTRAL;
        }
        
        return new SentimentResult(sentimentScore, category, positiveCount, negativeCount);
    }
}
```

**Testet bör nu lyckas!**

## 🧮 Del 2: Matematisk algoritm - Flesch Reading Ease

Nu implementerar vi en riktig matematisk formel genom TDD!

### Flesch Reading Ease Formula
```
Flesch Score = 206.835 - (1.015 × ASL) - (84.6 × ASW)
där:
ASL = Average Sentence Length (ord per mening)
ASW = Average Syllables per Word (stavelser per ord)
```

### Steg 1: Läsbarhetstester (RED)

```java
@Nested
@DisplayName("Readability Analysis Tests")
class ReadabilityAnalysisTests {
    
    @Test
    @DisplayName("Should calculate Flesch Reading Ease correctly")
    void shouldCalculateFleschReadingEaseCorrectly() {
        // Arrange
        String text = "The quick brown fox jumps over the lazy dog. This is a simple sentence for testing.";
        
        // Act
        ReadabilityResult result = analyzer.analyzeReadability(text);
        
        // Assert
        assertThat(result.getFleschScore()).isBetween(0.0, 100.0);
        assertThat(result.getReadingLevel()).isNotNull();
        assertThat(result.getSentenceCount()).isEqualTo(2);
        assertThat(result.getWordCount()).isGreaterThan(10);
        assertThat(result.getSyllableCount()).isGreaterThan(10);
    }
    
    @Test
    @DisplayName("Should classify reading levels correctly")
    void shouldClassifyReadingLevelsCorrectly() {
        // Arrange - Mycket enkel text
        String simpleText = "Cat sat on mat. Dog ran fast.";
        
        // Act
        ReadabilityResult result = analyzer.analyzeReadability(simpleText);
        
        // Assert
        assertThat(result.getFleschScore()).isGreaterThan(70.0); // Bör vara lätt att läsa
        assertThat(result.getReadingLevel()).isIn(ReadingLevel.VERY_EASY, ReadingLevel.EASY);
    }
    
    @Test
    @DisplayName("Should handle complex sentences correctly")
    void shouldHandleComplexSentencesCorrectly() {
        // Arrange - Komplex akademisk text
        String complexText = "The methodological framework utilized in this comprehensive investigation " +
                "demonstrates significant correlations between multidimensional variables.";
        
        // Act
        ReadabilityResult result = analyzer.analyzeReadability(complexText);
        
        // Assert
        assertThat(result.getFleschScore()).isLessThan(50.0); // Bör vara svår att läsa
        assertThat(result.getReadingLevel()).isIn(ReadingLevel.DIFFICULT, ReadingLevel.VERY_DIFFICULT);
        assertThat(result.getAverageWordsPerSentence()).isGreaterThan(15.0);
        assertThat(result.getAverageSyllablesPerWord()).isGreaterThan(2.0);
    }
}
```

### Steg 2: Skapa ReadingLevel enum och ReadabilityResult

```java
// ReadingLevel.java
public enum ReadingLevel {
    VERY_EASY,
    EASY,
    FAIRLY_EASY,
    STANDARD,
    FAIRLY_DIFFICULT,
    DIFFICULT,
    VERY_DIFFICULT
}

// ReadabilityResult.java
public class ReadabilityResult {
    private final double fleschScore;
    private final ReadingLevel readingLevel;
    private final int sentenceCount;
    private final int wordCount;
    private final int syllableCount;
    private final double averageWordsPerSentence;
    private final double averageSyllablesPerWord;
    
    public ReadabilityResult(double fleschScore, ReadingLevel readingLevel, int sentenceCount,
                           int wordCount, int syllableCount, double averageWordsPerSentence,
                           double averageSyllablesPerWord) {
        this.fleschScore = fleschScore;
        this.readingLevel = readingLevel;
        this.sentenceCount = sentenceCount;
        this.wordCount = wordCount;
        this.syllableCount = syllableCount;
        this.averageWordsPerSentence = averageWordsPerSentence;
        this.averageSyllablesPerWord = averageSyllablesPerWord;
    }
    
    // Getters...
    public double getFleschScore() { return fleschScore; }
    public ReadingLevel getReadingLevel() { return readingLevel; }
    public int getSentenceCount() { return sentenceCount; }
    public int getWordCount() { return wordCount; }
    public int getSyllableCount() { return syllableCount; }
    public double getAverageWordsPerSentence() { return averageWordsPerSentence; }
    public double getAverageSyllablesPerWord() { return averageSyllablesPerWord; }
}
```

### Steg 3: Implementera Flesch Reading Ease algoritm

Lägg till i `TextAnalyzer.java`:

```java
public ReadabilityResult analyzeReadability(String text) {
    if (text == null || text.trim().isEmpty()) {
        return new ReadabilityResult(0.0, ReadingLevel.VERY_EASY, 0, 0, 0, 0.0, 0.0);
    }
    
    // Räkna meningar
    int sentenceCount = countSentences(text);
    if (sentenceCount == 0) sentenceCount = 1; // Undvik division med noll
    
    // Räkna ord med StringProcessor
    int wordCount = stringProcessor.countWords(text);
    if (wordCount == 0) {
        return new ReadabilityResult(100.0, ReadingLevel.VERY_EASY, sentenceCount, 0, 0, 0.0, 0.0);
    }
    
    // Räkna stavelser
    int syllableCount = countSyllables(text);
    
    // Beräkna genomsnitt med Calculator
    double avgWordsPerSentence = calculator.divide(wordCount, sentenceCount);
    double avgSyllablesPerWord = calculator.divide(syllableCount, wordCount);
    
    // Flesch Reading Ease formel
    double fleschScore = 206.835 - (1.015 * avgWordsPerSentence) - (84.6 * avgSyllablesPerWord);
    
    // Säkerställ att poängen är inom gränser
    fleschScore = Math.max(0, Math.min(100, fleschScore));
    
    // Bestäm läsningsnivå
    ReadingLevel level = determineReadingLevel(fleschScore);
    
    return new ReadabilityResult(fleschScore, level, sentenceCount, wordCount, 
                               syllableCount, avgWordsPerSentence, avgSyllablesPerWord);
}

// Hjälpmetoder
private int countSentences(String text) {
    if (text == null || text.trim().isEmpty()) {
        return 0;
    }
    int count = 0;
    for (char c : text.toCharArray()) {
        if (c == '.' || c == '!' || c == '?') {
            count++;
        }
    }
    return Math.max(count, text.trim().isEmpty() ? 0 : 1);
}

private int countSyllables(String text) {
    if (text == null || text.trim().isEmpty()) {
        return 0;
    }
    
    String[] words = text.toLowerCase().replaceAll("[^a-z\\s]", "").split("\\s+");
    int totalSyllables = 0;
    
    for (String word : words) {
        if (!word.isEmpty()) {
            totalSyllables += countSyllablesInWord(word);
        }
    }
    
    return Math.max(totalSyllables, 1);
}

private int countSyllablesInWord(String word) {
    if (word.length() <= 3) return 1;
    
    word = word.toLowerCase();
    int syllables = 0;
    boolean previousVowel = false;
    
    for (int i = 0; i < word.length(); i++) {
        char c = word.charAt(i);
        boolean isVowel = "aeiouy".indexOf(c) != -1;
        
        if (isVowel && !previousVowel) {
            syllables++;
        }
        previousVowel = isVowel;
    }
    
    // Hantera tyst 'e'
    if (word.endsWith("e") && syllables > 1) {
        syllables--;
    }
    
    return Math.max(syllables, 1);
}

private ReadingLevel determineReadingLevel(double fleschScore) {
    if (fleschScore >= 90) return ReadingLevel.VERY_EASY;
    if (fleschScore >= 80) return ReadingLevel.EASY;
    if (fleschScore >= 70) return ReadingLevel.FAIRLY_EASY;
    if (fleschScore >= 60) return ReadingLevel.STANDARD;
    if (fleschScore >= 50) return ReadingLevel.FAIRLY_DIFFICULT;
    if (fleschScore >= 30) return ReadingLevel.DIFFICULT;
    return ReadingLevel.VERY_DIFFICULT;
}
```

## 📊 Del 3: Statistisk validering och prestandatestning

### Steg 1: Prestandatester (RED)

```java
@Nested
@DisplayName("Performance and Complex Scenario Tests")
class PerformanceAndComplexScenarioTests {
    
    @Test
    @DisplayName("Should handle large text analysis efficiently")
    void shouldHandleLargeTextAnalysisEfficiently() {
        // Arrange - Generera stor text (cirka 10 000 ord)
        StringBuilder largeText = new StringBuilder();
        String baseText = "The quick brown fox jumps over the lazy dog. ";
        for (int i = 0; i < 1000; i++) {
            largeText.append(baseText).append("Sentence number ").append(i).append(". ");
        }
        
        // Act
        long startTime = System.currentTimeMillis();
        SentimentResult sentiment = analyzer.analyzeSentiment(largeText.toString());
        ReadabilityResult readability = analyzer.analyzeReadability(largeText.toString());
        long endTime = System.currentTimeMillis();
        
        // Assert
        assertThat(endTime - startTime).isLessThan(5000); // Ska slutföras inom 5 sekunder
        assertThat(sentiment).isNotNull();
        assertThat(readability).isNotNull();
        assertThat(readability.getWordCount()).isGreaterThan(9000);
    }
    
    @Test
    @DisplayName("Should handle edge cases gracefully")
    void shouldHandleEdgeCasesGracefully() {
        // Arrange
        String[] edgeCases = {
            null, "", "   ", "!@#$%^&*()", "12345678990",
            "ALLUPPERCASE", "alllowercase"
        };
        
        // Act & Assert
        for (String edgeCase : edgeCases) {
            assertThatCode(() -> {
                analyzer.analyzeSentiment(edgeCase);
                analyzer.analyzeReadability(edgeCase);
            }).doesNotThrowAnyException();
        }
    }
}
```

### Steg 2: Statistisk validering med tolerans

```java
@Nested
@DisplayName("Statistical Analysis Tests")
class StatisticalAnalysisTests {
    
    @Test
    @DisplayName("Should provide consistent mathematical calculations")
    void shouldProvideConsistentMathematicalCalculations() {
        // Arrange
        String text = "The quick brown fox jumps over the lazy dog. This is a test sentence.";
        
        // Act
        ReadabilityResult result = analyzer.analyzeReadability(text);
        
        // Assert - Matematiska samband ska stämma
        double expectedAvgWordsPerSentence = (double) result.getWordCount() / result.getSentenceCount();
        double expectedAvgSyllablesPerWord = (double) result.getSyllableCount() / result.getWordCount();
        
        assertThat(result.getAverageWordsPerSentence())
            .isCloseTo(expectedAvgWordsPerSentence, offset(0.01));
        assertThat(result.getAverageSyllablesPerWord())
            .isCloseTo(expectedAvgSyllablesPerWord, offset(0.01));
    }
    
    @Test
    @DisplayName("Should maintain sentiment score bounds")
    void shouldMaintainSentimentScoreBounds() {
        // Arrange
        String[] testTexts = {
            "This is amazing fantastic wonderful excellent!",
            "This is terrible awful horrible disgusting!",
            "The weather is cloudy today."
        };
        
        // Act & Assert
        for (String text : testTexts) {
            SentimentResult result = analyzer.analyzeSentiment(text);
            
            // Sentimentpoäng ska vara inom rimliga gränser
            assertThat(result.getSentimentScore()).isBetween(-5.0, 5.0);
            assertThat(result.getPositiveWordCount()).isGreaterThanOrEqualTo(0);
            assertThat(result.getNegativeWordCount()).isGreaterThanOrEqualTo(0);
        }
    }
}
```

## 🔗 Del 4: Integration med befintliga klasser

### Integration med Calculator och StringProcessor

```java
@Nested
@DisplayName("Integration Tests")
class IntegrationTests {
    
    @Test
    @DisplayName("Should integrate with Calculator for mathematical operations")
    void shouldIntegrateWithCalculatorForMathematicalOperations() {
        // Arrange
        String text = "Hello world! This is a test with multiple sentences.";
        
        // Act
        ReadabilityResult result = analyzer.analyzeReadability(text);
        
        // Assert - Verifiera Calculator-integration
        int totalWords = result.getWordCount();
        int totalSentences = result.getSentenceCount();
        double avgWordsPerSentence = result.getAverageWordsPerSentence();
        
        // Dessa beräkningar ska använda vår Calculator internt
        double expectedAvg = calculator.divide(totalWords, totalSentences);
        assertThat(avgWordsPerSentence).isCloseTo(expectedAvg, offset(0.01));
    }
    
    @Test
    @DisplayName("Should integrate with StringProcessor for text manipulation")
    void shouldIntegrateWithStringProcessorForTextManipulation() {
        // Arrange
        String text = "Hello WORLD! This is a TEST with Mixed Case.";
        
        // Act
        SentimentResult result = analyzer.analyzeSentiment(text);
        
        // Assert - Verifiera StringProcessor-integration
        int wordCount = stringProcessor.countWords(text);
        assertThat(wordCount).isGreaterThan(0);
        
        // Sentimentanalys ska hantera text korrekt oavsett skiftläge
        assertThat(result.getSentimentCategory()).isIn(
            SentimentCategory.POSITIVE, 
            SentimentCategory.NEGATIVE, 
            SentimentCategory.NEUTRAL
        );
    }
}
```

## 🧪 Komplett TextAnalyzerTest.java struktur

```java
@DisplayName("TextAnalyzer Complex Algorithms")
class TextAnalyzerTest {
    
    private TextAnalyzer analyzer;
    private Calculator calculator;
    private StringProcessor stringProcessor;
    
    @BeforeEach
    void setUp() {
        calculator = new Calculator();
        stringProcessor = new StringProcessor();
        analyzer = new TextAnalyzer(calculator, stringProcessor);
    }
    
    @Nested
    @DisplayName("Sentiment Analysis Tests")
    class SentimentAnalysisTests {
        // 9 tester för sentimentanalys
    }
    
    @Nested
    @DisplayName("Readability Analysis Tests")
    class ReadabilityAnalysisTests {
        // 6 tester för läsbarhet
    }
    
    @Nested
    @DisplayName("Statistical Analysis Tests")
    class StatisticalAnalysisTests {
        // 4 tester för statistisk validering
    }
    
    @Nested
    @DisplayName("Performance and Complex Scenario Tests")
    class PerformanceAndComplexScenarioTests {
        // 2 tester för prestanda och edge cases
    }
    
    @Nested
    @DisplayName("Integration Tests")
    class IntegrationTests {
        // 2 tester för integration
    }
}
```

## 🏃‍♂️ Kör alla tester

```bash
# Kör endast TextAnalyzer-tester
mvn test -Dtest=TextAnalyzerTest

# Kör specifika testkategorier
mvn test -Dtest="TextAnalyzerTest#*Sentiment*"
mvn test -Dtest="TextAnalyzerTest#*Readability*"
mvn test -Dtest="TextAnalyzerTest#*Performance*"

# Kör alla tester från alla lektioner
mvn test
```

## 🎓 Vad har du lärt dig i Lektion 3?

### ✅ Komplexa algoritmer genom TDD
- **Sentimentanalys** med ordbaserad poängsättning
- **Flesch Reading Ease** - riktig matematisk formel
- **Stavelseuträkning** - fonematisk algoritm
- **Statistisk validering** med Calculator-integration

### ✅ Avancerad arkitektur
- **Dependency Injection** utan mock-ramverk
- **Immutable result objects** för trådsäkerhet
- **Composition pattern** - TextAnalyzer orkestrerar andra klasser
- **Strategy pattern** - olika analysmetoder

### ✅ Prestandamedvetenhet
- **Prestandatester** som del av TDD-cykeln
- **Skalbarhetstestning** med stora dataset
- **Tidsgränser** för algoritmkomplexitet
- **Memory-effektiv** implementation

### ✅ Statistisk programmering
- **Toleransintervall** i matematiska jämförelser
- **Boundary value testing** för algoritmer
- **Edge case resilience** för produktionskvalitet
- **Data validation** med matematisk precision

### ✅ Integration testing
- **Cross-component functionality** 
- **Real-world scenarios** med flera klasser
- **End-to-end workflows** genom TDD

## 🚀 Utmaning: Fördjupningsövningar

### 🎯 Avancerad sentimentanalys
Utöka sentimentanalys med:
```java
// Lägg till emotionsdetektering
public EmotionResult analyzeEmotion(String text) {
    // Detektera: GLÄDJE, SORG, ILSKA, RÄDSLA, FÖRVÅNING
}

// Lägg till ironidetektering  
public IronyResult detectIrony(String text) {
    // Detektera sarkasm och ironi
}
```

### 🎯 Textlikhet och plagiatdetektering
```java
public SimilarityResult calculateSimilarity(String text1, String text2) {
    // Implementera Jaccard similarity med TDD
}

public PlagiarismResult checkPlagiarism(String original, String suspicious) {
    // Detektera potentiellt plagiat
}
```

### 🎯 Språkdetektering
```java
public LanguageResult detectLanguage(String text) {
    // Detektera textspråk baserat på ordfrekvens
}
```

## 📚 Nästa lektion förhandsvisning

I **Lektion 4 - Integration Testing & Production Ready TDD** kommer vi att utforska:
- **End-to-end testing strategies**
- **Performance benchmarking**
- **Error handling and resilience**
- **Batch processing patterns**
- **Real-world deployment scenarios**
- **TDD in team environments**

---

**🎉 Fantastiskt arbete!** Du har nu implementerat sofistikerade algoritmer som sentimentanalys och läsbarhetsmätning genom ren TDD. Du kan hantera:

- ✅ **Komplexa matematiska formler** (Flesch Reading Ease)
- ✅ **Multi-step algoritmer** (sentimentanalys)
- ✅ **Prestandakritisk kod** med tidsgränser
- ✅ **Statistisk validering** med toleranser
- ✅ **Integration mellan komplexa system**

**Du är nu en avancerad TDD-praktiker!** Dessa färdigheter förbereder dig för verklig mjukvaruutveckling där komplexa algoritmer och prestandakrav är vardagsmat. 🚀

**Kom ihåg:** Även de mest komplexa algoritmerna kan byggas steg för steg genom RED-GREEN-REFACTOR. TDD ger dig modet att tackla alla programmeringsutmaningar! 💪
