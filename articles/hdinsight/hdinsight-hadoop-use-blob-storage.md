<properties
    pageTitle="HDFS uyumlu Blob Storage’da veri sorgulama | Microsoft Azure"
    description="HDInsight HDFS için büyük veri deposu olarak Azure Blob Storage’ı kullanır. Blob Storage’da verileri sorgulamayı ve çözümleme sonuçlarınızı depolamayı öğrenin."
    keywords="blob depolama,hdfs,yapılandırılmış veriler,yapılandırılmamış veriler"
    services="hdinsight,storage"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="jgao"/>


# HDInsight’ta Hadoop ile HDFS uyumlu Azure Blob Storage kullanma

HDInsight ile düşük maliyetli Azure Blob Storage’ı kullanmayı, Azure Storage hesabı ve Blob Storage kapsayıcısı oluşturmayı ve sonra içindeki verileri işlemeyi öğrenin.

Azure Blob Storage HDInsight ile sorunsuz bir şekilde tümleşen, güçlü, genel amaçlı depolama çözümüdür. Hadoop dağıtılmış dosya sistemi (HDFS) arabirimi aracılığıyla, HDInsight’taki bileşenler kümesinin tümü Blob Storage’daki yapılandırılmış veya yapılandırılmamış veriler üzerinde doğrudan çalışabilir.

Verileri Blob Storage’da depolamak, işlem için kullanılan HDInsight kümelerini kullanıcı verilerini kaybetmeden güvenle silmenizi sağlar.

> [AZURE.IMPORTANT] HDInsight yalnızca blok blob'larını destekler. Blob sayfalamayı veya eklemeyi desteklemez.

HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz. [HDInsight kullanmaya başlama][hdinsight-get-started] veya [HDInsight kümeleri oluşturma][hdinsight-creation].


## HDInsight depolama mimarisi
Aşağıdaki diyagram, HDInsight depolama mimarisine ilişkin özet görünümünü sağlar:

