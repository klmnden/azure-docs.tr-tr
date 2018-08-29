---
title: İzleyici mantıksal uygulama çalıştırmaları Log Analytics - Azure Logic Apps | Microsoft Docs
description: Insights ve veri mantıksal uygulama çalıştırmalarınızı Log Analytics ile ilgili sorun giderme ve tanılama için hata ayıklama
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/19/2018
ms.openlocfilehash: 1aa55728b222c2838026cf5b06175736c5c84194
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123299"
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-log-analytics"></a>İzleyin ve mantıksal uygulama çalıştırmaları Log Analytics ile ilgili Öngörüler edinin

İzleme ve daha zengin hata ayıklama bilgileri almak için bir mantıksal uygulama oluşturduğunuzda, aynı anda Log Analytics temelinde kapatabilirsiniz. Log Analytics, Azure portalı üzerinden mantıksal uygulamanız için izleme ve günlüğe kaydetme tanılama çalıştıran sağlar. Logic Apps yönetim çözümü eklediğinizde, mantıksal uygulama çalıştırmaları ve durumunu, yürütme süresi, yeniden gönderme durumu ve bağıntı kimlikleri gibi belirli Ayrıntılar için toplanan durumunu alın.

Bu makalede, çalışma zamanı olayları görüntüleyebilir ve mantıksal uygulamanız için verileri çalıştırmak için Log Analytics temelinde açma gösterilmektedir.

 > [!TIP]
 > Mevcut mantıksal uygulamalarınızı izlemek için bu adımları izleyin. [tanılama günlük özelliğini açar ve mantıksal uygulama çalışma zamanı verilerini Log Analytics'e gönderme](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Gereksinimler

Başlamadan önce Log Analytics çalışma alanına sahip olmanız gerekir. Bilgi [bir Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Mantıksal uygulama oluşturma tanılama günlüğünü etkinleştirme

1. İçinde [Azure portalında](https://portal.azure.com), mantıksal uygulama oluşturun. Seçin **kaynak Oluştur** > **Kurumsal tümleştirme** > **mantıksal uygulama**.

   ![Mantıksal uygulama oluşturma](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. İçinde **mantıksal uygulama oluştur** sayfasında, gösterildiği gibi şu görevleri gerçekleştirebilirsiniz:

   1. Mantıksal uygulamanız için bir ad belirtin ve Azure aboneliğinizi seçin. 
   2. Oluşturun veya bir Azure kaynak grubu seçin.
   3. Ayarlama **Log Analytics** için **üzerinde**. 
   Mantıksal uygulamanız için veri çalıştıran göndermek istediğiniz Log Analytics çalışma alanı seçin. 
   4. Hazır olduğunuzda seçin **panoya Sabitle** > **Oluştur**.

      ![Mantıksal uygulama oluşturma](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Bu adımı tamamladıktan sonra Azure artık, mantıksal uygulama oluşturur, Log Analytics çalışma alanıyla ilişkili. 
      Ayrıca, bu adım ayrıca otomatik olarak çalışma alanınızda Logic Apps yönetim çözümü yükler.

3. Mantıksal uygulamanızı görüntülemek için çalıştığında, [bu adımlarla devam edin](#view-logic-app-runs-oms).

## <a name="install-the-logic-apps-management-solution"></a>Logic Apps yönetimi çözümü yüklemek

Mantıksal uygulamanızı oluştururken Log Analytics temelinde zaten etkinleştirdiyseniz, bu adımı atlayın. Zaten yüklü Logic Apps yönetimi çözümü vardır.

1. İçinde [Azure portalında](https://portal.azure.com), seçin **diğer hizmetler**. Filtreniz olarak "log analytics" için arama yapın ve seçin **Log Analytics** gösterildiği gibi:

   !["Log Analytics" seçin](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. Altında **Log Analytics**bulup Log Analytics çalışma alanınızı seçin. 

   ![Log Analytics çalışma alanınızı seçin](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. Altında **Yönetim**, seçin **genel bakış**.

   !["OMS portalı" seçin](media/logic-apps-monitor-your-logic-apps-oms/ibiza-portal-page.png)

4. Genel bakış sayfasında, **Ekle** yönetim çözümleri kutucuğu açın. 

   !["Logic Apps Yönetimi" seçin](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

5. Listesini kaydırın **yönetim çözümleri**, seçin **Logic Apps Yönetimi** çözümü ve **Oluştur** Genel Bakış sayfasına yüklenecek.

   !["" Logic Apps yönetimi için"Ekle" öğesini seçin](media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-log-analytics-workspace"></a>Mantıksal uygulama çalıştırmalarınızı Log Analytics çalışma alanınızda görüntüleyin

1. Sayısı ve mantıksal uygulama çalıştırmalarınızı durumunu görüntülemek için Log Analytics çalışma alanınız için genel bakış sayfasına gidin. Ayrıntıları gözden **Logic Apps Yönetimi** Döşe.

   ![Mantıksal uygulama çalıştırması sayısı ve durumunu gösteren genel bakış kutucuğu](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

2. Mantıksal uygulama çalıştırmalarınızı hakkında daha fazla ayrıntı özeti görüntülemek için seçin **Logic Apps Yönetimi** Döşe.

   Burada, mantıksal uygulama çalıştırmalarınızı adına veya yürütme durumu göre gruplandırılır. Ayrıca, eylemleri ve Tetikleyicileri mantıksal uygulama çalıştırmaları için hata ayrıntılarını görebilirsiniz.

   ![Mantıksal uygulamanız için Özet durum çalıştırır](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. Belirli bir mantıksal uygulama ya da durumu için tüm çalıştırmalarını görüntülemek için bir mantıksal uygulamada veya bir durum için satır seçin.

   Belirli bir mantıksal uygulama için tüm çalıştırmalarını gösteren bir örnek aşağıda verilmiştir:

   ![Mantıksal uygulama veya bir durum için çalıştırmalarını görüntüle](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Bu sayfadaki iki Gelişmiş seçenekler vardır:
   * **İzlenen özellikler:** Eylemler, mantıksal uygulamanın göre gruplandırılmış izlenen özellikler bu sütunda görüntülenir. İzlenen özellikleri görüntülemek için seçin **görünümü**. Sütun filtresi kullanılarak izlenen özellikler arama yapabilirsiniz.
   
     ![Mantıksal uygulama için izlenen özellikleri görüntüleyin](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Yeni eklenen tüm izlenen özellikler, bunlar ilk kez görünmesi 10-15 dakika sürebilir. Bilgi [mantıksal uygulamanız için izlenen Özellikler ekleme](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Yeniden gönderin:** başarılı, başarısız olan bir veya daha fazla mantıksal uygulama çalıştırmaları yeniden gönderebilirsiniz veya çalışmakta olan. Çalıştırmaları yeniden gönderin ve istediğiniz onay kutularını seçin **yeniden**. 

     ![Mantıksal uygulama çalıştırmalarını yeniden gönder](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

4. Bu sonuçları filtrelemek için hem istemci hem de sunucu tarafı filtreleme gerçekleştirebilirsiniz.

   * İstemci tarafı filtresi: her sütun için istediğiniz filtreleri seçin. 
   İşte bazı örnekler:

     ![Örnek sütun filtreleri](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * Sunucu tarafı filtresi: belirli bir zaman penceresinde'i seçin ya da görünen çalışmaları sayısını sınırlandırmak için sayfanın en üstündeki kapsam denetimi kullanın. 
   Varsayılan olarak, aynı anda yalnızca 1.000 kayıtları görünür. 
   
     ![Zaman penceresini değiştirme](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. Tüm eylemleri ve belirli bir çalıştırma ayrıntılarını görüntülemek için bir mantıksal uygulama çalıştırması için bir satır seçin.

   Belirli bir mantıksal uygulama için çalıştırma için tüm eylemleri gösteren bir örnek aşağıda verilmiştir:

   ![Bir mantıksal uygulama çalıştırması için eylemleri görüntüleme](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
6. Tüm sonuçları sayfasında, sonuçları ardındaki sorguyu görüntülemek için veya tüm sonuçları görmek için seçin **bkz tüm**, günlük araması sayfasını açar.
   
   ![Tüm sonuçları sayfalarına bakın](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   Günlük araması sayfasında
   * Bir tablodaki sorgu sonuçlarını görmek için **tablo**.
   * Sorguyu değiştirmek için sorgu dizesini arama çubuğunda düzenleyebilirsiniz. 
   Daha iyi bir deneyim için seçin **Advanced Analytics**.

     ![Eylemler ve bir mantıksal uygulama çalıştırması ayrıntılarını görüntüleme](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     Burada Azure Log Analytics sayfasında sorguları güncelleştirebilirsiniz ve tablodan sonuçları görüntüleyin. 
     Bu sorgu kullanan [Kusto sorgu dili](https://docs.loganalytics.io/docs/Language-Reference), farklı sonuçlar görüntülemek istiyorsanız, düzenleyebilirsiniz. 

     ![Azure Log Analytics - sorgu görünümü](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Sonraki adımlar

* [B2B iletilerini izleme](../logic-apps/logic-apps-monitor-b2b-message.md)

