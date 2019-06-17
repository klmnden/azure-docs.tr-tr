---
title: Bootstrap ile Azure HDInsight küme yapılandırmalarını özelleştirme
description: HDInsight küme yapılandırması, .net, PowerShell ve Resource Manager kullanarak program aracılığıyla özelleştirmeyi öğrenin şablonları.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/19/2019
ms.openlocfilehash: 7f9100686eaab8c4c75e3d862026b18b6c46ed09
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65203716"
---
# <a name="customize-hdinsight-clusters-using-bootstrap"></a>Bootstrap ile HDInsight kümeleri özelleştirme

Önyükleme betikleri bileşenlerini yükleme ve Azure HDInsight içinde program aracılığıyla yapılandırma sağlar. 

HDInsight kümenizi oluşturulurken yapılandırma dosyası ayarlarının ayarlamak için üç yaklaşım vardır:

* Azure PowerShell kullanma
* .NET SDK kullanma
* Azure Resource Manager şablonu kullanma

Örneğin, program bu yöntemleri kullanarak bu dosyaları seçeneklerini yapılandırabilirsiniz:

* clusterIdentity.xml
* Core-site.xml
* Gateway.XML
* hbase env.xml
* hbase-site.xml
* hdfs-site.xml
* Hive env.xml
* Hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml
* Server.Properties (kafka Aracısını Yapılandırma)

Ek bileşenler oluşturma süre boyunca HDInsight kümesinde yükleme hakkında daha fazla bilgi için bkz: [özelleştirme HDInsight kümelerini betik eylemi (Linux) kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="prerequisites"></a>Önkoşullar

* PowerShell kullanarak, ihtiyacınız olacak [Az modül](https://docs.microsoft.com/powershell/azure/overview).

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Aşağıdaki PowerShell kod özelleştirir bir [Apache Hive](https://hive.apache.org/) yapılandırma:

> [!IMPORTANT]  
> Parametre `Spark2Defaults` ile birlikte gerekebilir [Ekle AzHDInsightConfigValue](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightconfigvalue). Aşağıdaki kod örneğinde gösterildiği gibi parametresine boş değer geçirebilirsiniz.


```powershell
# hive-site.xml configuration
$hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValue `
        -HiveSite $hiveConfigValues `
        -Spark2Defaults @{}

New-AzHDInsightCluster `
    -ResourceGroupName $existingResourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -Config $config
```

Tam çalışan bir PowerShell Betiği bulunabilir [ek](#appendix-powershell-sample).

**Değişikliği doğrulamak için:**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Sol menüden **HDInsight kümeleri**. Görmüyorsanız, tıklayın **tüm hizmetleri** ilk.
3. PowerShell betiğini kullanarak yeni oluşturduğunuz kümeye tıklayın.
4. Tıklayın **Pano** Ambari UI'ı açmak için dikey pencerenin üst.
5. Tıklayın **Hive** sol menüden.
6. Tıklayın **HiveServer2** gelen **özeti**.
7. Tıklayın **yapılandırmaları** sekmesi.
8. Tıklayın **Hive** sol menüden.
9. Tıklayın **Gelişmiş** sekmesi.
10. Aşağı kaydırın ve ardından **hive site Gelişmiş**.
11. Aranacak **hive.metastore.client.socket.timeout** bölümünde.

Diğer yapılandırma dosyalarını özelleştirme hakkında daha fazla bazı örnekleri:

```xml
# hdfs-site.xml configuration
$HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

# core-site.xml configuration
$CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

# mapred-site.xml configuration
$MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

# oozie-site.xml configuration
$OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120
```

## <a name="use-net-sdk"></a>.NET SDK kullanma
Bkz: [.NET SDK kullanarak HDInsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).

## <a name="use-resource-manager-template"></a>Kullanım Resource Manager şablonu
Resource Manager şablonunda önyükleme kullanabilirsiniz:

```json
"configurations": {
    "hive-site": {
        "hive.metastore.client.connect.retry.delay": "5",
        "hive.execution.engine": "mr",
        "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
    }
}
```

![HDInsight Hadoop kümesi önyükleme Azure Resource Manager şablonu özelleştirir](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)

## <a name="see-also"></a>Ayrıca bkz.
* [HDInsight Apache Hadoop kümeleri oluşturma] [ hdinsight-provision-cluster] diğer özel seçenekleri kullanarak bir HDInsight kümesi oluşturma hakkında yönergeler açıklanmaktadır.
* [HDInsight için betik eylemi betikleri geliştirme][hdinsight-write-script]
* [Yükleme ve Apache Spark HDInsight kümeleri kullanma][hdinsight-install-spark]
* [Yükleme ve HDInsight kümeleri üzerinde Apache giraph'ı kullanma](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions-linux.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Küme oluşturma sırasında aşamaları"

## <a name="appendix-powershell-sample"></a>Ek: PowerShell örneği

Bu PowerShell Betiği bir HDInsight kümesi oluşturur ve Hive ayarı özelleştirir. İçin değerler girdiğinizden emin olun `$nameToken`, `$httpPassword`, ve `$sshPassword`.

> [!IMPORTANT]  
> Değerleri `DefaultStorageAccount`, ve `DefaultStorageContainer` döndürülen değil [Get-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster) olduğunda [güvenli aktarım](../storage/common/storage-require-secure-transfer.md) depolama hesabı etkinleştirilir.

> [!WARNING]  
> Depolama hesabı türü `BlobStorage` HDInsight kümeleri için kullanılamaz.


```powershell
####################################
# Set these variables
####################################
#region - used for creating Azure service names
$nameToken = "<ENTER AN ALIAS>" 
#endregion

#region - cluster user accounts
$httpUserName = "admin"  #HDInsight cluster username
$httpPassword = '<ENTER A PASSWORD>' 

$sshUserName = "sshuser" #HDInsight ssh user name
$sshPassword = '<ENTER A PASSWORD>' 
#endregion

####################################
# Service names and varialbes
####################################
#region - service names
$namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

$resourceGroupName = $namePrefix + "rg"
$hdinsightClusterName = $namePrefix + "hdi"
$defaultStorageAccountName = $namePrefix + "store"
$defaultBlobContainerName = $hdinsightClusterName

$location = "East US"
#endregion


####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

#endregion

#region - Create an HDInsight cluster
####################################
# Create dependent components
####################################
Write-Host "Creating a resource group ..." -ForegroundColor Green
New-AzResourceGroup `
    -Name  $resourceGroupName `
    -Location $location

Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
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

New-AzStorageContainer `
    -Name $defaultBlobContainerName `
    -Context $defaultStorageContext #use the cluster name as the container name

####################################
# Create a configuration object
####################################
$hiveConfigValues = @{"hive.metastore.client.socket.timeout"="90"}

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValue `
        -HiveSite $hiveConfigValues `
        -Spark2Defaults @{}

####################################
# Create an HDInsight cluster
####################################
$httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

$sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
$sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

New-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -Location $location `
    -ClusterSizeInNodes 1 `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -Config $config

####################################
# Verify the cluster
####################################
Get-AzHDInsightCluster `
    -ClusterName $hdinsightClusterName

#endregion
```
