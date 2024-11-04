# Struktury danych oraz kolekcje w C#

Artykuł ten jest poświęcony strukturom danych, które znamy w współczesnej informatyce oraz gotowym kolekcjom w C#.

Dowiesz się miedzy innymi:

- Czym są struktury danych oraz w jaki sposób działają i jakie istnieją w informatyce
- Czym są kolekcje, ich implementacje i różnice
- Porównanie funkcjonalności
- Dodatkowe niestandardowe kolekcje

---

### Spis treści:

1. Struktury danych
   1. Struktura (rekord)
   2. krotka
   3. Tablica
   4. Lista (Linked list)
   5. Stos
   6. Kolejka
   7. Drzewo
   8. Graf
2. Kolekcje
   1. Co to są kolekcje?
   2. Złożoność obliczeniowa
   3. Dlaczego kolekcje dziedziczą po wielu interfejsach?
   4. Interfejs IEnumerable i IEnumerator
   5. Kolekcje generyczne i niegeneryczne
   6. Thread-safe w kolekcjach
3. Kolekcje niegeneryczne
   1. TODO
4. Kolekcje generyczne
   1. TODO
5. Kolekcje thread-safe
   1. TODO
6. Niestandardowe implementacje kolekcji w C#
   1. TODO
7. Własna kolekcja w C#
8. Benchmark kolekcji i struktur danych
9. Porównywanie kolekcji i struktur danych
10. Literatura

---

### 1. Struktury danych

Struktury danych to sposób **przechowywania naszych danych w pamięci komputera.** Danymi z pamięci komputera zarządzają algorytmy, które są zaimplementowane w kolekcjach, ale o tym w kolejnych rozdziałach.

Struktury danych wykorzystujemy do świadomego zarządzania naszymi danymi, aby przyśpieszyć lub zoptymalizować dostęp do naszych danych.

Wiele języków programowania posiada gotowe implementacje struktury danych już w swoich wbudowanych bibliotekach, np. w C# jest to **System.Collections.**

Jest 8 głównych strutkur danych, które są najpopularniejsze w programowaniu:

- struktura (rekord)
- krotka,
- tablica,
- lista,
- stos,
- kolejka,
- drzewo,
- graf

### 1.1 Struktura (rekord)

Struktura, inaczej zwana rekordem jest to grupa powiązana ze sobą danych różnego typu w jednym obszarze pamięci. Każda struktur składa się z pól do których ma się dostęp za pomocą unikatowych nazw. Podanie unikatowej nazwy daje nam dostęp do wskazanego pola.

![Alokacja pamięci](Images/structure_memory_allocation.png)

Na zdjęciu powyżej przedstawione jest w jaki sposób alokowana jest pamięć struktury w pamięci RAM.

Struktura alokuje większa ilość pamięci dla niektórych struktur tworząc tzw. padding. Padding pomaga w lepszej pracy mikroprocesora.

Struktura w C# jest typem wartościowym, dlatego więc zarządzana jest na stosie tak jak inne typy wartościowe np. int.

Przykład w C#:

```csharp
    struct Point
    {
        private readonly int _x;
        private readonly int _y;

        public Point(int x, int y)
        {
            _x = x;
            _y = y;
        }

        public override string ToString()
        {
            return $"X: {_x}, Y: {_y}";
        }
    }

    static void Main(string[] args)
    {
        var point = new Point(1, 2);
        Console.WriteLine(point);
    }
```

### 1.2 Krotka

Krotka jest reprezentacją n-ki (para uporządkowana) w informatyce przedstawiająca uporzadkowane ciągi wartości. Typ ten przechowuje **stałe wartości** o różnych typach danych. Nie jest możliwa modyfikacja żadnego elementu, a odczyt wymaga podania indeksu liczbowego (zamiast nazwy jak występuje to w strukturach).

Typowa tupla jest typem wartościowym, jednak w C# jest i również typem referencyjnym. (ValueTuple jest typem wartościowym, a Tuple typem referencyjnym). Dlatego, że ValueTuple jest to tak naprawdę struktura, a Tuple to klasa.

Przykład Tuple oraz ValueTuple:

```csharp
    static void Main(string[] args)
    {
        var valueTuple = new ValueTuple<int,string>(1, "testowa wartosc");
        var valueTuple2 = (1, "testowa wartosc");
        Console.WriteLine($"{valueTuple.Item2} - {valueTuple.Item1}");
        Console.WriteLine($"{valueTuple2.Item2} - {valueTuple2.Item1}");

        var tuple = new Tuple<int, string>(1, "testowa wartosc");
        Console.WriteLine($"{tuple.Item2} - {tuple.Item1}");
    }
```

