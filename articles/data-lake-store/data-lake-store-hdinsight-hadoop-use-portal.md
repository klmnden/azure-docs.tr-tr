---
title: Azure HDInsight kümeleri ile Azure Data Lake depolama Gen1 oluşturmak için Azure portalını kullanma | Microsoft Docs
description: Oluşturun ve HDInsight kümeleri ile Azure Data Lake depolama Gen1 kullanmak için Azure portalını kullanma
services: data-lake-store,hdinsight
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: a8c45a83-a8e3-4227-8b02-1bc1e1de6767
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 6f9064c6027499fff3a8551ee60722cd66c54dc2
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58883437"
---
# <a name="create-hdinsight-clusters-with-azure-data-lake-storage-gen1-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure Data Lake depolama Gen1 ile HDInsight kümeleri oluşturma
> [!div class="op_single_selector"]
> * [Azure portal’ı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [(Varsayılan depolama için) PowerShell kullanma](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell'i kullanma (ek depolama için)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisi'ni kullanın](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Varsayılan depolama alanı veya ek depolama alanı olarak Azure Data Lake depolama Gen1 hesabınız ile HDInsight kümesi oluşturmak için Azure portalını kullanmayı öğrenin. Ek depolama alanı bir HDInsight kümesi için isteğe bağlı olsa bile, ek depolama hesapları iş verilerinizi depolamak için önerilir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki gereksinimleri karşılamanızın emin olun:

* **Bir Azure aboneliği**. Git [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Data Lake depolama Gen1 hesabı**. Makalesindeki yönergeleri izleyin [Azure portalını kullanarak Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md). Ayrıca, hesapta bir kök klasör oluşturmanız gerekir.  Bu öğreticide, bir kök klasör olarak adlandırılan __/kümeleri__ kullanılır.
* **Bir Azure Active Directory Hizmet sorumlusu**. Bu öğreticide, bir hizmet sorumlusu, Azure Active Directory'de (Azure AD) oluşturma hakkında yönergeler açıklanmaktadır. Ancak, bir hizmet sorumlusu oluşturmak için Azure AD yönetici olmanız gerekir. Bir yöneticiyseniz, bu önkoşulu atlayabilirsiniz ve öğreticiyle devam edin.

    >[!NOTE]
    >Yalnızca bir Azure AD Yöneticisi olduğunuz bir hizmet sorumlusu oluşturabilirsiniz. Azure AD yöneticinizin bir HDInsight kümesi ile Data Lake depolama Gen1 oluşturabilmeniz için önce bir hizmet sorumlusu oluşturmanız gerekir. Ayrıca, hizmet sorumlusunun bir sertifika ile açıklandığı oluşturulmalıdır [sertifika ile hizmet sorumlusu oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).
    >

## <a name="create-an-hdinsight-cluster"></a>HDInsight kümesi oluşturma

Bu bölümde, varsayılan veya ek depolama alanı olarak Data Lake depolama Gen1 hesapları ile HDInsight kümesi oluşturun. Bu makale, Data Lake depolama Gen1 hesapları yapılandırma bölümü yalnızca odaklanır.  Genel bir küme oluşturma bilgileri ve yordamları için bkz: [Hadoop kümeleri oluşturma HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

### <a name="create-a-cluster-with-data-lake-storage-gen1-as-default-storage"></a>Varsayılan depolama alanı olarak Data Lake depolama Gen1 ile küme oluşturma

**Bir HDInsight kümesi varsayılan depolama hesabı olarak bir Data Lake depolama Gen1 hesabıyla oluşturmak için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. İzleyin [küme oluşturma](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) HDInsight kümeleri oluşturma hakkında genel bilgi için.
3. Üzerinde **depolama** dikey altında **birincil depolama türü**seçin **Azure Data Lake depolama Gen1**ve ardından aşağıdaki bilgileri girin:

    ![HDInsight kümesi için hizmet sorumlusu ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "HDInsight kümesi için hizmet sorumlusu ekleme")

    - **Select Data Lake Store hesabı**: Mevcut bir Data Lake depolama Gen1 hesabını seçin. Mevcut bir Data Lake depolama Gen1 hesabı gereklidir.  [Ön koşullara](#prerequisites) bakın.
    - **Kök yolu**: Kümeye özgü dosyalar depolanacak nerede bir yol girin. Ekran görüntüsünde olduğu __/kümeleri/myhdiadlcluster/__, hangi __/kümeleri__ klasör var olmalıdır ve portalın oluşturduğu *myhdicluster* klasör.  *Myhdicluster* küme adıdır.
    - **Data Lake Store erişimi**: HDInsight kümesi ve Data Lake depolama Gen1 hesabı arasında erişim yapılandırın. Data Lake depolama Gen1 yapılandırma yönergeleri için bkz erişim.
    - **Ek depolama hesapları**: Azure depolama hesapları, küme için ek depolama hesapları olarak ekleyin. Ek Data Lake depolama Gen1 hesapları eklemek için birincil depolama türü olarak Data Lake depolama Gen1 hesabı yapılandırırken verileri daha fazla Data Lake depolama Gen1 hesaplarında küme izinleri vermeyi tarafından gerçekleştirilir. Bkz: yapılandırma Data Lake depolama Gen1 erişim.

4. Üzerinde **Data Lake Store erişimi**, tıklayın **seçin**ve açıklandığı gibi küme oluşturma işlemine devam etmek [Hadoop kümeleri oluşturma HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md).


### <a name="create-a-cluster-with-data-lake-storage-gen1-as-additional-storage"></a>Ek depolama alanı olarak Data Lake depolama Gen1 ile küme oluşturma

Aşağıdaki yönergeler, varsayılan depolama alanı olarak Azure depolama hesabı ve ek depolama alanı olarak Data Lake depolama Gen1 hesabı ile bir HDInsight kümesi oluşturun.

**Bir HDInsight kümesine ek depolama hesabı bir Data Lake depolama Gen1 hesabıyla oluşturmak için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. İzleyin [küme oluşturma](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) HDInsight kümeleri oluşturma hakkında genel bilgi için.
3. Üzerinde **depolama** dikey altında **birincil depolama türü**seçin **Azure depolama**ve ardından aşağıdaki bilgileri girin:

    ![HDInsight kümesi için hizmet sorumlusu ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "HDInsight kümesi için hizmet sorumlusu ekleme")

    - **Seçim yöntemini**: aşağıdaki seçeneklerden birini kullanın:

        * Azure aboneliğinizin bir parçası olan bir depolama hesabı belirtmek için seçin **Aboneliklerim**ve ardından depolama hesabını seçin.
        * Azure aboneliğiniz bir depolama hesabı belirtmek için seçin **erişim anahtarı**ve ardından dış depolama hesabı için bilgileri sağlayın.

    - **Varsayılan kapsayıcı**: varsayılan değeri kullanın veya kendi ad belirtin.

    - Ek depolama hesapları: ek depolama alanı olarak daha fazla Azure depolama hesapları ekleme.
    - Data Lake Store erişimi: HDInsight kümesi ve Data Lake depolama Gen1 hesabı arasında erişim yapılandırın. Yönergeler için bkz [yapılandırma Data Lake depolama Gen1 erişim](#configure-data-lake-storage-gen1-access).

## <a name="configure-data-lake-storage-gen1-access"></a>Data Lake depolama Gen1 erişimi yapılandırma 

Bu bölümde, bir Azure Active Directory Hizmet sorumlusunu kullanarak HDInsight kümeleri Data Lake depolama Gen1 erişim yapılandırın. 

### <a name="specify-a-service-principal"></a>Bir hizmet sorumlusu belirtin

Azure portalından için mevcut bir hizmet sorumlusunu kullanabilir veya yeni bir tane oluşturun.

**Azure portalından bir hizmet sorumlusu oluşturmak için**

1. Tıklayın **Data Lake Store erişimi** depolama dikey penceresinden.
2. Üzerinde **Data Lake depolama Gen1 erişim** dikey penceresinde tıklayın **Yeni Oluştur**.
3. Tıklayın **hizmet sorumlusu**ve ardından bir hizmet sorumlusu oluşturmak için yönergeleri izleyin.
4. Gelecekte yeniden kullanmaya karar verirseniz sertifikayı indirin. Sertifika Yükleme ek HDInsight kümeleri oluşturduğunuzda, aynı hizmet asıl adı kullanmak istiyorsanız yararlıdır.

    ![HDInsight kümesi için hizmet sorumlusu ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "HDInsight kümesi için hizmet sorumlusu ekleme")

4. Tıklayın **erişim** klasör erişimi yapılandırmak için.  Bkz: [dosya izinleri yapılandırma](#configure-file-permissions).


**Mevcut bir hizmet sorumlusunu Azure portalından kullanmak için**

1. Tıklayın **Data Lake Store erişimi**.
1. Üzerinde **Data Lake depolama Gen1 erişim** dikey penceresinde tıklayın **var olanı kullan**.
2. Tıklayın **hizmet sorumlusu**ve ardından bir hizmet sorumlusu seçin. 
3. Seçili Hizmet sorumlusu ile ilişkili sertifika (.pfx dosyası) karşıya yükleyin ve sonra sertifikanın parolasını girin.

    ![HDInsight kümesi için hizmet sorumlusu ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "HDInsight kümesi için hizmet sorumlusu ekleme")

4. Tıklayın **erişim** klasör erişimi yapılandırmak için.  Bkz: [dosya izinleri yapılandırma](#configure-file-permissions).


### <a name="configure-file-permissions"></a>Dosya izinleri yapılandırma

Yapılandırır hesabı varsayılan depolama alanı veya ek bir depolama hesabı olarak kullanılan bağlı olarak farklılık gösterir:

- Varsayılan depolama alanı olarak kullanılan

    - Data Lake depolama Gen1 hesabının kök düzeyinde izni
    - HDInsight küme depolama kök düzeyinde izni. Örneğin, __/kümeleri__ öğreticide daha önce kullanılan klasörü.
- Ek depolama alanı olarak kullanma

    - Burada erişim dosya klasörleri izni.

**Data Lake depolama Gen1 hesap kök düzeyinde izin atamak için**

1. Üzerinde **Data Lake depolama Gen1 erişim** dikey penceresinde tıklayın **erişim**. **Dosya izinlerini seçin** dikey penceresi açılır. Aboneliğinizdeki tüm Data Lake depolama Gen1 hesapları listeler.
2. Vurgu (tıklamayın) onay kutusu görünür hale getirmek için Data Lake depolama Gen1 hesap adının üzerine fare sonra onay kutusunu seçin.

    ![HDInsight kümesi için hizmet sorumlusu ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "HDInsight kümesi için hizmet sorumlusu ekleme")

   Varsayılan olarak, __okuma__, __yazma__, ve __yürütme__ hepsi seçilidir.

3. Tıklayın **seçin** sayfanın alt kısmındaki.
4. Tıklayın **çalıştırma** izin atamak için.
5. **Bitti**’ye tıklayın.

**HDInsight kümesi kök düzeyinde izin atamak için**

1. Üzerinde **Data Lake depolama Gen1 erişim** dikey penceresinde tıklayın **erişim**. **Dosya izinlerini seçin** dikey penceresi açılır. Aboneliğinizdeki tüm Data Lake depolama Gen1 hesapları listeler.
1. Gelen **dosya izinlerini seçin** dikey penceresinde, içeriğini gösterecek şekilde Data Lake depolama Gen1 hesabın adına tıklayın.
2. HDInsight küme depolama kök klasörü solundaki onay kutusunu seçerek seçin. Ekran göre daha önce küme depolama köküdür __/kümeleri__ varsayılan depolama alanı olarak Data Lake depolama Gen1 seçerken belirttiğiniz klasör.
3. Klasör izinleri ayarlayın.  Varsayılan olarak, okuma, yazma ve yürütme hepsi seçilidir.
4. Tıklayın **seçin** sayfanın alt kısmındaki.
5. **Çalıştır**’a tıklayın.
6. **Bitti**’ye tıklayın.

Ek depolama alanı olarak Data Lake depolama Gen1 kullanıyorsanız, yalnızca HDInsight kümesinden erişmek istediğiniz klasörleri için izin atamanız gerekir. Örneğin, aşağıdaki ekran görüntüsünde erişim yalnızca sağladığınız **mynewfolder** bir Data Lake depolama Gen1 hesabında klasör.

![HDInsight kümesine hizmet sorumlusu izinleri atama](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "HDInsight kümesi için hizmet sorumlusu izinleri atama")


## <a name="verify-cluster-set-up"></a>Küme doğrulayın

Küme kurulumu tamamlandıktan sonra küme dikey penceresinde ya da aşağıdaki adımları gerçekleştirerek sonuçlarınızı doğrulayın:

* İlişkili depolama alanının küme için belirttiğiniz Data Lake depolama Gen1 hesabı olduğunu doğrulamak için tıklayın **depolama hesapları** sol bölmesinde.

    ![HDInsight kümesi için hizmet sorumlusu ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "HDInsight kümesi için hizmet sorumlusu ekleme")

* Hizmet sorumlusu ile HDInsight kümesi doğru ilişkili olduğunu doğrulamak için tıklayın **Data Lake depolama Gen1 erişim** sol bölmesinde.

    ![HDInsight kümesi için hizmet sorumlusu ekleme](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "HDInsight kümesi için hizmet sorumlusu ekleme")


## <a name="examples"></a>Örnekler

Data Lake depolama Gen1 içinde depolanan verileri çözümlemek için HDInsight kümesi kullanmak üzere bu örnekler, depolama alanı olarak Data Lake depolama Gen1 ile kümesi ayarladıktan sonra bakın.

### <a name="run-a-hive-query-against-data-in-a-data-lake-storage-gen1-account-as-primary-storage"></a>(Birincil depolama olarak) bir Data Lake depolama Gen1 hesaptaki verilere karşı bir Hive sorgusu çalıştırma

Bir Hive sorgusu çalıştırmak için portalda Ambari Hive görünümleri arabirimi kullanın. Ambari Hive görünümleri kullanma hakkında yönergeler için bkz: [HDInsight Hadoop ile Hive görünümünü kullanma](../hdinsight/hadoop/apache-hadoop-use-hive-ambari-view.md).

Bir Data Lake depolama Gen1 hesabında verilerle çalışırken değiştirmek için birkaç dizeler vardır.

Örneğin, birincil depolama olarak Data Lake depolama Gen1 ile oluşturduğunuz küme kullanırsanız verileri yoludur: *adl: / / < data_lake_storage_gen1_account_name > /azuredatalakestore.net/path/to/file*. Bir Hive sorgusu Data Lake depolama Gen1 hesapta depolanan örnek verilerden bir tablo oluşturmak için aşağıdaki deyimi gibi görünür:

    CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsg1storage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

Açıklamaları:
* `adl://hdiadlsg1storage.azuredatalakestore.net/` Data Lake depolama Gen1 hesabı köküdür.
* `/clusters/myhdiadlcluster` kümeyi oluştururken belirttiğiniz Küme verilerini köküdür.
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/` sorguda kullanılan örnek dosyanın konumudur.

### <a name="run-a-hive-query-against-data-in-a-data-lake-storage-gen1-account-as-additional-storage"></a>(Ek depolama alanı olarak) bir Data Lake depolama Gen1 hesaptaki verilere karşı bir Hive sorgusu çalıştırma

Oluşturduğunuz kümenin varsayılan depolama alanı olarak Blob Depolama alanı kullanıyorsa, örnek verileri ek depolama alanı olarak kullanılan Data Lake depolama Gen1 hesabında yer almıyor. Böyle bir durumda, Blob depolama alanından verileri Data Lake depolama Gen1 hesabına aktarın ve ardından yukarıdaki örnekte gösterildiği gibi sorgular çalıştırın.

Bir Data Lake depolama Gen1 hesabına Blob depolamadan veri kopyalama hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure depolama blobları ile Data Lake depolama Gen1 arasında veri kopyalamak için Distcp kullanma](data-lake-store-copy-data-wasb-distcp.md)
* [Data Lake depolama Gen1 için Azure depolama bloblarından veri kopyalamak için AdlCopy kullanma](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-storage-gen1-with-a-spark-cluster"></a>Data Lake depolama Gen1 bir Spark kümesi ile kullanma
Bir Spark kümesi, Spark, Data Lake depolama Gen1 hesabında depolanan veriler üzerinde işlerini çalıştırmak için kullanabilirsiniz. Daha fazla bilgi için [Data Lake depolama Gen1 verileri çözümlemek için HDInsight Spark kullanma küme](../hdinsight/spark/apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-storage-gen1-in-a-storm-topology"></a>Data Lake depolama Gen1 bir Storm topolojisinde kullanın
Bir Storm topolojisinden veri yazmak için Data Lake depolama Gen1 hesabı kullanabilirsiniz. Bu senaryo elde etmek yönergeler için bkz: [kullanımı Azure Data Lake depolama Gen1 HDInsight ile Apache Storm ile](../hdinsight/storm/apache-storm-write-data-lake-store.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake depolama Gen1 Azure HDInsight kümeleri ile kullanma](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [PowerShell: Data Lake depolama Gen1 kullanılacak bir HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
