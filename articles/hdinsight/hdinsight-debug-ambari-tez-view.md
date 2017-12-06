---
title: "Hdınsight ile - Azure Ambari Tez görünümünü kullanın | Microsoft Docs"
description: "Tez işlerinde hdınsight'ta hata ayıklamak için Ambari Tez görünümü kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 9c39ea56-670b-4699-aba0-0f64c261e411
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2017
ms.author: larryfr
ms.openlocfilehash: b565ef0f7672d1288e922e28551ad3f6ec5b6cb7
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="use-ambari-views-to-debug-tez-jobs-on-hdinsight"></a>Tez işlerinde hdınsight'ta hata ayıklamak için Ambari görünümleri kullanma

Hdınsight için Ambari Web kullanıcı arabirimini anlama ve Tez kullanan işleri hata ayıklamak için kullanılan bir Tez görünümü içerir. Tez görünümü iş bağlı öğelerinin bir grafik olarak görselleştirme, her öğenin ayrıntısına ve istatistikler ve günlük bilgileri almasını sağlar.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz: [Hdınsight bileşen sürümü oluşturma](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Ön koşullar

* Linux tabanlı Hdınsight kümesi. Küme oluşturma adımları için bkz: [Linux tabanlı Hdınsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).
* HTML5'i destekleyen modern bir web tarayıcısı.

## <a name="understanding-tez"></a>Tez anlama

Tez geleneksel MapReduce işleme büyük hızlarından sağlayan hadoop'ta veri işleme için genişletilebilir bir çerçevedir. Linux tabanlı Hdınsight kümeleri için onu varsayılan Hive için altyapısıdır.

Tez yönlendirilmiş Çevrimsiz grafik (işlerinin gerekli eylemlerin sırasını açıklayan DAG) oluşturur. Tek tek Eylemler köşeleri olarak adlandırılır ve genel iş parçası yürütün. Gerçek yürütme köşe tarafından açıklanan iş bir görev çağrılır ve kümedeki birden çok düğüm arasında dağıtılmış.

### <a name="understanding-the-tez-view"></a>Tez görünüm anlama

Tez görünümü, çalışan işlemler üzerinde hem geçmiş bilgileri hem de bilgi sağlar. Bu bilgileri, bir iş kümeler arasında nasıl dağıtıldığını gösterir. Ayrıca, görevler ve köşeleri tarafından kullanılan sayaçlarını ve işle ilgili hata bilgilerini görüntüler. Aşağıdaki senaryolarda yararlı bilgiler teklif edebilir:

* Uzun süre çalışan izleme harita ilerlemesini görüntüleme, işler ve görevler azaltır.
* İşleme nasıl geliştirilmiş veya neden başarısız öğrenmek başarılı veya başarısız işlemler için geçmiş verileri analiz etme.

## <a name="generate-a-dag"></a>Bir DAG oluştur

Tez görünüm Tez Altyapısı şu anda çalışıyor ya da bırakıldı kullanan önceden çalıştıran bir iş, yalnızca veri içeriyor. Basit Hive sorguları, Tez kullanmadan çözülebilir. Daha karmaşık Bu filtreleme, gruplama, sıralama, birleşimler, vb. sorgular. Tez altyapısını kullanır.

Tez kullanan bir Hive sorgusu çalıştırmak için aşağıdaki adımları kullanın:

1. Bir web tarayıcısında https://CLUSTERNAME.azurehdinsight.net için gidin nerede **CLUSTERNAME** Hdınsight kümenizin adıdır.

2. Sayfanın üstündeki menüsünden seçin **görünümleri** simgesi. Bu simgeyi bir dizi kare gibi görünüyor. Görüntülenen açılır menüde seçin **Hive görünümü**.

    ![Hive görünümünü seçme](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. Hive görünümü yüklediğinde, aşağıdaki sorguyu sorgu düzenleyicisine yapıştırın ve ardından **yürütme**.

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    İş tamamlandıktan sonra görüntülenen bir çıktı göreceksiniz **sorgu işleminin sonuçları** bölümü. Sonuçlar aşağıdakine benzer olmalıdır:

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. Seçin **günlük** sekmesi. Aşağıdakine benzer bilgiler bakın:

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    Kaydet **uygulama kimliği** değeri olarak bu değer bir sonraki bölümde kullanılır.

## <a name="use-the-tez-view"></a>Tez görünümünü kullanın

1. Sayfanın üstündeki menüsünden seçin **görünümleri** simgesi. Görüntülenen açılır menüde seçin **Tez Görünüm**.

    ![Tez görünümü seçme](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. Tez görünüm yüklenirken, şu anda çalışmakta olan veya kaldırılmış hive sorguları listesini küme üzerinde çalışan bakın.

    ![Tüm DAG'leri](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. Yalnızca bir giriş varsa, önceki bölümde çalıştırdığınız sorgu içindir. Birden çok girdi varsa, sayfanın en üstünde alanlar kullanılarak arayabilirsiniz.

4. Seçin **sorgu kimliği** bir Hive sorgusu için. Sorgu hakkında bilgi görüntülenir.

    ![DAG ayrıntıları](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. Bu sayfa sekmelerinde, aşağıdaki bilgileri görüntülemek izin ver:

    * **Sorgu ayrıntıları**: Hive sorgusu hakkında ayrıntılar.
    * **Zaman Çizelgesi**: her aşaması ne kadar sürdü hakkında bilgi.
    * **Yapılandırmaları**: Bu sorgu için kullanılan yapılandırma.

    Gelen __sorgu ayrıntıları__ bağlantılar hakkında bilgi bulmak için kullanabileceğiniz __uygulama__ veya __DAG__ bu sorgu için.
    
    * __Uygulama__ bağlantı bu sorgu için YARN uygulaması hakkında daha fazla bilgi görüntüler. Buradan YARN uygulama günlüklerini erişebilir.
    * __DAG__ bağlantı yönlendirilmiş Çevrimsiz grafik için bu sorguyu hakkında daha fazla bilgi görüntüler. Buradan, grafiksel DAG görüntüleyebilirsiniz. Ayrıca, DAG içinde köşeleri hakkında bilgi bulabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Tez görünümü kullanmak öğrendiniz, daha fazla bilgi edinmek [hdınsight'ta Hive kullanarak](hadoop/hdinsight-use-hive.md).

Daha ayrıntılı Tez teknik bilgi için bkz: [Hortonworks Tez sayfanın](http://hortonworks.com/hadoop/tez/).

Ambari kullanarak Hdınsight ile ilgili daha fazla bilgi için bkz: [Ambari Web kullanıcı arabirimini kullanarak Hdınsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)
