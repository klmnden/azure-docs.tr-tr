---
title: "Hdınsight kümeleri - Azure özelleştirmek için R kullanımda | Microsoft Docs"
description: "Betik eylemi kullanarak R yüklemeyi öğrenin ve Hdınsight kümelerinde R kullanın."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: be851270-afa5-4af0-a69e-2d343a4deeb7
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 5b9b793d49217acd9f0c6c518596a7afb5600d69
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>HDInsight'taki Hadoop kümelerinde R programını yükleme ve kullanma

Windows özelleştirmek nasıl Hdınsight kümesi ile betik eylemi kullanarak R dayalı ve kümeleri Hdınsight'ta R kullanmayı öğrenin. [Hdınsight teklifi](https://azure.microsoft.com/pricing/details/hdinsight/) R Server Hdınsight kümenize bir parçası olarak içerir. Bu, MapReduce ve Spark dağıtılmış hesaplamaları çalıştırmak R betiklerini sağlar. Daha fazla bilgi için bkz. [HDInsight R Server kullanmaya başlama](hdinsight-hadoop-r-server-get-started.md). Linux tabanlı bir kümeyle R kullanma hakkında daha fazla bilgi için bkz: [yükleme ve kullanma R Hdınsight Hadoop kümeleri (Linux) üzerinde](hdinsight-hadoop-r-scripts-linux.md).

