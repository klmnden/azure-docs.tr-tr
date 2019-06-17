---
title: Azure İzleyici günlüklerine - Azure Logic Apps B2B iletileri için izleme sorguları oluşturma | Microsoft Docs
description: AS2, X 12 ve EDIFACT iletileri Azure Log analytics'te Azure Logic Apps için izleme sorguları oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: d4a94e75de34bbafd3bc8f1c1a0d1a6817245e5f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60846618"
---
# <a name="create-tracking-queries-for-b2b-messages-in-azure-monitor-logs-for-azure-logic-apps"></a>B2B iletileri için izleme sorguları, Azure Logic Apps için Azure İzleyici günlüklerine oluşturma

AS2 bulmak için X12 veya EDIFACT ile takip ettiğiniz iletileri [Azure İzleyici günlükleri](../log-analytics/log-analytics-overview.md), filtre eylemleri belirli ölçütlere göre sorgular oluşturabilirsiniz. Örneğin, belirli bir değişim denetimi sayısına göre iletileri bulabilirsiniz.

> [!NOTE]
> Bu sayfa, Microsoft Operations Management Suite (olan OMS ile), bu görevleri gerçekleştirmek adımlar daha önce açıklanan [Ocak 2019 ' devre dışı bırakma](../azure-monitor/platform/oms-portal-transition.md), bu adımlar, bunun yerine Azure Log Analytics ile değiştirir. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

* Tanılama günlük kaydı ile ayarlanmış bir mantıksal uygulama. Bilgi [bir mantıksal uygulama oluşturma işlemini](quickstart-create-first-logic-app-workflow.md) ve [nasıl ayarlanacağı, mantıksal uygulama için günlüğe kaydetmeyi](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* İzleme ve günlüğe kaydetme ile ayarlanmış bir tümleştirme hesabı. Bilgi [tümleştirme hesabı oluşturma işlemini](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ve [izleme ve günlüğe kaydetme için bu hesabı ayarlamak nasıl](../logic-apps/logic-apps-monitor-b2b-message.md).

* Henüz kaydolmadıysanız [tanılama verilerini Azure İzleyici günlüklerine yayımlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) ve [ileti Azure İzleyici günlüklerine izleme ayarlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="create-queries-with-filters"></a>Filtrelerle sorguları oluşturma

Belirli özellikler veya değerleri temel alan iletileri bulmak için filtreleri kullanan sorgular oluşturabilirsiniz. 

1. [Azure portalda](https://portal.azure.com) **Tüm hizmetler**’i seçin. Arama kutusuna "log analytics" bulup seçin **Log Analytics**.

   ![Log Analytics'e seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/find-log-analytics.png)

1. Altında **Log Analytics**bulup Log Analytics çalışma alanınızı seçin. 

   ![Log Analytics çalışma alanı seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/select-log-analytics-workspace.png)

1. Çalışma alanı menüsünde, altında **genel**, şunlardan birini seçin **günlükleri (Klasik)** veya **günlükleri**. 

   Bu örnek, Klasik günlükleri görünümünün nasıl kullanılacağı gösterilmektedir. 
   Seçerseniz **günlükleri görüntüleyebilir** içinde **Log Analytics deneyiminizi en üst düzeye** bölümündeki **arama ve günlüklerini çözümleme**, size **günlükleri (Klasik görünümü)** . 

   ![Klasik günlüklerini görüntüle](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/view-classic-logs.png)

1. Sorgu düzenleme kutusu, bulmak istediğiniz alan adını yazmaya başlayın. Yazmaya başladığınızda, sorgu Düzenleyicisi'ni kullanabilirsiniz işlemleri ve olası eşleşmeler gösterir. Sorgunuzu oluşturduktan sonra seçin **çalıştırma** veya Enter tuşuna basın.

   Bu örnek üzerinde eşleşmeleri arar **LogicAppB2B**. 
   Daha fazla bilgi edinin [Azure İzleyici günlüklerine veri bulma](../log-analytics/log-analytics-log-searches.md).

   ![Sorgu dizesi yazmaya başlayın](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/create-query.png)

1. Sol bölmede, görüntülemek istediğiniz zaman çerçevesini değiştirmek için süre listesinden seçin veya kaydırıcıyı sürükleyin. 

   ![Değişiklik zaman çerçevesi](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/change-timeframe.png)

1. Sorgunuz için bir filtre eklemek için **Ekle**. 

   ![Sorgu Filtresi Ekle](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/add-filter.png)

1. Altında **ekleme filtreleri**, bulmak istediğiniz filtre adı girin. Filtre fark ederseniz, bu filtre'ı seçin. Sol bölmede seçin **Ekle** yeniden.

   Örneğin, üzerinde farklı bir sorgu işte **türü "AzureDiagnostics" ==** seçerek Değişim Denetimi sayısına göre olaylar ve bulduğu sonuçları **event_record_messageProperties_ interchangeControlNumber_s** filtre.

   ![Filtre değeri seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/filter-example.png)

   Seçtiğiniz sonra **Ekle**, sorgunuz, seçilen filtre olay ve değer güncelleştirilir. 
   Önceki sonuçlarınızı şimdi çok filtrelenir. 

   Bu sorgu gibi arar **türü "AzureDiagnostics" ==** ve sonuçları kullanarak bir değişim denetimi sayısına göre bulur **event_record_messageProperties_interchangeControlNumber_s**filtre.

   ![Filtrelenmiş sonuçları](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-query"></a>Sorguyu Kaydet

Sorgunuzda kaydetmek için **günlükleri (Klasik)** görüntülemek için aşağıdaki adımları izleyin:

1. Üzerinde sorgunuzun **günlükleri (Klasik)** sayfasında **Analytics**. 

   !["Analiz" seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/choose-analytics.png)

1. Sorgu araç çubuğunda **Kaydet**.

   !["Kaydet"i seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/save-query.png)

1. Örneğin, sorgu hakkındaki ayrıntıları sağlayın, sorgunuzu seçin bir ad verin **sorgu**ve bir kategori adı sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

   !["Kaydet"i seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query-details.png)

1. Kaydedilmiş Sorgular görüntülemek için sorgu sayfasına geri dönün. Sorgu araç çubuğunda **kayıtlı aramalar**.

   ![Seçin "Kayıtlı aramalar"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/choose-saved-searches.png)

1. Altında **kayıtlı aramalar**, sonuçları görüntülemek için sorguyu seçin. 

   ![Sorgunuzu seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/saved-query-results.png)

   Farklı sonuçlar sizi bulabilmesi için sorguyu güncellemek için sorguyu düzenleyin.

## <a name="find-and-run-saved-queries"></a>Bulma ve kaydedilmiş sorgular çalıştırma

1. [Azure portalda](https://portal.azure.com) **Tüm hizmetler**’i seçin. Arama kutusuna "log analytics" bulup seçin **Log Analytics**.

   ![Log Analytics'e seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/find-log-analytics.png)

1. Altında **Log Analytics**bulup Log Analytics çalışma alanınızı seçin. 

   ![Log Analytics çalışma alanı seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/select-log-analytics-workspace.png)

1. Çalışma alanı menüsünde, altında **genel**, şunlardan birini seçin **günlükleri (Klasik)** veya **günlükleri**. 

   Bu örnek, Klasik günlükleri görünümünün nasıl kullanılacağı gösterilmektedir. 

1. Sorgu araç çubuğunda, sorgu sayfası, açıldıktan sonra seçin **kayıtlı aramalar**.

   ![Seçin "Kayıtlı aramalar"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/choose-saved-searches.png)

1. Altında **kayıtlı aramalar**, sonuçları görüntülemek için sorguyu seçin. 

   ![Sorgunuzu seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/saved-query-results.png) 

   Sorguyu otomatik olarak çalışır, ancak sorgu, sorgu Düzenleyicisi'nde herhangi bir nedenle çalışmazsa seçin **çalıştırma**.

## <a name="next-steps"></a>Sonraki adımlar

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şemaları](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Özel İzleme şemaları](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)