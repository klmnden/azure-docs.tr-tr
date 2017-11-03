---
title: "Sorgu Azure Hdınsight kümeleri izlemek için Azure günlük analizi | Microsoft Docs"
description: "Bir Hdınsight kümesinde çalışan işleri izlemek için Azure günlük analizi sorguları çalıştırmayı öğrenin."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: nitinme
ms.openlocfilehash: 8fe91bed69a1c06367346041d8caba4aaee4c82a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="query-azure-log-analytics-to-monitor-hdinsight-clusters-preview"></a>Hdınsight kümeleri (Önizleme) izlemek için sorgu Azure günlük analizi

Bu makalede, Azure günlük analizi Azure Hdınsight kümeleri ile kullanma hakkında bazı senaryolarına bakın. Üç yaygın senaryolar şunlardır:

* Hdınsight küme ölçümlerini OMS Çözümle
* Hdınsight kümeleri için belirli günlük iletileri ara
* Kümelerde gerçekleşen olayların dayanan uyarıları oluşturma

## <a name="prerequisites"></a>Ön koşullar

* Hdınsight kümesi Azure günlük analizi kullanmak üzere yapılandırmış olmanız gerekir. Yönergeler için bkz: [kullanım Azure günlük analizi Hdınsight kümeleri ile](hdinsight-hadoop-oms-log-analytics-tutorial.md).

* Hdınsight kümesi-özel yönetim çözümleri için OMS çalışma alanına açıklandığı gibi eklediğiniz gerekir [ekleme Hdınsight küme yönetim çözümleri için günlük analizi](hdinsight-hadoop-oms-log-analytics-management-solutions.md).

## <a name="analyze-hdinsight-cluster-metrics-in-oms"></a>Hdınsight küme ölçümlerini OMS Çözümle

Bu bölümde, biz Hdınsight kümenize belirli ölçümleri aramak için izleyeceğiniz adımlarda size yol.

1. OMS panosunu açın. Azure portalında Azure günlük analizi ile ilişkili Hdınsight küme dikey penceresini açın, izleme sekmesinin ve tıklatın **açmak OMS Pano**.

    ![Açık OMS Pano](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "açık OMS Panosu")

2. OMS panodan giriş ekranı tıklatın **günlük arama**.

    ![Açık günlük arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "açık günlük arama")

3. Günlük arama penceresinde içinde **burada başlangıç arama** metin kutusunda, `*` tüm ölçümleri Azure günlük analizi kullanmak üzere yapılandırılmış tüm Hdınsight kümeleri için tüm kullanılabilir ölçümler için aramak için. ENTER tuşuna basın.

    ![Tüm ölçümleri arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "tüm ölçümleri arama")

4. Aşağıdakine benzer bir çıktı görmeniz gerekir.

    ![Tüm ölçümleri çıkış arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "tüm ölçümleri çıkış arama")

5. Sol bölmeden altında **türü** kategorisi, derin içine derinliklerine istediğiniz ölçüm bir arama. Bu öğretici için şimdi çekme `metrics_resourcemanager_queue_root_default_CL`. Ölçüm için karşılık gelen onay kutusunu seçin ve ardından **Uygula**.

    > [!NOTE]
    > Tıklatın gerekebilir **[+] daha fazla** aradığınız ölçüm bulmak için düğmesini. Ayrıca, **Uygula** düğmesini görmek için aşağı kaydırmanız gerekir listenin en altında olduğundan.
    > 
    >    
    Metin kutusuna sorgu şimdi bir aşağıdaki ekran görüntüsünde vurgulanan kutusunda gösterilen değiştiğine dikkat edin:

    ![Belirli ölçümleri Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "belirli ölçümleri arayın")

6. Artık bu belirli ölçüm derinliklerine. Örneğin, küme adı tarafından kategorilere ayrılmış bir 10 dakika arayla kullanılan kaynakların ortalama göre varolan çıkış şimdi geliştirebilirsiniz. Aşağıdaki sorgu sorgu metin kutusuna yazın.

        * (Type=metrics_resourcemanager_queue_root_default_CL) | measure avg(UsedAMResourceMB_d) by ClusterName_s interval 10minute

    ![Belirli ölçümleri Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-more-specific-metrics.png "belirli ölçümleri arayın")

7. Kullanılan kaynaklar ortalamasını dayalı daraltmayı yerine, aşağıdaki sorguyu maksimum kaynaklar (90 ve 95 yüzdebirlik yanı sıra) kullanılan zamana göre sonuçları daraltmak için 10 dakika penceresinde kullanabilirsiniz.

        * (Type=metrics_resourcemanager_queue_root_default_CL) | measure max(UsedAMResourceMB_d) , pct95(UsedAMResourceMB_d), pct90(UsedAMResourceMB_d)  by ClusterName_s interval 10minute

    ![Belirli ölçümleri Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-more-specific-metrics-1.png "belirli ölçümleri arayın")

## <a name="search-for-specific-log-messages-in-hdinsight-clusters"></a>Hdınsight kümelerinde belirli günlük iletileri ara

Bu bölümde, belirli bir zaman penceresi sırasında hata iletileri aramak için adımları size rehberlik eder. Burada nasıl, hata ileti, ulaşması üzerinde tek bir örnek ilgilendiğiniz adımlardır. Bulmayı denediğiniz hataları aramak kullanılabilir herhangi bir özelliği kullanabilirsiniz.

