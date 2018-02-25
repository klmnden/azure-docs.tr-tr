---
title: "Hdınsight için Azure Kaynak Yöneticisi Araçları geçirme | Microsoft Docs"
description: "Hdınsight kümeleri için Azure Resource Manager geliştirme araçları geçirme"
services: hdinsight
editor: cgronlun
manager: jhubbard
author: nitinme
documentationcenter: 
ms.assetid: 05efedb5-6456-4552-87ff-156d77fbe2e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: 8ce1d6300731af5ae972675a08ef64f5c4ffa342
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Hdınsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme

Hdınsight, Hdınsight için Azure Service Manager ASM tabanlı araçlar kaldırmaktadır. Azure PowerShell, Azure CLI veya Hdınsight .NET SDK'sı Hdınsight kümeleriyle çalışmak için kullanıyorsanız, PowerShell'i, CLI ve .NET SDK'sı ileride Azure Resource Manager ARM tabanlı sürümleri kullanmanız önerilir. Bu makalede yeni ARM tabanlı yaklaşımı geçirmek nasıl işaretçileri sağlar. Uygun olduğunda, bu makalede ayrıca ASM ve ARM arasındaki farklar çıkışı yaklaşımlar Hdınsight için işaret eder.

> [!IMPORTANT]
> ASM desteği PowerShell'i, CLI, temel ve .NET SDK'yı Durdur **1 Ocak 2017**.
> 
> 

## <a name="migrating-azure-cli-to-azure-resource-manager"></a>Azure CLI Azure Kaynak Yöneticisi geçirme
Önceki bir yükleme Yükseltmekte olduğunuz sürece Azure CLI artık Azure Resource Manager (ARM) moduna varsayılan olarak ayarlanır; Bu durumda, kullanmanız gerekebilir `azure config mode arm` ARM moduna geçmek için komutu.

Azure Hizmet Yönetimi (ASM) kullanılarak Hdınsight ile çalışmak için Azure CLI sağlanan temel komutları ARM kullanırken aynıdır; Ancak bazı parametreler ve anahtarları yeni adlara sahip ve çok sayıda yeni parametreler kullanılabilir ARM kullanırken yoktur. Örneğin, artık kullanabilirsiniz `azure hdinsight cluster create` Azure sanal bir küme içinde oluşturulmalıdır veya Hive ağ ve Oozie meta depo bilgileri belirtmek için.

Hdınsight ile Azure Resource Manager ile çalışmak için temel komutlar şunlardır:

* `azure hdinsight cluster create` -Yeni bir Hdınsight kümesi oluşturur
* `azure hdinsight cluster delete` -Mevcut bir Hdınsight kümesine siler
* `azure hdinsight cluster show` -Varolan bir kümeye hakkındaki bilgileri görüntüleme
* `azure hdinsight cluster list` -Azure aboneliğiniz için Hdınsight kümeleri listeler

Kullanım `-h` parametreleri ve her komut için kullanılabilen anahtarlar incelemek için anahtar.

### <a name="new-commands"></a>Yeni komutları
Azure Resource Manager ile kullanılabilen yeni komutlar şunlardır:

* `azure hdinsight cluster resize` -Kümedeki çalışan düğümü sayısını dinamik olarak değiştirir
* `azure hdinsight cluster enable-http-access` -Küme HTTPs erişmesini sağlar (üzerinde varsayılan olarak)
* `azure hdinsight cluster disable-http-access` -kümesine HTTPs erişimi devre dışı bırakır
* `azure hdinsight script-action` -oluşturma/betik eylemleri bir kümede yönetmek için komutlar sağlar
* `azure hdinsight config` -bir yapılandırma dosyası oluşturma ile birlikte kullanılabilir komutlar sağlar `hdinsight cluster create` yapılandırma bilgilerini sağlamak için komutu.