### 1.3 Tablica

Kontener (kontener to również struktura danych, lecz mocno zbliżona do właściwości tablicy) uporządkowanych danych, zazwyczaj tego samego typu dostępne za pomocą kluczu, czyli tzw. indeksu. Tablica może przechowywać inne tablice tworząc w ten sposób tablice wielowymiarowe. Tablice są statycznych rozmiarów, jeśli chcemy rozszerzyć naszą tablicę, wtedy tworzymy nową tablicę i przenosimy te dane do nowej, rozszerzając ją o dodatkowe miejsca. Tablica zajmuje tyle miejsca w pamięci ile jej wskazujemy przy inicjalizacji. Niewykorzystane miejsce jest ciągle zarezerwowane na przyszłość.

Przykład tablicy:

![Przykładowa tablica](Images/array.png)

Tablica umożliwia szybki dostęp do danych za pomocą wskazania indeksu, w którym dana wartość się znajduję. Usuwanie elementów nie istnieje, lecz istnieje możliwość zastąpienia istniejącej wartości inną lub pustą. Jest to jedna z najszybszych i najprostszych struktur danych.

Przykład w C#

```csharp
    static void Main(string[] args)
    {
        var array = new int[3];
        array[0] = 1;
        array[1] = 2;
        array[2] = 3;
        Console.WriteLine(array[0] + array[1] + array[2]);
    }
```

### 1.4 Lista (Linked list)

Struktura danych, która posiada uporządkowane elementy w liniowym porządku. Każdy element posiada referencje do następnego elementu. Wymieniamy 2 główne listy, czyli jednokierunkowa lub dwukierunkowa. W jednokierunkowej poruszanie się po elementach występuje w jedną stronę, a w dwukierunkowej po obu stronach.

Przykład w C#

```csharp
    static void Main(string[] args)
    {
        var linkedList = new LinkedList<string>();
        linkedList.AddLast("first");
        linkedList.AddLast("second");
        linkedList.AddLast("third");
        Console.WriteLine(linkedList.First.Value);
        Console.WriteLine(linkedList.Last.Value);
        Console.WriteLine(linkedList.Find("second").Value);

        linkedList.RemoveFirst();
        Console.WriteLine(linkedList.First.Value);

        linkedList.AddBefore(linkedList.First, "new first");
        Console.WriteLine(linkedList.First.Value);
    }
```

### 1.5 Stos

Stos to również liniowa struktura, w której występuje tzw. LIFO (ostatni wchodzi pierwszy wychodzi). Tą strukturę danych można sobie porównać do sterty książek, za każdym razem dokładamy książki i czytamy tylko te, które są na wierzchu. Elementy na stosie można obejrzeć, jednak należy robić to pokolei od pierwszego elementu do ostatniego. Nie można wyciągać elementów ze środka stosu, należy ściągnać wszystkie te, które są nad danym elementem.

Stos jest popularny w informatyce m.in. w rejestrze procesora, do przechowywania zmiennych lokalnych w naszych programach.

W niektórych językach programowania tablica posiada funkcję stosu, czyli push, pop.

Przykład w C#

```csharp
    static void Main(string[] args)
    {
        var stos = new Stack<string>();

        stos.Push("1");
        stos.Push("2");
        stos.Push("3");

        foreach (var element in stos)
        {
            Console.WriteLine(element);
        }
    }
```

### 1.6 Kolejka

Struktura danych podobna do stosu, tylko wyznaje inną zasadę, czyli FIFO (Pierwszy wchodzi pierwszy wychodzi).
Kolejka jak sama nazwa mówi działa jak kolejka np. w sklepie. Specjalną modyfikacją kolejki może być kolejka priorytetowa, która narzuca priorytet swoim elementom (kolejka priorytetowa występuje w priorytecie procesów w procesorze).

Przykład w C#

```csharp
    static void Main(string[] args)
    {
        var stos = new Queue<string>();

        stos.Enqueue("1");
        stos.Enqueue("2");
        stos.Enqueue("3");

        foreach (var element in stos)
        {
            Console.WriteLine(element);
        }
    }
```

### 1.7 Drzewo

Struktura danych reprezentująca w naturalny sposób hierarchię danych. Drzewa ułatwiają i przyśpieszają pracę w wyszukiwaniach danych oraz pozwalają na łatwą pracę na posortowanych danych.
Drzewa są najpopularniejsze w informatyce i stosowane w m.in. bazach danych, serwerach, telekomunikacji itd.

