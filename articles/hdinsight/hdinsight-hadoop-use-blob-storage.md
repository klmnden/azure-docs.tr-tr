---
title: HDFS uyumlu Azure Depolama'da veri sorgulama - Azure HDInsight
description: Azure depolama ve analiz sonuçlarınızı depolamak için Azure Data Lake Storage verilerini sorgulamayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/08/2019
ms.openlocfilehash: 3356d3eee00a640efe10e2d9f3aa4fa7be775995
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59360788"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile Azure Depolama'yı kullanma

HDInsight kümesindeki verileri çözümlemek için veri ya da depolayabilirsiniz içinde [Azure depolama](../storage/common/storage-introduction.md), [Azure Data Lake depolama Gen 1](../data-lake-store/data-lake-store-overview.md)/[Azure Data Lake depolama Gen 2](../storage/blobs/data-lake-storage-introduction.md), veya bir birleşimi. Bu depolama seçeneklerinin, kullanıcı verilerini kaybetmeden hesaplama için kullanılan HDInsight kümelerini güvenle silmenizi sağlar.

Apache Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight kümesi oluşturma işlemi sırasında varsayılan dosya sistemi olarak Azure storage'da bir blob kapsayıcısı belirtebilirsiniz veya HDInsight 3.6 ile Azure depolama veya Azure Data Lake depolama Gen 1 / Azure Data Lake seçebilirsiniz depolama Gen 2 varsayılan dosyaları Sistem birkaç özel durum. Hem varsayılan hem de bağlı depolama olarak Data Lake depolama Gen 1 kullanmanın desteklenebilirliği için bkz: [HDInsight kümesi için kullanılabilirlik](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

Bu makalede Azure Depolama'nın HDInsight kümeleri ile nasıl çalıştığı hakkında bilgi edinebilirsiniz. Data Lake depolama Gen 1 HDInsight kümeleriyle nasıl çalıştığı hakkında bilgi için bkz: [kullanımı Azure Data Lake Storage, Azure HDInsight kümeleri](hdinsight-hadoop-use-data-lake-store.md). HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz. [Apache Hadoop kümeleri oluşturma HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

Azure depolama, HDInsight ile sorunsuz bir şekilde tümleşen, sağlam ve genel amaçlı bir depolama çözümüdür. HDInsight, Azure Depolama’daki bir blob kapsayıcıyı kümenin varsayılan dosya sistemi olarak kullanabilir. Hadoop dağıtılmış dosya sistemi (HDFS) arabirimi aracılığıyla, HDInsight’taki bileşenler kümesinin tümü blob olarak depolanan yapılandırılmış veya yapılandırılmamış veriler üzerinde doğrudan çalışabilir.

> [!WARNING]  
> Azure Depolama hesabı oluşturmanın birden fazla yolu vardır. Aşağıdaki tabloda HdInsight ile hangi seçeneklerin desteklendiği gösterilmektedir:

| Depolama hesabı türü | Desteklenen hizmetler | Desteklenen performans katmanları | Desteklenen erişim katmanları |
|----------------------|--------------------|-----------------------------|------------------------|
| Genel amaçlı V2   | Blob               | Standart                    | Sık erişimli, seyrek erişimli ve Arşiv\*    |
| Genel amaçlı V1   | Blob               | Standart                    | Yok                    |
| Blob depolama         | Blob               | Standart                    | Sık erişimli, seyrek erişimli ve Arşiv\*    |

İş verilerini depolamak için, varsayılan blob kapsayıcısını kullanmanızı önermiyoruz. Depolama maliyetini azaltmak için blob kapsayıcısının her kullanımdan sonra silinmesi iyi bir uygulamadır. Uygulama ve sistem varsayılan kapsayıcı içeren günlükleri. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Bir blob kapsayıcısının birden fazla küme için varsayılan dosya sistemi olarak paylaşılması desteklenmez.
 
 > [!NOTE]  
 > Arşiv erişim katmanı bir birkaç saatlik alma gecikmesinin ve HDInsight ile kullanım için önerilmez çevrimdışı bir katmandır. Daha fazla bilgi için <a href="https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers#archive-access-tier">arşiv erişim katmanı</a>.

Depolama hesabınızın güvenliğini sağlamak tercih ederseniz **güvenlik duvarları ve sanal ağlar** kısıtlamalar **seçili ağlar**, özel durum etkinleştirdiğinizden emin olun **güvenilen Microsoft izin ver Hizmetleri...**  HDInsight depolama hesabınıza erişebilmesi için.