### <a name="deprecated-commands"></a>Kullanım dışı komutları
Kullanırsanız `azure hdinsight job` Hdınsight kümenize iş göndermek için komutları bu ARM komutlarını kullanılabilir değil. İşlerini Hdınsight'a gelen komut dosyalarını program aracılığıyla göndermek gerekiyorsa, bunun yerine Hdınsight tarafından sağlanan REST API'lerini kullanmanız gerekir. REST API'lerini kullanarak işlerini göndermenin daha fazla bilgi için aşağıdaki belgelere bakın.

* [CURL kullanarak Hdınsight'ta Hadoop ile MapReduce işleri çalıştırma](hadoop/apache-hadoop-use-mapreduce-curl.md)
* [CURL kullanarak Hdınsight'ta Hadoop ile Hive sorguları çalıştırma](hadoop/apache-hadoop-use-hive-curl.md)
* [CURL kullanarak Hdınsight'ta Hadoop ile pig işleri çalıştırma](hadoop/apache-hadoop-use-pig-curl.md)

MapReduce çalıştırmak için diğer yöntemler hakkında bilgi, Hive ve etkileşimli olarak Pig için bkz: [hdınsight'ta Hadoop ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md), [hdınsight'ta Hadoop ile Hive kullanma](hadoop/hdinsight-use-hive.md), ve [Hadoop ile Pig kullanma Hdınsight](hadoop/hdinsight-use-pig.md).

### <a name="examples"></a>Örnekler
**Küme oluşturma**

* Eski komutu (ASM)- `azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Yeni komutu (ARM)- `azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**Küme silme**

* Eski komutu (ASM)- `azure hdinsight cluster delete myhdicluster`
* Yeni komutu (ARM)- `azure hdinsight cluster delete mycluster -g myresourcegroup`

**Liste kümeleri**

* Eski komutu (ASM)- `azure hdinsight cluster list`
* Yeni komutu (ARM)- `azure hdinsight cluster list`

> [!NOTE]
> Liste komutu için kaynak grubunu kullanarak belirtme `-g` yalnızca kümeler belirtilen kaynak grubunda döndürür.
> 
> 

**Küme bilgilerini göster**

* Eski komutu (ASM)- `azure hdinsight cluster show myhdicluster`
* Yeni komutu (ARM)- `azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-to-azure-resource-manager"></a>Azure Resource Manager Azure PowerShell geçirme
Azure Resource Manager (ARM) modunda Azure PowerShell hakkında genel bilgi şu adreste bulunabilir: [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

Azure PowerShell ARM cmdlet'leri yüklü yan yana ASM cmdlet'leri olabilir. İki modun cmdlet'leri, adlarına göre ayırt edilebilir.  ARM modu *AzureRmHDInsight* için karşılaştırma cmdlet'in adlarındaki *AzureHDInsight* ASM modunda.  Örneğin, *yeni AzureRmHDInsightCluster* vs. *New-AzureHDInsightCluster*. Parametreleri ve anahtarları haber adlara sahip ve çok sayıda yeni parametreler kullanılabilir ARM kullanırken yoktur.  Örneğin, birkaç cmdlet'leri adlı yeni bir anahtar gerektiren *- ResourceGroupName*. 

Hdınsight cmdlet'lerini kullanabilmek için Azure hesabınıza bağlanın ve yeni bir kaynak grubu oluşturmanız gerekir:

* Login-AzureRmAccount veya [seçin AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). Bkz: [bir hizmet sorumlusu Azure Resource Manager ile kimlik doğrulaması](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Yeniden adlandırılmış cmdlet'leri
Windows PowerShell konsolunda Hdınsight ASM cmdlet'leri listelemek için:

    help *azurermhdinsight*

Aşağıdaki tabloda, ASM cmdlet'leri ve ARM modunda adları listelenmektedir:

| ASM cmdlet'leri | ARM cmdlet'leri |
| --- | --- |
| Add-AzureHDInsightConfigValues |[Add-AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx) |
| Add-AzureHDInsightMetastore |[Add-AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx) |
| Add-AzureHDInsightScriptAction |[Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) |
| Add-AzureHDInsightStorage |[Add-AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx) |
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx) |
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx) |
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx) |
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Grant-AzureHDInsightHttpServicesAccess |[Grant-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx) |
| Grant-AzureHdinsightRdpAccess |[Grant-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx) |
| Invoke-AzureHDInsightHiveJob |[Invoke-AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx) |
| New-AzureHDInsightCluster |[New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) |
| New-AzureHDInsightClusterConfig |[New-AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx) |
| New-AzureHDInsightHiveJobDefinition |[New-AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx) |
| New-AzureHDInsightMapReduceJobDefinition |[New-AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| New-AzureHDInsightPigJobDefinition |[New-AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx) |
| New-AzureHDInsightSqoopJobDefinition |[New-AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[New-AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| Remove-AzureHDInsightCluster |[Remove-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx) |
| Revoke-AzureHDInsightHttpServicesAccess |[Revoke-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx) |
| Revoke-AzureHdinsightRdpAccess |[Revoke-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx) |
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx) |
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx) |
| Start-AzureHDInsightJob |[Start-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx) |
| Stop-AzureHDInsightJob |[Stop-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx) |
| Use-AzureHDInsightCluster |[Use-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx) |
| Wait-AzureHDInsightJob |[Wait-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a>Yeni cmdlet’ler
Yalnızca ARM modunda kullanılabilir yeni cmdlet'leri şunlardır: 

**Betik eylemi cmdlet'leri ilgili:**

* **Get-AzureRmHDInsightPersistedScriptAction**: Gets the persisted script actions for a cluster and lists them in chronological order, or gets details for a specified persisted script action. 
* **Get-AzureRmHDInsightScriptActionHistory**: Gets the script action history for a cluster and lists it in reverse chronological order, or gets details of a previously executed script action. 
* **Remove-AzureRmHDInsightPersistedScriptAction**: Removes a persisted script action from an HDInsight cluster.
* **Set-AzureRmHDInsightPersistedScriptAction**: Sets a previously executed script action to be a persisted script action.
* **Submit-AzureRmHDInsightScriptAction**: Submits a new script action to an Azure HDInsight cluster. 

Ek kullanım bilgileri için bkz: [özelleştirme Linux tabanlı Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

**Clsuter kimlik cmdlet'leri ilgili:**

* **Add-AzureRmHDInsightClusterIdentity**: Adds a cluster identity to a cluster configuration object so that the HDInsight cluster can access Azure Data Lake Stores. Bkz: [Azure PowerShell kullanarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Örnekler
Küme oluşturma

Eski komutu (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Yeni komut (ARM):

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


Küme silme

Eski komutu (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Yeni komut (ARM):

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

**Liste küme**

Eski komutu (ASM):

    Get-AzureHDInsightCluster

Yeni komut (ARM):

    Get-AzureRmHDInsightCluster 

**Küme Göster**

Eski komutu (ASM):

    Get-AzureHDInsightCluster -Name $clusterName

Yeni komut (ARM):

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a>Diğer örnekleri
* [Hdınsight kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Hive işlerini gönderme](hadoop/apache-hadoop-use-hive-powershell.md)
* [Pig işleri gönderme](hadoop/apache-hadoop-use-pig-powershell.md)
* [Sqoop işleri gönderme](hadoop/apache-hadoop-use-sqoop-powershell.md)

## <a name="migrating-to-the-arm-based-hdinsight-net-sdk"></a>ARM tabanlı Hdınsight .NET SDK'sı geçirme
Azure Hizmet Yönetimi-based [(ASM) Hdınsight .NET SDK'sı](https://msdn.microsoft.com/library/azure/mt416619.aspx) artık kullanım dışı bırakılmıştır. Azure kaynak yönetimi tabanlı kullanmaları [(ARM) Hdınsight .NET SDK'sı](https://msdn.microsoft.com/library/azure/mt271028.aspx). Aşağıdaki ASM tabanlı Hdınsight paketleri kullanım dışıdır.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Bu bölümde ARM tabanlı SDK'sını kullanarak bazı görevleri gerçekleştirmek hakkında daha fazla bilgi için işaretçiler sağlar.

| Nasıl yapılır... ARM tabanlı Hdınsight SDK kullanarak | Bağlantılar |
| --- | --- |
| .NET SDK kullanarak Hdınsight kümeleri oluşturma |Bkz: [Hdınsight kümeleri oluşturma .NET SDK kullanarak](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |
| .NET SDK'sı ile betik eylemi kullanarak bir küme özelleştirme |Bkz: [özelleştirme Hdınsight Linux kümeleri betik eylemi kullanarak](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| .NET SDK'sı ile etkileşimli olarak Azure Active Directory kullanarak uygulamaları kimlik doğrulaması |Bkz: [.NET SDK kullanarak Hive sorgularını çalıştırma](hadoop/apache-hadoop-use-hive-dotnet-sdk.md). Bu makaledeki kod parçacığında, etkileşimli kimlik doğrulaması yaklaşımı kullanır. |
| Uygulamaları etkileşimsiz .NET SDK ile Azure Active Directory'yi kullanarak kimlik doğrulaması |Bkz: [Hdınsight için etkileşimli olmayan uygulamalar oluşturma](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| .NET SDK kullanarak Hive işi Gönder |Bkz: [gönderme Hive işleri](hadoop/apache-hadoop-use-hive-dotnet-sdk.md) |
| .NET SDK kullanarak bir Pig işi gönderin |Bkz: [gönderme Pig işleri](hadoop/apache-hadoop-use-pig-dotnet-sdk.md) |
| .NET SDK kullanarak Sqoop işi Gönder |Bkz: [gönderme Sqoop işleri](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md) |
| .NET SDK kullanarak Hdınsight kümeleri listesi |Bkz: [listesi Hdınsight kümeleri](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| .NET SDK kullanarak Hdınsight kümelerini ölçeklendirme |Bkz: [ölçek Hdınsight kümeleri](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| .NET SDK kullanarak Hdınsight kümelerini GRANT/revoke erişimi |Bkz: [Hdınsight kümelerine Grant/revoke erişim](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| HTTP kullanıcı kimlik bilgilerini .NET SDK kullanarak Hdınsight kümelerini güncelleştirme |Bkz: [Hdınsight kümeleri için güncelleştirme HTTP kullanıcı kimlik bilgileri](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| .NET SDK kullanarak Hdınsight kümeleri için varsayılan depolama hesabı bulunamadı |Bkz: [Hdınsight kümeleri için varsayılan depolama hesabı bulunamadı](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| .NET SDK kullanarak Hdınsight kümelerini Sil |Bkz: [silmek Hdınsight kümeleri](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Örnekler
Nasıl bir işlem olduğunu ilişkin bazı örnekler şunlardır ASM tabanlı SDK'sı ve eşdeğer kod parçacığında için ARM tabanlı SDK kullanılarak gerçekleştirilir.

**Bir küme CRUD istemcisi oluşturma**

* Eski komutu (ASM)
  
        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Yeni komutu (ARM) (hizmet sorumlusu yetkilendirme)
  
        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* Yeni komutu (ARM) (kullanıcı kimlik doğrulaması)
  
        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

**Küme oluşturma**

* Eski komutu (ASM)
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* Yeni komutu (ARM)
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Linux,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

**HTTP erişimini etkinleştirme**

* Eski komutu (ASM)
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* Yeni komutu (ARM)
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Küme silme**

* Eski komutu (ASM)
  
        client.DeleteCluster(dnsName);
* Yeni komutu (ARM)
  
        client.Clusters.Delete(resourceGroup, dnsname);

