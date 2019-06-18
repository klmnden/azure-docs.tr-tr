---
title: Bir Cosmos DB uç noktası ile Azure Databricks uygulayın
description: Bu öğreticide, Azure Databricks, Cosmos DB için etkin bir hizmet uç noktası ile bir sanal ağda uygulamak açıklar.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: tutorial
ms.date: 04/17/2019
ms.openlocfilehash: d1268ea2cfc22e6350edb32230588a497be8bc79
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67054611"
---
# <a name="tutorial-implement-azure-databricks-with-a-cosmos-db-endpoint"></a>Öğretici: Bir Cosmos DB uç noktası ile Azure Databricks uygulayın

Bu öğreticide, Cosmos DB için eklenen Databricks ortamı bir hizmet uç noktası ile etkinleştirilmiş bir sanal ağa ekleme açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sanal ağ içinde bir Azure Databricks çalışma alanı oluşturma
> * Bir Cosmos DB hizmet uç noktası oluşturma
> * Bir Cosmos DB hesabı oluşturmayı ve verileri içeri aktarın
> * Azure Databricks kümesi oluşturma
> * Bir Azure Databricks not defterinden Cosmos DB'yi sorgulama

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakileri yapın:

* Oluşturma bir [Azure Databricks çalışma alanı bir sanal ağdaki](quickstart-create-databricks-workspace-vnet-injection.md).