## <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi
Aşağıdaki diyagram, Azure Depolama ile kullanılan HDInsight depolama mimarisine ilişkin bir özet görünüm sağlar:

![Hadoop kümeleri, Blob Depolamada yapılandırılmış ve yapılandırılmamış verilere erişmek ve depolamak için HDFS API’sini kullanır.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Depolama Mimarisi")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

Ayrıca HDInsight, Azure Depolama'da depolanan verilere erişebilmenizi de sağlar. Söz dizimi aşağıdaki gibidir:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

HDInsight kümeleriyle Azure Depolama hesabını kullanırken dikkat etmeniz gereken bazı noktalar mevcuttur.

* **Bir kümeye bağlı depolama hesaplarındaki kapsayıcılar:** Oluşturma sırasında hesap adı ve anahtar kümeyle ilişkili olduğundan bu kapsayıcılardaki blob'lara tam erişiminiz vardır.

* **Genel kapsayıcılar veya bir kümeye bağlı olmayan depolama hesaplarındaki genel BLOB'lar:** Kapsayıcılardaki blob'lara salt okunur iznine sahip.
  
  > [!NOTE]  
  > Genel kapsayıcılar, bu kapsayıcıda bulunan tüm blob’ların bir listesini ve kapsayıcı meta verilerini almanıza olanak tanır. Genel blob'lar, yalnızca tam URL'yi biliyorsanız blob erişiminize izin verir. Daha fazla bilgi için <a href="https://docs.microsoft.com/azure/storage/blobs/storage-manage-access-to-resources">kapsayıcılara ve blob'lara erişimi yönetme</a>.

* **Bir kümeye bağlı olmayan depolama hesaplarındaki özel kapsayıcılar:** WebHCat işleri gönderdiğinizde depolama hesabını tanımlamadığınız sürece kapsayıcılardaki blob'lara erişemezsiniz. Bu, bu makalenin sonraki bölümlerinde açıklanmıştır.

Oluşturma işleminde tanımlanan depolama hesapları ve bunların anahtarların %HADOOP_HOME%/conf/core-site.xml küme düğümlerinde depolanır. HDInsight’ın varsayılan davranışı core-site.xml dosyasında tanımlanan depolama hesaplarını kullanmaktır. Bu ayarı kullanarak değiştirebilirsiniz [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Depolama hesapları ve bunların meta veri açıklamasını Apache Hive, MapReduce, akış Apache Hadoop ve Apache Pig dahil olmak üzere birden çok WebHCat işleri gerçekleştirebilirsiniz. (Bu şu anda yalnızca Pig depolama hesapları ile çalışmakta, ancak meta verilerle çalışmamaktadır.) Daha fazla bilgi için bkz. [Alternatif Depolama Hesapları ve Meta Depolarla HDInsight Kümesi kullanma](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Bloblar yapılandırılmış ve yapılandırılmamış veriler için kullanılabilir. Blob kapsayıcıları, verileri anahtar/değer çiftleri olarak depolar ve dizin hiyerarşisi bulunmaz. Ancak, bir dosyayı dizin yapısında depolanmış gibi göstermek için anahtar adında eğik çizgi karakteri (/) kullanılabilir. Örneğin, bir blob'un anahtarı *input/log1.txt* şeklinde olabilir. Gerçek *giriş* dizini yoktur, ancak anahtar adında eğik çizgi karakteri bulunması nedeniyle, bir dosya yolu görünümüne sahiptir.

## <a id="benefits"></a>Azure Depolamanın yararları
İşlem kümelerini ve depolama kaynaklarını yeniden bulmamanın örtülü performans maliyeti, yüksek hızlı ağın işlem düğümlerinin Azure Depolama’daki verilere erişimini verimli hale getirdiği, Azure bölgesindeki depolama hesabı kaynaklarına yakın şekilde oluşturulmasıyla azaltılabilir.

Verileri HDFS yerine Azure Depolama’da depolamanın çeşitli avantajları vardır:

* **Verileri yeniden kullanma ve paylaşma:** HDFS'deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Azure depolamada bulunan verilere HDFS API'si aracılığıyla veya [Blob Depolama REST API'leri][blob-storage-restAPI] aracılığıyla erişilir. Bu nedenle, daha büyük uygulama kümeleri (diğer HDInsight kümeleri dahil olmak üzere) ve araçları veri üretmek ve kullanmak için kullanılabilir.
* **Veri arşivleme:** Verileri Azure depolamada depolamak, hesaplama için kullanılan kullanıcı verilerini kaybetmeden güvenle silinmesini HDInsight kümeleri sağlar.
* **Veri depolama maliyeti:** Verileri DFS uzun süreli depolamak, işlem kümesinin maliyeti Azure depolama maliyeti yüksek olduğu için verileri Azure depolamada depolamaktan daha maliyetlidir. Ayrıca, verilerin her işlem kümesi oluşturmada yeniden yüklenmesi gerekmediğinden, veri yükleme maliyetlerinden de tasarruf edersiniz.
* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi ile sağlar ancak ölçek kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, Azure depolamada otomatik olarak aldığınız esnek ölçeklendirme özelliklere bağlı kalmaktan daha karmaşık bir işlem haline gelebilir.
* **Coğrafi çoğaltma:** Azure depolama, coğrafi olarak çoğaltılmış olabilir. Bu, size coğrafi kurtarma ve veri yedekliği sağlamakla birlikte, coğrafi olarak çoğaltılan bir konuma yük devretmek performansınızı ciddi bir şekilde etkiler ve ek ücrete neden olabilir. Bu nedenle, coğrafi çoğaltmayı akıllıca ve yalnızca verilerin değeri ek maliyetlere değer durumdaysa tercih etmenizi öneririz.

Bazı MapReduce işleri ve paketleri gerçekte Azure depolamada depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS’de depolamak üzere seçebilirsiniz. Aslında, HDInsight Hive işleri ve diğer işlemlerdeki bu ara sonuçların bazıları için DFS kullanır.

> [!NOTE]  
> Çoğu HDFS komutu (örneğin, <b>ls</b>, <b>copyFromLocal</b> ve <b>mkdir</b>) hala beklendiği gibi çalışmaktadır. Yalnızca, <b>fschk</b> ve <b>dfsadmin</b> gibi yerel HDFS uygulamasına özgü komutlar (DFS olarak adlandırılır), Azure depolamada farklı bir davranış gösterir.


## <a name="create-blob-containers"></a>Blob kapsayıcıları oluşturma
Blob'ları kullanmak için önce bir [Azure depolama hesabı][azure-storage-create] oluşturursunuz. Bunun bir parçası olarak depolama hesabının oluşturulduğu Azure bölgesini belirtirsiniz. Küme ve depolama hesabının aynı bölgede barındırılması gerekir. Hive meta depo SQL Server veritabanı ve Apache Oozie meta depo SQL Server veritabanı da aynı bölgede bulunmalıdır.

Nerede olursa olsun, oluşturduğunuz her blob Azure Storage hesabınızdaki bir kapsayıcıya aittir. Bu kapsayıcı HDInsight dışında oluşturulmuş bir blob veya bir HDInsight kümesi için oluşturulan bir kapsayıcı olabilir.

Varsayılan Blob kapsayıcısı iş geçmişi ve iş günlükleri gibi kümeye özel bilgileri depolar. Varsayılan Blob kapsayıcısını birden çok HDInsight kümesiyle paylaşmayın. Bu durum iş geçmişinin bozulmasına neden olabilir. Her küme için farklı bir kapsayıcı kullanmanız ve paylaşılan verileri varsayılan depolama hesabı yerine tüm ilgili kümelerin dağıtımında belirtilen bağlantılı depolama hesabına yerleştirmeniz önerilir. Bağlantılı Depolama hesaplarını yapılandırma hakkında daha fazla bilgi için bkz: [HDInsight kümeleri oluşturma][hdinsight-creation]. Ancak özgün HDInsight kümesi silindikten sonra varsayılan depolama kapsayıcısını yeniden kullanabilirsiniz. HBase kümeleri için, silinmiş bir HBase kümesi tarafından kullanılan varsayılan blob kapsayıcısını kullanan yeni bir HBase kümesi oluşturarak HBase tablo şemasını ve verileri tutabilirsiniz.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a>Azure portalı kullanma
Portal’da HDInsight kümesi oluştururken, depolama hesabı ayrıntılarını girmek için aşağıdaki seçenekleriniz vardır. Ayrıca, ek bir depolama hesabı, kümeyle ilişkili ve bu durumda, Data Lake Storage ek depolama alanı olarak başka bir Azure depolama blobu seçin veya isteyip istemediğinizi de belirtebilirsiniz.

![HDInsight hadoop oluşturma veri kaynağı](./media/hdinsight-hadoop-use-blob-storage/storage.png)

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.


### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure PowerShell yüklenmiş ve yapılandırılmışsa][powershell-install], bir depolama hesabı ve kapsayıcı oluşturmak için Azure PowerShell isteminde aşağıdakileri kullanabilirsiniz:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Connect-AzAccount
    Select-AzSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzStorageContainer -Name $containerName -Context $destContext

### <a name="use-azure-classic-cli"></a>Klasik Azure CLI kullanma

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

Varsa [Azure Klasik CLI yükleyip](../cli-install-nodejs.md), aşağıdaki komut bir depolama hesabı ve kapsayıcı için kullanılabilir.

```cli
azure storage account create <storageaccountname> --type LRS
```

> [!NOTE]  
> `--type` parametresi depolama hesabının nasıl çoğaltıldığını gösterir. Daha fazla bilgi için bkz. [Azure Storage çoğaltma](../storage/storage-redundancy.md). ZRS, sayfa blobu, dosya, tablo veya kuyruğu desteklemediğinden, ZRS kullanmayın.

Depolama hesabının oluşturulacağı coğrafi bölgeyi belirtmeniz istenir. HDInsight kümenizi oluşturmayı planladığınızla aynı bölgede depolama hesabı oluşturmanız gerekir.

Depolama hesabı oluşturulduğunda, depolama hesabı anahtarlarını almak için aşağıdaki komutu kullanın:

```cli
azure storage account keys list <storageaccountname>
```

Bir kapsayıcı oluşturmak için aşağıdaki komutu kullanın:

```cli
azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>
```

## <a name="address-files-in-azure-storage"></a>Azure depolamada dosyaları adresleme
HDInsight’ta Azure depolamadaki dosyalara erişmek için URI şeması aşağıdaki gibidir:

```config
wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>
```

URI şeması şifrelenmemiş erişim (ile *wasb:* öneki ile) ve SSL şifreli erişim (*wasbs* ile) şifrelenmemiş erişim sağlar. Azure’da aynı bölgede bulunan verilere erişirken dahi mümkün olduğunda *wasbs* kullanmanızı öneririz.

&lt;BlobStorageContainerName&gt; Azure depolamada blob kapsayıcının adını tanımlar.
&lt;StorageAccountName&gt; Azure Storage hesabı adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

&lt;BlobStorageContainerName&gt; ya da &lt;StorageAccountName&gt; belirtilmediyse, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanabilirsiniz. Örneğin, HDInsight kümeleriyle gelen *hadoop mapreduce examples.jar* dosyasına aşağıdakilerden birini kullanarak başvurulabilir:

```config
wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
wasb:///example/jars/hadoop-mapreduce-examples.jar
/example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> HDInsight sürüm 2.1 ve 1.6 kümelerinde dosya adı <i>hadoop examples.jar</i> şeklindedir.

&lt;Yol&gt; dosya veya dizin HDFS yol adıdır. Azure depolamadaki kapsayıcılar anahtar-değer depoları olduğundan, doğru hiyerarşik dosya sistemi yoktur. Bir blob anahtarındaki bir eğik çizgi karakteri (/) dizin ayırıcı olarak yorumlanır. Örneğin, *hadoop mapreduce examples.jar* için blob adı aşağıdaki gibidir:

```bash
example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> HDInsight dışındaki blob'larla çalışırken, yardımcı programların çoğu WASB biçimini tanımaz ve bunun yerine, `example/jars/hadoop-mapreduce-examples.jar` gibi temel yol biçimi gibi bekler.

## <a name="access-blobs"></a>Bloblara erişme

### <a name="access-blobs-using-azure-powershell"></a> Azure PowerShell'i kullanma

> [!NOTE]
> 
> Bu bölümdeki komutlar blob'larda depolanan verilere erişmek için PowerShell kullanmaya ilişkin temel bir örnek sağlar. HDInsight ile çalışmak için özelleştirilmiş daha tam özellikli bir örnek için bkz. [HDInsight Araçları](https://github.com/Blackmist/hdinsight-tools).

Blob ile ilgili cmdlet’leri listelemek için aşağıdaki komutu kullanın:

```powershell 
Get-Command *blob*
```

![Blob’la ilgili PowerShell cmdlet’lerinin listesi.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a>Dosyaları karşıya yükleme

Bkz. [HDInsight'a veri yükleme][hdinsight-upload-data].

#### <a name="download-files"></a>Dosyaları indirme

Aşağıdaki betik geçerli klasöre bir blok blobu indirir. Betiği çalıştırmadan önce, dizini yazma izinlerine sahip olduğunuz bir klasör olarak değiştirin.

```powershell
$resourceGroupName = "<AzureResourceGroupName>"
$storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
$containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
$blob = "example/data/sample.log" # The name of the blob to be downloaded.

# Use Add-AzureAccount if you haven't connected to your Azure subscription
Connect-AzAccount 
Select-AzSubscription -SubscriptionID "<Your Azure Subscription ID>"

Write-Host "Create a context object ... " -ForegroundColor Green
$storageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
$storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

Write-Host "Download the blob ..." -ForegroundColor Green
Get-AzStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

Write-Host "List the downloaded file ..." -ForegroundColor Green
cat "./$blob"
```

Kaynak grubu adını ve küme adını vererek, aşağıdaki kodu kullanabilirsiniz:

```powershell
$resourceGroupName = "<AzureResourceGroupName>"
$clusterName = "<HDInsightClusterName>"
$blob = "example/data/sample.log" # The name of the blob to be downloaded.

$cluster = Get-AzHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
$defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
$defaultStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
$defaultStorageContainer = $cluster.DefaultStorageContainer
$storageContext = New-AzStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

Write-Host "Download the blob ..." -ForegroundColor Green
Get-AzStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force
```

#### <a name="delete-files"></a>Dosyaları silme

```powershell
Remove-AzStorageBlob -Container $containerName -Context $storageContext -blob $blob
```

#### <a name="list-files"></a>Dosyaları listeleme

```powershell
Get-AzStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"
```

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a>Tanımlanmamış depolama hesabı kullanarak Hive sorgularını çalıştırma

Bu örnekte, oluşturma işlemi sırasında tanımlanmamış depolama hesabındaki klasörün nasıl listeleneceği gösterilmektedir.

```powershell
$clusterName = "<HDInsightClusterName>"

$undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
$undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

$undefinedStorageKey = Get-AzStorageKey $undefinedStorageAccount | %{ $_.Primary }

Use-AzHDInsightCluster $clusterName

$defines = @{}
$defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

Invoke-AzHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"
```

### <a name="use-azure-classic-cli"></a>Klasik Azure CLI kullanma

Blob ile ilgili komutları listelemek için aşağıdaki komutu kullanın:

```cli
azure storage blob
```

**Bir dosyayı karşıya yüklemek için Klasik Azure CLI kullanma örneği**

```cli
azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>
```

**Bir dosyayı indirmek için Klasik Azure CLI kullanma örneği**

```cli
azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>
```

**Bir dosyayı silmek için Klasik Azure CLI kullanma örneği**

```cli
azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>
```

**Dosyaları listelemek için Klasik Azure CLI kullanma örneği**

```cli
azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>
```

## <a name="use-additional-storage-accounts"></a>Ek depolama hesaplarını kullanma

HDInsight kümesi oluştururken ilişkilendirmek istediğiniz Azure Depolama hesabını belirtirsiniz. Bu depolama hesabına ek olarak, oluşturma işlemi sırasında veya bir küme oluşturulduktan sonra aynı Azure aboneliğinden veya farklı Azure aboneliklerinden başka depolama hesapları ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede HDFS ile uyumlu Azure Depolama'yı HDInsight ile nasıl kullanacağınızı öğrendiniz. Bu, ölçeklenebilir, uzun vadeli, arşivlemeli veri edinme çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış verilerdeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]
* [Azure Data Lake Store ile çalışmaya başlama](../data-lake-store/data-lake-store-get-started-portal.md)
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [Apache Hive, HDInsight ile kullanma][hdinsight-use-hive]
* [Apache Pig, HDInsight ile kullanma][hdinsight-use-pig]
* [HDInsight ile verilere erişimi kısıtlamak için Azure Depolama Paylaşılan Erişim İmzaları kullanma][hdinsight-use-sas]
* [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[blob-storage-restAPI]: https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
