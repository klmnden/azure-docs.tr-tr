---
title: 'Öğretici: Azure Databricks kullanarak ETL işlemleri gerçekleştirme'
description: Verileri Data Lake depolama Gen2 ' Azure Databricks'e veri ayıklamayı, verileri dönüştürün ve ardından verileri Azure SQL Data Warehouse'a veri yükleme konusunda bilgi edinin.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.workload: Active
ms.date: 01/24/2019
ms.openlocfilehash: 57de2d9c63a4185997ac86056b9e3189ad66e478
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55893146"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Öğretici: Ayıklama, dönüştürme ve Azure Databricks kullanarak verileri yüklemek

Bu öğreticide, bir ETL (ayıklama, dönüştürme ve veri yükleme) gerçekleştirmek Azure Databricks kullanarak işlemi. Verileri Azure Data Lake depolama Gen2 ' Azure Databricks'e ayıklar, Azure databricks'te veriler üzerinde dönüştürmeler çalıştırın ve ardından dönüştürülmüş verileri Azure SQL Data Warehouse'a veri yükleme.

Bu öğreticideki adımlarda, verileri Azure Databricks'e aktarmak üzere Azure Databricks için SQL Veri Ambarı bağlayıcısı kullanılır. Bu bağlayıcı da, Azure Databricks kümesiyle Azure SQL Veri Ambarı arasında aktarılan veriler için geçici depolama alanı olarak Azure Blob Depolama'yı kullanır.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Databricks hizmeti oluşturun.
> * Azure Databricks'te Spark kümesi oluşturma.
> * Bir dosya sistemi oluşturun ve Azure Data Lake depolama Gen2 veri yükleyin.
> * Bir hizmet sorumlusu oluşturun.
> * Data Lake Store ' verileri ayıklayın.
> * Azure databricks'te verileri dönüştürün.
> * Verileri Azure SQL veri ambarı'na yükleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki görevleri tamamlayın:

* Bir Azure SQL veri ambarı oluşturma, sunucu düzeyinde güvenlik duvarı kuralı oluşturun ve sunucu yöneticisi olarak sunucuya bağlanma Bkz: [hızlı başlangıç: Bir Azure SQL veri ambarı oluşturma](../sql-data-warehouse/create-data-warehouse-portal.md).

* Azure SQL veri ambarı için veritabanı ana anahtarı oluşturun. Bkz: [bir veritabanı ana anahtarı oluşturma](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key).

* Bir Azure Data Lake depolama Gen2 hesabı oluşturun. Bkz: [bir Azure Data Lake depolama Gen2 hesabı oluşturma](../storage/blobs/data-lake-storage-quickstart-create-account.md).

* Azure Blob depolama hesabı ve bu hesabın içinde bir kapsayıcı oluşturun. Ayrıca, depolama hesabına erişmek için erişim anahtarını alın. Bkz: [hızlı başlangıç: Bir Azure Blob Depolama hesabı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md).

* [Azure Portal](https://portal.azure.com/) oturum açın.

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

## <a name="create-a-file-system-and-upload-sample-data"></a>Bir dosya sistemi oluşturun ve örnek verileri karşıya yükleme

İlk olarak Data Lake depolama Gen2 hesabınızda bir dosya sistemi oluşturun. Ardından, Data Lake Store için örnek veri dosyasını karşıya yükleyebilirsiniz. Bu filtreyi daha sonra Azure Databricks'te bazı dönüştürmeleri çalıştırmak için kullanacaksınız.

1. İndirme [small_radio_json.json](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) örnek veri dosyasını yerel dosya sisteminize.

2. Gelen [Azure portalında](https://portal.azure.com/), Bu öğretici için bir önkoşul olarak oluşturduğunuz Data Lake depolama Gen2 hesabına gidin.

3. Gelen **genel bakış** sayfa seçin depolama hesabının **Gezgini'nde Aç**.

   ![Depolama Gezgini'ni açın](./media/databricks-extract-load-sql-data-warehouse/data-lake-storage-open-storage-explorer.png "Depolama Gezgini'ni açın")

4. Seçin **Azure Depolama Gezgini'ni açın** Depolama Gezgini'ni açın.

   ![Depolama Gezgini ikinci istemi Aç](./media/databricks-extract-load-sql-data-warehouse/data-lake-storage-open-storage-explorer-2.png "ikinci istemi Depolama Gezgini'ni Aç")

   Depolama Gezgini'ni açar. Bir dosya sistemi oluşturun ve bu konudaki yönergeleri kullanarak örnek verileri karşıya yükleyin: [Hızlı Başlangıç: Bir Azure Data Lake depolama Gen2 hesabındaki verileri yönetmek için Azure Depolama Gezgini'ni kullanma](../storage/blobs/data-lake-storage-explorer.md).

<a id="service-principal"/>

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bu konudaki yönergeleri izleyerek bir hizmet sorumlusu oluşturun: [Nasıl yapılır: Azure AD'yi kaynaklara erişebilen uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Birkaç, bu makaledeki adımları gerçekleştirmek olarak gerçekleştirmeniz yeterli bir şey yoktur.

:heavy_check_mark: Adımları gerçekleştirirken [uygulamanızı bir role atama](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) bölümü makalenin uygulamanıza atanacak emin **Blob Depolama katkıda bulunan rolü**.

:heavy_check_mark: Adımları gerçekleştirirken [oturum açma için değerleri alma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) makalesi, Yapıştır Kiracı kimliği, uygulama kimliği ve kimlik doğrulama anahtarı değerleri bir metin dosyasına bölümü. Bu kısa süre içinde olması gerekir.
İlk olarak, Azure Databricks çalışma alanınızda bir not defteri oluşturun ve ardından dosya sistemi depolama hesabınızı oluşturmak için kod parçacıkları çalıştırırsınız.

## <a name="extract-data-from-the-data-lake-store"></a>Data Lake Store ' veri ayıklamak

Bu bölümde, Azure Databricks çalışma alanında bir not defteri oluşturun ve ardından verileri Data Lake Store ' Azure Databricks'e ayıklamak için kod parçacıkları çalıştırırsınız.

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
   ```

6. Bu kod bloğunda değiştirin `application-id`, `authentication-id`, ve `tenant-id` Bu kod bloğu içinde yer tutucu değerlerini kümesi aside depolama hesabı yapılandırma adımları tamamlandığında, toplanan değerlere sahip. Değiştirin `storage-account-name` yer tutucu değerini, depolama hesabınızın adı.

7. Tuşuna **SHIFT + ENTER** bu blok kodu çalıştırmak için anahtarları.

8. Artık, bir Azure databricks'te veri çerçevesi olarak örnek json dosyasını yükleyebilirsiniz. Aşağıdaki kod, yeni bir hücreye yapıştırın. Yer tutucuları değerleriniz ile parantez içinde gösterilen değiştirin.

   ```scala
   val df = spark.read.json("abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/small_radio_json.json")
   ```

   * Değiştirin `file-system-name` yer tutucu değerini, depolama Gezgini'nde dosya sisteminize verdiğiniz ad.

   * Değiştirin `storage-account-name` depolama hesabınızın adıyla yer tutucu.

9. Tuşuna **SHIFT + ENTER** bu blok kodu çalıştırmak için anahtarları.

10. Veri çerçevesinin içeriğini görmek için aşağıdaki kodu çalıştırın:

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
   val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
   val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ".database.windows.net:" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
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
