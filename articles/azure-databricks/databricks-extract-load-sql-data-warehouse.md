---
title: 'Öğretici: Azure Databricks kullanarak ETL işlemleri gerçekleştirme'
description: Verileri Data Lake depolama Gen2 ' Azure Databricks'e veri ayıklamayı, verileri dönüştürün ve ardından verileri Azure SQL Data Warehouse'a veri yükleme konusunda bilgi edinin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.workload: Active
ms.date: 02/15/2019
ms.openlocfilehash: e306245da2c76560ad447358fa1a57e491c370ee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60239242"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Öğretici: Ayıklama, dönüştürme ve Azure Databricks kullanarak verileri yüklemek

Bu öğreticide, bir ETL (ayıklama, dönüştürme ve veri yükleme) gerçekleştirmek Azure Databricks kullanarak işlemi. Verileri Azure Data Lake depolama Gen2 ' Azure Databricks'e ayıklar, Azure databricks'te veriler üzerinde dönüştürmeler çalıştırın ve ardından dönüştürülmüş verileri Azure SQL Data Warehouse'a veri yükleme.

Bu öğreticideki adımlarda, verileri Azure Databricks'e aktarmak üzere Azure Databricks için SQL Veri Ambarı bağlayıcısı kullanılır. Bu bağlayıcı da, Azure Databricks kümesiyle Azure SQL Veri Ambarı arasında aktarılan veriler için geçici depolama alanı olarak Azure Blob Depolama'yı kullanır.

Aşağıdaki şekilde uygulama akışı gösterilmektedir:

![Data Lake Store ve SQL Veri Ambarı ile Azure Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-extract-transform-load-sql-datawarehouse.png "Data Lake Store ve SQL Veri Ambarı ile Azure Databricks")

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Databricks hizmeti oluşturun.
> * Azure Databricks'te Spark kümesi oluşturma.
> * Bir dosya sistemi, Data Lake depolama Gen2 hesabı oluşturun.
> * Örnek verileri Azure Data Lake depolama Gen2 hesabına yükleyin.
> * Bir hizmet sorumlusu oluşturun.
> * Azure Data Lake depolama Gen2 hesabından verileri ayıklayın.
> * Azure databricks'te verileri dönüştürün.
> * Verileri Azure SQL veri ambarı'na yükleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!Note]
> Kullanarak bu öğreticiyi gerçekleştirilmesi **Azure ücretsiz deneme aboneliği**.
> Azure Databricks kümesini oluşturmak için ücretsiz hesap oluşturmak istiyorsanız kümeyi oluşturmadan önce profilinize gidin ve aboneliğini **kullandıkça öde** modeline geçirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/).
     
## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki görevleri tamamlayın:

* Bir Azure SQL veri ambarı oluşturma, sunucu düzeyinde güvenlik duvarı kuralı oluşturun ve sunucu yöneticisi olarak sunucuya bağlanma Bkz: [hızlı başlangıç: Bir Azure SQL veri ambarı oluşturma](../sql-data-warehouse/create-data-warehouse-portal.md).

* Azure SQL veri ambarı için veritabanı ana anahtarı oluşturun. Bkz: [bir veritabanı ana anahtarı oluşturma](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key).

* Azure Blob depolama hesabı ve bu hesabın içinde bir kapsayıcı oluşturun. Ayrıca, depolama hesabına erişmek için erişim anahtarını alın. Bkz: [hızlı başlangıç: Bir Azure Blob Depolama hesabı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md).

* Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturun. Bkz: [Azure Data Lake depolama Gen2 hesap oluşturma](../storage/blobs/data-lake-storage-quickstart-create-account.md).

