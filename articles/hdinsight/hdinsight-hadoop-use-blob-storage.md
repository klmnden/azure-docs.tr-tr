---
title: "HDFS uyumlu Azure Depolama’da veri sorgulama | Microsoft Belgeleri"
description: "Analiz sonuçlarını kaydetmek üzere Azure depolama ve Azure Data Lake Store’dan veri sorgulamayı öğrenin."
keywords: "blob depolama,hdfs,yapılandırılmış veriler,yapılandırılmamış veriler,data lake store,Hadoop girdisi,Hadoop çıktısı, hadoop depolama, hdfs girdisi,hdfs çıktısı,hdfs depolama,wasb azure"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1d2e65f2-16de-449e-915f-3ffbc230f815
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: bc7707f3bbb6639699826550f39876d0096d6c03
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="use-hdfs-compatible-storage-with-hadoop-in-hdinsight"></a>HDInsight’ta Hadoop ile HDFS uyumlu depolama kullanma

HDInsight kümesindeki verileri çözümlemek için Azure Depolama, Azure Data Lake Store veya her ikisinde birden verileri depolayabilirsiniz. İki depolama seçeneği de işlem için kullanılan HDInsight kümelerini kullanıcı verilerini kaybetmeden güvenle silmenizi sağlar.

Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight kümesi oluşturma işlemi sırasında Azure Depolama’da bir blob kapsayıcıyı varsayılan dosya sistemi olarak belirtebilir veya HDInsight 3.5 ile Azure Depolama ya da Azure Data Lake Store'u varsayılan dosya sistemi olarak seçebilirsiniz.

Bu makalede, iki depolama seçeneğinin HDInsight kümeleri ile nasıl çalıştığı hakkında bilgi edinirsiniz. HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz. [HDInsight kullanmaya başlama](hdinsight-hadoop-linux-tutorial-get-started.md).

## <a name="using-azure-storage-with-hdinsight-clusters"></a>HDInsight kümeleri ile Azure depolamayı kullanma

Azure depolama, HDInsight ile sorunsuz bir şekilde tümleşen, sağlam ve genel amaçlı bir depolama çözümüdür. HDInsight, Azure Depolama’daki bir blob kapsayıcıyı kümenin varsayılan dosya sistemi olarak kullanabilir. Hadoop dağıtılmış dosya sistemi (HDFS) arabirimi aracılığıyla, HDInsight’taki bileşenler kümesinin tümü blob olarak depolanan yapılandırılmış veya yapılandırılmamış veriler üzerinde doğrudan çalışabilir.

> [!WARNING]
> Bir Azure depolama hesabı oluştururken çeşitli seçenekleri kullanabilirsiniz. Aşağıdaki tabloda HdInsight ile hangi seçeneklerin desteklendiği gösterilmektedir:
> 
> | Depolama hesabı türü | Depolama katmanı | HDInsight ile desteklenen |
> | ------- | ------- | ------- |
> | Genel amaçlı Depolama Hesabı | Standart | __Evet__ |
> | &nbsp; | Premium | Hayır |
> | Blob Depolama Hesabı | Sık Erişimli | Hayır |
> | &nbsp; | Seyrek Erişimli | Hayır |

### <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi
Aşağıdaki diyagram, HDInsight depolama mimarisine ilişkin özet görünümünü sağlar:

