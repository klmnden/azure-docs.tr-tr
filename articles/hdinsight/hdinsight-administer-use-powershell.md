---
title: PowerShell - Azure ile HDInsight Apache Hadoop kümelerini yönetme
description: Azure PowerShell kullanarak HDInsight, Apache Hadoop kümeleri için yönetim görevlerini gerçekleştirmeyi öğreneceksiniz.
services: hdinsight
ms.reviewer: tyfox
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: tylerfox
ms.openlocfilehash: 09574647aae8725a614dd20fd0247b0f8cf8b68a
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446978"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Azure PowerShell kullanarak HDInsight Apache Hadoop kümelerini yönetme
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell, denetlemek ve iş yüklerinizi azure'da yönetimini ve dağıtımı otomatik hale getirmek için kullanılabilir. Bu makalede, nasıl yöneteceğinizi öğrenin [Apache Hadoop](https://hadoop.apache.org/) Azure PowerShell kullanarak Azure HDInsight kümeleri. HDInsight PowerShell cmdlet'leri listesi için bkz: [HDInsight cmdlet başvurusu](https://msdn.microsoft.com/library/azure/dn479228.aspx).

**Önkoşullar**

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Azure PowerShell sürüm 0.9 yüklediyseniz x kaldırmanız gerekir, daha yeni sürümü yüklemeden önce.

Yüklü PowerShell sürümü denetlemek için:

```powershell
Get-Module *Az*
```

Eski sürümü kaldırmak için Denetim Masası'ndaki Programlar ve Özellikler'ı çalıştırın.

## <a name="create-clusters"></a>Küme oluşturma
Bkz: [Azure PowerShell kullanarak HDInsight oluşturma Linux tabanlı kümeler](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Kümeleri listeleme
Geçerli Abonelikteki tüm kümelerini listelemek için aşağıdaki komutu kullanın:

```powershell
Get-AzHDInsightCluster
```

## <a name="show-cluster"></a>Kümenin Göster
Geçerli abonelikte belirli bir küme ayrıntılarını görüntülemek için aşağıdaki komutu kullanın:

```powershell
Get-AzHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>Kümeleri Sil
Bir kümeyi silmek için aşağıdaki komutu kullanın:

```powershell
Remove-AzHDInsightCluster -ClusterName <Cluster Name>
```

Kümeyi içeren kaynak grubunu kaldırarak bir küme de silebilirsiniz. Bir kaynak grubunun silinmesi, varsayılan depolama hesabı dahil olmak üzere grubundaki tüm kaynakları siler.

```powershell
Remove-AzResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Özellik ölçeklendirme kümesi Azure HDInsight kümesini yeniden oluşturmak zorunda kalmadan çalışan bir küme tarafından kullanılan çalışan düğümlerinin sayısını değiştirmenize izin verir.

> [!NOTE]  
> Yalnızca, HDInsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümü hakkında şüpheleriniz varsa, Özellikler sayfasını kontrol edebilirsiniz.  Bkz: [kümeleri Listele ve Göster](hdinsight-administer-use-portal-linux.md#showClusters).

HDInsight tarafından desteklenen küme her tür veri düğümü sayısı değiştirmenin etkisi:

* Apache Hadoop

    Sorunsuz bir şekilde, bekleyen veya çalışan tüm işleri etkilemeden çalışan bir Hadoop kümesinde çalışan düğümleri sayısını artırabilirsiniz. İşlem devam ederken yeni işleri da gönderilebilir. Böylece küme her zaman işlevsel bir durumda bırakılır bir ölçeklendirme işlemi hataları düzgün bir şekilde ele alınır.

    Bir Hadoop kümesini veri düğümü sayısını azaltarak ölçeklendiğinde, kümedeki hizmetlerinden bazılarını yeniden başlatılır. Hizmetleri yeniden başlatma bekleyen işleri tüm çalışan ve ölçeklendirme işleminin tamamlanması sırasında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, işleri yeniden oluşturabilirsiniz.
* Apache HBase

    Sorunsuz bir şekilde ekleyebilir veya çalışırken düğümleri HBase kümenize kaldırın. Bölge sunucuları ölçeklendirme işlemi tamamladıktan birkaç dakika içinde otomatik olarak dengelenir. Ancak, el ile küme baş düğümüne oturum açarak bölgesel sunucular dengelemek ve ardından bir komut istemi penceresinden aşağıdaki komutları çalıştırın:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

* Apache Storm

    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümenize veri düğümleri kaldırma. Ancak, ölçeklendirme işlemi başarıyla tamamlandıktan sonra topoloji yeniden dengelemeniz gerekir.

    Yeniden Dengeleme iki şekilde gerçekleştirilebilir:

  * Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı

    Başvurmak [Apache Storm belgeleri](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.

    HDInsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:

    ![HDInsight storm ölçek yeniden Dengeleme](./media/hdinsight-administer-use-powershell/hdinsight.portal.scale.cluster.png)

    Storm topolojiyi yeniden dengelemek için CLI komutunu kullanmak nasıl bir örnek aşağıdadır:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Azure PowerShell kullanarak Hadoop kümenizin boyutunu değiştirmek için bir istemci makineden aşağıdaki komutu çalıştırın:

```powershell
Set-AzHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```


## <a name="grantrevoke-access"></a>GRANT/revoke-access
HDInsight kümeleri aşağıdaki HTTP web Hizmetleri (Bu hizmetlerin tümü, RESTful uç noktalarına sahip) sahip:

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton da

Varsayılan olarak, bu hizmetler için erişim verilir. İptal etme / erişim izni. İptal etmek için:

```powershell
Revoke-AzHDInsightHttpServicesAccess -ClusterName <Cluster Name>
```

Vermek için:

```powershell
$clusterName = "<HDInsight Cluster Name>"

# Credential option 1
$hadoopUserName = "admin"
$hadoopUserPassword = "<Enter the Password>"
$hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

# Credential option 2
#$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"

Grant-AzHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential
```

> [!NOTE]  
> Verme/erişimini iptal ederek, küme kullanıcı adını ve parolasını sıfırlayın.

Ayrıca verme ve erişimi iptal ediliyor portalı üzerinden yapılabilir. Bkz: [yönetme Apache Hadoop, Azure portalını kullanarak HDInsight kümeleri](hdinsight-administer-use-portal-linux.md).

## <a name="update-http-user-credentials"></a>HTTP kullanıcısı kimlik bilgilerini güncelleştirme
Aynı Grant/revoke HTTP erişim yordam var. Kümenin HTTP erişim verilmişse, öncelikle iptal gerekir.  ' İ tıklatın ve ardından yeni HTTP kullanıcı kimlik bilgileriyle erişim verin.

## <a name="find-the-default-storage-account"></a>Varsayılan depolama hesabı bulunamadı
Aşağıdaki PowerShell Betiği, varsayılan depolama hesabı adını ve ilgili bilgi almak gösterilmektedir:

```powershell
#Connect-AzAccount
$clusterName = "<HDInsight Cluster Name>"

$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$storageInfo = $clusterInfo.DefaultStorageAccount.split('.')
$defaultStoreageType = $storageInfo[1]
$defaultStorageName = $storageInfo[0]

echo "Default Storage account name: $defaultStorageName"
echo "Default Storage account type: $defaultStoreageType"

if ($defaultStoreageType -eq "blob")
{
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

    echo "Default Blob container name: $defaultBlobContainerName"
    echo "Default Storage account key: $defaultStorageAccountKey"
}
```


## <a name="find-the-resource-group"></a>Kaynak Grup bulunamıyor
Resource Manager modunda her HDInsight kümesinde bir Azure kaynak grubuna aittir.  Kaynak grubunu bulmak için:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```


## <a name="submit-jobs"></a>İş gönderme
**MapReduce işleri göndermek için**

Bkz: [dahil HDInsight MapReduce örneklerini çalıştırma](hadoop/apache-hadoop-run-samples-linux.md).

**Apache Hive işleri göndermek için**

Bkz: [PowerShell kullanarak Apache Hive sorgularını çalıştırma](hadoop/apache-hadoop-use-hive-powershell.md).

**Apache Pig işleri göndermek için**

Bkz: [PowerShell kullanarak çalıştırma Apache Pig işleri](hadoop/apache-hadoop-use-pig-powershell.md).

**Apache Sqoop işleri göndermek için**

Bkz: [HDInsight ile Apache Sqoop'u kullanma](hadoop/hdinsight-use-sqoop.md).

**Apache Oozie iş göndermek için**

Bkz: [tanımlamak ve iş akışı çalıştırma HDInsight için Apache Hadoop ile Apache Oozie kullanma](hdinsight-use-oozie-linux-mac.md).

## <a name="upload-data-to-azure-blob-storage"></a>Azure Blob depolama alanına veri yükleme
Bkz. [HDInsight'a veri yükleme][hdinsight-upload-data].

## <a name="see-also"></a>Ayrıca Bkz.
* [HDInsight cmdlet başvurusu belgeleri](https://msdn.microsoft.com/library/azure/dn479228.aspx)
* [Azure portalını kullanarak HDInsight Apache Hadoop kümelerini yönetme](hdinsight-administer-use-portal-linux.md)
* [Bir komut satırı arabirimi ile HDInsight'ı yönetme][hdinsight-admin-cli]
* [HDInsight kümeleri oluşturma][hdinsight-provision]
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [Program aracılığıyla Apache Hadoop işlerini gönderme][hdinsight-submit-jobs]
* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
