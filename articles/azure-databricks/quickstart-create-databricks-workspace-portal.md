---
title: "Hızlı Başlangıç: Azure portalını kullanarak Azure Databricks'te Spark işini çalıştırma"
description: Bu hızlı başlangıçta Azure portalını kullanarak bir Azure Databricks çalışma alanı, bir Apache Spark kümesi oluşturma ve bir Spark işi çalıştırma işlemi gösterilmektedir.
services: azure-databricks
ms.service: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.workload: big-data
ms.topic: quickstart
ms.date: 07/23/2018
ms.custom: mvc
ms.openlocfilehash: 3a58a34271562b127735a4682046a7b646d0c085
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60771133"
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure Databricks'te Spark işini çalıştırma

Bu hızlı başlangıçta bir Azure Databricks çalışma alanı ve bu çalışma alanı içinde bir Apache Spark kümesi oluşturma işlemi gösterilir. Son olarak, Databricks kümesinde bir Spark işi çalıştırma hakkında bilgi edinirsiniz. Azure Databricks hakkında daha fazla bilgi için bkz. [Azure Databricks nedir?](what-is-azure-databricks.md)

Bu hızlı başlangıçta, Spark işinin parçası olarak, demografiye dayalı ücretsiz/ücretli kullanıma yönelik öngörüler elde etmek için bir radyo kanalının abonelik verilerini analiz edersiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde Azure portalını kullanarak bir Azure Databricks çalışma alanı oluşturursunuz.

1. Azure portalında **Kaynak oluşturun** > **Veri ve Analiz** > **Azure Databricks** seçeneklerini belirleyin.

    ![Azure portalında Databricks](./media/quickstart-create-databricks-workspace-portal/azure-databricks-on-portal.png "Databricks on Azure portal")

2. **Azure Databricks Hizmeti** bölümünde, Databricks çalışma alanı oluşturmak için değerler sağlayın.

    ![Azure Databricks çalışma alanı oluşturma](./media/quickstart-create-databricks-workspace-portal/create-databricks-workspace.png "Create an Azure Databricks workspace")

    Aşağıdaki değerleri sağlayın:
    
    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../azure-resource-manager/resource-group-overview.md). |
    |**Konum**     | **Doğu ABD 2**’yi seçin. Kullanılabilir diğer bölgeler için bkz. [Bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/).        |
    |**Fiyatlandırma Katmanı**     |  **Standart** veya **Premium** arasında seçim yapın. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

    **Panoya sabitle**’yi seçin ve sonra **Oluştur**’a tıklayın.

4. Çalışma alanının oluşturulması birkaç dakika sürer. Çalışma alanı oluşturma sırasında portal sağ tarafta **Azure Databricks için dağıtım gönderiliyor** kutucuğunu gösterir. Kutucuğu görmek için panonuzu sağa kaydırmanız gerekebilir. Ayrıca, ekranın üst kısmında gösterilen bir ilerleme çubuğu vardır. İlerleme durumu için her iki alanı da izleyebilirsiniz.

    ![Databricks dağıtım kutucuğu](./media/quickstart-create-databricks-workspace-portal/databricks-deployment-tile.png "Databricks dağıtım kutucuğu")

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

