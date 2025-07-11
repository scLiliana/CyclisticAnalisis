# Cyclistic

## Escenario

Eres un analista de datos júnior que trabaja en el equipo de analistas
de marketing de Cyclistic, una empresa de bicicletas compartidas de
Chicago. La directora de marketing cree que el éxito futuro de la
empresa depende de maximizar la cantidad de membresías anuales. Por lo
tanto, tu equipo quiere entender qué diferencias existen en el uso de
las bicicletas Cyclistic entre los ciclistas ocasionales y los miembros
anuales. A través de estos conocimientos, tu equipo diseñará una nueva
estrategia de marketing para convertir a los ciclistas ocasionales en
miembros anuales. Sin embargo, antes de eso, los ejecutivos de Cyclistic
deben aprobar tus recomendaciones; por eso, debes respaldar tu propuesta
con una visión convincente de los datos y visualizaciones profesionales
de los mismos.

## Tarea empresarial

La tarea empresarial se centra en utilizar el análisis de datos para
entender el comportamiento de los usuarios y guiar una estrategia de
marketing que convierta a ciclistas ocasionales en miembros anuales,
asegurando el crecimiento sostenible de Cyclistic.

## Fuentes de datos utilizadas

<https://divvy-tripdata.s3.amazonaws.com/index.html>

Serán utilizados los datos históricos de los viajes de Cyclistic (una
empresa ficticia) proporcionados por Motivate International Inc. Estos
son datos públicos y pertinentes para realizar este análisis.

No hay problemas con la credibilidad de estos datos, pues son datos
proporcionados de una sola fuente confiable, son originales,
entendibles, y actuales. Los datos han sido obtenidos bajo una licencia,
respecto a la privacidad los datos han sido tratados para evitar
conectar con los datos personales de los usuarios.

## Primera visualización de los datos

    #install.packages("tidyverse")
    #install.packages("ggrepel")
    #install.packages("lubridate")
    #install.packages("hms")

    library(tidyverse)

    ## Warning: package 'tidyverse' was built under R version 4.5.1

    ## Warning: package 'lubridate' was built under R version 4.5.1

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.2     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.4     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

    library(ggrepel)

    ## Warning: package 'ggrepel' was built under R version 4.5.1

    library(lubridate)
    library(hms)

    ## Warning: package 'hms' was built under R version 4.5.1

    ## 
    ## Adjuntando el paquete: 'hms'
    ## 
    ## The following object is masked from 'package:lubridate':
    ## 
    ##     hms

    library(readr)

Explorando cada archivo descargado (correspondientes a mayo 2024 a abril
2025), cada uno correspondiente a un mes, vemos que los datos están
organizados por mes, en tablas donde los nombres de las columnas son:
“ride\_id”, “rideable\_type”, “started\_at”, “ended\_at”,
“start\_station\_name”, “start\_station\_id”, “end\_station\_name”,
“end\_station\_id”, “start\_lat”, “start\_lng”, “end\_lat”, “end\_lng”,
“member\_casual”. Todas las columnas coinciden en tipo de dato con las
de los otros meses, por lo que podemos unir estos datos.

Para hacer la primera visualización de los datos, cargarémos los datos
descargados y los uniremos en un solo archivo para verificar la
integridad de los datos.

    # Crear una lista para almacenar los datos temporales
    datos_combinados <- list()

    # Vector con los meses (asumiendo que son consecutivos)
    meses24 <- sprintf("%02d", 5:12) # Formato 01, 02, ..., 12

    # Año de los datos 2024
    anio <- "2024"

    for (mes in meses24) {
      # Construir el nombre del archivo
      nombre_archivo <- paste0("Datos/", anio, mes, "-divvy-tripdata.csv")
      
      # Cargar el archivo
      datos_mes <- read_csv(nombre_archivo)
      
      # Almacenar en la lista
      datos_combinados[[mes]] <- datos_mes
    }

    ## Rows: 609493 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 710721 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 748962 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 755639 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 821276 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 616281 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 335075 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 178372 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    # Vector con los meses (asumiendo que son consecutivos)
    meses25 <- sprintf("%02d", 1:4) # Formato 01, 02, ..., 12

    # Año de los datos 2025
    anio <- "2025"

    for (mes in meses25) {
      # Construir el nombre del archivo
      nombre_archivo <- paste0("Datos/", anio, mes, "-divvy-tripdata.csv")
      
      # Cargar el archivo
      datos_mes <- read_csv(nombre_archivo)
      
      # Almacenar en la lista
      datos_combinados[[mes]] <- datos_mes
    }

    ## Rows: 138689 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 151880 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 298155 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 371341 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    # Combinar todos los data frames en uno solo
    datos <- bind_rows(datos_combinados)

    # Opcional: guardar el archivo combinado
    write_csv(datos, "Datos/052024_042025.csv")

Cargamos el archivo generado

    datos <- read_csv("Datos/052024_042025.csv")

    ## Rows: 5735884 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Realizamos un exploración de los datos

    head(datos)

    ## # A tibble: 6 × 13
    ##   ride_id          rideable_type started_at          ended_at           
    ##   <chr>            <chr>         <dttm>              <dttm>             
    ## 1 7D9F0CE9EC2A1297 classic_bike  2024-05-25 15:52:42 2024-05-25 16:11:50
    ## 2 02EC47687411416F classic_bike  2024-05-14 15:11:51 2024-05-14 15:22:00
    ## 3 101370FB2D3402BE classic_bike  2024-05-30 17:46:04 2024-05-30 18:09:16
    ## 4 E97E396331ED6913 electric_bike 2024-05-17 20:21:54 2024-05-17 20:40:32
    ## 5 674EDE311C543165 classic_bike  2024-05-22 18:52:20 2024-05-22 18:59:04
    ## 6 2E3EA4C19F0341A6 electric_bike 2024-05-25 19:32:12 2024-05-25 19:36:17
    ## # ℹ 9 more variables: start_station_name <chr>, start_station_id <chr>,
    ## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
    ## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>

    # Estructura
    str(datos)

    ## spc_tbl_ [5,735,884 × 13] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:5735884] "7D9F0CE9EC2A1297" "02EC47687411416F" "101370FB2D3402BE" "E97E396331ED6913" ...
    ##  $ rideable_type     : chr [1:5735884] "classic_bike" "classic_bike" "classic_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:5735884], format: "2024-05-25 15:52:42" "2024-05-14 15:11:51" ...
    ##  $ ended_at          : POSIXct[1:5735884], format: "2024-05-25 16:11:50" "2024-05-14 15:22:00" ...
    ##  $ start_station_name: chr [1:5735884] "Streeter Dr & Grand Ave" "Sheridan Rd & Greenleaf Ave" "Streeter Dr & Grand Ave" "Streeter Dr & Grand Ave" ...
    ##  $ start_station_id  : chr [1:5735884] "13022" "KA1504000159" "13022" "13022" ...
    ##  $ end_station_name  : chr [1:5735884] "Clark St & Elm St" "Sheridan Rd & Loyola Ave" "Wabash Ave & 9th St" "Sheffield Ave & Wellington Ave" ...
    ##  $ end_station_id    : chr [1:5735884] "TA1307000039" "RP-009" "TA1309000010" "TA1307000052" ...
    ##  $ start_lat         : num [1:5735884] 41.9 42 41.9 41.9 41.9 ...
    ##  $ start_lng         : num [1:5735884] -87.6 -87.7 -87.6 -87.6 -87.6 ...
    ##  $ end_lat           : num [1:5735884] 41.9 42 41.9 41.9 41.9 ...
    ##  $ end_lng           : num [1:5735884] -87.6 -87.7 -87.6 -87.7 -87.6 ...
    ##  $ member_casual     : chr [1:5735884] "casual" "casual" "member" "member" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   ride_id = col_character(),
    ##   ..   rideable_type = col_character(),
    ##   ..   started_at = col_datetime(format = ""),
    ##   ..   ended_at = col_datetime(format = ""),
    ##   ..   start_station_name = col_character(),
    ##   ..   start_station_id = col_character(),
    ##   ..   end_station_name = col_character(),
    ##   ..   end_station_id = col_character(),
    ##   ..   start_lat = col_double(),
    ##   ..   start_lng = col_double(),
    ##   ..   end_lat = col_double(),
    ##   ..   end_lng = col_double(),
    ##   ..   member_casual = col_character()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

    # Variables contenidas
    colnames(datos)

    ##  [1] "ride_id"            "rideable_type"      "started_at"        
    ##  [4] "ended_at"           "start_station_name" "start_station_id"  
    ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
    ## [10] "start_lng"          "end_lat"            "end_lng"           
    ## [13] "member_casual"

    # Número de datos
    nrow(datos)

    ## [1] 5735884