Drzewo składa się z wierzchołka oraz krawędzi. Jeśli mamy więcej niż 1 wierzchołek to wyznaczamy jeden główny wierzchołek nazywając go korzeniem drzewa. Ciąg krawędzi nazywamy ścieżką. Główny wierzchołek jest rodzicem, a wierzchołki pod nim dziećmi.

Prostym drzewem jest drzewo binarne, które posiada tylko maksymalnie 2 dzieci.

Przykładem drzewa w C# może być LinkedList, która reprezentuje drzewo posiadające po jednym dziecku dla każdego rodzica.

Kopiec, inaczej zwany sterta również jest oparty na drzewie, znajdziemy go w m.in zarządzaniu pamięcią w C# za pomocą Garbage Collectora.

### 1.8 Graf

Graf podstawowy obiekt rozważań teorii grafów, struktura matematyczna przedstawiająca relacje pomiędzy obiektami. Grafy wykorzystywane są bardzo często, np. w systemach GPS, sztucznej inteligencji, trasowanie tras w telekomunikacji.

Wierzchołki w grafie mogą być numerowane, a krawędzie mogą posiadać wagi oraz wyznaczony kierunek.

Drzewo możemy nazwać grafem, dlatego że również posiada wierzchołki oraz krawędzie, które prowadzą Nas do jakiegoś wierzchołka.

Grafy są najbardziej rozbudowaną strukturą danych i najbardziej zaawansowaną.

Przykład grafu:
![Przykładowy graf](Images/Directed_graph_no_background.svg.png)

### 2.1 Co to są kolekcje?

Kolekcje są to zbiory danych, które posiadają gotową implementację narzuconą przez twórce biblioteki. Kolekcja posiada określoną implementację na daną metodę.

Np. metoda sortująca w kolekcji **A** może mieć inną implementacje (użyty algorytm) w kolekcji **B**.

Każda kolekcja posiada swoje unikalne zastosowanie. W niektórych przypadkach użyjemy jednej kolekcji, a w innych nie. Musimy pamiętać, aby świadomie dobierać kolekcję do danego problemu. Kolekcje są, aby nam pomagać, niż utrudniać.

Każdą kolekcję możemy nadpisać lub jeśli chcemy możemy utworzyć własną implementacje kolekcji.

Kolekcje przechowują dane, jednak niektóre robią to w inny sposób narzucając klucz, który daje dostęp do danego obiektu lub typu wartościowego albo także ignoruje duplikaty. Są też kolekcje, które narzucają dany typ lub są generyczne (Przykłady List oraz List<T>).

### 2.2 Złożoność obliczeniowa

Kolekcje posiadają różną złożoność (Wielkie O) tzn. każda operacja może wyglądać inaczej na danej kolekcji. Jest to spowodowane tym, że każda kolekcja posiada swoją implementację, która może się różnić od innej implementacji w innej kolekcji oraz zastosowanej struktury danych.

Np.
Zczytywanie obiektu z Listy może trwać dłużej w najgorszym przypadku, niż w tablicy, która ma określony rozmiar.

Co to znaczy najgorszy lub najlepszy przypadek?
Opisuje to sytuację, w której algortm zachowa się najszybciej lub najwolniej.

Wymieniamy kilka złożoności obliczeniowych:

- O(1) – Stała złożoność czasowa
  Algorytm działa zawsze w stałym czasie, niezależnie od rozmiaru danych wejściowych. Przykładem może być dostęp do elementu w tablicy za pomocą indeksu.

- O(log n) – Logarytmiczna złożoność czasowa
  Czas działania rośnie logarytmicznie wraz ze wzrostem rozmiaru danych. Przykładem jest wyszukiwanie binarne w posortowanej tablicy.

- O(n) – Liniowa złożoność czasowa
  Czas działania rośnie liniowo wraz z rozmiarem danych wejściowych. Przykładem może być przechodzenie przez wszystkie elementy tablicy lub listy.

- O(n log n) – Liniowo-logarytmiczna złożoność czasowa
  Algorytm rośnie szybciej niż liniowy, ale wolniej niż kwadratowy. Przykładem jest sortowanie szybkie (quicksort) lub sortowanie przez scalanie (merge sort).

- O(n²) – Kwadratowa złożoność czasowa
  Czas działania rośnie proporcjonalnie do kwadratu rozmiaru danych wejściowych. Przykładem może być sortowanie bąbelkowe.

- O(2ⁿ) – Eksponencjalna złożoność czasowa
  Czas działania rośnie wykładniczo wraz z rozmiarem danych, co czyni je bardzo nieefektywnymi dla większych zbiorów danych. Przykładem jest algorytm rekurencyjny dla problemu plecakowego (knapsack problem).