> [!NOTE]
> Azure Databricks kümesini oluşturmak için ücretsiz hesap oluşturmak istiyorsanız kümeyi oluşturmadan önce profilinize gidin ve aboneliğini **kullandıkça öde** modeline geçirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/).

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’a tıklayın.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Küme**’ye tıklayın.

    ![Azure’da Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-on-azure.png "Databricks on Azure")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure’da Databricks Spark kümesi oluşturma](./media/quickstart-create-databricks-workspace-portal/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

    Aşağıdakiler dışında diğer tüm varsayılan değerleri kabul edin:

   * Küme için bir ad girin.
   * Bu makale için **4.0** çalışma zamanıyla bir küme oluşturun.
   * **\_\_ dakika işlem yapılmadığında sonlandır** onay kutusunu seçtiğinizden emin olun. Küme kullanılmazsa kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.
    
     **Küme oluştur**’u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

Küme oluşturma hakkında daha fazla bilgi için bkz. [Azure Databricks üzerinde Spark kümesi oluşturma](https://docs.azuredatabricks.net/user-guide/clusters/create.html).


## <a name="download-a-sample-data-file"></a>Örnek veri dosyası indirme
Örnek bir JSON veri dosyası indirin ve Azure blob depolama alanına kaydedin.

1. Bu örnek JSON veri dosyasını indirme [github'dan](https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) yerel bilgisayarınıza. Ham dosyayı yerel ortama kaydetmek için sağ tıklayın ve kaydedin.

2. Depolama hesabınız yoksa oluşturabilirsiniz.
   - Azure portalda **Kaynak oluştur**’u seçin. **Depolama** kategorisini ve ardından **Depolama Hesapları**'nı seçin
   - Depolama hesabına benzersiz bir ad verin.
   - Seçin **hesap türü**: **Blob Depolama**
   - **Kaynak Grubu** adı belirleyin. Databricks çalışma alanını oluşturduğunuz aynı kaynak grubunu kullanın.
    
     Daha fazla bilgi için bkz. [Azure Blob depolama hesabını oluşturma](../storage/common/storage-quickstart-create-account.md).

3. Blob Depolama hesabında bir depolama Kapsayıcısı oluşturun ve örnek json dosyasını kapsayıcıya yükleyin. Dosyayı yüklemek için Azure portalı veya [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md)'ni kullanabilirsiniz.

    - Azure portalında depolama hesabını açın.
    - **Bloblar**'ı seçin.
    - Boş bir kapsayıcı oluşturmak için **+ Kapsayıcı**'yı seçin.
    - Kapsayıcı için **Ad** belirtin, örneğin: `databricks`.
    - Seçin **özel (anonim olmayan erişim)** erişim düzeyi.
    - Kapsayıcı oluşturulduktan sonra kapsayıcı adını seçin.
    - Ardından **Yükle** düğmesini seçin.
    - **Dosyalar** sayfasında **Klasör simgesini** seçerek yüklenecek `small_radio_json.json` örnek dosyasını bulun.
    - Dosyayı yüklemek için **Yükle**'yi seçin.

## <a name="run-a-spark-sql-job"></a>Spark SQL işi çalıştırma
Databricks içinde bir not defteri oluşturmak, not defterini bir Azure Blob depolama hesabındaki verileri okuyacak şekilde yapılandırmak ve sonra veriler üzerinde bir Spark SQL işi çalıştırmak için aşağıdaki görevleri gerçekleştirin.

1. Sol bölmedeki **Çalışma Alanı**'na tıklayın. **Çalışma Alanı** açılır listesinden **Oluştur**’a ve sonra **Not Defteri**’ne tıklayın.

    ![Databricks’te not defteri oluşturma](./media/quickstart-create-databricks-workspace-portal/databricks-create-notebook.png "Create notebook in Databricks")

2. **Not Defteri Oluşturma** iletişim kutusuna bir ad girin, dil olarak **Scala**’yı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Databricks’te not defteri oluşturma](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-details.png "Create notebook in Databricks")

    **Oluştur**’a tıklayın.

3. Bu adımda Azure Depolama hesabını Databricks Spark kümesiyle ilişkilendirin. Bu ilişkilendirmeyi gerçekleştirmenin iki yolu vardır. Azure Depolama hesabını Databricks Dosya Sistemine (DBFS) bağlayabilir veya Azure Depolama hesabına doğrudan oluşturduğunuz uygulamadan erişebilirsiniz.

    > [!IMPORTANT]
    >Bu makalede **depolamayı DBFS'ye bağlama** yaklaşımı kullanılır. Bu yaklaşım, bağlı deponun küme dosya sistemiyle ilişkilendirilmesini sağlar. Bu sayede kümeye erişen tüm uygulamalar ilişkilendirilmiş depolamayı da kullanabilir. Doğrudan erişim yaklaşımı, erişimi yapılandırdığınız uygulamayla kısıtlıdır.
    >
    > Bağlama yaklaşımını kullanmak için bu makalede seçtiğiniz Databricks çalışma zamanı sürüm **4.0** ile bir Spark kümesi oluşturmanız gerekir.

    Aşağıdaki kod parçacığında `{YOUR CONTAINER NAME}`, `{YOUR STORAGE ACCOUNT NAME}` ve`{YOUR STORAGE ACCOUNT ACCESS KEY}` değerlerini Azure Depolama hesabınız için uygun değerlerle değiştirin. Kod parçacığını not defterindeki boş bir hücreye yapıştırın ve sonra kod hücresini çalıştırmak için SHIFT + ENTER tuşlarına basın.

   * **Depolama hesabını DBFS'ye bağlayın (önerilen)**. Kod parçacığında Azure Depolama hesabı yolu `/mnt/mypath` yoluna bağlanır. Böylece ilerde Azure Depolama hesabına eriştiğiniz her durumda tam yolu vermenize gerek kalmaz. Yalnızca `/mnt/mypath` yolunu kullanabilirsiniz.

         dbutils.fs.mount(
           source = "wasbs://{YOUR CONTAINER NAME}@{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net/",
           mountPoint = "/mnt/mypath",
           extraConfigs = Map("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net" -> "{YOUR STORAGE ACCOUNT ACCESS KEY}"))

   * **Depolama hesabına doğrudan erişme**

         spark.conf.set("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net", "{YOUR STORAGE ACCOUNT ACCESS KEY}")

     Depolama hesabı anahtarınızı alma yönergeleri için bkz. [Depolama erişim anahtarlarınızı yönetme](../storage/common/storage-account-manage.md#access-keys).

     > [!NOTE]
     > Azure Data Lake Store’u Azure Databricks üzerine bir Spark kümesi ile de kullanabilirsiniz. Yönergeler için bkz. [Data Lake Store’u Azure Databricks ile Kullanma](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html).

4. **small_radio_json.json** adlı örnek JSON veri dosyasındaki verileri kullanarak geçici tablo oluşturmak için bir SQL deyimi çalıştırın. Aşağıdaki kod parçacığında yer tutucu değerlerini kapsayıcınızın adı ve depolama hesabı adı ile değiştirin. Kod parçacığını not defterindeki bir kod hücresine yapıştırın ve sonra SHIFT + ENTER tuşlarına basın. Kod parçacığında `path` değeri, Azure Depolama hesabınıza yüklediğiniz örnek JSON dosyasının konumunu gösterir.

    ```sql
    %sql
    DROP TABLE IF EXISTS radio_sample_data;
    CREATE TABLE radio_sample_data
    USING json
    OPTIONS (
     path "/mnt/mypath/small_radio_json.json"
    )
    ```

    Komut başarıyla tamamlandıktan sonra, JSON dosyasındaki tüm verileri Databricks kümesinde tablo olarak görüntüleyebilirsiniz.

    `%sql` dili sihirli komutu, not defteri başka bir türde olsa bile not defterinden bir SQL kodu çalıştırmanızı sağlar. Daha fazla bilgi için bkz. [Bir not defterinde dilleri karıştırma](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook).

5. Çalıştırdığınız sorguyu daha iyi anlamak için örnek JSON verilerinin bir anlık görüntüsüne bakalım. Kod hücresine aşağıdaki kod parçacığını yapıştırın ve **SHIFT + ENTER** tuşuna basın.

    ```sql
    %sql
    SELECT * from radio_sample_data
    ```

6. Aşağıdaki ekran görüntüsünde gösterildiği gibi bir tablo çıktısı görürsünüz (yalnızca bazı sütunlar gösterilmiştir):

    ![Örnek JSON verileri](./media/quickstart-create-databricks-workspace-portal/databricks-sample-csv-data.png "Sample JSON data")

    Diğer ayrıntılara ek olarak, örnek veriler bir radyo kanalının dinleyicilerinin cinsiyetini (sütun adı: **cinsiyet**) ve aboneliğin ücretsiz veya ücretli olduğunu (sütun adı: **düzey**) kaydeder.

7. Bu durumda, bu verilerin her bir cinsiyet, ücretsiz hesaba sahip kullanıcı sayısı ve ücretli hesabı olan abone sayısı için gösterilecek görsel bir açıklamasını oluşturursunuz. Tablo çıktısının alt kısmında bulunan **Çubuk grafik** simgesine ve sonra **Çizim Seçenekleri**’ne tıklayın.

    ![Çubuk grafik oluşturma](./media/quickstart-create-databricks-workspace-portal/create-plots-databricks-notebook.png "Create bar chart")

8. **Çizimi Özelleştir** menüsünde, değerleri ekran görüntüsünde gösterilen şekilde sürükleyip bırakın.

    ![Çubuk grafiği özelleştirme](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-customize-plot.png "Customize bar chart")

   * **Anahtarlar**’ı **cinsiyet** olarak ayarlayın.
   * **Seri gruplandırmalar**’ı **düzey** olarak ayarlayın.
   * **Değerler**’ı **düzey** olarak ayarlayın.
   * **Toplama**’yı **SAYI** olarak ayarlayın.

     **Uygula**'ya tıklayın.

9. Çıktı aşağıdaki ekran görüntüsünde gösterildiği gibi görsel açıklamayı gösterir:

    ![Çubuk grafiği özelleştirme](./media/quickstart-create-databricks-workspace-portal/databricks-sql-query-output-bar-chart.png "Customize bar chart")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Makaleyi tamamladıktan sonra kümeyi sonlandırabilirsiniz. Bunu yapmak için Azure Databricks çalışma alanında sol bölmedeki **Kümeler**’i seçin. Sonlandırmak istediğiniz küme için imleci **Eylemler** sütunu altındaki üç noktanın üzerine taşıyın ve **Sonlandır** simgesini seçin.

![Databricks kümesini durdurma](./media/quickstart-create-databricks-workspace-portal/terminate-databricks-cluster.png "Databricks kümesini durdurma")

El ile otomatik olarak durdurur küme sonlandırmazsanız, seçtiğiniz sağlanan **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** küme oluşturulurken onay kutusu. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Databricks’te bir Spark kümesi oluşturdunuz ve Azure depolama alanındaki verileri kullanarak bir Spark işi çalıştırdınız. Diğer veri kaynaklarından Azure Databricks’e verileri aktarma hakkında bilgi almak için [Spark veri kaynakları](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html) bölümüne de bakabilirsiniz. Azure Databricks kullanılarak bir ETL işleminin (verileri ayıklama, dönüştürme ve yükleme) nasıl gerçekleştirileceğini öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme](databricks-extract-load-sql-data-warehouse.md)
