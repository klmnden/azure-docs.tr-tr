---
title: "İzleyici ve get Öngörüler mantıksal uygulamanızı hakkında çalıştıran OMS - Azure mantıksal uygulamaları kullanma | Microsoft Docs"
description: "Mantıksal uygulama çalışmalarınız sorun giderme ve tanılama için Öngörüler ve daha zengin hata ayıklama ayrıntılarını almak için günlük analizi ve Operations Management Suite (OMS) ile izleme"
author: divyaswarnkar
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/9/2017
ms.author: LADocs; divswa
ms.openlocfilehash: 2f9f27dc74348909b89941c2bb17ccdf610dba33
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a>İzleyici ve get Öngörüler mantıksal uygulama hakkında Operations Management Suite (OMS) ve günlük analizi ile çalışır

İzleme ve daha zengin hata ayıklama bilgileri almak için bir mantıksal uygulama oluşturduğunuzda, aynı anda günlük analizi kapatabilirsiniz. Günlük analizi günlüğe kaydetme ve izleme mantığı uygulamanız için tanılama Operations Management Suite (OMS) portalı üzerinden çalışan sağlar. Logic Apps yönetim çözümü için OMS eklediğinizde, logic app çalıştırır ve durumu, yürütme süresi, yeniden gönderme durumu ve bağıntı kimlikleri gibi belirli Ayrıntılar için toplanan durumunu alın.

