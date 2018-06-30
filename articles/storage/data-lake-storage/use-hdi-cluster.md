---
title: Azure Hdınsight kümeleri ile Azure Data Lake Storage Gen2 Önizleme kullanın
description: Azure Data Lake Storage Gen2 Önizleme verileri sorgulamak ve çözümleme sonuçlarınızı depolama hakkında bilgi edinin.
keywords: hdfs, yapılandırılmış veri, yapılandırılmamış verileri, data lake Store'a, Hadoop giriş, Hadoop çıkış, hadoop depolama, hdfs giriş, hdfs çıkış, hdfs depolama, azure wasb
services: hdinsight,storage
documentationcenter: ''
tags: azure-portal
author: jamesbak
manager: jahogg
ms.component: data-lake-storage-gen2
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: d5b67d971c2261084857d6b512c8d011173884af
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113264"
---
# <a name="use-azure-data-lake-storage-gen2-preview-with-azure-hdinsight-clusters"></a>Azure Hdınsight kümeleri ile Azure Data Lake Storage Gen2 Önizleme kullanın

Hdınsight kümesindeki verileri çözümlemek için ya da Azure Storage, Azure Data Lake Storage Gen1 veya Azure Data Lake Storage Gen2 Önizleme herhangi bir bileşimini veri depolayabilirsiniz. Tüm depolama seçenekleri güvenle kullanıcı verilerini kaybetmeden hesaplama için kullanılan Hdınsight kümeleri silmenize olanak sağlar.

Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. Hdınsight küme oluşturma işlemi sırasında bir blob kapsayıcısını Azure Storage veya Azure Data Lake Storage varsayılan dosya sistemi olarak belirtebilirsiniz. Alternatif olarak Hdınsight 3.5 ile birlikte, Azure Storage veya Azure Data Lake Storage birkaç istisna dışında varsayılan dosya sistemi olarak seçebilirsiniz.

Bu makalede, Azure Data Lake Storage Gen2 Hdınsight kümeleri ile nasıl çalıştığını öğrenin. Hdınsight kümesi oluşturma hakkında daha fazla bilgi için bkz: [Hadoop, Spark, Kafka ve daha fazla ile Azure Data Lake Storage kullanarak Hdınsight ayarlama kümelerini](quickstart-create-connect-hdi-cluster.md).

Azure depolama, HDInsight ile sorunsuz bir şekilde tümleşen, sağlam ve genel amaçlı bir depolama çözümüdür. Hdınsight, küme için varsayılan dosya sistemi olarak Azure Data Lake Storage kullanabilirsiniz. Hadoop dağıtılmış dosya sistemi (HDFS) arabirimi aracılığıyla, Hdınsight'taki bileşenler kümesinin doğrudan Azure Data Lake Storage bulunan dosyalar üzerinde çalışabilir.

İş verilerini depolamak için varsayılan dosya sistemi kullanmanızı önermiyoruz. Varsayılan dosya sistemi, depolama maliyetini azaltmak için her kullanmak iyi bir uygulamadır sonra siliniyor. Bu varsayılan kapsayıcı içeren uygulama ve sistem Not günlükleri. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Birden fazla küme için bir dosya sistemi paylaşımı desteklenmiyor.

## <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi

Aşağıdaki diyagram, Azure Depolama ile kullanılan HDInsight depolama mimarisine ilişkin bir özet görünüm sağlar:

![Hadoop kümeleri, Blob Depolamada yapılandırılmış ve yapılandırılmamış verilere erişmek ve depolamak için HDFS API’sini kullanır.](./media/use-hdi-cluster/HDI.ABFS.Arch.png "HDInsight Depolama Mimarisi")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

Ayrıca, Hdınsight Azure Data Lake Store içinde depolanan verileri erişmenize olanak tanır. Söz dizimi aşağıdaki gibidir:

    abfs[s]://<file_system>@<accountname>.dfs.core.windows.net/<path>

Burada, Hdınsight kümeleri ile bir Azure Storage hesabı kullanırken, bazı hususlar bulunmaktadır.

* **Bir kümeye bağlı depolama hesaplarındaki dosyaları** hesap adı ve oluşturma sırasında kümeyle ilişkili anahtar. Bu yapılandırma tam erişimi dosya sistemindeki dosyaları sağlar.

