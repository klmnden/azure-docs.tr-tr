---
title: ScaleR ve SparkR Azure HDInsight ile kullanma
description: ScaleR ve SparkR HDInsight üzerinde ML Hizmetleri ile kullanma
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/19/2017
ms.openlocfilehash: 78ea29b9b37c55a588a44f8d4b69486b89ee2fee
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63761353"
---
# <a name="combine-scaler-and-sparkr-in-hdinsight"></a>ScaleR ve SparkR HDInsight içinde birleştirin

Bu belge, uçuş varış gecikmeler kullanarak tahmin etmek gösterilmiştir bir **ScaleR** Lojistik regresyon modeli. Uçuş gecikme ve hava durumu verilerini kullanılarak, örnekte **SparkR**.

Apache Hadoop'ın Spark yürütme altyapısının iki paketi de çalıştırmasına rağmen bellek içi verileri kendi ilgili Spark oturumlarında gereksinim duydukları her gibi paylaşım engellenir. ML Server gelecek bir sürümünde bu sorun giderilinceye kadar çakışmayan Spark oturumlar kullanan ve Ara dosyaları veri değişimi için çözüm olabilir. Buradaki yönergeleri Bu gereksinimleri elde etmek basit olduğunu gösterir.

Bu örnek başlangıçta bir konuşması konumundaki Strata 2016 içinde Roni Burd Mario Inchiosa ile paylaşıldı. Bu konuşmada, bulabilirsiniz [R ile ölçeklenebilir bir veri bilimi platformu oluşturmayı](https://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio).

Kodu, azure'da bir HDInsight kümesinde Spark üzerinde çalıştırılan ML Server için ilk olarak yazılmıştır. Ancak, bir komut dosyası kullanımda ScaleR ve SparkR karıştırma kavramı da şirket içi ortamlara bağlamında geçerli.

Bu belgedeki adımlarda bir ara R ve bu bilgi düzeyi sahip olduğunuzu varsaymaktadır [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) ML Server kitaplığı. İçin sunulan [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) bu senaryoyu adım anlatarak oluştu.

## <a name="the-airline-and-weather-datasets"></a>Uçak ve hava durumu veri kümeleri

Uçuş verileri kullanılabilir [ABD devlet kurumları arşivlerinizin](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236). Bir zip olarak kullanılabilir [AirOnTimeCSV.zip](https://packages.revolutionanalytics.com/datasets/AirOnTime87to12/AirOnTimeCSV.zip).

Hava Durumu verilerini ham biçimiyle, zip dosyaları olarak aya göre indirilebileceğini [Ulusal Okyanus ve Atmosfer Yönetimi depo](https://www.ncdc.noaa.gov/orders/qclcd/). Bu örnekte, Mayıs 2007: Kasım 2012 verileri indirin. Saatlik veri dosyalarını kullanmak ve `YYYYMMMstation.txt` her zıps dosya. 

## <a name="setting-up-the-spark-environment"></a>Spark ortamını ayarlama

Spark ortamı ayarlamak için aşağıdaki kodu kullanın:

```
workDir        <- '~'  
myNameNode     <- 'default' 
myPort         <- 0
inputDataDir   <- 'wasb://hdfs@myAzureAccount.blob.core.windows.net'
hdfsFS         <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

# create a persistent Spark session to reduce startup times 
#   (remember to stop it later!)
 
sparkCC        <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort, persistentRun=TRUE)

# create working directories 

rxHadoopMakeDir('/user')
rxHadoopMakeDir('user/RevoShare')
rxHadoopMakeDir('user/RevoShare/remoteuser')

(dataDir <- '/share')
rxHadoopMakeDir(dataDir)
rxHadoopListFiles(dataDir) 

setwd(workDir)
getwd()

# version of rxRoc that runs in a local CC 
rxRoc <- function(...){
  rxSetComputeContext(RxLocalSeq())
  roc <- RevoScaleR::rxRoc(...)
  rxSetComputeContext(sparkCC)
  return(roc)
}

logmsg <- function(msg) { cat(format(Sys.time(), "%Y-%m-%d %H:%M:%S"),':',msg,'\n') } 
t0 <- proc.time() 

#..start 

logmsg('Start') 
(trackers <- system("mapred job -list-active-trackers", intern = TRUE))
logmsg(paste('Number of task nodes=',length(trackers)))
```

Ardından, ekleme `Spark_Home` R Paketlerine yönelik arama yolu. Arama yoluna ekleme SparkR kullanın ve SparkR oturumu başlatmak sağlar:

```
#..setup for use of SparkR  

logmsg('Initialize SparkR') 

.libPaths(c(file.path(Sys.getenv("SPARK_HOME"), "R", "lib"), .libPaths()))
library(SparkR)

sparkEnvir <- list(spark.executor.instances = '10',
                   spark.yarn.executor.memoryOverhead = '8000')

sc <- sparkR.init(
  sparkEnvir = sparkEnvir,
  sparkPackages = "com.databricks:spark-csv_2.10:1.3.0"
)

sqlContext <- sparkRSQL.init(sc)
```

## <a name="preparing-the-weather-data"></a>Hava durumu verileri hazırlama

Hava Durumu verilerini hazırlamak için alt sütunları için modelleme için gerekli: 

- "Görünürlüğünü"
- "DryBulbCelsius"
- "DewPointCelsius"
- "RelativeHumidity"
- "WindSpeed"
- "Altimeter"

Ardından hava durumu istasyonu ile ilişkili bir havaalanı kodu ekleyin ve ölçümler yerel saatten UTC'ye dönüştürün.

Hava durumu istasyonu (WBAN) bilgileri bir havaalanı kodu eşlemek için bir dosya oluşturarak başlayın. Aşağıdaki kod, biz gerekir, hava durumu istasyonu eşleme dosyası birleştirir, ölçümlerin UTC tarih saatleri ayarlar ve sonra dosyayı yeni bir sürümü Yazar sütunlara alt kümelerini saatlik ham hava durumu verileri dosyaların her biri okur:

```
# Look up AirportID and Timezone for WBAN (weather station ID) and adjust time

adjustTime <- function(dataList)
{
  dataset0 <- as.data.frame(dataList)
  
  dataset1 <- base::merge(dataset0, wbanToAirIDAndTZDF1, by = "WBAN")

  if(nrow(dataset1) == 0) {
    dataset1 <- data.frame(
      Visibility = numeric(0),
      DryBulbCelsius = numeric(0),
      DewPointCelsius = numeric(0),
      RelativeHumidity = numeric(0),
      WindSpeed = numeric(0),
      Altimeter = numeric(0),
      AdjustedYear = numeric(0),
      AdjustedMonth = numeric(0),
      AdjustedDay = integer(0),
      AdjustedHour = integer(0),
      AirportID = integer(0)
    )
    
    return(dataset1)
  }
  
  Year <- as.integer(substr(dataset1$Date, 1, 4))
  Month <- as.integer(substr(dataset1$Date, 5, 6))
  Day <- as.integer(substr(dataset1$Date, 7, 8))
  
  Time <- dataset1$Time
  Hour <- ceiling(Time/100)
  
  Timezone <- as.integer(dataset1$TimeZone)
  
  adjustdate = as.POSIXlt(sprintf("%4d-%02d-%02d %02d:00:00", Year, Month, Day, Hour), tz = "UTC") + Timezone * 3600

  AdjustedYear = as.POSIXlt(adjustdate)$year + 1900
  AdjustedMonth = as.POSIXlt(adjustdate)$mon + 1
  AdjustedDay   = as.POSIXlt(adjustdate)$mday
  AdjustedHour  = as.POSIXlt(adjustdate)$hour
  
  AirportID = dataset1$AirportID
  Weather = dataset1[,c("Visibility", "DryBulbCelsius", "DewPointCelsius", "RelativeHumidity", "WindSpeed", "Altimeter")]
  
  data.set = data.frame(cbind(AdjustedYear, AdjustedMonth, AdjustedDay, AdjustedHour, AirportID, Weather))
  
  return(data.set)
}

wbanToAirIDAndTZDF <- read.csv("wban-to-airport-id-tz.csv")

colInfo <- list(
  WBAN = list(type="integer"),
  Date = list(type="character"),
  Time = list(type="integer"),
  Visibility = list(type="numeric"),
  DryBulbCelsius = list(type="numeric"),
  DewPointCelsius = list(type="numeric"),
  RelativeHumidity = list(type="numeric"),
  WindSpeed = list(type="numeric"),
  Altimeter = list(type="numeric")
)

weatherDF <- RxTextData(file.path(inputDataDir, "WeatherRaw"), colInfo = colInfo)

weatherDF1 <- RxTextData(file.path(inputDataDir, "Weather"), colInfo = colInfo,
                filesystem=hdfsFS)

rxSetComputeContext("localpar")
rxDataStep(weatherDF, outFile = weatherDF1, rowsPerRead = 50000, overwrite = T,
           transformFunc = adjustTime,
           transformObjects = list(wbanToAirIDAndTZDF1 = wbanToAirIDAndTZDF))
```

## <a name="importing-the-airline-and-weather-data-to-spark-dataframes"></a>Uçak ve hava durumu verileri için Spark veri çerçevelerini içeri aktarma

SparkR kullandığımız artık [read.df()](https://docs.databricks.com/spark/1.6/sparkr/functions/read.df.html#read-df) hava durumu ve Havayolu verileri için Spark veri çerçevelerini içeri aktarmak için işlevi. Diğer birçok Spark yöntemler gibi bu işlev yürütülür gevşek, bunların yürütme için sıraya alınmış ancak gerekli kadar yürütülmedi, anlamına gelir.

```
airPath     <- file.path(inputDataDir, "AirOnTime08to12CSV")
weatherPath <- file.path(inputDataDir, "Weather") # pre-processed weather data
rxHadoopListFiles(airPath) 
rxHadoopListFiles(weatherPath) 

# create a SparkR DataFrame for the airline data

logmsg('create a SparkR DataFrame for the airline data') 
# use inferSchema = "false" for more robust parsing
airDF <- read.df(sqlContext, airPath, source = "com.databricks.spark.csv", 
                 header = "true", inferSchema = "false")

# Create a SparkR DataFrame for the weather data

logmsg('create a SparkR DataFrame for the weather data') 
weatherDF <- read.df(sqlContext, weatherPath, source = "com.databricks.spark.csv", 
                     header = "true", inferSchema = "true")
```

## <a name="data-cleansing-and-transformation"></a>Veri temizleme ve dönüştürme

Sonraki içe Havayolu veriler üzerinde biraz temizlik yaptığımız sütun yeniden adlandırılamadı. Biz yalnızca gereken değişkenleri tutmak ve en son hava durumu verileri ayrılma sırasında birleştirmeyi etkinleştirmek için en yakın saat zamanlanmış kalkış saatler yuvarlar:

```
logmsg('clean the airline data') 
airDF <- rename(airDF,
                ArrDel15 = airDF$ARR_DEL15,
                Year = airDF$YEAR,
                Month = airDF$MONTH,
                DayofMonth = airDF$DAY_OF_MONTH,
                DayOfWeek = airDF$DAY_OF_WEEK,
                Carrier = airDF$UNIQUE_CARRIER,
                OriginAirportID = airDF$ORIGIN_AIRPORT_ID,
                DestAirportID = airDF$DEST_AIRPORT_ID,
                CRSDepTime = airDF$CRS_DEP_TIME,
                CRSArrTime =  airDF$CRS_ARR_TIME
)

# Select desired columns from the flight data. 
varsToKeep <- c("ArrDel15", "Year", "Month", "DayofMonth", "DayOfWeek", "Carrier", "OriginAirportID", "DestAirportID", "CRSDepTime", "CRSArrTime")
airDF <- select(airDF, varsToKeep)

# Apply schema
coltypes(airDF) <- c("character", "integer", "integer", "integer", "integer", "character", "integer", "integer", "integer", "integer")

# Round down scheduled departure time to full hour.
airDF$CRSDepTime <- floor(airDF$CRSDepTime / 100)
```

Artık hava durumu verilerini benzer işlemleri gerçekleştirin:

```
# Average weather readings by hour
logmsg('clean the weather data') 
weatherDF <- agg(groupBy(weatherDF, "AdjustedYear", "AdjustedMonth", "AdjustedDay", "AdjustedHour", "AirportID"), Visibility="avg",
                  DryBulbCelsius="avg", DewPointCelsius="avg", RelativeHumidity="avg", WindSpeed="avg", Altimeter="avg"
                  )

weatherDF <- rename(weatherDF,
                    Visibility = weatherDF$'avg(Visibility)',
                    DryBulbCelsius = weatherDF$'avg(DryBulbCelsius)',
                    DewPointCelsius = weatherDF$'avg(DewPointCelsius)',
                    RelativeHumidity = weatherDF$'avg(RelativeHumidity)',
                    WindSpeed = weatherDF$'avg(WindSpeed)',
                    Altimeter = weatherDF$'avg(Altimeter)'
)
```

## <a name="joining-the-weather-and-airline-data"></a>Hava durumu ve Havayolu verileri birleştirme

Artık SparkR kullandığımız [join()](https://docs.databricks.com/spark/1.6/sparkr/functions/join.html#join) işlevini bir sol dış birleşim kalkış AirportID Havayolu ve hava durumu verileri ve datetime. Dış birleşim eşleşen hava durumu verileri olsa bile tüm Havayolu veri kayıtlarının tutulacağı sağlıyor. Birleştirme aşağıdaki biz bazı gereksiz sütunları kaldırma ve birleşimi tarafından sunulan gelen DataFrame ön ekini kaldırmak için tutulan sütunları yeniden adlandırın.

```
logmsg('Join airline data with weather at Origin Airport')
joinedDF <- SparkR::join(
  airDF,
  weatherDF,
  airDF$OriginAirportID == weatherDF$AirportID &
    airDF$Year == weatherDF$AdjustedYear &
    airDF$Month == weatherDF$AdjustedMonth &
    airDF$DayofMonth == weatherDF$AdjustedDay &
    airDF$CRSDepTime == weatherDF$AdjustedHour,
  joinType = "left_outer"
)

# Remove redundant columns
vars <- names(joinedDF)
varsToDrop <- c('AdjustedYear', 'AdjustedMonth', 'AdjustedDay', 'AdjustedHour', 'AirportID')
varsToKeep <- vars[!(vars %in% varsToDrop)]
joinedDF1 <- select(joinedDF, varsToKeep)

joinedDF2 <- rename(joinedDF1,
                    VisibilityOrigin = joinedDF1$Visibility,
                    DryBulbCelsiusOrigin = joinedDF1$DryBulbCelsius,
                    DewPointCelsiusOrigin = joinedDF1$DewPointCelsius,
                    RelativeHumidityOrigin = joinedDF1$RelativeHumidity,
                    WindSpeedOrigin = joinedDF1$WindSpeed,
                    AltimeterOrigin = joinedDF1$Altimeter
)
```

Benzer şekilde, biz varış AirportID ve datetime göre hava durumu ve Havayolu veri Katıl:

```
logmsg('Join airline data with weather at Destination Airport')
joinedDF3 <- SparkR::join(
  joinedDF2,
  weatherDF,
  airDF$DestAirportID == weatherDF$AirportID &
    airDF$Year == weatherDF$AdjustedYear &
    airDF$Month == weatherDF$AdjustedMonth &
    airDF$DayofMonth == weatherDF$AdjustedDay &
    airDF$CRSDepTime == weatherDF$AdjustedHour,
  joinType = "left_outer"
)

# Remove redundant columns
vars <- names(joinedDF3)
varsToDrop <- c('AdjustedYear', 'AdjustedMonth', 'AdjustedDay', 'AdjustedHour', 'AirportID')
varsToKeep <- vars[!(vars %in% varsToDrop)]
joinedDF4 <- select(joinedDF3, varsToKeep)

joinedDF5 <- rename(joinedDF4,
                    VisibilityDest = joinedDF4$Visibility,
                    DryBulbCelsiusDest = joinedDF4$DryBulbCelsius,
                    DewPointCelsiusDest = joinedDF4$DewPointCelsius,
                    RelativeHumidityDest = joinedDF4$RelativeHumidity,
                    WindSpeedDest = joinedDF4$WindSpeed,
                    AltimeterDest = joinedDF4$Altimeter
                    )
```

## <a name="save-results-to-csv-for-exchange-with-scaler"></a>ScaleR exchange'le için CSV'ye Sonuçları Kaydet

Birleştirmeler ile SparkR yapmanız gereken tamamlanan. Biz, verileri son Spark DataFrame girişi için bir CSV için "joinedDF5" için ScaleR kaydetmek ve SparkR oturumu kapatın. Biz, ScaleR işleme yeterli paralellik sağlamak için 80 ayrı bölümlerde sonuç CSV kaydetmek için SparkR açıkça bildirin:

```
logmsg('output the joined data from Spark to CSV') 
joinedDF5 <- repartition(joinedDF5, 80) # write.df below will produce this many CSVs

# write result to directory of CSVs
write.df(joinedDF5, file.path(dataDir, "joined5Csv"), "com.databricks.spark.csv", "overwrite", header = "true")

# We can shut down the SparkR Spark context now
sparkR.stop()

# remove non-data files
rxHadoopRemove(file.path(dataDir, "joined5Csv/_SUCCESS"))
```

## <a name="import-to-xdf-for-use-by-scaler"></a>ScaleR tarafından kullanılmak XDF alma

CSV dosyası birleştirilmiş Havayolu ve hava durumu verileri olarak kullanabiliriz-modelleme ScaleR metin veri kaynağı aracılığıyla içindir. Ancak, veri kümesindeki birden çok işlemleri çalıştırırken daha etkili olduğundan biz XDF için ilk olarak, içe aktarın:

```
logmsg('Import the CSV to compressed, binary XDF format') 

# set the Spark compute context for ML Services 
rxSetComputeContext(sparkCC)
rxGetComputeContext()

colInfo <- list(
  ArrDel15 = list(type="numeric"),
  Year = list(type="factor"),
  Month = list(type="factor"),
  DayofMonth = list(type="factor"),
  DayOfWeek = list(type="factor"),
  Carrier = list(type="factor"),
  OriginAirportID = list(type="factor"),
  DestAirportID = list(type="factor"),
  RelativeHumidityOrigin = list(type="numeric"),
  AltimeterOrigin = list(type="numeric"),
  DryBulbCelsiusOrigin = list(type="numeric"),
  WindSpeedOrigin = list(type="numeric"),
  VisibilityOrigin = list(type="numeric"),
  DewPointCelsiusOrigin = list(type="numeric"),
  RelativeHumidityDest = list(type="numeric"),
  AltimeterDest = list(type="numeric"),
  DryBulbCelsiusDest = list(type="numeric"),
  WindSpeedDest = list(type="numeric"),
  VisibilityDest = list(type="numeric"),
  DewPointCelsiusDest = list(type="numeric")
)

joinedDF5Txt <- RxTextData(file.path(dataDir, "joined5Csv"),
                           colInfo = colInfo, fileSystem = hdfsFS)
rxGetInfo(joinedDF5Txt) 

destData <- RxXdfData(file.path(dataDir, "joined5XDF"), fileSystem = hdfsFS)

rxImport(inData = joinedDF5Txt, destData, overwrite = TRUE)

rxGetInfo(destData, getVarInfo = T)

# File name: /user/RevoShare/dev/delayDataLarge/joined5XDF 
# Number of composite data files: 80 
# Number of observations: 148619655 
# Number of variables: 22 
# Number of blocks: 320 
# Compression type: zlib 
# Variable information: 
#   Var 1: ArrDel15, Type: numeric, Low/High: (0.0000, 1.0000)
# Var 2: Year
# 26 factor levels: 1987 1988 1989 1990 1991 ... 2008 2009 2010 2011 2012
# Var 3: Month
# 12 factor levels: 10 11 12 1 2 ... 5 6 7 8 9
# Var 4: DayofMonth
# 31 factor levels: 1 3 4 5 7 ... 29 30 2 18 31
# Var 5: DayOfWeek
# 7 factor levels: 4 6 7 1 3 2 5
# Var 6: Carrier
# 30 factor levels: PI UA US AA DL ... HA F9 YV 9E VX
# Var 7: OriginAirportID
# 374 factor levels: 15249 12264 11042 15412 13930 ... 13341 10559 14314 11711 10558
# Var 8: DestAirportID
# 378 factor levels: 13303 14492 10721 11057 13198 ... 14802 11711 11931 12899 10559
# Var 9: CRSDepTime, Type: integer, Low/High: (0, 24)
# Var 10: CRSArrTime, Type: integer, Low/High: (0, 2400)
# Var 11: RelativeHumidityOrigin, Type: numeric, Low/High: (0.0000, 100.0000)
# Var 12: AltimeterOrigin, Type: numeric, Low/High: (28.1700, 31.1600)
# Var 13: DryBulbCelsiusOrigin, Type: numeric, Low/High: (-46.1000, 47.8000)
# Var 14: WindSpeedOrigin, Type: numeric, Low/High: (0.0000, 81.0000)
# Var 15: VisibilityOrigin, Type: numeric, Low/High: (0.0000, 90.0000)
# Var 16: DewPointCelsiusOrigin, Type: numeric, Low/High: (-41.7000, 29.0000)
# Var 17: RelativeHumidityDest, Type: numeric, Low/High: (0.0000, 100.0000)
# Var 18: AltimeterDest, Type: numeric, Low/High: (28.1700, 31.1600)
# Var 19: DryBulbCelsiusDest, Type: numeric, Low/High: (-46.1000, 53.9000)
# Var 20: WindSpeedDest, Type: numeric, Low/High: (0.0000, 136.0000)
# Var 21: VisibilityDest, Type: numeric, Low/High: (0.0000, 88.0000)
# Var 22: DewPointCelsiusDest, Type: numeric, Low/High: (-43.0000, 29.0000)

finalData <- RxXdfData(file.path(dataDir, "joined5XDF"), fileSystem = hdfsFS)

```

## <a name="splitting-data-for-training-and-test"></a>Verileri eğitim ve test için bölme

RxDataStep test 2012 veri çıkış bölme ve eğitim için rest tutmak için kullanırız:

```
# split out the training data

logmsg('split out training data as all data except year 2012')
trainDS <- RxXdfData( file.path(dataDir, "finalDataTrain" ),fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = trainDS,
            rowSelection = ( Year != 2012 ), overwrite = T )

# split out the testing data

logmsg('split out the test data for year 2012') 
testDS <- RxXdfData( file.path(dataDir, "finalDataTest" ), fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = testDS,
            rowSelection = ( Year == 2012 ), overwrite = T )

rxGetInfo(trainDS)
rxGetInfo(testDS)
```

## <a name="train-and-test-a-logistic-regression-model"></a>Eğitme ve bir Lojistik regresyon modeli test etme

Şimdi bir model oluşturmak üzere hazırız. Hava durumu verileri etki gecikmesine geliş saati görmek için ScaleR'ın Lojistik regresyon yordamı kullanırız. Biz bunu 15 dakikadan daha büyük bir alma gecikme kalkış ve varış havaalanları, hava durumu etkilediği olup model oluşturmak için kullanın:

```
logmsg('train a logistic regression model for Arrival Delay > 15 minutes') 
formula <- as.formula(ArrDel15 ~ Year + Month + DayofMonth + DayOfWeek + Carrier +
                     OriginAirportID + DestAirportID + CRSDepTime + CRSArrTime + 
                     RelativeHumidityOrigin + AltimeterOrigin + DryBulbCelsiusOrigin +
                     WindSpeedOrigin + VisibilityOrigin + DewPointCelsiusOrigin + 
                     RelativeHumidityDest + AltimeterDest + DryBulbCelsiusDest +
                     WindSpeedDest + VisibilityDest + DewPointCelsiusDest
                   )

# Use the scalable rxLogit() function but set max iterations to 3 for the purposes of 
# this exercise 

logitModel <- rxLogit(formula, data = trainDS, maxIterations = 3)

base::summary(logitModel)
```

Şimdi nasıl bazı Öngörüler yapma ve ROC ve AUC tarafından test verileri üzerindeki yaptığını görelim.

```
# Predict over test data (Logistic Regression).

logmsg('predict over the test data') 
logitPredict <- RxXdfData(file.path(dataDir, "logitPredict"), fileSystem = hdfsFS)

# Use the scalable rxPredict() function

rxPredict(logitModel, data = testDS, outData = logitPredict,
          extraVarsToWrite = c("ArrDel15"), 
          type = 'response', overwrite = TRUE)

# Calculate ROC and Area Under the Curve (AUC).

logmsg('calculate the roc and auc') 
logitRoc <- rxRoc("ArrDel15", "ArrDel15_Pred", logitPredict)
logitAuc <- rxAuc(logitRoc)
head(logitAuc)
logitAuc

plot(logitRoc)
```

## <a name="scoring-elsewhere"></a>Başka bir yerde Puanlama

Ayrıca model Puanlama verileri başka bir platform için kullanabiliriz. Bir RDS dosyasına kaydediliyor aktarmak ve puanlama ortama MIcrosoft SQL Server R Services gibi bir hedef, RDS alma tarafından. Faktörü düzeyleri puanlanması veri modeli üzerinde oluşturulmuş eşleştiğinden emin olmak önemlidir. Eşleşen ayıklayarak ve modelleme verileri ScaleR'ın aracılığıyla ilişkili sütun bilgileri kaydedilirken ulaşılabilecek `rxCreateColInfo()` işlevi ve tahmin için giriş veri kaynağı, sütun bilgileri uygulamak. Aşağıdaki biz birkaç test veri satırlarını kaydetmek ve ayıklayın ve tahmin betikte bu örnekten sütun bilgileri kullanın:

```
# save the model and a sample of the test dataset 

logmsg('save serialized version of the model and a sample of the test data')
rxSetComputeContext('localpar') 
saveRDS(logitModel, file = "logitModel.rds")
testDF <- head(testDS, 1000)  
saveRDS(testDF    , file = "testDF.rds"    )
list.files()

rxHadoopListFiles(file.path(inputDataDir,''))
rxHadoopListFiles(dataDir)

# stop the spark engine 
rxStopEngine(sparkCC) 

logmsg('Done.')
elapsed <- (proc.time() - t0)[3]
logmsg(paste('Elapsed time=',sprintf('%6.2f',elapsed),'(sec)\n\n'))
```

## <a name="summary"></a>Özet

Bu makalede, biz nasıl SparkR kullanımı için Hadoop Spark model geliştirme için veri işleme ScaleR ile birleştirmek mümkündür gösterilen. Bu senaryo, yalnızca bir oturum aynı anda çalışan ayrı Spark oturumları, koruyun ve CSV dosyaları aracılığıyla veri değişimi gerektirir. SparkR ve ScaleR bir Spark oturumuna paylaşın ve bu nedenle Spark veri çerçevelerini paylaşın basit olsa da, bu işlem yaklaşan bir ML Hizmetleri sürümde daha da kolay olmalıdır.

## <a name="next-steps-and-more-information"></a>Sonraki adımlar ve daha fazla bilgi

- Apache Spark ML sunucuda kullanımı hakkında daha fazla bilgi için bkz. [Başlarken kılavuzunda](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started).

- ML Server hakkında genel bilgi için bkz. [R ile çalışmaya başlama](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) makalesi.

- HDInsight üzerinde ML hizmetleri hakkında daha fazla bilgi için bkz: [HDInsight üzerinde ML hizmetleri genel bakış](r-server/r-server-overview.md) ve [ML Hizmetleri üzerindeki Azure HDInsight ile çalışmaya başlama](r-server/r-server-get-started.md).

SparkR kullanımı hakkında daha fazla bilgi için bkz:

- [Apache SparkR belge](https://spark.apache.org/docs/2.1.0/sparkr.html).

- [SparkR genel bakış](https://docs.databricks.com/spark/latest/sparkr/overview.html) Databricks öğesinden.