Verificamos la integridad de los datos

    colSums(is.na(datos))

    ##            ride_id      rideable_type         started_at           ended_at 
    ##                  0                  0                  0                  0 
    ## start_station_name   start_station_id   end_station_name     end_station_id 
    ##            1087633            1087633            1116035            1116035 
    ##          start_lat          start_lng            end_lat            end_lng 
    ##                  0                  0               6399               6399 
    ##      member_casual 
    ##                  0

Observamos que existen datos que están incompletos, ya que no cuentan
con el nombre de la estación de inicio y su id, la estación final y su
id, así como las posiciónes de longitud y latitud de los puntos finales.
Sin embargo las variables que trabajaremos en este análisis se
encuentran completas, por lo que concluimos que los datos cumplen con la
integridad necesaria para continuar con el análisis.

## Limpieza / Procesamiento de los datos

(Por cuestiones de recursos, el procesamiento de los datos se realizará
por archivo para unirse al final)

Agregamos las columnas ride\_length, day\_of\_week, para el análisis y
eliminamos las columnas que no utilizaremos:
end\_station\_id,start\_station\_id,start\_lat, start\_lng, end\_lat,
end\_lng. Además verificamos que los datos sean relevantes verificando
que ride\_length sea mayor que 0.

    #Función para cargar cada archivo descargado y procesarlo
    #para generar un nuevo archivo de datos ya formateados
    formatear_mes <- function(mes, anio){
      
      nombre_archivo_original <- paste0("Datos/", anio, mes, "-divvy-tripdata.csv")
      
      if(!file.exists(nombre_archivo_original)) {
        message(paste("Archivo no encontrado:", nombre_archivo_original))
        return()
      }
      
      message(paste("Procesando:", anio, "-", mes))
      
      datos_mes <- read_csv(nombre_archivo_original)
      
      nuevos_datos <- datos_mes %>%
        mutate(
          ride_length = hms::as_hms(difftime(ended_at, started_at, units = "secs")),
          date = as.Date(started_at),
          month = format(as.Date(started_at), "%m"),
          day_of_week = wday(started_at)
        )%>%
        select(-c(end_station_id,start_station_id,start_lat, start_lng, end_lat, end_lng))
      
      nuevos_datos_v2 <- nuevos_datos[!(nuevos_datos$ride_length<0),]
      
      str(nuevos_datos_v2)
      
      nombre_archivo_nuevo <- paste0("Datos/DatosFormateados/", anio, mes, ".csv")
      write_csv(nuevos_datos_v2, nombre_archivo_nuevo)
      
    }


    formatea_datos <- function(mes_inicio, anio_inicio, mes_final, anio_final){
      
      # Recibe los datos numéricos enteros del periodo de tiempo que se va a 
      # procesar, identifica si se trata de un periodo durante el mismo año o
      # si se trata de años distintos, y formatea mes por mes.
      
      
      # Verificar si es el mismo año
      if(anio_inicio == anio_final) {
        meses <- mes_inicio:mes_final
        for(mes in meses) {
          mes <- sprintf("%02d", mes)
          formatear_mes(mes, anio_inicio)
        }
        return()
      }
      
      # Procesar múltiples años
      for(anio in anio_inicio:anio_final) {
        # Determinar los meses para cada año
        if(anio == anio_inicio) {
          meses <- mes_inicio:12
        } else if(anio == anio_final) {
          meses <- 1:mes_final
        } else {
          meses <- 1:12
        }
        
        for(mes in meses) {
          mes <- sprintf("%02d", mes)
          formatear_mes(mes, anio)
        }
      }
    }

    formatea_datos(5,2024,4,2025)

    ## Procesando: 2024 - 05

    ## Rows: 609493 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [609,417 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:609417] "7D9F0CE9EC2A1297" "02EC47687411416F" "101370FB2D3402BE" "E97E396331ED6913" ...
    ##  $ rideable_type     : chr [1:609417] "classic_bike" "classic_bike" "classic_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:609417], format: "2024-05-25 15:52:42" "2024-05-14 15:11:51" ...
    ##  $ ended_at          : POSIXct[1:609417], format: "2024-05-25 16:11:50" "2024-05-14 15:22:00" ...
    ##  $ start_station_name: chr [1:609417] "Streeter Dr & Grand Ave" "Sheridan Rd & Greenleaf Ave" "Streeter Dr & Grand Ave" "Streeter Dr & Grand Ave" ...
    ##  $ end_station_name  : chr [1:609417] "Clark St & Elm St" "Sheridan Rd & Loyola Ave" "Wabash Ave & 9th St" "Sheffield Ave & Wellington Ave" ...
    ##  $ member_casual     : chr [1:609417] "casual" "casual" "member" "member" ...
    ##  $ ride_length       : 'hms' num [1:609417] 00:19:08 00:10:09 00:23:12 00:18:38 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:609417], format: "2024-05-25" "2024-05-14" ...
    ##  $ month             : chr [1:609417] "05" "05" "05" "05" ...
    ##  $ day_of_week       : num [1:609417] 7 3 5 6 4 7 6 1 1 2 ...

    ## Procesando: 2024 - 06
    ## Rows: 710721 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [710,721 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:710721] "CDE6023BE6B11D2F" "462B48CD292B6A18" "9CFB6A858D23ABF7" "6365EFEB64231153" ...
    ##  $ rideable_type     : chr [1:710721] "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:710721], format: "2024-06-11 17:20:06" "2024-06-11 17:19:21" ...
    ##  $ ended_at          : POSIXct[1:710721], format: "2024-06-11 17:21:39" "2024-06-11 17:19:36" ...
    ##  $ start_station_name: chr [1:710721] NA NA NA NA ...
    ##  $ end_station_name  : chr [1:710721] NA NA NA NA ...
    ##  $ member_casual     : chr [1:710721] "casual" "casual" "casual" "casual" ...
    ##  $ ride_length       : 'hms' num [1:710721] 00:01:33.175 00:00:14.810 00:04:45.946 00:14:22.613 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:710721], format: "2024-06-11" "2024-06-11" ...
    ##  $ month             : chr [1:710721] "06" "06" "06" "06" ...
    ##  $ day_of_week       : num [1:710721] 3 3 3 3 3 3 3 3 3 3 ...

    ## Procesando: 2024 - 07
    ## Rows: 748962 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [748,962 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:748962] "2658E319B13141F9" "B2176315168A47CE" "C2A9D33DF7EBB422" "8BFEA406DF01D8AD" ...
    ##  $ rideable_type     : chr [1:748962] "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:748962], format: "2024-07-11 08:15:14" "2024-07-11 15:45:07" ...
    ##  $ ended_at          : POSIXct[1:748962], format: "2024-07-11 08:17:56" "2024-07-11 16:06:04" ...
    ##  $ start_station_name: chr [1:748962] NA NA NA NA ...
    ##  $ end_station_name  : chr [1:748962] NA NA NA NA ...
    ##  $ member_casual     : chr [1:748962] "casual" "casual" "casual" "casual" ...
    ##  $ ride_length       : 'hms' num [1:748962] 00:02:41.551 00:20:56.392 00:03:17.045 00:28:04.800 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:748962], format: "2024-07-11" "2024-07-11" ...
    ##  $ month             : chr [1:748962] "07" "07" "07" "07" ...
    ##  $ day_of_week       : num [1:748962] 5 5 5 5 5 5 5 5 5 4 ...

    ## Procesando: 2024 - 08
    ## Rows: 755639 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [755,639 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:755639] "BAA154388A869E64" "8752245932EFF67A" "44DDF9F57A9A161F" "44AAAF069B0C78C3" ...
    ##  $ rideable_type     : chr [1:755639] "classic_bike" "electric_bike" "classic_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:755639], format: "2024-08-02 13:35:14" "2024-08-02 15:33:13" ...
    ##  $ ended_at          : POSIXct[1:755639], format: "2024-08-02 13:48:24" "2024-08-02 15:55:23" ...
    ##  $ start_station_name: chr [1:755639] "State St & Randolph St" "Franklin St & Monroe St" "Franklin St & Monroe St" "Clark St & Elm St" ...
    ##  $ end_station_name  : chr [1:755639] "Wabash Ave & 9th St" "Damen Ave & Cortland St" "Clark St & Elm St" "McClurg Ct & Ohio St" ...
    ##  $ member_casual     : chr [1:755639] "member" "member" "member" "member" ...
    ##  $ ride_length       : 'hms' num [1:755639] 00:13:10.023 00:22:09.900 00:13:45.876 00:09:21.414 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:755639], format: "2024-08-02" "2024-08-02" ...
    ##  $ month             : chr [1:755639] "08" "08" "08" "08" ...
    ##  $ day_of_week       : num [1:755639] 6 6 6 2 7 7 4 3 3 3 ...

    ## Procesando: 2024 - 09
    ## Rows: 821276 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [821,276 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:821276] "31D38723D5A8665A" "67CB39987F4E895B" "DA61204FD26EC681" "06F160D46AF235DD" ...
    ##  $ rideable_type     : chr [1:821276] "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:821276], format: "2024-09-26 15:30:58" "2024-09-26 15:31:32" ...
    ##  $ ended_at          : POSIXct[1:821276], format: "2024-09-26 15:30:59" "2024-09-26 15:53:13" ...
    ##  $ start_station_name: chr [1:821276] NA NA NA NA ...
    ##  $ end_station_name  : chr [1:821276] NA NA NA NA ...
    ##  $ member_casual     : chr [1:821276] "member" "member" "member" "member" ...
    ##  $ ride_length       : 'hms' num [1:821276] 00:00:01.287 00:21:40.972 00:01:52.394 00:19:47.024 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:821276], format: "2024-09-26" "2024-09-26" ...
    ##  $ month             : chr [1:821276] "09" "09" "09" "09" ...
    ##  $ day_of_week       : num [1:821276] 5 5 5 5 3 4 4 4 7 7 ...

    ## Procesando: 2024 - 10
    ## Rows: 616281 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [616,281 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:616281] "4422E707103AA4FF" "19DB722B44CBE82F" "20AE2509FD68C939" "D0F17580AB9515A9" ...
    ##  $ rideable_type     : chr [1:616281] "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:616281], format: "2024-10-14 03:26:04" "2024-10-13 19:33:38" ...
    ##  $ ended_at          : POSIXct[1:616281], format: "2024-10-14 03:32:56" "2024-10-13 19:39:04" ...
    ##  $ start_station_name: chr [1:616281] NA NA NA NA ...
    ##  $ end_station_name  : chr [1:616281] NA NA NA NA ...
    ##  $ member_casual     : chr [1:616281] "member" "member" "member" "member" ...
    ##  $ ride_length       : 'hms' num [1:616281] 00:06:52.452 00:05:25.564 00:07:13.817 00:11:58.455 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:616281], format: "2024-10-14" "2024-10-13" ...
    ##  $ month             : chr [1:616281] "10" "10" "10" "10" ...
    ##  $ day_of_week       : num [1:616281] 2 1 1 2 1 2 2 6 7 7 ...

    ## Procesando: 2024 - 11
    ## Rows: 335075 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [335,032 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:335032] "578DDD7CE1771FFA" "78B141C50102ABA6" "1E794CF36394E2D7" "E5DD2CAB58D73F98" ...
    ##  $ rideable_type     : chr [1:335032] "classic_bike" "classic_bike" "classic_bike" "classic_bike" ...
    ##  $ started_at        : POSIXct[1:335032], format: "2024-11-07 19:21:58" "2024-11-22 14:49:00" ...
    ##  $ ended_at          : POSIXct[1:335032], format: "2024-11-07 19:28:57" "2024-11-22 14:56:15" ...
    ##  $ start_station_name: chr [1:335032] "Walsh Park" "Walsh Park" "Walsh Park" "Clark St & Elm St" ...
    ##  $ end_station_name  : chr [1:335032] "Leavitt St & North Ave" "Leavitt St & Armitage Ave" "Damen Ave & Cortland St" "Clark St & Drummond Pl" ...
    ##  $ member_casual     : chr [1:335032] "member" "member" "member" "member" ...
    ##  $ ride_length       : 'hms' num [1:335032] 00:06:59.095 00:07:15.044 00:04:33.242 00:14:18.430 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:335032], format: "2024-11-07" "2024-11-22" ...
    ##  $ month             : chr [1:335032] "11" "11" "11" "11" ...
    ##  $ day_of_week       : num [1:335032] 5 6 6 1 2 1 4 2 2 6 ...

    ## Procesando: 2024 - 12
    ## Rows: 178372 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [178,372 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:178372] "6C960DEB4F78854E" "C0913EEB2834E7A2" "848A37DD4723078A" "3FA09C762ECB48BD" ...
    ##  $ rideable_type     : chr [1:178372] "electric_bike" "classic_bike" "classic_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:178372], format: "2024-12-31 01:38:35" "2024-12-21 18:41:26" ...
    ##  $ ended_at          : POSIXct[1:178372], format: "2024-12-31 01:48:45" "2024-12-21 18:47:33" ...
    ##  $ start_station_name: chr [1:178372] "Halsted St & Roscoe St" "Clark St & Wellington Ave" "Sheridan Rd & Montrose Ave" "Aberdeen St & Jackson Blvd" ...
    ##  $ end_station_name  : chr [1:178372] "Clark St & Winnemac Ave" "Halsted St & Roscoe St" "Broadway & Barry Ave" "Green St & Randolph St*" ...
    ##  $ member_casual     : chr [1:178372] "member" "member" "member" "member" ...
    ##  $ ride_length       : 'hms' num [1:178372] 00:10:10.757 00:06:07.393 00:11:43.430 00:03:26.604 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:178372], format: "2024-12-31" "2024-12-21" ...
    ##  $ month             : chr [1:178372] "12" "12" "12" "12" ...
    ##  $ day_of_week       : num [1:178372] 3 7 7 5 6 1 1 2 5 7 ...

    ## Procesando: 2025 - 01
    ## Rows: 138689 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [138,689 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:138689] "7569BC890583FCD7" "013609308856B7FC" "EACACD3CE0607C0D" "EAA2485BA64710D3" ...
    ##  $ rideable_type     : chr [1:138689] "classic_bike" "electric_bike" "classic_bike" "classic_bike" ...
    ##  $ started_at        : POSIXct[1:138689], format: "2025-01-21 17:23:54" "2025-01-11 15:44:06" ...
    ##  $ ended_at          : POSIXct[1:138689], format: "2025-01-21 17:37:52" "2025-01-11 15:49:11" ...
    ##  $ start_station_name: chr [1:138689] "Wacker Dr & Washington St" "Halsted St & Wrightwood Ave" "Southport Ave & Waveland Ave" "Southport Ave & Waveland Ave" ...
    ##  $ end_station_name  : chr [1:138689] "McClurg Ct & Ohio St" "Racine Ave & Belmont Ave" "Broadway & Cornelia Ave" "Southport Ave & Roscoe St" ...
    ##  $ member_casual     : chr [1:138689] "member" "member" "member" "member" ...
    ##  $ ride_length       : 'hms' num [1:138689] 00:13:57.477 00:05:04.344 00:11:35.500 00:03:34.233 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:138689], format: "2025-01-21" "2025-01-11" ...
    ##  $ month             : chr [1:138689] "01" "01" "01" "01" ...
    ##  $ day_of_week       : num [1:138689] 3 7 5 5 5 5 1 6 2 4 ...

    ## Procesando: 2025 - 02
    ## Rows: 151880 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [151,880 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:151880] "A246CA24873F7C5C" "303C0906F3F068AE" "A0F65F3531F1FB2B" "CE663C815B6A6D73" ...
    ##  $ rideable_type     : chr [1:151880] "classic_bike" "classic_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:151880], format: "2025-02-25 21:21:21" "2025-02-08 14:55:13" ...
    ##  $ ended_at          : POSIXct[1:151880], format: "2025-02-25 21:30:09" "2025-02-08 15:13:39" ...
    ##  $ start_station_name: chr [1:151880] "Michigan Ave & Lake St" "Ogden Ave & Race Ave" "Michigan Ave & Lake St" "Ogden Ave & Race Ave" ...
    ##  $ end_station_name  : chr [1:151880] "Clark St & Elm St" "Clark St & Elm St" "Wabash Ave & 9th St" "Clark St & Elm St" ...
    ##  $ member_casual     : chr [1:151880] "member" "member" "casual" "casual" ...
    ##  $ ride_length       : 'hms' num [1:151880] 00:08:48.770 00:18:26.397 00:05:25.158 00:33:50.366 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:151880], format: "2025-02-25" "2025-02-08" ...
    ##  $ month             : chr [1:151880] "02" "02" "02" "02" ...
    ##  $ day_of_week       : num [1:151880] 3 7 2 6 2 5 4 2 3 6 ...

    ## Procesando: 2025 - 03
    ## Rows: 298155 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [298,155 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:298155] "16CBE9844D401954" "1CB408029E2B5F74" "7B6A76CD0F204D08" "4F7084E3D75CDE31" ...
    ##  $ rideable_type     : chr [1:298155] "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : POSIXct[1:298155], format: "2025-03-18 08:39:20" "2025-03-24 16:04:22" ...
    ##  $ ended_at          : POSIXct[1:298155], format: "2025-03-18 08:51:37" "2025-03-24 16:27:41" ...
    ##  $ start_station_name: chr [1:298155] NA NA NA NA ...
    ##  $ end_station_name  : chr [1:298155] "Canal St & Jackson Blvd" "Albany Ave & Bloomingdale Ave" "Albany Ave & Bloomingdale Ave" "Canal St & Jackson Blvd" ...
    ##  $ member_casual     : chr [1:298155] "member" "member" "member" "member" ...
    ##  $ ride_length       : 'hms' num [1:298155] 00:12:17.568 00:23:19.108 00:22:57.749 00:06:51.581 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:298155], format: "2025-03-18" "2025-03-24" ...
    ##  $ month             : chr [1:298155] "03" "03" "03" "03" ...
    ##  $ day_of_week       : num [1:298155] 3 2 2 6 6 2 3 7 5 6 ...

    ## Procesando: 2025 - 04
    ## Rows: 371341 Columns: 13── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (7): ride_id, rideable_type, start_station_name, start_station_id, end_...
    ## dbl  (4): start_lat, start_lng, end_lat, end_lng
    ## dttm (2): started_at, ended_at
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## tibble [371,341 × 11] (S3: tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:371341] "AF3863596DF9D94B" "8B38081EBE918800" "1C7F1DE826BBBC8D" "CAD23D69A79A6C3B" ...
    ##  $ rideable_type     : chr [1:371341] "classic_bike" "electric_bike" "electric_bike" "classic_bike" ...
    ##  $ started_at        : POSIXct[1:371341], format: "2025-04-27 14:29:34" "2025-04-23 17:48:51" ...
    ##  $ ended_at          : POSIXct[1:371341], format: "2025-04-27 14:36:23" "2025-04-23 17:59:06" ...
    ##  $ start_station_name: chr [1:371341] "Troy St & Elston Ave" "Wabash Ave & Adams St" "Damen Ave & Cortland St" "Clark St & Elm St" ...
    ##  $ end_station_name  : chr [1:371341] "Richmond St & Diversey Ave" "Green St & Madison St" "California Ave & Fletcher St" "Orleans St & Merchandise Mart Plaza" ...
    ##  $ member_casual     : chr [1:371341] "member" "member" "member" "member" ...
    ##  $ ride_length       : 'hms' num [1:371341] 00:06:48.965 00:10:14.152 00:10:09.187 00:10:01.606 ...
    ##   ..- attr(*, "units")= chr "secs"
    ##  $ date              : Date[1:371341], format: "2025-04-27" "2025-04-23" ...
    ##  $ month             : chr [1:371341] "04" "04" "04" "04" ...
    ##  $ day_of_week       : num [1:371341] 1 4 7 5 3 4 7 7 3 4 ...

