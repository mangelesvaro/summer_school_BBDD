# summer_school_BBDD
Archivos relacionados con el workshop Introducción a las BBDD y SQL del Summer School de Geoforest

# Workshop Introducción a bases de datos y SQL

Se va a trabajar con el monte público Pinar de Yunquera, con código MA-30037-AY. Se trata de un monte de unas 2.000 ha de titularidad pública, perteneciente al Ayuntamiento de Yunquera y cuya gestión ha venido realizando la Consejería de Medio Ambiente de la Junta de Andalucía. Está localizado en el interior del Parque Nacional Sierra de las Nieves y contiene una variedad florística de incalculable valor.

## 1. Tipos de Bases de datos

La función principal de una base de datos es almacenar información. Con este criterio tan genérico hay muchas soluciones y muchos tipos de bases de datos, con características muy distintas y, como consecuencia, existen múltiples formas de clasificarlas. Una de las clasificaciones que más se utiliza en la actualidad distingue entre bases de datos relacionales SQL y bases de datos no relacionales o NoSQL. 

![](./Auxiliares/Tipos_BBDD.png)

SQL es el lenguaje que se utiliza para administrar y recuperar las bases de datos relacionales y que ha terminado identificándolas.

### Procesado de información en Bases de Datos relacionales

La información que proporcionan las parcelas medidas en una única ocasión no se incluye, lógicamente, el crecimiento de las variables dendrométricas y dasométricas, por lo que con esos datos no es posible utilizar determinadas técnicas de ajuste estadístico que resultan muy efectivas y prácticas si se dispone de datos de crecimiento. Por tanto, a partir de los datos de un único inventario sólo es posible la elaboración de modelos estáticos, como son las tablas de producción de selvicultura media observada, que reflejan únicamente un número limitado de evoluciones de la densidad, o los diagramas de manejo de la densidad. La realización de un segundo inventario permite disponer de datos reales de crecimiento, lo que posibilita el desarrollo de modelos dinámicos, más realistas que los estáticos.

El Inventario Forestal Nacional (IFN) podría definirse como un proyecto encaminado a obtener el máximo de información posible sobre la situación, régimen de propiedad y protección, naturaleza, estado legal, probable evolución y capacidad productora de todo tipo de bienes de los montes españoles. Este inventario caracteriza los tipos de montes en España, cuantificando los recursos forestales disponibles, y presentando datos de densidades, existencias, crecimientos, etc., y facilitando otros parámetros que describen los bosques y las superficies desarboladas en España así como su biodiversidad, todo ello con una metodología y características comunes para todo el territorio. El inventario proporciona una información estadística homogénea y adecuada sobre el estado y la evolución de los ecosistemas forestales españoles que sirve, entre otros, como instrumento para la coordinación de las políticas forestales y de conservación de la naturaleza. La unidad básica de trabajo es la provincia y, al ser un inventario continuo, se repiten las mismas mediciones cada 10 años, recorriéndose todo el territorio nacional en cada ciclo decenal [^1]

[^1]:https://www.mapa.gob.es/es/desarrollo-rural/estadisticas/Inventarios_nacionales.aspx

