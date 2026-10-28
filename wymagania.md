# Projekt zaliczeniowy - Zaawansowane techniki obiektowe

## 1. Cel projektu

Celem projektu jest samodzielne zaprojektowanie i zaimplementowanie w języku Java lub C# aplikacji, w której w praktyce zastosowane zostaną techniki omawiane na zajęciach: zasady SOLID, wzorce projektowe, programowanie generyczne, TDD i BDD, elementy DDD oraz refaktoryzacja i zarządzanie długiem technicznym.

Projekt realizowany jest indywidualnie. Rezultatem pracy są kod źródłowy w repozytorium Git oraz dokument PDF opisujący stworzone rozwiązanie.

## 2. Temat

Temat, czyli domena aplikacji, wybierany jest samodzielnie przez studenta. Wybrana domena musi zawierać rzeczywiste reguły biznesowe - aplikacja nie może ograniczać się do dodawania, wyświetlania i usuwania danych.

Przykładowe kierunki, z których można skorzystać lub potraktować je jako inspirację do własnego tematu: wypożyczalnia sprzętu, system rezerwacji sal lub wizyt, sklep z koszykiem i promocjami, budżet domowy, system biletowy, planer zadań w zespole.

Minimalny zakres to 3 przypadki użycia zawierające reguły biznesowe, takie jak walidacja, obliczenia, przejścia między stanami czy ograniczenia.

## 3. Wymagania techniczne

- Język: Java 17 lub nowsza, zalecana Java 21.
- Projekt może zostać wykonany również w języku C# na platformie .NET 8 lub nowszej. Wybór języka należy zgłosić prowadzącemu najpóźniej na drugich zajęciach. Po tym terminie zmiana języka nie jest możliwa. Zajęcia i materiały prowadzone są w Javie, dlatego student wybierający C# samodzielnie przenosi omawiane rozwiązania na platformę .NET. Odpowiedniki narzędzi i konstrukcji wymienionych w dalszej części opisu:

| Java | C# |
|---|---|
| Maven lub Gradle | .NET CLI, czyli polecenia `dotnet build` i `dotnet test`, z plikiem `.sln` lub `.csproj` |
| JavaFX lub Swing | WPF, WinForms lub Avalonia |
| JUnit 5 | xUnit lub NUnit |
| AssertJ | Shouldly lub FluentAssertions |
| Mockito | Moq lub NSubstitute |
| Cucumber | Reqnroll |
| `Optional` | typy dopuszczające null, np. `T?`, z włączoną opcją `<Nullable>enable</Nullable>` w pliku `.csproj` |
| `<T extends Encja>` | `where T : Encja` |
| SonarLint, PMD, Checkstyle, SpotBugs | analizatory Roslyn wbudowane w .NET SDK lub SonarLint |
| diagram klas w IntelliJ IDEA Ultimate | diagram klas w JetBrains Rider lub w Visual Studio, funkcja Class Designer |

- Narzędzie budowania: Maven lub Gradle. Projekt musi się budować, uruchamiać i testować z linii poleceń.
- Interfejs użytkownika: zalecana jest aplikacja konsolowa jako najprostsze rozwiązanie. Dopuszczalny jest inny interfejs, np. okienkowy w JavaFX lub Swing albo REST API, pod warunkiem zachowania wymaganego podziału na warstwy.
- Dozwolone są biblioteki pomocnicze, m.in. JUnit 5, AssertJ, Mockito, Cucumber. Logika aplikacji musi być własnym kodem studenta.
- Kod należy tworzyć od zera - nie wolno korzystać z gotowych projektów ani kodu startowego z innych źródeł.
- Nie dopuszcza się użycia frameworków aplikacyjnych, takich jak Spring Boot, Jakarta EE czy Quarkus, a w C# ASP.NET Core MVC i ABP, ani gotowych kontenerów wstrzykiwania zależności, np. Microsoft.Extensions.DependencyInjection lub Autofac. W C# dopuszczalne jest REST API w ASP.NET Core Minimal API, pod warunkiem że własne klasy aplikacji nie są rejestrowane w `builder.Services`, a obiekty są tworzone i łączone ręcznie. Struktura aplikacji, tworzenie obiektów i powiązania między nimi muszą być zaprojektowane samodzielnie.

## 4. Repozytorium i historia commitów

- Projekt musi być prowadzony w repozytorium Git od początku prac.
- Obowiązuje zasada małych commitów:
  - jeden commit to jedna logiczna zmiana, np. nowa klasa z testami, jedna reguła biznesowa, poprawka błędu lub jedna refaktoryzacja,
  - commit nie może obejmować kilku funkcjonalności ani kilku warstw aplikacji naraz,
  - orientacyjny rozmiar commitu to do ok. 150 zmienionych linii kodu, nie licząc plików konfiguracyjnych,
  - komunikat commitu opisuje, co zostało zmienione, np. „Dodaj walidację terminu rezerwacji" zamiast „poprawki",
  - projekt musi mieć co najmniej **20 commitów** rozłożonych w czasie trwania prac.