Unimos los datos ya formateados.

    # Crear una lista para almacenar los datos temporales
    datos_combinados <- list()

    # Vector con los meses (asumiendo que son consecutivos)
    meses24 <- sprintf("%02d", 5:12) # Formato 01, 02, ..., 12

    # Año de los datos 2024
    anio <- "2024"

    for (mes in meses24) {
      # Construir el nombre del archivo
      nombre_archivo <- paste0("Datos/DatosFormateados/", anio, mes, ".csv")
      
      # Cargar el archivo y asegurar que month sea character
      datos_mes <- read_csv(nombre_archivo) %>%
        mutate(month = as.character(month))
      
      # Almacenar en la lista
      datos_combinados[[mes]] <- datos_mes
    }

    ## Rows: 609417 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (6): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (1): day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 710721 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (6): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (1): day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 748962 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (6): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (1): day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 755639 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (6): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (1): day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 821276 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (6): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (1): day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 616281 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (2): month, day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 335032 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (2): month, day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 178372 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (2): month, day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    # Vector con los meses (asumiendo que son consecutivos)
    meses25 <- sprintf("%02d", 1:4) # Formato 01, 02, ..., 12

    # Año de los datos 2025
    anio <- "2025"

    for (mes in meses25) {
      # Construir el nombre del archivo
      nombre_archivo <- paste0("Datos/DatosFormateados/", anio, mes, ".csv")
      
      # Cargar el archivo y asegurar que month sea character
      datos_mes <- read_csv(nombre_archivo) %>%
        mutate(month = as.character(month))
      
      # Almacenar en la lista
      datos_combinados[[mes]] <- datos_mes
    }

    ## Rows: 138689 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (6): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (1): day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 151880 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (6): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (1): day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 298155 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (6): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (1): day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## Rows: 371341 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (6): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (1): day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    # Combinar todos los data frames en uno solo
    datos <- bind_rows(datos_combinados)

    # Opcional: guardar el archivo combinado
    write_csv(datos, "Datos/DatosResumen/052024_042025.csv")

