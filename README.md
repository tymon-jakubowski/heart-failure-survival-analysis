# Analiza statystyczna i predykcja śmiertelności w niewydolności serca

Projekt z zakresu analizy danych i uczenia maszynowego w języku **R**, którego celem jest kompleksowa analiza statystyczna zbioru danych klinicznych pacjentów z niewydolnością serca oraz identyfikacja najważniejszych predyktorów zgonu. Projekt łączy klasyczne metody statystyczne (testy normalności, wielomodalności, detekcja outlierów) z technikami uczenia maszynowego (PCA, K-means, Random Forest).

## Spis treści
- [Opis projektu](#opis-projektu)
- [Zbiór danych](#zbiór-danych)
- [Zastosowane technologie](#zastosowane-technologie)
- [Struktura projektu](#struktura-projektu)
- [Etapy analizy](#etapy-analizy)
- [Kluczowe wnioski](#kluczowe-wnioski)
- [Jak uruchomić](#jak-uruchomić)

## Opis projektu

Projekt realizuje pełen pipeline analizy danych medycznych — od eksploracji i diagnostyki rozkładów, przez redukcję wymiarowości i klasteryzację, po budowę modelu klasyfikacji. Cele szczegółowe:

1. Kompleksowy opis statystyczny zmiennych (statystyki sumaryczne, skośność)
2. Wykrycie obserwacji odstających oraz ocena rozkładów (normalność, wielomodalność)
3. Analiza korelacji i redukcja wymiarowości (PCA)
4. Klasteryzacja bez nadzoru (K-means) z optymalizacją liczby klastrów metodą łokcia
5. Klasyfikacja pod nadzorem (Random Forest) i identyfikacja najważniejszych cech wpływających na ryzyko zgonu

## Zbiór danych

**Heart Failure Clinical Records Dataset** — 299 pacjentów, 13 zmiennych klinicznych.

| Zmienna | Opis |
|---|---|
| `age` | Wiek pacjenta |
| `anaemia` | Niedokrwistość (0/1) |
| `creatinine_phosphokinase` | Poziom enzymu CPK |
| `diabetes` | Cukrzyca (0/1) |
| `ejection_fraction` | Frakcja wyrzutowa serca (%) |
| `high_blood_pressure` | Nadciśnienie (0/1) |
| `platelets` | Liczba płytek krwi |
| `serum_creatinine` | Poziom kreatyniny |
| `serum_sodium` | Poziom sodu |
| `sex` | Płeć (0 = K, 1 = M) |
| `smoking` | Palenie (0/1) |
| `time` | Czas obserwacji (dni) |
| `DEATH_EVENT` | **Zmienna celu** — zgon pacjenta (0/1) |

## Zastosowane technologie

- **R** + **R Markdown** (raport HTML)
- **randomForest** — model klasyfikacji i ocena ważności cech
- **cluster** — analiza skupień
- **moments** — skośność, kurtoza
- **outliers** — test Grubbsa
- **nortest** — testy normalności
- **diptest** — test wielomodalności Hartigana
- **heatmaply** — interaktywna macierz korelacji
- **plotly** — wizualizacja PCA w 3D


## Etapy analizy

1. **Wczytanie danych i konwersja zmiennych binarnych na `factor`**
2. **Statystyki sumaryczne** dla zmiennych ciągłych (min, max, mediana, średnia, SD, skośność)
3. **Histogramy** wszystkich zmiennych ciągłych
4. **Testy normalności** (Shapiro-Wilka) — dla `age`, `serum_creatinine`, `ejection_fraction`, `platelets`
5. **Testy wielomodalności** (Hartigan's Dip Test) — wykrycie podgrup pacjentów
6. **Test Grubbsa** dla wartości odstających w każdej zmiennej ciągłej
7. **Analiza korelacji** — macierz korelacji Pearsona (wizualizacja `heatmaply`)
8. **PCA** — redukcja wymiarowości, interpretacja ładunków składowych, wizualizacja 3D z podziałem na `DEATH_EVENT`
9. **K-means** — klasteryzacja ze skalowaniem, tabela kontyngencji vs `DEATH_EVENT`, metoda łokcia
10. **Random Forest** — walidacja krzyżowa (`rfcv`), model z wszystkimi zmiennymi, analiza ważności cech (`varImpPlot`)

## Kluczowe wnioski

**Charakterystyka danych:**
- Wszystkie zmienne ciągłe mają rozkłady istotnie odbiegające od normalnego (p < 0.05 w teście Shapiro-Wilka)
- Test Hartigana wskazał **wielomodalność** zmiennych `creatinine_phosphokinase` i `ejection_fraction` — sugeruje to istnienie podgrup pacjentów (np. z różnym stopniem uszkodzenia mięśnia sercowego)
- Test Grubbsa potwierdził istotne wartości odstające, szczególnie w CPK (np. wartości > 7000)

**Kluczowe predyktory zgonu (Random Forest):**
1. `ejection_fraction` — niższa frakcja wyrzutowa → wyższe ryzyko
2. `serum_creatinine` — wyższy poziom kreatyniny → wyższe ryzyko
3. `age` — wiek jako istotny czynnik ryzyka

**PCA i klasteryzacja:**
- Pierwsze 3 składowe główne wyjaśniają ok. 60% wariancji
- Klasteryzacja K-means częściowo odzwierciedla podział na grupy ryzyka, ale nie pokrywa się w pełni ze zmienną `DEATH_EVENT`



## Autor

Tymon Jakubowski — projekt zaliczeniowy z Pakietów Statystycznych
