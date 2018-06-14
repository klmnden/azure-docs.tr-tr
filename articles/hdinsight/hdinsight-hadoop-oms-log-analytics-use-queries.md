---
title: Sorgu Azure Hdınsight kümeleri izlemek için Azure günlük analizi | Microsoft Docs
description: Bir Hdınsight kümesinde çalışan işleri izlemek için Azure günlük analizi sorguları çalıştırmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: 61467d702f3123085fd7e067a8d56c30331c5bc6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31401110"
---
# <a name="query-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Hdınsight kümeleri izlemek için sorgu Azure günlük analizi

Bazı temel senaryoları Azure günlük analizi Azure Hdınsight kümeleri izlemek için nasıl kullanılacağı hakkında bilgi edinin:

* [Hdınsight küme ölçümleri analiz edin](#analyze-hdinsight-cluster-metrics)
* [Özel günlük iletilerini arayın](#search-for-specific-log-messages)
* [Olay uyarı oluşturma](#create-alerts-for-tracking-events)

## <a name="prerequisites"></a>Önkoşullar

* Hdınsight kümesi Azure günlük analizi kullanmak üzere yapılandırmış olmanız gerekir. Yönergeler için bkz: [kullanım Azure günlük analizi Hdınsight kümeleri ile](hdinsight-hadoop-oms-log-analytics-tutorial.md).

* Hdınsight kümesi-özel yönetim çözümleri eklenmesi gerekir [günlük analizi](../operations-management-suite/operations-management-suite-overview.md) açıklandığı gibi çalışma [ekleme Hdınsight küme yönetim çözümleri için günlük analizi](hdinsight-hadoop-oms-log-analytics-management-solutions.md).

## <a name="analyze-hdinsight-cluster-metrics"></a>Hdınsight küme ölçümleri analiz edin

Hdınsight kümenizle ilgili belirli ölçümleri arayın öğrenin.

1. Azure portalında Azure günlük analizi ile ilişkili bir Hdınsight kümesi'ni açın.
2. Tıklatın **izleme**ve ardından **açık OMS Pano**.

    ![Açık OMS Pano](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "açık OMS Panosu")

2. Tıklatın **günlük arama** sol menüde.

    ![Açık günlük arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "açık günlük arama")

3. Aşağıdaki sorgu tüm ölçümleri Azure günlük analizi kullanın ve ENTER tuşuna basın yapılandırılmış tüm Hdınsight kümeleri için tüm kullanılabilir ölçümler için aramak için arama kutusuna yazın **ENTER**.

        `search *` 

    ![Tüm ölçümleri arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "tüm ölçümleri arama")

    Çıktı neye benzediğini gösteren:

    ![Tüm ölçümleri çıkış arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "tüm ölçümleri çıkış arama")

5. Sol bölmeden altında **türü**, derin içine sorgulaması ve ardından istediğiniz ölçümü seçin **Uygula**. Aşağıdaki ekran görüntüsü gösterildiği `metrics_resourcemanager_queue_root_default_CL` türü seçilidir. 

    > [!NOTE]
    > ' İ tıklatmanız gerekir **[+] daha fazla** aradığınız ölçüm bulmak için düğmesini. Ayrıca, **Uygula** düğmesini görmek için aşağı kaydırmanız gerekir listenin en altında olduğundan.
    > 
    >    

    Sorgu metin kutusuna bir aşağıdaki ekran görüntüsünde vurgulanan kutusunda gösterilen değiştiğine dikkat edin:

    ![Belirli ölçümleri Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "belirli ölçümleri arayın")

6. Bu belirli ölçüm derinliklerine için. Örneğin, aşağıdaki sorguyu kullanarak küme adıyla kategorilere ayrılmış bir 10 dakika arayla kullanılan kaynakların ortalama göre varolan çıkış görüntüleneceğini belirtebilirsiniz:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)

7. Kullanılan kaynaklar ortalamasını dayalı daraltmayı yerine, aşağıdaki sorguyu maksimum kaynaklar (90 ve 95 yüzdebirlik yanı sıra) kullanılan zamana göre sonuçları daraltmak için 10 dakika penceresinde kullanabilirsiniz:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)

## <a name="search-for-specific-log-messages"></a>Özel günlük iletilerini arayın

Belirli bir zaman penceresi sırasında hata iletileri ara öğrenin. Burada nasıl, hata ileti, ulaşması üzerinde tek bir örnek ilgilendiğiniz adımlardır. Bulmayı denediğiniz hataları aramak kullanılabilir herhangi bir özelliği kullanabilirsiniz.

1. Azure portalında Azure günlük analizi ile ilişkili bir Hdınsight kümesi'ni açın.
2. Tıklatın **izleme**ve ardından **açık OMS Pano**.

    ![Açık OMS Pano](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "açık OMS Panosu")

2. OMS portalında, başlangıç ekranından tıklatın **günlük arama**.

    ![Açık günlük arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "açık günlük arama")

3. Aşağıdaki Azure günlük analizi kullanmak üzere yapılandırılmış tüm Hdınsight kümeleri için tüm hata iletilerini aramak için sorgu ve tuşuna basarak türü **ENTER**. 

         search "Error"

    Aşağıdaki çıkış benzer bir çıktı göreceksiniz:

    ![Tüm hataları çıkış arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "tüm hataları çıkış arama")

5. Sol bölmeden altında **türü** kategori, derin içine derinliklerine istediğiniz bir hata türünü seçin ve ardından **Uygula**.  Sonuçları yalnızca seçtiğiniz türü hata göstermek için iyileştirilmiştir dikkat edin.
7. Bu özel hata listesine sol bölmede kullanılabilir seçenekleri kullanarak derinliklerine. Örneğin, 

    - Belirli alt düğümünden hata iletilerini görmek için:

        ![Belirli hataları çıkış Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "belirli hataları çıkış Ara")

    - Belirli bir süre sonunda bir hata oluştu görmek için:

        ![Belirli hataları çıkış Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "belirli hataları çıkış Ara")

9. Belirli bir hatayı görmek için. Tıklayabilirsiniz **[+] daha fazla Göster** gerçek hata ileti aramak için.

    ![Belirli hataları çıkış Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "belirli hataları çıkış Ara")

## <a name="create-alerts-for-tracking-events"></a>Olayları izleme için uyarı oluşturma

Bir uyarı oluşturmak için ilk uyarıyı tetikleyen üzerinde temel sorgusu gelmesi adımdır. Kolaylık olması için Hdınsight kümelerinde çalışan başarısız uygulamaların bir listesini sağlar aşağıdaki sorguyu kullanalım.

    metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0

Bir uyarı oluşturmak istediğiniz herhangi bir sorgu kullanabilirsiniz.

1. Azure portalında Azure günlük analizi ile ilişkili bir Hdınsight kümesi'ni açın.
2. Tıklatın **izleme**ve ardından **açık OMS Pano**.

    ![Açık OMS Pano](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "açık OMS Panosu")

2. OMS portalında, başlangıç ekranından tıklatın **günlük arama**.

    ![Açık günlük arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "açık günlük arama")

3. Tuşuna basın ve bir uyarı oluşturmak istediğiniz aşağıdaki sorguyu çalıştırın **ENTER**.

        metrics_resourcemanager_queue_root-default-CL | where AppsFailed_d > 0

4. Tıklatın **uyarı** sayfanın üst kısmında.

    ![Bir uyarı oluşturmak için Enter sorgu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Enter sorgu bir uyarı oluşturmak için")

4. İçinde **uyarı kuralı Ekle** penceresinde, sorgu ve bir uyarı oluşturabilir ve ardından diğer ayrıntılarını girin **kaydetmek**.

    ![Bir uyarı oluşturmak için Enter sorgu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Enter sorgu bir uyarı oluşturmak için")

    Ekran çıktı uyarı sorgu geri döndüğünde bir e-posta bildirimi göndermek için yapılandırmasını gösterir.

5. Ayrıca, düzenleyin veya varolan bir uyarı silebilirsiniz. Bunu, OMS portalında herhangi bir sayfadan yapmak için tıklatın **ayarları** simgesi.

    ![Bir uyarı oluşturmak için Enter sorgu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png "Enter sorgu bir uyarı oluşturmak için")

6. Gelen **ayarları** sayfasında, **uyarıları** oluşturduğunuz uyarıları görmek için. Ayrıca etkinleştirebilir veya bir uyarıyı devre dışı bırakmak düzenlemek veya silin. Daha fazla bilgi için bkz: [uyarı kurallarında günlük analizi çalışma](../log-analytics/log-analytics-alerts-creating.md).

## <a name="see-also"></a>Ayrıca bkz.

* [Günlük analizi ile çalışma](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Günlük analizi uyarı kuralları oluşturma](../log-analytics/log-analytics-alerts-creating.md)
