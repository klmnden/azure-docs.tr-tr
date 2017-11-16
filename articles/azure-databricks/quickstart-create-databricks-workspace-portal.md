---
title: "Hızlı Başlangıç: bir ilk Spark Azure Azure portalını kullanarak Databricks üzerinde çalıştırın | Microsoft Docs"
description: "Hızlı Başlangıç Azure portalında bir Azure Databricks çalışma alanında, bir Apache Spark kümesi oluşturma ve Spark işi çalıştırmak için nasıl kullanılacağını gösterir."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: nitinme
ms.openlocfilehash: d384a1aef89941c2c9b547e5e0d05bb562578393
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-portal"></a>Hızlı Başlangıç:, Azure portalını kullanarak Azure Databricks üzerinde Spark işini çalıştır

Bu hızlı başlangıç Azure Databricks çalışma ve bu çalışma alanı içindeki bir Apache Spark kümesi nasıl oluşturulacağını gösterir. Son olarak, Spark iş Databricks küme üzerinde çalışacak şekilde nasıl öğrenin. Azure Databricks hakkında daha fazla bilgi için bkz: [Azure Databricks nedir?](what-is-azure-databricks.md)

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

Oturum [Azure portal](https://portal.azure.com).

## <a name="create-a-databricks-workspace"></a>Databricks çalışma alanı oluşturma

Bu bölümde Azure Portalı'nı kullanarak bir Azure Databricks çalışma alanı oluşturun. 

1. Azure portalında tıklatın  **+** , tıklatın **veri + analiz**ve ardından **Azure Databricks (Önizleme)**. 

    ![Azure Portal'da Databricks](./media/quickstart-create-databricks-workspace-portal/azure-databricks-on-portal.png "Databricks Azure portalında")

2. Altında **Azure Databricks (Önizleme)**, tıklatın **oluşturma**.

    > [!NOTE]
    > Azure Databricks şu anda sınırlı önizlemede değil. Uygulamaları güvenilir listeye almayı Önizleme için kabul edilmesi için Azure aboneliğinizin istiyorsanız, doldurduğunuz gerekir [kayıt formu](https://databricks.azurewebsites.net/).

2. Altında **Azure Databricks hizmet**, aşağıdaki değerleri girin:

    ![Bir Azure Databricks çalışma alanı oluşturma](./media/quickstart-create-databricks-workspace-portal/create-databricks-workspace.png "Azure Databricks çalışma alanı oluşturma")

    * İçin **çalışma alanı adı**, Databricks çalışma alanınız için bir ad sağlayın.
    * İçin **abonelik**, açılan listeden, Azure aboneliğinizi seçin.
    * İçin **kaynak grubu**, yeni bir kaynak grubu oluşturmak veya mevcut bir kullanmak isteyip istemediğinizi belirtin. Bir kaynak grubu Azure çözümünü ilgili kaynaklara tutan bir kapsayıcıdır. Daha fazla bilgi için bkz: [Azure kaynak grubu genel bakış](../azure-resource-manager/resource-group-overview.md).
    * İçin **konumu**seçin **Doğu ABD 2**. Kullanılabilir diğer bölgeler için bkz: [kullanılabilir bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/services/).

3. **Oluştur**'a tıklayın.

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks içinde bir Spark kümesi oluşturma

1. Azure portalında oluşturduğunuz Databricks çalışma alanına gidin ve ardından **başlatma çalışma**.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan tıklatın **küme**.

    ![Azure üzerinde Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-on-azure.png "Databricks Azure ile ilgili")

3. İçinde **yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure üzerinde Databricks Spark küme oluştur](./media/quickstart-create-databricks-workspace-portal/create-databricks-spark-cluster.png "Azure oluşturmak Databricks Spark kümesinde")

    * Küme için bir ad girin.
    * Seçtiğinizden emin olun **Sonlandır etkinliği ___ dakika sonra** onay kutusu. Küme olmayan kullanılıyorsa, küme sonlandırmak için bir süre (dakika cinsinden) sağlayın.
    * Tüm diğer varsayılan değerleri kabul edin. 
    * Tıklatın **küme oluştur**. Kümenin çalışmaya başladıktan sonra dizüstü bilgisayarlar kümeye ekleyin ve Spark işleri çalıştırma.

Kümeleri oluşturma hakkında daha fazla bilgi için bkz: [Azure Databricks bir Spark kümesi oluşturma](https://docs.azuredatabricks.net/user-guide/clusters/create.html).

## <a name="run-a-spark-sql-job"></a>Spark SQL işini çalıştır

Bu bölümde ile başlamadan önce aşağıdakileri tamamlamanız gerekir:

* [Bir Azure depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account). 
* Örnek JSON dosyası indirmeniz [github'dan](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json). 
* Örnek JSON dosyasını karşıya yükleyin, oluşturduğunuz Azure depolama hesabı. Kullanabileceğiniz [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) dosyaları karşıya yüklemek için.

Databricks içinde bir not defteri oluşturun, dizüstü bilgisayarın bir Azure Blob Depolama hesabından veri okuma yapılandırın ve ardından Spark SQL verileri çalıştırın için aşağıdaki adımları gerçekleştirin.

1. Sol bölmede **çalışma**. Gelen **çalışma** açılır menüsünde tıklatın **oluşturma**ve ardından **not defteri**.

    ![Create not defterinde Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-create-notebook.png "Databricks not defteri oluşturma")

2. İçinde **not defteri oluşturma** iletişim kutusunda, bir ad girin, seçin **Scala** dili ve daha önce oluşturduğunuz Spark kümesi seçin.

    ![Create not defterinde Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-details.png "Databricks not defteri oluşturma")

    **Oluştur**'a tıklayın.

3. Aşağıdaki kod parçacığında, yerini `{YOUR STORAGE ACCOUNT NAME}` Azure depolama hesabı adı ile oluşturduğunuz ve `{YOUR STORAGE ACCOUNT ACCESS KEY}` depolama hesabının erişim anahtarı ile. Kod parçacığını not defteri boş bir hücreye yapıştırın ve ardından kod hücresini çalıştırmak için SHIFT + ENTER tuşuna basın. Bu kod parçacığında, bir Azure blob depolama alanından verileri okumak için Not Defteri yapılandırır.

       spark.conf.set("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net", "{YOUR STORAGE ACCOUNT ACCESS KEY}")
    
    Depolama hesabı anahtarı alma hakkında daha fazla yönerge için bkz: [depolama erişim tuşlarınızı yönetme](../storage/common/storage-create-storage-account.md#manage-your-storage-account)

    > [!NOTE]
    > Azure Data Lake Store, Azure Databricks Spark kümesinde ile de kullanabilirsiniz. Yönergeler için bkz: [kullanım Data Lake Store ile Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store).

4. Örnek JSON veri dosyasından veri kullanarak geçici bir tablo oluşturmak için bir SQL deyimini çalıştırın **small_radio_json.json**. Aşağıdaki kod parçacığında, yer tutucu değerlerini kapsayıcı adı ve depolama hesabı adı ile değiştirin. Not Defteri kod hücresinde parçacığını yapıştırın ve sonra SHIFT + ENTER tuşuna basın. Parçacığında bulunan `path` , Azure Storage hesabınıza yüklediğiniz örnek JSON dosyasının konumunu gösterir.

    ```sql
    %sql 
    CREATE TEMPORARY TABLE radio_sample_data
    USING json
    OPTIONS (
     path "wasbs://{YOUR CONTAINER NAME}@{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net/small_radio_json.json"
    )
    ```

    Komut başarıyla tamamlandığında, tablo Databricks küme olarak JSON dosyasından tüm verilere sahip.

    `%sql` Dil Sihirli komut etkinleştirir, dizüstü bilgisayarınızı, bir SQL kodu çalıştırmak için Not Defteri başka türde olsa bile. Daha fazla bilgi için bkz: [bir not defteri dillerde karıştırma](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook).

5. Verilerin bir anlık görüntüsünü çalıştıracağımız sorguyu daha iyi anlamak için örnek JSON bakalım. Kod hücresini ve tuşuna aşağıdaki kod parçacığını yapıştırın **SHIFT + ENTER**.

    ```sql
    %sql 
    SELECT * from radio_sample_data
    ```

6. (Yalnızca bazı sütunları gösterilir) aşağıdaki ekran görüntüsünde gösterildiği gibi tablolu bir çıktı bakın:

    ![Örnek JSON veri](./media/quickstart-create-databricks-workspace-portal/databricks-sample-csv-data.png "örnek JSON verileri")

    Diğer Ayrıntılar arasında radyo kanal İzleyici cinsiyetiniz örnek verileri yakalar (sütun adı, **cinsiyetiniz**) ve aboneliğini ücretsiz veya Ücretli mi (sütun adı, **düzeyi**).

7. Şimdi bu verileri her cinsiyetiniz için Göster, boş hesapları kaç kullanıcınız ve aboneleri kaç ödenen görsel gösterimi yarat Tablo çıktısı aşağıdan tıklatın **çubuk grafik** simgesine ve ardından **Çizim Seçenekleri**.

    ![Çubuk grafik oluşturma](./media/quickstart-create-databricks-workspace-portal/create-plots-databricks-notebook.png "çubuk grafik oluşturma")

8. İçinde **özelleştirme çizim**, sürükle ve bırak değerleri ekran görüntüsünde gösterildiği gibi.

    ![Çubuk grafiği özelleştirme](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-customize-plot.png "çubuk grafiği özelleştirme")

    * Ayarlama **anahtarları** için **cinsiyetiniz**.
    * Ayarlama **seri gruplandırmaları** için **düzeyi**.
    * Ayarlama **değerleri** için **düzeyi**.
    * Ayarlama **toplama** için **sayısı**.

    **Uygula**'ya tıklayın.

9. Çıktı aşağıdaki ekran görüntüsünde gösterildiği gibi görsel gösterimi gösterir:

     ![Çubuk grafiği özelleştirme](./media/quickstart-create-databricks-workspace-portal/databricks-sql-query-output-bar-chart.png "çubuk grafiği özelleştirme")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Onay kutusunu seçtiyseniz, Spark kümesi oluşturma sırasında **Sonlandır etkinliği ___ dakika sonra**, belirtilen süre boyunca etkin olması durumunda kümeye otomatik olarak sona erdirir.

Onay kutusunu seçmediyseniz, küme el ile sonlanır gerekir. Bunu, sol bölmeden Azure Databricks çalışma alanından yapmak için tıklatın **kümeleri**. Altında üç nokta üzerinden sonlandırmak istediğinizden küme için imleci taşıma **Eylemler** sütun ve tıklatın **Sonlandır** simgesi.

![Sonlandırma Databricks küme](./media/quickstart-create-databricks-workspace-portal/terminate-databricks-cluster.png "sonlandırmak Databricks küme")

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Databricks bir Spark kümesi oluşturulur ve verileri Azure depolama alanında kullanarak Spark işi çalıştı. Ayrıca bakabilir [Spark veri kaynakları](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html) Azure Databricks diğer veri kaynaklarından veri içeri aktarma öğrenin. Azure Data Lake Store ile Azure Databricks kullanmayı öğrenmek için sonraki makalede ilerleyin.

> [!div class="nextstepaction"]
>[Kullanım Data Lake Store ile Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store)