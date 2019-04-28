---
title: Azure HDInsight kümelerinizi izlemek için sorgu Azure izleme günlükleri
description: Bir HDInsight kümesinde çalışan işleri izlemek için Azure İzleyici günlüklerine üzerinde sorgular çalıştırın öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: cbaaed3fff99778bfab1feeacdab02bf8245a85a
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63761224"
---
# <a name="query-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>HDInsight kümelerinizi izlemek için sorgu Azure izleme günlükleri

Bazı temel senaryolar Azure HDInsight kümelerinizi izlemek için Azure İzleyici günlüklerine kullanma hakkında bilgi edinin:

* [HDInsight küme ölçümleri analiz](#analyze-hdinsight-cluster-metrics)
* [Belirli günlük iletilerini arayın](#search-for-specific-log-messages)
* [Olay uyarıları oluşturma](#create-alerts-for-tracking-events)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

* Azure İzleyici günlüklerine kullanmak için HDInsight kümesi yapılandırılmış ve gerekir izleme çözümleri çalışma alanına HDInsight kümeye özgü Azure İzleyici günlüklerine eklendi. Yönergeler için [kullanımı Azure İzleyici günlükleri HDInsight kümeleriyle](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="analyze-hdinsight-cluster-metrics"></a>HDInsight küme ölçümleri analiz

HDInsight kümenizin belirli ölçümleri arayın öğrenin.

1. Azure portalından, HDInsight kümenize ilişkili Log Analytics çalışma alanını açın.
2. Seçin **günlük araması** Döşe.
3. Azure İzleyici günlüklerine kullanın ve ardından seçmek için yapılandırılan tüm HDInsight kümeleri için kullanılabilen tüm ölçümler için tüm ölçümleri aramak için arama kutusuna aşağıdaki sorguyu yazın **ÇALIŞTIRMA**.

        search *

    ![Tüm ölçümleri arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "tüm ölçümler arama")

    Çıkış, gibi benzer:

    ![Tüm ölçümleri çıkış arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "tüm ölçümleri çıktı Ara")

5. Sol bölmeden altında **türü**, ayrıntılı olarak ele alacak ve ardından istediğiniz bir ölçüm seçin **Uygula**. Aşağıdaki ekran görüntüsü gösterildiği `metrics_resourcemanager_queue_root_default_CL` türü seçilidir.

    > [!NOTE]  
    > Seçmek için gerek duyabileceğiniz **[+] daha fazla** aradığınız ölçüm bulmak için düğme. Ayrıca, **Uygula** düğmesini görmek için aşağı kaydırmanız gerekir listesinin en altında olduğundan.

    Aşağıdaki ekran görüntüsünde vurgulanan kutusunda gösterilen bir metin kutusundaki sorguyu değiştiğine dikkat edin:

    ![Belirli ölçümleri Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "belirli ölçümleri arayın")

6. Bu belirli Ölçüm içinde daha ayrıntılı incelemek için. Örneğin, aşağıdaki sorguyu kullanarak küme adına göre kategorilere ayrılmış bir 10 dakika arayla kullanılan kaynakların ortalama göre mevcut çıkış daraltabilirsiniz:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)

7. Ortalama kullanılan kaynakların dayalı iyileştirme yerine aşağıdaki sorguyu en çok kaynak (yanı sıra 90'ıncı ve 95'lik dilim) karşılaştıklarını göre sonuçları daraltmak için 10 dakikalık penceresinde kullanabilirsiniz:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)

## <a name="search-for-specific-log-messages"></a>Belirli günlük iletilerini arayın

Belirli bir zaman penceresi sırasında hata iletilerini arayın öğrenin. Burada bir örnek üzerinde nasıl, hata iletisi, ulaşmak ilgilendiğiniz adımlardır. Bulmayı denediğiniz hataları aramak kullanılabilir olan herhangi bir özelliği kullanabilirsiniz.

1. Azure portalından, HDInsight kümenize ilişkili Log Analytics çalışma alanını açın.
2. Seçin **günlük araması** Döşe.
3. Aşağıdaki Azure İzleyici günlüklerine kullanmak üzere yapılandırılmış tüm HDInsight kümeleri için tüm hata iletileri için aranacak sorgu ve ardından türü **ÇALIŞTIRMA**. 

         search "Error"

    Aşağıdaki çıktı gibi bir çıktı görmeniz gerekir:

    ![Tüm hataları çıkış arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "tüm hataları çıktı Ara")

4. Sol bölmeden altında **türü** kategori ayrıntılı olarak incelemek istediğiniz bir hata türünü seçin ve ardından **Uygula**.  Sonuçları yalnızca seçtiğiniz türü hatayı göstermek için iyileştirilmektedir dikkat edin.
5. Bu belirli hata listesine sol bölmede Seçenekler'i kullanarak derinlemesine. Örneğin:

    - Belirli bir alt düğüm hata iletilerini görmek için:

        ![Belirli hatalar çıkış Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "belirli hatalar çıkış arayın")

    - Belirli bir süre sonunda bir hata oluştu görmek için:

        ![Belirli hatalar çıkış Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "belirli hatalar çıkış arayın")

6. Belirli bir hatayı görmek için. Seçebileceğiniz **[+] daha fazla Göster** gerçek hata ileti aramak için.

    ![Belirli hatalar çıkış Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "belirli hatalar çıkış arayın")

## <a name="create-alerts-for-tracking-events"></a>Olayları izleme uyarıları oluşturma

Bir uyarı oluşturmak için ilk adımı, uyarıyı tetikleyen temel alarak sorguyu ulaşırsınız sağlamaktır. Bir uyarı oluşturmak istediğiniz herhangi bir sorgu kullanabilirsiniz.

1. Azure portalından, HDInsight kümenize ilişkili Log Analytics çalışma alanını açın.
2. Seçin **günlük araması** Döşe.
3. İstediğiniz bir uyarı oluşturmak ve ardından seçmek aşağıdaki sorguyu çalıştırın **ÇALIŞTIRMA**.

        metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0

    Sorgu, HDInsight kümelerinde çalışan başarısız uygulamaların bir listesini sağlar.

4. Seçin **yeni uyarı kuralı** sayfanın üstündeki.

    ![Bir uyarı oluşturmak için Enter sorguyu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Enter sorgu bir uyarı oluşturmak için")

5. İçinde **oluşturma kuralı** penceresi, sorgu ve uyarı oluşturma ve ardından diğer ayrıntıları girin **uyarı kuralı oluştur**.

    ![Bir uyarı oluşturmak için Enter sorguyu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Enter sorgu bir uyarı oluşturmak için")

Düzenleyebilir veya var olan bir uyarıyı silmek için:

1. Log Analytics çalışma alanını Azure portalından açın.
2. Sol menüden **uyarı**.
3. Düzenlemek veya silmek için istediğiniz uyarıyı seçin.
4. Aşağıdaki seçenekleriniz vardır: **Kaydet**, **at**, **devre dışı**, ve **Sil**.

    ![HDInsight Azure İzleyici günlüklerine uyarısını silme Düzenle](media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png)

Daha fazla bilgi için [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak ölçüm Uyarıları yönetme](../azure-monitor/platform/alerts-metric.md).

## <a name="see-also"></a>Ayrıca bkz.

* [Azure İzleyici'de Görünüm Tasarımcısı kullanarak özel görünümlerini oluşturma](../azure-monitor/platform/view-designer.md)
* [Oluşturun, görüntüleyin ve ölçüm uyarıları Azure İzleyicisi'ni kullanarak yönetme](../azure-monitor/platform/alerts-metric.md)
