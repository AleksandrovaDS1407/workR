# Основы обработки данных с помощью R и Dplyr
deesshii@yandex.ru

## Цель работы:

1.Развить практические навыки использования языка программирования R для
обработки данных 2. Закрепить знания базовых типов данных языка R 3.
Развить практические навыки использования функций обработки данных
пакета dplyr – функции select(), filter(), mutate(), arrange(),
group_by()

## Исходные данные

1.  Программное обеспечение Windows 10
2.  Rstudio Desktop
3.  Наше рабочее окружение

``` r
sessionInfo()
```

    R version 4.5.2 (2025-10-31 ucrt)
    Platform: x86_64-w64-mingw32/x64
    Running under: Windows 11 x64 (build 26100)

    Matrix products: default
      LAPACK version 3.12.1

    locale:
    [1] LC_COLLATE=Russian_Russia.utf8  LC_CTYPE=Russian_Russia.utf8   
    [3] LC_MONETARY=Russian_Russia.utf8 LC_NUMERIC=C                   
    [5] LC_TIME=Russian_Russia.utf8    

    time zone: Europe/Moscow
    tzcode source: internal

    attached base packages:
    [1] stats     graphics  grDevices utils     datasets  methods   base     

    loaded via a namespace (and not attached):
     [1] compiler_4.5.2    fastmap_1.2.0     cli_3.6.5         tools_4.5.2      
     [5] htmltools_0.5.8.1 rstudioapi_0.17.1 yaml_2.3.10       rmarkdown_2.30   
     [9] knitr_1.50        jsonlite_2.0.0    xfun_0.54         digest_0.6.37    
    [13] rlang_1.1.6       evaluate_1.0.5   

## План

Используя R и среду разработки RstudioIDE, выполнить задания.

## Шаги:

    > install.packages("nycflights13")
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакет в ‘C:/Users/futyr/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/nycflights13_1.0.2.zip'
    Content type 'application/zip' length 4511564 bytes (4.3 MB)
    downloaded 4.3 MB

    пакет ‘nycflights13’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\futyr\AppData\Local\Temp\RtmpCidVMC\downloaded_packages
    > #Сколько встроенных в пакет датафреймов?
    > data(package = "nycflights13")$results %>% nrow()
    [1] 5
    > # Сколько строк в каждом датафрейме?
    > data_names <- data(package = "nycflights13")$results[, "Item"]
    > data_list <- list(airlines, airports, flights, planes, weather)
    > 
    > sapply(data_list, nrow)
    [1]     16   1458 336776   3322  26115

    > #Сколько столбцов в каждом датафрейме?
    > sapply(data_list, ncol)
    [1]  2  8 19  9 15
    > #Как просмотреть примерный вид датафрейма?
    > glimpse(flights)
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
    > #Сколько уникальных компаний-перевозчиков (carrier) представлено?
    > n_distinct(flights$carrier)
    [1] 16
    > #Сколько рейсов принял аэропорт John F Kennedy Intl в мае?
    > flights %>%
    +     filter(month == 5 & dest == "JFK") %>%
    +     nrow()
    [1] 0
    > #Какой самый северный аэропорт?
    > airports %>%
    +     filter(lat == max(lat, na.rm = TRUE)) %>%
    +     select(name, lat)
    # A tibble: 1 × 2
      name                      lat
      <chr>                   <dbl>
    1 Dillant Hopkins Airport  72.3
    > #Какой аэропорт самый высокогорный?
    > airports %>%
    +     filter(alt == max(alt, na.rm = TRUE)) %>%
    +     select(name, alt)
    # A tibble: 1 × 2
      name        alt
      <chr>     <dbl>
    1 Telluride  9078
    > #Какие бортовые номера у самых старых самолетов?
    > planes %>%
    +     filter(year == min(year, na.rm = TRUE)) %>%
    +     select(tailnum, year)
    # A tibble: 1 × 2
      tailnum  year
      <chr>   <int>
    1 N381AA   1956
    > #Средняя температура в сентябре в аэропорту JFK (в Цельсиях)
    > weather %>%
    +     filter(month == 9 & origin == "JFK") %>%
    +     summarise(avg_temp_c = mean((temp - 32) * 5/9, na.rm = TRUE))
    # A tibble: 1 × 1
      avg_temp_c
           <dbl>
    1       19.4
    > #Авиакомпания с наибольшим количеством вылетов в июне
    > flights %>%
    +     filter(month == 6) %>%
    +     count(carrier, sort = TRUE) %>%
    +     slice_max(n, n = 1) %>%
    +     left_join(airlines, by = "carrier")
    # A tibble: 1 × 3
      carrier     n name                 
      <chr>   <int> <chr>                
    1 UA       4975 United Air Lines Inc.
    > #Авиакомпания с самыми частыми задержками в 2013 году
    > flights %>%
    +     filter(dep_delay > 0 | arr_delay > 0) %>%
    +     count(carrier, sort = TRUE) %>%
    +     slice_max(n, n = 1) %>%
    +     left_join(airlines, by = "carrier")
    # A tibble: 1 × 3
      carrier     n name                 
      <chr>   <int> <chr>                
    1 UA      32877 United Air Lines Inc.

## Оценка результата

Развили практические навыки использования языка программирования R для
обработки данных. Закрепили знания базовых типов данных языка R. Развили
практические навыки использования функций обработки данных пакета dplyr
– функции select(), filter(), mutate(), arrange(), group_by()

## Вывод

Развили практические навыки использования функций обработки данных
пакета dplyr – функции select(), filter(), mutate(), arrange(),
group_by()
