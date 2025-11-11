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