- O(n!) – Silniowa złożoność czasowa
  Czas działania rośnie proporcjonalnie do silni rozmiaru danych wejściowych, co czyni je niepraktycznymi dla większości problemów o większym rozmiarze. Przykładem jest algorytm przeszukiwania wszystkich permutacji, jak problem komiwojażera (travelling salesman problem).

### 2.3 Dlaczego kolekcje dziedziczą po wielu interfejsach?

Kolekcje dziedziczą po wielu interfesjach, dlatego aby wszystkie kolekcje były spójne z takimi metodami jak np. Add(), RemoveAt(), CopyTo().

Używanie interfejsów daje nam większa swobodę w implementacji metod, które dana kolekcja będzie wykorzystywała nie narzucając jej implementacji, tylko uwzględniając nazwę metody oraz jej typy, które powinna owa kolekcja obsługiwać. Pozwala również na podmianę implementacji ze względu na to, że nie naruszymy poprawności naszego kodu.

Wszystkie kolekcje dziedziczą po IEnumerable, więc wiadomo, że umożliwiają iteracje po elementach. Niektóre kolekcje tak jak Lista implementują IList, który pozwala na wykorzystywanie metod, które narzuca dany interfejs.

Przykład:

```csharp
// Lista implementuje aż 8 interfejsów.
public class List<T> :
    IList<T>,
    ICollection<T>,
    IEnumerable<T>,
    IEnumerable,
    IList,
    ICollection,
    IReadOnlyList<T>,
    IReadOnlyCollection<T>
```

Interfejs IList<T> narzuca implementację 3 metod oraz indexera.

```csharp
public interface IList<T> : ICollection<T>, IEnumerable<T>, IEnumerable
{
    T this[int index] { get; set; }
    int IndexOf(T item);
    void Insert(int index, T item);
    void RemoveAt(int index);
}
```

### 2.4 Interfejs IEnumerable i IEnumerator

IEnumerable i IEnumerator umożliwiają nam iterowanie po danej kolekcji. Iterowanie oznacza przechodzenie element po elemencie w kolekcji.

IEnumerable posiada tylko jedną metodę, która zwraca nam IEnumerator.

```csharp
  public interface IEnumerable
  {
    IEnumerator GetEnumerator();
  }
```

IEnumerator natomiast narzuca nam implementację 3 metod, które będziemy wykorzystywać do przechodzenia po elementach w kolekcji:

```csharp
  public interface IEnumerator
  {
    bool MoveNext();
    object Current { get; }
    void Reset();
  }
```

MoveNext -> przesuwa wskaźnik w pamięci do następnego elementu, jeśli istnieje.

Current -> pobiera aktualny element na który wskazuje wskaźnik w pamięci.

Reset -> ustawia wskaźnik na 0 miejscu, czyli na miejsce **przed pierwszym elementen w kolekcji**, dlatego, że kolekcja może być pusta.

Bez implementacji IEnumerator nasza niestandardowa kolekcja nie będzie miała możliwości wykorzystania metody **foreach.**

Jest różnica pomiędzy IEnumerator, a IEnumerator w wersji generycznej, taka, że wersja generyczna implementuje IDisposable do zwalniania zasobów, a wersja niegeryczna nie implementuje tego interfejsu.

### 2.5 Kolekcje generyczne i niegeneryczne

Kolekcje niegeneryczne to kolekcje, które umożliwiają przechowywanie różnych obiektów, wyróżniają się tym na tle kolekcji generycznych. Jednak mają swoją wadę, wydajność takich kolekcji jest mniejsza ze względu na to, że muszą rzutować każdy obiekt, co w kolekcjach generycznych nie ma miejsca, ze względu na to, że mają narzucony dany typ na starcie.

Aktualnie kolekcje generyczne są nadal rozwijane dodawając im co raz to więcej ich implementacji, np. LinkedList.

Poniżej obrazek przedstawiający kolekcje generyczne i ich odpowiedniki niegeneryczne:

![kolekcje generyczne i ich odpowiedniki niegeneryczne](/Images/generyczne_niegeneryczne_kolekcje.PNG)

### 2.6 Thread-safe w kolekcjach