## Análisis

    datos_anio <- read_csv("Datos/DatosResumen/052024_042025.csv")

    ## Rows: 5735765 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (6): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (1): day_of_week
    ## dttm (2): started_at, ended_at
    ## date (1): date
    ## time (1): ride_length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    head(datos_anio)

    ## # A tibble: 6 × 11
    ##   ride_id          rideable_type started_at          ended_at           
    ##   <chr>            <chr>         <dttm>              <dttm>             
    ## 1 7D9F0CE9EC2A1297 classic_bike  2024-05-25 15:52:42 2024-05-25 16:11:50
    ## 2 02EC47687411416F classic_bike  2024-05-14 15:11:51 2024-05-14 15:22:00
    ## 3 101370FB2D3402BE classic_bike  2024-05-30 17:46:04 2024-05-30 18:09:16
    ## 4 E97E396331ED6913 electric_bike 2024-05-17 20:21:54 2024-05-17 20:40:32
    ## 5 674EDE311C543165 classic_bike  2024-05-22 18:52:20 2024-05-22 18:59:04
    ## 6 2E3EA4C19F0341A6 electric_bike 2024-05-25 19:32:12 2024-05-25 19:36:17
    ## # ℹ 7 more variables: start_station_name <chr>, end_station_name <chr>,
    ## #   member_casual <chr>, ride_length <time>, date <date>, month <chr>,
    ## #   day_of_week <dbl>

    datos_analizados <- nrow(datos_anio)
    inicio_periodo <- as.Date(min(datos_anio$started_at))
    termino_periodo <- as.Date(max(datos_anio$started_at))

    print("Cantidad de datos analizados:")

    ## [1] "Cantidad de datos analizados:"

    print(datos_analizados)

    ## [1] 5735765

    print(paste0("Del periodo de ", inicio_periodo, " a ", termino_periodo))

    ## [1] "Del periodo de 2024-05-01 a 2025-04-30"

    num_viajes_vs <- datos_anio %>%
      group_by(member_casual) %>%
      summarise(
        total_rides = n(),
      ) 
    #View(num_viajes_vs)
    pct <- round(num_viajes_vs$total_rides/sum(num_viajes_vs$total_rides)*100)
    etiquetas <- paste(num_viajes_vs$member_casual, pct) 
    etiquetas <- paste(etiquetas,"%",sep="")

    ggplot(num_viajes_vs, aes(x="", y = total_rides, fill = member_casual )) +
      geom_bar(stat = "identity", width = 1) +
      coord_polar("y", start = 0) +
      geom_label_repel(
        aes(label = etiquetas, y = total_rides),
        position = position_stack(vjust = 0.5),
        size = 3,
        color = "white",
        fill = scales::alpha("gray20", 0.7),
        box.padding = 0.3, 
        point.padding = 0.5,
        min.segment.length = 0.1,  
        force = 0.5, 
        direction = "both"  
      )  +
      theme_void() +
      guides(fill = guide_legend(title = "Tipo cliente")) +
      labs(title = "Porcentaje de clientes casuales vs miembros")+ 
      scale_fill_viridis_d() 