* İndirme [Spark Bağlayıcısı](https://search.maven.org/remotecontent?filepath=com/microsoft/azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.4/azure-cosmosdb-spark_2.4.0_2.11-1.3.4-uber.jar).

* Örnek verileri indirme [ortam bilgilerini NOAA Ulusal merkezine](https://www.ncdc.noaa.gov/stormevents/). Durum veya alanı seçip **arama**. Sonraki sayfada seçme ve varsayılan değerleri kabul **arama**. Ardından **CSV indirme** sonuçları indirmek için sayfanın sol tarafındaki.

* İndirme [önceden derlenmiş bir ikilidir](https://aka.ms/csdmtool) Azure Cosmos DB veri geçiş aracı.

## <a name="create-a-cosmos-db-service-endpoint"></a>Bir Cosmos DB hizmet uç noktası oluşturma

1. Bir sanal ağ için bir Azure Databricks çalışma alanı dağıttıktan sonra sanal ağa gidin [Azure portalında](https://portal.azure.com). Databricks dağıtım oluşturulan genel ve özel alt ağları dikkat edin.

   ![Sanal ağ alt ağları](./media/service-endpoint-cosmosdb/virtual-network-subnets.png)

2. Seçin *Genel alt* ve Cosmos DB hizmet uç noktası oluşturun. Ardından **Kaydet**.
   
   ![Bir Cosmos DB hizmet uç noktası ekleme](./media/service-endpoint-cosmosdb/add-cosmosdb-service-endpoint.png)

## <a name="create-a-cosmos-db-account"></a>Cosmos DB hesabı oluşturma

1. Azure portalı açın. Ekranın sol üst tarafında seçin **kaynak Oluştur > veritabanları > Azure Cosmos DB**.

2. Doldurun **örnek ayrıntıları** üzerinde **Temelleri** aşağıdaki ayarlar ile sekmesindeki:

   |Ayar|Değer|
   |-------|-----|
   |Abonelik|*aboneliğiniz*|
   |Kaynak Grubu|*Kaynak grubunuz*|
   |Hesap Adı|DB-vnet-hizmet uç noktası|
   |API|Çekirdek (SQL)|
   |Location|Batı ABD|
   |Coğrafi Yedeklilik|Devre Dışı Bırak|
   |Çok bölgeli yazma|Etkinleştirme|

   ![Bir Cosmos DB hizmet uç noktası ekleme](./media/service-endpoint-cosmosdb/create-cosmosdb-account-basics.png)

3. Seçin **ağ** sekme ve sanal ağ yapılandırın. 

   a. Bir önkoşul olarak oluşturduğunuz sanal ağı seçin ve ardından *Genel alt*. Dikkat *özel alt* Not *'Microsoft AzureCosmosDB' uç noktası eksik '* . Cosmos DB hizmet uç noktası üzerinde etkinleştirilebilmesi olmasıdır *Genel alt*.

   b. Olduğundan emin olun **Azure portalından erişim izni** etkin. Bu ayar, Azure portalından Cosmos DB hesabınıza erişmenize olanak sağlar. Bu seçenek ayarlanırsa **Reddet**, hesabınıza erişmeye çalışırken hataları alırsınız. 

   > [!NOTE]
   > Bu öğretici için gerekli değildir, ancak ayrıca etkinleştirebilirsiniz *IP'mi erişime izin* yerel makinenizden Cosmos DB hesabınızın erişim olanağı istiyorsanız. Örneğin, hesabınıza Cosmos DB SDK'sını kullanarak bağlanıyorsanız, bu ayarın etkinleştirilmesi gerekir. Etkinleştirilirse, "Erişim reddedildi" hata alırsınız.

   ![Cosmos DB hesabı ağ ayarları](./media/service-endpoint-cosmosdb/create-cosmosdb-account-network.png)

4. Seçin **gözden geçir + Oluştur**, ardından **Oluştur** sanal ağ içindeki Cosmos DB hesabınızı oluşturmak için.

5. Cosmos DB hesabınız oluşturulduktan sonra gidin **anahtarları** altında **ayarları**. Birincil bağlantı dizesini kopyalayın ve daha sonra kullanmak için bir metin düzenleyicisinde kaydedin.

    ![Cosmos DB hesabı anahtarları sayfası](./media/service-endpoint-cosmosdb/cosmos-keys.png)

6. Seçin **Veri Gezgini** ve **yeni koleksiyon** yeni veritabanını ve koleksiyonu Cosmos DB hesabınıza eklemek için.

    ![Cosmos DB yeni koleksiyon](./media/service-endpoint-cosmosdb/new-collection.png)

## <a name="upload-data-to-cosmos-db"></a>Cosmos DB'ye veri yükleme

1. Grafik arabirim sürümünü açın [Cosmos DB için veri geçiş aracı](https://aka.ms/csdmtool), **Dtui.exe**.

    ![Cosmos DB veri geçiş aracı](./media/service-endpoint-cosmosdb/cosmos-data-migration-tool.png)

2. Üzerinde **kaynak bilgileri** sekmesinde **CSV dosyaları** içinde **içe** açılır. Ardından **Add Files** ve storm verileri CSV indirdiğiniz önkoşul olarak ekleyin.

    ![Cosmos DB veri geçiş aracı kaynak bilgileri](./media/service-endpoint-cosmosdb/cosmos-source-information.png)

3. Üzerinde **hedef bilgileri** sekmesinde, bağlantı dizenizi girin. Bağlantı dizesi biçimi `AccountEndpoint=<URL>;AccountKey=<key>;Database=<database>`. Önceki bölümde kaydettiğiniz birincil bağlantı dizesi AccountEndpoint ve AccountKey dahil edilmiştir. Append `Database=<your database name>` bağlantı dizesini ve select sonuna **doğrulama**. Ardından koleksiyon adını ve bölüm anahtarını ekleyin.

    ![Cosmos DB veri geçiş aracı hedef bilgileri](./media/service-endpoint-cosmosdb/cosmos-target-information.png)

4. Seçin **sonraki** Özet sayfasına ulaşana kadar. Ardından, **alma**.

## <a name="create-a-cluster-and-add-library"></a>Küme oluşturma ve kitaplık Ekle

1. Azure Databricks hizmetinize gidin [Azure portalında](https://portal.azure.com) seçip **çalışma alanını Başlat**.

   ![Databricks çalışma alanını başlatma](./media/service-endpoint-cosmosdb/launch-workspace.png)

2. Yeni bir küme oluşturun. Bir küme adı seçin ve geri kalan varsayılan ayarları kabul edin.

   ![Yeni küme ayarları](./media/service-endpoint-cosmosdb/create-cluster.png)

3. Kümeniz oluşturulduktan sonra küme sayfasına gidin ve seçin **kitaplıkları** sekmesi. Seçin **yükleme yeni** ve kitaplığını yüklemek için Spark Bağlayıcısı jar dosyasını karşıya yükleyin.

    ![Spark Bağlayıcısı Kitaplığı yükleyin](./media/service-endpoint-cosmosdb/install-cosmos-connector-library.png)

    Kitaplık üzerinde yüklendiğini doğrulayabilirsiniz **kitaplıkları** sekmesi.

    ![Databricks kümesini kitaplıkları sekmesi](./media/service-endpoint-cosmosdb/installed-library.png)

## <a name="query-cosmos-db-from-a-databricks-notebook"></a>Bir Databricks not defterinden Cosmos DB'yi sorgulama

1. Azure Databricks çalışma alanınıza gidin ve yeni bir python not defteri oluşturun.

    ![Yeni bir Databricks not defteri oluşturma](./media/service-endpoint-cosmosdb/new-python-notebook.png)

2. Aşağıdaki python Cosmos DB bağlantı yapılandırmasını ayarlamak üzere kod çalıştırın. Değişiklik **uç nokta**, **Masterkey**, **veritabanı**, ve **koleksiyon** uygun şekilde.

    ```python
    connectionConfig = {
      "Endpoint" : "https://<your Cosmos DB account name.documents.azure.com:443/",
      "Masterkey" : "<your Cosmos DB primary key>",
      "Database" : "<your database name>",
      "preferredRegions" : "West US 2",
      "Collection": "<your collection name>",
      "schema_samplesize" : "1000",
      "query_pagesize" : "200000",
      "query_custom" : "SELECT * FROM c"
    }
    ```

3. Verileri yüklemek ve geçici bir görünüm oluşturmak için aşağıdaki python kodu kullanın.

    ```python
    users = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**connectionConfig).load()
    users.createOrReplaceTempView("storm")
    ```

4. Veri döndüren bir SQL deyimi yürütmek için aşağıdaki Sihirli komutu kullanın.

    ```python
    %sql
    select * from storm
    ```

    Bir hizmet uç noktası etkin Cosmos DB kaynak VNet eklenen Databricks çalışma alanınız başarıyla bağlandınız. Okunacak Cosmos DB'ye bağlanma hakkında daha fazla bilgi bkz [Apache Spark için Azure Cosmos DB Bağlayıcısı](https://github.com/Azure/azure-cosmosdb-spark).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, Azure Databricks çalışma ve tüm ilgili kaynakları silin. İşin silinmesi, gereksiz faturalandırma önler. Azure Databricks çalışma gelecekte kullanmayı planlıyorsanız, kümeyi durdurun ve daha sonra yeniden başlatın. Size bu Azure Databricks çalışma alanını kullanmaya devam etmeyecekseniz, aşağıdaki adımları kullanarak bu öğreticide oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden **kaynak grupları** ve ardından oluşturduğunuz kaynak grubunun adına tıklayın.

2. Kaynak grubu sayfanızda seçin **Sil**, adını, metin kutusuna silinecek ve ardından kaynak **Sil** yeniden.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure Databricks çalışma alanı dağıtılan bir sanal ağa ve Cosmos DB Spark Bağlayıcısı Databricks'ten Cosmos DB veri sorgu için kullanılan. Bir sanal ağda Azure Databricks ile çalışma hakkında daha fazla bilgi için SQL Server ile Azure Databricks kullanarak öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Sorgu bir SQL Server Linux Docker kapsayıcısı içinde bir sanal ağdan bir Azure Databricks not defteri](vnet-injection-sql-server.md)