En la página web del Ministerio para la Transición Ecológica y el Reto Demográfico se puede encontrar [documentación relativa para la descripción de las tablas y códigos utilizados en esta base de datos.](https://www.miteco.gob.es/es/biodiversidad/servicios/banco-datos-naturaleza/documentador_bdcampo_ifn3_tcm30-282240.pdf)  

Los diferentes organismos con competencia en materia forestal dedican mucho tiempo y recursos para obtener, procesar e interpretar datos. Asimismo, cada administración abarca un ámbito específico, ya sea a nivel territorial, como municipal, provincial, autonómico o estatal, o a nivel competencial. Por ejemplo, cada ministerio se enfoca en datos de áreas concretas como transición ecológica, sanidad, movilidad, entre otras. En la actualidad, las administraciones públicas gestionan vastas cantidades de datos en diversos formatos y con diferentes métodos de gestión. El análisis de datos forestales, dada la variedad de fuentes y formatos disponibles, requiere, previo a su utilización, de procesos que permitan su uso conjunto. Generalmente, para la utilización de los datos, se consideran necesarias 5 etapas, que son las que vamos a seguir en la práctica:

![](./Auxiliares/Etapas_datos.png)

### 1.1. Recopilación de los datos

#### 1.1.1. IFN2

El segundo ciclo del Inventario Forestal Nacional (IFN2) se inició en 1986 y acabó en 1996. Los datos presentados contienen toda la información disponible del IFN2 digitalizada para la correspondiente provincia y se presenta en dos formas, cartográfica y alfanumérica. 

La primera en un formato tipo sistema de información geográfica (SIG) y corresponde a los estratos, los tipos de propiedad y a las parcelas de campo.

La información alfanumérica está separada en dos grupos: tablas de la publicación y ficheros del proceso de datos. El primero contiene los mismos cuadros de letras y cifras que el libro en soporte papel publicado. En cambio, Los ficheros del proceso de datos se componen de la información presente en los estadillos de las parcelas de campo, de los resultados intermedios del proceso no publicados y de los estadísticos de los parámetros de los árboles medidos, especialmente interesantes para los análisis dendrométricos y dasométricos. 

Todos los datos se encuentran disponibles a través de la web del Ministerio para la Transición Ecológica y el Reto Demográfico en [este enlace.](https://www.miteco.gob.es/es/biodiversidad/servicios/banco-datos-naturaleza/informacion-disponible/ifn2_descargas.aspx)

![](./Auxiliares/IFN2.png)

#### 1.1.2. IFN3

La información del tercer ciclo del Inventario Forestal Nacional (IFN3) fue realizado entre los años 1997-2007 y se encuentra disponible en ficheros MDB de Access comprimidos en formato ZIP o bien en formato .accdb a través de la web del Ministerio para la Transición Ecológica y el Reto Demográfico en [este enlace.](https://www.miteco.gob.es/es/biodiversidad/servicios/banco-datos-naturaleza/informacion-disponible/ifn3_base_datos_26_50.aspx)

![](./Auxiliares/IFN3.jpg)

![](./Auxiliares/IFN3.png)

### 1.2. Preparación y limpieza de los datos

#### 1.2.1. Creación de la base de datos en Spatialite

![](./Auxiliares/spatialite-logo.png)

Si bien PostGIS es el gestor de bases de datos más utilizado por proporcionar capacidades de base de datos espacial a múltiples usuarios al mismo tiempo, QGIS también admite el uso de un formato de archivo llamado SpatiaLite que es una forma ligera y portátil de almacenar una base de datos espacial completa en un solo archivo. SpatiaLite es un motor de bases de datos SQLite al que se han agregado funciones espaciales. SQLite is un Sistema Gestor de Bases de Datos (DBMS, por sus siglas en inglés) que es simple, robusto, fácil de usar y realmente poco pesado. Cada base de datos es simplemente un archivo. Se puede copiar, comprimir y portar entre Windows, Linux, MacOs, etc.

Usando el panel del navegador, podemos crear una nueva base de datos SpatiaLite y configurarla para usarla en QGIS.

![](./Auxiliares/Crear_BBDD_Spatialite.png)

Posteriormente, usando el administrador de Bases de Datos de Qgis podremos acceder a ella.

![](./Auxiliares/Administrador_BBDD.png)

### 1.2.2. IFN2

Durante el procesado de los datos del segundo Inventario Nacional, se generaron 5 tablas con los datos recogidos en campo y 3 más con los datos procesados y agrupados.

![](./Auxiliares/IFN2_tablas.png)

Por otro lado, las tablas se sirven en formato *.dbf*. Históricamente, este tipo de archivos supusieron una solución de base de datos muy popular para MS-DOS que más tarde fue llevado a otras plataformas como Unix y dio inicio a una serie de productos similares. Básicamente, este formato permite organizar los datos en varios registros con campos con un encabezado con información sobre la estructura de datos y de los registros mismos. Además, es compatible con Windows, Linus y Mac.

Para el desarrollo del ejercicio, se van a emplear la tabla de pies mayores y la de valores resumidos por estadillo. Para ello se van a importar en nuestra base de datos las tablas de *PIESMA29.dbf* y *IIFL03BD.dbf*

![](./Auxiliares/Importar.png)

### 1.2.3. IFN3

Por otro lado, la gestión de la información perteneciente al IFN3 es algo diferente. Una vez descargados y descomprimidos los ficheros de bases de datos SIG y Campo de la provincia de Málaga es posible abrirlos empleando la librería odbc. Con Microsoft Access es posible exportar las tablas necesarias a csv para poder ser utilizadas en Spatialite.

![](./Auxiliares/Access.png)

Para importar la tabla de parcelas (*PCParcelas*) primero es necesario introducirla como capa csv dentro de QGIS.

![](./Auxiliares/Introducir_csv.png)

El resultado es la representación cartográfica de las parcelas empleadas en el inventario. Los campos "CoorX" y "Coory" representan las coordenadas X e Y en el sistema de referencia local que se empleaba cuando se generaron los datos, el sistema European Datum 1950 UTM zona 30 Norte. Sin embargo, como se verá ahora, estos campos presentan errores que debemos subsanar para poder realizar el ploteado.

![](./Auxiliares/PCParcelas.png)

Hay numerosos registros en los que el valor de la coordenada X o Y es 0. También se dan errores humanos de introducción de los datos, como uno de los registros de las coordenadas X por encima del parámetro de *Falso Este* de 500.000 de los valores de definición de la proyección o el registro con valor de 4092.2 en las coordenadas de las Y, en lugar de valores por encima de 4.000.000, que es donde se sitúan las latitudes en las que se encuentra el área de estudio. 

![](./Auxiliares/PCParcelas_tabla.png)

![](./Auxiliares/PCParcelas_Malaga.png)

![](./Auxiliares/Guardar.png)

Y ya aprecerá en la base de datos que hemos creado como una tabla más con información geográfica.

![](./Auxiliares/Administrador_BBDD2.png)

Veamos el contenido de la tabla de Pies Mayores. 

![](./Auxiliares/Introducir_csv2.png)

![](./Auxiliares/Introducir_csv3.png)



### 1.3. Análisis de los datos

Se va a analizar el crecimiento en diámetros y altura de los pies de la especie *Abies pinsapo* (código 32 en las tablas del IFN)

Por un lado, en el Inventario Forestal Nacional, el diámetro normal se mide cuidadosamente a 1,30 m del suelo, con una forcípula graduada para apreciar el milímetro, en dos direcciones perpendiculares, de tal manera que, en la primera de ellas, el eje del aparato esté alineado con el centro de la parcela. Para el análisis de datos, necesitamos el valor medio de ambas mediciones.

Para el IFN2

```sql
CREATE VIEW diam_medio_IFN2 AS
SELECT
    estadillo AS estadillo,
	numorden AS numorden,
	(diametro1 + diametro2)/2 as Dn
FROM
    PIESMA29
```



Se va a evaluar más concretamente la especie de pinsapo, que corresponde a la de código 32.

```r
#Pies de pinsapo
pinsapo<-Pies.monte.IFN[which(Pies.monte.IFN$ESPECIE.x=="032"),]

#Parcelas de pinsapo
parcelas.pinsapo<-unique(pinsapo[,c('ESTADILLO', 'geometry')])
```

Será interesante conocer cómo se comportan los crecimientos en altura y en diámetro normal según las densidades de la parcela y ver qué patrón espacial representa el resultado. Por eso, se agregan los valores medios por parcela y se le añaden los valores del resumen del total de densidad de pies de cada parcela y la geometría de las parcelas.

```r
#Valores medios de crecimientos por ESTADILLO
resumen.pinsapo<-aggregate(cbind(Dif.DN,Dif.H)~ESTADILLO,
                     data=pinsapo,FUN=mean,na.rm=TRUE)

#Añadir valores de densidad de las parcelas
resumen.pinsapo<-merge(resumen.pinsapo,resumen.parcela,by="ESTADILLO")

#Añadir geometría
resumen.pinsapo<-merge(resumen.pinsapo,parcelas.pinsapo,by="ESTADILLO")

#Conversión de la tabla en datos geográficos
resumen.pinsapo<-st_as_sf(resumen.pinsapo)
```

Se puede explorar la asociación de interdependencia que cabría pensar que pudiera existir entre los crecimientos en altura y en diámetro, con la densidad de la parcela. 

Para saber cuál método usar para calcular el coeficiente de correlación entre las variables, primero es necesario explorar si tienen una distribución normal.

```r
#Histograma de los datos de altura
hist(resumen.pinsapo$Dif.H)
```

![](./Auxiliares/Dif.H.png)

```r
#Histograma de los datos de dap
hist(resumen.pinsapo$Dif.DN)
```

![](./Auxiliares/Dif.DN.png)

```r
#Histograma de los datos de dap
hist(resumen.pinsapo$Npies_parc)
```

![](./Auxiliares/Npies_parc.png)

Ninguna de las variables sigue una distribución normal, por lo que el método empleado será el de Spearman.

```r
#Test de correlación entre las alturas y las densidades
correlacion.alturas<-cor.test(resumen.pinsapo$Dif.H,
                              resumen.pinsapo$Npies_parc,
                              method="spearman")

#Correlación entre las alturas y las densidades
correlacion.alturas$estimate
```

```r annotate
##       rho 
## 0.4457831
```

```r
#Significancia de la correlación entre las alturas y las densidades
correlacion.alturas$p.value
```

```r annotate
## [1] 0.268289
```

```r
#Test de correlación entre los diámetros y las densidades
correlacion.diametros<-cor.test(resumen.pinsapo$Dif.DN,
                                resumen.pinsapo$Npies_parc,
                                method="spearman")

#Correlación entre los diámetros y las densidades
correlacion.diametros$estimate
```

```r annotate
##        rho 
## -0.3493976 
```

```r
#Significancia de la correlación entre las alturas y las densidades
correlacion.diametros$p.value
```

```r annotate
## [1] 0.3962443
```

Ninguna de las correlaciones con la densidad parece tener significancia estadística (*p-valores* por encima de 0.05). Sin embargo, este bajo valor es probable que se deba a la muestra tan pequeña de parcelas en la que se ha empleado, tan sólo 8 parcelas (Ver el número de registros de la tabla *resumen.pinsapo*). Sería necesario aumentar la superficie de estudio y, por tanto, el número de parcelas, para contrastar los resultados, ya que la asociación de interdependencia esperada responde a una hipótesis plausible y lógica contrastada en el mundo forestal. 

### 1.4. Visualización de los datos

Se puede entender la dinámica general del crecimiento en las especies forestales del monte, a través de un gráfico donde se representen los segmentos de crecimiento de cada uno de los pies.

```r
plot(0,0,col = "white",
     xlim=c(80,900),ylim=c(0,21),main="Crecimientos.Todas las especies",
     xlab="dap (mm)", ylab="altura (m)")
segments(Pies.monte.IFN$DN.IFN3,Pies.monte.IFN$HT,
         Pies.monte.IFN$DN.IF2,Pies.monte.IFN$HT.IFN2,
         col=rgb(0,0,0,0.35))
```

![](./Auxiliares/Cremientos.png)

Si se particulariza a nivel especie, se pueden intuir dinámicas distintas.

```{r }
plot(0,0,col = "white",
     xlim=c(80,900),ylim=c(0,21),main="Crecimientos.Todas las especies",
     xlab="dap (mm)", ylab="altura (m)")
#Especie 021="Pinus sylvestris"
segments(Pies.monte.IFN$DN.IFN3[which(Pies.monte.IFN$ESPECIE.x=="021")], 
         Pies.monte.IFN$HT[which(Pies.monte.IFN$ESPECIE.x=="021")],
         Pies.monte.IFN$DN.IF2[which(Pies.monte.IFN$ESPECIE.x=="021")],
         Pies.monte.IFN$HT.IFN2[which(Pies.monte.IFN$ESPECIE.x=="021")],
         col="red")
#Especie 024="Pinus halepensis"
segments(Pies.monte.IFN$DN.IFN3[which(Pies.monte.IFN$ESPECIE.x=="024")], 
         Pies.monte.IFN$HT[which(Pies.monte.IFN$ESPECIE.x=="024")],
         Pies.monte.IFN$DN.IF2[which(Pies.monte.IFN$ESPECIE.x=="024")],
         Pies.monte.IFN$HT.IFN2[which(Pies.monte.IFN$ESPECIE.x=="024")],
         col="green")
#Especie 026="Pinus pinaster"
segments(Pies.monte.IFN$DN.IFN3[which(Pies.monte.IFN$ESPECIE.x=="026")],
         Pies.monte.IFN$HT[which(Pies.monte.IFN$ESPECIE.x=="026")],
         Pies.monte.IFN$DN.IF2[which(Pies.monte.IFN$ESPECIE.x=="026")],
         Pies.monte.IFN$HT.IFN2[which(Pies.monte.IFN$ESPECIE.x=="026")],
         col="black")
#Especie 032="Abies pinsapo"
segments(Pies.monte.IFN$DN.IFN3[which(Pies.monte.IFN$ESPECIE.x=="032")],
         Pies.monte.IFN$HT[which(Pies.monte.IFN$ESPECIE.x=="032")],
         Pies.monte.IFN$DN.IF2[which(Pies.monte.IFN$ESPECIE.x=="032")],
         Pies.monte.IFN$HT.IFN2[which(Pies.monte.IFN$ESPECIE.x=="032")],
         col="blue")
legend("bottomright",legend=c("Pinus sylvestris","Pinus halepensis",
                              "Pinus pinaster","Abies pinsapo"),
       col=c("red","green","black","blue"),lty=c(1,1,1,1),cex=0.75,
       box.lty=0)
```

![](./Auxiliares/Cremientos2.png)

La especie que parece mostrar mayor variabilidad de comportamiento en su crecimiento es el pinsapo. Por eso, se va a particularizar para dicha especie.

```{r }
plot(0,0,col = "white",
     xlim=c(80,900),ylim=c(0,21),main="Crecimientos.Pinsapo",
     xlab="dap (mm)", ylab="altura (m)")
segments(pinsapo$DN.IFN3,pinsapo$HT,
         pinsapo$DN.IF2,pinsapo$HT.IFN2,
         col=rgb(0,0,0,0.35))
```

![](./Auxiliares/Cremientos3.png)