![](analisisCyclistic_files/figure-markdown_strict/Viajes%20hechos%20por%20miembros%20vs%20casual-1.png)

    ggplot(datos_anio, 
           aes(x = rideable_type,
               fill = member_casual)) + 
      geom_bar(position = position_dodge(width = 0.9)) +
      geom_label(
        stat = "count",
        aes(label = after_stat(count), group = member_casual),  # Crucial: agregar group
        position = position_dodge(width = 0.9),
        vjust = -0.1,
        color = "white",
        fill = scales::alpha("gray20", 0.7),
        size = 3,
        show.legend = FALSE
      ) +
      labs(x = "Tipo de transporte", y = "Número de viajes") +
      guides(fill = guide_legend(title = "Tipo de cliente")) + 
      ggtitle("Tipos de transporte usados por miembros vs clientes casuales") +
      scale_fill_viridis_d() +
      ylim(0, NA)  # Ajusta automáticamente el límite superior

![](analisisCyclistic_files/figure-markdown_strict/Tipos%20de%20transporte%20usados%20por%20miembros%20vs%20casual-1.png)

    rideable_type_vs <- datos_anio %>%
      group_by(member_casual, rideable_type) %>%
      summarise(
        total_rides = n(),
      ) 

    ## `summarise()` has grouped output by 'member_casual'. You can override using the
    ## `.groups` argument.

    miembros_type <- rideable_type_vs %>%
      filter(member_casual == 'member')

    miembros_type$pct <- miembros_type$total_rides/sum(miembros_type$total_rides)*100
    miembros_type$etiqueta <- ifelse(miembros_type$pct < 1, 
                                     paste0(miembros_type$rideable_type, "\n", round(miembros_type$pct, 1), "%"),
                                     paste0(miembros_type$rideable_type, "\n", round(miembros_type$pct), "%"))

    ggplot(miembros_type, aes(x = "", y = total_rides, fill = rideable_type)) +
      geom_bar(stat = "identity", width = 1) +
      coord_polar("y", start = 0) +
      geom_text(aes(label = etiqueta), 
                position = position_stack(vjust = 0.5),
                size = 3,
                color = 'white') +
      theme_void() +
      guides(fill = guide_legend(title = "Tipo de transporte")) + 
      labs(title = "Tipo de transporte usado por miembros")+ 
      scale_fill_viridis_d() 