Bu konu, günlük analizi kapatabilir veya çalıştırmak mantığı uygulamanız için çalışma zamanı olayları ve veri görüntüleyebilmeniz için Logic Apps yönetimi çözümü içinde OMS yükleme gösterilmektedir.

 > [!TIP]
 > Mevcut mantıksal uygulamalarınızı izlemek için aşağıdaki adımları izleyin [tanılama günlük özelliğini açar ve mantığı uygulama çalışma zamanı veri göndermek için OMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Gereksinimler

Başlamadan önce OMS çalışma alanınızın olması gerekir. Bilgi [bir OMS çalışma alanı oluşturmak nasıl](../log-analytics/log-analytics-get-started.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Logic apps oluştururken tanılama günlüğünü etkinleştirme

1. İçinde [Azure portal](https://portal.azure.com), bir mantıksal uygulama oluşturma. Seçin **kaynak oluşturma** > **Kurumsal tümleştirme** > **mantıksal uygulama**.

   ![Mantıksal uygulama oluşturma](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. İçinde **oluşturma mantıksal uygulama** sayfasında, gösterildiği gibi bu görevleri gerçekleştirin:

   1. Mantıksal uygulamanız için bir ad ve Azure aboneliğinizi seçin. 
   2. Bir Azure kaynak grubu seçin veya oluşturun.
   3. Ayarlama **oturum Analytics** için **üzerinde**. 
   Mantıksal uygulamanız için veri çalıştıran göndermek istediğiniz OMS çalışma alanını seçin. 
   4. Hazır olduğunuzda, seçin **panoya Sabitle** > **oluşturma**.

      ![Mantıksal uygulama oluşturma](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Bu adımı tamamladıktan sonra artık mantıksal uygulamanızı Azure oluşturur, OMS çalışma alanıyla ilişkilendirilmiş. 
      Ayrıca, bu adım OMS çalışma alanınızda Logic Apps yönetimi çözümü de otomatik olarak yükler.

3. Mantığınızı görüntülemek için uygulamanın OMS içinde çalıştığı [bu adımlarla devam](#view-logic-app-runs-oms).

## <a name="install-the-logic-apps-management-solution-in-oms"></a>Logic Apps yönetimi çözümü içinde OMS yükleyin

Mantıksal uygulamanızı oluşturduğunuzda günlük analizi zaten etkinleştirdiyseniz, bu adımı atlayın. Logic Apps yönetimi çözümü içinde OMS yüklenmiş zaten var.

1. İçinde [Azure portal](https://portal.azure.com), seçin **daha Hizmetleri**. Filtre olarak "günlük analizi" arayın ve seçin **günlük analizi** gösterildiği gibi:

   !["Günlük analizi" seçin](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. Altında **günlük analizi**, bulma ve OMS çalışma alanınızı seçin. 

   ![OMS çalışma alanınızı seçin](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. Altında **Yönetim**, seçin **OMS portalı**.

   !["OMS portalı" seçin](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. Yükseltme başlık görünürse, OMS sayfanız, OMS çalışma yükseltmeniz başlığı seçin. Ardından **Çözümleri Galerisi**.

   !["Çözümleri Galerisi" seçin](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. Altında **tüm çözümleri**, bulmak ve seçmek için döşeme **Logic Apps Yönetim** çözümü.

   !["Logic Apps Yönetimi" seçin](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. OMS çalışma alanınızda çözümü yüklemek için tercih **Ekle**.

   !["" Logic Apps yönetimi için"Ekle" yi seçin](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a>OMS çalışma alanınızda mantıksal uygulamanızı çalıştıran görünümü

1. Sayısı ve logic app çalışmalarınız durumunu görüntülemek için OMS çalışma alanınız için genel bakış sayfasına gidin. Ayrıntıları gözden **Logic Apps Yönetim** döşeme.

   ![Mantığı çalıştırmak uygulama sayısı ve durumunu gösteren genel bakış kutucuğu](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > Bu yükseltme başlık yerine Logic Apps yönetim döşeme görünürse, böylece OMS çalışma alanınızı yükseltmeniz başlığını seçin.
  
   > ![Yükseltme "OMS çalışma"](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. Bir Özet mantığı uygulama çalışmalarınız hakkında daha fazla ayrıntı görüntülemek için seçin **Logic Apps Yönetim** döşeme.

   Burada, logic app çalışmalarınız adına veya yürütme durumu göre gruplandırılır. Ayrıca, Eylemler ve Tetikleyicileri mantığı uygulama çalışmaları için hata ayrıntılarını görebilirsiniz.

   ![Mantıksal uygulamanız için Özet durum çalıştırır](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. Belirli mantıksal uygulama ya da durum için tüm çalıştırmalarını görüntülemek için bir mantıksal uygulama veya bir durum için satır seçin.

   Belirli mantıksal uygulama için tüm çalıştırmalarını gösteren bir örnek aşağıda verilmiştir:

   ![Bir mantıksal uygulama veya bir durum görünümü çalıştırır](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Bu sayfadaki iki Gelişmiş Seçenekler şunlardır:
   * **Özellikleri izlenir:** bu sütun için mantıksal uygulama eylemleri göre gruplandırılmış izlenen özellikleri gösterir. İzlenen özellikleri görüntülemek için seçin **Görünüm**. Sütun filtresini kullanarak izlenen özellikleri arama yapabilirsiniz.
   
     ![Bir mantıksal uygulama için izlenen özellikleri görüntüleyin](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Yeni eklenen tüm izlenen özelliklerini ilk kez görünmesi 10-15 dakika sürebilir. Bilgi [mantıksal uygulamanızı izlenen Özellikler ekleme](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Yeniden gönderin:** başarılı, başarısız bir veya daha fazla mantıksal uygulama çalıştırır yeniden gönderebilirsiniz veya çalışmakta olan. Yeniden gönderin ve seçmek istediğiniz çalışmaları için onay kutularını seçin **yeniden**. 

     ![Mantıksal uygulama çalıştırır yeniden gönderin](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

4. Bu sonuçları filtrelemek için istemci tarafı ve sunucu tarafı filtreleme gerçekleştirebilirsiniz.

   * İstemci tarafı filtresi: her sütun için istediğiniz filtreleri seçin. 
   İşte bazı örnekler:

     ![Örnek sütun filtreleri](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * Sunucu tarafı filtresi: belirli bir zaman penceresinin seçin ya da görünür çalıştırmalarının sayısını sınırlamak için sayfanın en üstünde kapsam denetimini kullanın. 
   Varsayılan olarak, aynı anda yalnızca 1.000 kayıtları görünür. 
   
     ![Değişiklik zaman penceresi](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. Tüm Eylemler ve bunların belirli bir çalışma ayrıntılarını görüntülemek için bir mantıksal uygulama çalıştırmak için bir satır seçin.

   Belirli mantıksal uygulama çalıştırmak için tüm eylemleri gösteren bir örnek aşağıda verilmiştir:

   ![Bir mantıksal uygulama için Görünüm eylemleri çalıştır](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
6. Sorgu sonuçları arkasında görüntülemek veya tüm sonuçları görmek için herhangi bir sonuç sayfasında seçin **bkz tüm**, günlük arama sayfası açılır.
   
   ![Tüm sonuçları sayfalarına bakın](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   Günlük arama sayfasında,
   * Bir tablodaki sorgu sonuçlarını görüntülemek için seçin **tablo**.
   * Sorguyu değiştirmek için arama çubuğunda sorgu dizesi düzenleyebilirsiniz. 
   Daha iyi bir deneyim için seçin **Advanced Analytics**.

     ![Eylemler ve Çalıştır bir mantıksal uygulama ayrıntılarını görüntüleyin](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     Burada Azure günlük analizi sayfasında sorguları güncelleştirebilirsiniz ve tablodan sonuçları görüntüleyin. 
     Bu sorgu kullanır [Kusto sorgu dili](https://docs.loganalytics.io/docs/Language-Reference), farklı sonuçlar görüntülemek istiyorsanız, düzenleyebilirsiniz. 

     ![Azure günlük analizi - sorgu görünümü](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Sonraki adımlar

* [B2B iletilerini izleme](../logic-apps/logic-apps-monitor-b2b-message.md)

