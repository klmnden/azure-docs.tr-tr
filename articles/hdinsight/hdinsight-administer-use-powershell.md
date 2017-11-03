---
title: "PowerShell - Azure ile hdınsight'ta Hadoop kümelerini yönetme | Microsoft Docs"
description: "Azure PowerShell kullanarak hdınsight'ta Hadoop kümeleri için yönetim görevlerini gerçekleştirmek öğrenin."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: bfdfa754-18e5-4ef9-b0d6-2dbdcebc0283
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: jgao
ms.openlocfilehash: 3522cae228e92b47023cfca217e09c2e2104190b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Azure PowerShell kullanarak hdınsight'ta Hadoop kümelerini yönetme
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell denetlemek ve dağıtımı ve Yönetimi azure'da iş yüklerinizin otomatik hale getirmek için kullanılabilir. Bu makalede, Azure PowerShell kullanarak Azure hdınsight'ta Hadoop kümelerini yönetme öğrenin. Hdınsight PowerShell cmdlet'leri listesi için bkz: [Hdınsight cmdlet başvurusu][hdinsight-powershell-reference].

**Önkoşullar**

Bu makaleye başlamadan önce aşağıdaki öğeleri sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Azure PowerShell sürüm 0.9 yüklediyseniz x, bunu yeni bir sürümünü yüklemeden önce kaldırmalısınız.

Yüklü PowerShell sürümü denetlemek için:

```powershell
Get-Module *azure*
```

Eski sürümünü kaldırmak için Denetim Masası'ndaki Programlar ve Özellikler'ı çalıştırın.

