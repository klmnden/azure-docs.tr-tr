---
title: 'Öğretici: Spark kullanarak Azure Databricks ile Azure Data Lake Storage 2. Nesil Önizleme verilerine erişme | Microsoft Docs'
description: Bu öğreticide, Spark, Azure Data Lake depolama Gen2'ye depolama hesabınız verilere erişmek için bir Azure Databricks kümesinde sorguları çalıştırma işlemi gösterilmektedir.
services: storage
author: dineshmurthy
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 01/14/2019
ms.author: dineshm
ms.openlocfilehash: e72a4f71a42a892d14fad076b124426f0c32ac7d
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321815"
---
# <a name="tutorial-access-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Öğretici: Spark'ı kullanarak Azure Databricks ile Data Lake depolama Gen2 önizlemesi verilere erişme

Bu öğreticide, Azure Databricks kümesiyle Azure Data Lake depolama Gen2'ye sahip bir Azure depolama hesabında depolanan verilere bağlanmak gösterilir (Önizleme) etkin. Bu bağlantı, yerel olarak sorgular ve analiz, verilerinizde kümenizden çalıştırmanızı sağlar.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Databricks kümesi oluşturma
> * Yapılandırılmamış verileri bir depolama hesabına alma
> * Blob depolama alanındaki verilerinizde analiz çalıştırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide [Amerika Birleşik Devletleri Ulaştırma Bakanlığı](https://transtats.bts.gov/DL_SelectFields.asp) tarafından sunulan hava yolları uçuş verilerini kullanma ve sorgulama adımları gösterilmektedir. 

1. Seçin **Prezipped dosya** tüm veri alanlarını seçmek için onay kutusunu.
2. Seçin **indirme** ve sonuçları makinenize kaydedin.
3. Dosya adını not ve indirme yolunu oluşturmak; Bu bilgiler sonraki adımda ihtiyacınız var.

Bu öğreticiyi tamamlamak için bir depolama hesabı analitik özellikleriyle gerekir. Tamamlama öneririz bizim [hızlı](data-lake-storage-quickstart-create-account.md) oluşturmak için konu ile ilgili. 

## <a name="set-aside-storage-account-configuration"></a>Depolama hesabı yapılandırmasını not alın

Depolama hesabınızın ve bir dosya sistemi uç noktası URI'si adı gerekir.

Azure portalında depolama hesabınızın adını almak için seçtiğiniz **tüm hizmetleri** ve filtre terimini *depolama*. Ardından, **depolama hesapları** ve depolama hesabınızı bulun.

Dosya sistemi uç noktası URI'si almak için seçtiğiniz **özellikleri**, Özellikler bölmesinde değerini bulun **birincil ADLS dosya sistemi uç noktası** alan.

Hem bu değerleri bir metin dosyasına yapıştırın. Bunları yakında gerekir.

<a id="service-principal"/>

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bu konudaki yönergeleri izleyerek bir hizmet sorumlusu oluşturun: [Nasıl yapılır: Azure AD'yi kaynaklara erişebilen uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Bu makaledeki adımları gerçekleştirmek gibi gerekir ve belirli birkaç şey var.

:heavy_check_mark: Adımları gerçekleştirirken [bir Azure Active Directory uygulaması oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application) bölümü makalenin ayarladığınızdan emin olun **oturum açma URL'si** alanını **Oluştur** iletişim kutusu uç nokta URI'si, az önce toplanan.

:heavy_check_mark: Adımları gerçekleştirirken [uygulamanızı bir role atama](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) bölümü makalenin uygulamanıza atanacak emin **Blob Depolama katkıda bulunan rolü**.

:heavy_check_mark: Adımları gerçekleştirirken [oturum açma için değerleri alma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) makalesi, Yapıştır Kiracı kimliği, uygulama kimliği ve kimlik doğrulama anahtarı değerleri bir metin dosyasına bölümü. Bu kısa süre içinde olması gerekir.

## <a name="create-a-databricks-cluster"></a>Databricks kümesi oluşturma

Sonraki adım, veri çalışma alanı oluşturmak için Databricks kümesini oluşturmaktır.

1. Gelen [Azure portalında](https://portal.azure.com)seçin **kaynak Oluştur**.
2. Girin **Azure Databricks** arama alanına.
3. Seçin **Oluştur** Azure Databricks dikey penceresinde.
4. Databricks hizmetinizi adlandırın **myFlightDataService** (mutlaka denetleyin *panoya Sabitle* checkbox aynı hizmet oluşturma).
5. Seçin **çalışma alanını Başlat** çalışma alanına yeni bir tarayıcı penceresinde açın.
6. Seçin **kümeleri** sol gezinti çubuğundaki.
7. Seçin **küme oluşturma**.
8. **Cluster name** (Küme adı) alanına **myFlightDataCluster** yazın.
9. **Worker Type** (Çalışan Türü) alanında **Standard_D8s_v3** seçeneğini belirleyin.
10. **Min Workers** (Minimum Çalışan) değerini **4** olarak değiştirin.
11. Seçin **küme oluşturma** sayfanın üstünde. (Bu işlem son 5 dakika kadar sürebilir.)
12. İşlem tamamlandığında seçin **Azure Databricks** üzerinde gezinti çubuğunda sol üst köşesindeki.
13. Sayfanın alt yarısındaki **New** (Yeni) bölümünden **Notebook** (Not Defteri) öğesini seçin.
14. İçinde tercih ettiğiniz bir ad girin **adı** alan ve seçim **Python** dili olarak.
15. Diğer tüm alanlar varsayılan değerlerde bırakılabilir.
16. **Oluştur**’u seçin.
17. Kopyala ve ilk hücreye aşağıdaki kod bloğu yapıştırın, ancak bu kodun henüz çalışmıyor.

    ```Python
    configs = {"fs.azure.account.auth.type": "OAuth",
           "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
           "fs.azure.account.oauth2.client.id": "<application-id>",
           "fs.azure.account.oauth2.client.secret": "<authentication-id>",
           "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token",
           "fs.azure.createRemoteFileSystemDuringInitialization": "true"}

    dbutils.fs.mount(
    source = "abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/folder1",
    mount_point = "/mnt/flightdata",
    extra_configs = configs)
    ```
18. Bu kod bloğunda değiştirin `storage-account-name`, `application-id`, `authentication-id`, ve `tenant-id` adımları tamamlandığında topladığınız değerleri bu kod bloğu içinde yer tutucu değerlerini [bir kenara depolama hesabı Yapılandırma](#config) ve [hizmet sorumlusu oluşturma](#service-principal) bu makalenin bölümler. Değiştirin `file-system-name` yer tutucu dosya sisteminize vermek istediğiniz herhangi bir ada sahip.

19. Tuşuna **SHIFT + ENTER** bu blok kodu çalıştırmak için anahtarları.

## <a name="ingest-data"></a>Veriyi çekme

### <a name="copy-source-data-into-the-storage-account"></a>Kaynak verilerini depolama hesabına kopyalama

Bir sonraki görev, *.csv* dosyasındaki verileri Azure depolama alanına kopyalamak için AzCopy komutunu kullanmaktır. Bir komut istemi penceresi açın ve aşağıdaki komutları girin. Yer tutucuları değiştirmek emin `<DOWNLOAD_FILE_PATH>`, `<ACCOUNT_NAME>`, ve `<ACCOUNT_KEY>` , kenara bir önceki adımda karşılık gelen değerlerle.

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_FILE_PATH>" https://<ACCOUNT_NAME>.dfs.core.windows.net/dbricks/folder1/On_Time --recursive 
```

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Databricks Not Defteri'ni kullanarak CSV'yi Parquet biçimine dönüştürme

Databricks’i tarayıcınızda yeniden açın ve aşağıdaki adımları uygulayın:

1. Seçin **Azure Databricks** üzerinde gezinti çubuğunda sol üst köşesindeki.
2. Sayfanın alt yarısındaki **New** (Yeni) bölümünden **Notebook** (Not Defteri) öğesini seçin.
3. **Name** (Ad) alanına **CSV2Parquet** yazın.
4. Diğer tüm alanlar varsayılan değerlerde bırakılabilir.
5. **Oluştur**’u seçin.
6. Aşağıdaki kodu yapıştırın **Cmd 1** hücre. (Bu kodu otomatik-Düzenleyicisi'nde kaydeder.)

    ```python
    # Use the previously established DBFS mount point to read the data
    # create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time_On_Time*.csv")
    # read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
    print("Done")
    ```

## <a name="explore-data"></a>Verileri inceleme

Databricks çalışma alanına geri dönün ve seçin **son** sol gezinti çubuğunda simgesi.

1. Seçin **uçuş Data Analytics** dizüstü bilgisayar.
2. Yeni hücre oluşturmak için **Ctrl + Alt + N** tuşlarına basın.

Aşağıdaki kod bloklarını **Cmd 1** bölümüne girin ve **Cmd + Enter** tuşlarına basarak Python betiğini çalıştırın.

AzCopy ile yüklenen CSV dosyalarının bir listesini almak için aşağıdaki betiği çalıştırın:

```python
import os.path
import IPython
from pyspark.sql import SQLContext
display(dbutils.fs.ls("/mnt/flightdata/temp/"))
```

Yeni bir dosya oluşturmak ve *parquet/flights* klasöründeki dosyaları listelemek için şu betiği çalıştırın:

```python
dbutils.fs.put("/mnt/flightdata/temp/1.txt", "Hello, World!", True)
dbutils.fs.ls("/mnt/flightdata/temp/parquet/flights")
```

Bu kod örnekleriyle Data Lake Storage 2. Nesil etkin bir depolama hesabında depolanan verileri kullanarak HDFS’nin hiyerarşik özelliklerini keşfettiniz.

## <a name="query-the-data"></a>Verileri sorgulama

Bir sonraki adımda depolama hesabınıza yüklediğiniz verileri sorgulamaya başlayabilirsiniz. Aşağıdaki kod bloklarını **Cmd 1** bölümüne girin ve **Cmd + Enter** tuşlarına basarak Python betiğini çalıştırın.

### <a name="run-simple-queries"></a>Basit Sorgu çalıştırma

Veri kaynaklarınız için veri çerçevesi oluşturma amacıyla aşağıdaki betiği çalıştırın:

> [!IMPORTANT]
> **<YOUR_CSV_FILE_NAME>** yer tutucusunun yerine bu öğreticinin başında indirdiğiniz dosyanın adını yazmayı unutmayın.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/<YOUR_CSV_FILE_NAME>.csv")
acDF.write.parquet('/mnt/flightdata/parquet/airlinecodes')

#read the existing parquet file for the flights database that was created earlier
flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load("/mnt/flightdata/parquet/flights")

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
#preferably run this in a separate cmd cell
display(flightDF)
```

Verilerle analiz sorguları çalıştırmak için şu betiği çalıştırın:

```python
#Run each of these queries, preferably in a separate cmd cell for separate analysis
#create a temporary sql view for querying flight information
FlightTable = spark.read.parquet('/mnt/flightdata/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

#create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet('/mnt/flightdata/parquet/airlinecodes')
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

### <a name="run-complex-queries"></a>Karmaşık sorgular çalıştırma

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

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulan olmadığında kaynak grubunu ve tüm ilgili kaynakları silin. Bunu yapmak için depolama hesabı için kaynak grubunu seçin ve seçin **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

[!div class="nextstepaction"] 
> [Azure HDInsight üzerinde Apache Hive kullanarak verileri ayıklama, dönüştürme ve yükleme](data-lake-storage-tutorial-extract-transform-load-hive.md)

