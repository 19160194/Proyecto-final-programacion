# MAPA DE PRECIPITACIÓN CON DATOS PISCO
En este trabajo nos centraremos en la comparación de datos de Senamhi  de  5 estaciones del departamento de Cusco. 
Este trabajo fue realizado por los siguientes estudiantes

Camacho Vega  Mercedes 

Huayllani  Panuera Dialdina

León Chaiña  Lisbeth Karola

Ramos Cerna Alejandra Gianella




Primero instalamos las librerias

    install.packages(c("raster", "ncdf4"))
Leer el archivo long_lat.csv poniendo el directorio de la carpeta final con coordenadas XX Longitud e YY Latitud

    long_lat <- read.csv("estaciones_c.csv", header = T)
    View(long_lat)
    
Ensamblamos los datos .nc de nuestra data grillada

    raster_pp   <- raster::brick("FINAL/DATA GRILLADA/Precipitacion/Mensual/Prec.nc")
    raster_tmax <- raster::brick("FINAL/DATA GRILLADA/Temperatura/Mensual/Tmax.nc")
    raster_tmin <- raster::brick("FINAL/DATA GRILLADA/Temperatura/Mensual/Tmin.nc")
Asignamos las coordenadas 

    sp::coordinates(long_lat) <- ~XX+YY
Igualamos las proyecciones del raster y de los puntos a extraer

    raster::projection(long_lat) <- raster::projection(raster_pp)
    raster::projection(long_lat) <- raster::projection(raster_tmax)
    raster::projection(long_lat) <- raster::projection(raster_tmin)
Leemos la Precipitation

        # PRECIPITACION
        points_long_lat_pp <- raster::extract(raster_pp[[1]], long_lat, cellnumbers = T)[,1]
        data_long_lat_pp <- t(raster_pp[points_long_lat_pp])
        colnames(data_long_lat_pp) <- as.character(long_lat$NN)
        write.csv(data_long_lat_pp, "FINAL/DATA PISCO/PP MENSUAL/prep.csv", quote = F)
Leemos las temperaturas

        #TEMPERATURA MAXIMA
        points_long_lat_tmax <- raster::extract(raster_tmax[[1]], long_lat, cellnumbers = T)[,1]
        data_long_lat_tmax <- t(raster_tmax[points_long_lat_tmax])
        colnames(data_long_lat_tmax) <- as.character(long_lat$NN)
        write.csv(data_long_lat_tmax, "FINAL/DATA PISCO/TMAX/tmax.csv", quote = F)

        #TEMPERATURA MINIMA
        points_long_lat_tmin <- raster::extract(raster_tmin[[1]], long_lat, cellnumbers = T)[,1]
        data_long_lat_tmin <- t(raster_tmin[points_long_lat_tmin])
        colnames(data_long_lat_tmin) <- as.character(long_lat$NN)
        write.csv(data_long_lat_tmin, "FINAL/DATA PISCO/TMIN/tmin.csv", quote = F)
 
 
        #TEMPERATURA MEDIA
        tmedia <- (data_long_lat_tmax+data_long_lat_tmin)/2
        write.csv(tmedia, "FINAL/DATA PISCO/tmedia.csv", quote = F)
        
 Instalamos las librerías
 
        install.packages(c("tidyverse", "ggplot2", "dplyr"))
        
 Cargamos las librerías  
 
        library(tidyverse)
        library(ggplot2)
        library(dplyr)
        
Asignación de series temporales para las precipitaciones y temperaturas, se asigna desde 1981 hasta el 2016 debido a la distribución temporal de los datos 

        Pp_pisco <- read.csv("FINAL/DATA PISCO/PP MENSUAL/prep.csv", header = T, sep = ",") %>%
          tibble() %>%
          dplyr::select(-X) %>%
          mutate(fecha = seq(as.Date("1981-01-01"), as.Date("2016-12-01"), by = "month"))
        View(Pp_pisco)
        write.csv(Pp_pisco, "FINAL/DATA EN CSV/PP ESTACIONES/pp.csv")
        colnames(Pp_pisco)

        tmin_pisco <- read.csv("FINAL/DATA PISCO/TMIN/tmin.csv", header = T, sep = ",") %>%
          tibble() %>%
          dplyr::select(-X) %>%
          mutate(fecha = seq(as.Date("1981-01-01"), as.Date("2016-12-01"), by = "month"))
        View(tmin_pisco)
        write.csv(tmin_pisco, "FINAL/DATA EN CSV/PP ESTACIONES/tmin.csv")
        colnames(tmin_pisco)
        
      tmax_pisco <- read.csv("FINAL/DATA PISCO/TMAX/tmax.csv", header = T, sep = ",") %>%
          tibble() %>%
          dplyr::select(-X) %>%
          mutate(fecha = seq(as.Date("1981-01-01"), as.Date("2016-12-01"), by = "month"))
        View(tmax_pisco)
        write.csv(tmax_pisco, "FINAL/DATA EN CSV/PP ESTACIONES/tmax.csv")
        colnames(tmax_pisco)
        
        tmedia_pisco <- read.csv("FINAL/DATA PISCO/tmedia.csv", header = T, sep = ",") %>%
          tibble() %>%
          dplyr::select(-X) %>%
          mutate(fecha = seq(as.Date("1981-01-01"), as.Date("2016-12-01"), by = "month"))
        View(tmedia_pisco)
        write.csv(tmedia_pisco, "FINAL/DATA EN CSV/PP ESTACIONES/tmedia.csv")
        colnames(tmedia_pisco)
        
 Añadimos los gráficos de serie de tiempo de cada estación meteorológica, en la que se presentan la precipitación por un lapso de tiempo
 
        pisac <- plot(Pp_pisco$fecha,Pp_pisco$PISAC, type = "l", col= 'blue',
              main= 'Serie de Tiempo de la estación Pisac', xlab= 'Año',
              ylab= 'Precipitación')
