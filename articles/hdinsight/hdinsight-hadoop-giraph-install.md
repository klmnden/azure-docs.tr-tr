---
title: Yükleme ve HDInsight - Azure Hadoop kümelerinde Giraph kullanma
description: Giraph ile HDInsight kümesi özelleştirme ve Giraph'ı kullanmayı öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2016
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 96a334b4bd39513bfad128a8f1b59f319fef013e
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58317417"
---
# <a name="install-and-use-apache-giraph-on-windows-based-hdinsight-clusters"></a>Yükleme ve Windows tabanlı HDInsight kümeleri üzerinde Apache giraph'ı kullanma

Windows tabanlı HDInsight kümesi Apache Giraph ile betik eylemi kullanarak özelleştirme ve büyük ölçekli grafikleri işlemek için giraph'ı kullanmayı öğrenin. Linux tabanlı küme ile Giraph'ı kullanma hakkında daha fazla bilgi için bkz. [(Linux) HDInsight Hadoop kümeleri üzerinde Apache Giraph'ı yükleme](hdinsight-hadoop-giraph-install-linux.md).

> [!IMPORTANT]  
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı HDInsight kümeleri ile çalışır. HDInsight yalnızca Windows üzerinde HDInsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Bir Linux tabanlı HDInsight kümesi üzerinde Giraph yükleme hakkında daha fazla bilgi için bkz: [(Linux) HDInsight Hadoop kümeleri üzerinde Apache Giraph'ı yükleme](hdinsight-hadoop-giraph-install-linux.md).


Kullanarak Azure HDInsight (Hadoop, Storm, HBase, Spark) kümesinde herhangi bir türde üzerinde Giraph yükleyebilirsiniz *betik eylemi*. Bir HDInsight kümesi üzerinde Giraph'ı yüklemek için örnek betik salt okunur bir Azure depolama blob'nden kullanılabilir [ https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1). Örnek betik yalnızca HDInsight kümesi sürüm ile 3.1 çalışır. HDInsight küme sürümleri hakkında daha fazla bilgi için bkz. [HDInsight küme sürümleri](hdinsight-component-versioning.md).

**İlgili makaleler**

* [(Linux) HDInsight Hadoop kümeleri üzerinde Apache giraph'ı yükleme](hdinsight-hadoop-giraph-install-linux.md)
* [HDInsight Apache Hadoop kümeleri oluşturma](hdinsight-provision-clusters.md): HDInsight kümeleri oluşturma hakkında genel bilgi.
* [Betik eylemi kullanarak HDInsight kümesi özelleştirme] [hdınsight-kümesi-özelleştirme]: HDInsight kümelerini betik eylemi kullanarak özelleştirme hakkında genel bilgiler.
* [HDInsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Giraph nedir?
<a href="https://giraph.apache.org/" target="_blank">Apache giraph'ı</a> grafik Hadoop kullanarak işleme yapmanıza olanak tanır ve Azure HDInsight ile kullanılabilir. Graflar sosyal ağlardaki (bazen bir sosyal grafik olarak adlandırılır) kişiler arasındaki ilişkileri veya Internet gibi büyük bir ağda bulunan yönlendiriciler arasındaki bağlantıları gibi nesneler arasındaki ilişkileri modellemek. Grafik işleme nedeni hakkında bir grafta nesneleri arasındaki ilişkileri gibi sağlar:

* Geçerli ilişkilerinizi tabanlı olası arkadaş tanımlayıcı.
* Bir ağda iki bilgisayar arasındaki en kısa yolu tanımlama.
* Web sayfalarının sayfası boyut hesaplanıyor.

## <a name="install-giraph-using-portal"></a>Portalı kullanarak Giraph yükleme
1. Kullanarak bir küme oluşturmaya başlayın **özel Oluştur** anlatıldığı gibi seçeneği [Hadoop kümeleri oluşturma HDInsight](hdinsight-provision-clusters.md).
2. Üzerinde **betik eylemleri** Sayfası Sihirbazı'nın **betik eylemi ekleme** aşağıda gösterildiği gibi bir betik eylemi ayrıntılarını sağlamak için:

    ![Bir küme özelleştirmek için betik eylemi kullanmanız](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "küme özelleştirmek için betik eylemini kullanın")

    |Özellik|Değer|  
    |---|---|  
    |Ad|Betik eylemi için bir ad belirtin. Örneğin, **Giraph yükleme**|
    |Betiği URI'si|Küme özelleştirmek için çağrılır betik Tekdüzen Kaynak Tanımlayıcısı (URI) belirtin. Örneğin, *https:\//hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1*|
    |Düğüm Türü|Özelleştirme kodun çalıştığı düğüm belirtin. Seçebileceğiniz **tüm düğümleri**, **baş düğümlerine yalnızca**, veya **çalışan düğümleri yalnızca**.
    |Parametreler|Komut dosyası tarafından gerekli parametreleri belirtin. Bu boş bırakabilirsiniz şekilde Giraph'ı yüklemek için betik herhangi bir parametre gerektirmez.|  

    Küme üzerinde birden çok bileşenleri yüklemek için birden fazla betik eylemi ekleyebilirsiniz. Komut ekledikten sonra küme oluşturmaya başlamak için onay işaretine tıklayın.

## <a name="use-giraph"></a>Giraph kullanma
Temel göstermek için SimpleShortestPathsComputation örnek kullanıyoruz <a href = "https://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> grafikteki nesneler arasındaki en kısa yolu bulmak için uygulama. Örnek verileri ve örnek jar dosyasını karşıya yükleyin, SimpleShortestPathsComputation örnek bir iş çalıştırmak için aşağıdaki adımları kullanın ve sonra sonuçları görüntüleyin.

1. Bir örnek veri dosyasını Azure Blob depolama alanına yükleyin. Yerel iş istasyonunda, adlı yeni bir dosya oluşturmak **tiny_graph.txt**. Bunu, aşağıdaki satırları içermesi gerekir:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Birincil depolama, HDInsight kümeniz tiny_graph.txt dosyasını yükleyin. Karşıya veri yükleme konusunda yönergeler için bkz. [HDInsight Apache Hadoop işleri için verileri karşıya yükleme](hdinsight-upload-data.md).

    Bu veri biçimi kullanarak bir yönlü graf nesneler arasındaki ilişkiyi tanımlayan [kaynak\_kimliği, kaynak\_değeri, [[dest\_kimliği], [edge\_değeri],...]]. Her satırı arasında bir ilişki temsil eden bir **kaynak\_kimliği** nesnesi ve bir veya daha fazla **dest\_kimliği** nesneleri. **Edge\_değer** (veya ağırlığı), güçlü veya arasındaki bağlantının uzaklık olarak düşünülebilir **source_id** ve **dest\_kimliği**.

    Çıkış çizilmiş ve nesneler arasındaki uzaklık değeri (veya ağırlık) kullanarak, yukarıdaki verileri şuna benzeyebilir:

    ![tiny_graph.txt arasında değişen mesafenin satırlarla bir daire olarak çizilir.](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)

2. SimpleShortestPathsComputation örneği çalıştırın. Giriş olarak tiny_graph.txt dosyası kullanarak örneği çalıştırmak için aşağıdaki Azure PowerShell cmdlet'lerini kullanın.

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

    Yukarıdaki örnekte değiştirin **clustername** giraph'ın yüklü olan, HDInsight kümenizin adıyla.
3. Sonuçlara bakın. İş tamamlandıktan sonra sonuçları iki çıkış dosyalarını depolanacak **wasb: / / / örnek/out/shotestpaths** klasör. Dosya adında **bölümü m 00001** ve **bölümü m 00002**. İndirin ve çıktısını görüntülemek için aşağıdaki adımları gerçekleştirin:

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

    Bu oluşturur **çıkış/örnek/shortestpaths** dizin yapısı iş istasyonu ve indirme iki çıkış dosyalarını bu konuma geçerli dizin.

    Kullanım **Cat** cmdlet'i dosyaların içeriğini görüntülemek için:

        Cat example/output/shortestpaths/part*

    Çıktı aşağıdakine benzer görünmelidir:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Örnek sabit kodlanmış başlamak SimpleShortestPathComputation nesne kimliği 1 ve diğer nesnelerin en kısa yolu bulmak. Çıktı olarak okunacak şekilde `destination_id distance`, nesne kimliği 1 ile hedef kimliğini arasında imkansız kenarlarına değer (veya ağırlık) uzaklığı.

    Bu görselleştirme, kısa yolları kimliği 1 ile tüm diğer nesneler arasında seyahat sonuçları doğrulayabilirsiniz. En kısa yolu kimliği 4 kimliği 1 ila 5 olduğunu unutmayın. Bu arasındaki toplam uzaklık, <span style="color:orange">kimliği 1 ile 3</span>, ardından <span style="color:red">kimliği 3 ve 4</span>.

    ![Nesneler arasında çizilmiş kısa yolları daireler olarak çizme](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-azure-powershell"></a>Azure PowerShell kullanarak Giraph yükleme
Bkz: [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).  Örnek Azure PowerShell kullanarak Apache Spark'ı yüklemek nasıl gösterir. Kullanılacak betiği dosyasını özelleştirmeniz gerektiğini [ https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="install-giraph-using-net-sdk"></a>.NET SDK kullanarak Giraph yükleme
Bkz: [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md). Örnek .NET SDK kullanarak Spark'ı yüklemek nasıl gösterir. Kullanılacak betiği dosyasını özelleştirmeniz gerektiğini [ https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="see-also"></a>Ayrıca bkz.
* [(Linux) HDInsight Hadoop kümeleri üzerinde Apache giraph'ı yükleme](hdinsight-hadoop-giraph-install-linux.md)
* [HDInsight Apache Hadoop kümeleri oluşturma](hdinsight-provision-clusters.md): HDInsight kümeleri oluşturma hakkında genel bilgi.
* [Betik eylemi kullanarak HDInsight kümesi özelleştirme](hdinsight-hadoop-customize-cluster-linux.md): HDInsight kümelerini betik eylemi kullanarak özelleştirme hakkında genel bilgiler.
* [HDInsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions.md).
* [Yükleme ve Apache Spark HDInsight kümelerinde kullanma][hdinsight-install-spark]: Spark'ı yükleme hakkında daha fazla betik eylemi örneği.

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: https://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
