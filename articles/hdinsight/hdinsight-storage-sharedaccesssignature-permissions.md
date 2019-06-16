---
title: Paylaşılan erişim imzaları - Azure HDInsight'ı kullanarak erişimi kısıtlama
description: Azure storage bloblarında depolanan verilere HDInsight erişimi kısıtlamak için paylaşılan erişim imzaları'nı kullanmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: hrasheed
ms.openlocfilehash: 7f7f6fe31afe35d9ccfd6ee33617bd7e4fbe46b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65409553"
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a>HDInsight ile verilere erişimi kısıtlamak için Azure depolama paylaşılan erişim imzaları kullanma

HDInsight kümesi ile ilişkili Azure depolama hesaplarında veri tam erişimi vardır. Blob kapsayıcı paylaşılan erişim imzaları, verilere erişimi kısıtlamak için kullanabilirsiniz. Paylaşılan erişim imzaları (SAS), verilere erişimi sınırlamanıza olanak sağlayan bir Azure depolama hesapları özelliğidir. Örneğin, verilere yalnızca okuma erişimi sağlama.

> [!IMPORTANT]  
> Apache Ranger'ı kullanarak bir çözüm için etki alanına katılmış HDInsight kullanmayı düşünün. Daha fazla bilgi için [etki alanına katılmış HDInsight yapılandırma](./domain-joined/apache-domain-joined-configure.md) belge.

> [!WARNING]  
> HDInsight, küme için varsayılan depolama alanı için tam erişimi olmalıdır.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](./hdinsight-hadoop-linux-use-ssh-unix.md).

* Mevcut bir [depolama kapsayıcısı](../storage/blobs/storage-quickstart-blobs-portal.md).  

