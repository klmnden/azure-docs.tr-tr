---
title: Yükleme ve hdınsight'ta - Azure Hadoop kümeleri üzerinde Giraph kullanma | Microsoft Docs
description: Giraph Hdınsight kümesiyle özelleştirmeyi ve Giraph kullanmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 77a1d0e0-55de-4e61-98a0-060914fb7ca0
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: c4cd643d4bdd95493f63bb5b1c1f855bc95bf226
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34271370"
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a>Yükleme ve Windows tabanlı Hdınsight kümelerinde Giraph kullanma

Windows tabanlı Hdınsight kümesi ile betik eylemi kullanarak Giraph özelleştirmeyi ve Giraph büyük ölçekli grafikleri işlemek için nasıl kullanılacağını öğrenin. Linux tabanlı bir kümeyle Giraph kullanma hakkında daha fazla bilgi için bkz: [yükleme Giraph Hdınsight Hadoop kümeleri (Linux) üzerinde](hdinsight-hadoop-giraph-install-linux.md).

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı Hdınsight kümeleri ile çalışır. Hdınsight yalnızca Windows'da Hdınsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Linux tabanlı Hdınsight kümesinde Giraph yükleme hakkında daha fazla bilgi için bkz: [yükleme Giraph Hdınsight Hadoop kümeleri (Linux) üzerinde](hdinsight-hadoop-giraph-install-linux.md).


Kullanarak Azure Hdınsight (Hadoop, Storm, HBase, Spark) kümede herhangi bir türde üzerinde Giraph yükleyebilirsiniz *betik eylemi*. Giraph bir Hdınsight kümesine yüklemek için örnek komut dosyası salt okunur Azure depolama blobunu gelen kullanılabilir [ https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1). Örnek komut dosyası yalnızca Hdınsight küme sürümü ile 3.1 çalışır. Hdınsight küme sürümleri hakkında daha fazla bilgi için bkz: [Hdınsight küme sürümleri](hdinsight-component-versioning.md).

**İlgili makaleler**

