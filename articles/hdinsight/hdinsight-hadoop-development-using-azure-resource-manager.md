---
title: Hdınsight için Azure Kaynak Yöneticisi Araçları geçirme | Microsoft Docs
description: Hdınsight kümeleri için Azure Resource Manager geliştirme araçları geçirme
services: hdinsight
editor: cgronlun
manager: jhubbard
author: nitinme
documentationcenter: ''
ms.assetid: 05efedb5-6456-4552-87ff-156d77fbe2e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: b0a73ea89bec67cbf644cce60913981a0533360a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32179699"
---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Hdınsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme

Hdınsight, Hdınsight için Azure Service Manager ASM tabanlı araçlar kaldırmaktadır. Azure PowerShell, Azure CLI veya Hdınsight .NET SDK'sı Hdınsight kümeleriyle çalışmak için kullanıyorsanız, PowerShell'i, CLI ve .NET SDK'sı ileride Azure Resource Manager sürümleri kullanmanız önerilir. Bu makalede yeni Resource Manager tabanlı yaklaşımı geçirmek nasıl işaretçileri sağlar. Uygun olduğunda, bu belgede Hdınsight ASM ve Resource Manager yaklaşımlar arasındaki farklar vurgular.

> [!IMPORTANT]
> ASM desteği PowerShell'i, CLI, temel ve .NET SDK'yı Durdur **1 Ocak 2017**.
> 
> 

## <a name="migrating-azure-cli-to-azure-resource-manager"></a>Azure CLI Azure Kaynak Yöneticisi geçirme

> [!IMPORTANT]
> Azure CLI 2.0, Hdınsight kümeleriyle çalışmak için destek sağlamaz. Azure CLI 1.0 kullanım dışıdır ancak Azure CLI 1.0 Hdınsight ile kullanmaya devam edebilirsiniz.

Azure CLI 1.0 üzerinden Hdınsight ile çalışmak için temel komutları şunlardır:

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
Kullanırsanız `azure hdinsight job` komutların Hdınsight kümenize iş göndermek için bu komutları aracılığıyla Resource Manager komutlar kullanılabilir değil. İşlerini Hdınsight'a gelen komut dosyalarını program aracılığıyla göndermek gerekiyorsa, bunun yerine Hdınsight tarafından sağlanan REST API'lerini kullanmanız gerekir. REST API'lerini kullanarak işlerini göndermenin daha fazla bilgi için aşağıdaki belgelere bakın.

