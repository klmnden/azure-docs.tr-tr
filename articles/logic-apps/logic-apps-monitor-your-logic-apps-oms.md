---
title: İzleyici ve get Öngörüler mantıksal uygulamanızı hakkında çalıştıran günlük analizi - Azure mantıksal uygulamaları kullanma | Microsoft Docs
description: Mantıksal uygulama çalışmalarınız sorun giderme ve tanılama için Öngörüler ve daha zengin hata ayıklama ayrıntılarını almak için günlük analizi ile izleme
author: divyaswarnkar
manager: anneta
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/9/2017
ms.author: LADocs; divswa
ms.openlocfilehash: d484aaf7d7582bd474d7437a7a62f41880690dbc
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-log-analytics"></a>İzleme ve günlük analizi ile mantığı uygulama çalışır hakkında Öngörüler alın

İzleme ve daha zengin hata ayıklama bilgileri almak için bir mantıksal uygulama oluşturduğunuzda, aynı anda günlük analizi kapatabilirsiniz. Günlük analizi günlüğe kaydetme ve izleme mantığı uygulamanız için tanılama Azure portalı üzerinden çalışan sağlar. Logic Apps yönetim çözümü eklediğinizde, logic app çalıştırır ve durumu, yürütme süresi, yeniden gönderme durumu ve bağıntı kimlikleri gibi belirli Ayrıntılar için toplanan durumunu alın.

Bu konu, günlük analizi çalışma olaylarını görüntüleyebilir ve veri mantığı uygulamanız için çalıştırmaları için devre dışı bırakma gösterir.

 > [!TIP]
 > Mevcut mantıksal uygulamalarınızı izlemek için aşağıdaki adımları izleyin [tanılama günlük özelliğini açar ve günlük analizi için mantığı uygulama çalışma zamanı verileri Gönder](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Gereksinimler

Başlamadan önce bir günlük analizi çalışma alanınızın olması gerekir. Bilgi [günlük analizi çalışma alanı oluşturmak nasıl](../log-analytics/log-analytics-quick-create-workspace.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Logic apps oluştururken tanılama günlüğünü etkinleştirme

1. İçinde [Azure portal](https://portal.azure.com), bir mantıksal uygulama oluşturma. Seçin **kaynak oluşturma** > **Kurumsal tümleştirme** > **mantıksal uygulama**.

   ![Mantıksal uygulama oluşturma](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. İçinde **oluşturma mantıksal uygulama** sayfasında, gösterildiği gibi bu görevleri gerçekleştirin:

   1. Mantıksal uygulamanız için bir ad ve Azure aboneliğinizi seçin. 
   2. Bir Azure kaynak grubu seçin veya oluşturun.
   3. Ayarlama **oturum Analytics** için **üzerinde**. 
   Mantıksal uygulamanız için veri çalıştıran göndermek istediğiniz günlük analizi çalışma alanını seçin. 
   4. Hazır olduğunuzda, seçin **panoya Sabitle** > **oluşturma**.

      ![Mantıksal uygulama oluşturma](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Bu adımı tamamladıktan sonra artık mantıksal uygulamanızı Azure oluşturduğu günlük analizi çalışma alanıyla ilişkilendirilmiş. 
      Ayrıca, bu adım çalışma alanınızda Logic Apps yönetimi çözümü de otomatik olarak yükler.

3. Çalışan mantıksal uygulamanızı görüntülemek için [bu adımlarla devam](#view-logic-app-runs-oms).

## <a name="install-the-logic-apps-management-solution"></a>Logic Apps yönetim çözümü

Mantıksal uygulamanızı oluşturduğunuzda günlük analizi zaten etkinleştirdiyseniz, bu adımı atlayın. Zaten yüklü Logic Apps yönetim çözümü vardır.

1. İçinde [Azure portal](https://portal.azure.com), seçin **daha Hizmetleri**. Filtre olarak "günlük analizi" arayın ve seçin **günlük analizi** gösterildiği gibi:

   !["Günlük analizi" seçin](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. Altında **günlük analizi**, bulma ve günlük analizi çalışma alanınızı seçin. 

   ![Günlük analizi çalışma alanınızı seçin](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. Altında **Yönetim**, seçin **OMS portalı**.

   !["OMS portalı" seçin](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. Altında **tüm çözümleri**, bulmak ve seçmek için döşeme **Logic Apps Yönetim** çözümü.

   !["Logic Apps Yönetimi" seçin](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

5. Günlük analizi çalışma alanınızda çözümü yüklemek için tercih **Ekle**.

   !["" Logic Apps yönetimi için"Ekle" yi seçin](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-log-analytics-workspace"></a>Günlük analizi çalışma alanınızda mantıksal uygulamanızı çalıştıran görünümü

1. Sayısı ve logic app çalışmalarınız durumunu görüntülemek için günlük analizi çalışma alanınız için genel bakış sayfasına gidin. Ayrıntıları gözden **Logic Apps Yönetim** döşeme.

   ![Mantığı çalıştırmak uygulama sayısı ve durumunu gösteren genel bakış kutucuğu](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

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