- Repozytorium, w którym duże części projektu pojawiają się w pojedynczych commitach, nie spełnia wymagań.
- Historia commitów stanowi dowód zastosowania cyklu TDD oraz przeprowadzonych refaktoryzacji.
- W repozytorium musi znajdować się plik `README.md` z instrukcją zbudowania i uruchomienia aplikacji oraz uruchomienia testów.

## 5. Wymagania merytoryczne

### 5.1. Konstrukcja systemu obiektowego i elementy DDD

- Wyraźny podział na warstwy: domena, aplikacja, infrastruktura i interfejs użytkownika. Warstwa domeny nie może zależeć od pozostałych warstw.
- W modelu domeny należy zastosować:
  - **encje**,
  - niemutowalne **obiekty wartości**, np. jako `record`,
  - co najmniej jeden **agregat**, który sam pilnuje reguł biznesowych dotyczących jego danych, tak aby nie dało się ich obejść z zewnątrz,
  - **repozytorium** zdefiniowane jako interfejs w domenie i zaimplementowane w warstwie infrastruktury. Wystarczy przechowywanie danych w pamięci lub w pliku. Baza danych nie jest wymagana.
- Nazewnictwo w kodzie musi być zgodne z językiem domeny opisanym w słowniku pojęć.

### 5.2. BDD

- Dla co najmniej **3 reguł biznesowych** należy zapisać w dokumentacji scenariusz składający się z trzech części: *Zakładając*, *Gdy* i *Wtedy*.
- Każdemu scenariuszowi musi odpowiadać test w JUnit 5 o tej samej strukturze, podzielony na części given, when i then, z nazwą opisującą zachowanie.
- Można użyć narzędzia Cucumber, które pozwala uruchamiać scenariusze zapisane w języku Gherkin bezpośrednio jako testy. Nie jest to wymagane.

### 5.3. Testy jednostkowe i TDD

- Każda reguła biznesowa z opisu domeny musi mieć testy jednostkowe w JUnit 5 sprawdzające co najmniej:
  - przypadek poprawny, w którym operacja się udaje,
  - przypadek, w którym reguła blokuje operację, np. przekroczony limit, błędne dane lub niedozwolony stan.
- Co najmniej **jedna reguła biznesowa** musi zostać wytworzona w cyklu TDD. W historii muszą być widoczne kolejno: commit z testem, który nie przechodzi, a następnie commit z implementacją, po której test przechodzi.
- W ostatecznej wersji projektu wszystkie testy muszą przechodzić.

### 5.4. Zasady SOLID

- Kod musi przestrzegać wszystkich pięciu zasad SOLID.
- W dokumentacji dla każdej zasady należy wskazać konkretne miejsce w kodzie, w którym jest widoczna, i krótko je uzasadnić.

### 5.5. Wzorce projektowe

- Należy zastosować co najmniej **4 wzorce projektowe**. Mogą pochodzić z dowolnych kategorii.
- Każdy wzorzec musi rozwiązywać rzeczywisty problem w aplikacji. Wzorce dodane „na siłę", bez uzasadnienia, nie będą uwzględniane.
- Można skorzystać ze wzorców omawianych na zajęciach: **Strategia, Łańcuch zobowiązań, Obserwator, Fabryka**. Nie jest to obowiązkowe - należy wybrać te, które pasują do domeny projektu.

### 5.6. Programowanie generyczne

- Należy zaprojektować co najmniej jeden **własny typ generyczny lub metodę generyczną z ograniczeniem typu**, np. `<T extends Encja>`.
- Przykładowe zastosowania: generyczne repozytorium, metoda wyszukująca lub filtrująca obiekty domeny, walidator.
- Element generyczny musi być użyty w co najmniej dwóch miejscach z różnymi typami.

### 5.7. Jakość, bezpieczeństwo i wydajność kodu

- Hermetyzacja: brak publicznych modyfikowalnych pól, kopie obronne kolekcji, niemutowalność tam, gdzie to możliwe.
- Walidacja danych wprowadzanych przez użytkownika - błędne dane nie mogą powodować awarii aplikacji ani naruszenia stanu domeny.
- Świadoma obsługa wyjątków: własne wyjątki domenowe i brak „połykania" wyjątków. Unikanie zwracania `null`, np. przez użycie `Optional`.
- Dobór odpowiednich struktur danych do realizowanych operacji.

### 5.8. Dług techniczny i refaktoryzacja - wymaganie na ocenę bardzo dobrą