![Rplot](https://user-images.githubusercontent.com/78572913/107041783-e7fbfc00-678e-11eb-8c91-02ac8af39e89.png)

        Paruro <- plot(Pp_pisco$fecha, Pp_pisco$PARURO, type = "l", col= 'blue',
               main= 'Serie de Tiempo de la estación Paruro', xlab= 'Año',
               ylab= 'Precipitación')
![image](https://user-images.githubusercontent.com/78572913/107049584-49749880-6798-11eb-8bbc-6291cb2e85ec.png)

        colquepata <- plot(Pp_pisco$fecha,Pp_pisco$COLQUEPATA, type = "l", col= 'blue',
                   main= 'Serie de Tiempo de la estación Colquepata', xlab= 'Año',
                   ylab= 'Precipitación')
![Rplot02](https://user-images.githubusercontent.com/77855207/107051577-9e191300-679a-11eb-8293-d13f09cf7413.png)

        catca <-  plot(Pp_pisco$fecha,Pp_pisco$CCATCCA, type = "l", col= 'blue',
               main= 'Serie de Tiempo de la estación Ccatcca', xlab= 'Año',
               ylab= 'Precipitación')
![CCATCCA (1)](https://user-images.githubusercontent.com/77855207/107053516-e6d1cb80-679c-11eb-877b-d0c3c5bb3e4e.png)

        caycay <- plot(Pp_pisco$fecha,Pp_pisco$CAICAY, type = "l", col= 'blue',
               main= 'Serie de Tiempo de la estación Cay cay', xlab= 'Año',
               ylab= 'Precipitación')
![Rplot04](https://user-images.githubusercontent.com/77855207/107051957-17186a80-679b-11eb-9fb1-e3eef68e81c4.png)

 
 Llamamos  a la librería GGPLOT,que nos ayuda a gráficar. 
 
            library(ggplot2)  
            
Ahora  ejecutaremos el comando ggplot para graficar la  precipitación-temperatura en Pisac

            ggplot(Pp_pisco, aes(fecha, PISAC)) +
              geom_line() +
          geom_smooth(span = 0.4)
        ggplot(Pp_pisco, aes(fecha, precipitacion)) +
          geom_line() +
          geom_smooth(span = 0.)
          
![Rplot05](https://user-images.githubusercontent.com/78572913/107045499-90ac5a80-6793-11eb-99b6-0c940a4e3cdf.png)

Asimismo, también ejecutaremos el ggplot para graficar la precipitación-temperatura de Paruro          

        names(Pp_pisco)
        ggplot(Pp_pisco, aes(fecha, PARURO)) +
          geom_line() +
          geom_smooth(span = 0.4)
         
![Rplot06](https://user-images.githubusercontent.com/78572913/107046803-0b29aa00-6795-11eb-989b-e2c569c8fa44.png) 

Gráfico de precipitación-temperatura de Colquepata

        ggplot(Pp_pisco, aes(fecha, COLQUEPATA)) +
          geom_line() +
          geom_smooth(span = 0.5)
          
![Rplot07](https://user-images.githubusercontent.com/78572913/107046981-4926ce00-6795-11eb-8320-669a8c04d0bf.png)

Gráfico de precipitación-temperatura de CayCay

        ggplot(Pp_pisco, aes(fecha, CAICAY)) +
          geom_line() +
          geom_smooth(span = 0.5)
          
![Rplot08](https://user-images.githubusercontent.com/78572913/107049819-8d679d80-6798-11eb-818b-e669c9b162aa.png)  

Gráfico de precipitación-temperatura  de Ccatcca

        ggplot(Pp_pisco, aes(fecha, CCATCCA)) +
          geom_line() +
          geom_smooth(span = 0.5)
        names(Pp_pisco)
        
![Rplot09](https://user-images.githubusercontent.com/78572913/107049915-a708e500-6798-11eb-8bc5-131a03061a98.png)


Con la función de ggplot nos ayudamos para crear historigramas de las frecuencias de temperaturas

Histograma de Pisac

        ggplot(tmedia_pisco, aes(PISAC)) + geom_histogram(colour= 'blue')
        
![Rplot10](https://user-images.githubusercontent.com/78572913/107051786-dd476400-679a-11eb-8642-672abe1b27ce.png)


Histograma de Paruro

        ggplot(tmedia_pisco, aes(PARURO)) + geom_histogram(colour= 'blue')
        
 ![Rplot11](https://user-images.githubusercontent.com/78572913/107051830-e9332600-679a-11eb-8002-72eb1acfbc26.png) 
 
Histograma de Colquepata

        ggplot(tmedia_pisco, aes(COLQUEPATA) + geom_histogram(colour= 'blue)
        
![Rplot12 (1)](https://user-images.githubusercontent.com/78572913/107051862-f3552480-679a-11eb-9efb-a9afdd164c3b.png)  

Histograma de CCatca

    ggplot(tmedia_pisco, aes(CCATCCA)) + geom_histogram(colour= 'blue')
    
![Rplot13](https://user-images.githubusercontent.com/78572913/107051899-fe0fb980-679a-11eb-87e4-d97e09c01b59.png)


Histograma de  CayCay

    ggplot(tmedia_pisco, aes(CAICAY)) + geom_histogram(colour= 'blue')    
![Rplot14](https://user-images.githubusercontent.com/78572913/107053947-79726a80-679d-11eb-9f52-6d3013d1594b.png)
