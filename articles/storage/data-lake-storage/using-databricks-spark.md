---
title: Spark kullanarak DataBricks ile Azure Data Lake Storage Gen2 Önizleme verilerine erişme | Microsoft Docs
description: Azure Data Lake Storage Gen2 depolama hesabındaki verilere erişmek için bir DataBricks kümesinde Spark sorgularını çalıştırmayı öğrenin.
services: hdinsight,storage
tags: azure-portal
author: dineshm
manager: twooley
ms.component: data-lake-storage-gen2
ms.service: hdinsight
ms.workload: big-data
ms.topic: tutorial
ms.date: 6/27/2018
ms.author: dineshm
ms.openlocfilehash: 013369c84ca7f2ec232f542549c22260eca46980
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062543"
---
# <a name="tutorial-access-azure-data-lake-storage-gen2-preview-data-with-databricks-using-spark"></a>Öğretici: Spark kullanarak DataBricks ile Azure Data Lake Storage Gen2 Önizleme verilerine erişme

Bu öğreticide Azure Data Lake Storage Gen2 Önizleme ile uyumlu bir hesaptaki verileri sorgulamak için bir DataBricks kümesinde Spark sorguları çalıştırmayı öğreneceksiniz.

> [!div class="checklist"]
> * DataBricks kümesi oluşturma
> * Yapılandırılmamış verileri bir depolama hesabına alma
> * Verileri işlemek için bir Azure İşlevini tetikleme
> * Blob depolama alanındaki verilerinizde analiz çalıştırma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide [Amerika Birleşik Devletleri Ulaştırma Bakanlığı](https://transtats.bts.gov/Tables.asp?DB_ID=120&DB_Name=Airline%20On-Time%20Performance%20Data&DB_Short_Name=On-Time) tarafından sunulan hava yolları uçuş verilerini kullanma ve sorgulama adımları gösterilmektedir. En az iki yıllık hava yolları verisini (tüm alanları seçerek) indirin ve sonucu bilgisayarınıza kaydedin. Sonraki adımlarda ihtiyacınız olacağından dosya adını ve indirdiğiniz yolu not almayı unutmayın.

> [!NOTE]
> Tüm veri alanlarını seçmek için **Prezipped file** (Önceden sıkıştırılmış dosya) onay kutusuna tıklayın. İndirilen veriler gigabayt boyutunda olabilir ancak analiz için bu boyutta veri olması şarttır.

## <a name="create-an-azure-data-lake-storage-gen2-account"></a>Azure Data Lake Storage Gen2 hesabı oluşturma

Başlamak için yeni bir [Azure Data Lake Storage Gen2 hesabı](quickstart-create-account.md) oluşturun ve benzersiz bir ad verin. Ardından yapılandırma ayarlarını almak için depolama hesabına gidin.

> [!IMPORTANT]
> Önizleme sürümünde Azure İşlevleri yalnızca düz ad alanıyla oluşturulan Azure Data Lake Storage Gen2 hesaplarıyla çalışır.

1. **Settings** (Ayarlar) altında **Access keys** (Erişim anahtarları) öğesine tıklayın.
3. Anahtar değerini kopyalamak için **key1**'in yanındaki **Copy** (Kopyala) düğmesine tıklayın.

Bu öğreticinin sonraki adımlarında kullanmanız gerekeceğinden hesap adını ve anahtarı not alın. Bir metin düzenleyici açıp hesap adını ve anahtarı daha sonra kullanmak üzere kaydedin.

## <a name="create-a-databricks-cluster"></a>DataBricks kümesi oluşturma

Bir sonraki adım, veri çalışma alanı oluşturmak için bir [DataBricks kümesi](https://docs.azuredatabricks.net/) oluşturmaktır.

1. Bir [DataBricks hizmeti](https://ms.portal.azure.com/#create/Microsoft.Databricks) oluşturun ve **myFlightDataService** olarak adlandırın (hizmeti oluştururken *Panoya sabitle* onay kutusunu işaretlemeyi unutmayın).
2. Çalışma alanını yeni bir tarayıcı penceresinde açmak için **Launch Workspace** (Çalışma Alanın Başlat) öğesine tıklayın.
3. Sol gezinti çubuğunda **Clusters** (Kümeler) öğesine tıklayın.
4. **Create Cluster** (Küme Oluştur) öğesine tıklayın.
5. *Cluster name* (Küme adı) alanına **myFlightDataCluster** yazın.
6. *Worker Type* (Çalışan Türü) alanında **Standard_D8s_v3** seçeneğini belirleyin.
7. **Min Workers** (Minimum Çalışan) değerini *4* olarak değiştirin.
8. Sayfanın en üstündeki **Create Cluster** (Küme Oluştur) öğesine tıklayın (bu işlemin tamamlanması 5 dakikaya kadar sürebilir).
9. İşlem tamamlandıktan sonra gezinti çubuğunun sol üst kısmından **Azure Databricks**'i seçin.
10. Sayfanın alt yarısındaki **New** (Yeni) bölümünden **Notebook** (Not Defteri) öğesini seçin.
11. **Name** (Ad) alanına istediğiniz adı girin.
12. Diğer tüm alanlar varsayılan değerlerde bırakılabilir.
13. **Oluştur**’u seçin.
14. Aşağıdaki kodu **Cmd 1** hücresine yapıştırın, değerleri depolama hesabınızdaki değerlerle değiştirin.

    ```bash
    spark.conf.set("fs.azure.account.key.<account_name>.dfs.core.windows.net", "<account_key>") 
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
    dbutils.fs.ls("abfs://<file_system>@<account_name>.dfs.core.windows.net/")
    spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")
    ```

## <a name="ingest-data"></a>Veriyi çekme

### <a name="copy-source-data-into-the-storage-account"></a>Kaynak verilerini depolama hesabına kopyalama

Bir sonraki görev, *.csv* dosyasındaki verileri Azure depolama alanına kopyalamak için AzCopy komutunu kullanmaktır. Bir komut istemi penceresi açın ve aşağıdaki komutları girin. `<DOWNLOAD_FILE_PATH>`, `<ACCOUNT_NAME>` ve `<ACCOUNT_KEY>` yer tutucularının yerine bir önceki adımda kaydettiğiniz değerleri girmeyi unutmayın.

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_FILE_PATH>" https://<ACCOUNT_NAME>.dfs.core.windows.net/dbricks/folder1/On_Time --recursive 
```

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>DataBricks Not Defteri'ni kullanarak CSV'yi Parquet biçimine dönüştürme

DataBricks'i tarayıcınızda tekrar açın ve aşağıdaki adımları uygulayın:

1. Gezinti çubuğunun sol üst kısmından **Azure Databricks**'i seçin.
2. Sayfanın alt yarısındaki **New** (Yeni) bölümünden **Notebook** (Not Defteri) öğesini seçin.
3. **Name** (Ad) alanına **CSV2Parquet** yazın.
4. Diğer tüm alanlar varsayılan değerlerde bırakılabilir.
5. **Oluştur**’u seçin.
6. Aşağıdaki kodu **Cmd 1** hücresine yapıştırın (bu kod düzenleyicide otomatik olarak kaydedilir).

    ```
    #mount Azure Blob Storage as an HDFS file system to your databricks cluster
    #you need to specify a storage account and container to connect to. 
    #use a SAS token or an account key to connect to Blob Storage.  
    accountname = "<insert account name>' 
    accountkey = " <insert account key>'
    fullname = "fs.azure.account.key." +accountname+ ".blob.core.windows.net"
    accountsource = "abfs://dbricks@" +accountname+ ".blob.core.windows.net/folder1"
    #create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load(accountsource + "/On_Time_On_Time*.csv")
    #read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet(accountsource + '/parquet/flights')

    #read the flight details parquet file 
    #flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load(accountsource + "/parquet/flights")
    print("Done")
    ```

## <a name="explore-data-using-hadoop-distributed-file-system"></a>Hadoop Dağıtılmış Dosya Sistemi kullanarak verileri keşfetme

DataBricks çalışma alanına dönün ve sol gezinti çubuğundaki **Recent** (Son Öğeler) simgesine tıklayın.

1. **Flight Data Analytics** not defterine tıklayın.
2. Yeni hücre oluşturmak için **Ctrl + Alt + N** tuşlarına basın.

Aşağıdaki kod bloklarını **Cmd 1** bölümüne girin ve **Cmd + Enter** tuşlarına basarak Python betiğini çalıştırın.

AzCopy ile yüklenen CSV dosyalarının bir listesini almak için aşağıdaki betiği çalıştırın:

```python
import os.path
import IPython
from pyspark.sql import SQLContext
source = "abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/"
dbutils.fs.ls(source + "/temp")
display(dbutils.fs.ls(source + "/temp/"))
```

Yeni bir dosya oluşturmak ve *parquet/flights* klasöründeki dosyaları listelemek için şu betiği çalıştırın:

```python
source = "abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/"

dbutils.fs.help()

dbutils.fs.put(source + "/temp/1.txt", "Hello, World!", True)
dbutils.fs.ls(source + "/temp/parquet/flights")
```
Bu kod örnekleriyle Azure Data Lake Storage Gen2 uyumlu bir hesapta depolanan verileri kullanarak HDFS'nin hiyerarşik özelliklerini keşfettiniz.

## <a name="query-the-data"></a>Verileri sorgulama

Bir sonraki adımda Azure Data Lake Storage'a yüklediğiniz verileri sorgulamaya başlayabilirsiniz. Aşağıdaki kod bloklarını **Cmd 1** bölümüne girin ve **Cmd + Enter** tuşlarına basarak Python betiğini çalıştırın.

### <a name="simple-queries"></a>Basit sorgular

Veri kaynaklarınız için veri çerçevesi oluşturma amacıyla aşağıdaki betiği çalıştırın:

> [!IMPORTANT]
> **<YOUR_CSV_FILE_NAME>** yer tutucusunun yerine bu öğreticinin başında indirdiğiniz dosyanın adını yazmayı unutmayın.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load(accountsource + "/<YOUR_CSV_FILE_NAME>.csv")
acDF.write.parquet(accountsource + '/parquet/airlinecodes')

#read the existing parquet file for the flights database that was created via the Azure Function
flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load(accountsource + "/parquet/flights")

#print the schema of the dataframes
acDF.printSchema()
flightDF.printSchema()

#print the flight database size
print("Number of flights in the database: ", flightDF.count())

#show the first 20 rows (20 is the default)
#to show the first n rows, run: df.show(n)
acDF.show(100, False)
flightDF.show(20, False)

#Display to run visualizations
#preferrably run this in a separate cmd cell
display(flightDF)
```

Verilerle analiz sorguları çalıştırmak için şu betiği çalıştırın:

```python
#Run each of these queries, preferrably in a separate cmd cell for separate analysis
#create a temporary sql view for querying flight information
FlightTable = spark.read.parquet(accountsource + '/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

#create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet(accountsource + '/parquet/airlinecodes')
AirlineCodes.createOrReplaceTempView('AirlineCodes')

#using spark sql, query the parquet file to return total flights in January and February 2016
out1 = spark.sql("SELECT * FROM FlightTable WHERE Month=1 and Year= 2016")
NumJan2016Flights = out1.count()
out2 = spark.sql("SELECT * FROM FlightTable WHERE Month=2 and Year= 2016")
NumFeb2016Flights=out2.count()
print("Jan 2016: ", NumJan2016Flights," Feb 2016: ",NumFeb2016Flights)
Total= NumJan2016Flights+NumFeb2016Flights
print("Total flights combined: ", Total)

# List out all the airports in Texas
out = spark.sql("SELECT distinct(OriginCityName) FROM FlightTable where OriginStateName = 'Texas'") 
print('Airports in Texas: ', out.show(100))

#find all airlines that fly from Texas
out1 = spark.sql("SELECT distinct(Carrier) FROM FlightTable WHERE OriginStateName='Texas'")
print('Airlines that fly to/from Texas: ', out1.show(100, False))
```
### <a name="complex-queries"></a>Karmaşık sorgular

Daha karmaşık sorguları yürütmek için not defterindeki segmentleri sırayla çalıştırıp sonuçları inceleyin.

```python
#find the airline with the most flights

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")

#show the top row without truncation
output.show(1, False)

#show the top 10 airlines
output.show(10, False)

#Determine which is the least on time airline

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE DepDelay>60 or ArrDelay>60 group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("select * from v")
#output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")
#show the top row without truncation
output.show(1, False)

#which airline improved its performance
#find the airline with the most improvement in delays
#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v1")
spark.sql("DROP VIEW IF EXISTS v2")
spark.sql("CREATE TEMPORARY VIEW v1 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2016 group by Carrier order by NumFlights desc LIMIT 10")
spark.sql("CREATE TEMPORARY VIEW v2 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2017 group by Carrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT distinct ac.AirlineName, v1.Carrier, v1.NumFlights, v2.NumFlights from v1 INNER JOIN v2 ON v1.Carrier = v2.Carrier INNER JOIN AirlineCodes ac ON v2.Carrier = ac.AirlineCode WHERE v1.NumFlights > v2.NumFlights")
#show the top row without truncation
output.show(10, False)

#display for visual analysis
display(output)
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight üzerinde Apache Hive kullanarak verileri ayıklama, dönüştürme ve yükleme](tutorial-extract-transform-load-hive.md)