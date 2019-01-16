---
title: 'Öğretici: Ayıklama, yerine yükleyin ve aktarımı işlemlerinin Azure Databricks kullanarak'
description: Bu öğreticide, verileri Azure Data Lake depolama Gen2 Preview'dan Azure Databricks'e veri ayıklamayı, verileri dönüştürün ve ardından verileri Azure SQL Data Warehouse'a veri yükleme konusunda bilgi edinin.
services: storage
author: jamesbak
ms.service: storage
ms.author: jamesbak
ms.topic: tutorial
ms.date: 01/14/2019
ms.component: data-lake-storage-gen2
ms.openlocfilehash: e4e75c65178c4bbedcf781c2fbf2149a94a702cd
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321203"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Öğretici: Ayıklama, dönüştürme ve Azure Databricks kullanarak verileri yüklemek

Bu öğreticide, bir ETL (ayıklama, dönüştürme ve veri yükleme) gerçekleştirmek için Azure Databricks'i kullanmayı işlemi. Azure Data Lake depolama 2. nesil Azure SQL veri ambarı'na etkin olan bir Azure depolama hesabındaki verileri taşıyın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure Databricks çalışma alanı oluşturun.
> * Azure Databricks'te Spark kümesi oluşturma.
> * Azure Data Lake depolama Gen2 yeteneğine sahip bir hesap oluşturun.
> * Verileri Azure Data Lake depolama Gen2 karşıya yükleyin.
> * Azure Databricks'te not defteri oluşturun.
> * Data Lake depolama Gen2 ' verileri ayıklayın.
> * Azure databricks'te verileri dönüştürün.
> * Verileri Azure SQL veri ambarı'na yükleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* Bir Azure SQL veri ambarı oluşturma, sunucu düzeyinde güvenlik duvarı kuralı oluşturun ve sunucu yöneticisi olarak sunucuya bağlanma Bölümündeki yönergeleri [hızlı başlangıç: Bir Azure SQL veri ambarı oluşturma](../../sql-data-warehouse/create-data-warehouse-portal.md) makalesi.
* Azure SQL veri ambarı için veritabanı ana anahtarı oluşturun. Bölümündeki yönergeleri [bir veritabanı ana anahtarı oluşturma](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key) makalesi.
* [Azure Data Lake depolama Gen2 hesabı oluşturma](data-lake-storage-quickstart-create-account.md).
* [U-SQL Examples and Issue Tracking](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) deposundan (**small_radio_json.json**) dosyasını indirin ve kaydettiğiniz yeri not edin.
* [Azure Portal](https://portal.azure.com/) oturum açın.

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

## <a name="create-the-workspace"></a>Çalışma alanı oluşturma

Bu bölümde, Azure portalını kullanarak bir Azure Databricks çalışma alanı oluşturun.

1. Azure portalında **Kaynak oluşturun** > **Analiz** > **Azure Databricks**'i seçin.

    ![Azure portalında Databricks](./media/data-lake-storage-handle-data-using-databricks/azure-databricks-on-portal.png "Databricks on Azure portal")

1. Altında **Azure Databricks hizmeti**, Databricks çalışma alanı oluşturmak için aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın.        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../../azure-resource-manager/resource-group-overview.md). |
    |**Konum**     | **Batı ABD 2**'yi seçin.        |
    |**Fiyatlandırma Katmanı**     |  Seçin **standart**.     |

    ![Azure Databricks çalışma alanı oluşturma](./media/data-lake-storage-handle-data-using-databricks/create-databricks-workspace.png "Create an Azure Databricks workspace")

1. **Panoya sabitle**’yi ve sonra **Oluştur**’u seçin.

1. Hesabın oluşturulması birkaç dakika sürer. Hesap oluşturma sırasında portal görüntüler **Azure Databricks için dağıtım gönderiliyor** kutucuğuna sağ tarafta. İşlem durumunu izlemek için üst kısmında ilerleme çubuğunu görüntüleyin.

    ![Databricks dağıtım kutucuğu](./media/data-lake-storage-handle-data-using-databricks/databricks-deployment-tile.png "Databricks dağıtım kutucuğu")

## <a name="create-the-spark-cluster"></a>Spark kümesi oluşturma

