---
title: HDInsight için Azure Resource Manager araçları geçirme | Microsoft Docs
description: HDInsight kümeleri için Azure Resource Manager geliştirme araçlarına geçiş yapma
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
ms.openlocfilehash: 7234341fd63f4e841ac8d15a51293148d2c60975
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001880"
---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçiş

HDInsight, HDInsight için Azure Service Manager ASM tabanlı araçlar kaldırmaktadır. Azure PowerShell, Azure CLI veya HDInsight .NET SDK'ın HDInsight kümeleriyle çalışmak için kullandığınız, PowerShell, CLI ve .NET SDK'sı ileride Azure Resource Manager sürümleri kullanmanız önerilir. Bu makalede, Resource Manager tabanlı yeni yaklaşıma için geçiş yapmaya yönelik işaretçiler sağlar. Geçerli olduğunda, bu belgede ASM ve Resource Manager yaklaşım HDInsight için arasındaki farklar vurgulanmaktadır.

> [!IMPORTANT]
> ASM desteği, PowerShell, CLI, temel alır ve .NET SDK'sı üzerinde sona erecek **1 Ocak 2017**.
> 
> 

## <a name="migrating-azure-cli-to-azure-resource-manager"></a>Azure CLI, Azure Resource Manager'a geçiş

> [!IMPORTANT]
> Azure CLI 2.0, HDInsight kümeleriyle çalışmak için destek sağlamaz. Azure CLI 1.0 kullanım dışıdır, ancak yine de HDInsight ile Azure CLI 1.0 kullanabilirsiniz.

HDInsight ile Azure CLI 1.0 ile çalışmak için temel komutlar şunlardır:

* `azure hdinsight cluster create` -Yeni bir HDInsight kümesi oluşturur
* `azure hdinsight cluster delete` -Mevcut bir HDInsight kümesini siler
* `azure hdinsight cluster show` -Mevcut bir kümeye hakkındaki bilgileri görüntüleme
* `azure hdinsight cluster list` -Azure aboneliğiniz için HDInsight kümelerini listeler

Kullanım `-h` parametreler ve anahtarlar her komut için kullanılabilir incelemek için anahtar.

### <a name="new-commands"></a>Yeni komutlar
Azure Resource Manager ile kullanılabilen yeni komutlar şunlardır:

* `azure hdinsight cluster resize` -Kümedeki çalışan düğümü sayısını dinamik olarak değiştirir
* `azure hdinsight cluster enable-http-access` -Küme için HTTPs erişim sağlar (üzerinde varsayılan olarak)
* `azure hdinsight cluster disable-http-access` -Küme HTTPs erişimi devre dışı bırakır
* `azure hdinsight script-action` -oluşturma/betik eylemleri bir kümede yönetmek için komutlar sağlar.
* `azure hdinsight config` -oluşturduğunuz bir yapılandırma dosyası ile birlikte kullanılabilir komutları sağlar `hdinsight cluster create` yapılandırma bilgilerini sağlamak için komutu.

### <a name="deprecated-commands"></a>Kullanımdan kaldırılan komutların
Kullanırsanız `azure hdinsight job` komutları HDInsight kümenize işleri göndermek için şu komutları Resource Manager komutları kullanılabilir değil. Programlama yoluyla işleri HDInsight için betiklerin göndermek istiyorsanız, bunun yerine HDInsight tarafından sağlanan REST API'leri kullanmanız gerekir. REST API'lerini kullanarak işleri gönderme hakkında daha fazla bilgi için aşağıdaki belgelere bakın.

* [MapReduce işleri cURL kullanarak HDInsight üzerinde Hadoop ile çalıştırın.](hadoop/apache-hadoop-use-mapreduce-curl.md)
* [CURL kullanarak HDInsight üzerinde Hadoop ile Hive sorguları çalıştırma](hadoop/apache-hadoop-use-hive-curl.md)
* [Pig işleri cURL kullanarak HDInsight üzerinde Hadoop ile çalıştırın.](hadoop/apache-hadoop-use-pig-curl.md)

MapReduce çalıştırmak için diğer yollar hakkında bilgi için Hive ve etkileşimli olarak Pig, bkz: [HDInsight üzerinde Hadoop ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md), [HDInsight üzerinde Hadoop ile Hive kullanma](hadoop/hdinsight-use-hive.md), ve [üzerinde Hadoop ile Pig kullanma HDInsight](hadoop/hdinsight-use-pig.md).

### <a name="examples"></a>Örnekler
**Küme oluşturma**

* Eski komut (ASM)- `azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Yeni komut- `azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**Küme silme**

* Eski komut (ASM)- `azure hdinsight cluster delete myhdicluster`
* Yeni komut- `azure hdinsight cluster delete mycluster -g myresourcegroup`

**Kümeleri listeleme**

* Eski komut (ASM)- `azure hdinsight cluster list`
* Yeni komut- `azure hdinsight cluster list`

