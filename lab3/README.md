# Основы обработки данных с помощью R и Dplyr
a.nikitchik@yandex.ru

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить практические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by()

## Исходные данные

1.  Программное обеспечение Windows 11
2.  Rstudio Desktop и библиотека Dplyr
3.  Интерпретатор языка R 4.4.2
4.  Пакет dplyr
5.  Пакет nycflights13

## Ход работы

### 1. Установка пакета nycflights13

``` r
#install.packages("nycflights13")
```

### 2. Подгружение библиотек

``` r
library(nycflights13)
library(dplyr)
```


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

### 3. Сколько встроенных в пакет nycflight13 датафреймов?

``` r
length(data(package = "nycflights13")$results[, "Item"])
```

    [1] 5

### 4. Сколько строк в каждом датафрейме?

``` r
list(nrow(airlines), nrow(airports), nrow(flights), nrow(planes), nrow(weather))
```

    [[1]]
    [1] 16

    [[2]]
    [1] 1458

    [[3]]
    [1] 336776

    [[4]]
    [1] 3322

    [[5]]
    [1] 26115

### 5. Сколько столбцов в каждом датафрейме?

``` r
sapply(list(flights, airlines, airports, planes, weather), ncol)
```

    [1] 19  2  8  9 15

### 6. Как посмотреть примерный вид датафрейма?

``` r
flights %>% glimpse()
```

    Rows: 336,776
    Columns: 19
    $ year           <int> 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2…
    $ month          <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
    $ day            <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
    $ dep_time       <int> 517, 533, 542, 544, 554, 554, 555, 557, 557, 558, 558, …
    $ sched_dep_time <int> 515, 529, 540, 545, 600, 558, 600, 600, 600, 600, 600, …
    $ dep_delay      <dbl> 2, 4, 2, -1, -6, -4, -5, -3, -3, -2, -2, -2, -2, -2, -1…
    $ arr_time       <int> 830, 850, 923, 1004, 812, 740, 913, 709, 838, 753, 849,…
    $ sched_arr_time <int> 819, 830, 850, 1022, 837, 728, 854, 723, 846, 745, 851,…
    $ arr_delay      <dbl> 11, 20, 33, -18, -25, 12, 19, -14, -8, 8, -2, -3, 7, -1…
    $ carrier        <chr> "UA", "UA", "AA", "B6", "DL", "UA", "B6", "EV", "B6", "…
    $ flight         <int> 1545, 1714, 1141, 725, 461, 1696, 507, 5708, 79, 301, 4…
    $ tailnum        <chr> "N14228", "N24211", "N619AA", "N804JB", "N668DN", "N394…
    $ origin         <chr> "EWR", "LGA", "JFK", "JFK", "LGA", "EWR", "EWR", "LGA",…
    $ dest           <chr> "IAH", "IAH", "MIA", "BQN", "ATL", "ORD", "FLL", "IAD",…
    $ air_time       <dbl> 227, 227, 160, 183, 116, 150, 158, 53, 140, 138, 149, 1…
    $ distance       <dbl> 1400, 1416, 1089, 1576, 762, 719, 1065, 229, 944, 733, …
    $ hour           <dbl> 5, 5, 5, 5, 6, 5, 6, 6, 6, 6, 6, 6, 6, 6, 6, 5, 6, 6, 6…
    $ minute         <dbl> 15, 29, 40, 45, 0, 58, 0, 0, 0, 0, 0, 0, 0, 0, 0, 59, 0…
    $ time_hour      <dttm> 2013-01-01 05:00:00, 2013-01-01 05:00:00, 2013-01-01 0…

### 7. Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных (представлено в наборах данных)?

``` r
a = flights %>% filter(!is.na(carrier)) %>% distinct(carrier) 
nrow(a)
```

    [1] 16

### 6. Сколько рейсов принял аэропорт John F Kennedy Intl в мае?

``` r
a = flights %>% filter(dest == "JFK", month == 5)
nrow(a)
```

    [1] 0

### 7. Какой самый северный аэропорт?

