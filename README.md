# summer_school_BBDD
Archivos relacionados con el workshop Introducción a las BBDD y SQL del Summer School de Geoforest

Mª Ángeles Varo Martínez y Rafael Mª Navarro Cerrillo

# Workshop Introducción a bases de datos y SQL

Se va a trabajar con el monte público Pinar de Yunquera, con código MA-30037-AY. Se trata de un monte de unas 2.000 ha de titularidad pública, perteneciente al Ayuntamiento de Yunquera y cuya gestión ha venido realizando la Consejería de Medio Ambiente de la Junta de Andalucía. Está localizado en el interior del Parque Nacional Sierra de las Nieves y contiene una variedad florística de incalculable valor.

## 1. TIPOS DE BASES DE DATOS

## PROCESADO DE INFORMACIÓN EN BASES DE DATOS RELACIONALES

La información que proporcionan las parcelas medidas en una única ocasión no se incluye, lógicamente, el crecimiento de las variables dendrométricas y dasométricas, por lo que con esos datos no es posible utilizar determinadas técnicas de ajuste estadístico que resultan muy efectivas y prácticas si se dispone de datos de crecimiento. Por tanto, a partir de los datos de un único inventario sólo es posible la elaboración de modelos estáticos, como son las tablas de producción de selvicultura media observada, que reflejan únicamente un número limitado de evoluciones de la densidad, o los diagramas de manejo de la densidad. La realización de un segundo inventario permite disponer de datos reales de crecimiento, lo que posibilita el desarrollo de modelos dinámicos, más realistas que los estáticos.

El Inventario Forestal Nacional (IFN) podría definirse como un proyecto encaminado a obtener el máximo de información posible sobre la situación, régimen de propiedad y protección, naturaleza, estado legal, probable evolución y capacidad productora de todo tipo de bienes de los montes españoles. Este inventario caracteriza los tipos de montes en España, cuantificando los recursos forestales disponibles, y presentando datos de densidades, existencias, crecimientos, etc., y facilitando otros parámetros que describen los bosques y las superficies desarboladas en España así como su biodiversidad, todo ello con una metodología y características comunes para todo el territorio. El inventario proporciona una información estadística homogénea y adecuada sobre el estado y la evolución de los ecosistemas forestales españoles que sirve, entre otros, como instrumento para la coordinación de las políticas forestales y de conservación de la naturaleza. La unidad básica de trabajo es la provincia y, al ser un inventario continuo, se repiten las mismas mediciones cada 10 años, recorriéndose todo el territorio nacional en cada ciclo decenal [^1]

[^1]:https://www.mapa.gob.es/es/desarrollo-rural/estadisticas/Inventarios_nacionales.aspx

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

### 1.2.1. IFN3

Una vez descargadas y descomprimidas los ficheros de bases de datos SIG y Campo de la provincia de Málaga es posible abrirlos empleando la librería odbc, cuyo objetivo es proporcionar una interfaz para los controladores de Open Database Connectivity (ODBC) y también la librería DBI que permite una definición de interfaz de la base de datos para la comunicación entre R y los sistemas de gestión de bases de datos relacionales.

```r
#Librería ODBC que permite la conectividad ODBC con Bases de Datos
# install.packages("odbc")
library(odbc)
# Librería DBI que permite comunicación con BD relacional
# install.packages("DBI")
library(DBI)
```

```r
#Listar drivers
odbc::odbcListDrivers()
```

