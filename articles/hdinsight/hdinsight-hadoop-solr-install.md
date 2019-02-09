---
title: Solr Hadoop kümesi - Azure yüklemek üzere betik eylemi kullanın
description: HDInsight kümesi ile betik eylemi kullanarak Solr özelleştirmeyi öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2016
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 95b5bbb6c227b5001865a751abddddc4924e7b2d
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55981086"
---
# <a name="install-and-use-apache-solr-on-windows-based-hdinsight-clusters"></a>Yükleme ve Windows tabanlı HDInsight kümeleri üzerinde Apache Solr kullanma

Windows tabanlı HDInsight kümesi Apache Solr betik eylemi kullanarak özelleştirme ve Solr veri aramak için nasıl kullanılacağını öğrenin.

> [!IMPORTANT]  
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı HDInsight kümeleri ile çalışır. HDInsight yalnızca Windows üzerinde HDInsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement). Linux tabanlı küme ile Solr kullanma hakkında daha fazla bilgi için bkz: [yükleme ve (Linux) Hdınsight Hadoop kümeleri üzerinde Apache Solr kullanma](hdinsight-hadoop-solr-install-linux.md).


Kullanarak Azure HDInsight (Hadoop, Storm, HBase, Spark) kümesinde herhangi bir türde üzerinde Solr yükleyebilirsiniz *betik eylemi*. Solr'ın bir HDInsight kümesine yüklemek için örnek betik salt okunur bir Azure depolama blob'nden kullanılabilir [ https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

Örnek betik yalnızca HDInsight kümesi sürüm ile 3.1 çalışır. HDInsight küme sürümleri hakkında daha fazla bilgi için bkz. [HDInsight küme sürümleri](hdinsight-component-versioning.md).

Bu konuda kullanılan örnek betik, belirli bir yapılandırma ile bir Windows tabanlı Solr kümesi oluşturur. Farklı koleksiyonlar, parçalar, şemalar, çoğaltmalar, vb. ile Solr kümesini yapılandırmak, betik ve Solr ikili dosyaları uygun şekilde değiştirmelisiniz.

**İlgili makaleler**

* [Yükleme ve (Linux) Hdınsight Hadoop kümeleri üzerinde Apache Solr kullanma](hdinsight-hadoop-solr-install-linux.md)
* [HDInsight Hadoop kümeleri oluşturma](hdinsight-provision-clusters.md): HDInsight kümeleri oluşturma hakkında genel bilgi.
* [Betik eylemi kullanarak HDInsight kümesi özelleştirme][hdinsight-cluster-customize]: HDInsight kümelerini betik eylemi kullanarak özelleştirme hakkında genel bilgiler.
* [HDInsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions.md).

## <a name="what-is-solr"></a>Solr nedir?
<a href="https://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> veriler üzerinde güçlü tam metin araması sağlayan bir kurumsal arama platformudur. Hadoop depolamak ve yönetmek çok büyük miktarda veri etkinleştirse bile, Apache Solr hızlıca veri almak için arama özellikleri sağlar.

## <a name="install-solr-using-portal"></a>Portalı kullanarak Solr yükleme
1. Kullanarak bir küme oluşturmaya başlayın **özel Oluştur** anlatıldığı gibi seçeneği [Apache Hadoop kümeleri oluşturma HDInsight](hdinsight-provision-clusters.md).
2. Üzerinde **betik eylemleri** Sayfası Sihirbazı'nın **betik eylemi ekleme** aşağıda gösterildiği gibi bir betik eylemi ayrıntılarını sağlamak için:

    ![Bir küme özelleştirmek için betik eylemi kullanmanız](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "küme özelleştirmek için betik eylemini kullanın")

    |Özellik|Değer|
    |---|---|
    |Ad|Betik eylemi için bir ad belirtin. Örneğin, **Solr yükleme**.|
    |Betiği URI'si|Küme özelleştirmek için çağrılır betik Tekdüzen Kaynak Tanımlayıcısı (URI) belirtin. Örneğin, *https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1*|
    |Düğüm Türü|Özelleştirme kodun çalıştığı düğüm belirtin. Seçebileceğiniz **tüm düğümleri**, **baş düğümlerine yalnızca**, veya **çalışan düğümleri yalnızca**.
    |Parametreler|Komut dosyası tarafından gerekli parametreleri belirtin. Bu boş bırakabilirsiniz şekilde Solr yükleme betiği herhangi bir parametre gerektirmez.|

    Küme üzerinde birden çok bileşenleri yüklemek için birden fazla betik eylemi ekleyebilirsiniz. Komut ekledikten sonra küme oluşturmaya başlamak için onay işaretine tıklayın.

## <a name="use-solr"></a>Solr kullanma
Solr ile bazı veri dosyalarının dizinini oluşturarak ile başlamalıdır. Solr ardından dizinli veriler üzerinde arama sorguları çalıştırmak için de kullanabilirsiniz. Solr'ın bir HDInsight kümesinde kullanmak için aşağıdaki adımları gerçekleştirin:

1. **Uzak Masaüstü Protokolü (RDP) için Uzak HDInsight kümesine ile yüklü Solr kullanma**. Azure portalından, solr'ın yüklü ve ardından uzak kümeye oluşturduğunuz küme için Uzak masaüstünü etkinleştirin. Yönergeler için [RDP kullanarak HDInsight kümelerini Bağlan](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Dizin Solr'ın veri dosyaları karşıya yükleyerek**. Solr dizin, içindeki arama yapmak için ihtiyacınız olan belgeleri yerleştirin. Solr dizin, RDP uzak kümeye kullanın, masaüstüne gidin, Hadoop komut satırı açın ve gidin **C:\apps\dist\solr-4.7.2\example\exampledocs**. Şu komutu çalıştırın:

        java -jar post.jar solr.xml monitor.xml

    Konsolunda aşağıdaki çıktıyı görürsünüz:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    İki örnek belge ile post.jar yardımcı programı dizinler Solr **solr.xml** ve **monitor.xml**. Solr yükleme post.jar yardımcı programı ve örnek belgeleri kullanılabilir.
3. **Solr içinde dizinli belgelerde arama yapmak için kullanılacağını**. HDInsight kümesi için RDP oturumu, Internet Explorer'ı açın ve Solr Panosu'nda başlatın **http://headnodehost:8983/solr/#/**. Sol bölmeden gelen **çekirdek Seçici** açılan listesinde, select **collection1**, tıklatıp, içinde **sorgu**. Örneğin, seçin ve tüm belgeleri Solr döndürmek için aşağıdaki değerleri sağlayın:

   * İçinde **q** metin kutusuna  **\*:**\*. Bu dizine alınmış tüm belgelerin Solr döndürür. Belirli bir dizeyi belgelerde arama yapmak istiyorsanız, buraya o dizenin girebilirsiniz.
   * İçinde **wt** metin kutusunda, çıkış biçimini seçin. Varsayılan değer **json**. Tıklayın **sorguyu**.

     ![Bir küme özelleştirmek için betik eylemi kullanmanız](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Solr Panoda bir sorgu çalıştırın")

     Çıktı, Solr dizin oluşturma için kullanılan iki belgeleri döndürür. Çıktı aşağıdakine benzer:

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
4. **Önerilen: Solr dizinlenmiş verileri Azure Blob Depolama, HDInsight kümesi ile ilişkili yedekleme**. İyi bir yöntem olarak, Azure Blob Depolama üzerine Solr küme düğümlerinden dizinlenmiş verileri yedeklemelisiniz. Bunu yapmak için aşağıdaki adımları gerçekleştirin:

   1. RDP oturumundan, Internet Explorer'ı açın ve aşağıdaki URL'ye noktası:

           http://localhost:8983/solr/replication?command=backup

       Şuna benzer bir yanıt görmeniz gerekir:
            
      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
          <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
            <str name="status">OK</str>
          </response>
      ```
      
   2. Uzak bir oturumda {SOLR_HOME için} gidin\{koleksiyon} \data. Bu örnek betik oluşturulan küme için olmalıdır `C:\apps\dist\solr-4.7.2\example\solr\collection1\data`. Bu konumda benzer bir ad ile oluşturulmuş bir anlık görüntü klasörü görmelisiniz **anlık görüntü.* zaman damgası ***.
   
   3. Anlık görüntü klasörü zip ve Azure Blob depolama alanına yükleyin. Hadoop komut satırından aşağıdaki komutu kullanarak anlık görüntü klasörü konumuna gidin:

      ```
      hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data
      ```

   Bu komut anlık görüntü /example/data/kapsayıcı içinde ' % s'varsayılan kümeyle ilişkili depolama hesabı altında kopyalar.

## <a name="install-solr-using-azure-powershell"></a>Azure PowerShell kullanarak Solr yükleme
Bkz: [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Örnek Azure PowerShell kullanarak Apache Spark'ı yüklemek nasıl gösterir. Kullanılacak betiği dosyasını özelleştirmeniz gerektiğini [ https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>.NET SDK kullanarak Solr yükleme
Bkz: [özelleştirme HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Örnek .NET SDK kullanarak Apache Spark'ı yüklemek nasıl gösterir. Kullanılacak betiği dosyasını özelleştirmeniz gerektiğini [ https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="see-also"></a>Ayrıca bkz.
* [Yükleme ve (Linux) Hdınsight Hadoop kümeleri üzerinde Apache Solr kullanma](hdinsight-hadoop-solr-install-linux.md)
* [HDInsight Apache Hadoop kümeleri oluşturma](hdinsight-provision-clusters.md): HDInsight kümeleri oluşturma hakkında genel bilgi.
* [Betik eylemi kullanarak HDInsight kümesi özelleştirme][hdinsight-cluster-customize]: HDInsight kümelerini betik eylemi kullanarak özelleştirme hakkında genel bilgiler.
* [HDInsight için betik eylemi betikleri geliştirme](hdinsight-hadoop-script-actions.md).
* [Yükleme ve Apache Spark HDInsight kümelerinde kullanma][hdinsight-install-spark]: Spark'ı yükleme hakkında daha fazla betik eylemi örneği.
* [HDInsight kümeleri üzerinde Apache Giraph yükleme](hdinsight-hadoop-giraph-install.md): Giraph yükleme hakkında daha fazla betik eylemi örneği.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