* PowerShell kullanarak, ihtiyacınız olacak [Az modül](https://docs.microsoft.com/powershell/azure/overview).

* Kullanmak isteyen eğitimcilere ve Azure CLI'yi henüz yüklemediyseniz, bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

* Kullanıyorsanız [Python](https://www.python.org/downloads/), sürüm 2.7 veya üstü.

* Kullanıyorsanız C#, Visual Studio 2013 veya üzeri bir sürüm olması gerekir.

* [URI şeması](./hdinsight-hadoop-linux-information.md#URI-and-scheme) depolama hesabınız için. Bu `wasb://` Azure depolama için `abfs://` için Azure Data Lake depolama Gen2'ye veya `adl://` Azure Data Lake depolama Gen1 için. Güvenli aktarım, Azure Depolama'da veya Data Lake depolama Gen2 için etkinse, URI olacaktır `wasbs://` veya `abfss://`sırasıyla ayrıca bakın [güvenli aktarım](../storage/common/storage-require-secure-transfer.md).

* Bir paylaşılan erişim imzası eklemek için mevcut bir HDInsight kümesine. Aksi durumda, küme oluşturma ve küme oluşturma sırasında bir paylaşılan erişim imzası eklemek için Azure PowerShell kullanabilirsiniz.

* Örnek dosyaları [ https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature ](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Bu depo, aşağıdaki öğeleri içerir:

  * Depolama kapsayıcı, depolanan ilke ve SAS, HDInsight ile kullanmak için oluşturabileceğiniz bir Visual Studio projesi
  * Depolama kapsayıcı, depolanan ilke ve SAS, HDInsight ile kullanmak için oluşturabileceğiniz bir Python betiği
  * Bir HDInsight kümesi oluşturma ve SAS'ı kullanacak şekilde yapılandırma PowerShell Betiği. Güncelleştirilmiş sürüm kullanılır daha aşağıda.
  * Bir örnek dosyası: `hdinsight-dotnet-python-azure-storage-shared-access-signature-master\sampledata\sample.log`

## <a name="shared-access-signatures"></a>Paylaşılan Erişim İmzaları

Paylaşılan erişim imzaları iki tür vardır:

* Geçici: Başlangıç zamanı, süre sonu ve SAS izinlerini tüm SAS URI öğesinde belirtilir.

* Depolanmış erişim ilkesini: Bir depolanmış erişim ilkesini bir blob kapsayıcısı gibi bir kaynak kapsayıcısı üzerinde tanımlanır. Bir ilke kısıtlamaları bir veya daha fazla paylaşılan erişim imzalarını yönetmek için kullanılabilir. Bir SAS bir depolanmış erişim ilkesini ile ilişkilendirdiğinizde, SAS kısıtlamaları - başlangıç zamanı, süre sonu ve izinleri için depolanmış erişim ilkesini tanımlanan - devralır.

Bir anahtar senaryosu için iki biçim arasındaki fark önemlidir: iptal etme. Bir SAS URL olduğundan herkes SAS alır, kimin başlangıç istendiğinde bağımsız olarak kullanabilirsiniz. Bir SAS yayımlandığını, herkes tarafından kullanılabilir. Dağıtılan bir SAS dört şeylerden biri oluşuncaya kadar geçerlidir:

1. SAS üzerinde belirtilen süre sonu ulaşıldı.

2. SAS'den başvurulan depolanmış erişim ilkesini belirtilen süre sonu ulaşıldı. Aşağıdaki senaryolarda erişilmesi gereken süre sonu neden:

    * Zaman aralığı geçti.
    * Depolanmış erişim ilkesini bir bitiş zamanı geçmişte sahip şekilde değiştirilir. Sona erme saati değiştirme SAS iptal etmek için bir yoludur.

3. SAS iptal etmek için başka bir yolu olan SAS tarafından başvurulan depolanmış erişim ilkesini silinir. Aynı ada sahip depolanmış erişim ilkesini yeniden oluşturma, önceki ilke için tüm SAS belirteçlerini (süre sonu SAS üzerinde değil geçtiyse) geçerli değil. SAS iptal etmek istiyorsanız, farklı bir ad kullanırsanız erişim ilkesi gelecekte bir sona erme saati ile yeniden emin olun.

4. SAS oluşturmak için kullanılan hesap anahtarı yeniden oluşturuldu. Anahtarı yeniden kimlik doğrulaması başarısız önceki anahtar kullanan tüm uygulamalar neden olur. Tüm bileşenler için yeni anahtarı güncelleştirin.

> [!IMPORTANT]  
> Paylaşılan erişim imzası URI'si imza oluşturmak için kullanılan hesap anahtarı ile ilişkilidir, ve ilişkili erişim ilkesi (varsa) depolanır. Hiçbir depolanmış erişim ilkesini belirtilirse, paylaşılan erişim imzası iptal etmek için tek yolu hesap anahtarını değiştirmektir.

Saklı erişim ilkeleri her zaman kullanmanızı öneririz. Saklı ilkeler kullanıldığında imzaları iptal veya sona erme tarihi gerektiği şekilde genişletin. Bu belgeyi kullanmak adımda SAS oluşturmak için erişim ilkeleri depolanır.

Paylaşılan erişim imzaları ile ilgili daha fazla bilgi için bkz [SAS modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

## <a name="create-a-stored-policy-and-sas"></a>Depolanan ilke ve SAS oluşturma

Her bir yöntemin sonunda üretilen bir SAS belirteci kaydedin. Belirteç, aşağıdakine benzer görünecektir:

```output
?sv=2018-03-28&sr=c&si=myPolicyPS&sig=NAxefF%2BrR2ubjZtyUtuAvLQgt%2FJIN5aHJMj6OsDwyy4%3D
```

### <a name="using-powershell"></a>PowerShell’i kullanma

Değiştirin `RESOURCEGROUP`, `STORAGEACCOUNT`, ve `STORAGECONTAINER` ile var olan depolama kapsayıcınızda için uygun değerleri. Dizini `hdinsight-dotnet-python-azure-storage-shared-access-signature-master` veya düzeltmek `-File` parametre için mutlak yol içerecek şekilde `Set-AzStorageblobcontent`. Aşağıdaki PowerShell komutunu girin:

```PowerShell
$resourceGroupName = "RESOURCEGROUP"
$storageAccountName = "STORAGEACCOUNT"
$containerName = "STORAGECONTAINER"
$policy = "myPolicyPS"

# Login to your Azure subscription
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Get the access key for the Azure Storage account
$storageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $storageAccountName)[0].Value

# Create an Azure Storage context
$storageContext = New-AzStorageContext `
                                -StorageAccountName $storageAccountName `
                                -StorageAccountKey $storageAccountKey

# Create a stored access policy for the Azure storage container
New-AzStorageContainerStoredAccessPolicy `
   -Container $containerName `
   -Policy $policy `
   -Permission "rl" `
   -ExpiryTime "12/31/2025 08:00:00" `
   -Context $storageContext

# Get the stored access policy or policies for the Azure storage container
Get-AzStorageContainerStoredAccessPolicy `
    -Container $containerName `
    -Context $storageContext

# Generates an SAS token for the Azure storage container
New-AzStorageContainerSASToken `
    -Name $containerName `
    -Policy $policy `
    -Context $storageContext

<# Removes a stored access policy from the Azure storage container
Remove-AzStorageContainerStoredAccessPolicy `
    -Container $containerName `
    -Policy $policy `
    -Context $storageContext
#>

# upload a file for a later example
Set-AzStorageblobcontent `
    -File "./sampledata/sample.log" `
    -Container $containerName `
    -Blob "samplePS.log" `
    -Context $storageContext
```

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Bu bölümdeki değişkenlerini bir Windows ortamına dayanır. Küçük farklılıklar bash veya diğer ortamlar için gerekli olacaktır.

1. Değiştirin `STORAGEACCOUNT`, ve `STORAGECONTAINER` ile var olan depolama kapsayıcınızda için uygun değerleri.

    ```azurecli
    # set variables
    set AZURE_STORAGE_ACCOUNT=STORAGEACCOUNT
    set AZURE_STORAGE_CONTAINER=STORAGECONTAINER

    #Login
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription SUBSCRIPTION

    # Retrieve the primary key for the storage account
    az storage account keys list --account-name %AZURE_STORAGE_ACCOUNT% --query "[0].{PrimaryKey:value}" --output table
    ```

2. Alınan birincil anahtarı daha sonra kullanmak için bir değişken ayarlayın. Değiştirin `PRIMARYKEY` ile alınan değeri önceki adımda ve ardından aşağıdaki komutu girin:

    ```azurecli
    #set variable for primary key
    set AZURE_STORAGE_KEY=PRIMARYKEY
    ```

3. Dizini `hdinsight-dotnet-python-azure-storage-shared-access-signature-master` veya düzeltmek `--file` parametre için mutlak yol içerecek şekilde `az storage blob upload`. Kalan şu komutları çalıştırın:

    ```azurecli
    # Create stored access policy on the containing object
    az storage container policy create --container-name %AZURE_STORAGE_CONTAINER% --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT% --expiry 2025-12-31 --permissions rl

    # List stored access policies on a containing object
    az storage container policy list --container-name %AZURE_STORAGE_CONTAINER% --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # Generate a shared access signature for the container
    az storage container generate-sas --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # Reversal
    # az storage container policy delete --container-name %AZURE_STORAGE_CONTAINER% --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # upload a file for a later example
    az storage blob upload --container-name %AZURE_STORAGE_CONTAINER% --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT% --name sampleCLI.log --file "./sampledata/sample.log"
    ```

### <a name="using-python"></a>Python’u kullanma

Açık `SASToken.py` değiştirin ve dosya `storage_account_name`, `storage_account_key`, ve `storage_container_name` uygun değerlerini, mevcut depolama kapsayıcısını ve betiği çalıştırın.

Yürütme gerekebilir `pip install --upgrade azure-storage` hata iletisi alırsanız `ImportError: No module named azure.storage`.

### <a name="using-c"></a>C# kullanma

1. Visual Studio içinde çözümü açın.

2. Çözüm Gezgini'nde sağ **SASExample** seçin ve proje **özellikleri**.

3. Seçin **ayarları** ve aşağıdaki girdileri için değerleri ekleyin:

   * StorageConnectionString: Depolanan ilke ve için SAS oluşturmak istediğiniz depolama hesabı için bağlantı dizesi. Biçim olmalıdır `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` burada `myaccount` depolama hesabınızın adı ve `mykey` depolama hesabı için anahtar.

   * Kapsayıcı adı: Erişimi kısıtlamak istediğiniz depolama hesabındaki kapsayıcı.

   * SASPolicyName: Saklı ilkesi oluşturmak için kullanılacak ad.

   * FileToUpload: Kapsayıcı için karşıya bir dosya yolu.

4. Projeyi çalıştırın. SAS İlkesi belirteç, depolama hesabı adı ve kapsayıcı adını kaydedin. HDInsight kümenizle depolama hesabını ilişkilendirerek bu değerler kullanılır.

## <a name="use-the-sas-with-hdinsight"></a>HDInsight ile SAS kullanın

Bir HDInsight kümesi oluştururken, birincil depolama hesabı belirtin ve isteğe bağlı olarak ek depolama hesapları belirtebilirsiniz. Depolama ekleme bu yöntemlerin ikisi de kullanılan kapsayıcıları ve depolama hesapları için tam erişim gerektirir.

Bir kapsayıcıya erişimi sınırlamak için bir paylaşılan erişim imzası kullanmak için özel bir girişe ekleme **çekirdek site** kümenin yapılandırmasını. Ambari kullanarak küme oluşturma PowerShell kullanarak küme oluşturma sırasında veya sonrasında giriş ekleyebilirsiniz.

### <a name="create-a-cluster-that-uses-the-sas"></a>SAS'ı kullanan bir kümesi oluşturma

Değiştirin `CLUSTERNAME`, `RESOURCEGROUP`, `DEFAULTSTORAGEACCOUNT`, `STORAGECONTAINER`, `STORAGEACCOUNT`, ve `TOKEN` uygun değerlerle. PowerShell komutları girin:

```powershell

$clusterName = 'CLUSTERNAME'
$resourceGroupName = 'RESOURCEGROUP'

# Replace with the Azure data center you want to the cluster to live in
$location = 'eastus'

# Replace with the name of the default storage account TO BE CREATED
$defaultStorageAccountName = 'DEFAULTSTORAGEACCOUNT'

# Replace with the name of the SAS container CREATED EARLIER
$SASContainerName = 'STORAGECONTAINER'

# Replace with the name of the SAS storage account CREATED EARLIER
$SASStorageAccountName = 'STORAGEACCOUNT'

# Replace with the SAS token generated earlier
$SASToken = 'TOKEN'

# Default cluster size (# of worker nodes), version, and type
$clusterSizeInNodes = "4"
$clusterVersion = "3.6"
$clusterType = "Hadoop"

# Login to your Azure subscription
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Create an Azure Storage account and container
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableHttpsTrafficOnly 1

$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value

$defaultStorageContext = New-AzStorageContext `
                                -StorageAccountName $defaultStorageAccountName `
                                -StorageAccountKey $defaultStorageAccountKey


# Create a blob container. This holds the default data store for the cluster.
New-AzStorageContainer `
    -Name $clusterName `
    -Context $defaultStorageContext 

# Cluster login is used to secure HTTPS services hosted on the cluster
$httpCredential = Get-Credential `
    -Message "Enter Cluster login credentials" `
    -UserName "admin"

# SSH user is used to remotely connect to the cluster using SSH clients
$sshCredential = Get-Credential `
    -Message "Enter SSH user credentials" `
    -UserName "sshuser"

# Create the configuration for the cluster
$config = New-AzHDInsightClusterConfig 

$config = $config | Add-AzHDInsightConfigValues `
    -Spark2Defaults @{} `
    -Core @{"fs.azure.sas.$SASContainerName.$SASStorageAccountName.blob.core.windows.net"=$SASToken}

# Create the HDInsight cluster
New-AzHDInsightCluster `
    -Config $config `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType $clusterType `
    -OSType Linux `
    -Version $clusterVersion `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultStorageContainer $clusterName

<# REVERSAL
Remove-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $clusterName

Remove-AzStorageContainer `
    -Name $clusterName `
    -Context $defaultStorageContext

Remove-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName

Remove-AzResourceGroup `
    -Name $resourceGroupName
#>
```

> [!IMPORTANT]  
> HTTP/s veya SSH kullanıcı adı ve parola istendiğinde, aşağıdaki ölçütlere uyan bir parola sağlamanız gerekir:
>
> * En az 10 karakter uzunluğunda olmalıdır.
> * En az bir rakam içermelidir.
> * En az bir alfasayısal olmayan karakter içermelidir.
> * En az bir büyük veya küçük harf içermelidir.

Bir süredir bu betik, tamamlanması genellikle yaklaşık 15 dakika sürer. Betik herhangi bir hata olmadan tamamlandığında, küme oluşturuldu.

### <a name="use-the-sas-with-an-existing-cluster"></a>SAS ile var olan bir küme kullanın

Mevcut bir kümeniz varsa SAS'ye ekleyebilirsiniz **çekirdek site** yapılandırmasını aşağıdaki adımları kullanarak:

1. Kümeniz için Ambari web kullanıcı arabirimini açın. Bu sayfa adresi `https://YOURCLUSTERNAME.azurehdinsight.net`. İstendiğinde, yönetici adı (Yönetici) kullanarak kümeye kimlik doğrulaması ve parola, kullanılan küme oluşturma.

2. Ambari web kullanıcı Arabirimi sol taraftan seçin **HDFS** seçip **yapılandırmaları** sayfanın ortasındaki sekmesi.

3. Seçin **Gelişmiş** sekmesine ve ardından bulana kadar kaydırın **özel çekirdek-site** bölümü.

4. Genişletin **özel çekirdek-site** bölümüne ve ardından seçin ve son gidin **Özellik Ekle...**  bağlantı. İçin aşağıdaki değerleri kullanın **anahtarı** ve **değer** alanlar:

   * **Anahtar**: `fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net`
   * **Değer**: Daha önce yürütülen yöntemlerden biri kullanılarak döndürülen SAS.

     Değiştirin `CONTAINERNAME` kapsayıcısı ile birlikte kullanılan adını C# veya SAS uygulama. Değiştirin `STORAGEACCOUNTNAME` ile kullanılan depolama hesabı adı.

5. Tıklayın **Ekle** bu anahtar ve değer Kaydet düğmesine ve ardından tıklayın **Kaydet** yapılandırma değişikliklerini kaydetmek için düğme. İstendiğinde, değişikliği ("SAS depolama erişim örneğin ekleme") bir açıklama ekleyin ve ardından **Kaydet**.

    Tıklayın **Tamam** zaman değişiklikleri tamamlanmıştır.

   > [!IMPORTANT]  
   > Değişikliğin etkili olmadan önce birkaç hizmeti yeniden başlatmanız gerekir.

6. Ambari web kullanıcı Arabirimi, seçin **HDFS** sol taraftaki listeden seçip **yeniden tüm etkilenen** gelen **hizmet eylemleri** listesi sağdaki aşağı açılır. Sorulduğunda, __tüm yeniden onaylayın__.

    MapReduce2 ve YARN için bu işlemi yineleyin.

7. Hizmetleri yeniden başlattıktan sonra her birini seçin ve Bakım modundan devre dışı **hizmet eylemleri** açılır.

## <a name="test-restricted-access"></a>Sınırlı erişimi test etme

SAS depolama hesabına yalnızca okuma ve liste öğeleri için doğrulamak için aşağıdaki adımları kullanın.

1. Kümeye bağlanın. Değiştirin `CLUSTERNAME` değerini kümenizin adıyla ve aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Kapsayıcı içeriğini listelemek için isteminden şu komutu kullanın:

    ```bash
    hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    Değiştirin `SASCONTAINER` SAS depolama hesabı için oluşturulan kapsayıcı adı. Değiştirin `SASACCOUNTNAME` SAS için kullanılan depolama hesabı adı ile.

    Listede, kapsayıcı ve SAS oluşturulduğunda karşıya yüklediğiniz dosyaya bulunur.

3. Dosyanın içeriğini okuyabilir doğrulamak için aşağıdaki komutu kullanın. Değiştirin `SASCONTAINER` ve `SASACCOUNTNAME` önceki adımla. Değiştirin `sample.log` önceki komutta gösterilen dosya adı:

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/sample.log
    ```

    Bu komut dosyasının içeriğini listeler.

4. Yerel dosya sistemine dosya indirmek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/sample.log testfile.txt
    ```

    Bu komut dosyası adlı bir yerel dosya yüklemeleri **testfile.txt**.

5. Yerel dosya adlı yeni bir dosya karşıya yüklemek için aşağıdaki komutu kullanın **testupload.txt** SAS depolama:

    ```bash
    hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    Aşağıdaki metne benzer bir ileti alırsınız:

        put: java.io.IOException

    Okuma + liste yalnızca depolama konumu olduğu için bu hata oluşur. Yazılabilir olduğundan kümenin varsayılan depolama üzerinde verileri yerleştirmek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -put testfile.txt wasbs:///testupload.txt
    ```

    Bu süre, işlem başarıyla tamamlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Sınırlı erişimli depolama, HDInsight kümenize eklemek öğrendiniz, kümenizde verilerle çalışmak için yollar öğrenin:

* [Apache Hive, HDInsight ile kullanma](hadoop/hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