```r annotate
## name 	attribute 	value
## SQL Server 	APILevel 	2
## SQL Server 	ConnectFunctions 	YYY
## SQL Server 	CPTimeout 	60
## SQL Server 	DriverODBCVer 	03.50
## SQL Server 	FileUsage 	0
## SQL Server 	SQLLevel 	1
## SQL Server 	UsageCount 	1
## Microsoft Access Driver (.mdb, .accdb) 	UsageCount 	3
## Microsoft Access Driver (.mdb, .accdb) 	APILevel 	1
## Microsoft Access Driver (.mdb, .accdb) 	ConnectFunctions 	YYN
## Microsoft Access Driver (.mdb, .accdb) 	DriverODBCVer 	02.50
## Microsoft Access Driver (.mdb, .accdb) 	FileUsage 	2
## Microsoft Access Driver (.mdb, .accdb) 	FileExtns 	.mdb,.accdb
## Microsoft Access Driver (.mdb, .accdb) 	SQLLevel 	0
## Microsoft Excel Driver (.xls, .xlsx, .xlsm, .xlsb) 	UsageCount 	3
## Microsoft Excel Driver (.xls, .xlsx, .xlsm, .xlsb) 	APILevel 	1
## Microsoft Excel Driver (.xls, .xlsx, .xlsm, .xlsb) 	ConnectFunctions 	YYN
## Microsoft Excel Driver (.xls, .xlsx, .xlsm, .xlsb) 	DriverODBCVer 	02.50
## Microsoft Excel Driver (.xls, .xlsx, .xlsm, .xlsb) 	FileUsage 	2
## Microsoft Excel Driver (.xls, .xlsx, .xlsm, .xlsb) 	FileExtns 	.xls,.xlsx, *.xlsb
## Microsoft Excel Driver (.xls, .xlsx, .xlsm, .xlsb) 	SQLLevel 	0
## Microsoft Access Text Driver (.txt, .csv) 	UsageCount 	3
## Microsoft Access Text Driver (.txt, .csv) 	APILevel 	1
## Microsoft Access Text Driver (.txt, .csv) 	ConnectFunctions 	YYN
## Microsoft Access Text Driver (.txt, .csv) 	DriverODBCVer 	02.50
## Microsoft Access Text Driver (.txt, .csv) 	FileUsage 	2
## Microsoft Access Text Driver (.txt, .csv) 	FileExtns 	.txt, .csv
## Microsoft Access Text Driver (.txt, .csv) 	SQLLevel 	0 
```

Si no aparece "Microsoft Access dBASE Driver" se deberá instalar el [driver de DB Access](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54920)

Para comenzar, se introduce la ruta a la base de datos de campo y se establece la conexión:
```r
#Ruta a la Base de datos de Access
IFN3 = "C:/DESCARGA//IFN3/Malaga/Ifn3p29.accdb" #Adaptar a la ruta que se vaya a emplear en el equipo

#Conexión a BD Access. Indicamos encoding
con <- dbConnect(odbc::odbc(), 
                 .connection_string = paste0("Driver={Microsoft Access Driver (*.mdb, *.accdb)};DBQ=",IFN3,";"),
                 encoding = "latin1")
```

Se ha abierto, por tanto, un canal hacía la base de datos al que se le ha llamado *con*. Se trabaja con ella como un objeto que contiene toda la información para hacer una conexión usando ODBC, incluyendo el tipo de conexión, la dirección de la máquina donde está la base de datos, y el nombre de la base de datos.  

Finalmente, se imprime en pantalla el listado de tablas presentes en la base de datos.
```r
#Listado de tablas de la base de datos
dbListTables(con) 
```

```r annotate
##  [1] "MSysAccessStorage"          "MSysAccessXML"             
##  [3] "MSysACEs"                   "MSysComplexColumns"        
##  [5] "MSysIMEXColumns"            "MSysIMEXSpecs"             
##  [7] "MSysNavPaneGroupCategories" "MSysNavPaneGroups"         
##  [9] "MSysNavPaneGroupToObjects"  "MSysNavPaneObjectIDs"      
## [11] "MSysObjects"                "MSysQueries"               
## [13] "MSysRelationships"          "Errores de conversión"     
## [15] "PCDatosMap"                 "PCEspMapa"                 
## [17] "PCEspParc"                  "PCMatorral"                
## [19] "PCMayores"                  "PCMayores2"                
## [21] "PCNueEsp"                   "PCParcelas"                
## [23] "PCRegenera"                 "PCTablaEsp"
```