## <a name="create-clusters"></a>Küme oluşturma
Bkz: [Azure PowerShell kullanarak Hdınsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Liste kümeleri
Tüm kümelerde geçerli abonelik listelemek için aşağıdaki komutu kullanın:

```powershell
Get-AzureRmHDInsightCluster
```

## <a name="show-cluster"></a>Küme Göster
Belirli bir kümeyi ayrıntılarını geçerli abonelikte göstermek için aşağıdaki komutu kullanın:

```powershell
Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>Küme silme
Bir küme silmek için aşağıdaki komutu kullanın:

```powershell
Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

Ayrıca, küme içeren kaynak grubunu kaldırarak bir küme silebilirsiniz. Bir kaynak grubunu silme varsayılan depolama hesabı dahil olmak üzere gruptaki tüm kaynakları siler.

```powershell
Remove-AzureRmResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme
Özellik ölçeklendirme küme kümeye yeniden oluşturmak zorunda kalmadan Azure Hdınsight'ta çalıştıran bir küme tarafından kullanılan çalışan düğümü sayısını değiştirmenize izin verir.

> [!NOTE]
> Yalnızca, Hdınsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümünü emin değilseniz, Özellikler sayfasını kontrol edebilirsiniz.  Bkz: [listesi ve Göster kümeleri](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
>
>

Her tür Hdınsight tarafından desteklenen küme için veri düğüm sayısını değiştirme etkisi:

* Hadoop

    Sorunsuz bir şekilde tüm bekleyen veya çalışan işler etkilemeden çalıştıran bir Hadoop kümesinde çalışan düğümü sayısını da artırabilirsiniz. İşlemi devam ederken yeni işleri da gönderilebilir. Kümenin her zaman işlevsel bir durumda bırakılır böylece bir ölçeklendirme işlemi hatalar düzgün bir şekilde ele alınır.

    Bir Hadoop kümesine veri düğüm sayısını azaltarak ölçeklendirilir, bazı kümedeki hizmetleri yeniden başlatılır. Hizmetleri yeniden tüm çalışan ve bekleyen işleri ölçekleme işlemi tamamlandığında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, sunmaları olabilir.
* HBase

    Sorunsuz bir şekilde ekleyebilir veya çalışırken, HBase kümesi düğümleri kaldırın. Bölgesel sunucular otomatik olarak ölçeklendirme işlemi tamamladıktan birkaç dakika içinde dengeli. Ancak, el ile de küme headnode için oturum açarak bölgesel sunucular dengelemek ve ardından bir komut istemi penceresinden aşağıdaki komutları çalıştırın:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

* Storm

    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümeniz veri düğümleri kaldırın. Ancak ölçeklendirme işlemi başarıyla tamamlandıktan sonra topoloji yeniden dengelemeniz gerekir.

    İki yolla yeniden dengelenmesi gerçekleştirilebilir:

  * Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı

    Başvurmak [Apache Storm belgelerine](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.

    Hdınsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:

    ![Hdınsight storm ölçek yeniden dengeleyin](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    Storm topolojisini yeniden dengelemeniz CLI komutunu kullanma örneği şöyledir:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Azure PowerShell kullanarak Hadoop küme boyutunu değiştirmek için bir istemci makinesinden aşağıdaki komutu çalıştırın:

```powershell
Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```


## <a name="grantrevoke-access"></a>GRANT/revoke erişim
Hdınsight kümeleri (Bu hizmetlerin tümü için RESTful uç noktaları vardır) aşağıdaki HTTP web hizmetleri vardır:

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Varsayılan olarak, bu hizmetleri için erişim verilir. İptal etme / erişim izninin. İptal etmek için:

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
> Verme/erişimi iptal ederek, küme kullanıcı adını ve parolasını sıfırlayın.
>
>

Ayrıca verme ve erişimi iptal ediliyor portalı yoluyla yapılabilir. Bkz: [yönetmek Azure portalını kullanarak Hdınsight][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>HTTP kullanıcı kimlik bilgilerini güncelleştirin
Yordamın aynısını olan [Grant/revoke HTTP erişimi](#grant/revoke-access). Küme HTTP erişim verilmişse, öncelikle iptal gerekir.  Ve ardından yeni HTTP kullanıcı kimlik bilgileriyle erişim verin.

## <a name="find-the-default-storage-account"></a>Varsayılan depolama hesabı bulunamadı
Aşağıdaki PowerShell betiğini varsayılan depolama hesabı adı ve ilgili bilgi almak nasıl gösterir:

```powershell
#Login-AzureRmAccount
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
Kaynak Yöneticisi modunda her Hdınsight kümesi bir Azure kaynak grubuna ait.  Kaynak grubu bulmak için:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```


## <a name="submit-jobs"></a>İşlerini gönderme
**MapReduce işleri göndermek için**

Bkz: [Windows tabanlı Hdınsight Hadoop MapReduce çalıştırma örnekleri](hdinsight-run-samples.md).

**Hive işlerini göndermek için**

Bkz: [PowerShell kullanarak Hive sorgularını çalıştırma](hdinsight-hadoop-use-hive-powershell.md).

**Pig işleri göndermek için**

Bkz: [çalıştırmak Pig işleri PowerShell kullanarak](hdinsight-hadoop-use-pig-powershell.md).

**Sqoop işlerini göndermek için**

Bkz: [Hdınsight ile Sqoop kullanma](hdinsight-use-sqoop.md).

**Oozie işlerini göndermek için**

Bkz: [tanımlamak ve Hdınsight'ta bir iş akışını çalıştırmak için Hadoop ile kullanım Oozie](hdinsight-use-oozie.md).

## <a name="upload-data-to-azure-blob-storage"></a>Azure Blob depolama alanına veri yükleme
Bkz. [HDInsight'a veri yükleme][hdinsight-upload-data].

## <a name="see-also"></a>Ayrıca Bkz.
* [Hdınsight cmdlet başvuru belgeleri][hdinsight-powershell-reference]
* [Hdınsight Azure Portalı'nı kullanarak yönetme][hdinsight-admin-portal]
* [Bir komut satırı arabirimi kullanarak Hdınsight yönetme][hdinsight-admin-cli]
* [Hdınsight kümeleri oluşturma][hdinsight-provision]
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [Hadoop işlerini programlı olarak gönderme][hdinsight-submit-jobs]
* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