> [!NOTE]
> LIST komutu için kaynak grubunu kullanarak belirtme `-g` kümeleri yalnızca belirtilen kaynak grubunda döndürür.
> 
> 

**Küme bilgilerini göster**

* Eski komut (ASM)- `azure hdinsight cluster show myhdicluster`
* Yeni komut- `azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-to-azure-resource-manager"></a>Azure PowerShell'i Azure Resource Manager'a geçiş
Azure Resource Manager modunda Azure PowerShell ile ilgili genel bilgileri şu adreste bulunabilir: [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

Azure PowerShell Resource Manager cmdlet'lerini ASM cmdlet'leri ile yan yana yüklenebilir. İki modun cmdlet'lerinden adlarına göre ayırt edilebilir.  Resource Manager moduna sahip *AzureRmHDInsight* için karşılaştırma cmdlet adları *AzureHDInsight* ASM modunda.  Örneğin, *New-Azurermhdınsightcluster* vs. *Yeni AzureHDInsightCluster*. Parametreler ve anahtarlar haber adlara sahip ve birçok yeni parametre yok kullanılabilir Kaynak Yöneticisi'ni kullanırken.  Örneğin, çeşitli cmdlet'ler adlı yeni bir anahtar gerekir *- ResourceGroupName*. 

HDInsight cmdlet'lerini kullanabilmeniz için Azure hesabınıza bağlanın ve yeni bir kaynak grubu oluşturun:

* Connect-AzureRmAccount veya [Select-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). Bkz: [Azure Resource Manager ile hizmet sorumlusu kimlik doğrulaması](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Yeniden adlandırılan cmdlet'leri
Windows PowerShell konsolunda HDInsight ASM cmdlet'leri listelemek için:

    help *azurermhdinsight*

ASM cmdlet'leri ve Resource Manager modunda adlarını aşağıdaki tabloda listelenmektedir:

| ASM cmdlet'leri | Resource Manager cmdlet'leri |
| --- | --- |
| Add-AzureHDInsightConfigValues |[Ekle-Azurermhdınsightconfigvalues](https://msdn.microsoft.com/library/mt603530.aspx) |
| Add-AzureHDInsightMetastore |[AzureRmHDInsightMetastore ekleyin](https://msdn.microsoft.com/library/mt603670.aspx) |
| Add-AzureHDInsightScriptAction |[Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) |
| Add-AzureHDInsightStorage |[AzureRmHDInsightStorage ekleyin](https://msdn.microsoft.com/library/mt619445.aspx) |
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx) |
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx) |
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx) |
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Grant-AzureHDInsightHttpServicesAccess |[GRANT-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx) |
| Grant-AzureHdinsightRdpAccess |[GRANT-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx) |
| Invoke-AzureHDInsightHiveJob |[Çağırma AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx) |
| New-AzureHDInsightCluster |[New-AzureRmHDInsightCluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightcluster) |
| New-AzureHDInsightClusterConfig |[New-AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx) |
| New-AzureHDInsightHiveJobDefinition |[Yeni AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx) |
| New-AzureHDInsightMapReduceJobDefinition |[Yeni AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| New-AzureHDInsightPigJobDefinition |[New-AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx) |
| New-AzureHDInsightSqoopJobDefinition |[New-AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[Yeni AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| Remove-AzureHDInsightCluster |[Remove-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx) |
| Revoke-AzureHDInsightHttpServicesAccess |[REVOKE-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx) |
| Revoke-AzureHdinsightRdpAccess |[REVOKE-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx) |
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx) |
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx) |
| Start-AzureHDInsightJob |[Başlangıç AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx) |
| Stop-AzureHDInsightJob |[Stop-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx) |
| Use-AzureHDInsightCluster |[Kullanımı-Azurermhdınsightcluster](https://msdn.microsoft.com/library/mt619442.aspx) |
| Wait-AzureHDInsightJob |[Wait-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a>Yeni cmdlet’ler
Yalnızca Resource Manager modunda kullanılabilen yeni cmdlet'ler şunlardır: 

**Betik eylemi ile ilgili cmdlet'leri:**

* **Get-AzureRmHDInsightPersistedScriptAction**: Gets the persisted script actions for a cluster and lists them in chronological order, or gets details for a specified persisted script action. 
* **Get-AzureRmHDInsightScriptActionHistory**: Gets the script action history for a cluster and lists it in reverse chronological order, or gets details of a previously executed script action. 
* **Remove-AzureRmHDInsightPersistedScriptAction**: Removes a persisted script action from an HDInsight cluster.
* **Set-AzureRmHDInsightPersistedScriptAction**: daha önce yürütülen betik eylemi kalıcı betik eylemi olacak şekilde ayarlar.
* **Gönderme AzureRmHDInsightScriptAction**: Azure HDInsight kümesine yeni betik eylemini gönderir. 

Ek kullanım bilgileri için bkz: [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

**Kimlikle ilgili cmdlet'leri küme:**

* **Ekleme AzureRmHDInsightClusterIdentity**: küme kimlik bir küme yapılandırma nesnesine ekler; böylece Azure Data Lake Stores HDInsight kümesine erişebilirsiniz. Bkz: [Azure PowerShell kullanarak Data Lake Store ile HDInsight kümesi oluşturma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

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

**Küme listesi**

Eski komutu (ASM):

    Get-AzureHDInsightCluster

Yeni komut:

    Get-AzureRmHDInsightCluster 

**Kümenin Göster**

Eski komutu (ASM):

    Get-AzureHDInsightCluster -Name $clusterName

Yeni komut:

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a>Diğer örnekleri
* [HDInsight kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Hive işlerini gönderme](hadoop/apache-hadoop-use-hive-powershell.md)
* [Pig işleri gönderme](hadoop/apache-hadoop-use-pig-powershell.md)
* [Sqoop işlerini gönderme](hadoop/apache-hadoop-use-sqoop-powershell.md)

## <a name="migrating-to-the-new-hdinsight-net-sdk"></a>Yeni HDInsight .NET SDK'sı için geçirme
Azure Hizmet Yönetimi tabanlı [(ASM) HDInsight .NET SDK'sı](https://msdn.microsoft.com/library/azure/mt416619.aspx) kullanım dışı bırakıldı. Azure kaynak yönetimi tabanlı kullanmaları [HDInsight .NET SDK'yı Resource Manager tabanlı](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight). Aşağıdaki ASM tabanlı HDInsight paketler kullanım dışıdır.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Bu bölüm Resource Manager tabanlı SDK'sını kullanarak belirli görevleri gerçekleştirme konusunda daha fazla bilgi için işaretçiler sağlar.

| Nasıl yapılır... Resource Manager tabanlı HDInsight SDK'sını kullanma | Bağlantılar |
| --- | --- |
| .NET SDK kullanarak HDInsight kümeleri oluşturma |Bkz: [oluşturma HDInsight .NET SDK kullanarak kümeleri](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |
| .NET SDK'sı ile betik eylemi kullanarak bir kümeyi özelleştirme |Bkz: [özelleştirme HDInsight Linux kümeleri betik eylemi kullanarak](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Uygulamaları etkileşimli olarak .NET SDK'sı ile Azure Active Directory'yi kullanarak kimlik doğrulaması |Bkz: [.NET SDK kullanarak Hive sorgularını çalıştırma](hadoop/apache-hadoop-use-hive-dotnet-sdk.md). Bu makalede kod parçacığında, etkileşimli bir kimlik doğrulama yaklaşımı kullanılmaktadır. |
| Uygulamaları etkileşimsiz .NET SDK'sı ile Azure Active Directory'yi kullanarak kimlik doğrulaması |Bkz: [HDInsight için etkileşimli olmayan uygulamalar oluşturun](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| .NET SDK'sını kullanarak bir Hive işi gönderdikten |Bkz: [gönderme Hive işleri](hadoop/apache-hadoop-use-hive-dotnet-sdk.md) |
| .NET SDK'sını kullanarak bir Pig işi Gönder |Bkz: [gönderme Pig işleri](hadoop/apache-hadoop-use-pig-dotnet-sdk.md) |
| .NET SDK'sını kullanarak bir Sqoop işi Gönder |Bkz: [Sqoop gönderme işleri](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md) |
| .NET SDK kullanarak HDInsight kümelerini Listele |Bkz: [listesi HDInsight kümeleri](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| .NET SDK kullanarak HDInsight kümeleri ölçeklendirme |Bkz: [ölçek HDInsight kümeleri](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| .NET SDK kullanarak HDInsight kümelerine erişim vermek/iptal etmek |Bkz: [HDInsight kümelerine erişim vermek/iptal etmek](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| .NET SDK kullanarak HDInsight kümelerini HTTP kullanıcı kimlik bilgilerini güncelleştirme |Bkz: [HDInsight kümelerini güncelleştirme HTTP kullanıcı kimlik bilgileri](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| .NET SDK kullanarak HDInsight kümeleri için varsayılan depolama hesabı bulunamadı |Bkz: [HDInsight kümeleri için varsayılan depolama hesabı bulunamadı](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| .NET SDK kullanarak HDInsight kümelerini Sil |Bkz: [Sil HDInsight kümeleri](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Örnekler
Nasıl bir işlem olduğunu ilişkin bazı örnekler aşağıda verilmiştir ASM tabanlı SDK'sı ve eşdeğer kod parçacığı için Resource Manager tabanlı SDK kullanılarak gerçekleştirilir.

**Bir küme CRUD istemcisi oluşturma**

* Eski komut (ASM)
  
        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Yeni komut (hizmet sorumlusu yetkilendirme)
  
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
* Yeni komut (kullanıcı kimlik doğrulaması)
  
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

* Eski komut (ASM)
  
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
* Yeni komut
  
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

* Eski komut (ASM)
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* Yeni komut
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Küme silme**

* Eski komut (ASM)
  
        client.DeleteCluster(dnsName);
* Yeni komut
  
        client.Clusters.Delete(resourceGroup, dnsname);

