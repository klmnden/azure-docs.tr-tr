---
title: "Hızlı Başlangıç: Azure portalını kullanarak Databricks üzerinde bir Spark işi çalıştırma | Microsoft Docs"
description: "Bu hızlı başlangıçta Azure portalını kullanarak bir Azure Databricks çalışma alanı, bir Apache Spark kümesi oluşturma ve bir Spark işi çalıştırma işlemi gösterilmektedir."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/22/2018
ms.author: nitinme
ms.custom: mvc
ms.openlocfilehash: c471baa287c3a51e9787cc2103b23c2bab458db2
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Databricks üzerinde bir Spark işi çalıştırma

Bu hızlı başlangıçta bir Azure Databricks çalışma alanı ve bu çalışma alanı içinde bir Apache Spark kümesi oluşturma işlemi gösterilir. Son olarak, Databricks kümesinde bir Spark işi çalıştırma hakkında bilgi edinirsiniz. Azure Databricks hakkında daha fazla bilgi için bkz. [Azure Databricks nedir?](what-is-azure-databricks.md)

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure portalı](https://portal.azure.com)’nda oturum açın.

## <a name="create-a-databricks-workspace"></a>Databricks çalışma alanı oluşturma

Bu bölümde Azure portalını kullanarak bir Azure Databricks çalışma alanı oluşturursunuz. 

1. Azure portalında **+** öğesine, **Veri ve Analiz**’e ve ardından **Azure Databricks (Önizleme)** öğesine tıklayın. 

    ![Azure portalında Databricks](./media/quickstart-create-databricks-workspace-portal/azure-databricks-on-portal.png "Databricks on Azure portal")

2. **Azure Databricks (Önizleme)** altında **Oluştur**’a tıklayın.

3. **Azure Databricks Hizmeti** altında aşağıdaki değerleri sağlayın:

    ![Azure Databricks çalışma alanı oluşturma](./media/quickstart-create-databricks-workspace-portal/create-databricks-workspace.png "Create an Azure Databricks workspace")

    * **Çalışma alanı adı** altında Databricks çalışma alanınız için bir ad sağlayın.
    * **Abonelik** için açılan listeden Azure aboneliğinizi seçin.
    * **Kaynak grubu** için yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../azure-resource-manager/resource-group-overview.md).
    * **Konum** için **Doğu ABD 2**’yi seçin. Kullanılabilir diğer bölgeler için bkz. [Bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/).