![](analisisCyclistic_files/figure-markdown_strict/Porcentajes%20de%20transporte%20usados%20por%20miembros-1.png)

    casual_type <- rideable_type_vs %>%
      filter(member_casual == 'casual')

    casual_type$pct <- casual_type$total_rides/sum(casual_type$total_rides)*100
    casual_type$etiqueta <- ifelse(casual_type$pct < 1, 
                                     paste0(casual_type$rideable_type, "\n", round(casual_type$pct, 1), "%"),
                                     paste0(casual_type$rideable_type, "\n", round(casual_type$pct), "%"))

    ggplot(casual_type, aes(x = "", y = total_rides, fill = rideable_type)) +
      geom_bar(stat = "identity", width = 1) +
      coord_polar("y", start = 0) +
      geom_text(aes(label = etiqueta), 
                position = position_stack(vjust = 0.5),
                size = 3,
                color = 'white')  +
      theme_void() +
      guides(fill = guide_legend(title = "Tipo de transporte")) + 
      labs(title = "Tipo de transporte usado por clientes casuales")+ 
      scale_fill_viridis_d() 

![](analisisCyclistic_files/figure-markdown_strict/Porcentajes%20de%20transporte%20usados%20por%20clientes%20casuales-1.png)

    mean_ride_length <- hms::as_hms(mean(datos_anio$ride_length))
    print("El tiempo promedio de viaje es de: ")

    ## [1] "El tiempo promedio de viaje es de: "

    print(mean_ride_length)

    ## 00:16:53.006419

    print('El viaje más largo ha sido de: ')

    ## [1] "El viaje más largo ha sido de: "

    print(hms::as_hms(max(datos_anio$ride_length)))

    ## 25:59:55

    print('El viaje más corto ha sido de: ')

    ## [1] "El viaje más corto ha sido de: "

    print(hms::as_hms(min(datos_anio$ride_length)))

    ## 00:00:00

    ride_length_gral <- datos_anio %>%
      group_by(rideable_type) %>%
      summarise(
        total_rides = n(),
        mean_length = mean(ride_length),
        max_length = max(ride_length),
        min_length = min(ride_length)
      )

    ggplot(ride_length_gral, 
      aes(x = rideable_type, y = hms::as_hms(mean_length), fill = rideable_type)) + 
      geom_bar(stat = "identity",position="dodge", show.legend = FALSE) + 
      geom_hline(yintercept = hms::as_hms(mean_ride_length),  
                 color = "black", 
                 linetype = "dashed", 
                 linewidth = 1) +
      labs(x = "Tipo de transporte", y = "Tiempo promedio (hms)") +
      ggtitle("Tiempos promedio por transporte usado")+
      geom_label(stat = 'identity', 
                 aes(label = hms::as_hms(mean_length)), 
                 position = position_dodge(width = 1),
                 vjust = 0,
                 color = "white",
                 fill = scales::alpha("gray20", 0.7),
                 size = 4,
                 label.padding = unit(0.15, "lines")) +
      annotate("text", 
               x = Inf, y = hms::as_hms(mean_ride_length), 
               label = paste("Promedio:", hms::as_hms(mean_ride_length)), 
               hjust = 1.1, vjust = -0.5, 
               color = "black")+ 
      scale_fill_viridis_d() 

![](analisisCyclistic_files/figure-markdown_strict/Tiempo%20promedio%20de%20uso%20en%20general%20y%20por%20tipo%20de%20transporte-1.png)

    ride_length_vs <- datos_anio %>%
      group_by(member_casual,rideable_type) %>%
      summarise(
        total_rides = n(),
        mean_length = hms::round_hms(hms::as_hms(mean(ride_length)), digits = 0),    
        max_length = hms::as_hms(max(ride_length)),
        min_length = hms::as_hms(min(ride_length))
      ) 

    ## `summarise()` has grouped output by 'member_casual'. You can override using the
    ## `.groups` argument.

    #View(ride_length_vs)

    ggplot(ride_length_vs, 
           aes(x = rideable_type, y = mean_length,
               fill=member_casual)) + 
      geom_bar(stat = "identity", position="dodge") + 
      labs(x = "Tipo de transporte", y = "Tiempo promedio de viaje") +
      guides(fill = guide_legend(title = "Tipo de cliente")) + 
      ggtitle("Tiempos de viaje según el tipo de transporte miembros vs clientes casuales")+
      geom_label(
        stat = "identity",
        aes(label = mean_length),
        position = position_dodge(width = 0.9),  
        vjust = -0.3,  
        color = "white",
        fill = scales::alpha("gray20", 0.7),
        size = 3,
        label.padding = unit(0.15, "lines"),
        show.legend = FALSE
      )  + 
      scale_fill_viridis_d() 