* **Bir kümeye bağlı olmayan depolama hesaplarındaki genel dosyaları** dosya sistemindeki dosyalar salt okunur izni kullanıma sunma.
  
  > [!NOTE]
  > Genel dosya sistemleri, dosya sisteminde kullanılabilir tüm dosyaların bir listesini almak ve meta verilerine erişmek izin verir. Genel dosya sistemleri, yalnızca tam URL'yi biliyorsanız dosyalara erişmesine izin verir. Daha fazla bilgi için bkz: [kapsayıcılara ve blob'lara erişimi kısıtlama](http://msdn.microsoft.com/library/windowsazure/dd179354.aspx) (kapsayıcılar ve bloblar için kuralları aynı ön iş dosyaları ve dosya sistemi).
 
* **Bir kümeye bağlı olmayan depolama hesaplarındaki özel dosya sistemleri** WebHCat işleri gönderdiğinizde depolama hesabını tanımlamadığınız sürece access dosyalarını dosya sisteminde izin verme. Bu sınırlama nedeniyle, bu makalenin sonraki bölümlerinde açıklanmıştır.

Oluşturma işlemi ve bunların anahtarların tanımlanan depolama hesaplarını depolanmış *%HADOOP_HOME%/conf/core-site.xml* küme düğümlerinde. Hdınsight varsayılan davranışını tanımlanan depolama hesaplarını kullanmaktır *core-site.xml* dosya. [Ambari](/hdinsight/hdinsight-hadoop-manage-ambari.md) kullanarak bu ayarı değiştirebilirsiniz.

Hive, MapReduce, Hadoop akış ve Pig dahil olmak üzere birden çok WebHCat işleri kendileriyle birlikte depolama hesapları açıklaması ve meta veriler taşıyabilir. (Bu yaklaşım şu anda Pig depolama hesapları ile ancak meta verilerle çalışır.) Daha fazla bilgi için bkz. [Alternatif Depolama Hesapları ve Meta Depolarla HDInsight Kümesi kullanma](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

## <a id="benefits"></a>Azure Depolamanın yararları

İşlem kümelerini ve depolama kaynaklarını yeniden bulmamanın örtülü performans maliyeti, yüksek hızlı ağın işlem düğümlerinin Azure Depolama’daki verilere erişimini verimli hale getirdiği, Azure bölgesindeki depolama hesabı kaynaklarına yakın şekilde oluşturulmasıyla azaltılabilir.

Verileri HDFS yerine Azure Depolama’da depolamanın çeşitli avantajları vardır:

* **Verileri yeniden kullanma ve paylaşma:** HDFS’deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Azure depolamada bulunan verilere HDFS API'si aracılığıyla veya [Blob Depolama REST API'leri][blob-storage-restAPI] aracılığıyla erişilir. Bu nedenle, daha büyük uygulama kümeleri (diğer HDInsight kümeleri dahil olmak üzere) ve araçları veri üretmek ve kullanmak için kullanılabilir.

* **Veri arşivleme:** Verileri Azure depolamada depolamak, hesaplama için kullanılan HDInsight kümelerinin kullanıcı verilerini kaybetmeden güvenle silinmesini sağlar.

* **Veri depolama maliyeti:** uzun vadeli işlem kümesinin maliyeti Azure depolama maliyeti yüksek olduğu için veriler Azure storage'da depolamaktan daha maliyetlidir; verileri yerel HDFS'de depolamak. Ayrıca, verilerin her işlem kümesi oluşturmada yeniden yüklenmesi gerekmediğinden, veri yükleme maliyetlerinden de tasarruf edersiniz.

* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi sağlamakla birlikte, ölçek, kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, Azure depolamada otomatik olarak aldığınız esnek ölçeklendirme özelliklere bağlı kalmaktan daha karmaşık bir işlem haline gelebilir.

* **Coğrafi çoğaltma:** Azure storage verilerinizin coğrafi olarak çoğaltılabilir. Bu özelliği coğrafi kurtarma ve veri yedekliği sağlamakla birlikte, bir yük devretme coğrafi olarak çoğaltılmış konuma ciddi bir şekilde, performansı etkiler ve desteklenmesi ek ücrete neden olabilir. Bu nedenle, coğrafi çoğaltma dikkatle yalnızca seçip verilerin değeri ek maliyetlere ise.

* **Veri yaşam döngüsü yönetimi:** tüm verileri herhangi bir dosya sistemi kendi ömrü gider. Veri genellikle çok değerli ve sık erişilen edilen kapalı başlatır, daha az değerli ve daha az erişmesi için geçişler ve sonuçta arşiv veya silinmesi gerekir. Azure depolama, veri kendi yaşam döngüsü aşaması için uygun şekilde katmanı yönetim ilkelerini veri katmanlama ve yaşam döngüsü sağlar.

Bazı MapReduce işleri ve paketleri gerçekte Azure depolamada depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS’de depolamak üzere seçebilirsiniz. Aslında, Hdınsight Hive işleri ve diğer işlemlerdeki bu Ara sonuçların bazıları için (hangi için DFS olarak adlandırılır) yerel HDFS uygulamasına kullanır.

> [!NOTE]
> Çoğu HDFS komutu (örneğin, `ls`, `copyFromLocal` ve `mkdir`) hala beklendiği gibi çalışmayabilir. Gibi DFS özgü komutlar `fschk` ve `dfsadmin`, Azure depolama alanında farklı bir davranış gösterebilir.

## <a name="create-an-data-lake-storage-file-system"></a>Bir Data Lake Store dosya sistemi oluştur

Dosya sistemi kullanmak için önce oluşturduğunuz bir [Azure depolama hesabı][azure-storage-create]. Bu işlemin bir parçası olarak, depolama hesabının oluşturulduğu bir Azure bölgesi belirtin. Küme ve depolama hesabının aynı bölgede barındırılması gerekir. Hive meta depo SQL Server veritabanı ve Oozie meta depo SQL Server veritabanı da aynı bölgede bulunmalıdır.

Nerede olursa olsun, oluşturduğunuz her blob Azure Data Lake Storage hesabınızdaki bir dosya sistemi ait. 

Varsayılan Data Lake Store dosya sistemi iş geçmişi ve günlükleri gibi kümeye özel bilgileri depolar. Varsayılan Data Lake Store dosya sistemi birden çok Hdınsight kümesiyle paylaşmayın. Bu durum iş geçmişinin bozulmasına neden olabilir. Her küme için farklı bir dosya sistemi kullanmak ve paylaşılan verileri varsayılan depolama hesabı yerine tüm ilgili kümelerin dağıtımında belirtilen bağlantılı depolama hesabındaki yerleştirmek için önerilir. Bağlantılı Depolama hesaplarını yapılandırma hakkında daha fazla bilgi için bkz: [HDInsight kümeleri oluşturma][hdinsight-creation]. Ancak özgün Hdınsight kümesi silindikten sonra varsayılan depolama dosya sistemi yeniden kullanabilirsiniz. HBase kümeleri için silinmiş olan bir silinen HBase kümesi tarafından kullanılan varsayılan blob kapsayıcısını kullanarak yeni bir HBase kümesi oluşturarak HBase tablo şemasını ve verileri koruyabilirsiniz.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Portaldan bir Hdınsight kümesi oluştururken, depolama hesabı ayrıntılarını sağlamak için seçenekleri (aşağıdaki ekran görüntüsünde gösterildiği gibi) sahip. Bir ek depolama alanı hesabı kümesi ile ilişkili ve bu durumda, ek depolama alanı kullanılabilir depolama alanı seçenekleri arasından seçim isteyip istemediğinizi de belirtebilirsiniz.

![HDInsight hadoop oluşturma veri kaynağı](./media/use-hdi-cluster/create-storage-account.png)

> [!WARNING]
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Varsa, [Azure PowerShell'i yükleyip][powershell-install], bir depolama hesabı ve kapsayıcı oluşturmak için Azure PowerShell isteminde aşağıdaki kodu kullanabilirsiniz:

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "WEST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Connect-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
      -Name StorageAccountName `
      -Location $Location `
      -SkuName Standard_LRS `
      -Kind StorageV2 
      -HierarchialNamespace $True

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

> [!NOTE]
> Bir kapsayıcı oluşturma, Azure Data Lake Store dosya sistemi oluşturma ile eşanlamlıdır.

### <a name="use-azure-cli"></a>Azure CLI kullanma

[!INCLUDE [use-latest-version](../../../includes/hdinsight-use-latest-cli.md)]

[Azure CLI yüklenmiş ve yapılandırılmışsa](../../cli-install-nodejs.md), aşağıdaki komut bir depolama hesabı ve kapsayıcı için kullanılabilir.

```bash
az storage account create \
    --name <STORAGE_ACCOUNT_NAME> \
    --resource-group <RESOURCE_GROUP_NAME> \
    --location westus2 \
    --sku Standard_LRS \
    --kind StorageV2 \
    --Enable-hierarchical-namespace true
```

> [!NOTE]
> Data Lake Storage Gen2, yalnızca genel Önizleme sırasında `--sku Standard_LRS` desteklenir.

Depolama hesabının oluşturulacağı coğrafi bölgeyi belirtmeniz istenir. Hdınsight kümenizi oluşturmayı planladığınızla aynı bölgede depolama hesabı oluşturun.

Depolama hesabı oluşturulduğunda, depolama hesabı anahtarlarını almak için aşağıdaki komutu kullanın:

    azure storage account keys list <STORAGE_ACCOUNT_NAME>

Bir kapsayıcı oluşturmak için aşağıdaki komutu kullanın:

    azure storage container create <CONTAINER_NAME> --account-name <STORAGE_ACCOUNT_NAME> --account-key <STORAGE_ACCOUNT_KEY>

> [!NOTE]
> Bir kapsayıcı oluşturma, Azure Data Lake Store dosya sistemi oluşturma ile eşanlamlıdır.

## <a name="address-files-in-azure-storage"></a>Azure depolamada dosyaları adresleme

HDInsight’ta Azure depolamadaki dosyalara erişmek için URI şeması aşağıdaki gibidir:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.widows.net/<PATH>

URI şeması şifrelenmemiş erişim sağlar (ile *abfs:* önek) ve SSL şifreli erişim (ile *abfss*). Kullanmanızı öneririz *abfss* mümkün olduğunda, hatta azure'da aynı bölgede bulunan verilere erişirken.

* &lt;FILE_SYSTEM_NAME&gt; yolun dosya sisteminin Azure Data Lake Storage tanımlar.
* &lt;ACCOUNT_NAME&gt; Azure depolama hesabı adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

    Varsa değerleri &lt;FILE_SYSTEM_NAME&gt; ya da &lt;ACCOUNT_NAME&gt; belirtildi, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanabilirsiniz. Örneğin, *hadoop mapreduce examples.jar* Hdınsight kümeleriyle gelen başvurulabilir için aşağıdaki yollardan birini kullanarak:
    
        abfs://myfilesystempath@myaccount.dfs.core.widows.net/example/jars/hadoop-mapreduce-examples.jar
        abfs:///example/jars/hadoop-mapreduce-examples.jar
        /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> HDInsight sürüm 2.1 ve 1.6 kümelerinde dosya adı *hadoop examples.jar* şeklindedir.

* &lt;Yolu&gt; dosya veya dizin HDFS yol adıdır.

> [!NOTE]
> Hdınsight dışındaki dosyaları ile çalışırken, yardımcı programların çoğu ABFS biçimlendirmek ve bunun yerine bir temel yol biçimi gibi bekler tanımaz `example/jars/hadoop-mapreduce-examples.jar`.
 
## <a name="use-additional-storage-accounts"></a>Ek depolama hesaplarını kullanma

HDInsight kümesi oluştururken ilişkilendirmek istediğiniz Azure Depolama hesabını belirtirsiniz. Bu depolama hesabına ek olarak, oluşturma işlemi sırasında veya bir küme oluşturulduktan sonra aynı Azure aboneliğinden veya farklı Azure aboneliklerinden başka depolama hesapları ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [HDInsight kümeleri oluşturma](/hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede HDFS ile uyumlu Azure Depolama'yı HDInsight ile nasıl kullanacağınızı öğrendiniz. Bu yaklaşım, ölçeklenebilir, uzun vadeli, arşivleme verileri edinme çözümleri oluşturmak ve bilgileri içinde depolanan yapılandırılmış ve yapılandırılmamış verileri kilidini açmak için Hdınsight kullanmanıza olanak tanır.

Daha fazla bilgi için bkz.

* [Azure Data Lake Storage Gen2 ABFS Hadoop dosya sistemi sürücüsü](abfs-driver.md)
* [Azure Data Lake Storage giriş](introduction.md)
* [Hadoop, Spark, Kafka ve daha fazla ile Azure Data Lake Storage kullanarak Hdınsight kümelerini ayarlama](quickstart-create-connect-hdi-cluster.md)
* [Azure Data Lake distcp'yi kullanarak depolama alanına veri alma](use-distcp.md)

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: /hdinsight/hdinsight-hadoop-provision-linux-clusters.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: /storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/use-hdi-cluster/HDI.PowerShell.BlobCommands.png