Bu öğreticide işlemleri gerçekleştirmek için bir Spark kümesi gerekir. Spark kümesi oluşturmak için aşağıdaki adımları kullanın.

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve seçin **çalışma alanını Başlat**.

1. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Küme**’yi seçin.

    ![Azure’da Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-on-azure.png "Databricks on Azure")

1. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure’da Databricks Spark kümesi oluşturma](./media/data-lake-storage-handle-data-using-databricks/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

1. Aşağıdaki alanlara değerleri girin ve diğer alanlar için varsayılan değerleri kabul edin:

    * Küme için bir ad girin.
    * Bu makale için bir küme oluşturun **5.1** çalışma zamanı.
    * Seçtiğinizden emin olun **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** onay kutusu. Küme kullanılmıyor ise küme sonlandırmak için bir süre (dakika cinsinden) belirtin.

1. **Küme oluştur**’u seçin.

Küme çalışmaya başladıktan sonra kümeye not defterleri ekleme ve Spark işleri çalıştırabilirsiniz.

## <a name="create-a-file-system"></a>Bir dosya sistemi oluşturun

Data Lake depolama Gen2'ye depolama hesabınızdaki verileri depolamak için bir dosya sistemi oluşturmak gerekir.

İlk olarak, Azure Databricks çalışma alanınızda bir not defteri oluşturun ve ardından dosya sistemi depolama hesabınızı oluşturmak için kod parçacıkları çalıştırırsınız.

1. İçinde [Azure portalında](https://portal.azure.com), oluşturduğunuz Azure Databricks çalışma alanına gidin ve seçin **çalışma alanını Başlat**.

2. Sol tarafta, seçin **çalışma**. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks'te not defteri oluşturma](./media/data-lake-storage-handle-data-using-databricks/databricks-create-notebook.png "Databricks not defteri oluşturma")

3. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Dil olarak **Scala**’yı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Bir Databricks not defteri için ayrıntıları sağlayın](./media/data-lake-storage-handle-data-using-databricks/databricks-notebook-details.png "bir Databricks not defteri için ayrıntıları sağlayın")

    **Oluştur**’u seçin.

4. Kopyala ve ilk hücreye aşağıdaki kod bloğu yapıştırın, ancak bu kodun henüz çalışmıyor.

    ```scala
    val configs = Map(
    "fs.azure.account.auth.type" -> "OAuth",
    "fs.azure.account.oauth.provider.type" -> "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
    "fs.azure.account.oauth2.client.id" -> "<application-id>",
    "fs.azure.account.oauth2.client.secret" -> "<authentication-key>"),
    "fs.azure.account.oauth2.client.endpoint" -> "https://login.microsoftonline.com/<tenant-id>/oauth2/token",
    "fs.azure.createRemoteFileSystemDuringInitialization"->"true")

    dbutils.fs.mount(
    source = "abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/<directory-name>",
    mountPoint = "/mnt/<mount-name>",
    extraConfigs = configs)
    ```

5. Bu kod bloğunda değiştirin `storage-account-name`, `application-id`, `authentication-id`, ve `tenant-id` adımları tamamlandığında topladığınız değerleri bu kod bloğu içinde yer tutucu değerlerini [bir kenara depolama hesabı Yapılandırma](#config) ve [hizmet sorumlusu oluşturma](#service-principal) bu makalenin bölümler. Ayarlama `file-system-name`, `directory-name`, ve `mount-name` noktası dosya sistemi, dizin ve bağlama vermek istediğiniz adı için yer tutucu değerleri.

6. Tuşuna **SHIFT + ENTER** bu blok kodu çalıştırmak için anahtarları.

## <a name="upload-the-sample-data"></a>Örnek verileri karşıya yükleme

Daha sonra Azure Databricks'te dönüştürmek için depolama hesabı için bir örnek veri dosyasını karşıya yüklemek için sonraki adımdır bakın.

Depolama hesabınıza yüklediğiniz örnek verileri karşıya yükleyin. Verileri, depolama hesabınıza yüklemek için kullandığınız yöntem, hiyerarşik ad alanı etkin olmasına bağlı olarak farklılık gösterir.

Karşıya yükleme yapmak için Azure Data Factory, distp veya AzCopy (sürüm 10) kullanabilirsiniz. Sürüm 10 AzCopy şu anda yalnızca önizleme kullanılabilir. AzCopy'yi kullanmak için aşağıdaki kodu bir komut penceresine yapıştırın:

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_PATH>\small_radio_json.json" https://<ACCOUNT_NAME>.dfs.core.windows.net/data --recursive 
```

## <a name="extract-the-data"></a>Veri ayıklamak

Databricks'te örnek verilerle çalışmak için depolama hesabınızdan verileri ayıklamak gerekir.

Databricks not defterinize dönün ve yeni bir not defterinizin hücresinde aşağıdaki kodu girin.

Aşağıdaki kod parçacığında, bir boş bir kod hücresine ekleyin. Depolama hesabından daha önce kaydettiğiniz değerlerle parantez içinde gösterilen yer tutucularını değiştirin.

```scala
dbutils.widgets.text("storage_account_name", "STORAGE_ACCOUNT_NAME", "<YOUR_STORAGE_ACCOUNT_NAME>")
dbutils.widgets.text("storage_account_access_key", "YOUR_ACCESS_KEY", "<YOUR_STORAGE_ACCOUNT_SHARED_KEY>")
```

Kodu çalıştırmak için SHIFT + Enter tuşlarını seçin.

Artık Azure Databricks'e veri çerçevesi olarak örnek json dosyasını yükleyebilirsiniz. Aşağıdaki kod, yeni bir hücreye yapıştırın. Yer tutucuları değerleriniz ile parantez içinde gösterilen değiştirin.

```scala
val df = spark.read.json("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/data/small_radio_json.json")
```

Kodu çalıştırmak için SHIFT + Enter tuşlarını seçin.

Veri çerçevesinin içeriğini görmek için aşağıdaki kodu çalıştırın:

```scala
df.show()
```

Aşağıdaki kod parçacığına benzer bir çıkış görürsünüz:

```bash
+---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
|               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
+---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
| El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
| Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
| Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
...
...
```

Artık verileri Azure Data Lake Storage Gen2'den Azure Databricks'e ayıkladınız.

## <a name="transform-the-data"></a>Verileri dönüştürme

Ham örnek veriler **small_radio_json.json** dosya radyo istasyonunun hedef kitlesini yakalar ve çeşitli sütunları vardır. Bu bölümde, veri kümesinden yalnızca belirli sütunları alınacak verileri dönüştürün.

İlk olarak, yalnızca sütunları almak **firstName**, **lastName**, **cinsiyet**, **konumu**, ve **düzeyi**, oluşturulan veri çerçevesi'nden.

```scala
val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
```

Aşağıdaki kod parçacığında gösterildiği gibi bir çıktı alırsınız:

```bash
+---------+----------+------+--------------------+-----+
|firstname|  lastname|gender|            location|level|
+---------+----------+------+--------------------+-----+
| Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|     Tess|  Townsend|     F|Nashville-Davidso...| free|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
|   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Tess|  Townsend|     F|Nashville-Davidso...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|     Liam|     Watts|     M|New York-Newark-J...| paid|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
|     Alan|     Morse|     M|Chicago-Napervill...| paid|
|   Dylann|    Thomas|     M|       Anchorage, AK| paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
+---------+----------+------+--------------------+-----+
```

Bu verilerde başka dönüştürmeler de yaparak **level** sütununun adını **subscription_type** olarak değiştirebilirsiniz.

```scala
val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
renamedColumnsDF.show()
```

Aşağıdaki kod parçacığında gösterildiği gibi bir çıktı alırsınız.

```bash
+---------+----------+------+--------------------+-----------------+
|firstname|  lastname|gender|            location|subscription_type|
+---------+----------+------+--------------------+-----------------+
| Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|     Tess|  Townsend|     F|Nashville-Davidso...|             free|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
|   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Tess|  Townsend|     F|Nashville-Davidso...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|     Liam|     Watts|     M|New York-Newark-J...|             paid|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
|     Alan|     Morse|     M|Chicago-Napervill...|             paid|
|   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
|  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
+---------+----------+------+--------------------+-----------------+
```

## <a name="load-the-data"></a>Verileri yükleme

Bu bölümde, dönüştürülen verileri Azure SQL Veri Ambarı'na yüklersiniz. Azure Databricks için Azure SQL veri ambarı Bağlayıcısı, SQL data warehouse'da tablo olarak, doğrudan bir dataframe karşıya yüklemek için kullanın.

SQL veri ambarı Bağlayıcısı verileri Azure Databricks ve Azure SQL veri ambarı arasında yüklemek için geçici depolama alanı olarak Azure Blob Depolama kullanır. Bu nedenle, depolama hesabına bağlanmak için kullanılacak yapılandırmayı sağlayarak başlarsınız. Hesap bu makalenin önkoşullarından bir parçası olarak oluşturmuş olmanız gerekir.

Azure Databricks'ten Azure Depolama hesabına erişmek için yapılandırmayı sağlayın.

```scala
val storageURI = "<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net"
val fileSystemName = "<FILE_SYSTEM_NAME>"
val accessKey =  "<ACCESS_KEY>"
```

Verileri Azure Databricks ile Azure SQL veri ambarı arasında taşırken kullanılacak geçici klasörü belirtin.

```scala
val tempDir = "abfs://" + fileSystemName + "@" + storageURI +"/tempDirs"
```

Aşağıdaki kod parçacığını çalıştırarak Azure Blob depolama erişim anahtarlarını yapılandırmada depolayın. Bu eylem, erişim anahtarını not defterinde düz metin tutmak çalışmamasını garantiler.

```scala
val acntInfo = "fs.azure.account.key."+ storageURI
sc.hadoopConfiguration.set(acntInfo, accessKey)
```

Azure SQL Veri Ambarı örneğine bağlanmak için değerleri sağlayın. SQL veri ambarı bir önkoşul olarak oluşturmuş olmanız gerekir.

```scala
//SQL Data Warehouse related settings
val dwDatabase = "<DATABASE NAME>"
val dwServer = "<DATABASE SERVER NAME>" 
val dwUser = "<USER NAME>"
val dwPass = "<PASSWORD>"
val dwJdbcPort =  "1433"
val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
```

Dönüştürülmüş veri çerçevesi yüklemek için aşağıdaki kod parçacığını çalıştırarak **renamedColumnsDF**, bir SQL data warehouse'da tablo olarak. Bu kod parçacığı SQL veritabanında **SampleTable** adlı bir tablo oluşturur.

```scala
spark.conf.set(
    "spark.sql.parquet.writeLegacyFormat",
    "true")
    
renamedColumnsDF.write
    .format("com.databricks.spark.sqldw")
    .option("url", sqlDwUrlSmall) 
    .option("dbtable", "SampleTable")
    .option( "forward_spark_azure_storage_credentials","True")
    .option("tempdir", tempDir)
    .mode("overwrite")
    .save()
```

Adlı bir veritabanı gördüğünüzü doğrulayın ve SQL veritabanı'na bağlanma **SampleTable**.

![Örnek tabloyu doğrulama](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table.png "örnek tabloyu doğrulama")

Tablonun içindekileri doğrulamak için bir seçme sorgusu çalıştırın. Tabloda aynı verileri olmalıdır **renamedColumnsDF** veri çerçevesi.

![Örnek tablo içeriğini doğrulama](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table-content.png "örnek tablo içeriğini doğrulama")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladıktan sonra kümeyi sonlandırabilirsiniz. Azure Databricks çalışma alanından seçin **kümeleri** soldaki. Kümenin altındaki sonlandırmak **eylemleri**seçin ve üç nokta (...)'in üzerine **sonlandırma** simgesi.

![Databricks kümesini durdurma](./media/data-lake-storage-handle-data-using-databricks/terminate-databricks-cluster.png "Databricks kümesini durdurma")

Kümeyi el ile sonlandırmaz, seçtiğiniz sağlanan bunu otomatik olarak durdurulur **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** küme oluşturulduğunda onay kutusu. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Azure Event Hubs'ı kullanarak Azure Databricks'e gerçek zamanlı veri akışını yapma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[Event Hubs kullanarak Azure Databricks Stream verileri](../../azure-databricks/databricks-stream-from-eventhubs.md)