* [CURL kullanarak Hdınsight'ta Hadoop ile MapReduce işleri çalıştırma](hadoop/apache-hadoop-use-mapreduce-curl.md)
* [CURL kullanarak Hdınsight'ta Hadoop ile Hive sorguları çalıştırma](hadoop/apache-hadoop-use-hive-curl.md)
* [CURL kullanarak Hdınsight'ta Hadoop ile pig işleri çalıştırma](hadoop/apache-hadoop-use-pig-curl.md)

MapReduce çalıştırmak için diğer yöntemler hakkında bilgi, Hive ve etkileşimli olarak Pig için bkz: [hdınsight'ta Hadoop ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md), [hdınsight'ta Hadoop ile Hive kullanma](hadoop/hdinsight-use-hive.md), ve [Hadoop ile Pig kullanma Hdınsight](hadoop/hdinsight-use-pig.md).

### <a name="examples"></a>Örnekler
**Küme oluşturma**

* Eski komutu (ASM)- `azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Yeni komut- `azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**Küme silme**

* Eski komutu (ASM)- `azure hdinsight cluster delete myhdicluster`
* Yeni komut- `azure hdinsight cluster delete mycluster -g myresourcegroup`

**Liste kümeleri**

* Eski komutu (ASM)- `azure hdinsight cluster list`
* Yeni komut- `azure hdinsight cluster list`

> [!NOTE]
> Liste komutu için kaynak grubunu kullanarak belirtme `-g` yalnızca kümeler belirtilen kaynak grubunda döndürür.
> 
> 

**Küme bilgilerini göster**

* Eski komutu (ASM)- `azure hdinsight cluster show myhdicluster`
* Yeni komut- `azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-to-azure-resource-manager"></a>Azure Resource Manager Azure PowerShell geçirme
Azure Resource Manager moduna Azure PowerShell hakkında genel bilgi şu adreste bulunabilir: [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

Azure PowerShell'i Resource Manager cmdlet'lerini ASM cmdlet'leri yan yana yüklenebilir. İki modun cmdlet'leri, adlarına göre ayırt edilebilir.  Resource Manager moduna sahip *AzureRmHDInsight* için karşılaştırma cmdlet'in adlarındaki *AzureHDInsight* ASM modunda.  Örneğin, *yeni AzureRmHDInsightCluster* vs. *AzureHDInsightCluster yeni*. Parametreleri ve anahtarları haber adlara sahip ve yok birçok yeni parametreler Kaynak Yöneticisi'ni kullanırken.  Örneğin, birkaç cmdlet'leri adlı yeni bir anahtar gerektiren *- ResourceGroupName*. 

Hdınsight cmdlet'lerini kullanabilmek için Azure hesabınıza bağlanın ve yeni bir kaynak grubu oluşturmanız gerekir:

* Connect-AzureRmAccount veya [seçin AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). Bkz: [bir hizmet sorumlusu Azure Resource Manager ile kimlik doğrulaması](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Yeniden adlandırılmış cmdlet'leri
Windows PowerShell konsolunda Hdınsight ASM cmdlet'leri listelemek için:

    help *azurermhdinsight*

ASM cmdlet'leri ve Resource Manager moduna adlarında aşağıdaki tabloda listelenmektedir:

| ASM cmdlet'leri | Resource Manager cmdlet'lerini |
| --- | --- |
| Add-AzureHDInsightConfigValues |[Ekleme AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx) |
| Add-AzureHDInsightMetastore |[Ekleme AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx) |
| Add-AzureHDInsightScriptAction |[Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) |
| Add-AzureHDInsightStorage |[Ekleme AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx) |
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx) |
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx) |
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx) |
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Grant-AzureHDInsightHttpServicesAccess |[GRANT-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx) |
| Grant-AzureHdinsightRdpAccess |[GRANT-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx) |
| Invoke-AzureHDInsightHiveJob |[Çağırma AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx) |
| New-AzureHDInsightCluster |[New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) |
| New-AzureHDInsightClusterConfig |[New-AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx) |
| New-AzureHDInsightHiveJobDefinition |[AzureRmHDInsightHiveJobDefinition yeni](https://msdn.microsoft.com/library/mt619448.aspx) |
| New-AzureHDInsightMapReduceJobDefinition |[AzureRmHDInsightMapReduceJobDefinition yeni](https://msdn.microsoft.com/library/mt603626.aspx) |
| New-AzureHDInsightPigJobDefinition |[New-AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx) |
| New-AzureHDInsightSqoopJobDefinition |[New-AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[AzureRmHDInsightStreamingMapReduceJobDefinition yeni](https://msdn.microsoft.com/library/mt603626.aspx) |
| Remove-AzureHDInsightCluster |[Remove-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx) |
| Revoke-AzureHDInsightHttpServicesAccess |[REVOKE-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx) |
| Revoke-AzureHdinsightRdpAccess |[REVOKE-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx) |
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx) |
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx) |
| Start-AzureHDInsightJob |[Start-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx) |
| Stop-AzureHDInsightJob |[Stop-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx) |
| Use-AzureHDInsightCluster |[Kullanım AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx) |
| Wait-AzureHDInsightJob |[Wait-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a>Yeni cmdlet’ler
Yalnızca Kaynak Yöneticisi modunda kullanılabilir yeni cmdlet'leri şunlardır: 

**Betik eylemi ile ilgili cmdlet'leri:**

* **Get-AzureRmHDInsightPersistedScriptAction**: Gets the persisted script actions for a cluster and lists them in chronological order, or gets details for a specified persisted script action. 
* **Get-AzureRmHDInsightScriptActionHistory**: Gets the script action history for a cluster and lists it in reverse chronological order, or gets details of a previously executed script action. 
* **Remove-AzureRmHDInsightPersistedScriptAction**: Removes a persisted script action from an HDInsight cluster.
* **Set-AzureRmHDInsightPersistedScriptAction**: kalıcı betik eylemi olması için daha önce yürütülen betik eylemi ayarlar.
* **Gönderme AzureRmHDInsightScriptAction**: Azure Hdınsight kümesi için yeni bir betik eylemi gönderir. 

Ek kullanım bilgileri için bkz: [özelleştirme Linux tabanlı Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

**Kimlikle ilgili cmdlet'leri kümesi:**

* **Ekleme AzureRmHDInsightClusterIdentity**: Hdınsight kümesi Azure Data Lake depoları erişebilmesi için bir küme kimliği bir küme yapılandırma nesnesine ekler. Bkz: [Azure PowerShell kullanarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Örnekler
**Küme oluşturma**

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

Yeni komut:

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


**Küme silme**

Eski komutu (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Yeni komut:

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

**Liste küme**

Eski komutu (ASM):

    Get-AzureHDInsightCluster

Yeni komut:

    Get-AzureRmHDInsightCluster 

**Küme Göster**

Eski komutu (ASM):

    Get-AzureHDInsightCluster -Name $clusterName

Yeni komut:

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a>Diğer örnekleri
* [Hdınsight kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Hive işlerini gönderme](hadoop/apache-hadoop-use-hive-powershell.md)
* [Pig işleri gönderme](hadoop/apache-hadoop-use-pig-powershell.md)
* [Sqoop işleri gönderme](hadoop/apache-hadoop-use-sqoop-powershell.md)

## <a name="migrating-to-the-new-hdinsight-net-sdk"></a>Yeni bir Hdınsight .NET SDK geçirme
Azure Hizmet Yönetimi-based [(ASM) Hdınsight .NET SDK'sı](https://msdn.microsoft.com/library/azure/mt416619.aspx) artık kullanım dışı bırakılmıştır. Azure kaynak yönetimi tabanlı kullanmaları [Resource Manager tabanlı Hdınsight .NET SDK'sı](https://msdn.microsoft.com/library/azure/mt271028.aspx). Aşağıdaki ASM tabanlı Hdınsight paketleri kullanım dışıdır.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Bu bölümde Resource Manager tabanlı SDK'sını kullanarak bazı görevleri gerçekleştirmek hakkında daha fazla bilgi için işaretçiler sağlar.

| Nasıl yapılır... Resource Manager tabanlı Hdınsight SDK kullanarak | Bağlantılar |
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
Nasıl bir işlem olduğunu ilişkin bazı örnekler şunlardır ASM tabanlı SDK'sı ve eşdeğer kod parçacığında için Resource Manager tabanlı SDK kullanılarak gerçekleştirilir.

**Bir küme CRUD istemcisi oluşturma**

* Eski komutu (ASM)
  
        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Yeni komutu (hizmet sorumlusu yetkilendirme)
  
        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* Yeni komutu (kullanıcı kimlik doğrulaması)
  
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
* Yeni komutu
  
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
* Yeni komutu
  
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
* Yeni komutu
  
        client.Clusters.Delete(resourceGroup, dnsname);

