---
title: "Sorgu konsolunda hdınsight'ta - Azure Hadoop Hive kullanma | Microsoft Docs"
description: "Web tabanlı sorgu konsol tarayıcınızdan bir Hdınsight Hadoop kümesindeki Hive sorguları çalıştırmak için nasıl kullanılacağını öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5ae074b0-f55e-472d-94a7-005b0e79f779
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: d6032b8a1e3d338b046c958804102aeb9efcf4ab
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="run-hive-queries-using-the-query-console"></a>Sorgu konsolunu kullanarak Hive sorguları çalıştırma
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Bu makalede, Hdınsight sorgu konsol tarayıcınızdan bir Hdınsight Hadoop kümesindeki Hive sorguları çalıştırmak için nasıl kullanılacağını öğreneceksiniz.

> [!IMPORTANT]
> Hdınsight sorgu konsolu yalnızca Windows tabanlı Hdınsight kümelerinde kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Hdınsight 3.4 veya büyük, bkz [Hive sorgularını çalıştırma Ambari Hive görünümünde](apache-hadoop-use-hive-ambari-view.md) bir web tarayıcısından Hive sorguları çalıştırma hakkında bilgi.

## <a id="prereq"></a>Önkoşullar
Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir.

* Windows tabanlı Hdınsight Hadoop kümesi
* Modern bir web tarayıcısı

## <a id="run"></a>Sorgu konsolunu kullanarak Hive sorguları çalıştırma
1. Bir web tarayıcısı açın ve gidin **https://CLUSTERNAME.azurehdinsight.net**, burada **CLUSTERNAME** Hdınsight kümenizin adıdır. İstenirse, kullanıcı adını ve küme oluştururken kullandığınız parolayı girin.
2. Sayfanın üstündeki bağlantılardan birini seçin **Hive Düzenleyicisi**. Bu, Hdınsight kümesinde çalıştırmak istediğiniz HiveQL ifadelerini girmek için kullanılan bir form görüntüler.

    ![hive düzenleyicisinin](./media/apache-hadoop-use-hive-query-console/queryconsole.png)

    Metnin yerine `Select * from hivesampletable` aşağıdaki HiveQL ifadelerini ile:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

   * **DROP TABLE**: Tablo zaten varsa, tablo ve veri dosyasını siler.
   * **Dış tablo oluştur**: kovanında yeni bir 'external' tablo oluşturur. Dış tablolara yalnızca tablo tanımı kovanında depolamak; veriler özgün konumda kalır.

     > [!NOTE]
     > Bir dış kaynağa (örneğin, bir otomatik veri karşıya yükleme işlemi) veya başka bir MapReduce işlemi tarafından güncelleştirilecek temel alınan veri beklediğiniz dış tablolara kullanılması gereken, ancak her zaman en son verileri kullanmak üzere Hive sorguları istiyor.
     >
     > Bir dış tablo bırakma mu **değil** verileri, yalnızca tablo tanımını silin.
     >
     >
   * **SATIR biçimi**: verileri nasıl biçimlendirildiğini Hive söyler. Bu durumda, her günlüğün içinde alanlar boşlukla ayrılır.
   * **AS TEXTFILE konumu DEPOLANAN**: (örneğin/veri dizini) depolanan verileri nerede Hive bildirir ve metin olarak depolanır
   * **SEÇİN**: tüm satırların sayımını seçin burada sütun **t4** değerini içeren **[Hata]**. Bu değeri döndürmelidir **3** çünkü bu değer içeren üç satır vardır.
   * **INPUT__FILE__NAME gibi '%.log'** -biz yalnızca veri biten dosyalarından döndürmelidir Hive söyler. günlük. Bu arama verileri içeren ve verileri diğer örnekten tanımladığımız şema eşleşmiyor veri dosyalarını döndürmesini tutar sample.log dosyasına kısıtlar.
3. Tıklatın **gönderme**. **Proje oturumunu** sayfasının en altında işe yönelik ayrıntıları görüntülenmelidir.
4. Zaman **durum** alan değişiklikleri **tamamlandı**seçin **ayrıntıları görüntüle** iş için. Ayrıntıları sayfasında **iş çıktısı** içeren `[ERROR]    3`. Kullanabileceğiniz **karşıdan** düğmesi iş çıkışını içeren bir dosyayı indirmek için bu alanı altında.

## <a id="summary"></a>Özet
Gördüğünüz gibi sorgu konsolu bir Hdınsight kümesi Hive sorgularını çalıştırma, iş durumunu izleyebilir ve çıkış almak için kolay bir yol sağlar.

Hive işlerini çalıştırmak için Hive sorgusu konsolunu kullanma hakkında daha fazla bilgi için seçin **Başlarken** sorgu konsol üst kısmında, ardından sağlanan örnekleri kullanır. Her örnek örnekte kullanılan HiveQL ifadelerini hakkında açıklamalar dahil verileri analiz etmek için Hive kullanma sürecinde size yol göstermektedir.

## <a id="nextsteps"></a>Sonraki adımlar
Hdınsight'ta Hive hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Tez Hive ile kullanıyorsanız, hata ayıklama bilgileri için aşağıdaki belgelere bakın:

* [Windows tabanlı Hdınsight üzerinde Tez kullanıcı arabirimini kullanma](../hdinsight-debug-tez-ui.md)
* [Linux tabanlı Hdınsight üzerinde Ambari Tez görünümünü kullanın](../hdinsight-debug-ambari-tez-view.md)

[1]:apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
