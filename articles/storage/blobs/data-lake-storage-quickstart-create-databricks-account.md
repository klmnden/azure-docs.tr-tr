---
title: 'Hızlı Başlangıç: Azure Databricks kullanarak verileri Azure Data Lake depolama Gen2 çözümleme | Microsoft Docs'
description: Azure portalı ve Azure Data Lake depolama Gen2'ye depolama hesabınız kullanarak Azure Databricks'te Spark işini çalıştırma öğrenin.
services: storage
author: normesta
ms.author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 02/15/2019
ms.openlocfilehash: c5c69ded05e5ec6d1df6bd2befb4fe89417bae06
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60849634"
---
# <a name="quickstart-analyze-data-in-azure-data-lake-storage-gen2-by-using-azure-databricks"></a>Hızlı Başlangıç: Azure Databricks kullanarak Azure Data Lake depolama Gen2 verileri çözümleme

Bu hızlı başlangıçta, Azure Data Lake depolama Gen2'ye etkin olan bir depolama hesabına depolanan veriler üzerinde analiz gerçekleştirmek için Azure Databricks kullanarak bir Apache Spark işi çalıştırma işlemini göstermektedir.

Spark işi bir parçası olarak, demografiye dayalı ücretsiz/Ücretli kullanımı hakkında Öngörüler elde etmek için bir radyo kanalının abonelik verilerini analiz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* Bir Data Lake Gen2'ye depolama hesabı oluşturun. Bkz: [hızlı başlangıç: Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturma](data-lake-storage-quickstart-create-account.md)

  Depolama hesabı adı, bir metin dosyasına yapıştırın. Yakında gerekir.

* Bir hizmet sorumlusu oluşturun. Bkz: [nasıl yapılır: Azure AD'yi kaynaklara erişebilen uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

  Birkaç, bu makaledeki adımları gerçekleştirmek olarak gerçekleştirmeniz yeterli belirli bir şey yoktur.

  :heavy_check_mark: Adımları gerçekleştirirken [uygulamanızı bir role atama](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) bölümü makalenin atadığınızdan emin olun **depolama Blob verileri katkıda bulunan** rolüne hizmet sorumlusu.

  > [!IMPORTANT]
  > Data Lake depolama Gen2'ye depolama hesabı kapsamında bir rol atamak emin olun. Üst kaynak grubuna veya aboneliğe rol atayabilir, ancak bu rol atamaları depolama hesabına dolmaya başladığını kadar izinleri ile ilgili hataları alırsınız.

  :heavy_check_mark: Adımları gerçekleştirirken [oturum açma için değerleri alma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) makalesi, Yapıştır Kiracı kimliği, uygulama kimliği ve kimlik doğrulama anahtarı değerleri bir metin dosyasına bölümü. Bu kısa süre içinde olması gerekir.

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde Azure portalını kullanarak bir Azure Databricks çalışma alanı oluşturursunuz.

1. Azure portalında **Kaynak oluşturun** > **Analiz** > **Azure Databricks**'i seçin.

    ![Azure portalında Databricks](./media/data-lake-storage-quickstart-create-databricks-account/azure-databricks-on-portal.png "Databricks on Azure portal")

2. **Azure Databricks Hizmeti** bölümünde, Databricks çalışma alanı oluşturmak için değerler sağlayın.

    ![Azure Databricks çalışma alanı oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/create-databricks-workspace.png "Create an Azure Databricks workspace")

    Aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../../azure-resource-manager/resource-group-overview.md). |
    |**Konum**     | **Batı ABD 2**'yi seçin. Tercih ettiğiniz başka bir genel bölgeyi seçebilirsiniz.        |
    |**Fiyatlandırma Katmanı**     |  **Standart** veya **Premium** arasında seçim yapın. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

    **Panoya sabitle**’yi seçin ve sonra **Oluştur**’a tıklayın.

3. Çalışma alanı oluşturmak için zaman biraz sürdüğünü. Çalışma alanı oluşturulurken, **Azure Databricks için dağıtım gönderiliyor** kutucuk sağ tarafında görünür. Başlığını görmek için panonuzu sağa kaydırarak gerekebilir. Ekranın görünür bir ilerleme çubuğu de mevcuttur. İlerleme durumu için her iki alanı da izleyebilirsiniz.

    ![Databricks dağıtım kutucuğu](./media/data-lake-storage-quickstart-create-databricks-account/databricks-deployment-tile.png "Databricks dağıtım kutucuğu")

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Yeni** > **Küme**'yi seçin.

    ![Azure’da Databricks](./media/data-lake-storage-quickstart-create-databricks-account/databricks-on-azure.png "Databricks on Azure")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure’da Databricks Spark kümesi oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

    Aşağıdakiler dışında diğer tüm varsayılan değerleri kabul edin:

    * Küme için bir ad girin.
    * Bir küme oluşturun **5.1** çalışma zamanı.
    * **120 dakika işlem yapılmadığında sonlandır** onay kutusunu seçtiğinizden emin olun. Küme kullanılmazsa kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.

