---
title: Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma
description: Azure Data Lake depolama Gen2 buradan veri sorgulamak ve analiz sonuçlarınızı depolama hakkında bilgi edinin.
author: jamesbak
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: jamesbak
ms.openlocfilehash: 15d9210e356da6d7757582dc4dad25cb2882ccfa
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58003228"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma

Bir HDInsight kümesindeki verileri çözümlemek için Azure Blob Depolama, Azure Data Lake depolama etkin Gen2 ile Azure Blob Depolama veya Azure Data Lake depolama Gen1 herhangi bir birleşimini ya da verileri depolayabilirsiniz. Tüm depolama seçenekleri, kullanıcı verilerini kaybetmeden hesaplama için kullanılan HDInsight kümelerini güvenle silmenizi sağlar.

Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight kümesi oluşturma işlemi sırasında Azure Depolama'da veya Data Lake depolama 2. nesil olarak varsayılan dosya sistemi tarafından sunulan hiyerarşik ad alanı, bir blob kapsayıcısı belirtebilirsiniz. Alternatif olarak HDInsight 3.5 ile kapsayıcı ya da hiyerarşik ad alanı birkaç özel durum varsayılan dosya sistemi olarak seçebilirsiniz.

Bu makalede, Data Lake depolama Gen2 HDInsight kümeleri ile nasıl çalıştığını öğrenin. HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz. [ayarlama HDInsight kümeleri Azure Data Lake Store, Hadoop, Spark, Kafka ve daha fazlası ile kullanarak](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters).

Azure depolama, HDInsight ile sorunsuz bir şekilde tümleşen, sağlam ve genel amaçlı bir depolama çözümüdür. HDInsight, küme için varsayılan dosya sistemi olarak Azure Data Lake Storage kullanabilirsiniz. Hadoop dağıtılmış dosya sistemi (HDFS) arabirimi aracılığıyla bileşenlerde HDInsight kümesini Azure Data Lake Storage dosyaları üzerinde doğrudan çalışabilir.

İş verilerini depolamak için varsayılan dosya sistemi kullanmanız önerilmez. Varsayılan dosya sistemi, depolama maliyetini azaltmak için her kullanmak iyi bir uygulamadır sonra siliniyor. Bu varsayılan kapsayıcı içeren uygulama ve sistem Not günlükleri. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Bir dosya sistemi birden fazla küme için paylaşımı desteklenmez.

## <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi

Aşağıdaki diyagram, Azure Depolama ile kullanılan HDInsight depolama mimarisine ilişkin bir özet görünüm sağlar:

![Hadoop kümeleri, Blob Depolamada yapılandırılmış ve yapılandırılmamış verilere erişmek ve depolamak için HDFS API’sini kullanır.](./media/data-lake-storage-use-hdi-cluster/HDI.ABFS.Arch.png "HDInsight Depolama Mimarisi")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<NAME_NODE_HOST>/<PATH>

Ayrıca, HDInsight, Azure Data Lake Store içinde depolanan verilere erişmesini sağlar. Söz dizimi aşağıdaki gibidir:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<path>

HDInsight kümeleri ile bir Azure depolama hesabını kullanırken bazı noktalar şunlardır.

* **Bir kümeye bağlı depolama hesaplarındaki dosyalara** hesap adı ve anahtarı ile küme oluşturma sırasında ilişkilendirilmiş. Bu yapılandırma tam erişim dosya sistemindeki dosyaları sağlar.

