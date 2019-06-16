---
title: Azure İzleyici günlükleri - Azure Logic Apps ile mantıksal uygulamaları izleme | Microsoft Docs
description: Insights ve sorun giderme ve mantıksal uygulama çalıştırmalarınızı Azure Log Analytics ile tanılama verilerini hata ayıklama
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: 3f890e6cabd757fdd38374befaaccd1a10c9bd96
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62106219"
---
# <a name="monitor-logic-apps-with-azure-monitor-logs"></a>Azure İzleyici günlükleri ile mantıksal uygulamaları izleme

İzleme ve logic apps hakkında daha zengin hata ayıklama bilgi almak üzere açmak [Azure İzleyici günlükleri](../log-analytics/log-analytics-overview.md) mantıksal uygulamanızı oluşturduğunuzda. Azure İzleyici günlüklerine Logic Apps yönetimi çözümü Azure Portalı'nda yüklediğinizde, logic apps için izleme ve günlüğe kaydetme tanılama sağlar. Bu çözüm, ayrıca mantıksal uygulamanız için toplanan bilgileri durumunu, yürütme süresi, yeniden gönderme durumu ve bağıntı kimlikleri gibi belirli Ayrıntılar ile çalışan sağlar. Bu makale Azure İzleyici günlüklerine üzerinde çalışma zamanı olayları görüntüleyebilir ve mantıksal uygulamanız için veri çalıştıran devre dışı bırakma.

Var olan mantıksal uygulamalar için Azure İzleyici günlüklerine etkinleştirmek için bu adımları izleyin. [tanılama günlük özelliğini açar ve Azure İzleyici günlüklerine logic app çalışma zamanı veri gönderme](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

> [!NOTE]
> Bu sayfa, Microsoft Operations Management Suite (olan OMS ile), bu görevleri gerçekleştirmek adımlar daha önce açıklanan [Ocak 2019 ' devre dışı bırakma](../azure-monitor/platform/oms-portal-transition.md), bu adımlar, bunun yerine Azure Log Analytics ile değiştirir. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce Log Analytics çalışma alanı gerekir. Bilgi [bir Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md). 

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

## <a name="install-logic-apps-management-solution"></a>Logic Apps yönetim çözümü yükleme

Mantıksal uygulamanızı oluştururken Azure İzleyici açtığında zaten etkinleştirdiyseniz, bu adımı atlayın. Zaten yüklü Logic Apps yönetimi çözümü vardır.

1. [Azure portalda](https://portal.azure.com) **Tüm hizmetler**’i seçin. Arama kutusuna "log analytics" bulup seçin **Log Analytics**.

   !["Log Analytics" seçin](./media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

1. Altında **Log Analytics**bulup Log Analytics çalışma alanınızı seçin. 

   ![Log Analytics çalışma alanınızı seçin](./media/logic-apps-monitor-your-logic-apps-oms/select-log-analytics-workspace.png)

1. Altında **Log Analytics ile çalışmaya başlama** > **izleme çözümleri yapılandırma**, seçin **çözümleri görüntülemek**.

   !["Çözümleri görüntüle" seçin](media/logic-apps-monitor-your-logic-apps-oms/log-analytics-workspace.png)

1. Genel bakış sayfasında, **Ekle**, açan **yönetim çözümleri** listesi. Bu listeden **Logic Apps Yönetimi**. 

   !["Logic Apps Yönetimi" seçin](./media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

   Listenin, çözüm bulamazsanız seçin **daha fazla Yükle** çözüm görünene kadar.

1. Seçin **Oluştur**, doğrulamak istediğiniz çözümü yüklemek ve sonra Log Analytics çalışma alanı **Oluştur** yeniden.   

   !["" Logic Apps yönetimi için"Oluştur" öğesini seçin.](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

   Mevcut bir çalışma alanı kullanmak istemiyorsanız, şu anda yeni bir çalışma alanı da oluşturabilirsiniz.

   İşiniz bittiğinde, Logic Apps yönetim çözümü genel bakış sayfasında görüntülenir. 

<a name="view-logic-app-runs-oms"></a>

## <a name="view-logic-app-run-information"></a>Bilgi görünümü mantıksal uygulama

Mantıksal uygulamanızı çalıştıktan sonra bu çalıştırmaları sayısını ve durum görüntüleyebilirsiniz **Logic Apps Yönetimi** Döşe. 

1. Log Analytics çalışma alanınıza gidin ve genel bakış sayfasını açın. Seçin **Logic Apps Yönetimi**. 

   ![Mantıksal uygulama durumu ve sayımı Çalıştır](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

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

   * **Yeniden gönderin:** Başarısız, bir veya daha fazla mantıksal uygulama çalıştırmaları başarılı, yeniden gönderebilirsiniz veya hala çalışıyor. Çalıştırmaları yeniden gönderin ve istediğiniz onay kutularını seçin **yeniden**. 

     ![Mantıksal uygulama çalıştırmalarını yeniden gönder](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

1. Bu sonuçları filtrelemek için hem istemci hem de sunucu tarafı filtreleme gerçekleştirebilirsiniz.

   * **İstemci tarafı filtresi**: Her sütun için örneğin istediğiniz filtreleri seçin:

     ![Örnek sütun filtreleri](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * **Sunucu tarafı filtresi**: Belirli bir zaman penceresinin seçin ya da görünen çalışmaları sayısını sınırlamak için sayfanın en üstündeki kapsam denetimi kullanın. Varsayılan olarak, aynı anda yalnızca 1.000 kayıtları görünür.
   
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
     
     Log analytics sayfasında güncelleştirme sorguları ve tablodan sonuçları görüntüleyin. Bu sorgu kullanan [Kusto sorgu dili](https://aka.ms/LogAnalyticsLanguageReference), farklı sonuçlar görüntülemek istiyorsanız, düzenleyebilirsiniz. 

     ![log analytics - sorgu görünümü](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Sonraki adımlar

* [B2B iletilerini izleme](../logic-apps/logic-apps-monitor-b2b-message.md)