![](analisisCyclistic_files/figure-markdown_strict/Tiempo%20promedio%20de%20uso%20por%20tipo%20de%20transporte%20member%20vs%20casual-1.png)

    concurrencia_dia_gral <- datos_anio %>%
      group_by(day_of_week) %>%
      summarise(
        total_rides = n(),
      ) %>%
      mutate(
        day_name = factor(
          day_of_week,
          levels = 1:7,
          labels = c("Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"),
          ordered = TRUE
        )
      )
    concurrencia_dia_gral$pct <- concurrencia_dia_gral$total_rides/sum(concurrencia_dia_gral$total_rides)*100
    concurrencia_dia_gral$etiqueta <- ifelse(concurrencia_dia_gral$pct < 1, 
                                     paste0(concurrencia_dia_gral$day_name, "\n", round(concurrencia_dia_gral$pct, 1), "%"),
                                     paste0(concurrencia_dia_gral$day_name, "\n", round(concurrencia_dia_gral$pct), "%"))

    ggplot(concurrencia_dia_gral, aes(x = "", y = total_rides, fill = day_name)) +
      geom_bar(stat = "identity", width = 1) +
      coord_polar("y", start = 0) +
      geom_text(aes(label = etiqueta), 
                position = position_stack(vjust = 0.5),
                size = 3,
                color = 'white') +
      theme_void() +
      guides(fill = guide_legend(title = "Día de la semana")) + 
      labs(title = "Porcentaje de viajes realizados por día de la semana")

![](analisisCyclistic_files/figure-markdown_strict/Concurrencia%20por%20días%20general-1.png)

    concurrencia_dia_vs <- datos_anio %>%
      group_by(day_of_week, member_casual) %>%
      summarise(
        total_rides = n(),
      ) %>%
      mutate(
        day_name = factor(
          day_of_week,
          levels = 1:7,
          labels = c("Lunes", "Martes", "Miércoles", "Jueves", "Viernes", "Sábado", "Domingo"),
          ordered = TRUE
        )
      )

    ## `summarise()` has grouped output by 'day_of_week'. You can override using the
    ## `.groups` argument.

    head(concurrencia_dia_vs)

    ## # A tibble: 6 × 4
    ## # Groups:   day_of_week [3]
    ##   day_of_week member_casual total_rides day_name 
    ##         <dbl> <chr>               <int> <ord>    
    ## 1           1 casual             354296 Lunes    
    ## 2           1 member             402920 Lunes    
    ## 3           2 casual             244523 Martes   
    ## 4           2 member             515691 Martes   
    ## 5           3 casual             224093 Miércoles
    ## 6           3 member             550242 Miércoles

    rides_day_member <- concurrencia_dia_vs %>%
      filter(member_casual == 'member')

    rides_day_member$pct <- rides_day_member$total_rides/sum(rides_day_member$total_rides)*100
    rides_day_member$etiqueta <- ifelse(
      rides_day_member$pct < 1,
      paste0(rides_day_member$day_name, "\n", round(rides_day_member$pct, 1), "%"),
      paste0(rides_day_member$day_name, "\n", round(rides_day_member$pct), "%")
    )

    head(rides_day_member)

    ## # A tibble: 6 × 6
    ## # Groups:   day_of_week [6]
    ##   day_of_week member_casual total_rides day_name    pct etiqueta        
    ##         <dbl> <chr>               <int> <ord>     <dbl> <chr>           
    ## 1           1 member             402920 Lunes      11.1 "Lunes\n11%"    
    ## 2           2 member             515691 Martes     14.2 "Martes\n14%"   
    ## 3           3 member             550242 Miércoles  15.2 "Miércoles\n15%"
    ## 4           4 member             594390 Jueves     16.4 "Jueves\n16%"   
    ## 5           5 member             558166 Viernes    15.4 "Viernes\n15%"  
    ## 6           6 member             531302 Sábado     14.7 "Sábado\n15%"

    ggplot(rides_day_member, aes(x = "", y = total_rides, fill = day_name)) +
      geom_bar(stat = "identity", width = 1) +
      coord_polar("y", start = 0) +
      geom_text(aes(label = etiqueta), 
                position = position_stack(vjust = 0.5),
                size = 3,
                color = 'white') +
      theme_void() +
      guides(fill = guide_legend(title = "Día de la semana")) + 
      labs(title = "Porcentaje de viajes realizados por día de la semana \n por miembros")

![](analisisCyclistic_files/figure-markdown_strict/Concurrencia%20por%20días%20miembros%20grafica%20de%20pastel-1.png)

    ggplot(rides_day_member, 
           aes(x = day_name, y = total_rides,
               fill=day_name)) + 
      geom_bar(stat = "identity",  position = position_dodge(width = 0.9)) + 
      labs(x = "Día de la semana", y = "Número de viajes") +
      guides(fill = guide_legend(title = "Tipo de cliente")) + 
      ggtitle("Viajes por día miembros")+
      geom_label(
        stat = "identity",
        aes(label = total_rides),
        position = position_dodge(width = 0.9),  
        vjust = -0.3,  
        color = "white",
        fill = scales::alpha("gray20", 0.7),
        size = 3,
        label.padding = unit(0.15, "lines"),
        show.legend = FALSE
      )

![](analisisCyclistic_files/figure-markdown_strict/Concurrencia%20por%20días%20miembros%20grafica%20de%20barras-1.png)

    rides_day_casual <- concurrencia_dia_vs %>%
      filter(member_casual == 'casual')

    rides_day_casual$pct <- rides_day_casual$total_rides/sum(rides_day_casual$total_rides)*100
    rides_day_casual$etiqueta <- ifelse(rides_day_casual$pct < 1, 
                                        paste0(rides_day_casual$day_name, "\n", round(rides_day_casual$pct, 1), "%"),
                                        paste0(rides_day_casual$day_name, "\n", round(rides_day_casual$pct), "%"))

    head(rides_day_casual)

    ## # A tibble: 6 × 6
    ## # Groups:   day_of_week [6]
    ##   day_of_week member_casual total_rides day_name    pct etiqueta        
    ##         <dbl> <chr>               <int> <ord>     <dbl> <chr>           
    ## 1           1 casual             354296 Lunes      16.8 "Lunes\n17%"    
    ## 2           2 casual             244523 Martes     11.6 "Martes\n12%"   
    ## 3           3 casual             224093 Miércoles  10.6 "Miércoles\n11%"
    ## 4           4 casual             266645 Jueves     12.6 "Jueves\n13%"   
    ## 5           5 casual             263459 Viernes    12.5 "Viernes\n12%"  
    ## 6           6 casual             325427 Sábado     15.4 "Sábado\n15%"

    ggplot(rides_day_casual, aes(x = "", y = total_rides, fill = day_name)) +
      geom_bar(stat = "identity", width = 1) +
      coord_polar("y", start = 0) +
      geom_text(aes(label = etiqueta), 
                position = position_stack(vjust = 0.5),
                size = 3,
                color = 'white') +
      theme_void() +
      guides(fill = guide_legend(title = "Día de la semana")) + 
      labs(title = "Porcentaje de viajes realizados por día de la semana \n por clientes casuales")

