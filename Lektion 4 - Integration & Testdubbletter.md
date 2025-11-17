Integration & Testdubbletter

## 🎯 Lärandemål

1. ✅ Förstå skillnaden mellan unit- och integrationstester
2. ✅ Implementera stubs för isolerad testning
3. ✅ Använda spies för beteendeverifiering
4. ✅ Testa hela systempipelines
5. ✅ Skriva realistiska integrationstester

---


## 📝 Nyckelkoncept

### Stubs för Testning

```java
// Stubimplementation för kontrollerat beteende
private static class CalculatorStub extends Calculator {
    @Override
    public double divide(double dividend, double divisor) {
        return 5.0;  // Kontrollerat returvärde för testning
    }
}

@Test
void shouldUseInjectedCalculator() {
    Calculator stub = new CalculatorStub();
    StringProcessor processor = new StringProcessor();
    TextAnalyzer analyzer = new TextAnalyzer(stub, processor);
    
    // Nu kommer divide() alltid returnera 5.0
    double result = analyzer.calculateAverageWordLength("test text");
    assertThat(result).isEqualTo(5.0);
}
```

### Spies för Beteendeverifiering

```java
// Spy spårar metodanrop
private static class CalculatorSpy extends Calculator {
    public int divideCallCount = 0;
    public double lastDividend;
    public double lastDivisor;
    
    @Override
    public double divide(double dividend, double divisor) {
        divideCallCount++;
        lastDividend = dividend;
        lastDivisor = divisor;
        return super.divide(dividend, divisor);
    }
}

@Test
void shouldCallDivideWithCorrectArguments() {
    CalculatorSpy spy = new CalculatorSpy();
    StringProcessor processor = new StringProcessor();
    TextAnalyzer analyzer = new TextAnalyzer(spy, processor);
    
    analyzer.calculateAverageWordLength("hello world");
    
    // Verifiera beteende
    assertThat(spy.divideCallCount).isEqualTo(1);
    assertThat(spy.lastDividend).isCloseTo(10.0, offset(0.1));
    assertThat(spy.lastDivisor).isCloseTo(2.0, offset(0.1));
}
```

### Integrationstester

```java
@Test
@DisplayName("Integration: Full text analysis pipeline")
void shouldPerformFullTextAnalysis() {
    // Riktiga komponenter, inget stubbing
    Calculator calculator = new Calculator();
    StringProcessor processor = new StringProcessor();
    TextAnalyzer analyzer = new TextAnalyzer(calculator, processor);
    
    String text = "The quick brown fox jumps over the lazy dog. " +
                  "The dog was not amused.";
    
    // Testa hela pipelinen
    int wordCount = processor.countWords(text);
    double avgLength = analyzer.calculateAverageWordLength(text);
    Map<String, Integer> frequency = analyzer.getWordFrequency(text);
    String mostFrequent = analyzer.getMostFrequentWord(text);
    
    // Assertions
    assertThat(wordCount).isEqualTo(13);
    assertThat(avgLength).isCloseTo(3.38, offset(0.1));
    assertThat(frequency.get("the")).isEqualTo(3);
    assertThat(mostFrequent).isEqualTo("the");
}
```

📚 UPPGIFT:
Slutför Integration-lektionen:
✅ Implementera egna stubs
✅ Skapa spies för verifiering
✅ Skriv 3 integrationstester
