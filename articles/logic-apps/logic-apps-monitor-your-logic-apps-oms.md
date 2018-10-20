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
ms.date: 10/11/2018
ms.openlocfilehash: 177c361734a88acab5fc10d6b460645be82bf437
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457151"
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-log-analytics"></a>İzleyin ve mantıksal uygulama çalıştırmaları Log Analytics ile ilgili Öngörüler edinin

İzleme ve daha zengin hata ayıklama bilgileri almak için bir mantıksal uygulama oluşturduğunuzda, aynı anda Log Analytics temelinde kapatabilirsiniz. Log Analytics, Azure portalı üzerinden mantıksal uygulamanız için izleme ve günlüğe kaydetme tanılama çalıştıran sağlar. Logic Apps yönetim çözümü eklediğinizde, mantıksal uygulama çalıştırmaları ve durumunu, yürütme süresi, yeniden gönderme durumu ve bağıntı kimlikleri gibi belirli Ayrıntılar için toplanan durumunu alın.

Bu makalede, çalışma zamanı olayları görüntüleyebilir ve mantıksal uygulamanız için verileri çalıştırmak için Log Analytics temelinde açma gösterilmektedir.

 > [!TIP]
 > Mevcut mantıksal uygulamalarınızı izlemek için bu adımları izleyin. [tanılama günlük özelliğini açar ve mantıksal uygulama çalışma zamanı verilerini Log Analytics'e gönderme](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Gereksinimler

Başlamadan önce Log Analytics çalışma alanına sahip olmanız gerekir. Bilgi [bir Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Mantıksal uygulama oluşturma tanılama günlüğünü etkinleştirme

1. İçinde [Azure portalında](https://portal.azure.com), mantıksal uygulama oluşturun. Seçin **kaynak Oluştur** > **tümleştirme** > **mantıksal uygulama**.

   ![Mantıksal uygulama oluşturma](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

1. Altında **mantıksal uygulama oluştur**, gösterildiği gibi şu görevleri gerçekleştirebilirsiniz:

   1. Mantıksal uygulamanız için bir ad belirtin ve Azure aboneliğinizi seçin. 

   1. Oluşturun veya bir Azure kaynak grubu seçin.

   1. Ayarlama **Log Analytics** için **üzerinde**. 

   1. Liste Log Analytics çalışma alanı listeden mantıksal uygulamanız için veri çalıştıran göndermek istediğiniz çalışma alanını seçin. 

      ![Mantıksal uygulama oluşturma](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Bu adımı tamamladıktan sonra Azure artık, mantıksal uygulama oluşturur, Log Analytics çalışma alanıyla ilişkili. 
      Ayrıca, bu adım ayrıca otomatik olarak çalışma alanınızda Logic Apps yönetim çözümü yükler.

   1. İşiniz bittiğinde **Oluştur**’u seçin.

1. Mantıksal uygulamanızı görüntülemek için çalıştığında, [bu adımlarla devam edin](#view-logic-app-runs-oms).

## <a name="install-the-logic-apps-management-solution"></a>Logic Apps yönetimi çözümü yüklemek

Mantıksal uygulamanızı oluştururken Log Analytics temelinde zaten etkinleştirdiyseniz, bu adımı atlayın. Zaten yüklü Logic Apps yönetimi çözümü vardır.

1. [Azure portalda](https://portal.azure.com) **Tüm hizmetler**’i seçin. Arama kutusuna "log analytics filtreniz olarak" girin ve seçin **Log Analytics**.

   !["Log Analytics" seçin](./media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

1. Altında **Log Analytics**bulup Log Analytics çalışma alanınızı seçin. 

   ![Log Analytics çalışma alanınızı seçin](./media/logic-apps-monitor-your-logic-apps-oms/select-log-analytics-workspace.png)

1. Altında **izleme çözümleri yapılandırma**, seçin **çözümleri görüntülemek**.

   !["Çözümleri görüntüle" seçin](media/logic-apps-monitor-your-logic-apps-oms/log-analytics-workspace.png)

1. Genel bakış sayfasında, **Ekle**, açan **yönetim çözümleri** listesi. Bu listeden **Logic Apps Yönetimi**. 

   !["Logic Apps Yönetimi" seçin](./media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

   Listenin, çözüm bulamazsanız seçin **daha fazla Yükle** çözüm görünene kadar.

1. Seçin **Oluştur**, çözümü yükler.

   !["" Logic Apps yönetimi için"Ekle" öğesini seçin](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-logic-app-runs-in-log-analytics-workspace"></a>Log Analytics çalışma alanında görünümü mantıksal uygulama çalıştırmaları

1. Sayısı ve mantıksal uygulama çalıştırmalarınızı durumunu görüntülemek için Log Analytics çalışma alanınıza gidin ve genel bakış sayfasını açın. 

   Mantıksal uygulama çalıştırmalarınızı hakkındaki ayrıntıları görünmez **Logic Apps Yönetimi** Döşe. Mantıksal uygulama çalıştırmalarınızı hakkında daha fazla ayrıntı özeti görüntülemek için seçin **Logic Apps Yönetimi** Döşe. 

   ![Mantıksal uygulama çalıştırması sayısı ve durumunu gösteren genel bakış kutucuğu](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   Burada, mantıksal uygulama çalıştırmalarınızı adına veya yürütme durumu göre gruplandırılır. 
   Bu sayfa ayrıca eylemler veya mantıksal uygulama çalıştırmaları için Tetikleyiciler hatalarıyla ilgili ayrıntıları gösterir.

   ![Mantıksal uygulamanız için Özet durum çalıştırır](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
1. Belirli bir mantıksal uygulama ya da durumu için tüm çalıştırmalarını görüntülemek için bir mantıksal uygulamada veya bir durum için satır seçin.

   Belirli bir mantıksal uygulama için tüm çalıştırmalarını gösteren bir örnek aşağıda verilmiştir:

   ![Mantıksal uygulama veya bir durum için çalıştırmalarını görüntüle](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Bu sayfa, bu Gelişmiş seçenekler vardır:

   * **İzlenen özellikler:**

     Bu sütun, Eylemler, mantıksal uygulamanın göre gruplandırılmış izlenen özellikleri gösterir. İzlenen özellikleri görüntülemek için seçin **görünümü**. 
     İzlenen özellikler aramak için sütun filtresi kullanın.
   
     ![Mantıksal uygulama için izlenen özellikleri görüntüleyin](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Yeni eklenen tüm izlenen özellikler, bunlar ilk kez görünmesi 10-15 dakika sürebilir. Bilgi [mantıksal uygulamanız için izlenen Özellikler ekleme](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Yeniden gönderin:** başarılı, başarısız olan bir veya daha fazla mantıksal uygulama çalıştırmaları yeniden gönderebilirsiniz veya çalışmakta olan. Çalıştırmaları yeniden gönderin ve istediğiniz onay kutularını seçin **yeniden**. 

     ![Mantıksal uygulama çalıştırmalarını yeniden gönder](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

1. Bu sonuçları filtrelemek için hem istemci hem de sunucu tarafı filtreleme gerçekleştirebilirsiniz.

   * **İstemci tarafı filtresi**: her sütun için örneğin istediğiniz filtreleri seçin:

     ![Örnek sütun filtreleri](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * **Sunucu tarafı filtresi**: belirli bir zaman penceresinde'i seçin ya da görünen çalışmaları sayısını sınırlandırmak için sayfanın en üstündeki kapsam denetimi kullanın. Varsayılan olarak, aynı anda yalnızca 1.000 kayıtları görünür.
   
     ![Zaman penceresini değiştirme](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
1. Tüm eylemleri ve belirli bir çalıştırma ayrıntılarını görüntülemek için bir mantıksal uygulama çalıştırması için bir satır seçin.

   Belirli bir mantıksal uygulama için çalıştırma için tüm eylemleri gösteren bir örnek aşağıda verilmiştir:

   ![Bir mantıksal uygulama çalıştırması için eylemleri görüntüleme](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
1. Tüm sonuçları sayfasında, sonuçları ardındaki sorguyu görüntülemek için veya tüm sonuçları görmek için seçin **bkz tüm**, günlük araması sayfasını açar.
   
   ![Tüm sonuçları sayfalarına bakın](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   Günlük araması sayfasında

   * Bir tablodaki sorgu sonuçlarını görmek için **tablo**.

   * Sorguyu değiştirmek için sorgu dizesini arama çubuğunda düzenleyebilirsiniz. 
   Daha iyi bir deneyim için seçin **Advanced Analytics**.

     ![Eylemler ve bir mantıksal uygulama çalıştırması ayrıntılarını görüntüleme](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     Azure Log Analytics sayfasında, güncelleştirme sorguları ve tablodan sonuçları görüntüleyin. Bu sorgu kullanan [Kusto sorgu dili](https://aka.ms/LogAnalyticsLanguageReference), farklı sonuçlar görüntülemek istiyorsanız, düzenleyebilirsiniz. 

     ![Azure Log Analytics - sorgu görünümü](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Sonraki adımlar

* [B2B iletilerini izleme](../logic-apps/logic-apps-monitor-b2b-message.md)