![](analisisCyclistic_files/figure-markdown_strict/Concurrencia%20por%20días%20casual%20grafica%20de%20pastel-1.png)

    ggplot(rides_day_casual, 
           aes(x = day_name, y = total_rides,
               fill=day_name)) + 
      geom_bar(stat = "identity", position="dodge") + 
      labs(x = "Día de la semana", y = "Número de viajes") +
      guides(fill = guide_legend(title = "Tipo de cliente")) + 
      ggtitle("Viajes por día clientes casuales")+
      geom_label(
        stat = "identity",
        aes(label = total_rides),
        position = position_dodge(width = 0.9),  
        vjust = -0.3,  
        color = "white",
        fill = scales::alpha("gray20", 0.7),
        size = 3,
        label.padding = unit(0.15, "lines"),
        show.legend = FALSE
      )

![](analisisCyclistic_files/figure-markdown_strict/Concurrencia%20por%20días%20casual%20grafica%20de%20barras-1.png)

    concurrencia_mes_gral <- datos_anio %>%
      group_by(month) %>%
      summarise(
        total_rides = n()
      ) %>%
      mutate(month = sprintf("%02d", as.numeric(month))) 

    ggplot(concurrencia_mes_gral, 
           aes(x = month, y = total_rides,
               fill=month)) + 
      geom_bar(stat = "identity",  position = position_dodge(width = 0.9)) + 
      labs(x = "Mes", y = "Número de viajes") + 
      ggtitle("Viajes por mes clientes en general")+
      geom_label(
        stat = "identity",
        aes(label = total_rides),
        position = position_dodge(width = 0.9),  
        vjust = -0.3,  
        color = "white",
        fill = scales::alpha("gray20", 0.7),
        size = 3,
        label.padding = unit(0.15, "lines"),
        show.legend = FALSE
      )+ 
      scale_fill_viridis_d() 

![](analisisCyclistic_files/figure-markdown_strict/Concurrencia%20de%20clientes%20en%20general%20según%20el%20mes-1.png)

    concurrencia_mes_vs <- datos_anio %>%
      group_by(month, member_casual) %>%
      summarise(
        total_rides = n(),
      ) %>%
      mutate(month = sprintf("%02d", as.numeric(month))) 

    ## `summarise()` has grouped output by 'month'. You can override using the
    ## `.groups` argument.

    rides_month_casual <- concurrencia_mes_vs %>%
      filter(member_casual == 'casual')

    ggplot(rides_month_casual, 
           aes(x = month, y = total_rides,
               fill=month)) + 
      geom_bar(stat = "identity",  position = position_dodge(width = 0.9)) + 
      labs(x = "Mes", y = "Número de viajes") +
      guides(fill = guide_legend(title = "Mes")) + 
      ggtitle("Viajes por mes clientes casuales")+
      geom_label(
        stat = "identity",
        aes(label = total_rides),
        position = position_dodge(width = 0.9),  
        vjust = -0.3,  
        color = "white",
        fill = scales::alpha("gray20", 0.7),
        size = 3,
        label.padding = unit(0.15, "lines"),
        show.legend = FALSE
      )+ 
      scale_fill_viridis_d()

![](analisisCyclistic_files/figure-markdown_strict/Concurrencia%20por%20mes%20casual%20grafica%20de%20barras-1.png)

    rides_month_member <- concurrencia_mes_vs %>%
      filter(member_casual == 'member')

    ggplot(rides_month_member, 
           aes(x = month, y = total_rides,
               fill=month)) + 
      geom_bar(stat = "identity",  position = position_dodge(width = 0.9)) + 
      labs(x = "mes", y = "Número de viajes") +
      guides(fill = guide_legend(title = "Mes")) + 
      ggtitle("Viajes por día miembros")+
      geom_label(
        stat = "identity",
        aes(label = total_rides),
        position = position_dodge(width = 0.9),  
        vjust = -0.3,  
        color = "white",
        fill = scales::alpha("gray20", 0.7),
        size = 3,
        label.padding = unit(0.15, "lines"),
        show.legend = FALSE
      )+ 
      scale_fill_viridis_d()

![](analisisCyclistic_files/figure-markdown_strict/Concurrencia%20por%20mes%20miembro%20grafica%20de%20barras-1.png)

## Conclusión del Análisis

A partir de los datos analizados, identificamos diferencias clave en el
comportamiento entre los usuarios **miembros anuales** y los **ciclistas
ocasionales (casuales)**, lo que permite orientar estrategias de
marketing efectivas para convertir a los casuales en miembros:

1.  **Preferencia por bicicletas eléctricas**: Ambos grupos muestran una
    clara preferencia por las bicicletas eléctricas (54% miembros, 52%
    casuales), seguidas de las clásicas. Esto sugiere que destacar la
    disponibilidad y beneficios de las bicicletas eléctricas en campañas
    de marketing podría ser un factor atractivo para impulsar
    conversiones.

2.  **Viajes más largos en usuarios casuales**: Los ciclistas
    ocasionales realizan viajes más largos, especialmente en bicicletas
    clásicas. Esto podría indicar que los casuales usan el servicio más
    para paseos recreativos o turísticos, mientras que los miembros lo
    hacen para desplazamientos rutinarios (como trabajo o estudios). Una
    estrategia podría ser resaltar el **ahorro económico** que
    obtendrían al convertirse en miembros, dado su alto uso.

3.  **Patrón de uso semanal**:

    -   **Miembros**: Uso constante durante la semana (posiblemente por
        movilidad laboral/estudios).  
    -   **Casuales**: Mayor uso los fines de semana (domingos y lunes),
        lo que refuerza la idea de un uso recreativo.

    **Estrategia recomendada**: Dirigir campañas promocionales los
    **viernes y sábados**, incentivando la membresía anual con
    beneficios como descuentos en viajes los fines de semana o acceso
    prioritario a bicicletas eléctricas.

### **Recomendaciones Finales**

-   **Enfoque en ahorro y conveniencia**: Destacar cómo la membresía
    anual reduce costos para usuarios frecuentes (como los casuales de
    fines de semana).  
-   **Promociones temporales**: Ofrecer descuentos o períodos de prueba
    gratuitos de la membresía durante temporadas de alta demanda
    recreativa (verano, festivos).  
-   **Personalización**: Segmentar campañas según el tipo de bicicleta
    preferida (ej: “¿Te gustan las bicicletas eléctricas? Con la
    membresía anual, tienes acceso ilimitado”).

Estas acciones, basadas en datos, podrían incrementar la conversión de
ciclistas ocasionales en miembros anuales, asegurando un crecimiento
sostenible para Cyclistic.