* [Giraph Hdınsight Hadoop kümeleri (Linux) yükleyin](hdinsight-hadoop-giraph-install-linux.md)
* [Hdınsight'ta Hadoop kümeleri oluşturma](hdinsight-provision-clusters.md): Hdınsight kümeleri oluşturma hakkında genel bilgi.
* [Betik eylemi kullanarak Hdınsight kümesi özelleştirme][hdinsight-cluster-customize]: betik eylemi kullanarak Hdınsight kümelerini özelleştirme hakkında genel bilgi.
* [Hdınsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Giraph nedir?
<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> Hadoop kullanarak işleme grafik işlemleri yapmanıza olanak tanır ve Azure Hdınsight ile kullanılabilir. Grafikler Internet gibi büyük bir ağdaki yönlendiricileri veya (bazen bir sosyal grafik adlandırılır) sosyal ağlar üzerindeki kişiler arasındaki ilişkileri arasındaki bağlantıları gibi nesneler arasındaki ilişkileri model. Grafik işleme nedeni bir grafik nesneleri arasındaki ilişkiler hakkında gibi sağlar:

* Geçerli ilişkilere dayanan olası arkadaş tanımlama.
* Bir ağda iki bilgisayar arasındaki en kısa yolu tanımlama.
* Web sayfalarındaki sayfa derecesini hesaplama.

## <a name="install-giraph-using-portal"></a>Portal kullanarak Giraph yükleyin
1. Kullanarak bir küme oluşturmaya başlamak **özel Oluştur** konusunda açıklandığı gibi seçeneği [Hdınsight'ta oluşturmak Hadoop kümeleri](hdinsight-provision-clusters.md).
2. Üzerinde **betik eylemleri** Sayfa Sihirbazı'nın tıklatın **betik eylemi eklemek** aşağıda gösterildiği gibi betik eylemi hakkındaki ayrıntıları sağlamak için:

    ![Bir küme özelleştirmek için betik eylemi kullanın](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "bir küme özelleştirmek için kullanım betik eylemi")

    <table border='1'>
        <tr><th>Özellik</th><th>Değer</th></tr>
        <tr><td>Ad</td>
            <td>Betik eylemi için bir ad belirtin. Örneğin, <b>yükleme Giraph</b>.</td></tr>
        <tr><td>Betik URI'si</td>
            <td>Küme özelleştirmek için çağrılan betik Tekdüzen Kaynak Tanımlayıcısı (URI) belirtin. Örneğin, <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Düğüm Türü</td>
            <td>Özelleştirme kodun çalıştığı düğüm belirtin. Seçebileceğiniz <b>tüm düğümleri</b>, <b>Head yalnızca düğümlerin</b>, veya <b>yalnızca çalışan düğümleri</b>.
        <tr><td>Parametreler</td>
            <td>Komut dosyası tarafından gerekli parametreleri belirtin. Bunu boş bırakabilirsiniz şekilde Giraph yüklemek için komut dosyası herhangi bir parametre gerektirmez.</td></tr>
    </table>

    Küme üzerinde birden çok bileşeni yüklemek için birden fazla betik eylemi ekleyebilirsiniz. Komut dosyaları ekledikten sonra küme oluşturmaya başlamak için onay işaretine tıklayın.

## <a name="use-giraph"></a>Giraph kullanma
Temel göstermek için SimpleShortestPathsComputation örnek kullanırız <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> bir grafik nesneler arasındaki en kısa yolu bulmak için uygulama. Örnek verileri ve örnek jar karşıya yükleyin, SimpleShortestPathsComputation örnek kullanarak bir iş çalıştırmak için aşağıdaki adımları kullanın ve sonra sonuçları görüntüleyin.

1. Azure Blob depolama alanına örnek veri dosyasını karşıya yükleyin. Yerel iş istasyonunda, adlı yeni bir dosya oluşturun **tiny_graph.txt**. Aşağıdaki satırları içermelidir:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Hdınsight kümeniz için birincil depolama tiny_graph.txt dosyası yükleyin. Veri yükleme hakkında daha fazla yönerge için bkz: [hdınsight'ta Hadoop işleri için verileri karşıya yükleme](hdinsight-upload-data.md).

    Bu veri biçimi kullanarak bir yönlendirilmiş grafik nesneler arasındaki ilişkiyi tanımlayan [kaynak\_kimliği, kaynak\_değeri [[hedef\_kimliği], [Kenar\_değer],...]]. Her satır arasındaki bir ilişkiyi temsil eder bir **kaynak\_kimliği** nesne ve bir veya daha fazla **taşınmaya\_kimliği** nesneleri. **Kenar\_değeri** (veya ağırlık), gücü veya arasındaki bağlantıyı uzaklığını olarak düşünülebilir **source_id** ve **taşınmaya\_kimliği**.

    Çıkış çizilmiş ve nesneleri arasındaki uzaklığı olarak değeri (veya ağırlık) kullanarak, yukarıdaki verileri şuna benzeyebilir:

    ![Daireler arasında değişen uzaklığı satır olarak çizilen tiny_graph.txt](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. SimpleShortestPathsComputation örneği çalıştırın. Örnek giriş olarak tiny_graph.txt dosyasını kullanarak çalıştırmak için aşağıdaki Azure PowerShell cmdlet'lerini kullanın.

    > [!IMPORTANT]
    > Azure Service Manager kullanılarak HDInsight kaynaklarının yönetilmesi için Azure PowerShell desteği **kullanım dışı bırakılmış** ve 1 Ocak 2017 tarihinde kaldırılmıştır. Bu belgede yer alan adımlar, Azure Resource Manager ile çalışan yeni HDInsight cmdlet'lerini kullanır.
    >
    > Azure PowerShell’in en son sürümünü yüklemek için lütfen [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs)’daki adımları uygulayın. Azure Resource Manager’la çalışan yeni cmdlet’lerle kullanmak için değiştirilmesi gereken komut dosyalarınız varsa, daha fazla bilgi için bkz. [HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme](hdinsight-hadoop-development-using-azure-resource-manager.md).

    ```powershell
    $clusterName = "clustername"
    # Giraph examples jar
    $jarFile = "wasb:///example/jars/giraph-examples.jar"
    # Arguments for this job
    $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                    "-ca", "mapred.job.tracker=headnodehost:9010",
                    "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                    "-vip", "wasb:///example/data/tiny_graph.txt",
                    "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                    "-op",  "wasb:///example/output/shortestpaths",
                    "-w", "2"
    # Create the definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run the job, write output to the Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    Yukarıdaki örnekte **clustername** Giraph yüklü olan Hdınsight kümenizin adıyla.
3. Sonuçlara bakın. İş tamamlandıktan sonra sonuçları iki çıktı dosyalarında depolanır **wasb: / / / örnek/çıkış/shotestpaths** klasör. Dosya adında **bölümü m 00001** ve **bölümü m 00002**. Karşıdan yükle ve çıktı görüntülemek için aşağıdaki adımları gerçekleştirin:

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select the current subscription
    Select-AzureSubscription $subscriptionName

    # Create the Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download the job output to the workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    Bu oluşturacak **çıkış/örnek/shortestpaths** geçerli dizinde iş istasyonu ve bu konuma çıktı dosyaları indirme iki dizin yapısı.

    Kullanım **kat** cmdlet dosyaların içeriğini görüntülemek için:

        Cat example/output/shortestpaths/part*

    Çıktı aşağıdakine benzer görünmelidir:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Örnek başlamak kodlanmış sabit SimpleShortestPathComputation kimliği 1 nesne ve diğer nesnelere en kısa yolu bulunamadı. Çıktı olarak okumanız gereken şekilde `destination_id distance`, burada uzaklığı nesne kimliği 1 ve hedef kimliği arasında seyahat kenarları değeri (veya ağırlık).

    Bu görselleştirme, kısa yolları kimliği 1 ile tüm diğer nesnelerin arasında seyahat sonuçları doğrulayabilirsiniz. Kimliği 1 ve kimliği 4 arasındaki en kısa yolu 5 olduğuna dikkat edin. Arasındaki toplam uzaklığı budur <span style="color:orange">kimliği 1 ve 3</span>ve ardından <span style="color:red">kimliği 3 ve 4</span>.

    ![Nesnelerin arasında çizilmiş kısa yolları daireler olarak çizme](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Giraph işlemleri PowerShell kullanarak yükleme
Bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Örnek, Azure PowerShell kullanarak Spark yükleneceği gösterilmiştir. Kullanılacak komut dosyasını özelleştirmeniz gerekiyorsa [ https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="install-giraph-using-net-sdk"></a>.NET SDK kullanarak Giraph yükleyin
Bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Örnek, .NET SDK kullanarak Spark yükleneceği gösterilmiştir. Kullanılacak komut dosyasını özelleştirmeniz gerekiyorsa [ https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="see-also"></a>Ayrıca bkz.
* [Giraph Hdınsight Hadoop kümeleri (Linux) yükleyin](hdinsight-hadoop-giraph-install-linux.md)
* [Hdınsight'ta Hadoop kümeleri oluşturma](hdinsight-provision-clusters.md): Hdınsight kümeleri oluşturma hakkında genel bilgi.
* [Betik eylemi kullanarak Hdınsight kümesi özelleştirme][hdinsight-cluster-customize]: betik eylemi kullanarak Hdınsight kümelerini özelleştirme hakkında genel bilgi.
* [Hdınsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions.md).
* [Yükleme ve Spark Hdınsight kümelerinde kullanma][hdinsight-install-spark]: Spark yükleme hakkında örnek betik eylemi.
* [Hdınsight kümelerinde Solr yükleme](hdinsight-hadoop-solr-install.md): Solr yükleme hakkında örnek betik eylemi.

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