Todas las bases de datos de Microsoft Access contienen varias tablas de "sistema" (todas las tablas cuyo nombre comienza por *Msys*) que se utilizan para codificar metadatos sobre la base de datos, como relaciones de clave primaria/clave externa (*MsysRelationships*) y tiempos de creación y actualización de consultas (*MsysObjects*). De forma predeterminada, el usuario no puede acceder a estas tablas, pero un usuario avanzado puede ejecutar consultas de alto nivel en ellas.  

Este apartado se centrará, por el contrario, en utilizar las tablas referentes a la información forestal. En la página web del Ministerio para la Transición Ecológica y el Reto Demográfico se puede encontrar [documentación relativa para la descripción de las tablas y códigos utilizados en esta base de datos.](https://www.miteco.gob.es/es/biodiversidad/servicios/banco-datos-naturaleza/documentador_bdcampo_ifn3_tcm30-282240.pdf)  

Se comienza leyendo los campos de la tabla de parcelas (*PCParcelas*)

```r
#Leer tabla
PCParcelas <- dbReadTable(con, "PCParcelas")

#Ver datos de las 2 primeras filas
head(PCParcelas,2)

#Convertir la matriz en un data frame para poder trabajar mejor con ella en R
PCParcelas<-as.data.frame(PCParcelas)
names(PCParcelas)
```

```r annotate
##  [1] "Provincia" "Estadillo" "Cla"       "Subclase"  "CoorX"     "Coory"    
##  [7] "Tipo"      "Vuelo1"    "Pasada1"   "Foto1"     "Vuelo2"    "Pasada2"  
## [13] "Foto2"     "Ano"       "INE"       "Nivel1"    "Nivel2"    "Nivel3"   
## [19] "FccTot"    "FccArb"    "DisEsp"    "ComEsp"    "Rocosid"   "Textura"  
## [25] "MatOrg"    "PhSuelo"   "FechaPh"   "HoraPh"    "TipSuelo1" "TipSuelo2"
## [31] "TipSuelo3" "MErosiva"  "ModComb"   "EspCMue"   "PresReg"   "EfecReg"  
## [37] "CortaReg"  "MejVue1"   "MejVue2"   "MejSue1"   "MejSue2"   "Orienta1" 
## [43] "Orienta2"  "MaxPend1"  "MaxPend2"  "Localiza"  "Acceso"    "Levanta"  
## [49] "Obser"     "Equipo"    "JefeEq"    "FechaIni"  "HoraIni"   "FechaFin" 
## [55] "HoraFin"   "Tiempo"    "Resid"     "RumboF1"   "RumboF2"   "DistFoto" 
## [61] "CarFoto1"  "NumFoto1"  "ConFoto1"  "CarFoto2"  "NumFoto2"  "ConFoto2" 
## [67] "Estado"    "Tecnico"
```

Se va a intentar la representación cartográfica de las parcelas empleadas en el inventario. Los campos "CoorX" y "Coory" representan las coordenadas X e Y en el sistema de referencia local que se empleaba cuando se generaron los datos, el sistema European Datum 1950 UTM zona 30 Norte. Sin embargo, como se verá ahora, estos campos presentan errores que debemos subsanar para poder realizar el ploteado.

```r
summary(PCParcelas$CoorX)
```

```r annotate
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0  301996  323011  321470  350361 2985988
```

```r
summary(PCParcelas$Coory)
```

```r annotate
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0 4048976 4059702 3906133 4077000 4127000
```

Hay numerosos registros en los que el valor de la coordenada X o Y es 0. También se dan errores humanos de introducción de los datos, como uno de los registros de las coordenadas X por encima del parámetro de *Falso Este* de 500.000 de los valores de definición de la proyección o el registro con valor de 4092.2 en las coordenadas de las Y, en lugar de valores por encima de 4.000.000, que es donde se sitúan las latitudes en las que se encuentra el área de estudio. 

```r
PCParcelas<-PCParcelas[which(PCParcelas$CoorX>0),]
PCParcelas<-PCParcelas[which(PCParcelas$CoorX<500000),]
PCParcelas<-PCParcelas[which(PCParcelas$Coory>4000000),]
```
Al eliminarlos se puede visualizar la localización de las parcelas. Para ello, se empleará la librería *sf*, que proporciona una forma estandarizada de codificar datos vectoriales.  

```r
#Activar la librería sf necesaria para datos espaciales
# install.packages("sf")
library(sf)
#Convertir data frame a SpatialPointsDataFrame
IFN3.sp <- st_as_sf(x=PCParcelas,coords=c("CoorX","Coory"), crs=23030) #EPSG:32630 ED50 UTM30N
plot(st_geometry(IFN3.sp), axes=TRUE,main="Parcelas IFN3 en la provincia de Málaga")
```

![](./Auxiliares/Mapa_parcelas.png)

El siguiente paso corresponde a la selección de las parcelas encontradas en el interior del monte Pinar de Yunquera. Para ello, ambas capas deben compartir el mismo sistema de referencia.  
Una vez realizada la transformación del sistema de coordenadas, ya es posible la representación conjunta de los datos.

```r
library(sf)
#Proyección de la capa del monte al mismo crs que los puntos
Pinar.Yunquera<-st_transform(Pinar.Yunquera, crs=32630) #EPSG:32630 WGS84 UTM30N

#Convertir los datos a coordenadas geograficas en wgs84
IFN3.sp.WGS84.geograf<-st_transform(IFN3.sp,4326) #EPSG:4326 WGS84 geográficas (latitud,longitud)

#Convertir los datos a coordenadas cartograficas en wgs84
IFN3.sp.WGS84.UTM30N<-st_transform(IFN3.sp.WGS84.geograf,crs=st_crs(32630))

plot(st_geometry(IFN3.sp.WGS84.UTM30N),axes=TRUE,main="Parcelas IFN3 en la provincia de Málaga")
plot(st_geometry(Pinar.Yunquera), border="red", add=TRUE)
```

![](./Auxiliares/Mapa_parcelas2.png)

Del total de las parcelas de la provincia de Málaga, se seleccionarán las que se localizan en el interior de la zona de estudio, del monte. Para ello, se realizará una intersección geográfica entre ambas capas.

```r
#Selección de parcelas
IFN3.monte<-IFN3.sp.WGS84.UTM30N[which(st_within(IFN3.sp.WGS84.UTM30N,
                                            st_geometry(Pinar.Yunquera),
                                            sparse=FALSE)==TRUE),]

length(unique(IFN3.monte$Estadillo))
```

```r annotate
## [1] 27
```

27 son las parcelas del IFN en el monte Pinar de Yunquera.

Abrimos ahora la tabla de Pies Mayores.

```r
#Leer tabla de pies mayores
PCMayores <- dbReadTable(con, "PCMayores")

#Nombre de los campos de la tabla
names(PCMayores)
```

```r annotate
##  [1] "Estadillo" "Cla"       "Subclase"  "nArbol"    "OrdenIf3"  "OrdenIf2" 
##  [7] "Rumbo"     "Distanci"  "DRed"      "Especie"   "Dn1"       "Dn2"      
## [13] "Ht"        "Calidad"   "Forma"     "ParEsp"    "Agente"    "Import"   
## [19] "Elemento"  "Compara"
```

Al tratarse de una base de datos relacional, para cada *Estadillo* o parcela de la tabla *PCParcelas* existen varios elementos u observaciones que corresponden en la tabla *PCMayores*. Ahora deben seleccionarse los pies de las parcelas que están incluidos en el interior del monte.

```r
#Unión de tablas por el concepto común Estadillo
Pies.IFN3.monte<-merge(IFN3.monte,PCMayores,by="Estadillo",all.x=TRUE)
```

### 1.2.2. IFN2

Por otro lado, la gestión de la información perteneciente al IFN2 es algo diferente. Durante el procesado de los datos, se generaron 5 tablas con los datos recogidos en campo y 3 más con los datos procesados y agrupados.

![](./Auxiliares/IFN2_tablas.png)

Por otro lado, las tablas descargadas referentes al Inventario Forestal Nacional 2 se sirven en formato *.dbf*. Históricamente, este tipo de archivos supusieron una solución de base de datos muy popular para MS-DOS que más tarde fue llevado a otras plataformas como Unix y dio inicio a una serie de productos similares. Básicamente, este formato permite organizar los datos en varios registros con campos con un encabezado con información sobre la estructura de datos y de los registros mismos. Además, es compatible con Windows, Linus y Mac.

Para el desarrollo del ejercicio, se van a emplear la tabla de pies mayores y la de valores resumidos por estadillo.

```r
#Librería que permite la lectura de archivos .dbf
library(foreign)

#Leer tabla de pies mayores
Pies.Mayores.IFN2<-read.dbf("E:/MAVARO/clases/Centro_Competencias_Digitales/DESCARGAS_IFN/IFN2/Malaga/PIESMA29.dbf")

#Leer tabla de valores agrupados por estadillo
resumen.parcela.IFN2<-read.dbf("E:/MAVARO/clases/Centro_Competencias_Digitales/DESCARGAS_IFN/IFN2/Malaga/IIFL03BD.dbf")
```

Para poder trabajar con datos temporales pertenecientes a ambos inventarios, es necesario homogeneizar los nombres y tipos de los campo.
Así, por ejemplo, los nombres de los campos de las tablas del IFN2 están en mayúscula, mientras que las del IFN3 están en minúscula. Es necesario unificar criterios para poder ejecutar uniones de tablas. Sin embargo, es necesario tener en cuenta que los campos que se hacen referencia a la geometría espacial de los puntos, deben permanecer en su estado actual para que puedan ser identificados como tales.

```r
#Nombres de los campos de la tabla del IFN2
names(Pies.Mayores.IFN2)
```

```r annotate
##  [1] "PROVINCIA" "ESTADILLO" "NUMORDEN"  "TIPO"      "ARBOL"     "RUMBO"    
##  [7] "DISTANCI"  "ESPECIE"   "DIAMETRO1" "DIAMETRO2" "CALIDAD"   "FORMA"    
## [13] "ALTURA"    "PARAMESP"
```

```r
#Nombres de los campos de la tabla del IFN3
names(Pies.IFN3.monte)
```

```r annotate
##  [1] "Estadillo"  "Provincia"  "Cla.x"      "Subclase.x" "Tipo"      
##  [6] "Vuelo1"     "Pasada1"    "Foto1"      "Vuelo2"     "Pasada2"   
## [11] "Foto2"      "Ano"        "INE"        "Nivel1"     "Nivel2"    
## [16] "Nivel3"     "FccTot"     "FccArb"     "DisEsp"     "ComEsp"    
## [21] "Rocosid"    "Textura"    "MatOrg"     "PhSuelo"    "FechaPh"   
## [26] "HoraPh"     "TipSuelo1"  "TipSuelo2"  "TipSuelo3"  "MErosiva"  
## [31] "ModComb"    "EspCMue"    "PresReg"    "EfecReg"    "CortaReg"  
## [36] "MejVue1"    "MejVue2"    "MejSue1"    "MejSue2"    "Orienta1"  
## [41] "Orienta2"   "MaxPend1"   "MaxPend2"   "Localiza"   "Acceso"    
## [46] "Levanta"    "Obser"      "Equipo"     "JefeEq"     "FechaIni"  
## [51] "HoraIni"    "FechaFin"   "HoraFin"    "Tiempo"     "Resid"     
## [56] "RumboF1"    "RumboF2"    "DistFoto"   "CarFoto1"   "NumFoto1"  
## [61] "ConFoto1"   "CarFoto2"   "NumFoto2"   "ConFoto2"   "Estado"    
## [66] "Tecnico"    "Cla.y"      "Subclase.y" "nArbol"     "OrdenIf3"  
## [71] "OrdenIf2"   "Rumbo"      "Distanci"   "DRed"       "Especie"   
## [76] "Dn1"        "Dn2"        "Ht"         "Calidad"    "Forma"     
## [81] "ParEsp"     "Agente"     "Import"     "Elemento"   "Compara"   
## [86] "geometry"
```

```r
#Conversión a mayúsculas
names(Pies.IFN3.monte)<-toupper(names(Pies.IFN3.monte))

#Recuperación del nombre del campo de geometría
names(Pies.IFN3.monte)[86]<-"geometry"
```

Uno de los campos comunes que sirve para identificar cada pie en cada una de las mediciones temporales es el número de orden que se empleó en la evaluación del árbol en cada inventario. Por eso, es necesario usar una denominación común en ambas tablas.

```r
#Cambiar nombre del campo que marca el orden del número de pie en el IFN2
names(Pies.Mayores.IFN2)[3]<-"ORDENIF2"
```

Además, los campos comunes entre ambas tablas deben pertenecer a la misma clase para poder relacionarlas, es decir, si en una tabla contiene valores numéricos en la otra también deben ser numéricos.

```r
#Comprobación de la clase del objeto en la tabla IFN2
is.numeric(Pies.Mayores.IFN2$ESTADILLO)
```

```r annotate
## [1] TRUE
```

```r
#Comprobación de la clase del objeto en la tabla IFN3
is.numeric(Pies.IFN3.monte$ESTADILLO)
```

```r annotate
## [1] FALSE
```

```r
#Conversión en valor numérico
Pies.IFN3.monte$ESTADILLO<-as.numeric(as.character(Pies.IFN3.monte$ESTADILLO))

#Comprobación de la clase del objeto en la tabla IFN2
is.numeric(Pies.IFN3.monte$ORDENIF2)
```

```r annotate
## [1] FALSE
```

```r
#Comprobación de la clase del objeto en la tabla IFN3
is.numeric(Pies.Mayores.IFN2$ORDENIF2)
```

```r annotate
## [1] TRUE
```

```r
#Conversión en valor numérico
Pies.IFN3.monte$ORDENIF2<-as.numeric(as.character(Pies.IFN3.monte$ORDENIF2))
```

Además, los campos que van a servir para ejecutar operaciones, por ejemplo, la media entre los 2 diámetros normales o la altura de los pies, es necesario que sean de la clase numérica.

```r
#Comprobación de la clase del objeto DIAMETRO1
is.numeric(Pies.Mayores.IFN2$DIAMETRO1)
```

```r annotate
## [1] FALSE
```

```r
#Comprobación de la clase del objeto DIAMETRO2
is.numeric(Pies.Mayores.IFN2$DIAMETRO2)
```

```r annotate
## [1] FALSE
```

```r
#Conversión en valor numérico
Pies.Mayores.IFN2$DN1.IFN2<-as.numeric(as.character(Pies.Mayores.IFN2$DIAMETRO1))

#Conversión en valor numérico
Pies.Mayores.IFN2$DN2.IFN2<-as.numeric(as.character(Pies.Mayores.IFN2$DIAMETRO2))

#Comprobación de la clase del objeto ALTURA
is.numeric(Pies.Mayores.IFN2$ALTURA)
```

```r annotate
## [1] FALSE
```

```r
#Conversión en valor numérico
Pies.Mayores.IFN2$HT.IFN2<-as.numeric(as.character(Pies.Mayores.IFN2$ALTURA))
```

Por otro lado, de nada sirve para los cálculos que falten los datos de cualquiera de los 2 años. Algunos de los árboles inventariados, al ser visitados de nuevo, habían muerto o habían sido tumbados, por lo que no se incorporaron en el IFN3. Puesto que se trata de trabajar con valores multitemporales, se van a seleccionar los pies en los que se ejecutaron mediciones repetidas en uno y otro inventario.

```r
#Selección de pies comunes entre ambos inventarios
Pies.IFN3.monte.com<-Pies.IFN3.monte[which(Pies.IFN3.monte$ORDENIF2!=0),]
```

Finalmente, se unen ambas tablas.

```r
#Unión de tablas del IFN2 y IFN3
Pies.monte.IFN<-merge(Pies.IFN3.monte.com,Pies.Mayores.IFN2,
                      by=c("ESTADILLO","ORDENIF2"))
```

Por otra parte, para considerar la densidad en la caracterización de los crecimientos, se hace uso de la tabla de valores agrupados por estadillo.

```r
resumen.parcela<-aggregate(resumen.parcela.IFN2$NARBOLES,
                           by=list(resumen.parcela.IFN2$CESTADILLO),FUN=sum,
                           na.rm=TRUE)

#Primeros 6 valores de la tabla
head(resumen.parcela)
```

```r annotate
##   Group.1      x
## 1       1 413.79
## 2       2 621.58
## 3      12  80.89
## 4      13  28.28
## 5      14 350.13
## 6      15 459.76
```

```r
#Nombre de los campos de la tabla
names(resumen.parcela)
```

```r annotate
## [1] "Group.1" "x"
```

```r

#Cambio de nombre de los campos de la tabla
names(resumen.parcela)<-c("ESTADILLO","Npies_parc")

#Comprobación de la clase del objeto ESTADILLO
is.numeric(resumen.parcela$ESTADILLO)
```

```r annotate
## [1] TRUE
```

### 1.3. Análisis de los datos

En el Inventario Forestal Nacional, el diámetro normal se mide cuidadosamente a 1,30 m del suelo, con una forcípula graduada para apreciar el milímetro, en dos direcciones perpendiculares, de tal manera que, en la primera de ellas, el eje del aparato esté alineado con el centro de la parcela. Para el análisis de datos, necesitamos el valor medio de ambas mediciones.

```r
#Cálculo de valor medio del diámetro normal en IFN2
Pies.monte.IFN$DN.IF2<-(Pies.monte.IFN$DN1.IFN2+Pies.monte.IFN$DN2.IFN2)/2

#Cálculo de valor medio del diámetro normal en IFN3
Pies.monte.IFN$DN.IFN3<-(Pies.monte.IFN$DN1+Pies.monte.IFN$DN2)/2

#Diferencias de diámetros entre los 2 tiempos
Pies.monte.IFN$Dif.DN<-Pies.monte.IFN$DN.IFN3-Pies.monte.IFN$DN.IF2

#Diferencias de alturas entre los 2 tiempos
Pies.monte.IFN$Dif.H<-Pies.monte.IFN$HT-Pies.monte.IFN$HT.IFN2

#Eliminación de errores de medición
Pies.monte.IFN<-Pies.monte.IFN[which(Pies.monte.IFN$Dif.DN>0&
                                       Pies.monte.IFN$Dif.H>0),]
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

### 1.5. Los datos contando historias...

Espacialmente, se puede visualizar dónde han ocurrido los mayores cambios tanto en diámetros, como en alturas dentro del monte a través de un dashboard que exponga el análisis realizado.

Primero, se guardan la tabla resumen de pinsapo y la capa del monte que se han generado en los procesos anteriores.

```r
st_write(st_zm(Pinar.Yunquera),"C:/DESCARGA/Pinar.Yunquera.shp") #Adaptar a la ruta que se vaya a emplear en el equipo

pinsapo<-pinsapo[,c(1,2,70:105)]

st_write(pinsapo,"C:/DESCARGA/pinsapo.shp") #Adaptar a la ruta que se vaya a emplear en el equipo

st_write(resumen.pinsapo,"C:/DESCARGA/resumen.pinsapo.shp") #Adaptar a la ruta que se vaya a emplear en el equipo
```

Y ahora se construye una aplicación de tablero que llamará a estás tablas y mostrará los resultados. Para crearla, se va a seguir el guión mostrado [aquí](https://bookdown.org/eljorgehdz/shiny_tuto/shiny_tuto.html)

```r
library(shiny)
library(shinydashboard)
library(mapview)
library(leaflet)
library(sf)

Pinar.Yunquera<-st_read("C:/DESCARGA/Pinar.Yunquera.shp")

pinsapo<-st_read("C:/DESCARGA/pinsapo.shp")

resumen.pinsapo<-st_read("C:/DESCARGA/resumen.pinsapo.shp")

ui <- dashboardPage(
  dashboardHeader(title = "Análisis de datos del IFN en Pinar de Yunquera",
                  titleWidth =500),
  dashboardSidebar(disable = TRUE),
  dashboardBody(
    #Gráficos del dashboard
    box(title = "Distribución de crecimiento en DN",
        solidHeader = TRUE,status = "primary",
        leafletOutput("mapa", height = 380)),
    box(title = "Distribución de crecimiento en H",
        solidHeader = TRUE,status = "primary",
        leafletOutput("mapa2", height = 380)),
    box(title = "Distribución de densidades",
        solidHeader = TRUE,status = "primary",
        leafletOutput("mapa3", height = 380)),
    box(title = "Relaciones alométricas DN/H según densidad",
        solidHeader = TRUE,status = "primary",
        plotOutput("plot1", height = 380)),
  )
)

server <- function(input, output) {

  output$mapa <- renderLeaflet({
    a<-mapview(Pinar.Yunquera)+mapview(resumen.pinsapo,zcol = "Dif_DN")
    a@map
    
  })
  output$mapa2 <- renderLeaflet({
    b<-mapview(Pinar.Yunquera)+mapview(resumen.pinsapo,zcol = "Dif_H")
    b@map
    
  })
  output$mapa3 <- renderLeaflet({
    c<-mapview(Pinar.Yunquera)+mapview(resumen.pinsapo,zcol = "Nps_prc")
    c@map
    
  })
  output$plot1 <- renderPlot({
    plot(0,0,col = "white",
         xlim=c(80,900),ylim=c(0,21),main="Crecimientos.Pinsapo",
         xlab="dap (mm)", ylab="altura (m)")
    segments(pinsapo$DN_IFN3[which(pinsapo$Dif_DN<20)],
             pinsapo$HT[which(pinsapo$Dif_DN<20)],
             pinsapo$DN_IF2[which(pinsapo$Dif_DN<20)],
             pinsapo$HT_IFN2[which(pinsapo$Dif_DN<20)],
             col="orange")
    segments(pinsapo$DN_IFN3[which(pinsapo$Dif_DN>=20&pinsapo$Dif_DN<50)],
             pinsapo$HT[which(pinsapo$Dif_DN>=20&pinsapo$Dif_DN<50)],
             pinsapo$DN_IF2[which(pinsapo$Dif_DN>=20&pinsapo$Dif_DN<50)],
             pinsapo$HT_IFN2[which(pinsapo$Dif_DN>=20&pinsapo$Dif_DN<50)],
             col="red")
    segments(pinsapo$DN_IFN3[which(pinsapo$Dif_DN>=50)],
             pinsapo$HT[which(pinsapo$Dif_DN>=50)],
             pinsapo$DN_IF2[which(pinsapo$Dif_DN>=50)],
             pinsapo$HT_IFN2[which(pinsapo$Dif_DN>=50)],
             col="brown")
    legend("bottomright",legend=c("Crecimiento DN < 20 mm","20 mm < Crecimiento DN < 50 mm",
                                  "Crecimiento DN > 50 mm"),
           col=c("orange","red","brown"),lty=c(1,1,1),cex=0.75,
           box.lty=0)
  })
}



# Run the application 
shinyApp(ui = ui, server = server)
```

Que dará como resultado el siguiente tablero:

![](./Auxiliares/Dashboard.png)

