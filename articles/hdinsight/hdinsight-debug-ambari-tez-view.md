---
title: HDInsight - Azure ile Ambari Tez görünümünü kullanın
description: HDInsight üzerinde Tez işlerinin hatalarını ayıklamak için Ambari Tez görünümü kullanmayı öğrenin.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: jasonh
ms.openlocfilehash: 576460f4b68d670e534e0ddeed920f7ac99e1458
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43108895"
---
# <a name="use-ambari-views-to-debug-tez-jobs-on-hdinsight"></a>HDInsight üzerinde Tez işlerinin hatalarını ayıklamak için Ambari görünümlerini kullanma

HDInsight için Ambari Web kullanıcı arabirimini anlama ve kullanma Tez işlerinin hatalarını ayıklamak için kullanılan bir Tez görünümü içerir. Tez görünümü bağlı öğelerin bir grafik olarak görselleştirin, her öğenin ayrıntısına ve istatistikleri ve günlük bilgilerini almak sağlar.

> [!IMPORTANT]
> Bu belgedeki adımlar, Linux kullanan bir HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight bileşen sürümü oluşturma](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Önkoşullar

* Bir Linux tabanlı HDInsight kümesi. Küme oluşturma adımları için bkz [Linux tabanlı HDInsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).
* HTML5'i destekleyen modern bir web tarayıcısı.

## <a name="understanding-tez"></a>Tez anlama

Tez büyük işleyebileceğiniz geleneksel MapReduce işleme özellikleri sağlayan Hadoop'u veri işleme için genişletilebilir bir çerçevedir. Linux tabanlı HDInsight kümeleri için Hive için varsayılan altyapısı olan.

Tez yönlendirilmiş Çevrimsiz graf (işleri tarafından gereken eylemlerin sırasını açıklayan DAG) oluşturur. Bireysel eylemleri köşeler verilir ve genel projenin parçası yürütün. Gerçek yürütmesi, bir köşe tarafından açıklanan iş bir görev çağrılır ve kümedeki birden fazla düğümde dağıtılmış.

### <a name="understanding-the-tez-view"></a>Tez görünümü anlama

Tez görünümü çalışan işlemler üzerinde hem geçmiş bilgileri hem de bilgi sağlar. Bu bilgiler, bir işi kümeler arasında nasıl dağıtıldığını gösterir. Ayrıca, görevler ve köşeler tarafından kullanılan sayaçları ve işle ilgili hata bilgilerini görüntüler. Bunu, aşağıdaki senaryolarda yararlı bilgiler teklif edebilir:

* Uzun süre çalışan izleme eşleme ilerleme durumunu görüntüleme, işler ve görevler azaltır.
* İşleme nasıl geliştirilmiş veya başarısız olma nedenine öğrenmek başarılı veya başarısız işlemler için geçmiş verilerini analiz etme.

## <a name="generate-a-dag"></a>Bir DAG oluştur

Tez görünüm Tez Altyapısı şu anda çalışıyor veya devre dışı bırakıldı kullanan önceden çalışan bir iş, yalnızca veri içeriyor. Hive sorguları basit, Tez kullanmadan çözümlenebilir. Daha karmaşık filtreleme yapın, gruplandırma, sıralama, birleştirme, vb. sorgular. Tez altyapısını kullanır.

Tez kullanan Hive sorgusu çalıştırmak için aşağıdaki adımları kullanın:

1. Bir web tarayıcısında gidin https://CLUSTERNAME.azurehdinsight.netburada **CLUSTERNAME** HDInsight kümenizin adıdır.

2. Sayfanın üst kısmındaki menüden **görünümleri** simgesi. Bu simge, kareler bir dizi gibi görünüyor. Görünen açılır menüden seçin **Hive görünümü**.

    ![Hive görünümünü seçme](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. Hive görünümünü yüklediğinde, aşağıdaki sorguyu sorgu düzenleyiciye yapıştırın ve ardından **yürütme**.

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    İş tamamlandıktan sonra görüntülenen bir çıktı görmeniz gerekir **sorgu işleminin sonuçları** bölümü. Sonuçlar aşağıdaki metne benzer olmalıdır:

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. Seçin **günlük** sekmesi. Aşağıdaki metne benzer bilgileri görebilirsiniz:

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    Kaydet **uygulama kimliği** değerini sonraki bölümde bu değeri kullanılır.

## <a name="use-the-tez-view"></a>Tez görünümünü kullanın

1. Sayfanın üst kısmındaki menüden **görünümleri** simgesi. Görünen açılır menüden seçin **Tez görünümü**.

    ![Tez görünümü seçme](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. Tez görünümü yüklediğinde, şu anda çalışmakta olan veya silinmiş hive sorgularının listesini küme üzerinde çalışan bakın.

    ![Tüm DAG'leri](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. Yalnızca bir giriş varsa, önceki bölümde çalıştırdığınız sorguyu içindir. Birden çok girişi varsa, sayfanın en üstündeki alanlara kullanarak arama yapabilirsiniz.

4. Seçin **sorgu kimliği** bir Hive sorgusu için. Sorgu hakkında bilgiler görüntülenir.

    ![DAG ayrıntıları](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. Bu sayfadaki sekmeleri, aşağıdaki bilgileri görüntülemek izin ver:

    * **Sorgu ayrıntıları**: Hive sorgusu hakkında ayrıntıları.
    * **Zaman Çizelgesi**: ne kadar sürdüğünü her aşaması hakkında bilgi.
    * **Yapılandırmaları**: Bu sorgu için kullanılan yapılandırma.

    Gelen __sorgu ayrıntıları__ bağlantıları hakkında bilgi bulmak için kullanabileceğiniz __uygulama__ veya __DAG__ bu sorgu için.
    
    * __Uygulama__ bağlantı YARN uygulama bu sorgu için ilgili bilgileri görüntüler. Buradan, YARN uygulama günlüklerine erişebilirsiniz.
    * __DAG__ bağlantı yönlendirilmiş Çevrimsiz grafik bu sorgu için ilgili bilgileri görüntüler. Buradan, DAG grafik gösterimi görüntüleyebilirsiniz. Ayrıca, DAG içindeki köşeler hakkında bilgi bulabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Tez görünümü kullanmak öğrendiniz, daha fazla bilgi edinin [HDInsight üzerinde Hive'ı kullanarak](hadoop/hdinsight-use-hive.md).

Daha ayrıntılı Tez üzerinde teknik bilgi için bkz. [Hortonworks Tez sayfanın](http://hortonworks.com/hadoop/tez/).

Ambari kullanarak HDInsight ile ilgili daha fazla bilgi için bkz: [yönetme HDInsight kümeleri Ambari Web kullanıcı arabirimini kullanarak](hdinsight-hadoop-manage-ambari.md)
