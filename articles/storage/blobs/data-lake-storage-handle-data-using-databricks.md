---
title: Azure Databricks'i kullanarak ayıklama, yükleme ve aktarma işlemlerini gerçekleştirmeyi öğrenin
description: Azure Data Lake Storage Gen2 Önizleme'den Azure Databricks'e veri ayıklamayı, verileri dönüştürmeyi ve sonra da Azure SQL Veri Ambarı'na yüklemeyi öğrenin.
services: storage
author: jamesbak
ms.service: storage
ms.author: jamesbak
ms.topic: tutorial
ms.date: 12/06/2018
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 108495c367bdb50ed4799aab929e4d26ad14ab87
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975744"
---
# <a name="tutorial-extract-transform-and-load-data-using-azure-databricks"></a>Öğretici: Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme

Bu öğreticide Azure Databricks kullanarak Azure Data Lake Storage 2. Nesil etkin bir Azure Depolama hesabından Azure SQL Veri Ambarı'na veri aktarmak için bir ETL (veri ayıklama, dönüştürme ve yükleme) işlemi gerçekleştireceksiniz.

Aşağıdaki şekilde uygulama akışı gösterilmektedir:

![Data Lake Storage Gen2 ve SQL Veri Ambarı ile Azure Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-extract-transform-load-sql-datawarehouse.png "Data Lake Storage Gen2 ve SQL Veri Ambarı ile Azure Databricks")

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Databricks çalışma alanı oluşturma
> * Azure Databricks’te Spark kümesi oluşturma
> * Azure Data Lake Storage Gen2 uyumlu bir hesap oluşturma
> * Azure Data Lake Storage Gen2'ye veri yükleme
> * Azure Databricks’te not defteri oluşturma
> * Data Lake Storage Gen2'den veri ayıklama
> * Azure Databricks'te verileri dönüştürme
> * Azure SQL Veri Ambarı’na veri yükleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* Bir Azure SQL Veri Ambarı oluşturun, sunucu düzeyi güvenlik duvarı kuralı oluşturun ve sunucu yöneticisi olarak sunucuya bağlanın. [Hızlı başlangıç: Azure SQL Veri Ambarı oluşturma](../../sql-data-warehouse/create-data-warehouse-portal.md) başlığı altındaki yönergeleri izleyin
* Azure SQL Veri Ambarı için veritabanı ana anahtarı oluşturun. [Veritabanı Ana Anahtarı oluşturma](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key) başlığı altındaki yönergeleri izleyin.
* [Azure Data Lake Storage Gen2 hesabı oluşturma](data-lake-storage-quickstart-create-account.md)

## <a name="sign-in-to-the-azure-portal"></a>Azure Portal'da oturum açma

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde Azure portalını kullanarak bir Azure Databricks çalışma alanı oluşturursunuz.

1. Azure portalında **Kaynak oluşturun** > **Analiz** > **Azure Databricks**'i seçin.

    ![Azure portalında Databricks](./media/data-lake-storage-handle-data-using-databricks/azure-databricks-on-portal.png "Databricks on Azure portal")

2. **Azure Databricks Hizmeti** bölümünde, Databricks çalışma alanı oluşturmak için değerler sağlayın.

    ![Azure Databricks çalışma alanı oluşturma](./media/data-lake-storage-handle-data-using-databricks/create-databricks-workspace.png "Create an Azure Databricks workspace")

    Aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın.        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../../azure-resource-manager/resource-group-overview.md). |
    |**Konum**     | **Batı ABD 2**'yi seçin. Kullanılabilir diğer bölgeler için bkz. [Bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/).        |
    |**Fiyatlandırma Katmanı**     |  **Standart** veya **Premium** arasında seçim yapın. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

    **Panoya sabitle**’yi ve sonra **Oluştur**’u seçin.