![Hadoop kümeleri, Blob Depolamada yapılandırılmış ve yapılandırılmamış verilere erişmek ve depolamak için HDFS API’sini kullanır.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Depolama Mimarisi")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

Ayrıca, HDInsight Azure Depolama’da depolanan verilere erişebilmeyi sağlar. Söz dizimi aşağıdaki gibidir:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

HDInsight kümeleriyle Azure Depolama hesabını kullanırken dikkat etmeniz gereken bazı noktalar mevcuttur.

* **Bir kümeye bağlı depolama hesaplarındaki kapsayıcılar:** Oluşturma sırasında hesap adı ve anahtar kümeyle ilişkilendirildiğinden, bu kapsayıcılardaki blob'lara tam erişiminiz vardır.

* **Bir kümeye bağlı OLMAYAN depolama hesaplarındaki genel kapsayıcılar veya genel blob’lar:** Kapsayıcılardaki blob’lara salt okunur iznine sahipsiniz.
  
  > [!NOTE]
  > Genel kapsayıcılar, bu kapsayıcıda bulunan tüm blob’ların bir listesini ve kapsayıcı meta verilerini almanıza olanak tanır. Genel blob'lar, yalnızca tam URL'yi biliyorsanız blob erişiminize izin verir. Daha fazla bilgi için bkz. <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Kapsayıcılara ve blob'lara erişimi kısıtlama</a>.
  > 
  > 
* **Bir kümeye bağlı OLMAYAN depolama hesaplarındaki özel kapsayıcılar:** WebHCat işleri gönderdiğinizde, depolama hesabını tanımlamadığınız sürece kapsayıcılardaki blob’lara erişemezsiniz. Bu, bu makalenin sonraki bölümlerinde açıklanmıştır.

Oluşturma işleminde tanımlanan depolama hesapları ve bunların anahtarların %HADOOP_HOME%/conf/core-site.xml küme düğümlerinde depolanır. HDInsight’ın varsayılan davranışı core-site.xml dosyasında tanımlanan depolama hesaplarını kullanmaktır. Küme baş düğümünün (ana) görüntüsü yeniden oluşturulabildiği veya taşınabildiği için ve bu dosyalardaki değişiklikler kaybolacağından core-site.xml dosyasının doğrudan düzenlenmesi önerilmez.

Hive, MapReduce, Hadoop akış ve Pig dahil olmak üzere birden çok WebHCat işleri kendileriyle birlikte depolama hesapları açıklaması ve meta veriler taşıyabilir. (Bu şu anda yalnızca Pig depolama hesapları ile çalışmakta, ancak meta verilerle çalışmamaktadır.) Bu makalenin [Azure PowerShell kullanarak blob’lara erişme](#powershell) bölümünde bu makalede, bu özelliğin bir örneği yer almaktadır. Daha fazla bilgi için bkz. [Alternatif Depolama Hesapları ve Meta Depolarla HDInsight Kümesi kullanma](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Bloblar yapılandırılmış ve yapılandırılmamış veriler için kullanılabilir. Blob kapsayıcıları, verileri anahtar/değer çiftleri olarak depolar ve dizin hiyerarşisi bulunmaz. Ancak, bir dosyayı dizin yapısında depolanmış gibi göstermek için anahtar adında eğik çizgi karakteri (/) kullanılabilir. Örneğin, bir blob'un anahtarı *input/log1.txt* şeklinde olabilir. Gerçek *giriş* dizini yoktur, ancak anahtar adında eğik çizgi karakteri bulunması nedeniyle, bir dosya yolu görünümüne sahiptir.

### <a id="benefits"></a>Azure Depolamanın yararları
İşlem kümelerini ve depolama kaynaklarını yeniden bulmamanın örtülü performans maliyeti, yüksek hızlı ağın işlem düğümlerinin Azure Depolama’daki verilere erişimini çok verimli hale getirdiği, Azure bölgesindeki depolama hesabı kaynaklarına yakın şekilde oluşturulmasıyla azaltılabilir.

Verileri HDFS yerine Azure Depolama’da depolamanın çeşitli avantajları vardır:

* **Verileri yeniden kullanma ve paylaşma:** HDFS’deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Azure depolamada bulunan verilere HDFS API'si aracılığıyla veya [Blob Depolama REST API'leri][blob-storage-restAPI] aracılığıyla erişilir. Bu nedenle, daha büyük uygulama kümeleri (diğer HDInsight kümeleri dahil olmak üzere) ve araçları veri üretmek ve kullanmak için kullanılabilir.
* **Veri arşivleme:** Verileri Azure depolamada depolamak, hesaplama için kullanılan HDInsight kümelerinin kullanıcı verilerini kaybetmeden güvenle silinmesini sağlar.
* **Veri depolama maliyeti:** Verileri DFS uzun süreli depolamak verileri Azure depolamada depolamaktan daha maliyetlidir; çünkü işlem kümesinin maliyeti Azure depolama maliyetinden daha fazladır. Ayrıca, verilerin her işlem kümesi oluşturmada yeniden yüklenmesi gerekmediğinden, veri yükleme maliyetlerinden de tasarruf edersiniz.
* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi sağlamakla birlikte, ölçek, kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, Azure depolamada otomatik olarak aldığınız esnek ölçeklendirme özelliklere bağlı kalmaktan daha karmaşık bir işlem haline gelebilir.
* **Coğrafi çoğaltma:** Azure depolamanız coğrafi olarak çoğaltılabilir. Bu, size coğrafi kurtarma ve veri yedekliği sağlamakla birlikte, coğrafi olarak çoğaltılan bir konuma yük devretmek performansınızı ciddi bir şekilde etkiler ve ek ücrete neden olabilir. Bu nedenle, coğrafi çoğaltmayı akıllıca ve yalnızca verilerin değeri ek maliyetlere değer durumdaysa tercih etmenizi öneririz.

Bazı MapReduce işleri ve paketleri gerçekte Azure depolamada depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS’de depolamak üzere seçebilirsiniz. Aslında, HDInsight Hive işleri ve diğer işlemlerdeki bu ara sonuçların bazıları için DFS kullanır.

> [!NOTE]
> Çoğu HDFS komutu (örneğin, <b>ls</b>, <b>copyFromLocal</b> ve <b>mkdir</b>) hala beklendiği gibi çalışmaktadır. Yalnızca, <b>fschk</b> ve <b>dfsadmin</b> gibi yerel HDFS uygulamasına özgü komutlar (DFS olarak adlandırılır), Azure depolamada farklı bir davranış gösterir.
> 
> 

### <a name="create-blob-containers"></a>Blob kapsayıcıları oluşturma
Blob'ları kullanmak için önce bir [Azure depolama hesabı][azure-storage-create] oluşturursunuz. Bunun bir parçası olarak depolama hesabının oluşturulduğu Azure bölgesini belirtirsiniz. Küme ve depolama hesabının aynı bölgede barındırılması gerekir. Hive meta depo SQL Server veritabanı ve Oozie meta depo SQL Server veritabanı da aynı bölgede bulunmalıdır.

Nerede olursa olsun, oluşturduğunuz her blob Azure Storage hesabınızdaki bir kapsayıcıya aittir. Bu kapsayıcı HDInsight dışında oluşturulmuş bir blob veya bir HDInsight kümesi için oluşturulan bir kapsayıcı olabilir.

Varsayılan Blob kapsayıcısı iş geçmişi ve günlükleri gibi kümeye özel bilgileri depolar. Varsayılan Blob kapsayıcısını birden çok HDInsight kümesiyle paylaşmayın. Bu durum iş geçmişinin bozulmasına neden olabilir. Her küme için farklı bir kapsayıcı kullanmanız ve paylaşılan verileri varsayılan depolama hesabı yerine tüm ilgili kümelerin dağıtımında belirtilen bağlantılı depolama hesabına yerleştirmeniz önerilir. Bağlantılı Depolama hesaplarını yapılandırma hakkında daha fazla bilgi için bkz: [HDInsight kümeleri oluşturma][hdinsight-creation]. Ancak özgün HDInsight kümesi silindikten sonra varsayılan depolama kapsayıcısını yeniden kullanabilirsiniz. HBase kümeleri için, silinmiş bir HBase kümesi tarafından kullanılan varsayılan blob kapsayıcısını kullanan yeni bir HBase kümesi oluşturarak aslında HBase tablo şemasını ve verileri tutarsınız.

#### <a name="using-the-azure-portal"></a>Azure portalını kullanma
Portal’da HDInsight kümesi oluştururken, depolama hesabı ayrıntılarını girmek için aşağıdaki seçenekleriniz vardır. Kümeyle ilişkili ek bir depolama hesabı isteyip istemediğinizi de belirtebilirsiniz. İstiyorsanız, ek depolama alanı olarak Data Lake Store veya başka bir Azure Depolama Blobu seçebilirsiniz.

![HDInsight hadoop oluşturma veri kaynağı](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

#### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

[Azure CLI yüklenmiş ve yapılandırılmışsa](../cli-install-nodejs.md), aşağıdaki komut bir depolama hesabı ve kapsayıcı için kullanılabilir.

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> `--type` parametresi depolama hesabının nasıl çoğaltıldığını gösterir. Daha fazla bilgi için bkz. [Azure Storage çoğaltma](../storage/storage-redundancy.md). ZRS, sayfa blobu, dosya, tablo veya kuyruğu desteklemediğinden, ZRS kullanmayın.
> 
> 

Depolama hesabının oluşturulacağı coğrafi bölgeyi belirtmeniz istenir. HDInsight kümenizi oluşturmayı planladığınızla aynı bölgede depolama hesabı oluşturmanız gerekir.

Depolama hesabı oluşturulduğunda, depolama hesabı anahtarlarını almak için aşağıdaki komutu kullanın:

    azure storage account keys list <storageaccountname>

Bir kapsayıcı oluşturmak için aşağıdaki komutu kullanın:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

#### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma
[Azure PowerShell yüklenmiş ve yapılandırılmışsa][powershell-install], bir depolama hesabı ve kapsayıcı oluşturmak için Azure PowerShell isteminde aşağıdakileri kullanabilirsiniz:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

### <a name="address-files-in-azure-storage"></a>Azure depolamada dosyaları adresleme
HDInsight’ta Azure depolamadaki dosyalara erişmek için URI şeması aşağıdaki gibidir:

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

URI şeması şifrelenmemiş erişim (ile *wasb:* öneki ile) ve SSL şifreli erişim (*wasbs* ile) şifrelenmemiş erişim sağlar. Azure’da aynı bölgede bulunan verilere erişirken dahi mümkün olduğunda *wasbs* kullanmanızı öneririz.

&lt;BlobStorageContainerName&gt; Azure depolamada blob kapsayıcının adını tanımlar.
&lt;StorageAccountName&gt; Azure Storage hesabı adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

&lt;BlobStorageContainerName&gt; ya da &lt;StorageAccountName&gt; belirtilmediyse, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanabilirsiniz. Örneğin, HDInsight kümeleriyle gelen *hadoop mapreduce examples.jar* dosyasına aşağıdakilerden birini kullanarak başvurulabilir:

    wasbs://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasbs:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> HDInsight sürüm 2.1 ve 1.6 kümelerinde dosya adı <i>hadoop examples.jar</i> şeklindedir.
> 
> 

&lt;Yol&gt; dosya veya dizin HDFS yol adıdır. Azure depolamadaki kapsayıcılar anahtar-değer depoları olduğundan, doğru hiyerarşik dosya sistemi yoktur. Bir blob anahtarındaki bir eğik çizgi karakteri (/) dizin ayırıcı olarak yorumlanır. Örneğin, *hadoop mapreduce examples.jar* için blob adı aşağıdaki gibidir:

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> HDInsight dışındaki blob'larla çalışırken, yardımcı programların çoğu WASB biçimini tanımaz ve bunun yerine, `example/jars/hadoop-mapreduce-examples.jar` gibi temel yol biçimi gibi bekler.
> 
> 

### <a name="access-blobs-using-azure-cli"></a>Azure CLI kullanarak blob’lara erişim
Blob ile ilgili komutları listelemek için aşağıdaki komutu kullanın:

    azure storage blob

**Bir dosyayı karşıya yüklemek için Azure CLI kullanma örneği**

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Bir dosyayı indirmek için Azure CLI kullanma örneği**

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

**Bir dosyayı silmek için Azure CLI kullanma örneği**

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Dosyaları listelemek için Azure CLI kullanma örneği**

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

### <a name="access-blobs-using-azure-powershell"></a>Azure PowerShell kullanarak blob’lara erişim
> [!NOTE]
> Bu bölümdeki komutlar blob'larda depolanan verilere erişmek için PowerShell kullanmaya ilişkin temel bir örnek sağlar. HDInsight ile çalışmak için özelleştirilmiş daha tam özellikli bir örnek için bkz. [HDInsight Araçları](https://github.com/Blackmist/hdinsight-tools).
> 
> 

Blob ile ilgili cmdlet’leri listelemek için aşağıdaki komutu kullanın:

    Get-Command *blob*

![Blob’la ilgili PowerShell cmdlet’lerinin listesi.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a>Dosyaları karşıya yükleme
Bkz. [HDInsight'a veri yükleme][hdinsight-upload-data].

#### <a name="download-files"></a>Dosyaları indirme
Aşağıdaki betik geçerli klasöre bir blok blobu indirir. Betiği çalıştırmadan önce, dizini yazma izinlerine sahip olduğunuz bir klasör olarak değiştirin.

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    # Use Add-AzureAccount if you haven't connected to your Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List the downloaded file ..." -ForegroundColor Green
    cat "./$blob"

Kaynak grubu adını ve küme adını vererek, aşağıdaki kodu kullanabilirsiniz:

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force

#### <a name="delete-files"></a>Dosyaları silme
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a>Dosyaları listeleme
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a>Tanımlanmamış depolama hesabı kullanarak Hive sorgularını çalıştırma
Bu örnekte, oluşturma işlemi sırasında tanımlanmamış depolama hesabındaki klasörün nasıl listeleneceği gösterilmektedir.
$clusterName = "<HDInsightClusterName>"

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasbs://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"


### <a name="using-additional-storage-accounts"></a>Ek depolama hesabı kullanma

HDInsight kümesi oluştururken ilişkilendirmek istediğiniz Azure Depolama hesabını belirtirsiniz. Bu depolama hesabına ek olarak, oluşturma işlemi sırasında veya bir küme oluşturulduktan sonra aynı Azure aboneliğinden veya farklı Azure aboneliklerinden başka depolama hesapları ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

## <a name="using-azure-data-lake-store-with-hdinsight-clusters"></a>Azure Data Lake Store’u HDInsight kümeleri ile kullanma

HDInsight kümeleri Azure Data Lake Store’u iki şekilde kullanabilir:

* Varsayılan depolama alanı olarak Azure Data Lake Store
* Varsayılan depolama alanı olarak Azure Depolama Blobu, ek depolama alanı olarak Azure Data Lake Store.

> [!NOTE]
> Azure Data Lake Store’a her zaman güvenli bir kanal üzerinden erişildiğinden `adls` dosya sistemi düzen adı yoktur. Her zaman `adl` kullanırsınız.
> 
> 

### <a name="using-azure-data-lake-store-as-default-storage"></a>Varsayılan depolama alanı olarak Azure Data Lake Store kullanma

HDInsight ile varsayılan depolama alanı olarak Azure Data Lake Store dağıtıldığında, kümeyle ilişkili dosyalar Azure Data Lake Store içinde şu konumda depolanır:

    adl://mydatalakestore/<cluster_root_path>/

`<cluster_root_path>` Azure Data Lake Store içinde oluşturduğunuz klasörün adıdır. Her küme için kök yolu belirterek, birden fazla küme için aynı Azure Data Lake Store hesabını kullanabilirsiniz. Bunu yaptığınızda şöyle bir durum olabilir:

* Cluster1 `adl://mydatalakestore/cluster1storage` yolunu kullanabilir.
* Cluster2 `adl://mydatalakestore/cluster2storage` yolunu kullanabilir.

Her iki kümenin aynı **mydatalakestore** Data Lake Store hesabını kullandığına dikkat edin. Her küme Data Lake Store içinde kendi kök dosya sistemine erişebilir. Özellikle Azure portalı dağıtımı deneyimi sizden kök yol olarak **/clusters/\<clustername>** gibi bir klasör adı kullanmanızı ister.

#### <a name="accessing-files-from-the-cluster"></a>Dosyalara kümeden erişme

Azure Data Lake Store dosyalarına HDInsight kümesinden erişmenin birkaç yolu vardır.

* **Tam adı kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın tam yolunu girersiniz.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Kısaltılmış yol biçimi kullanarak**. Bu yöntemle, yolu küme kökü ve adl ile değiştirirsiniz: / / /. Yukarıdaki örnekte `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` ile `adl:///` değerini değiştirebilirsiniz.

        adl:///<file path>

* **Göreli yolu kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın yalnızca göreli yolunu girersiniz. Örneğin dosyanın tam yolu şöyleyse:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    Aynı sample.log dosyasına göreli yolu kullanarak erişebilirsiniz.

        /example/data/sample.log

### <a name="using-azure-data-lake-store-as-additional-storage"></a>Azure Data Lake Store’u ek depolama alanı olarak kullanma

Data Lake Store'u da küme için ek depolama alanı olarak kullanabilirsiniz. Böyle durumlarda kümenin varsayılan depolama alanı Azure Depolama Blobu veya Azure Data Lake Store hesabı olabilir. HDInsight işlerini ek depolama alanı olarak Azure Data Lake Store'da depolanan veriler için kullanıyorsanız dosyaların tam tolunu belirtmeniz gerekir. Örneğin:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Artık URL'de **cluster_root_path** olmadığını unutmayın. Bunun nedeni Data Lake Store’un artık varsayılan depolama alanı olmamasıdır. Artık tüm yapmanız gereken dosyaların yolunu belirtmektir.


### <a name="creating-hdinsight-clusters-with-access-to-data-lake-store"></a>Data Lake Store erişimi olan HDInsight kümeleri oluşturma

Data Lake Store erişimi olan HDInsight kümeleri oluşturma hakkında ayrıntılı yönergeler için aşağıdaki bağlantıları izleyin.

* [Portalı kullanma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [PowerShell kullanma (varsayılan depolama alanı olarak Data Lake Store ile)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [PowerShell kullanma (ek depolama alanı olarak Data Lake Store ile)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Azure şablonlarını kullanma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, HDFS uyumlu Azure depolama ve HDInsight ile Azure Data Lake Store kullanmayı öğrendiniz. Bu, ölçeklenebilir, uzun vadeli, arşivlemeli veri edinme çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış verilerdeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]
* [Azure Data Lake Store ile çalışmaya başlama](../data-lake-store/data-lake-store-get-started-portal.md)
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [HDInsight ile Hive kullanma][hdinsight-use-hive]
* [HDInsight ile Pig kullanma][hdinsight-use-pig]
* [HDInsight ile verilere erişimi kısıtlamak için Azure Depolama Paylaşılan Erişim İmzaları kullanma][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  

