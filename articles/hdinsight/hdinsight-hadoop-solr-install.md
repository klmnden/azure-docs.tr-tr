---
title: Hadoop kümesi - Azure Solr yüklemek için betik eylemi kullanın | Microsoft Docs
description: Hdınsight kümesi ile betik eylemi kullanarak Solr özelleştirmeyi öğrenin.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b1e6f338-8ac1-4b38-bbb5-2f7388b9de3b
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: d263f6255eedb9b45b7f0b232e1595197556b7c3
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a>Yükleme ve Windows tabanlı Hdınsight kümelerinde Solr kullanma

Windows tabanlı Hdınsight kümesi ile betik eylemi kullanarak Solr özelleştirmeyi ve Solr verileri aramak için nasıl kullanılacağını öğrenin.

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı Hdınsight kümeleri ile çalışır. Hdınsight yalnızca Windows'da Hdınsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Linux tabanlı bir kümeyle Solr kullanma hakkında daha fazla bilgi için bkz: [yükleme ve kullanma Solr Hdınsight Hadoop kümeleri (Linux) üzerinde](hdinsight-hadoop-solr-install-linux.md).


Solr Azure Hdınsight (Hadoop, Storm, HBase, Spark) kümede herhangi bir türde kullanarak yükleyebileceğiniz *betik eylemi*. Solr bir Hdınsight kümesine yüklemek için örnek komut dosyası salt okunur Azure depolama blobunu gelen kullanılabilir [ https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

Örnek komut dosyası yalnızca Hdınsight küme sürümü ile 3.1 çalışır. Hdınsight küme sürümleri hakkında daha fazla bilgi için bkz: [Hdınsight küme sürümleri](hdinsight-component-versioning.md).

Bu konuda kullanılan örnek betik, belirli bir yapılandırmayla Windows tabanlı Solr küme oluşturur. Farklı Koleksiyonlara, parça, şemalar, çoğaltmalar, vb. ile Solr küme yapılandırmak istiyorsanız, Solr ikili dosyaları ve komut dosyası uygun şekilde değiştirmelisiniz.

**İlgili makaleler**

* [Yükleme ve Solr Hdınsight Hadoop kümeleri (Linux) kullanma](hdinsight-hadoop-solr-install-linux.md)
* [Hdınsight'ta Hadoop kümeleri oluşturma](hdinsight-provision-clusters.md): Hdınsight kümeleri oluşturma hakkında genel bilgi.
* [Betik eylemi kullanarak Hdınsight kümesi özelleştirme][hdinsight-cluster-customize]: betik eylemi kullanarak Hdınsight kümelerini özelleştirme hakkında genel bilgi.
* [Hdınsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions.md).

## <a name="what-is-solr"></a>Solr nedir?
<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> güçlü tam metin araması verileri sağlayan bir kurumsal arama platformudur. Hadoop depolamak ve çok büyük miktarda veri yönetme olanak sağlarken, Apache Solr hızlı bir şekilde veri almak için arama yetenekleri sağlar.

## <a name="install-solr-using-portal"></a>Portal kullanarak Solr yükleyin
1. Kullanarak bir küme oluşturmaya başlamak **özel Oluştur** konusunda açıklandığı gibi seçeneği [Hdınsight'ta oluşturmak Hadoop kümeleri](hdinsight-provision-clusters.md).
2. Üzerinde **betik eylemleri** Sayfa Sihirbazı'nın tıklatın **betik eylemi eklemek** aşağıda gösterildiği gibi betik eylemi hakkındaki ayrıntıları sağlamak için:

    ![Bir küme özelleştirmek için betik eylemi kullanın](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "bir küme özelleştirmek için kullanım betik eylemi")

    <table border='1'>
        <tr><th>Özellik</th><th>Değer</th></tr>
        <tr><td>Ad</td>
            <td>Betik eylemi için bir ad belirtin. Örneğin, <b>yükleme Solr</b>.</td></tr>
        <tr><td>Betik URI'si</td>
            <td>Küme özelleştirmek için çağrılan betik Tekdüzen Kaynak Tanımlayıcısı (URI) belirtin. Örneğin, <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Düğüm Türü</td>
            <td>Özelleştirme kodun çalıştığı düğüm belirtin. Seçebileceğiniz <b>tüm düğümleri</b>, <b>Head yalnızca düğümlerin</b>, veya <b>yalnızca çalışan düğümleri</b>.
        <tr><td>Parametreler</td>
            <td>Komut dosyası tarafından gerekli parametreleri belirtin. Bunu boş bırakabilirsiniz şekilde Solr yüklemek için komut dosyası herhangi bir parametre gerektirmez.</td></tr>
    </table>

    Küme üzerinde birden çok bileşeni yüklemek için birden fazla betik eylemi ekleyebilirsiniz. Komut dosyaları ekledikten sonra küme oluşturmaya başlamak için onay işaretine tıklayın.

## <a name="use-solr"></a>Solr kullanma
Bazı veri dosyalarıyla Solr dizin ile başlamalıdır. Solr sonra dizinli verileri arama sorguları çalıştırmak için de kullanabilirsiniz. Solr bir Hdınsight kümesi içinde kullanmak için aşağıdaki adımları gerçekleştirin:

1. **Uzak Masaüstü Protokolü (RDP) için Hdınsight kümesine yüklü Solr ile kullanma uzaktan**. Azure portalından, kümeye yüklü Solr ve sonra uzaktan ile oluşturduğunuz küme için Uzak Masaüstü'nü etkinleştirin. Yönergeler için bkz: [RDP kullanarak Hdınsight kümelerini Bağlan](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Dizin veri dosyaları yükleyerek Solr**. Solr dizin oluşturduğunuzda, arama gerekebilir, belgeleri yerleştirin. Solr dizin için RDP kümesine uzak kullanın, masaüstüne gidin, Hadoop komut satırı açın ve gidin **C:\apps\dist\solr-4.7.2\example\exampledocs**. Şu komutu çalıştırın:

        java -jar post.jar solr.xml monitor.xml

    Konsola aşağıdaki çıktı görürsünüz:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Post.jar yardımcı programı, iki örnek belgelerle Solr dizinler **solr.xml** ve **monitor.xml**. Solr yüklemeye post.jar yardımcı programı ve örnek belgeleri kullanılabilir.
3. **Solr Pano içinde dizinlenmiş belgeleri aramak için kullanın**. RDP oturumu Hdınsight kümesi için Internet Explorer'ı açın ve Solr Panosu'nda başlatın ** http://headnodehost:8983/solr/#/ **. Sol bölmeden gelen **çekirdek Seçici** açılan listesinde, select **collection1**, tıklatıp, içinde **sorgu**. Örnek olarak, seçin ve tüm belgeleri Solr döndürmek için aşağıdaki değerleri girin:

   * İçinde **q** metin kutusuna ** \*:**\*. Bu dizini tüm belgeleri Solr döndürür. Belirli bir dizeyi belgelerde arama yapmak istiyorsanız, o dizeyi buraya girebilirsiniz.
   * İçinde **wt** metin kutusunda, çıktı biçimi seçin. Varsayılan değer **json**. Tıklatın **sorgu yürütme**.

     ![Bir küme özelleştirmek için betik eylemi kullanın](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Solr Panoda bir sorgu çalıştırın")

     Çıktı Solr dizin oluşturma işlemi için kullanılan iki belgeleri döndürür. Çıktı aşağıdakine benzer:

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, the Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication to other Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over the e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }
4. **Önerilen: dizinli Solr Hdınsight kümesi ile ilişkili Azure Blob Depolama yedekleme**. İyi bir uygulama, Azure Blob Depolama üzerine Solr küme düğümlerinden dizinli verileri yedeklemelisiniz. Bunu yapmak için aşağıdaki adımları gerçekleştirin:

   1. RDP oturumu Internet Explorer'ı açın ve aşağıdaki URL'yi noktası:

           http://localhost:8983/solr/replication?command=backup

       Şuna benzer bir yanıt görmeniz gerekir:

           <?xml version="1.0" encoding="UTF-8"?>
           <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
             <str name="status">OK</str>
           </response>
   2. {SOLR_HOME} uzak oturumda gidin\{koleksiyonu} \data. Örnek komut dosyası oluşturulan küme için bu olmalıdır **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. Bu konumda benzer bir ad ile oluşturulmuş bir anlık görüntü klasörü görmelisiniz **anlık görüntü.* zaman damgası ***.
   3. Anlık görüntü klasörü zip ve Azure Blob depolama alanına yükleyin. Hadoop komut satırından aşağıdaki komutu kullanarak anlık görüntü klasörü konuma gidin:

             hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

       Bu komut anlık görüntü /example/data/varsayılan kümeyle ilişkili depolama hesabı kapsayıcıda altında kopyalar.

## <a name="install-solr-using-aure-powershell"></a>Solr işlemleri PowerShell kullanarak yükleme
Bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Örnek, Azure PowerShell kullanarak Spark yükleneceği gösterilmiştir. Kullanılacak komut dosyasını özelleştirmeniz gerekiyorsa [ https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>.NET SDK kullanarak Solr yükleyin
Bkz: [özelleştirme Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Örnek, .NET SDK kullanarak Spark yükleneceği gösterilmiştir. Kullanılacak komut dosyasını özelleştirmeniz gerekiyorsa [ https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="see-also"></a>Ayrıca bkz.
* [Yükleme ve Solr Hdınsight Hadoop kümeleri (Linux) kullanma](hdinsight-hadoop-solr-install-linux.md)
* [Hdınsight'ta Hadoop kümeleri oluşturma](hdinsight-provision-clusters.md): Hdınsight kümeleri oluşturma hakkında genel bilgi.
* [Betik eylemi kullanarak Hdınsight kümesi özelleştirme][hdinsight-cluster-customize]: betik eylemi kullanarak Hdınsight kümelerini özelleştirme hakkında genel bilgi.
* [Hdınsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions.md).
* [Yükleme ve Spark Hdınsight kümelerinde kullanma][hdinsight-install-spark]: Spark yükleme hakkında örnek betik eylemi.
* [Hdınsight kümelerinde R yüklemek][hdinsight-install-r]: betik eylemi örnek r yükleme hakkında
* [Hdınsight kümelerinde Giraph yükleme](hdinsight-hadoop-giraph-install.md): Giraph yükleme hakkında örnek betik eylemi.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
