# Исследование метаданных DNS трафика
deesshii@yandex.ru

## Цель работы:

1.  Зекрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R
3.  Закрепить навыки исследования метаданных DNS трафика

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

    > install.packages(c("tidyverse", "lubridate", "jsonlite", "httr"))
    WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

    https://cran.rstudio.com/bin/windows/Rtools/
    Устанавливаю пакеты в ‘C:/Users/futyr/AppData/Local/R/win-library/4.5’
    (потому что ‘lib’ не определено)
    устанавливаю также зависимости ‘bit’, ‘farver’, ‘labeling’, ‘RColorBrewer’, ‘viridisLite’, ‘rematch’, ‘bit64’, ‘prettyunits’, ‘backports’, ‘blob’, ‘DBI’, ‘data.table’, ‘gtable’, ‘isoband’, ‘S7’, ‘scales’, ‘gargle’, ‘uuid’, ‘cellranger’, ‘ids’, ‘rematch2’, ‘cpp11’, ‘systemfonts’, ‘textshaping’, ‘clipr’, ‘vroom’, ‘tzdb’, ‘progress’, ‘selectr’, ‘broom’, ‘conflicted’, ‘dbplyr’, ‘dtplyr’, ‘forcats’, ‘ggplot2’, ‘googledrive’, ‘googlesheets4’, ‘haven’, ‘hms’, ‘modelr’, ‘purrr’, ‘ragg’, ‘readr’, ‘readxl’, ‘reprex’, ‘rvest’, ‘tidyr’, ‘xml2’, ‘timechange’

      В наличии есть бинарные версии, но исходники новее:
            binary source needs_compilation
    vroom    1.6.6  1.6.7              TRUE
    selectr  0.4-2  0.5-0             FALSE
    xml2     1.4.1  1.5.0              TRUE

      Binaries will be installed
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/bit_4.6.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/farver_2.1.2.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/labeling_0.4.3.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/RColorBrewer_1.1-3.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/viridisLite_0.4.2.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/rematch_2.0.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/bit64_4.6.0-1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/prettyunits_1.2.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/backports_1.5.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/blob_1.2.4.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/DBI_1.2.3.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/data.table_1.17.8.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/gtable_0.3.6.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/isoband_0.2.7.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/S7_0.2.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/scales_1.4.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/gargle_1.6.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/uuid_1.2-1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/cellranger_1.1.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/ids_1.0.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/rematch2_2.1.2.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/cpp11_0.5.2.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/systemfonts_1.3.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/textshaping_1.0.4.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/clipr_0.8.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/vroom_1.6.6.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/tzdb_0.5.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/progress_1.2.3.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/broom_1.0.10.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/conflicted_1.2.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/dbplyr_2.5.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/dtplyr_1.3.2.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/forcats_1.0.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/ggplot2_4.0.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/googledrive_2.1.2.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/googlesheets4_1.1.2.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/haven_2.5.5.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/hms_1.1.4.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/modelr_0.1.11.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/purrr_1.2.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/ragg_1.5.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/readr_2.1.6.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/readxl_1.4.5.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/reprex_2.1.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/rvest_1.0.5.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/tidyr_1.3.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/xml2_1.4.1.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/timechange_0.3.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/tidyverse_2.0.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/lubridate_1.9.4.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/jsonlite_2.0.0.zip'
    пробую URL 'https://cran.rstudio.com/bin/windows/contrib/4.5/httr_1.4.7.zip'
    пакет ‘bit’ успешно распакован, MD5-суммы проверены
    пакет ‘farver’ успешно распакован, MD5-суммы проверены
    пакет ‘labeling’ успешно распакован, MD5-суммы проверены
    пакет ‘RColorBrewer’ успешно распакован, MD5-суммы проверены
    пакет ‘viridisLite’ успешно распакован, MD5-суммы проверены
    пакет ‘rematch’ успешно распакован, MD5-суммы проверены
    пакет ‘bit64’ успешно распакован, MD5-суммы проверены
    пакет ‘prettyunits’ успешно распакован, MD5-суммы проверены
    пакет ‘backports’ успешно распакован, MD5-суммы проверены
    пакет ‘blob’ успешно распакован, MD5-суммы проверены
    пакет ‘DBI’ успешно распакован, MD5-суммы проверены
    пакет ‘data.table’ успешно распакован, MD5-суммы проверены
    пакет ‘gtable’ успешно распакован, MD5-суммы проверены
    пакет ‘isoband’ успешно распакован, MD5-суммы проверены
    пакет ‘S7’ успешно распакован, MD5-суммы проверены
    пакет ‘scales’ успешно распакован, MD5-суммы проверены
    пакет ‘gargle’ успешно распакован, MD5-суммы проверены
    пакет ‘uuid’ успешно распакован, MD5-суммы проверены
    пакет ‘cellranger’ успешно распакован, MD5-суммы проверены
    пакет ‘ids’ успешно распакован, MD5-суммы проверены
    пакет ‘rematch2’ успешно распакован, MD5-суммы проверены
    пакет ‘cpp11’ успешно распакован, MD5-суммы проверены
    пакет ‘systemfonts’ успешно распакован, MD5-суммы проверены
    пакет ‘textshaping’ успешно распакован, MD5-суммы проверены
    пакет ‘clipr’ успешно распакован, MD5-суммы проверены
    пакет ‘vroom’ успешно распакован, MD5-суммы проверены
    пакет ‘tzdb’ успешно распакован, MD5-суммы проверены
    пакет ‘progress’ успешно распакован, MD5-суммы проверены
    пакет ‘broom’ успешно распакован, MD5-суммы проверены
    пакет ‘conflicted’ успешно распакован, MD5-суммы проверены
    пакет ‘dbplyr’ успешно распакован, MD5-суммы проверены
    пакет ‘dtplyr’ успешно распакован, MD5-суммы проверены
    пакет ‘forcats’ успешно распакован, MD5-суммы проверены
    пакет ‘ggplot2’ успешно распакован, MD5-суммы проверены
    пакет ‘googledrive’ успешно распакован, MD5-суммы проверены
    пакет ‘googlesheets4’ успешно распакован, MD5-суммы проверены
    пакет ‘haven’ успешно распакован, MD5-суммы проверены
    пакет ‘hms’ успешно распакован, MD5-суммы проверены
    пакет ‘modelr’ успешно распакован, MD5-суммы проверены
    пакет ‘purrr’ успешно распакован, MD5-суммы проверены
    пакет ‘ragg’ успешно распакован, MD5-суммы проверены
    пакет ‘readr’ успешно распакован, MD5-суммы проверены
    пакет ‘readxl’ успешно распакован, MD5-суммы проверены
    пакет ‘reprex’ успешно распакован, MD5-суммы проверены
    пакет ‘rvest’ успешно распакован, MD5-суммы проверены
    пакет ‘tidyr’ успешно распакован, MD5-суммы проверены
    пакет ‘xml2’ успешно распакован, MD5-суммы проверены
    пакет ‘timechange’ успешно распакован, MD5-суммы проверены
    пакет ‘tidyverse’ успешно распакован, MD5-суммы проверены
    пакет ‘lubridate’ успешно распакован, MD5-суммы проверены
    пакет ‘jsonlite’ успешно распакован, MD5-суммы проверены
    пакет ‘httr’ успешно распакован, MD5-суммы проверены

    Скачанные бинарные пакеты находятся в
        C:\Users\futyr\AppData\Local\Temp\Rtmp6RSMwW\downloaded_packages
    устанавливаю пакет ‘selectr’ из исходников
    пробую URL 'https://cran.rstudio.com/src/contrib/selectr_0.5-0.tar.gz'
    Content type 'application/x-gzip' length 54811 bytes (53 KB)
    downloaded 53 KB

    * installing *source* package 'selectr' ...
    ** this is package 'selectr' version '0.5-0'
    ** пакет 'selectr' удачно распакован, MD5 sums проверены
    ** using staged installation
    ** R
    ** inst
    ** byte-compile and prepare package for lazy loading
    ** help
    *** installing help indices
    ** building package indices
    ** testing if installed package can be loaded from temporary location
    ** testing if installed package can be loaded from final location
    ** testing if installed package keeps a record of temporary installation path
    * DONE (selectr)

    Скачанные исходники пакетов находятся в
        ‘C:\Users\futyr\AppData\Local\Temp\Rtmp6RSMwW\downloaded_packages’
    > library(tidyverse)
    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.6
    ✔ forcats   1.0.1     ✔ stringr   1.5.2
    ✔ ggplot2   4.0.1     ✔ tibble    3.3.0
    ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ✔ purrr     1.2.0     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package to force all conflicts to become errors
    > library(lubridate)
    > library(jsonlite)

    Присоединяю пакет: ‘jsonlite’

    Следующий объект скрыт от ‘package:purrr’:

        flatten
    > library(httr)
    > library(httr)
    > # Загрузка данных
    > temp_file <- tempfile()
    > download.file("https://storage.yandexcloud.net/dataset.ctfsec/dns.zip", temp_file)
    пробую URL 'https://storage.yandexcloud.net/dataset.ctfsec/dns.zip'
    Content type 'application/zip' length 6407934 bytes (6.1 MB)
    downloaded 6.1 MB

    > unzip(temp_file)
    > dns_data <- read_tsv("dns.log", comment = "#", col_names = FALSE)
    Rows: 427935 Columns: 23                                                        
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: "\t"
    chr (13): X2, X3, X5, X7, X9, X10, X11, X12, X13, X14, X15, X21, X22
    dbl  (5): X1, X4, X6, X8, X20
    lgl  (5): X16, X17, X18, X19, X23

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    > # Добавление названий столбцов (согласно формату Zeek DNS)
    > colnames(dns_data) <- c("ts", "uid", "id.orig_h", "id.orig_p", "id.resp_h", "id.resp_p",
    +                         "proto", "trans_id", "query", "qclass", "qclass_name", 
    +                         "qtype", "qtype_name", "rcode", "rcode_name", "AA", "TC",
    +                         "RD", "RA", "Z", "answers", "TTLs", "rejected")
    > # Преобразование форматов данных
    > dns_data <- dns_data %>%
    +     mutate(
    +         ts = as_datetime(ts),
    +         query = as.character(query),
    +         id.orig_h = as.character(id.orig_h),
    +         id.resp_h = as.character(id.resp_h)
    +     )
    > # Просмотр структуры данных
    > glimpse(dns_data)
    Rows: 427,935
    Columns: 23
    $ ts          <dttm> 2012-03-16 12:30:05, 2012-03-16 12:30:15, 2012-03-16 12:3…
    $ uid         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz7Bs…
    $ id.orig_h   <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", "19…
    $ id.orig_p   <dbl> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 1…
    $ id.resp_h   <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255", "1…
    $ id.resp_p   <dbl> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ proto       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "u…
    $ trans_id    <dbl> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187, 62…
    $ query       <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\…
    $ qclass      <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1"…
    $ qclass_name <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C…
    $ qtype       <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "32"…
    $ qtype_name  <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB…
    $ rcode       <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rcode_name  <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-…
    $ AA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ TC          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ RD          <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRU…
    $ RA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ Z           <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0…
    $ answers     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ TTLs        <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rejected    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    > 
    > 
    > # 4. Сколько участников информационного обмена в сети?
    > internal_ips <- unique(c(dns_data$id.orig_h, dns_data$id.resp_h))
    > cat("Всего участников:", length(internal_ips), "\n")
    Всего участников: 1359 
    > 
    > # 5. Соотношение внутренних и внешних участников
    > internal_network <- "192.168."  # предположим, что это внутренняя сеть
    > internal_count <- sum(str_detect(internal_ips, internal_network))
    > external_count <- length(internal_ips) - internal_count
    > cat("Внутренние участники:", internal_count, "\n")
    Внутренние участники: 1238 
    > cat("Внешние участники:", external_count, "\n")
    Внешние участники: 121 
    > cat("Соотношение:", round(internal_count/external_count, 2), "\n")
    Соотношение: 10.23 
    > 
    > # 6. Топ-10 самых активных участников сети
    > top_active_ips <- dns_data %>%
    + count(id.orig_h, sort = TRUE) %>%
    + head(10)
    > print("Топ-10 активных участников:")
    [1] "Топ-10 активных участников:"
    > print(top_active_ips)
    # A tibble: 10 × 2
       id.orig_h           n
       <chr>           <int>
     1 10.10.117.210   75943
     2 192.168.202.93  26522
     3 192.168.202.103 18121
     4 192.168.202.76  16978
     5 192.168.202.97  16176
     6 192.168.202.141 14967
     7 10.10.117.209   14222
     8 192.168.202.110 13372
     9 192.168.203.63  12148
    10 192.168.202.106 10784
    > 
    > # 7. Топ-10 доменов по количеству обращений
    > top_domains <- dns_data %>%
    +     count(query, sort = TRUE) %>%
    +     head(10)
    > print("Топ-10 доменов:")
    [1] "Топ-10 доменов:"
    > print(top_domains)
    # A tibble: 10 × 2
       query                                                                       n
       <chr>                                                                   <int>
     1 "teredo.ipv6.microsoft.com"                                             39273
     2 "tools.google.com"                                                      14057
     3 "www.apple.com"                                                         13390
     4 "time.apple.com"                                                        13109
     5 "safebrowsing.clients.google.com"                                       11658
     6 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x… 10401
     7 "WPAD"                                                                   9134
     8 "44.206.168.192.in-addr.arpa"                                            7248
     9 "HPE8AA67"                                                               6929
    10 "ISATAP"                                                                 6569
    > 
    > # 8. Статистические характеристики интервалов времени для топ-10 доменов
    > top_domains_list <- top_domains$query
    > 
    > time_intervals <- dns_data %>%
    +     filter(query %in% top_domains_list) %>%
    +     arrange(query, ts) %>%
    +     group_by(query) %>%
    +     mutate(time_diff = as.numeric(ts - lag(ts))) %>%
    +     filter(!is.na(time_diff))
    > 
    > print("Статистика интервалов времени:")
    [1] "Статистика интервалов времени:"
    > summary(time_intervals$time_diff)
         Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
        0.000     0.000     0.750     8.758     1.740 52723.500 
    > 
    > # 9. Поиск скрытых DNS каналов (периодические запросы)
    > suspicious_ips <- dns_data %>%
    +     group_by(id.orig_h, query) %>%
    +     summarise(
    +         request_count = n(),
    +         time_span = as.numeric(max(ts) - min(ts)),
    +         avg_interval = time_span / request_count,
    +         .groups = 'drop'
    +     ) %>%
    +     filter(request_count > 10, avg_interval < 3600) %>%  
    +     arrange(avg_interval)
    > 
    > print("Подозрительные IP с возможными DNS каналами:")
    [1] "Подозрительные IP с возможными DNS каналами:"
    > print(suspicious_ips)
    # A tibble: 3,015 × 5
       id.orig_h      query                     request_count time_span avg_interval
       <chr>          <chr>                             <int>     <dbl>        <dbl>
     1 10.10.117.210  hq.h                                377      0        0       
     2 10.10.117.210  httphq.hec.net                      102      0        0       
     3 10.10.117.210  www.h                                64      0        0       
     4 192.168.202.93 www.apple.com                     10852      1.33     0.000122
     5 10.10.117.210  teredo.ipv6.microsoft.com         27425      3.50     0.000128
     6 192.168.202.83 44.206.168.192.in-addr.a…          7248      1.34     0.000185
     7 192.168.203.63 imap.gmail.com                     5543      1.06     0.000192
     8 192.168.202.76 HPE8AA67                           6929      1.33     0.000192
     9 192.168.202.93 time.apple.com                     6038      1.31     0.000218
    10 192.168.202.76 WPAD                               5175      1.33     0.000257
    # ℹ 3,005 more rows
    # ℹ Use `print(n = ...)` to see more rows
    >
     # Исправленная функция для получения геоданных
    > get_ip_info <- function(ip) {
    +     # Правильное регулярное выражение для проверки IP
    +     if (str_detect(ip, "^(?:[0-9]{1,3}\\.){3}[0-9]{1,3}$")) {
    +         url <- paste0("http://ip-api.com/json/", ip)
    +         response <- GET(url)
    +         if (status_code(response) == 200) {
    +             data <- fromJSON(content(response, "text"))
    +             # Проверяем, что данные получены успешно
    +             if (data$status == "success") {
    +                 return(data.frame(
    +                     ip = ip,
    +                     country = data$country,
    +                     city = data$city,
    +                     isp = data$isp,
    +                     stringsAsFactors = FALSE
    +                 ))
    +             }
    +         }
    +     }
    +     # Возвращаем NA если что-то пошло не так
    +     return(data.frame(ip = ip, country = NA, city = NA, isp = NA, stringsAsFactors = FALSE))
    + }
    > 
    > # Получаем IP адреса для топ-10 доменов
    > domain_ips <- dns_data %>%
    +     filter(query %in% top_domains_list) %>%
    +     distinct(query, id.resp_h) %>%
    +     head(10)
    > 
    > # Проверим какие IP мы получили
    > print("IP адреса для анализа:")
    [1] "IP адреса для анализа:"
    > print(domain_ips)
    # A tibble: 10 × 2
       query                                                               id.resp_h
       <chr>                                                               <chr>    
     1 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x0… 192.168.…
     2 "HPE8AA67"                                                          192.168.…
     3 "WPAD"                                                              192.168.…
     4 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x0… 10.7.136…
     5 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x0… 192.168.…
     6 "www.apple.com"                                                     172.19.1…
     7 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x0… 10.7.137…
     8 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x0… 10.7.136…
     9 "safebrowsing.clients.google.com"                                   192.168.…
    10 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x0… 10.7.136…
    > 
    > # Получаем информацию о местоположении (с обработкой ошибок)
    > geo_info <- map_df(domain_ips$id.resp_h, safely(get_ip_info))
    > 
    > # Извлекаем только успешные результаты
    > geo_info_success <- geo_info$result %>% bind_rows()
    > 
    > print("Географическая информация:")
    [1] "Географическая информация:"
    > print(geo_info_success)
                    ip country city isp
    1   192.168.27.203      NA   NA  NA
    2  192.168.202.255      NA   NA  NA
    3  192.168.202.255      NA   NA  NA
    4     10.7.136.159      NA   NA  NA
    5   192.168.27.202      NA   NA  NA
    6     172.19.1.100      NA   NA  NA
    7     10.7.137.108      NA   NA  NA
    8     10.7.136.109      NA   NA  NA
    9    192.168.207.4      NA   NA  NA
    10     10.7.136.63      NA   NA  NA
    > 

## Оценка результата

В результате лабораторной работы мы зекрепили практические навыки
использования языка программирования R для обработки данных. Закрепили
знания основных функций обработки данных экосистемы tidyverse языка R.
Закрепили навыки исследования метаданных DNS трафика

## Вывод

В результате лабораторной работы мы зекрепили практические навыки
использования языка программирования R для обработки данных.