- Należy zidentyfikować w kodzie projektu co najmniej **3 problemy jakościowe**, tzw. code smells, które rzeczywiście pojawiły się w trakcie rozwoju projektu, np. długą metodę, duplikację, zbyt dużą klasę lub rozbudowane instrukcje warunkowe.
- Zidentyfikowane problemy należy zrefaktoryzować. Wersja kodu przed zmianą i po niej musi być widoczna w historii commitów. Problemy wprowadzone celowo tylko po to, by je naprawić, nie będą uwzględniane.
- Zalecane jest użycie narzędzia do statycznej analizy kodu, np. SonarLint, PMD, Checkstyle lub SpotBugs.

## 6. Narzędzia AI

Dopuszcza się korzystanie z narzędzi AI jako wsparcia, np. do wyjaśniania pojęć lub szukania błędów. Student odpowiada jednak za cały kod w repozytorium i musi umieć go wyjaśnić. W dokumentacji należy opisać w 2-3 zdaniach, z jakich narzędzi AI korzystano i do czego. Prowadzący może poprosić o wyjaśnienie dowolnego fragmentu kodu podczas oddawania projektu.

## 7. Dokumentacja PDF

Dokumentacja ma być zwięzła. Nie należy opisywać ogólnie technik omawianych na zajęciach, lecz wskazać, gdzie i dlaczego zostały zastosowane w kodzie projektu. Dokument PDF powinien zawierać:

1. **Stronę tytułową:** imię i nazwisko, numer albumu, tytuł projektu oraz opcjonalnie link do repozytorium.
2. **Opis domeny:** kilka zdań o aplikacji, lista przypadków użycia z regułami biznesowymi, słownik pojęć domenowych oraz scenariusze BDD.
3. **Strukturę projektu:** zrzut ekranu drzewa pakietów i klas z IDE, z krótkim opisem warstw.
   *Podpowiedź:* IntelliJ IDEA Ultimate potrafi wygenerować diagram klas z kodu funkcją *Diagrams -> Show Diagram*. Studenci mogą otrzymać bezpłatną licencję w ramach programu edukacyjnego JetBrains. Taki diagram można dołączyć obok zrzutu drzewa pakietów.
4. **Tabelę realizacji wymagań** z kolumnami: wymaganie, klasa lub plik, commit, uzasadnienie w 1-2 zdaniach. Osobny wiersz dla elementów DDD, scenariuszy BDD, cyklu TDD, każdej zasady SOLID, każdego wzorca projektowego i elementów generycznych.
5. **Zrzuty ekranu:**
   - przebieg każdego przypadku użycia w interfejsie aplikacji, w tym reakcja aplikacji na błędne dane,
   - wynik uruchomienia testów jednostkowych i scenariuszy BDD,
   - historia commitów, np. wynik `git log --oneline`.
6. **Refaktoryzacje**, jeśli zostały wykonane: dla każdej problem, zastosowana technika, fragment kodu przed zmianą i po niej oraz hash commitu.
7. **Deklarację użycia narzędzi AI:** 2-3 zdania o tym, z jakich narzędzi korzystano i do czego.

## 8. Sposób oddania

- **Kod:** archiwum ZIP z całym katalogiem projektu, **w tym z katalogiem `.git`** zawierającym historię commitów. Archiwum bez katalogu `.git` nie spełnia wymagań. Do archiwum **nie należy** dołączać skompilowanych plików ani katalogów generowanych przez narzędzia budowania i środowisko programistyczne, np. `target`, `build`, `out`, `bin`, `obj`, `.idea`, `.vs`.
- **Link do repozytorium:** opcjonalnie można dodatkowo podać link do repozytorium Git, np. na GitHubie. Link nie zastępuje archiwum ZIP.
- **Dokumentacja:** plik PDF.
- Archiwum ZIP i plik PDF należy przesłać przez Moodle w wyznaczonym terminie.

## Status przesłanego zadania

| | |
|---|---|
| Numer próby | To jest próba nr 1 (liczba dozwolonych prób: 3). |
| Status przesłanego zadania | Nie przesłano jeszcze zadania |
| Stan oceniania | Nie ocenione |
| Pozostały czas | Pozostało 66 dni 5 godzin |
| Ostatnio modyfikowane | - |

## Kryteria oceny

Przeliczenie na ocenę według karty przedmiotu:

| Punkty | Ocena |
|---|---|
| 90-100 | bardzo dobry |
| 80-89 | dobry plus |
| 70-79 | dobry |
| 60-69 | dostateczny plus |
| 50-59 | dostateczny |
| 0-49 | niedostateczny |

Maksymalna suma punktów bez kryterium 10, czyli refaktoryzacji, wynosi 88. Ocena bardzo dobra wymaga więc spełnienia wymagania z punktu 5.8.

### Warunki konieczne