*  Bir hizmet sorumlusu oluşturun. Bkz: [nasıl yapılır: Azure AD'yi kaynaklara erişebilen uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

   Birkaç, bu makaledeki adımları gerçekleştirmek olarak gerçekleştirmeniz yeterli belirli bir şey yoktur.

   * Adımları gerçekleştirirken [uygulamanızı bir role atama](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) bölümü makalenin atadığınızdan emin olun **depolama Blob verileri katkıda bulunan** rolüne hizmet sorumlusu.

     > [!IMPORTANT]
     > Data Lake depolama Gen2'ye depolama hesabı kapsamında bir rol atamak emin olun. Üst kaynak grubuna veya aboneliğe rol atayabilir, ancak bu rol atamaları depolama hesabına dolmaya başladığını kadar izinleri ile ilgili hataları alırsınız.

   * Adımları gerçekleştirirken [oturum açma için değerleri alma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) makalesi, Yapıştır Kiracı kimliği, uygulama kimliği ve kimlik doğrulama anahtarı değerleri bir metin dosyasına bölümü. Bu kısa süre içinde olması gerekir.

* [Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="gather-the-information-that-you-need"></a>Gereksinim duyduğunuz bilgileri toplayın

Bu öğretici önkoşulları tamamladığınızdan emin olun.

   Başlamadan önce bu öğeleri bilgilerin sahip olmalıdır:

   :heavy_check_mark:  Veritabanı adı, veritabanı sunucusu adı, kullanıcı adı ve parola, Azure SQL veri ambarı.

   :heavy_check_mark:  Blob depolama hesabınızın erişim anahtarı.

   :heavy_check_mark:  Data Lake depolama Gen2'ye depolama hesabınızın adı.

   :heavy_check_mark:  Aboneliğinizin Kiracı kimliği.

   :heavy_check_mark:  Azure Active Directory (Azure AD) ile kaydedilen uygulamasının uygulama kimliği.

   :heavy_check_mark:  Azure AD'ye kayıtlı uygulama için kimlik doğrulama anahtarı.

## <a name="create-an-azure-databricks-service"></a>Azure Databricks hizmeti oluşturma

Bu bölümde, Azure portalını kullanarak bir Azure Databricks hizmeti oluşturun.

1. Azure portalında **Kaynak oluşturun** > **Analiz** > **Azure Databricks**'i seçin.

    ![Azure portalında Databricks](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-on-portal.png "Databricks on Azure portal")

2. Altında **Azure Databricks hizmeti**, Databricks hizmeti oluşturmak için aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın.        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../azure-resource-manager/resource-group-overview.md). |
    |**Konum**     | **Batı ABD 2**'yi seçin.  Kullanılabilir diğer bölgeler için bkz. [Bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/).      |
    |**Fiyatlandırma Katmanı**     |  Seçin **standart**.     |

3. **Panoya sabitle**’yi ve sonra **Oluştur**’u seçin.

4. Hesabın oluşturulması birkaç dakika sürer. Hesap oluşturma sırasında portal görüntüler **Azure Databricks için dağıtım gönderiliyor** kutucuğuna sağ tarafta. İşlem durumunu izlemek için üst kısmında ilerleme çubuğunu görüntüleyin.

    ![Databricks dağıtım kutucuğu](./media/databricks-extract-load-sql-data-warehouse/databricks-deployment-tile.png "Databricks dağıtım kutucuğu")

## <a name="create-a-spark-cluster-in-azure-databricks"></a>Azure Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks hizmetine gidin ve seçin **çalışma alanını Başlat**.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Küme**’yi seçin.

    ![Azure’da Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-on-azure.png "Databricks on Azure")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure’da Databricks Spark kümesi oluşturma](./media/databricks-extract-load-sql-data-warehouse/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

4. Aşağıdaki alanlara değerleri girin ve diğer alanlar için varsayılan değerleri kabul edin:

    * Küme için bir ad girin.

    * Bu makale için bir küme oluşturun **5.1** çalışma zamanı.

    * Seçtiğinizden emin olun **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** onay kutusu. Küme kullanılmıyor ise küme sonlandırmak için bir süre (dakika cinsinden) belirtin.

    * **Küme oluştur**’u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleme ve Spark işleri çalıştırabilirsiniz.

## <a name="create-a-file-system-in-the-azure-data-lake-storage-gen2-account"></a>Azure Data Lake depolama Gen2 hesabı bir dosya sistemi oluşturun

Bu bölümde, Azure Databricks çalışma alanında bir not defteri oluşturun ve depolama hesabı yapılandırmak için kod parçacıkları çalıştırın

1. İçinde [Azure portalında](https://portal.azure.com), oluşturduğunuz Azure Databricks hizmetine gidin ve seçin **çalışma alanını Başlat**.

2. Sol tarafta, seçin **çalışma**. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks'te not defteri oluşturma](./media/databricks-extract-load-sql-data-warehouse/databricks-create-notebook.png "Databricks not defteri oluşturma")

3. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Dil olarak **Scala**’yı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Bir Databricks not defteri için ayrıntıları sağlayın](./media/databricks-extract-load-sql-data-warehouse/databricks-notebook-details.png "bir Databricks not defteri için ayrıntıları sağlayın")

4. **Oluştur**’u seçin.

5. Kopyalayıp ilk hücreye aşağıdaki kod bloğu yapıştırın.

   ```scala
   spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net", "<application-id>")
   spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net", "<authentication-key>")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net", "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
   dbutils.fs.ls("abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")
   ```

6. Bu kod bloğunda değiştirin `application-id`, `authentication-id`, `tenant-id`, ve `storage-account-name` Bu kod bloğu içinde yer tutucu değerlerini Bu öğretici önkoşulları tamamlanırken toplanan değerlere sahip. Değiştirin `file-system-name` yer tutucu değerini, ad ile istediğiniz dosya sistemi sağlar.

   * `application-id`, Ve `authentication-id` uygulamasından active Directory Hizmet sorumlusu oluşturma işleminin parçası olarak kayıtlı olduğunuz.

   * `tenant-id` Aboneliğinizden olduğu.

   * `storage-account-name` Azure Data Lake depolama Gen2'ye depolama hesabınızın adıdır.

7. Tuşuna **SHIFT + ENTER** bu blok kodu çalıştırmak için anahtarları.

## <a name="ingest-sample-data-into-the-azure-data-lake-storage-gen2-account"></a>Örnek verileri Azure Data Lake depolama Gen2 dikkate alma

Bu bölüme başlamadan önce aşağıdaki önkoşulları tamamlamanız gerekir:

Aşağıdaki kodu bir not defteri hücresine girin:

    %sh wget -P /tmp https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json

Hücre içine basın **SHIFT + ENTER** kodu çalıştırmak için.

Şimdi bunun altında yeni bir hücreye aşağıdaki kodu girin ve daha önce kullandığınız aynı değerlerle köşeli ayraçlar içindeki görülen değerleri değiştirin:

    dbutils.fs.cp("file:///tmp/small_radio_json.json", "abfss://<file-system>@<account-name>.dfs.core.windows.net/")

Hücre içine basın **SHIFT + ENTER** kodu çalıştırmak için.

## <a name="extract-data-from-the-azure-data-lake-storage-gen2-account"></a>Azure Data Lake depolama Gen2 hesabından veri ayıklamak

1. Artık, bir Azure databricks'te veri çerçevesi olarak örnek json dosyasını yükleyebilirsiniz. Aşağıdaki kod, yeni bir hücreye yapıştırın. Yer tutucuları değerleriniz ile parantez içinde gösterilen değiştirin.

   ```scala
   val df = spark.read.json("abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/small_radio_json.json")
   ```

   * Değiştirin `file-system-name` yer tutucu değerini, depolama Gezgini'nde dosya sisteminize verdiğiniz ad.

   * Değiştirin `storage-account-name` depolama hesabınızın adıyla yer tutucu.

2. Tuşuna **SHIFT + ENTER** bu blok kodu çalıştırmak için anahtarları.

3. Veri çerçevesinin içeriğini görmek için aşağıdaki kodu çalıştırın:

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

## <a name="transform-data-in-azure-databricks"></a>Azure Databricks'te verileri dönüştürme

Ham örnek veriler **small_radio_json.json** dosya radyo istasyonunun hedef kitlesini yakalar ve çeşitli sütunları vardır. Bu bölümde, veri kümesinden yalnızca belirli sütunları alınacak verileri dönüştürün.

1. İlk olarak, yalnızca sütunları almak **firstName**, **lastName**, **cinsiyet**, **konumu**, ve **düzeyi**, oluşturulan veri çerçevesi'nden.

   ```scala
   val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
   specificColumnsDf.show()
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

2. Bu verilerde başka dönüştürmeler de yaparak **level** sütununun adını **subscription_type** olarak değiştirebilirsiniz.

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

## <a name="load-data-into-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı’na veri yükleme

Bu bölümde, dönüştürülen verileri Azure SQL Veri Ambarı'na yüklersiniz. Azure Databricks için Azure SQL veri ambarı Bağlayıcısı, SQL data warehouse'da tablo olarak, doğrudan bir dataframe karşıya yüklemek için kullanın.

Daha önce bahsedildiği gibi SQL veri ambarı Bağlayıcısı verileri Azure Databricks ve Azure SQL veri ambarı arasında yüklemek için geçici depolama alanı olarak Azure Blob Depolama kullanır. Bu nedenle, depolama hesabına bağlanmak için kullanılacak yapılandırmayı sağlayarak başlarsınız. Zaten hesabın bu makalenin önkoşullarından bir parçası olarak oluşturmuş olmanız gerekir.

1. Azure Databricks'ten Azure Depolama hesabına erişmek için yapılandırmayı sağlayın.

   ```scala
   val blobStorage = "<blob-storage-account-name>.blob.core.windows.net"
   val blobContainer = "<blob-container-name>"
   val blobAccessKey =  "<access-key>"
   ```

2. Verileri Azure Databricks ile Azure SQL veri ambarı arasında taşırken kullanılacak geçici klasörü belirtin.

   ```scala
   val tempDir = "wasbs://" + blobContainer + "@" + blobStorage +"/tempDirs"
   ```

3. Aşağıdaki kod parçacığını çalıştırarak Azure Blob depolama erişim anahtarlarını yapılandırmada depolayın. Bu eylem, erişim anahtarını not defterinde düz metin tutmak çalışmamasını garantiler.

   ```scala
   val acntInfo = "fs.azure.account.key."+ blobStorage
   sc.hadoopConfiguration.set(acntInfo, blobAccessKey)
   ```

4. Azure SQL Veri Ambarı örneğine bağlanmak için değerleri sağlayın. SQL veri ambarı bir önkoşul olarak oluşturmuş olmanız gerekir.

   ```scala
   //SQL Data Warehouse related settings
   val dwDatabase = "<database-name>"
   val dwServer = "<database-server-name>"
   val dwUser = "<user-name>"
   val dwPass = "<password>"
   val dwJdbcPort =  "1433"
   val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
   val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ":" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
   val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ":" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
   ```

5. Dönüştürülmüş veri çerçevesi yüklemek için aşağıdaki kod parçacığını çalıştırarak **renamedColumnsDF**, bir SQL data warehouse'da tablo olarak. Bu kod parçacığı SQL veritabanında **SampleTable** adlı bir tablo oluşturur.

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

6. Adlı bir veritabanı gördüğünüzü doğrulayın ve SQL veritabanı'na bağlanma **SampleTable**.

   ![Örnek tabloyu doğrulama](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table.png "örnek tabloyu doğrulama")

7. Tablonun içindekileri doğrulamak için bir seçme sorgusu çalıştırın. Tabloda aynı verileri olmalıdır **renamedColumnsDF** veri çerçevesi.

    ![Örnek tablo içeriğini doğrulama](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table-content.png "örnek tablo içeriğini doğrulama")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladıktan sonra kümeyi sonlandırabilirsiniz. Azure Databricks çalışma alanından seçin **kümeleri** soldaki. Kümenin altındaki sonlandırmak **eylemleri**seçin ve üç nokta (...)'in üzerine **sonlandırma** simgesi.

![Databricks kümesini durdurma](./media/databricks-extract-load-sql-data-warehouse/terminate-databricks-cluster.png "Databricks kümesini durdurma")

Kümeyi el ile sonlandırmaz, seçtiğiniz sağlanan bunu otomatik olarak durdurulur **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** küme oluşturulduğunda onay kutusu. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Databricks hizmeti oluşturma
> * Azure Databricks’te Spark kümesi oluşturma
> * Azure Databricks’te not defteri oluşturma
> * Bir Data Lake depolama Gen2 hesabından veri ayıklamak
> * Azure Databricks'te verileri dönüştürme
> * Azure SQL Veri Ambarı’na veri yükleme

Azure Event Hubs kullanarak Azure Databricks'e gerçek zamanlı veri akışı yapmayı öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[Event Hubs kullanarak Azure Databricks’e veri akışı sağlama](databricks-stream-from-eventhubs.md)
