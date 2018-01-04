---
title: "Data Lake Store ile Azure Hdınsight kümeleri oluşturmak için Azure portalını kullanma | Microsoft Docs"
description: "Oluşturma ve Hdınsight kümeleri Azure Data Lake Store ile kullanmak için Azure portalını kullanın"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: a8c45a83-a8e3-4227-8b02-1bc1e1de6767
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 4a27ae1402717c91029eda9d635db124f8bb6b8d
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-by-using-the-azure-portal"></a>Azure portalını kullanarak Data Lake Store ile Hdınsight kümeleri oluşturma
> [!div class="op_single_selector"]
> * [Azure portal’ı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [(Varsayılan depolama için) PowerShell kullanma](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell (için ek depolama alanı) kullanın](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisi'ni kullanın](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Varsayılan depolama veya ek depolama alanı olarak bir Hdınsight kümesi ile bir Azure Data Lake Store hesabı oluşturmak için Azure portalını kullanmayı öğrenin. Ek depolama alanı bir Hdınsight kümesi için isteğe bağlı olsa da, ek depolama hesapları, iş verilerinizi depolamak için önerilir.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdaki gereksinimleri yerine getirdiğinizi emin olun:

* **Bir Azure aboneliği**. Git [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake Store hesabı**. Makalesindeki yönergeleri izleyin [Azure portalını kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md). Ayrıca hesabı kök klasör oluşturmanız gerekir.  Bu öğreticide, bir kök klasör adında __/kümeleri__ kullanılır.
* **Bir Azure Active Directory hizmet asıl**. Bu öğretici, bir hizmet asıl Azure Active Directory'de (Azure AD) oluşturma hakkında yönergeler sağlar. Ancak, bir hizmet sorumlusu oluşturmak için Azure AD yönetici olmanız gerekir. Bir yöneticiyseniz, bu önkoşulu atlamak ve bu öğreticiyi devam edin.

    >[!NOTE]
    >Yalnızca, Azure AD yöneticisiyseniz, bir hizmet asıl oluşturabilirsiniz. Azure AD yöneticinizin Data Lake Store ile Hdınsight kümesi oluşturmadan önce bir hizmet asıl oluşturmanız gerekir. Ayrıca, hizmet sorumlusu bir sertifikayla konusunda açıklandığı gibi oluşturulmalıdır [sertifikayla bir hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).
    >

## <a name="create-an-hdinsight-cluster"></a>Hdınsight kümesi oluşturma

Bu bölümde, varsayılan veya ek depolama alanı olarak Data Lake Store hesapları olan bir Hdınsight kümesi oluşturun. Bu makalede, Data Lake Store hesaplarını yapılandırma bölümü yalnızca odaklanır.  Genel küme oluşturma bilgiler ve yordamlar için bkz: [Hdınsight'ta oluşturmak Hadoop kümeleri](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

### <a name="create-a-cluster-with-data-lake-store-as-default-storage"></a>Data Lake Store ile varsayılan depolama alanı olarak bir küme oluşturma

**Varsayılan depolama hesabı olarak bir Data Lake Store ile bir Hdınsight kümesi oluşturmak için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. İzleyin [küme oluşturma](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) Hdınsight kümeleri oluşturma hakkında genel bilgi için.
3. Üzerinde **depolama** dikey altında **birincil depolama türü**seçin **Data Lake Store**ve ardından aşağıdaki bilgileri girin:

    ![Bir Hdınsight kümesine Ekle hizmet sorumlusu](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "hizmet sorumlusuna Hdınsight kümesi Ekle")

    - **Select Data Lake Store hesabı**: mevcut bir Data Lake Store hesabını seçin. Mevcut bir Data Lake Store hesabı gereklidir.  Bkz: [Önkoşullar](#prereuisites).
    - **Kök yolu**: kümeye özgü dosyaların depolanması olduğu bir yol girin. Ekran görüntüsü üzerinde olduğu __/ / myhdiadlcluster/kümeleri__, burada __/kümeleri__ klasör var olmalıdır ve Portal oluşturur *myhdicluster* klasör.  *Myhdicluster* küme adıdır.
    - **Data Lake Store erişim**: Data Lake Store hesabı ve Hdınsight kümesi arasında erişim yapılandırın. Yönergeler için bkz: [yapılandırma Data Lake Store erişim](#configure-data-lake-store-access).
    - **Ek depolama hesapları**: ek depolama alanı olarak Azure Storage hesapları eklemek için küme hesapları. Ek eklemek için Data Lake depoları küme izinleri daha fazla Data Lake Store hesapları veriler üzerinde bir Data Lake Store hesabı birincil depolama türü olarak yapılandırılırken verilerek yapılır. Bkz. [Data Lake Store erişimini yapılandırma](#configure-data-lake-store-access).

4. Üzerinde **Data Lake Store erişim**, tıklatın **seçin**ve açıklandığı gibi küme oluşturma ile devam et [Hdınsight'ta oluşturmak Hadoop kümeleri](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md).


### <a name="create-a-cluster-with-data-lake-store-as-additional-storage"></a>Data Lake Store ile ek depolama alanı olarak bir küme oluşturma

Aşağıdaki yönergeler, varsayılan depolama alanı olarak Azure Storage hesabı ve ek depolama alanı olarak Data Lake Store hesabı ile Hdınsight kümesi oluşturur.
**Varsayılan depolama hesabı olarak bir Data Lake Store ile bir Hdınsight kümesi oluşturmak için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. İzleyin [küme oluşturma](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) Hdınsight kümeleri oluşturma hakkında genel bilgi için.
3. Üzerinde **depolama** dikey altında **birincil depolama türü**seçin **Azure Storage**ve ardından aşağıdaki bilgileri girin:

    ![Bir Hdınsight kümesine Ekle hizmet sorumlusu](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "hizmet sorumlusuna Hdınsight kümesi Ekle")

    - **Seçim yöntemini**: aşağıdaki seçeneklerden birini kullanın:

        * Azure aboneliğinizin bir parçası olan bir depolama hesabını belirtmek için işaretleyin **My abonelikleri**ve ardından depolama hesabını seçin.
        * Azure aboneliğiniz bir depolama hesabı belirtmek için işaretleyin **erişim tuşu**, dış depolama hesabı bilgilerini girin.

    - **Varsayılan kapsayıcı**: varsayılan değeri kullanın veya kendi adı belirtin.

    - Ek depolama hesapları: ek depolama alanı olarak daha fazla Azure Storage hesaplarını ekleyin.
    - Data Lake Store erişim: Data Lake Store hesabı ve Hdınsight kümesi arasında erişim yapılandırın. Yönergeler için bkz [yapılandırma Data Lake Store erişim](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Data Lake Store erişimi yapılandırma 

Bu bölümde, bir Azure Active Directory hizmet asıl kullanarak Hdınsight kümelerini Data Lake Store erişimden yapılandırın. 

### <a name="specify-a-service-principal"></a>Bir hizmet sorumlusu belirtin

Azure portalından, mevcut bir hizmet sorumlusu kullanabilir veya yeni bir tane oluşturun.

**Azure portalından bir hizmet sorumlusu oluşturmak için**

1. Tıklatın **Data Lake Store erişim** Store dikey penceresinden.
2. Üzerinde **Data Lake Store erişim** dikey penceresinde tıklatın **Yeni Oluştur**.
3. Tıklatın **hizmet sorumlusu**ve ardından bir hizmet sorumlusu oluşturmak için yönergeleri izleyin.
4. Gelecekte tekrar kullanmaya karar verirseniz sertifikayı indirin. Sertifika indirme ek Hdınsight kümeleri oluşturduğunuzda aynı hizmet asıl adı kullanmak istiyorsanız yararlıdır.

    ![Bir Hdınsight kümesine Ekle hizmet sorumlusu](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "hizmet sorumlusuna Hdınsight kümesi Ekle")

4. Tıklatın **erişim** klasör erişimi yapılandırmak için.  Bkz: [dosya izinlerini yapılandırın](#configure-file-permissions).


**Var olan bir hizmet sorumlusu Azure portalından kullanmak için**

1. Tıklatın **Data Lake Store erişim**.
1. Üzerinde **Data Lake Store erişim** dikey penceresinde tıklatın **var olanı kullan**.
2. Tıklatın **hizmet sorumlusu**ve ardından bir hizmet sorumlusu seçin. 
3. Seçili Hizmet sorumlusu ile ilişkili sertifika (.pfx dosyası) karşıya yükleyin ve sonra sertifikanın parolasını girin.

    ![Bir Hdınsight kümesine Ekle hizmet sorumlusu](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "hizmet sorumlusuna Hdınsight kümesi Ekle")

4. Tıklatın **erişim** klasör erişimi yapılandırmak için.  Bkz: [dosya izinlerini yapılandırın](#configure-file-permissions).


### <a name="configure-file-permissions"></a>Dosya izinleri yapılandırma

Yapılandırır hesabının varsayılan depolama veya ek depolama alanı hesabı olarak kullanılması bağlı olarak farklı olur:

- Varsayılan depolama alanı olarak kullanılan

    - Data Lake Store hesabı kök düzeyinde izni
    - Hdınsight küme depolama kök düzeyinde izni. Örneğin, __/kümeleri__ öğreticide daha önce kullanılan klasör.
- Ek depolama alanı kullanın

    - Burada erişim dosya klasörleri izni.

**Data Lake Store hesap kök düzeyinde izin atamak için**

1. Üzerinde **Data Lake Store erişim** dikey penceresinde tıklatın **erişim**. **Dosya izinlerini seçin** dikey penceresi açılır. Aboneliğinizdeki tüm Data Lake Store hesaplarını listeler.
2. Getirin (tıklamayın) onay kutusu görünür yapmak için Data Lake Store hesabı adını üzerinden fare sonra onay kutusunu seçin.

    ![Bir Hdınsight kümesine Ekle hizmet sorumlusu](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "hizmet sorumlusuna Hdınsight kümesi Ekle")

  Varsayılan olarak, __okuma__, __yazma__, ve __yürütme__ hepsi seçilidir.

3. Tıklatın **seçin** sayfanın üzerinde.
4. Tıklatın **çalıştırmak** izin atamak için.
5. **Bitti**’ye tıklayın.

**Hdınsight küme kök düzeyinde izin atamak için**

1. Üzerinde **Data Lake Store erişim** dikey penceresinde tıklatın **erişim**. **Dosya izinlerini seçin** dikey penceresi açılır. Aboneliğinizdeki tüm Data Lake Store hesaplarını listeler.
1. Gelen **dosya izinlerini seçin** dikey penceresinde içeriğini göstermek için Data Lake Store adına tıklayın.
2. Hdınsight küme depolama kök klasörü solundaki onay kutusunu işaretleyerek seçin. Ekran göre daha önce küme depolama köküdür __/kümeleri__ varsayılan depolama alanı olarak Data Lake Store seçerken belirtilen klasör.
3. Klasör izinlerini ayarlayın.  Varsayılan olarak, okuma, yazma ve yürütme hepsi seçilidir.
4. Tıklatın **seçin** sayfanın üzerinde.
5. **Çalıştır**’a tıklayın.
6. **Bitti**’ye tıklayın.

Data Lake Store ek depolama alanı olarak kullanıyorsanız, yalnızca Hdınsight küme erişmek istediğiniz klasörleri izinlerini atamanız gerekir. Örneğin, aşağıdaki ekran görüntüsünde, yalnızca erişilmesine **hdiaddonstorage** bir Data Lake Store hesabında klasör.

![Hdınsight kümesi için hizmet asıl izinleri atamak](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "Hdınsight kümesi için hizmet asıl izinleri atama")


## <a name="verify-cluster-set-up"></a>Küme ayarlama doğrulayın

Küme kurulumu tamamlandıktan sonra küme dikey penceresinde sonuçlarınızı ya da aşağıdaki adımları uygulayarak doğrulayın:

* İlişkili depolama küme için belirttiğiniz Data Lake Store hesabı olduğunu doğrulamak için tıklatın **depolama hesapları** sol bölmede.

    ![Bir Hdınsight kümesine Ekle hizmet sorumlusu](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "hizmet sorumlusuna Hdınsight kümesi Ekle")

* Hizmet sorumlusu Hdınsight kümesi ile ilişkili doğru olduğunu doğrulamak için tıklatın **Data Lake Store erişim** sol bölmede.

    ![Bir Hdınsight kümesine Ekle hizmet sorumlusu](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "hizmet sorumlusuna Hdınsight kümesi Ekle")


## <a name="examples"></a>Örnekler

Data Lake Store ile küme, depolama alanı olarak ayarladıktan sonra bu Data Lake Store içinde depolanan verileri çözümlemek üzere Hdınsight kümesi kullanma örnekleri bakın.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-primary-storage"></a>(Birincil depolama) bir Data Lake Store'da verileri karşı Hive sorgusu çalıştırma

Bir Hive sorgusu çalıştırmak için Ambari portal Hive görünümleri arabiriminde kullanın. Ambari Hive görünümleri kullanma hakkında daha fazla yönerge için bkz: [hdınsight'ta Hadoop ile Hive görünümünü kullanın](../hdinsight/hadoop/apache-hadoop-use-hive-ambari-view.md).

Bir Data Lake Store'da verilerle çalışırken, değiştirmek için birkaç dizeleri vardır.

Veri yolu, örneğin, birincil depolama olarak Data Lake Store ile oluşturduğunuz küme kullanıyorsanız olan: *adl: / / < data_lake_store_account_name > /azuredatalakestore.net/path/to/file*. Data Lake Store hesabında depolanan örnek verilerden bir tablo oluşturmak için bir Hive sorgusu aşağıdaki deyim gibi görünür:

    CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsstorage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

Açıklamaları:
* `adl://hdiadlstorage.azuredatalakestore.net/`Data Lake Store hesabı köküdür.
* `/clusters/myhdiadlcluster`Küme oluşturulurken belirtilen küme verileri köküdür.
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/`sorguda kullanılan örnek dosyasının konumudur.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-additional-storage"></a>Bir Data Lake Store'da verileri karşı (ek depolama alanı olarak) Hive sorgusu çalıştırma

Oluşturduğunuz küme varsayılan depolama alanı olarak Blob Depolama birimi kullanıyorsa, örnek verileri ek depolama alanı olarak kullanılan Azure Data Lake Store hesabı dahil değil. Böyle bir durumda önce verileri Blob depolama alanından Data Lake Store'a aktarmak ve ardından önceki örnekte gösterildiği gibi sorgular çalıştırın.

Blob depolama alanından bir Data Lake Store'a veri kopyalama hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Storage blobları ve Data Lake Store arasında veri kopyalamak için Distcp'yi kullanın](data-lake-store-copy-data-wasb-distcp.md)
* [Azure Storage bloblarından Data Lake Store'a veri kopyalamak için AdlCopy kullanın](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-store-with-a-spark-cluster"></a>Kullanım Data Lake Store ile Spark kümesi
Spark kümesi, bir Data Lake Store'da depolanan veriler üzerinde Spark işlerini çalıştırmak için kullanabilirsiniz. Daha fazla bilgi için bkz: [Data Lake Store'da verileri çözümlemek üzere Hdınsight Spark kullanma küme](../hdinsight/spark/apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-store-in-a-storm-topology"></a>Data Lake Store'da Storm topolojisini kullan
Data Lake Store, bir Storm topolojisinin veri yazmak için kullanabilirsiniz. Bu senaryo elde etmek yönergeler için bkz: [kullanım Azure Data Lake Store Hdınsight ile Apache Storm ile](../hdinsight/storm/apache-storm-write-data-lake-store.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Hdınsight kümeleri ile kullanım Data Lake Store](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [PowerShell: Data Lake Store kullanmak için bir Hdınsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
