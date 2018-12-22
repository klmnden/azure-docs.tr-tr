---
title: "Hızlı Başlangıç: Azure portalını kullanarak Azure Databricks'te bir Spark işi çalıştırma | Microsoft Docs"
description: Bu hızlı başlangıçta Azure portalını kullanarak bir Azure Databricks çalışma alanı, bir Apache Spark kümesi oluşturma ve bir Spark işi çalıştırma işlemi gösterilmektedir.
services: storage
author: jamesbak
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 12/06/2018
ms.openlocfilehash: c820d2172c3e38d9d744e645d7c0e8b4749b42cd
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53743383"
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure Databricks'te Spark işini çalıştırma

Bu hızlı başlangıçta Data Lake Storage 2. Nesil Önizleme sürümü etkin bir depolama hesabında depolanan verileri analiz etmek için Azure Databricks kullanarak bir Apache Spark işini çalıştırmayı öğreneceksiniz.

Spark işinin parçası olarak, demografiye dayalı ücretsiz/ücretli kullanıma yönelik öngörüler elde etmek için bir radyo kanalının abonelik verilerini analiz edeceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

- [Data Lake Storage 2. Nesil etkin bir depolama hesabı oluşturma](data-lake-storage-quickstart-create-account.md)

## <a name="set-aside-storage-account-configuration"></a>Depolama hesabı yapılandırmasını not alın

> [!IMPORTANT]
> Bu öğretici sırasında depolama hesabı adına ve erişim anahtarına erişim sahibi olmanız gerekir. Azure portalında **Tüm Hizmetler**'i seçin ve *depolama* ölçütüne göre filtreleyin. **Depolama hesapları**'nı seçip bu öğretici için oluşturduğunuz hesabı bulun.
>
> **Genel bakış** bölümünden depolama hesabının **adını** bir metin düzenleyiciye kopyalayın. Ardından **Erişim anahtarları**'nı seçin ve **key1** değerini metin düzenleyicisine yapıştırın. Bu değerlerin ikisini de ilerleyen bölümlerdeki komutlarda kullanmanız gerekecek.

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

3. Çalışma alanının oluşturulması birkaç dakika sürer. Çalışma alanı oluşturma sırasında portal sağ tarafta **Azure Databricks için dağıtım gönderiliyor** kutucuğunu gösterir. Kutucuğu görmek için panonuzu sağa kaydırmanız gerekebilir. Ayrıca, ekranın üst kısmında gösterilen bir ilerleme çubuğu vardır. İlerleme durumu için her iki alanı da izleyebilirsiniz.

    ![Databricks dağıtım kutucuğu](./media/data-lake-storage-quickstart-create-databricks-account/databricks-deployment-tile.png "Databricks dağıtım kutucuğu")

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Yeni** > **Küme**'yi seçin.

    ![Azure’da Databricks](./media/data-lake-storage-quickstart-create-databricks-account/databricks-on-azure.png "Databricks on Azure")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure’da Databricks Spark kümesi oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

    Aşağıdakiler dışında diğer tüm varsayılan değerleri kabul edin:

    * Küme için bir ad girin.
    * Bir küme oluşturun **5.1 beta** çalışma zamanı.
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

4. Databricks çalışma alanı ADLS Gen2 hesabınıza bağlanın. Bunu elde etmek için kullanabileceğiniz üç desteklenen mekanizma vardır: paylaşılan anahtar ile OAuth ve OAuth ile doğrudan erişim doğrudan erişim kullanan bağlama. 

    Her mekanizması, bir örnek gösterilir. Örnek sınanırken, gösterilen örnekte kendi değerlerinizle köşeli ayraçlar içindeki yer tutucuları değiştirmeniz unutmayın:

    **OAuth kullanarak bağlayın**     
        
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

    **OAuth ile doğrudan erişim**

    ```scala
    spark.conf.set("fs.azure.account.auth.type.<account-name>.dfs.core.windows.net": "OAuth")
    spark.conf.set("fs.azure.account.oauth.provider.type.<account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
    spark.conf.set("fs.azure.account.oauth2.client.id.<account-name>.dfs.core.windows.net": "<service-client-id>")
    spark.conf.set("fs.azure.account.oauth2.client.secret.<account-name>.dfs.core.windows.net": "<service-credentials>")
    spark.conf.set("fs.azure.account.oauth2.client.endpoint.<account-name>.dfs.core.windows.net": "https://login.microsoftonline.com/<tenant-id>/oauth2/token")

    dbutils.fs.ls("abfss://<file-system-name>@<account-name>.dfs.core.windows.net/")
    ```
        
    **Paylaşılan anahtar ile doğrudan erişim** 

    ```scala    
    spark.conf.set("fs.azure.account.key.<account-name>.dfs.core.windows.net", "<account-key>")

    dbutils.fs.ls("abfss://<file-system-name>@<account-name>.dfs.core.windows.net/")
    ```

5. Kod ilk hücrenin ve enter tuşuna basın **SHIFT + ENTER** çalıştırmak için.

Depolama hesabı için dosya sistemi oluşturulmuş olur.

## <a name="ingest-sample-data"></a>Örnek verileri ekleme

Bu bölüme başlamadan önce aşağıdaki önkoşulları tamamlamanız gerekir:

Aşağıdaki kodu bir not defteri hücresine girin:

    %sh wget -P /tmp https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json

Hücre içine basın **SHIFT + ENTER** kodu çalıştırmak için.

Şimdi bunun altındaki yeni bir hücreye köşeli ayraçlardaki değerler, daha önce kullandığınız aynı değerlerle değiştirerek aşağıdaki kodu girin:

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
     path  "abfss://<file-system-name>@<account-name>.dfs.core.windows.net/<PATH>/small_radio_json.json"
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

Bu makaleyi tamamladıktan sonra kümeyi sonlandırabilirsiniz. Azure Databricks çalışma alanında **Kümeler**'i seçin ve sonlandırmak istediğiniz kümeyi bulun. Fare imlecini **Eylemler** sütununun altındaki üç noktanın üzerine götürün ve **Sonlandır** simgesini seçin.

![Databricks kümesini durdurma](./media/data-lake-storage-quickstart-create-databricks-account/terminate-databricks-cluster.png "Databricks kümesini durdurma")

Kümeyi el ile sonlandırmazsanız seçtiğiniz sağlanan bunu otomatik olarak durdurulur **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** küme oluşturulurken onay kutusu. Bu seçeneği belirlerseniz küme belirtilen zaman boyunca devre dışı olması halinde durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Databricks’te bir Spark kümesi oluşturdunuz ve Data Lake Storage 2. Nesil etkin bir depolama hesabındaki verileri kullanarak bir Spark işi çalıştırdınız. Diğer veri kaynaklarından Azure Databricks’e verileri aktarma hakkında bilgi almak için [Spark veri kaynakları](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html) bölümüne de bakabilirsiniz. Azure Databricks kullanılarak bir ETL işleminin (verileri ayıklama, dönüştürme ve yükleme) nasıl gerçekleştirileceğini öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
>[Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme](../../azure-databricks/databricks-extract-load-sql-data-warehouse.md)