![Hadoop kümeleri, Blob Storage’da yapılandırılmış ve yapılandırılmamış verilere erişmek ve depolamak için HDFS API’sini kullanır.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage Architecture")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

Ayrıca, HDInsight Azure Blob Storage’da depolanan verilere erişebilmeyi sağlar. Söz dizimi aşağıdaki gibidir:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

> [AZURE.NOTE] HDInsight’ın 3.0’dan önceki sürümlerinde `wasb://` yerine `asv://` kullanılmıştır. `asv://` bir hatayla sonuçlanacağı için HDInsight 3.0 veya üzeri kümelerle birlikte kullanılmamalıdır.

Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight oluşturma işlemi sırasında, bir Azure Storage hesabı ve bu hesaptan belirli bir Azure Blob Storage kapsayıcısı varsayılan dosya sistemi olarak atanır.

Bu depolama hesabına ek olarak, oluşturma işlemi sırasında aynı Azure aboneliğinden veya farklı Azure aboneliklerinden ek depolama hesapları ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [HDInsight kümeleri oluşturma][hdinsight-creation].

- **Bir kümeye bağlı depolama hesaplarındaki kapsayıcılar:** Oluşturma sırasında hesap adı ve anahtar kümeyle ilişkilendirildiğinden, bu kapsayıcılardaki blob'lara tam erişiminiz vardır.

- **Bir kümeye bağlı OLMAYAN depolama hesaplarındaki genel kapsayıcılar veya genel blob’lar:** Kapsayıcılardaki blob’lara salt okunur iznine sahipsiniz.

    > [AZURE.NOTE]
        > Genel kapsayıcılar, bu kapsayıcıda bulunan tüm blob’ların bir listesini ve kapsayıcı meta verilerini almanıza olanak tanır. Genel blob'lar, yalnızca tam URL'yi biliyorsanız blob erişiminize izin verir. Daha fazla bilgi için bkz. <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Kapsayıcılara ve blob'lara erişimi kısıtlama</a>.

- **Bir kümeye bağlı OLMAYAN depolama hesaplarındaki özel kapsayıcılar:** WebHCat işleri gönderdiğinizde, depolama hesabını tanımlamadığınız sürece kapsayıcılardaki blob’lara erişemezsiniz. Bu, bu makalenin sonraki bölümlerinde açıklanmıştır.


Oluşturma işleminde tanımlanan depolama hesapları ve bunların anahtarların %HADOOP_HOME%/conf/core-site.xml küme düğümlerinde depolanır. HDInsight’ın varsayılan davranışı core-site.xml dosyasında tanımlanan depolama hesaplarını kullanmaktır. Küme baş düğümü (ana) görüntüsü yeniden oluşturulabildiği veya taşınabildiği ve bu dosyalardaki değişiklikler kaybolacağı için core-site.xml dosyasının düzenlenmesi önerilmez.

Hive, MapReduce, Hadoop akış ve Pig dahil olmak üzere birden çok WebHCat işleri kendileriyle birlikte depolama hesapları açıklaması ve meta veriler taşıyabilir. (Bu şu anda yalnızca Pig depolama hesapları ile çalışmakta, ancak meta verilerle çalışmamaktadır.) Bu makalenin [Azure PowerShell kullanarak blob’lara erişme](#powershell) bölümünde bu makalede, bu özelliğin bir örneği yer almaktadır. Daha fazla bilgi için bkz. [Alternatif Depolama Hesapları ve Meta Depolarla HDInsight Kümesi kullanma](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Blob Storage yapılandırılmış ve yapılandırılmamış veriler için kullanılabilir. Blob Storage kapsayıcıları verileri anahtar/değer çiftleri olarak depolar ve dizin hiyerarşisi bulunmaz. Ancak, bir dosyayı dizin yapısında depolanmış gibi göstermek için anahtar adında eğik çizgi karakteri (/) kullanılabilir. Örneğin, bir blob'un anahtarı *input/log1.txt* şeklinde olabilir. Gerçek *giriş* dizini yoktur, ancak anahtar adında eğik çizgi karakteri bulunması nedeniyle, bir dosya yolu görünümüne sahiptir.

###<a id="benefits"></a>Blob Storage’ın avantajları
İşlem kümelerini ve depolama kaynaklarını yeniden bulmamanın örtülü performans maliyeti, yüksek hızlı ağın işlem düğümlerinin Azure Blob Storage’daki verilere erişimini çok verimli hale getirdiği, Azure bölgesindeki depolama hesabı kaynaklarına yakın şekilde oluşturulmasıyla azaltılabilir.

Verileri HDFS yerine Azure Blob Storage’da depolamanın çeşitli avantajları vardır:

* **Verileri yeniden kullanma ve paylaşma:** HDFS’deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Azure Blob Storage’da bulunan verilere HDFS API’si aracılığıyla veya [Blob Storage REST API'leri][blob-storage-restAPI] aracılığıyla erişilir. Bu nedenle, daha büyük uygulama kümeleri (diğer HDInsight kümeleri dahil olmak üzere) ve araçları veri üretmek ve kullanmak için kullanılabilir.
* **Veri arşivleme:**Verileri Azure Blob Storage’da depolamak, hesaplama için kullanılan HDInsight kümelerinin kullanıcı verilerini kaybetmeden güvenle silinmesini sağlar.
* **Veri depolama maliyeti:** Verileri DFS uzun süreli depolamak verileri Azure Blob Storage’da depolamaktan daha maliyetlidir; çünkü işlem kümesinin maliyeti Azure Blob Storage kapsayıcısının maliyetinden daha fazladır. Ayrıca, verilerin her işlem kümesi oluşturmada yeniden yüklenmesi gerekmediğinden, veri yükleme maliyetlerinden de tasarruf edersiniz.
* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi sağlamakla birlikte, ölçek, kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, Azure Blob Storage’da otomatik olarak aldığınız esnek ölçeklendirme özelliklere bağlı kalmaktan daha karmaşık bir işlem haline gelebilir.
* **Coğrafi çoğaltma:** Azure Blob Storage kapsayıcılarınız coğrafi olarak çoğaltılabilir. Bu, size coğrafi kurtarma ve veri yedekliği sağlamakla birlikte, coğrafi olarak çoğaltılan bir konuma yük devretmek performansınızı ciddi bir şekilde etkiler ve ek ücrete neden olabilir. Bu nedenle, coğrafi çoğaltmayı akıllıca ve yalnızca verilerin değeri ek maliyetlere değer durumdaysa tercih etmenizi öneririz.

Bazı MapReduce işleri ve paketleri gerçekte Azure Blob Storage’da depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS’de depolamak üzere seçebilirsiniz. Aslında, HDInsight Hive işleri ve diğer işlemlerdeki bu ara sonuçların bazıları için DFS kullanır.

> [AZURE.NOTE] Çoğu HDFS komutu (örneğin, <b>ls</b>, <b>copyFromLocal</b> ve <b>mkdir</b>) hala beklendiği gibi çalışmaktadır. Yalnızca, <b>fschk</b> ve <b>dfsadmin</b> gibi yerel HDFS uygulamasına özgü komutlar (DFS olarak adlandırılır), Azure Blob Storage’da farklı bir davranış gösterir.

## Blob kapsayıcıları oluşturma

Blob'ları kullanmak için önce [Azure Storage hesabı][azure-storage-create] oluşturmanız gerekir. Bunun bir parçası olarak, bu hesabı kullanarak oluşturduğunuz nesneleri depolayacağınız bir Azure bölgesi belirtirsiniz. Küme ve depolama hesabının aynı bölgede barındırılması gerekir. Hive meta depo SQL Server veritabanı ve Oozie meta depo SQL Server veritabanı da aynı bölgede bulunmalıdır.

Nerede olursa olsun, oluşturduğunuz her blob Azure Storage hesabınızdaki bir kapsayıcıya aittir. Bu kapsayıcı HDInsight dışında oluşturulmuş bir blob veya bir HDInsight kümesi için oluşturulan bir kapsayıcı olabilir.


Varsayılan Blob kapsayıcısı iş geçmişi ve günlükleri gibi kümeye özel bilgileri depolar. Varsayılan Blob kapsayıcısını birden çok HDInsight kümesiyle paylaşmayın. Bu iş geçmişinin bozulmasına ve kümenin işlememesine neden olabilir. Her küme için farklı bir kapsayıcı kullanmanız ve paylaşılan verileri varsayılan depolama hesabı yerine tüm ilgili kümelerin dağıtımında belirtilen bağlantılı depolama hesabına yerleştirmeniz önerilir. Bağlantılı Depolama hesaplarını yapılandırma hakkında daha fazla bilgi için bkz: [HDInsight kümeleri oluşturma][hdinsight-creation]. Ancak özgün HDInsight kümesi silindikten sonra varsayılan depolama kapsayıcısını yeniden kullanabilirsiniz. HBase kümeleri için, silinmiş bir HBase kümesi tarafından kullanılan varsayılan Blob Storage kapsayıcısını kullanan yeni bir HBase kümesi oluşturarak aslında HBase tablo şemasını ve verileri tutarsınız.


### Azure Portal’ı kullanma

Portal’da HDInsight kümesi oluştururken, varolan bir depolama hesabını kullanma veya yeni bir depolama hesabı oluşturma şeklinde için seçeneğiniz vardır:

![hdinsight hadoop creation data source](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

###Azure CLI’yı kullanma

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

[Azure CLI yüklenmiş ve yapılandırılmışsa](../xplat-cli-install.md), aşağıdaki komut bir depolama hesabı ve kapsayıcı için kullanılabilir.

    azure storage account create <storageaccountname> --type LRS

> [AZURE.NOTE] `--type` parametresi depolama hesabının nasıl çoğaltılacağını gösterir. Daha fazla bilgi için bkz. [Azure Storage çoğaltma](../storage/storage-redundancy.md). ZRS, sayfa blobu, dosya, tablo veya kuyruğu desteklemediğinden, ZRS kullanmayın.

Depolama hesabının bulunacağı coğrafi bölgeyi belirtmeniz istenir. HDInsight kümenizi oluşturmayı planladığınızla aynı bölgede depolama hesabı oluşturmanız gerekir.

Depolama hesabı oluşturulduğunda, depolama hesabı anahtarlarını almak için aşağıdaki komutu kullanın:

    azure storage account keys list <storageaccountname>

Bir kapsayıcı oluşturmak için aşağıdaki komutu kullanın:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

### Azure PowerShell’i kullanma

[ Azure PowerShell yüklenmiş ve yapılandırılmışsa][powershell install], bir depolama hesabı ve kapsayıcı oluşturmak için Azure PowerShell isteminde aşağıdakileri kullanabilirsiniz:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

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

## Blob Storage’da dosyaları adresleme

HDInsight’ta Blob Storage’daki dosyalara erişmek için URI şeması aşağıdaki gibidir:

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>


URI şeması şifrelenmemiş erişim (ile *wasb:* öneki ile) ve SSL şifreli erişim (*wasbs* ile) şifrelenmemiş erişim sağlar. Azure’da aynı bölgede bulunan verilere erişirken dahi mümkün olduğunda *wasbs* kullanmanızı öneririz.

&lt;BlobStorageContainerName&gt; Azure Blob Storage’da kapsayıcının adını tanımlar.
&lt;StorageAccountName&gt; Azure Storage hesabı adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

&lt;BlobStorageContainerName&gt; ya da &lt;StorageAccountName&gt; belirtilmediyse, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanabilirsiniz. Örneğin, HDInsight kümeleriyle gelen *hadoop mapreduce examples.jar* dosyasına aşağıdakilerden birini kullanarak başvurulabilir:

    wasbs://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasbs:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [AZURE.NOTE] HDInsight sürüm 2.1 ve 1.6 kümelerinde dosya adı <i>hadoop examples.jar</i> şeklindedir.


&lt;Yol&gt; dosya veya dizin HDFS yol adıdır. Azure Blob Storage’daki kapsayıcılar anahtar-değer depoları olduğundan, doğru hiyerarşik dosya sistemi yoktur. Bir blob anahtarındaki bir eğik çizgi karakteri (/) dizin ayırıcı olarak yorumlanır. Örneğin, *hadoop mapreduce examples.jar* için blob adı aşağıdaki gibidir:

    example/jars/hadoop-mapreduce-examples.jar

> [AZURE.NOTE] HDInsight dışındaki blob'larla çalışırken, yardımcı programların çoğu WASB biçimini tanımaz ve bunun yerine, `example/jars/hadoop-mapreduce-examples.jar` gibi temel yol biçimi gibi bekler.

## Azure CLI kullanarak blob’lara erişim

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

## Azure PowerShell kullanarak blob’lara erişim

> [AZURE.NOTE] Bu bölümdeki komutlar blob'larda depolanan verilere erişmek için PowerShell kullanmaya ilişkin temel bir örnek sağlar. HDInsight ile çalışmak için özelleştirilmiş daha tam özellikli bir örnek için bkz. [HDInsight Araçları](https://github.com/Blackmist/hdinsight-tools).

Blob ile ilgili cmdlet’leri listelemek için aşağıdaki komutu kullanın:

    Get-Command *blob*

![Blob’la ilgili PowerShell cmdlet’lerinin listesi.][img-hdi-powershell-blobcommands]

###Dosyaları karşıya yükleme

Bkz. [Verileri HDInsight’a yükleme][hdinsight-upload-data]

###Dosyaları indirme

Aşağıdaki betik, blok blobunu geçerli klasöre indirir. Betiği çalıştırmadan önce, dizini yazma izinlerine sahip olduğunuz bir klasör olarak değiştirin.

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

###Dosyaları silme


    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

###Dosyaları listeleme

    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

###Tanımlanmamış depolama hesabı kullanarak Hive sorgularını çalıştırma

Bu örnekte, oluşturma işlemi sırasında tanımlanmamış depolama hesabındaki klasörün nasıl listeleneceği gösterilmektedir.
$clusterName = "<HDInsightClusterName>"

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasbs://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

## Sonraki adımlar

Bu makalede, HDInsight ile HDFS uyumlu Azure Blob Storage’ı kullanmayı ve Azure Blob Storage’ın, HDInsight’ın temel bir bileşeni olduğunu öğrendiniz. Bu, Azure Blob Storage ile ölçeklenebilir, uzun vadeli, arşivleme verileri edinme çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış verilerdeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure HDInsight kullanmaya başlama][hdinsight-get-started]
* [Verileri HDInsight’a yükleme][hdinsight-upload-data]
* [HDInsight ile Hive kullanma ][hdinsight-use-hive]
* [HDInsight ile Pig kullanma ][hdinsight-use-pig]
* [HDInsight ile verilere erişimi kısıtlamak için Azure Storage Paylaşılan Erişim İmzaları kullanma][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell install]: ../powershell-install-configure.md
[hdinsight-creation]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  



<!--HONumber=Aug16_HO4-->