``` r
airports %>% glimpse()
```

    Rows: 1,458
    Columns: 8
    $ faa   <chr> "04G", "06A", "06C", "06N", "09J", "0A9", "0G6", "0G7", "0P2", "…
    $ name  <chr> "Lansdowne Airport", "Moton Field Municipal Airport", "Schaumbur…
    $ lat   <dbl> 41.13047, 32.46057, 41.98934, 41.43191, 31.07447, 36.37122, 41.4…
    $ lon   <dbl> -80.61958, -85.68003, -88.10124, -74.39156, -81.42778, -82.17342…
    $ alt   <dbl> 1044, 264, 801, 523, 11, 1593, 730, 492, 1000, 108, 409, 875, 10…
    $ tz    <dbl> -5, -6, -6, -5, -5, -5, -5, -5, -5, -8, -5, -6, -5, -5, -5, -5, …
    $ dst   <chr> "A", "A", "A", "A", "A", "A", "A", "A", "U", "A", "A", "U", "A",…
    $ tzone <chr> "America/New_York", "America/Chicago", "America/Chicago", "Ameri…

``` r
airports %>% arrange(desc(lat)) %>% head(1)
```

    # A tibble: 1 × 8
      faa   name                      lat   lon   alt    tz dst   tzone
      <chr> <chr>                   <dbl> <dbl> <dbl> <dbl> <chr> <chr>
    1 EEN   Dillant Hopkins Airport  72.3  42.9   149    -5 A     <NA> 

### 8. Какой аэропорт самый высокогорный (находится выше всех над уровнем моря)?

``` r
airports %>% arrange(desc(alt)) %>% head(1)
```

    # A tibble: 1 × 8
      faa   name        lat   lon   alt    tz dst   tzone         
      <chr> <chr>     <dbl> <dbl> <dbl> <dbl> <chr> <chr>         
    1 TEX   Telluride  38.0 -108.  9078    -7 A     America/Denver

### 9. Какие бортовые номера у самых старых самолетов?

``` r
planes %>% arrange(year) %>% head(10)
```

    # A tibble: 10 × 9
       tailnum  year type              manufacturer model engines seats speed engine
       <chr>   <int> <chr>             <chr>        <chr>   <int> <int> <int> <chr> 
     1 N381AA   1956 Fixed wing multi… DOUGLAS      DC-7…       4   102   232 Recip…
     2 N201AA   1959 Fixed wing singl… CESSNA       150         1     2    90 Recip…
     3 N567AA   1959 Fixed wing singl… DEHAVILLAND  OTTE…       1    16    95 Recip…
     4 N378AA   1963 Fixed wing singl… CESSNA       172E        1     4   105 Recip…
     5 N575AA   1963 Fixed wing singl… CESSNA       210-…       1     6    NA Recip…
     6 N14629   1965 Fixed wing multi… BOEING       737-…       2   149    NA Turbo…
     7 N615AA   1967 Fixed wing multi… BEECH        65-A…       2     9   202 Turbo…
     8 N425AA   1968 Fixed wing singl… PIPER        PA-2…       1     4   107 Recip…
     9 N383AA   1972 Fixed wing multi… BEECH        E-90        2    10    NA Turbo…
    10 N364AA   1973 Fixed wing multi… CESSNA       310Q        2     6   167 Recip…

### 10. Какая средняя температура воздуха была в сентябре в аэропорту John F Kennedy Intl (в градусах Цельсия).

``` r
weather %>% filter(origin == "JFK", month == 9, !is.na(temp)) %>% summarise(mt = mean((temp - 32) * 5 / 9))
```

    # A tibble: 1 × 1
         mt
      <dbl>
    1  19.4

### 11. Самолеты какой авиакомпании совершили больше всего вылетов в июне?

``` r
flights %>% filter(month == 6) %>% count(carrier, sort = TRUE) %>% head(1)
```

    # A tibble: 1 × 2
      carrier     n
      <chr>   <int>
    1 UA       4975

### 12. Самолеты какой авиакомпании задерживались чаще других в 2013 году?

``` r
flights %>% filter(dep_delay > 0 & year == 2013) %>% count(carrier, sort = TRUE) %>% head(1)
```

    # A tibble: 1 × 2
      carrier     n
      <chr>   <int>
    1 UA      27261

## Оценка результата

Были развиты практические навыки использования функций обработки данных
пакета dplyr на примере “nycflight13”

## Вывод

Был получен опыт обработки данных в ходе анализа пакета nycflight13