* **Bir kümeye bağlı olmayan depolama hesaplarındaki genel dosyaları** dosya sistemindeki dosyaları salt okunur izni kullanıma sunar.
  
  > [!NOTE]
  > Genel dosya sistemleri, dosya sisteminde kullanılabilir tüm dosyaların listesini almak ve meta verilerine erişim sağlar. Genel dosya sistemleri, yalnızca tam URL'yi biliyorsanız dosyalara erişmesine izin verin. Daha fazla bilgi için [erişimi kapsayıcılara ve blob'lara erişimi kısıtlama](https://msdn.microsoft.com/library/windowsazure/dd179354.aspx) (kapsayıcılar ve bloblar için kuralları aynı ön çalışma dosyalarını ve dosya sistemi).
 
* **Bir kümeye bağlı olmayan depolama hesaplarındaki özel dosya sistemleri** WebHCat işleri gönderdiğinizde depolama hesabını tanımlamadığınız sürece access dosyalarını dosya sisteminde izin verme. Bu kısıtlama nedeniyle, bu makalenin sonraki bölümlerinde açıklanmıştır.

Oluşturma işlemi ve bunların anahtarların tanımlanan depolama hesaplarını depolanan *%HADOOP_HOME%/conf/core-site.xml* küme düğümlerinde. HDInsight'ın varsayılan davranışı, tanımlanan depolama hesaplarını kullanmaktır *core-site.xml* dosya. [Ambari](../../hdinsight/hdinsight-hadoop-manage-ambari.md) kullanarak bu ayarı değiştirebilirsiniz.

Hive, MapReduce, Hadoop akış ve Pig dahil olmak üzere birden çok WebHCat işleri kendileriyle birlikte depolama hesapları açıklaması ve meta veriler taşıyabilir. (Bu yaklaşım şu anda Pig depolama hesapları ile ancak meta verilerle çalışmamaktadır.) Daha fazla bilgi için bkz. [Alternatif Depolama Hesapları ve Meta Depolarla HDInsight Kümesi kullanma](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

## <a id="benefits"></a>Azure Depolamanın yararları

İşlem kümelerini ve depolama kaynaklarını yeniden bulmamanın örtülü performans maliyeti, yüksek hızlı ağın işlem düğümlerinin Azure Depolama’daki verilere erişimini verimli hale getirdiği, Azure bölgesindeki depolama hesabı kaynaklarına yakın şekilde oluşturulmasıyla azaltılabilir.

Verileri HDFS yerine Azure Depolama’da depolamanın çeşitli avantajları vardır:

* **Verileri yeniden kullanma ve paylaşma:** HDFS'deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Azure depolamada bulunan verilere HDFS API'si aracılığıyla veya [Blob Depolama REST API'leri][blob-storage-restAPI] aracılığıyla erişilir. Bu nedenle, daha büyük uygulama kümeleri (diğer HDInsight kümeleri dahil olmak üzere) ve araçları veri üretmek ve kullanmak için kullanılabilir.

* **Veri arşivleme:** Verileri Azure depolamada depolamak, hesaplama için kullanılan kullanıcı verilerini kaybetmeden güvenle silinmesini HDInsight kümeleri sağlar.

* **Veri depolama maliyeti:** Verileri yerel HDFS'de için uzun vadeli depolama, işlem kümesinin maliyeti Azure depolama maliyeti yüksek olduğu için verileri Azure depolamada depolamaktan daha maliyetlidir. Ayrıca, verilerin her işlem kümesi oluşturmada yeniden yüklenmesi gerekmediğinden, veri yükleme maliyetlerinden de tasarruf edersiniz.

* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi ile sağlar ancak ölçek kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, Azure depolamada otomatik olarak aldığınız esnek ölçeklendirme özelliklere bağlı kalmaktan daha karmaşık bir işlem haline gelebilir.

* **Coğrafi çoğaltma:** Azure depolama verilerinizin coğrafi olarak çoğaltılmış olabilir. Bu özelliği size coğrafi kurtarma ve veri yedekliği sağlamakla rağmen coğrafi olarak çoğaltılmış konumu için bir yük devretme ciddi bir şekilde destekleyen, performansı etkiler ve ek ücrete neden olabilir. Bu nedenle dikkatli bir şekilde ve yalnızca coğrafi çoğaltma seçin verilerin değeri ek maliyetlere değer durumdaysa.

* **Veri yaşam döngüsü yönetimi:** Herhangi bir dosya sistemi tüm verileri kendi yaşam döngüsü gider. Veri, genellikle çok değerli ve sık erişilen olan kapalı başlatır, küçük değerli ve daha az erişmesi için geçiş ve sonuçta arşiv veya silinmesi gerekir. Azure depolama, veri katmanı ve yaşam döngüsü uygun şekilde kendi yaşam döngüsü aşaması için katmanlı bir veri yönetimi ilkeleri sağlar.

Bazı MapReduce işleri ve paketleri gerçekte Azure depolamada depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS’de depolamak üzere seçebilirsiniz. Aslında, HDInsight Hive işleri ve diğer işlemlerdeki bu Ara sonuçların bazıları için (DFS adlandırılır) yerel HDFS uygulamasına kullanır.

> [!NOTE]
> Çoğu HDFS komutu (örneğin, `ls`, `copyFromLocal` ve `mkdir`) hala beklendiği gibi çalışmayabilir. Gibi DFS özgü komutlar `fschk` ve `dfsadmin`, Azure depolamada farklı bir davranış gösterir.

## <a name="create-a-data-lake-storage-file-system"></a>Bir Data Lake Store dosya sistemi oluşturun

Dosya sistemi kullanmak için önce oluşturduğunuz bir [Azure depolama hesabı][azure-storage-create]. Bu işlemin bir parçası olarak depolama hesabının oluşturulduğu Azure bölgesini belirtin. Küme ve depolama hesabının aynı bölgede barındırılması gerekir. Hive meta depo SQL Server veritabanı ve Oozie meta depo SQL Server veritabanı da aynı bölgede bulunmalıdır.

Nerede olursa olsun, oluşturduğunuz her blob depolama hesabınızda bir dosya sistemine ait.

Varsayılan Data Lake depolama 2. nesil dosya sistemini iş geçmişi ve günlükleri gibi kümeye özel bilgileri depolar. Varsayılan Data Lake depolama 2. nesil dosya sistemini birden çok HDInsight kümesiyle paylaşmayın. Bu durum iş geçmişinin bozulmasına neden olabilir. Her küme için farklı bir dosya sistemi kullanmak ve paylaşılan verileri varsayılan depolama hesabı yerine tüm ilgili kümelerin dağıtımında belirtilen bağlantılı depolama hesabındaki yerleştirmek için önerilir. Bağlantılı Depolama hesaplarını yapılandırma hakkında daha fazla bilgi için bkz: [HDInsight kümeleri oluşturma][hdinsight-creation]. Ancak özgün HDInsight kümesi silindikten sonra varsayılan depolama dosya sistemi yeniden kullanabilirsiniz. HBase kümeleri için silinmiş olan bir silinmiş HBase kümesi tarafından kullanılan varsayılan blob kapsayıcısını kullanan yeni bir HBase kümesi oluşturarak HBase tablo şemasını ve verileri tutabilirsiniz.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Portaldan bir HDInsight kümesi oluştururken, depolama hesabı ayrıntılarını girmek için seçenekleri (aşağıdaki ekran görüntüsünde gösterildiği gibi) sahip. Ayrıca, ek bir depolama hesabı, kümeyle ilişkili ve bu durumda, herhangi bir ek depolama alanı kullanılabilir depolama seçenekleri arasından istediğinizi belirtebilirsiniz.

![HDInsight hadoop oluşturma veri kaynağı](./media/data-lake-storage-use-hdi-cluster/create-storage-account.png)

> [!WARNING]
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Varsa, [Azure PowerShell yüklenmiş ve yapılandırılmışsa][powershell-install], bir depolama hesabı ve kapsayıcı oluşturmak için Azure PowerShell isteminde aşağıdaki kodu kullanabilirsiniz:

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

```azurepowershell
$SubscriptionID = "<Your Azure Subscription ID>"
$ResourceGroupName = "<New Azure Resource Group Name>"
$Location = "westus2"

$StorageAccountName = "<New Azure Storage Account Name>"
$containerName = "<New Azure Blob Container Name>"

Select-AzSubscription -SubscriptionId $SubscriptionID

# Create resource group
New-AzResourceGroup -name $ResourceGroupName -Location $Location

# Create default storage account
New-AzStorageAccount -ResourceGroupName $ResourceGroupName `
  -Name $StorageAccountName `
  -Location $Location `
  -SkuName Standard_LRS `
  -Kind StorageV2 `
  -EnableHierarchicalNamespace $True

# Create default blob containers
$storageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
$destContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
New-AzStorageContainer -Name $containerName -Context $destContext
```

> [!NOTE]
> Bir kapsayıcı oluşturmak, Data Lake depolama Gen2'ye bir dosya sistemi oluşturma ile eşanlamlıdır.

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

Depolama hesabının oluşturulacağı coğrafi bölgeyi belirtmeniz istenir. HDInsight kümenizi oluşturmayı planladığınız aynı bölgede depolama hesabı oluşturun.

Depolama hesabı oluşturulduğunda, depolama hesabı anahtarlarını almak için aşağıdaki komutu kullanın:

    azure storage account keys list <STORAGE_ACCOUNT_NAME>

Bir kapsayıcı oluşturmak için aşağıdaki komutu kullanın:

    azure storage container create <CONTAINER_NAME> --account-name <STORAGE_ACCOUNT_NAME> --account-key <STORAGE_ACCOUNT_KEY>

> [!NOTE]
> Bir kapsayıcı oluşturmak, Data Lake depolama Gen2'ye bir dosya sistemi oluşturma ile eşanlamlıdır.

## <a name="address-files-in-azure-storage"></a>Azure depolamada dosyaları adresleme

HDInsight’ta Azure depolamadaki dosyalara erişmek için URI şeması aşağıdaki gibidir:

    abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>

URI şeması şifrelenmemiş erişim sağlar (ile *abfs:* önek) ve SSL şifreli erişim (ile *abfss*). Kullanmanızı öneririz *abfss* mümkün olduğunda, hatta azure'da aynı bölgede bulunan verilere erişirken.

* &lt;FILE_SYSTEM_NAME&gt; Data Lake depolama Gen2'ye dosya sistemi yolunu tanımlar.
* &lt;ACCOUNT_NAME&gt; Azure depolama hesabı adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

    Değilse değerleri &lt;FILE_SYSTEM_NAME&gt; ya da &lt;ACCOUNT_NAME&gt; belirtildiğini, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanabilirsiniz. Örneğin, *hadoop mapreduce examples.jar* HDInsight kümeleriyle gelen dosya başvurulabilir aşağıdaki yollardan birini kullanarak:
    
        abfs://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
        abfs:///example/jars/hadoop-mapreduce-examples.jar
        /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> HDInsight sürüm 2.1 ve 1.6 kümelerinde dosya adı *hadoop examples.jar* şeklindedir.

* &lt;Yolu&gt; dosya veya dizin HDFS yol adıdır.

> [!NOTE]
> HDInsight dışında dosyalarla çalışırken, yardımcı programların çoğu ABFS biçimlendirmek ve bunun yerine bir temel yol biçimi gibi bekler tanımaz `example/jars/hadoop-mapreduce-examples.jar`.
 
## <a name="use-additional-storage-accounts"></a>Ek depolama hesaplarını kullanma

HDInsight kümesi oluştururken ilişkilendirmek istediğiniz Azure Depolama hesabını belirtirsiniz. Bu depolama hesabına ek olarak, oluşturma işlemi sırasında veya bir küme oluşturulduktan sonra aynı Azure aboneliğinden veya farklı Azure aboneliklerinden başka depolama hesapları ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [HDInsight kümeleri oluşturma](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede HDFS ile uyumlu Azure Depolama'yı HDInsight ile nasıl kullanacağınızı öğrendiniz. Bu yaklaşım, ölçeklenebilir, uzun vadeli, arşivlemeli veri edinme çözümleri oluşturmanıza ve bilgileri depolanan yapılandırılmış ve yapılandırılmamış verileri kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure Data Lake depolama Gen2 ABFS Hadoop dosya sistemi sürücüsü](data-lake-storage-abfs-driver.md)
* [Azure Data Lake depolama Gen2'ye Giriş](data-lake-storage-introduction.md)
* [Azure Data Lake depolama Gen2 distcp kullanarak veri alma](data-lake-storage-use-distcp.md)

[powershell-install]: /powershell/azure/install-az-ps
[hdinsight-creation]: ../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md

[blob-storage-restAPI]: https://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/data-lake-storage-use-hdi-cluster/HDI.PowerShell.BlobCommands.png