3. Hesabın oluşturulması birkaç dakika sürer. Hesap oluşturma sırasında portal sağ tarafta **Azure Databricks için dağıtım gönderiliyor** kutucuğunu gösterir. Kutucuğu görmek için panonuzu sağa kaydırmanız gerekebilir. Ayrıca, ekranın üst kısmında gösterilen bir ilerleme çubuğu vardır. İlerleme durumu için her iki alanı da izleyebilirsiniz.

    ![Databricks dağıtım kutucuğu](./media/data-lake-storage-handle-data-using-databricks/databricks-deployment-tile.png "Databricks dağıtım kutucuğu")

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Küme**’yi seçin.

    ![Azure’da Databricks](./media/data-lake-storage-handle-data-using-databricks/databricks-on-azure.png "Databricks on Azure")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure’da Databricks Spark kümesi oluşturma](./media/data-lake-storage-handle-data-using-databricks/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

    Aşağıdaki alanlara değerleri girin ve diğer alanlar için varsayılan değerleri kabul edin:

    * Küme için bir ad girin.
    * Bu makale için **4.2** çalışma zamanıyla bir küme oluşturun.
    * **\_\_ dakika işlem yapılmadığında sonlandır** onay kutusunu seçtiğinizden emin olun. Küme kullanılmazsa kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.

    **Küme oluştur**’u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

## <a name="create-storage-account-file-system"></a>Depolama hesabı dosya sistemi oluşturma

Bu bölümde, Azure Databricks çalışma alanında bir not defteri oluşturacak ve ardından depolama hesabını yapılandırmak için kod parçacıklarını çalıştıracaksınız.

1. [Azure portalında](https://portal.azure.com), oluşturduğunuz Azure Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Sol bölmede **Çalışma Alanı**’nı seçin. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks’te not defteri oluşturma](./media/data-lake-storage-handle-data-using-databricks/databricks-create-notebook.png "Create notebook in Databricks")

3. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Dil olarak **Scala**’yı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Databricks’te not defteri oluşturma](./media/data-lake-storage-handle-data-using-databricks/databricks-notebook-details.png "Create notebook in Databricks")

    **Oluştur**’u seçin.

4. İlk hücreye aşağıdaki kodu girin ve kod yürütün. Gösterilen örnekte kendi değerlerinizle köşeli ayraçlar içindeki yer tutucuları değiştirmeniz unutmayın:

        ```scala
        %python%
        configs = {"fs.azure.account.auth.type": "OAuth",
            "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
            "fs.azure.account.oauth2.client.id": "<service-client-id>",
            "fs.azure.account.oauth2.client.secret": "<service-credentials>",
            "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token"}
     
        dbutils.fs.mount(
            source = "abfss://<file-system-name>@<account-name>.dfs.core.windows.net/[<directory-name>]",
            mount_point = "/mnt/<mount-name>",
            extra_configs = configs)
        ```

5. Kod hücresini çalıştırmak için **SHIFT + ENTER** tuşlarına basın.

Depolama hesabı için dosya sistemi oluşturulmuş olur.

## <a name="upload-data-to-the-storage-account"></a>Verileri depolama hesabına yükleme

Bir sonraki adım, örnek verileri daha sonra Azure Databricks'te dönüştürmek üzere depolama hesabına yüklemektir. 

> [!NOTE]
> Azure Data Lake Storage Gen2 ile uyumlu bir hesabınız yoksa [oluşturmak için hızlı başlangıç](./data-lake-storage-quickstart-create-account.md) bölümüne bakın.

1. [U-SQL Examples and Issue Tracking](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) deposundan (**small_radio_json.json**) dosyasını indirin ve kaydettiğiniz yeri not edin.

2. Sonraki adımda örnek verileri depolama hesabınıza yükleyin. Verileri depolama hesabınıza yüklemek için kullandığınız yöntem, hiyerarşik ad alanını etkinleştirme durumuna göre değişiklik gösterir.

    Hiyerarşik ad alanı Azure Depolama hesabınızda etkinleştirilmişse yükleme için Azure Data Factory, distp veya AzCopy (sürüm 10) araçlarını kullanabilirsiniz. AzCopy sürüm 10 şu anda yalnızca önizleme yoluyla kullanılabilir. AzCopy'yi kullanmak için aşağıdaki kodu bir komut penceresine yapıştırın:

    ```bash
    set ACCOUNT_NAME=<ACCOUNT_NAME>
    set ACCOUNT_KEY=<ACCOUNT_KEY>
    azcopy cp "<DOWNLOAD_PATH>\small_radio_json.json" https://<ACCOUNT_NAME>.dfs.core.windows.net/data --recursive 
    ```
    
## <a name="extract-data-from-azure-storage"></a>Azure Depolama verilerini ayıklama

DataBricks Not Defterinize dönün ve aşağıdaki kodu yeni bir hücreye girin:

1. Aşağıdaki kod parçacığını boş bir kod hücresine ekleyin ve yer tutucu değerleri daha önce depolama hesabından kaydettiğiniz değerlerle değiştirin.

    ```scala
    dbutils.widgets.text("storage_account_name", "STORAGE_ACCOUNT_NAME", "<YOUR_STORAGE_ACCOUNT_NAME>")
    dbutils.widgets.text("storage_account_access_key", "YOUR_ACCESS_KEY", "<YOUR_STORAGE_ACCOUNT_SHARED_KEY>")
    ```

    Kod hücresini çalıştırmak için **SHIFT + ENTER** tuşlarına basın.

2. Artık Azure Databricks'e veri çerçevesi olarak örnek json dosyasını yükleyebilirsiniz. Aşağıdaki kodu yeni bir hücreye yapıştırıp **SHIFT + ENTER** tuşlarına basın (yer tutucu değerlerini değiştirdiğinizden emin olun):

    ```scala
    val df = spark.read.json("abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/data/small_radio_json.json")
    ```

3. Veri çerçevesinin içeriğini görmek için aşağıdaki kodu çalıştırın.

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

Ham örnek veriler (**small_radio_json.json**) radyo istasyonunun hedef kitlesini yakalar ve çeşitli sütunları vardır. Bu bölümde, veri kümesinden yalnızca belirli sütunları içeri almak için verileri dönüştürürsünüz.

1. Oluşturduğunuz veri çerçevesinden yalnızca *firstName*, *lastName*, *gender*, *location* ve *level* sütunlarını alarak başlayın.

    ```scala
    val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
    ```

    Aşağıdaki kod parçacığında gösterildiği gibi bir çıkış alırsınız:

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

2.  Bu verilerde başka dönüştürmeler de yaparak **level** sütununun adını **subscription_type** olarak değiştirebilirsiniz.

    ```scala
    val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
    renamedColumnsDF.show()
    ```

    Aşağıdaki kod parçacığında gösterildiği gibi bir çıkış alırsınız.

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

Bu bölümde, dönüştürülen verileri Azure SQL Veri Ambarı'na yüklersiniz. Azure Databricks için Azure SQL Veri Ambarı bağlayıcısını kullanıp veri çerçevesini bir tablo olarak doğrudan SQL veri ambarına yükleyebilirsiniz.

Daha önce de belirtildiği gibi, SQL veri ambarı bağlayıcısı verileri Azure Databricks ile Azure SQL Veri Ambarı arasında aktarmak üzere karşıya yüklemek için geçici depolama alanı olarak Azure Blob Depolama'yı kullanır. Bu nedenle, depolama hesabına bağlanmak için kullanılacak yapılandırmayı sağlayarak başlarsınız. Bu makalenin önkoşullarından biri olarak hesabı önceden oluşturmuş olmalısınız.

1. Azure Databricks'ten Azure Depolama hesabına erişmek için yapılandırmayı sağlayın.

    ```scala
    val storageURI = "<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net"
    val fileSystemName = "<FILE_SYSTEM_NAME>"
    val accessKey =  "<ACCESS_KEY>"
    ```

2. Verileri Azure Databricks ile Azure SQL Veri Ambarı arasında taşırken kullanılacak geçici klasörü belirtin.

    ```scala
    val tempDir = "abfs://" + fileSystemName + "@" + storageURI +"/tempDirs"
    ```

3. Aşağıdaki kod parçacığını çalıştırarak Azure Blob depolama erişim anahtarlarını yapılandırmada depolayın. Bu sayede erişim anahtarını not defterinde düz metin olarak saklamanız gerekmez.

    ```scala
    val acntInfo = "fs.azure.account.key."+ storageURI
    sc.hadoopConfiguration.set(acntInfo, accessKey)
    ```

4. Azure SQL Veri Ambarı örneğine bağlanmak için değerleri sağlayın. Önkoşulların bir parçası olarak SQL veri ambarını oluşturmuş olmalısınız.

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

5. Dönüştürülmüş veri çerçevesi **renamedColumnsDF**'yi SQL veri ambarına tablo olarak yüklemek için aşağıdaki kod parçacığını çalıştırın. Bu kod parçacığı SQL veritabanında **SampleTable** adlı bir tablo oluşturur.

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

6. SQL veritabanına bağlanın ve **SampleTable** tablosunu gördüğünüzü doğrulayın.

    ![Örnek tabloyu doğrulama](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table.png "Örnek tabloyu doğrulama")

7. Tablonun içindekileri doğrulamak için bir seçme sorgusu çalıştırın. **renamedColumnsDF** veri çerçevesiyle aynı verileri içermelidir.

    ![Örnek tablo içeriğini doğrulama](./media/data-lake-storage-handle-data-using-databricks/verify-sample-table-content.png "Örnek tablo içeriğini doğrulama")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi çalıştırdıktan sonra kümeyi sonlandırabilirsiniz. Bunu yapmak için Azure Databricks çalışma alanında sol bölmedeki **Kümeler**’i seçin. Sonlandırmak istediğiniz küme için imleci **Eylemler** sütunu altındaki üç noktanın üzerine taşıyın ve **Sonlandır** simgesini seçin.

![Databricks kümesini durdurma](./media/data-lake-storage-handle-data-using-databricks/terminate-databricks-cluster.png "Databricks kümesini durdurma")

El ile otomatik olarak durdurur küme sonlandırmazsanız, seçtiğiniz sağlanan **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** küme oluşturulurken onay kutusu. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Databricks çalışma alanı oluşturma
> * Azure Databricks’te Spark kümesi oluşturma
> * Azure Data Lake Storage Gen2 uyumlu bir hesap oluşturma
> * Azure Data Lake Storage Gen2'ye veri yükleme
> * Azure Databricks’te not defteri oluşturma
> * Data Lake Storage Gen2'den veri ayıklama
> * Azure Databricks'te verileri dönüştürme
> * Azure SQL Veri Ambarı’na veri yükleme

Azure Event Hubs kullanarak Azure Databricks'e gerçek zamanlı veri akışı yapmayı öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[Event Hubs kullanarak Azure Databricks’e veri akışı sağlama](../../azure-databricks/databricks-stream-from-eventhubs.md)