Kullanarak Azure Hdınsight (Hadoop, Storm, HBase, Spark) kümede herhangi bir türde üzerinde R yükleyebilirsiniz *betik eylemi*. R bir Hdınsight kümesine yüklemek için örnek komut dosyası salt okunur Azure depolama blobunu gelen kullanılabilir [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

**İlgili makaleler**

* [Yükleme ve Hdınsight Hadoop kümeleri (Linux) R kullanma](hdinsight-hadoop-r-scripts-linux.md)
* [Hdınsight'ta Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md): Hdınsight kümeleri oluşturma hakkında genel bilgiler
* [Betik eylemi kullanarak Hdınsight kümesi özelleştirme][hdinsight-cluster-customize]: betik eylemi kullanarak Hdınsight kümelerini özelleştirme hakkında genel bilgiler
* [Hdınsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>R nedir?
<a href="http://www.r-project.org/" target="_blank">R proje istatistiksel bilgi işlem için</a> bir açık kaynak dili ve istatistiksel bilgi işlem ortamı. R yüzlerce yapı içinde istatistik işlevleri ve işlev ve nesne odaklı programlama yönlerini birleştirir kendi programlama dili sağlar. Yoğun grafik yetenekleri de sağlar. R en profesyonel istatistikçiler ve çeşitli alanları bilimcilerine için tercih edilen programlama ortamıdır.

R Azure Blob Storage (WASB) ile uyumlu, böylelikle var. depolanan verileri R kullanarak işlenebilir.  

## <a name="install-r"></a>R yükleme
A [örnek komut dosyası](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) bir Hdınsight'ta R yüklemek için küme salt okunur bir blob Azure depolama alanında kullanılabilir. Bu bölümde Azure Portalı'nı kullanarak küme oluşturulurken örnek betiği kullanmak hakkında yönergeler sağlar.

> [!NOTE]
> Örnek betiği Hdınsight kümesi sürüm 3.1 sunulmuştur. Hdınsight küme sürümleri hakkında daha fazla bilgi için bkz: [Hdınsight küme sürümleri](hdinsight-component-versioning.md).
>
>

1. Portaldan bir Hdınsight kümesi oluştururken tıklatın **isteğe bağlı yapılandırma**ve ardından **betik eylemleri**.
2. Üzerinde **betik eylemleri** sayfasında, aşağıdaki değerleri girin:

    ![Bir küme özelleştirmek için betik eylemi kullanın](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "bir küme özelleştirmek için kullanım betik eylemi")

    <table border='1'>
        <tr><th>Özellik</th><th>Değer</th></tr>
        <tr><td>Ad</td>
            <td>Örneğin, betik eylemi için bir ad belirtmeniz <b>R yükleme</b>.</td></tr>
        <tr><td>Betik URI'si</td>
            <td>Bu gibi bir durumda küme özelleştirmek için çağrılan betik URI'si belirtin <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Düğüm Türü</td>
            <td>Özelleştirme kodun çalıştığı düğüm belirtin. Seçebileceğiniz <b>tüm düğümleri</b>, <b>Head yalnızca düğümlerin</b>, veya <b>çalışan düğümleri</b> yalnızca.
        <tr><td>Parametreler</td>
            <td>Komut dosyası tarafından gerekli parametreleri belirtin. Ancak, bunu boş bırakabilirsiniz şekilde R yüklemek için komut dosyası herhangi bir parametre gerektirmez.</td></tr>
    </table>

    Küme üzerinde birden çok bileşeni yüklemek için birden fazla betik eylemi ekleyebilirsiniz. Komut dosyaları ekledikten sonra küme crating başlatmak için onay işaretine tıklayın.

Komut dosyası, Azure PowerShell veya Hdınsight .NET SDK kullanarak Hdınsight'ta R yüklemek için de kullanabilirsiniz. Bu yordamları için yönergeler bu makalenin sonraki bölümlerinde verilmiştir.

## <a name="run-r-scripts"></a>R betiği çalıştırın
Bu bölümde, Hdınsight ile Hadoop küme üzerinde bir R betiği çalıştırmak açıklar.

1. **Küme için Uzak Masaüstü Bağlantısı**: gelen portalı yüklü R ile oluşturduğunuz küme için Uzak masaüstünü etkinleştir ve kümeye bağlanın. Yönergeler için bkz: [RDP kullanarak Hdınsight kümelerini Bağlan](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **R Konsolu**: R yükleme baş düğüm masaüstünde R konsola bir bağlantı yerleştirir. R konsolunu açmak için tıklayın.
3. **R betiği Çalıştır**: R betiği, yapıştırma seçerek ve ENTER tuşuna basarak doğrudan R konsolundan çalıştırılabilir. Burada, 1 ile 100 arası sayılar oluşturur ve bunları 2 ile çarpar basit bir örnek komut verilmiştir.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

İlk iki satır ile r yüklü RHadoop kitaplıkları çağırın Son satırı sonuçlarını konsola yazdırır. Çıktı aşağıdaki gibi görünmelidir:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>R işlemleri PowerShell kullanarak yükleme
Bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Örnek, Azure PowerShell kullanarak Spark yükleneceği gösterilmiştir. Kullanılacak komut dosyasını özelleştirmeniz gerekiyorsa [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## <a name="install-r-using-net-sdk"></a>.NET SDK kullanarak R yükleme
Bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Örnek, .NET SDK kullanarak Spark yükleneceği gösterilmiştir. Kullanılacak komut dosyasını özelleştirmeniz gerekiyorsa [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).

## <a name="see-also"></a>Ayrıca bkz.
* [Yükleme ve Hdınsight Hadoop kümeleri (Linux) R kullanma](hdinsight-hadoop-r-scripts-linux.md)
* [Hdınsight'ta Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md): Hdınsight kümeleri oluşturma hakkında genel bilgiler
* [Betik eylemi kullanarak Hdınsight kümesi özelleştirme][hdinsight-cluster-customize]: betik eylemi kullanarak Hdınsight kümelerini özelleştirme hakkında genel bilgiler
* [Hdınsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions.md)
* [Yükleme ve Spark Hdınsight kümelerinde kullanma][hdinsight-install-spark]: betik eylemi örnek Spark yükleme hakkında
* [Hdınsight kümelerinde Giraph yükleme](hdinsight-hadoop-giraph-install.md): betik eylemi örnek Giraph yükleme hakkında
* [Hdınsight kümelerinde Solr yükleme](hdinsight-hadoop-solr-install-linux.md): Solr yükleme hakkında örnek betik eylemi.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
