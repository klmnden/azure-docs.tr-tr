---
title: PowerShell - Azure ile HDInsight Hadoop kümelerini yönetme
description: Azure PowerShell kullanarak HDInsight Hadoop kümeleri için yönetim görevlerini gerçekleştirmeyi öğreneceksiniz.
services: hdinsight
editor: jasonwhowell
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: b23662ccda090b4d8a4cbb5eae3029affd075c33
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42059752"
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Azure PowerShell kullanarak HDInsight Hadoop kümelerini yönetme
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell, denetlemek ve iş yüklerinizi azure'da yönetimini ve dağıtımı otomatik hale getirmek için kullanılabilir. Bu makalede, Azure PowerShell kullanarak Azure HDInsight Hadoop kümelerini yönetme konusunda bilgi edinin. HDInsight PowerShell cmdlet'leri listesi için bkz: [HDInsight cmdlet başvurusu](https://msdn.microsoft.com/library/azure/dn479228.aspx).

**Önkoşullar**

Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Azure PowerShell sürüm 0.9 yüklediyseniz x kaldırmanız gerekir, daha yeni sürümü yüklemeden önce.

Yüklü PowerShell sürümü denetlemek için:

```powershell
Get-Module *azure*
```

Eski sürümü kaldırmak için Denetim Masası'ndaki Programlar ve Özellikler'ı çalıştırın.

## <a name="create-clusters"></a>Küme oluşturma
Bkz: [Azure PowerShell kullanarak HDInsight oluşturma Linux tabanlı kümeler](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Kümeleri listeleme
Geçerli Abonelikteki tüm kümelerini listelemek için aşağıdaki komutu kullanın:

```powershell
Get-AzureRmHDInsightCluster
```

## <a name="show-cluster"></a>Kümenin Göster
Geçerli abonelikte belirli bir küme ayrıntılarını görüntülemek için aşağıdaki komutu kullanın:

```powershell
Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>Kümeleri Sil
Bir kümeyi silmek için aşağıdaki komutu kullanın:

```powershell
Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

Kümeyi içeren kaynak grubunu kaldırarak bir küme de silebilirsiniz. Bir kaynak grubunun silinmesi, varsayılan depolama hesabı dahil olmak üzere grubundaki tüm kaynakları siler.

```powershell
Remove-AzureRmResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Özellik ölçeklendirme kümesi Azure HDInsight kümesini yeniden oluşturmak zorunda kalmadan çalışan bir küme tarafından kullanılan çalışan düğümlerinin sayısını değiştirmenize izin verir.

> [!NOTE]
> Yalnızca, HDInsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümü hakkında şüpheleriniz varsa, Özellikler sayfasını kontrol edebilirsiniz.  Bkz: [kümeleri Listele ve Göster](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
>
>

HDInsight tarafından desteklenen küme her tür veri düğümü sayısı değiştirmenin etkisi:

* Hadoop

    Sorunsuz bir şekilde, bekleyen veya çalışan tüm işleri etkilemeden çalışan bir Hadoop kümesinde çalışan düğümleri sayısını artırabilirsiniz. İşlem devam ederken yeni işleri da gönderilebilir. Böylece küme her zaman işlevsel bir durumda bırakılır bir ölçeklendirme işlemi hataları düzgün bir şekilde ele alınır.

    Bir Hadoop kümesini veri düğümü sayısını azaltarak ölçeklendiğinde, kümedeki hizmetlerinden bazılarını yeniden başlatılır. Hizmetleri yeniden başlatma bekleyen işleri tüm çalışan ve ölçeklendirme işleminin tamamlanması sırasında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, işleri yeniden oluşturabilirsiniz.
* HBase

    Sorunsuz bir şekilde ekleyebilir veya çalışırken düğümleri HBase kümenize kaldırın. Bölge sunucuları ölçeklendirme işlemi tamamladıktan birkaç dakika içinde otomatik olarak dengelenir. Ancak, el ile küme baş düğümüne oturum açarak bölgesel sunucular dengelemek ve ardından bir komut istemi penceresinden aşağıdaki komutları çalıştırın:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

* Storm

    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümenize veri düğümleri kaldırma. Ancak, ölçeklendirme işlemi başarıyla tamamlandıktan sonra topoloji yeniden dengelemeniz gerekir.

    Yeniden Dengeleme iki şekilde gerçekleştirilebilir:

  * Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı

    Başvurmak [Apache Storm belgeleri](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.

    HDInsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:

    ![HDInsight storm ölçek yeniden Dengeleme](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    Storm topolojiyi yeniden dengelemek için CLI komutunu kullanmak nasıl bir örnek aşağıdadır:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Azure PowerShell kullanarak Hadoop kümenizin boyutunu değiştirmek için bir istemci makineden aşağıdaki komutu çalıştırın:

```powershell
Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
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
Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>
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

Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential
```

> [!NOTE]
> Verme/erişimini iptal ederek, küme kullanıcı adını ve parolasını sıfırlayın.
>
>

Ayrıca verme ve erişimi iptal ediliyor portalı üzerinden yapılabilir. Bkz: [Azure portalını kullanarak HDInsight yönetmek][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>HTTP kullanıcısı kimlik bilgilerini güncelleştirme
Yordamın aynısını olan [vermek/iptal etmek HTTP erişim](#grant/revoke-access). Kümenin HTTP erişim verilmişse, öncelikle iptal gerekir.  ' İ tıklatın ve ardından yeni HTTP kullanıcı kimlik bilgileriyle erişim verin.

## <a name="find-the-default-storage-account"></a>Varsayılan depolama hesabı bulunamadı
Aşağıdaki PowerShell Betiği, varsayılan depolama hesabı adını ve ilgili bilgi almak gösterilmektedir:

```powershell
#Connect-AzureRmAccount
$clusterName = "<HDInsight Cluster Name>"

$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$storageInfo = $clusterInfo.DefaultStorageAccount.split('.')
$defaultStoreageType = $storageInfo[1]
$defaultStorageName = $storageInfo[0]

echo "Default Storage account name: $defaultStorageName"
echo "Default Storage account type: $defaultStoreageType"

if ($defaultStoreageType -eq "blob")
{
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

    echo "Default Blob container name: $defaultBlobContainerName"
    echo "Default Storage account key: $defaultStorageAccountKey"
}
```


## <a name="find-the-resource-group"></a>Kaynak Grup bulunamıyor
Resource Manager modunda her HDInsight kümesinde bir Azure kaynak grubuna aittir.  Kaynak grubunu bulmak için:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```


## <a name="submit-jobs"></a>İş gönderme
**MapReduce işleri göndermek için**

Bkz: [Windows tabanlı HDInsight Hadoop MapReduce çalıştırma örnekleri](hdinsight-run-samples.md).

**Hive işlerini göndermek için**

Bkz: [PowerShell kullanarak Hive sorgularını çalıştırma](hadoop/apache-hadoop-use-hive-powershell.md).

**Pig işleri göndermek için**

Bkz: [PowerShell kullanarak çalıştırma Pig işleri](hadoop/apache-hadoop-use-pig-powershell.md).

**Sqoop işleri göndermek için**

Bkz: [HDInsight ile Sqoop kullanma](hadoop/hdinsight-use-sqoop.md).

**Oozie işleri göndermek için**

Bkz: [tanımlamak ve HDInsight içinde bir iş akışı çalıştırmak için Hadoop ile Oozie kullanma](hdinsight-use-oozie.md).

## <a name="upload-data-to-azure-blob-storage"></a>Azure Blob depolama alanına veri yükleme
Bkz. [HDInsight'a veri yükleme][hdinsight-upload-data].

## <a name="see-also"></a>Ayrıca Bkz.
* [HDInsight cmdlet başvurusu belgeleri](https://msdn.microsoft.com/library/azure/dn479228.aspx)
* [HDInsight Azure portalını kullanarak yönetme][hdinsight-admin-portal]
* [Bir komut satırı arabirimi ile HDInsight'ı yönetme][hdinsight-admin-cli]
* [HDInsight kümeleri oluşturma][hdinsight-provision]
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [Program aracılığıyla Hadoop işlerini gönderme][hdinsight-submit-jobs]
* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