Thread-safe w kolekcjach oznacza, że możliwa jest praca z kolekcją dla wielu wątków. W przeszłości kolekcje niegeneryczne były z góry bezpieczne wątkowo, dlatego, że umożliwiały blokadę kolekcji do momentu dodania lub usunięcia elementu za pomocą Synchronized. Aktualnie posiadamy do tego kolekcje thread-safe. Ciekawostką jest to, że kolekcje generyczne nie posiadają już opcji Synchronized ze względu na to, że programiści Microsoft po konsultacjach z innymi stwierdzili, że lepiej zwykłe kolekcje zostawić jako jednowątkowe, a dla wielu wątków stworzyć nowe kolekcje, które zoptymalizują pracę na wielu wątkach. Starsza metoda Synchronized nie była wydajna i bardzo mało skalowalna i przy dużym ruchu do kolekcji potrafiła spowodować duże straty wydajności.

#### TODO rozwinac bardziej.

Ciekawostką jest to, ze klasy współbieżne implementują Synchronized i i SyncRoot, jednak zwracają stałe wartości:

> Ponieważ klasy kolekcji współbieżnych obsługują interfejs ICollection, implementują one właściwości IsSynchronized i SyncRoot, nawet jeśli te właściwości są nieistotne. Właściwość IsSynchronized zawsze zwraca wartość false, a właściwość SyncRoot zawsze ma wartość null (Nothing w języku Visual Basic).

### 3. Kolekcje niegeneryczne

### 4. Kolekcje generyczne

### 5. Kolekcje thread-safe

### 6. Niestandardowe implementacje kolekcji w C#

### 7. Własna kolekcja w C#

### 8. Benchmark kolekcji i struktur danych

### 9. Porównywanie kolekcji i struktur danych

### 10. Literatura

http://kurs.aspnetmvc.pl/Csharp/Tablice
http://kurs.aspnetmvc.pl/Csharp/Lancuchy
http://kurs.aspnetmvc.pl/Csharp/Kolekcje
http://kurs.aspnetmvc.pl/Csharp/Kolekcje_generyczne
http://kurs.aspnetmvc.pl/Csharp/Interfejsy_kolekcji_generycznych
http://kurs.aspnetmvc.pl/Csharp/Ienumerable_ienumerator
http://kurs.aspnetmvc.pl/Csharp/Iteratory_yield_return
https://www.plukasiewicz.net/Csharp_dla_zaawansowanych/Kolekcje

https://learn.microsoft.com/pl-pl/dotnet/standard/collections/

https://learn.microsoft.com/pl-pl/dotnet/standard/collections/selecting-a-collection-class

https://learn.microsoft.com/pl-pl/dotnet/csharp/language-reference/builtin-types/collections

https://learn.microsoft.com/pl-pl/dotnet/csharp/language-reference/builtin-types/arrays

https://learn.microsoft.com/pl-pl/dotnet/standard/collections/commonly-used-collection-types

https://learn.microsoft.com/pl-pl/dotnet/standard/collections/when-to-use-generic-collections

https://learn.microsoft.com/pl-pl/dotnet/standard/collections/comparisons-and-sorts-within-collections

https://learn.microsoft.com/pl-pl/dotnet/standard/collections/sorted-collection-types

https://learn.microsoft.com/pl-pl/dotnet/standard/collections/hashtable-and-dictionary-collection-types

https://learn.microsoft.com/pl-pl/dotnet/standard/collections/thread-safe/?toc=%2Fdotnet%2Ffundamentals%2Ftoc.json&bc=%2Fdotnet%2Fbreadcrumb%2Ftoc.json

https://learn.microsoft.com/pl-pl/dotnet/fundamentals/runtime-libraries/system-collections-generic-hashset%7Bt%7D

https://learn.microsoft.com/pl-pl/dotnet/fundamentals/runtime-libraries/system-collections-generic-list%7Bt%7D

https://learn.microsoft.com/pl-pl/dotnet/fundamentals/runtime-libraries/system-collections-objectmodel-observablecollection%7Bt%7D

https://pl.wikipedia.org/wiki/Struktura_danych

https://pl.wikipedia.org/wiki/Struktura_(programowanie)

https://pl.wikipedia.org/wiki/Krotka_(struktura_danych)

https://pl.wikipedia.org/wiki/Tablica_(informatyka)

https://pl.wikipedia.org/wiki/Lista

https://pl.wikipedia.org/wiki/Stos_(informatyka)

https://pl.wikipedia.org/wiki/Kolejka_(informatyka)

https://pl.wikipedia.org/wiki/Drzewo_(informatyka)

https://pl.wikipedia.org/wiki/Graf_(matematyka)

https://pl.wikipedia.org/wiki/Kopiec_(informatyka)

https://www.javatpoint.com/structure-in-c

https://www.geeksforgeeks.org/dynamic-memory-allocation-in-c-using-malloc-calloc-free-and-realloc/
