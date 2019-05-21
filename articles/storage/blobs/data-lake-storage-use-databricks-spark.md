---
title: "Öğretici: Azure Databricks Spark'ı kullanarak Azure Data Lake depolama Gen2'ye veri erişim | Microsoft Docs"
description: Bu öğreticide, Spark, Azure Data Lake depolama Gen2'ye depolama hesabınız verilere erişmek için Azure Databricks kümesinde sorguları çalıştırma işlemi gösterilmektedir.
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 03/11/2019
ms.author: normesta
ms.reviewer: dineshm
ms.openlocfilehash: b332c11e76ad335772cc607edcf569f896acb873
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65951386"
---
# <a name="tutorial-access-data-lake-storage-gen2-data-with-azure-databricks-using-spark"></a>Öğretici: Spark'ı kullanarak Azure Databricks ile Data Lake depolama Gen2 verilere erişme

Bu öğreticide, Azure Databricks kümesiyle Azure Data Lake depolama Gen2'ye etkin olan bir Azure depolama hesabına depolanan verilere bağlanma gösterilmektedir. Bu bağlantı, yerel olarak sorgular ve analiz, verilerinizde kümenizden çalıştırmanızı sağlar.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Databricks kümesi oluşturma
> * Yapılandırılmamış verileri bir depolama hesabına alma
> * Blob depolama alanındaki verilerinizi analiz çalıştırın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Data Lake depolama Gen2 hesabı oluşturun.

  Bkz: [Azure Data Lake depolama Gen2 hesap oluşturma](data-lake-storage-quickstart-create-account.md).

* Kullanıcı hesabınız olduğundan emin olun [depolama Blob verileri katkıda bulunan rolü](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac) atanmış.