1. OMS panosunu açın. Azure portalında Azure günlük analizi ile ilişkili Hdınsight küme dikey penceresini açın, izleme sekmesinin ve tıklatın **açmak OMS Pano**.

    ![Açık OMS Pano](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "açık OMS Panosu")

2. OMS panodan giriş ekranı tıklatın **günlük arama**.

    ![Açık günlük arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "açık günlük arama")

3. Günlük arama penceresinde içinde **burada başlangıç arama** metin kutusunda, `"Error"` (tırnak işaretleriyle) Azure günlük analizi kullanmak üzere yapılandırılmış tüm Hdınsight kümeleri için tüm hata iletilerini aramak için. ENTER tuşuna basın.

    ![Tüm hataları arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors.png "tüm hataları arama")

4. Aşağıdakine benzer bir çıktı görmeniz gerekir.

    ![Tüm hataları çıkış arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "tüm hataları çıkış arama")

5. Sol bölmeden altında **türü** kategorisi, derin içine derinliklerine istediğiniz bir hata türünü arayın. Bu öğretici için şimdi çekme `log_sparkappsexecutors_CL`. Ölçüm için karşılık gelen onay kutusunu seçin ve ardından **Uygula**.

    ![Belirli hataları Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error.png "belirli hata arayın")

        
6. Metin kutusuna sorgu şimdi biri aşağıda vurgulanan kutu ve sonuçları gösterilen değişikliklerini bildirimi yalnızca seçtiğiniz türü hata göstermek için Gelişmiş.

    ![Belirli hataları çıkış Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-output.png "belirli hataları çıkış Ara")

7. Artık bu belirli hata listesine sol bölmede kullanılabilir seçenekleri kullanarak derinliklerine. Örneğin, yalnızca hata iletileri sırasında belirli alt düğüm aramak için sorgu geliştirebilirsiniz.

    ![Belirli hataları çıkış Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "belirli hataları çıkış Ara")

8. Daha fazla, sol bölmeden ilgili zaman değeri seçerek hatanın oluştuğu düşünme süresi üzerinde bölge.

    ![Belirli hataları çıkış Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "belirli hataları çıkış Ara")

9. Artık, aradığınız belirli hata aşağıya doğru bulunur. Tıklayabilirsiniz **[+] daha fazla Göster** gerçek hata ileti aramak için.

    ![Belirli hataları çıkış Ara](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "belirli hataları çıkış Ara")

## <a name="create-alerts-to-track-events"></a>Olayları izlemek için uyarı oluşturma

Bir uyarı oluşturmak için ilk uyarıyı tetikleyen üzerinde temel sorgusu gelmesi adımdır. Kolaylık olması için Hdınsight kümelerinde çalışan başarısız uygulamaların bir listesini sağlar aşağıdaki sorguyu kullanalım.

    * (Tür metrics_resourcemanager_queue_root_default_CL =) AppsFailed_d > 0 

Bir uyarı oluşturmak istediğiniz herhangi bir sorgu kullanabilirsiniz.

1. OMS panosunu açın. Azure portalında Azure günlük analizi ile ilişkili Hdınsight küme dikey penceresini açın, izleme sekmesinin ve tıklatın **açmak OMS Pano**.

    ![Açık OMS Pano](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "açık OMS Panosu")

2. OMS panodan giriş ekranı tıklatın **günlük arama**.

    ![Açık günlük arama](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "açık günlük arama")

3. Günlük arama penceresinde içinde **burada başlangıç arama** metin kutusunda, istediğiniz bir uyarı oluşturabilir, ENTER tuşuna basın ve ardından sorguyu yapıştırın **uyarı** düğmesi.

    ![Bir uyarı oluşturmak için Enter sorgu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Enter sorgu bir uyarı oluşturmak için")

4. İçinde **uyarı kuralı Ekle** penceresinde, sorgu ve bir uyarı oluşturabilir ve ardından diğer ayrıntılarını girin **kaydetmek**.

    ![Bir uyarı oluşturmak için Enter sorgu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Enter sorgu bir uyarı oluşturmak için")

    Uyarı sorgusu bir çıktı alıyorsa bu ekran görüntüsünde, biz yalnızca bir e-posta bildirimi gönder.

5. Ayrıca, düzenleyin veya varolan bir uyarı silebilirsiniz. Bunu, OMS portalında herhangi bir sayfadan yapmak için tıklatın **ayarları** simgesi.

    ![Bir uyarı oluşturmak için Enter sorgu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png "Enter sorgu bir uyarı oluşturmak için")

6. Gelen **ayarları** sayfasında, **uyarıları** oluşturduğunuz uyarıları görmek için. Ayrıca etkinleştirebilir veya bir uyarıyı devre dışı bırakmak düzenlemek veya silin. Daha fazla bilgi için bkz: [uyarı kurallarında günlük analizi çalışma](../log-analytics/log-analytics-alerts-creating.md).

## <a name="see-also"></a>Ayrıca bkz.

* [OMS günlük analizi ile çalışma](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Günlük analizi uyarı kuralları oluşturma](../log-analytics/log-analytics-alerts-creating.md)