Projekt, który nie spełnia któregokolwiek z poniższych warunków, nie jest oceniany rubryką i otrzymuje ocenę niedostateczną:

- archiwum ZIP zawiera katalog `.git` z historią commitów,
- duże części projektu nie pojawiają się w pojedynczych commitach,
- projekt buduje się i uruchamia zgodnie z instrukcją w `README.md`,
- projekt nie korzysta z niedozwolonych frameworków aplikacyjnych,
- student potrafi wyjaśnić kod na prośbę prowadzącego.

### Tabela kryteriów

**1. Model domeny i elementy DDD**

| Punkty | Opis |
|---|---|
| 0 | Brak modelu domeny lub brak podziału na warstwy. |
| 6 | Podział na warstwy niepełny lub domena zależy od innych warstw. Brakuje części wymaganych elementów DDD. |
| 12 | Wszystkie wymagane elementy DDD są obecne, ale z usterkami, np. agregat nie chroni wszystkich reguł biznesowych. |
| 18 | Kompletny i poprawny model: warstwy, encje, obiekty wartości, agregat chroniący reguły biznesowe, repozytorium jako interfejs w domenie, nazewnictwo zgodne ze słownikiem pojęć. |

**2. Zasady SOLID**

| Punkty | Opis |
|---|---|
| 0 | Zasady SOLID nie są stosowane. |
| 5 | Widoczne pojedyncze zasady, liczne naruszenia pozostałych. |
| 10 | Większość zasad zastosowana poprawnie, część wskazań w dokumentacji ogólnikowa lub nietrafna. |
| 14 | Wszystkie zasady zastosowane i trafnie wskazane w kodzie. |

**3. Wzorce projektowe**

| Punkty | Opis |
|---|---|
| 0 | Brak wzorców lub wyłącznie wzorce dodane „na siłę". |
| 6 | 1-2 poprawnie zastosowane i uzasadnione wzorce. |
| 12 | 3 poprawnie zastosowane wzorce lub 4 wzorce, z których część ma błędy implementacji albo słabe uzasadnienie. |
| 18 | Co najmniej 4 poprawnie zaimplementowane wzorce rozwiązujące rzeczywiste problemy w aplikacji. |

**4. Programowanie generyczne**

| Punkty | Opis |
|---|---|
| 0 | Brak własnego typu lub metody generycznej. |
| 3 | Element generyczny bez ograniczenia typu lub użyty tylko z jednym typem. |
| 6 | Element generyczny z ograniczeniem typu, użyty w co najmniej dwóch miejscach z różnymi typami. |

**5. Jakość, bezpieczeństwo i wydajność kodu**

| Punkty | Opis |
|---|---|
| 0 | Aplikacja ulega awarii przy błędnych danych, brak hermetyzacji. |
| 4 | Hermetyzacja, walidacja lub obsługa wyjątków z istotnymi lukami. |
| 8 | Poprawna hermetyzacja, walidacja danych, obsługa wyjątków i dobór struktur danych. |

**6. Testy jednostkowe i TDD**

| Punkty | Opis |
|---|---|
| 0 | Brak testów jednostkowych. |
| 3 | Testy sprawdzają tylko przypadki poprawne, nie obejmują wszystkich reguł biznesowych lub część testów nie przechodzi. |
| 6 | Testy kompletne, ale cykl TDD nie jest widoczny w historii commitów. |
| 8 | Testy kompletne i co najmniej jeden cykl TDD widoczny w historii commitów. |

**7. BDD**

| Punkty | Opis |
|---|---|
| 0 | Brak scenariuszy BDD. |
| 2 | Mniej niż 3 scenariusze lub scenariusze bez odpowiadających im testów. |
| 4 | Co najmniej 3 scenariusze z odpowiadającymi im testami. |

**8. Historia commitów**

| Punkty | Opis |
|---|---|
| 0 | Mniej niż 20 commitów. |
| 3 | Co najmniej 20 commitów, ale część z nich jest zbyt duża lub ma nieopisowe komunikaty. |
| 6 | Historia w pełni zgodna z zasadą małych commitów. |

**9. Dokumentacja PDF**

| Punkty | Opis |
|---|---|
| 0 | Brak dokumentacji lub brak większości wymaganych elementów. |
| 3 | Brakuje części elementów lub zamiast odwołań do kodu są ogólne opisy technik. |
| 6 | Wszystkie wymagane elementy z konkretnymi odwołaniami do kodu i commitów. |

**10. Dług techniczny i refaktoryzacja - wymaganie na ocenę bardzo dobrą**

| Punkty | Opis |
|---|---|
| 0 | Wymaganie niespełnione: mniej niż 3 rzeczywiste refaktoryzacje lub brak ich dokumentacji. |
| 12 | Co najmniej 3 rzeczywiste refaktoryzacje, widoczne w historii commitów i udokumentowane. |
