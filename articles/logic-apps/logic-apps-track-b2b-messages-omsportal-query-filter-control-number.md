---
title: Log Analytics - Azure Logic Apps B2B iletileri için sorgular oluşturma | Microsoft Docs
description: Azure Logic Apps için AS2, X 12 ve EDIFACT iletileri Log Analytics ile izleme sorguları oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/19/2018
ms.openlocfilehash: baccd255fc2812eae0de3a98dfcef3dcbc7e1b46
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124279"
---
# <a name="create-queries-for-tracking-as2-x12-and-edifact-messages-in-log-analytics-for-azure-logic-apps"></a>AS2, X 12 ve EDIFACT iletileri Log analytics'te Azure Logic Apps için izleme sorguları oluşturma

AS2 bulmak için X12 veya EDIFACT ile takip ettiğiniz iletileri [Azure Log Analytics](../log-analytics/log-analytics-overview.md), filtre eylemleri belirli ölçütlere göre sorgular oluşturabilirsiniz. Örneğin, belirli bir değişim denetimi sayısına göre iletileri bulabilirsiniz.

## <a name="requirements"></a>Gereksinimler

* Tanılama günlük kaydı ile ayarlanmış bir mantıksal uygulama. Bilgi [bir mantıksal uygulama oluşturma işlemini](../logic-apps/quickstart-create-first-logic-app-workflow.md) ve [nasıl ayarlanacağı, mantıksal uygulama için günlüğe kaydetmeyi](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* İzleme ve günlüğe kaydetme ile ayarlanmış bir tümleştirme hesabı. Bilgi [tümleştirme hesabı oluşturma işlemini](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ve [izleme ve günlüğe kaydetme için bu hesabı ayarlamak nasıl](../logic-apps/logic-apps-monitor-b2b-message.md).

* Henüz kaydolmadıysanız [Log Analytics için tanılama verilerini yayımlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) ve [ileti Log Analytics'te izleme ayarlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Önceki gereksinimlerini karşılamanızın sonra bir Log Analytics çalışma alanı olmalıdır. B2B iletişiminizin Log Analytics, izleme için aynı çalışma alanı kullanmanız gerekir. 
>  
> Bir Log Analytics çalışma alanınız yoksa, bilgi [bir Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="create-message-queries-with-filters-in-log-analytics"></a>Log analytics'te filtrelerle iletisi sorguları oluşturma

Bu örnekte, iletilerin değişim denetim numaralarına göre nasıl bulabileceğinizi gösterilmektedir.

> [!TIP] 
> Log Analytics çalışma alanınızın adının biliyorsanız, çalışma alanı giriş sayfanıza gidin (`https://{your-workspace-name}.portal.mms.microsoft.com`) ve adım 4 hızla başlatın. Aksi takdirde, adım 1'den başlar.

1. İçinde [Azure portalında](https://portal.azure.com), seçin **tüm hizmetleri**. "Log analytics için" için arama yapın ve ardından **Log Analytics** burada gösterildiği gibi:

   ![Log Analytics'i bulma](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. Altında **Log Analytics**bulup Log Analytics çalışma alanınızı seçin.

   ![Log Analytics çalışma alanınızı seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. Altında **Yönetim**, seçin **günlük araması**.

   ![Lo arama seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/azure-portal-page.png)

4. Arama kutusuna bulmak ve tuşuna basarak istediğiniz bir alan girin **Enter**. Yazmaya başladığınızda, Log Analytics, olası eşleşmeler ve kullanabileceğiniz işlemleri gösterilmektedir. Daha fazla bilgi edinin [Log Analytics'te veri bulma](../log-analytics/log-analytics-log-searches.md).

   Bu örnekte arama olan olaylar için **türü AzureDiagnostics =**.

   ![Sorgu dizesi yazmaya başlayın](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

5. Sol çubuğunda, görüntülemek istediğiniz zaman çerçevesini seçin. Sorgunuz için bir filtre eklemek için **+ Ekle**.

   ![Sorgu Filtresi Ekle](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

6. Altında **ekleme filtreleri**, istediğiniz filtreyi bulabilmesi için filtre adı girin. Filtre seçin ve **+ Ekle**.

   Değişim denetim numarası bulmak için bu örnekte "değişim" sözcüğü için arar ve seçer **event_record_messageProperties_interchangeControlNumber_s** filtre olarak.

   ![Filtre seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

7. Sol çubuğunda, seçin ve istediğiniz filtre değeri **Uygula**.

   Bu örnek, değişim denetim numarası istiyoruz iletileri seçer.

   ![Filtre değeri seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

8. Artık oluşturduğunuz sorgu için döndürür. Sorgunuz, değer ve seçilen filtre olay ile güncelleştirildi. Önceki sonuçlarınızı şimdi çok filtrelenir.

    ![Sorgunuzu filtrelenmiş sonuçları ile geri dönün](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Sorgunuzu gelecekte kullanım için kaydedin

1. Üzerinde sorgunuzun **günlük araması** sayfasında **Kaydet**. Sorgunuzu bir ad verin, bir kategori seçip seçin **Kaydet**.

   ![Bir ad ve kategori sorgunuzu verin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. Sorgunuzu görüntülemek için seçin **Sık Kullanılanlar**.

   !["Sık Kullanılanlar" seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. Altında **kayıtlı aramalar**, sorgunuzu seçin, böylece sonuçlarını görüntüleyebilirsiniz. Farklı sonuçlar sizi bulabilmesi için sorguyu güncellemek için sorguyu düzenleyin.

   ![Sorgunuzu seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-log-analytics"></a>Bulma ve Log Analytics'te kaydedilmiş sorgular çalıştırma

1. Log Analytics çalışma alanı giriş sayfanızı açın (`https://{your-workspace-name}.portal.mms.microsoft.com`) ve **günlük araması**.

   ![Log Analytics'e giriş sayfanızda "Günlük araması" seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   -veya-

   !["Ara" menüsünde'yi seçin.](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. Üzerinde **günlük araması** öğesini giriş sayfasını **Sık Kullanılanlar**.

   !["Sık Kullanılanlar" seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. Altında **kayıtlı aramalar**, sorgunuzu seçin, böylece sonuçlarını görüntüleyebilirsiniz. Farklı sonuçlar sizi bulabilmesi için sorguyu güncellemek için sorguyu düzenleyin.

   ![Sorgunuzu seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Sonraki adımlar

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şemaları](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Özel İzleme şemaları](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)