4. **Oluştur**’a tıklayın.

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’a tıklayın.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Küme**’ye tıklayın.

    ![Azure’da Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-on-azure.png "Databricks on Azure")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure’da Databricks Spark kümesi oluşturma](./media/quickstart-create-databricks-workspace-portal/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

    * Küme için bir ad girin.
    * **___ dakika işlem yapılmadığında sonlandır** onay kutusunu seçtiğinizden emin olun. Küme kullanılmazsa kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.
    * Diğer tüm varsayılan değerleri kabul edin. 
    * **Küme oluştur**’a tıklayın. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

Küme oluşturma hakkında daha fazla bilgi için bkz. [Azure Databricks üzerinde Spark kümesi oluşturma](https://docs.azuredatabricks.net/user-guide/clusters/create.html).

## <a name="run-a-spark-sql-job"></a>Spark SQL işi çalıştırma

Bu bölüme başlamadan önce aşağıdakileri tamamlamanız gerekir:

* [Bir Azure depolama hesabı oluşturun](../storage/common/storage-create-storage-account.md#create-a-storage-account). 
* Örnek JSON dosyasını [Github'dan](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json) indirin. 
* Örnek JSON dosyasını, oluşturduğunuz Azure depolama hesabına yükleyin. Dosyaları karşıya yüklemek için [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md)’ni kullanabilirsiniz.

Databricks içinde bir not defteri oluşturmak, not defterini bir Azure Blob depolama hesabındaki verileri okuyacak şekilde yapılandırmak ve sonra veriler üzerinde bir Spark SQL işi çalıştırmak için aşağıdaki adımları gerçekleştirin.

1. Sol bölmedeki **Çalışma Alanı**'na tıklayın. **Çalışma Alanı** açılır listesinden **Oluştur**’a ve sonra **Not Defteri**’ne tıklayın.

    ![Databricks’te not defteri oluşturma](./media/quickstart-create-databricks-workspace-portal/databricks-create-notebook.png "Create notebook in Databricks")

2. **Not Defteri Oluşturma** iletişim kutusuna bir ad girin, dil olarak **Scala**’yı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Databricks’te not defteri oluşturma](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-details.png "Create notebook in Databricks")

    **Oluştur**’a tıklayın.

3. Aşağıdaki kod parçacığında `{YOUR STORAGE ACCOUNT NAME}` değerini oluşturduğunuz Azure depolama hesabı adı ile, `{YOUR STORAGE ACCOUNT ACCESS KEY}` değerini ise depolama hesabı erişim anahtarınız ile değiştirin. Kod parçacığını not defterindeki boş bir hücreye yapıştırın ve sonra kod hücresini çalıştırmak için SHIFT + ENTER tuşlarına basın. Bu kod parçacığı, not defterini bir Azure blob depolama alanından verileri okuyacak şekilde yapılandırır.

       spark.conf.set("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net", "{YOUR STORAGE ACCOUNT ACCESS KEY}")
    
    Depolama hesabı anahtarınızı almaya ilişkin yönergeler için bkz. [Depolama erişim anahtarlarınızı yönetme](../storage/common/storage-create-storage-account.md#manage-your-storage-account)

    > [!NOTE]
    > Azure Data Lake Store’u Azure Databricks üzerine bir Spark kümesi ile de kullanabilirsiniz. Yönergeler için bkz. [Data Lake Store’u Azure Databricks ile Kullanma](https://go.microsoft.com/fwlink/?linkid=864084).

4. **small_radio_json.json** adlı örnek JSON veri dosyasındaki verileri kullanarak geçici tablo oluşturmak için bir SQL deyimi çalıştırın. Aşağıdaki kod parçacığında yer tutucu değerlerini kapsayıcınızın adı ve depolama hesabı adı ile değiştirin. Kod parçacığını not defterindeki bir kod hücresine yapıştırın ve sonra SHIFT + ENTER tuşlarına basın. Kod parçacığında `path` değeri, Azure Depolama hesabınıza yüklediğiniz örnek JSON dosyasının konumunu gösterir.

    ```sql
    %sql 
    CREATE TEMPORARY TABLE radio_sample_data
    USING json
    OPTIONS (
     path "wasbs://{YOUR CONTAINER NAME}@{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net/small_radio_json.json"
    )
    ```

    Komut başarıyla tamamlandıktan sonra, JSON dosyasındaki tüm verileri Databricks kümesinde tablo olarak görüntüleyebilirsiniz.

    `%sql` dili sihirli komutu, not defteri başka bir türde olsa bile not defterinden bir SQL kodu çalıştırmanızı sağlar. Daha fazla bilgi için bkz. [Bir not defterinde dilleri karıştırma](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook).

5. Çalıştırdığımız sorguyu daha iyi anlamak için örnek JSON verilerinin bir anlık görüntüsüne bakalım. Kod hücresine aşağıdaki kod parçacığını yapıştırın ve **SHIFT + ENTER** tuşuna basın.

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

Spark kümesini oluştururken **___ dakika işlem yapılmadığında sonlandır** onay kutusunu seçtiyseniz, küme belirtilen süre boyunca etkin olmazsa otomatik olarak sona erer.

Onay kutusunu seçmediyseniz kümeyi el ile sonlandırmanız gerekir. Bunu yapmak için Azure Databricks çalışma alanında sol bölmedeki **Kümeler**’e tıklayın. Sonlandırmak istediğiniz küme için imleci **Eylemler** sütunu altındaki üç noktanın üzerine taşıyın ve **Sonlandır** simgesine tıklayın.

![Databricks kümesini sonlandırma](./media/quickstart-create-databricks-workspace-portal/terminate-databricks-cluster.png "Terminate Databricks cluster")

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Databricks’te bir Spark kümesi oluşturdunuz ve Azure depolama alanındaki verileri kullanarak bir Spark işi çalıştırdınız. Diğer veri kaynaklarından Azure Databricks’e verileri aktarma hakkında bilgi almak için [Spark veri kaynakları](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html) bölümüne de bakabilirsiniz. Azure Data Lake Store’u Azure Databricks ile kullanma hakkında bilgi için sonraki makaleye geçin.

> [!div class="nextstepaction"]
>[Data Lake Store’u Azure Databricks ile Kullanma](https://go.microsoft.com/fwlink/?linkid=864084)