* AzCopy v10 yükleyin. Bkz: [v10 AzCopy ile veri aktarma](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

* Bir hizmet sorumlusu oluşturun. Bkz: [nasıl yapılır: Azure AD'yi kaynaklara erişebilen uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

  Birkaç, bu makaledeki adımları gerçekleştirmek olarak gerçekleştirmeniz yeterli belirli bir şey yoktur.

  :heavy_check_mark: Adımları gerçekleştirirken [uygulamanızı bir role atama](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) bölümü makalenin atadığınızdan emin olun **depolama Blob verileri katkıda bulunan** rolüne hizmet sorumlusu.

  > [!IMPORTANT]
  > Data Lake depolama Gen2'ye depolama hesabı kapsamında bir rol atamak emin olun. Üst kaynak grubuna veya aboneliğe rol atayabilir, ancak bu rol atamaları depolama hesabına dolmaya başladığını kadar izinleri ile ilgili hataları alırsınız.

  :heavy_check_mark: Adımları gerçekleştirirken [oturum açma için değerleri alma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) makalesi, Yapıştır Kiracı kimliği, uygulama kimliği ve parola değerlerini bir metin dosyasına bölümü. Bu kısa süre içinde olması gerekir.

### <a name="download-the-flight-data"></a>Uçuş verilerini indirme

Bu öğreticide, nakliye büro istatistikleri uçuş verileri bir ETL işlemi gerçekleştirmek nasıl göstermek için kullanılır. Bu öğreticiyi tamamlamak için bu verileri indirmeniz gerekir.

1. Git [araştırma ve yenilikçi teknoloji yönetim, nakliye istatistikleri bürosu](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time).

2. Seçin **Prezipped dosya** tüm veri alanlarını seçmek için onay kutusunu.

3. Seçin **indirme** düğmesine tıklayın ve sonuçlar bilgisayarınıza kaydedin. 

4. Sıkıştırılmış dosyanın içeriğini sıkıştırmasını ve dosya adını not ve dosyanın yolu. Bu bilgiler sonraki adımda ihtiyacınız var.

## <a name="create-an-azure-databricks-service"></a>Azure Databricks hizmeti oluşturma

Bu bölümde, Azure portalını kullanarak bir Azure Databricks hizmeti oluşturun.

1. Azure portalında **Kaynak oluşturun** > **Analiz** > **Azure Databricks**'i seçin.

    ![Azure portalında Databricks](./media/data-lake-storage-use-databricks-spark/azure-databricks-on-portal.png "Databricks on Azure portal")

2. Altında **Azure Databricks hizmeti**, Databricks hizmeti oluşturmak için aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın.  |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../../azure-resource-manager/resource-group-overview.md). |
    |**Konum**     | **Batı ABD 2**'yi seçin. Kullanılabilir diğer bölgeler için bkz. [Bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/).       |
    |**Fiyatlandırma Katmanı**     |  Seçin **standart**.     |

    ![Bir Azure Databricks çalışma alanı oluşturma](./media/data-lake-storage-use-databricks-spark/create-databricks-workspace.png "bir Azure Databricks hizmeti oluşturma")

3. Hesabın oluşturulması birkaç dakika sürer. İşlem durumunu izlemek için üst kısmında ilerleme çubuğunu görüntüleyin.

4. **Panoya sabitle**’yi ve sonra **Oluştur**’u seçin.

## <a name="create-a-spark-cluster-in-azure-databricks"></a>Azure Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks hizmetine gidin ve seçin **çalışma alanını Başlat**.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Küme**’yi seçin.

    ![Azure’da Databricks](./media/data-lake-storage-use-databricks-spark/databricks-on-azure.png "Databricks on Azure")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure’da Databricks Spark kümesi oluşturma](./media/data-lake-storage-use-databricks-spark/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

4. Aşağıdaki alanlara değerleri girin ve diğer alanlar için varsayılan değerleri kabul edin:

    * Küme için bir ad girin.

    * Bu makale için bir küme oluşturun **5.1** çalışma zamanı.

    * Seçtiğinizden emin olun **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** onay kutusu. Küme kullanılmıyor ise küme sonlandırmak için bir süre (dakika cinsinden) belirtin.

    * **Küme oluştur**’u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleme ve Spark işleri çalıştırabilirsiniz.

## <a name="ingest-data"></a>Veriyi çekme

### <a name="copy-source-data-into-the-storage-account"></a>Kaynak verilerini depolama hesabına kopyalama

Veri kopyalamak için AzCopy kullanın, *.csv* Data Lake depolama Gen2 hesabınızı dosyasına.

1. Bir komut istemi penceresi açın ve depolama hesabınızda oturum açın aşağıdaki komutu girin.

   ```bash
   azcopy login
   ```

   Kullanıcı hesabınızın kimliğini doğrulamak için komut istemi penceresinde görüntülenen yönergeleri izleyin.

2. Verileri kopyalamak için *.csv* hesap, aşağıdaki komutu girin.

   ```bash
   azcopy cp "<csv-folder-path>" https://<storage-account-name>.dfs.core.windows.net/<file-system-name>/folder1/On_Time.csv
   ```

   * Değiştirin `<csv-folder-path>` yolu ile yer tutucu değerini *.csv* dosya.

   * Değiştirin `<storage-account-name>` yer tutucu değerini, depolama hesabınızın adı.

   * Değiştirin `<file-system-name>` yer tutucu dosya sisteminize vermek istediğiniz herhangi bir ada sahip.

## <a name="create-a-file-system-and-mount-it"></a>Bir dosya sistemi oluşturun ve bunu bağlama

Bu bölümde, depolama hesabınızdaki bir dosya sistemi ve klasör oluşturacaksınız.

1. İçinde [Azure portalında](https://portal.azure.com), oluşturduğunuz Azure Databricks hizmetine gidin ve seçin **çalışma alanını Başlat**.

2. Sol tarafta, seçin **çalışma**. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks'te not defteri oluşturma](./media/data-lake-storage-use-databricks-spark/databricks-create-notebook.png "Databricks not defteri oluşturma")

3. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Seçin **Python** dilini ve ardından Spark kümesini gibi daha önce oluşturduğunuz.

4. **Oluştur**’u seçin.

5. Kopyala ve ilk hücreye aşağıdaki kod bloğu yapıştırın, ancak bu kodun henüz çalışmıyor.

    ```Python
    configs = {"fs.azure.account.auth.type": "OAuth",
           "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
           "fs.azure.account.oauth2.client.id": "<appId>",
           "fs.azure.account.oauth2.client.secret": "<password>",
           "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant>/oauth2/token",
           "fs.azure.createRemoteFileSystemDuringInitialization": "true"}

    dbutils.fs.mount(
    source = "abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/folder1",
    mount_point = "/mnt/flightdata",
    extra_configs = configs)
    ```

18. Bu kod bloğunda değiştirin `appId`, `password`, `tenant`, ve `storage-account-name` Bu kod bloğu içinde yer tutucu değerlerini Bu öğretici önkoşulları tamamlanırken toplanan değerlere sahip. Değiştirin `file-system-name` yer tutucu değerini önceki adımda ADLS dosya sistemine verdiğiniz ada sahip.

Belirtilen yer tutucuları değiştirmek için bu değerleri kullanırsınız.

   * `appId`, Ve `password` uygulamasından active Directory Hizmet sorumlusu oluşturma işleminin parçası olarak kayıtlı olduğunuz.

   * `tenant-id` Aboneliğinizden olduğu.

   * `storage-account-name` Azure Data Lake depolama Gen2'ye depolama hesabınızın adıdır.

   * Değiştirin `file-system-name` yer tutucu dosya sisteminize vermek istediğiniz herhangi bir ada sahip.

   > [!NOTE]
   > Bir üretim ayarında, Azure Databricks'te parolanızı depolamayı düşünün. Ardından, arama anahtarı parolası yerine, kod bloğu ekleyin. Bu hızlı başlangıcı tamamladıktan sonra bkz [Azure Data Lake depolama Gen2](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html) makalede bu yaklaşım örneklerini görmek için Azure Databricks Web sitesinde.

19. Tuşuna **SHIFT + ENTER** bu blok kodu çalıştırmak için anahtarları.

   Buna daha sonra komutları ekleyeceksiniz gibi bu not defteri, açık tutun.

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Databricks Not Defteri'ni kullanarak CSV'yi Parquet biçimine dönüştürme

Daha önce oluşturduğunuz not defterine yeni bir hücresi ekleyin ve bu hücreye aşağıdaki kodu yapıştırın. 

```python
# Use the previously established DBFS mount point to read the data.
# create a data frame to read data.

flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/*.csv")

# read the airline csv file and write the output to parquet format for easy query.
flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
print("Done")
```

## <a name="explore-data"></a>Verileri inceleme

Yeni bir hücreye AzCopy karşıya CSV dosyaları listesini almak için aşağıdaki kodu yapıştırın.

```python
import os.path
import IPython
from pyspark.sql import SQLContext
display(dbutils.fs.ls("/mnt/flightdata"))
```

Yeni bir dosya oluşturmak ve *parquet/flights* klasöründeki dosyaları listelemek için şu betiği çalıştırın:

```python
dbutils.fs.put("/mnt/flightdata/1.txt", "Hello, World!", True)
dbutils.fs.ls("/mnt/flightdata/parquet/flights")
```

Bu kod örnekleriyle Data Lake Storage 2. Nesil etkin bir depolama hesabında depolanan verileri kullanarak HDFS’nin hiyerarşik özelliklerini keşfettiniz.

## <a name="query-the-data"></a>Verileri sorgulama

Bir sonraki adımda depolama hesabınıza yüklediğiniz verileri sorgulamaya başlayabilirsiniz. Aşağıdaki kod bloklarını **Cmd 1** bölümüne girin ve **Cmd + Enter** tuşlarına basarak Python betiğini çalıştırın.

Veri kaynaklarınız için veri çerçevelerini oluşturmak için aşağıdaki betiği çalıştırın:

* Değiştirin `<csv-folder-path>` yolu ile yer tutucu değerini *.csv* dosya.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time.csv")
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

Verilerde bazı temel analiz sorguları çalıştırmak için bu betiği girin.

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
out1 = spark.sql("SELECT distinct(Reporting_Airline) FROM FlightTable WHERE OriginStateName='Texas'")
print('Airlines that fly to/from Texas: ', out1.show(100, False))
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulan olmadığında kaynak grubunu ve tüm ilgili kaynakları silin. Bunu yapmak için depolama hesabı için kaynak grubunu seçin ve seçin **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure HDInsight üzerinde Apache Hive kullanarak verileri ayıklama, dönüştürme ve yükleme](data-lake-storage-tutorial-extract-transform-load-hive.md)
