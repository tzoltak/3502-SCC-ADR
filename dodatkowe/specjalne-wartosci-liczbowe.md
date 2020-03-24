Wprowadzenie
============

Pewne operacje są w matematyce w zasadzie niedozwolone, lub nie mają
sensu, np. dzielenie przez zero, czy wykonywanie jakichkolwiek operacji
arytmetycznych na nieskończonościach. Próba ich wykonania w R nie wywoła
jednak błędu, za to spowoduje zwrócenie w wyniku jednej ze specjalnych
wartości liczbowych.

Nieskończoność
==============

W R element wektora liczb może mieć wartość *nieskończoność*: `Inf` lub
*minus nieskończoność*: `-Inf` i **nie są** one traktowane jak braki
danych! Wartości te możemy wykorzystać do porównań większe/mniejsze:

``` r
> Inf > 0
[1] TRUE
> c(1, 0, -Inf) >= 0
[1]  TRUE  TRUE FALSE
> Inf > 10^308
[1] TRUE
> # ale
> Inf > 10^309
[1] FALSE
> # bo
> 10^308  # to się jeszcze daje reprezentować w typie danych 'numeric'
[1] 1e+308
> 10^309  # a to jest już zbyt duża liczba
[1] Inf
> Inf == Inf
[1] TRUE
> # Inf to nie brak danych!
> is.na(Inf)
[1] FALSE
```

Poddają się też one operacjom arytmetycznym (z pewnym ograniczeniem
w zakresie odejmowania i dzielenia), choć w większości wypadków zwracają
w nich siebie lub minus siebie:

``` r
> Inf + 2
[1] Inf
> Inf + Inf
[1] Inf
> Inf * 5
[1] Inf
> Inf * -5
[1] -Inf
> Inf * Inf
[1] Inf
> Inf / 123
[1] Inf
> # ale (p. sekcja "Nie liczba" poniżej)
> Inf - Inf
[1] NaN
> Inf / Inf
[1] NaN
```

Wartości `Inf` lub `-Inf` typowo uzyskujemy w wyniku wykonania jednej
z dwóch operacji:

1.  Podania jako argumentu funkcji `min()` lub `max()` wektora
    zawierającego same braki danych, przy podaniu wartości argumentu
    `na.rm = TRUE` (poskutkuje to wywołaniem ostrzeżenia).

``` r
> min(NA, na.rm = TRUE)
Warning in min(NA, na.rm = TRUE): brak argumentów w min; zwracanie wartości Inf
[1] Inf
> max(NA, na.rm = TRUE)
Warning in max(NA, na.rm = TRUE): brak argumentów w max; zwracanie wartości -Inf
[1] -Inf
```

1.  Podzieleniu liczby (nie będącej zerem) przez zero (zwróćmy uwagę, że
    oznacza to przyjęcie milczącego założenia, że tak na prawdę to nie
    chcieliśmy podzielić przez zero, tylko coś bardzo, bardzo małego -
    na tyle, że zostało zapisane jako zero, bo w programie zabrakło nam
    precyzji pozwalającej reprezentować tak małe liczby).

``` r
> 10 / 0
[1] Inf
> -5 / 0
[1] -Inf
> -5 / (10 / 10^309)
[1] -Inf
```

“Nie liczba”
============

Wartość “Nie liczba”: `NaN` jest specyficznym rodzajem braku danych,
który uzyskamy, jeśli spróbujemy wykonać jedną z trzech operacji:

1.  Podzielić zero przez zero.
2.  Odjąć *nieskończoność* od *nieskończoności*.
3.  Podzielić *nieskończoność* przez *nieskończoność*.

``` r
> 0 / 0
[1] NaN
> Inf - Inf
[1] NaN
> Inf / Inf
[1] NaN
> 
> is.na(NaN)
[1] TRUE
```

Wartość `NaN` jest uznawana za brak danych. Tyle tylko, że niesie ze
sobą dodatkową inforację, że powstał on w wyniku próby wykonania
*nielegalnej* operacji arytmetycznej.