4. **Küme oluştur**’u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

Küme oluşturma hakkında daha fazla bilgi için bkz. [Azure Databricks üzerinde Spark kümesi oluşturma](https://docs.azuredatabricks.net/user-guide/clusters/create.html).

## <a name="create-storage-account-file-system"></a>Depolama hesabı dosya sistemi oluşturma

Bu bölümde, Azure Databricks çalışma alanında bir not defteri oluşturacak ve ardından depolama hesabını yapılandırmak için kod parçacıklarını çalıştıracaksınız.

1. [Azure portalında](https://portal.azure.com), oluşturduğunuz Azure Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Sol bölmede **Çalışma Alanı**’nı seçin. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks’te not defteri oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/databricks-create-notebook.png "Create notebook in Databricks")

3. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Dil olarak **Scala**’yı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Databricks’te not defteri oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/databricks-notebook-details.png "Create notebook in Databricks")

    **Oluştur**’u seçin.

4. Kopyala ve ilk hücreye aşağıdaki kod bloğu yapıştırın, ancak bu kodun henüz çalışmıyor.

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

    > [!NOTE]
    > Data Lake Gen2 uç noktası doğrudan erişir OAuth kullanarak bu kod bloğu, ancak Databricks çalışma alanı, Data Lake depolama Gen2 hesabınıza bağlanmak için farklı yöntemleri vardır. Örneğin, OAuth kullanarak dosya sistemini bağlamalarına veya paylaşılan anahtar ile doğrudan bir erişim kullanın. <br>Bu yaklaşımların örneklerini görmek için bkz: [Azure Data Lake depolama Gen2](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html) makale Azure Databricks Web sitesinde.

5. Bu kod bloğunda değiştirin `storage-account-name`, `application-id`, `authentication-id`, ve `tenant-id` Bu kod bloğu içinde yer tutucu değerlerini, hizmet sorumlusu oluştururken, toplanan değerlere sahip. Ayarlama `file-system-name` örneğin adı için yer tutucu değerini istediğiniz dosya sistemi sağlar.

    > [!NOTE]
    > Bir üretim ayarında, Azure Databricks'te, kimlik doğrulama anahtarı depolamayı düşünün. Ardından, kimlik doğrulama anahtarı yerine, kod bloğu için bir arama anahtarı ekleyin. Bu hızlı başlangıcı tamamladıktan sonra bkz [Azure Data Lake depolama Gen2](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html) makalede bu yaklaşım örneklerini görmek için Azure Databricks Web sitesinde.

6. Tuşuna **SHIFT + ENTER** bu blok kodu çalıştırmak için anahtarları.

## <a name="ingest-sample-data"></a>Örnek verileri ekleme

Bu bölüme başlamadan önce aşağıdaki önkoşulları tamamlamanız gerekir:

Aşağıdaki kodu bir not defteri hücresine girin:

    %sh wget -P /tmp https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json

Hücre içine basın **SHIFT + ENTER** kodu çalıştırmak için.

Şimdi bunun altında yeni bir hücreye aşağıdaki kodu girin ve daha önce kullandığınız aynı değerlerle köşeli ayraçlar içindeki görülen değerleri değiştirin:

    dbutils.fs.cp("file:///tmp/small_radio_json.json", "abfss://<file-system>@<account-name>.dfs.core.windows.net/")

Hücre içine basın **SHIFT + ENTER** kodu çalıştırmak için.

## <a name="run-a-spark-sql-job"></a>Spark SQL işi çalıştırma

Verilerde bir Spark SQL işi çalıştırmak için aşağıdaki görevleri gerçekleştirin.

1. **small_radio_json.json** adlı örnek JSON veri dosyasındaki verileri kullanarak geçici tablo oluşturmak için bir SQL deyimi çalıştırın. Aşağıdaki kod parçacığında yer tutucu değerlerini dosya sisteminizin adı ve depolama hesabı adı ile değiştirin. Daha önceden oluşturduğunuz not defterini kullanarak kod parçacığını not defterindeki yeni bir kod hücresine yapıştırın ve sonra SHIFT + ENTER tuşlarına basın.

    ```sql
    %sql
    DROP TABLE IF EXISTS radio_sample_data;
    CREATE TABLE radio_sample_data
    USING json
    OPTIONS (
     path  "abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/<PATH>/small_radio_json.json"
    )
    ```

    Komut başarıyla tamamlandıktan sonra, JSON dosyasındaki tüm verileri Databricks kümesinde tablo olarak görüntüleyebilirsiniz.

    `%sql` dili sihirli komutu, not defteri başka bir türde olsa bile not defterinden bir SQL kodu çalıştırmanızı sağlar. Daha fazla bilgi için bkz. [Bir not defterinde dilleri karıştırma](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook).

2. Çalıştırdığınız sorguyu daha iyi anlamak için örnek JSON verilerinin bir anlık görüntüsüne bakalım. Kod hücresine aşağıdaki kod parçacığını yapıştırın ve **SHIFT + ENTER** tuşuna basın.

    ```sql
    %sql
    SELECT * from radio_sample_data
    ```

3. Aşağıdaki ekran görüntüsünde gösterildiği gibi bir tablo çıktısı görürsünüz (yalnızca bazı sütunlar gösterilmiştir):

    ![Örnek JSON verileri](./media/data-lake-storage-quickstart-create-databricks-account/databricks-sample-csv-data.png "Sample JSON data")

    Diğer ayrıntılara ek olarak, örnek veriler bir radyo kanalının dinleyicilerinin yakalar (sütun adı **cinsiyet**) ve ücretsiz veya Ücretli aboneliğini olup (sütun adı **düzeyi**).

4. Bu durumda, bu verilerin her bir cinsiyet, ücretsiz hesaba sahip kullanıcı sayısı ve ücretli hesabı olan abone sayısı için gösterilecek görsel bir açıklamasını oluşturursunuz. Tablo çıktısının alt kısmında bulunan **Çubuk grafik** simgesine ve sonra **Çizim Seçenekleri**’ne tıklayın.

    ![Çubuk grafik oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/create-plots-databricks-notebook.png "Create bar chart")

5. **Çizimi Özelleştir** menüsünde, değerleri ekran görüntüsünde gösterilen şekilde sürükleyip bırakın.

    ![Çubuk grafiği özelleştirme](./media/data-lake-storage-quickstart-create-databricks-account/databricks-notebook-customize-plot.png "Customize bar chart")

    - **Anahtarlar**’ı **cinsiyet** olarak ayarlayın.
    - **Seri gruplandırmalar**’ı **düzey** olarak ayarlayın.
    - **Değerler**’ı **düzey** olarak ayarlayın.
    - **Toplama**’yı **SAYI** olarak ayarlayın.

6. **Uygula**'ya tıklayın.

7. Çıktı aşağıdaki ekran görüntüsünde gösterildiği gibi görsel açıklamayı gösterir:

     ![Çubuk grafiği özelleştirme](./media/data-lake-storage-quickstart-create-databricks-account/databricks-sql-query-output-bar-chart.png "Customize bar chart")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makaleyle bitirdikten sonra kümeyi sonlandırabilirsiniz. Azure Databricks çalışma alanında **Kümeler**'i seçin ve sonlandırmak istediğiniz kümeyi bulun. Fare imlecini **Eylemler** sütununun altındaki üç noktanın üzerine götürün ve **Sonlandır** simgesini seçin.

![Databricks kümesini durdurma](./media/data-lake-storage-quickstart-create-databricks-account/terminate-databricks-cluster.png "Databricks kümesini durdurma")

Kümeyi el ile sonlandırmazsanız seçtiğiniz sağlanan bunu otomatik olarak durdurulur **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** küme oluşturulurken onay kutusu. Bu seçeneği belirlerseniz küme belirtilen zaman boyunca devre dışı olması halinde durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Databricks’te bir Spark kümesi oluşturdunuz ve Data Lake Storage 2. Nesil etkin bir depolama hesabındaki verileri kullanarak bir Spark işi çalıştırdınız. Diğer veri kaynaklarından Azure Databricks’e verileri aktarma hakkında bilgi almak için [Spark veri kaynakları](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html) bölümüne de bakabilirsiniz. Azure Databricks kullanılarak bir ETL işleminin (verileri ayıklama, dönüştürme ve yükleme) nasıl gerçekleştirileceğini öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme](../../azure-databricks/databricks-extract-load-sql-data-warehouse.md)
