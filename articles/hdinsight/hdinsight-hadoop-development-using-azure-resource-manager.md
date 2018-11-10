---
title: HDInsight için Azure Resource Manager araçlarına geçiş
description: HDInsight kümeleri için Azure Resource Manager geliştirme araçlarına geçiş yapma
services: hdinsight
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: hrasheed
ms.openlocfilehash: f4a1ba29e569d4605c3aa6f2fb6c238c8ba22434
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51006286"
---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçiş

HDInsight, HDInsight için Azure Service Manager ASM tabanlı araçlar kaldırmaktadır. Azure PowerShell, Azure Klasik CLI veya HDInsight .NET SDK'ın HDInsight kümeleriyle çalışmak için kullandığınız, PowerShell, CLI ve .NET SDK'sı ileride Azure Resource Manager sürümleri kullanmanız önerilir. Bu makalede, Resource Manager tabanlı yeni yaklaşıma için geçiş yapmaya yönelik işaretçiler sağlar. Geçerli olduğunda, bu belgede ASM ve Resource Manager yaklaşım HDInsight için arasındaki farklar vurgulanmaktadır.

> [!IMPORTANT]
> ASM desteği, PowerShell, CLI, temel alır ve .NET SDK'sı üzerinde sona erecek **1 Ocak 2017**.
> 
> 

## <a name="migrating-azure-classic-cli-to-azure-resource-manager"></a>Klasik Azure CLI, Azure Resource Manager'a geçiş

> [!IMPORTANT]
> Azure CLI, HDInsight kümeleriyle çalışmak için destek sağlamaz. Ancak, Klasik Azure CLI kullanım dışı bırakılmıştır, Klasik Azure CLI'yı HDInsight ile kullanabilirsiniz.

Klasik Azure CLI aracılığıyla HDInsight ile çalışmak için temel komutlar şunlardır:

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

* Connect-AzureRmAccount veya [Select-AzureRmProfile](https://docs.microsoft.com/powershell/module/servicemanagement/azurerm.profile/select-azurermprofile?view=azuresmps-4.0.0). Bkz: [Azure Resource Manager ile hizmet sorumlusu kimlik doğrulaması](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Yeniden adlandırılan cmdlet'leri
Windows PowerShell konsolunda HDInsight ASM cmdlet'leri listelemek için:

    help *azurermhdinsight*

ASM cmdlet'leri ve Resource Manager modunda adlarını aşağıdaki tabloda listelenmektedir:

| ASM cmdlet'leri | Resource Manager cmdlet'leri |
| --- | --- |
| Add-AzureHDInsightConfigValues |[Ekle-Azurermhdınsightconfigvalues](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightconfigvalues?view=azurermps-6.6.0) |
| Add-AzureHDInsightMetastore |[AzureRmHDInsightMetastore ekleyin](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightmetastore?view=azurermps-6.6.0) |
| Add-AzureHDInsightScriptAction |[Add-AzureRmHDInsightScriptAction](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightscriptaction?view=azurermps-6.6.0) |
| Add-AzureHDInsightStorage |[AzureRmHDInsightStorage ekleyin](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightstorage?view=azurermps-6.6.0) |
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/get-azurermhdinsightcluster?view=azurermps-6.6.0) |
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/get-azurermhdinsightjob?view=azurermps-6.6.0) |
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/get-azurermhdinsightjoboutput?view=azurermps-6.6.0) |
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/get-azurermhdinsightproperties?view=azurermps-6.6.0) |
| Grant-AzureHDInsightHttpServicesAccess |[GRANT-AzureRmHDInsightHttpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/grant-azurermhdinsighthttpservicesaccess?view=azurermps-6.6.0) |
| Grant-AzureHdinsightRdpAccess |[GRANT-AzureRmHDInsightRdpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/grant-azurermhdinsightrdpservicesaccess?view=azurermps-6.6.0) |
| Invoke-AzureHDInsightHiveJob |[Çağırma AzureRmHDInsightHiveJob](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/invoke-azurermhdinsighthivejob?view=azurermps-6.6.0) |
| New-AzureHDInsightCluster |[New-AzureRmHDInsightCluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightcluster) |
| New-AzureHDInsightClusterConfig |[New-AzureRmHDInsightClusterConfig](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightclusterconfig?view=azurermps-6.6.0) |
| New-AzureHDInsightHiveJobDefinition |[Yeni AzureRmHDInsightHiveJobDefinition](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsighthivejobdefinition?view=azurermps-6.6.0) |
| New-AzureHDInsightMapReduceJobDefinition |[Yeni AzureRmHDInsightMapReduceJobDefinition](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightmapreducejobdefinition?view=azurermps-6.6.0) |
| New-AzureHDInsightPigJobDefinition |[New-AzureRmHDInsightPigJobDefinition](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightpigjobdefinition?view=azurermps-6.6.0) |
| New-AzureHDInsightSqoopJobDefinition |[New-AzureRmHDInsightSqoopJobDefinition](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightsqoopjobdefinition?view=azurermps-6.6.0) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[Yeni AzureRmHDInsightStreamingMapReduceJobDefinition](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightstreamingmapreducejobdefinition?view=azurermps-6.6.0) |
| Remove-AzureHDInsightCluster |[Remove-AzureRmHDInsightCluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/remove-azurermhdinsightcluster?view=azurermps-6.6.0) |
| Revoke-AzureHDInsightHttpServicesAccess |[REVOKE-AzureRmHDInsightHttpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/revoke-azurermhdinsighthttpservicesaccess?view=azurermps-6.6.0) |
| Revoke-AzureHdinsightRdpAccess |[REVOKE-AzureRmHDInsightRdpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/revoke-azurermhdinsightrdpservicesaccess?view=azurermps-6.6.0) |
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize?view=azurermps-6.6.0) |
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightdefaultstorage?view=azurermps-6.6.0) |
| Start-AzureHDInsightJob |[Başlangıç AzureRmHDInsightJob](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/start-azurermhdinsightjob?view=azurermps-6.6.0) |
| Stop-AzureHDInsightJob |[Stop-AzureRmHDInsightJob](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/stop-azurermhdinsightjob?view=azurermps-6.6.0) |
| Use-AzureHDInsightCluster |[Kullanımı-Azurermhdınsightcluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/use-azurermhdinsightcluster?view=azurermps-6.6.0) |
| Wait-AzureHDInsightJob |[Wait-AzureRmHDInsightJob](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/wait-azurermhdinsightjob?view=azurermps-6.6.0) |

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

