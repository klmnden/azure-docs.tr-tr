---
title: Günlük analizi - Azure Logic Apps B2B iletiler için sorgu | Microsoft Docs
description: Günlük analizi izleme AS2, X 12 ve EDIFACT iletileri için sorgular oluşturun
author: padmavc
manager: anneta
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 345857801035fb7f149a57a4f0d58e7668f35b81
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="query-for-as2-x12-and-edifact-messages-in-log-analytics"></a>Sorgu için AS2, X 12 ve EDIFACT iletileri günlük analizi

AS2 bulmak için X12 veya EDIFACT ile takip ettiğiniz iletileri [Azure günlük analizi](../log-analytics/log-analytics-overview.md), belirli bir ölçüte dayalı Eylemler filtre sorgusu oluşturabilirsiniz. Örneğin, belirli Değişim Denetimi sayısına bağlı olarak iletileri bulabilirsiniz.

## <a name="requirements"></a>Gereksinimler

* Tanılama Günlüğü ile ayarlanmış bir mantıksal uygulama. Bilgi [bir mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) ve [bu mantıksal uygulama için günlük kaydını ayarlamak nasıl](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* İzleme ve günlük ile ayarlanan bir tümleştirme hesabı. Bilgi [tümleştirme hesap oluşturmak nasıl](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ve [izlemeyi ve o hesap için günlüğe kaydetmeyi nasıl ayarlanacağını](../logic-apps/logic-apps-monitor-b2b-message.md).

* Henüz yapmadıysanız [günlük analizi için tanılama veri yayımlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) ve [günlük analizi izleme iletisi ayarlama](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Önceki gereksinimlerini karşılamanızın sonra günlük analizi çalışma olması gerekir. Günlük analizi, B2B iletişiminde izlemek için aynı çalışma alanı kullanmanız gerekir. 
>  
> Günlük analizi çalışma alanı yoksa, bilgi [günlük analizi çalışma alanı oluşturmak nasıl](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="create-message-queries-with-filters-in-log-analytics"></a>Günlük analizi filtreleri ile iletisi sorguları oluşturma

Bu örnek, değişim denetimi numarasına göre iletileri nasıl bulabilirsiniz gösterir.

> [!TIP] 
> Günlük analizi çalışma alanı adınız biliyorsanız, çalışma giriş sayfasına gidin (`https://{your-workspace-name}.portal.mms.microsoft.com`) ve adım 4 olarak başlatın. Aksi takdirde, adım 1'den başlar.

1. İçinde [Azure portal](https://portal.azure.com), seçin **tüm hizmetleri**. "İçin günlük analizi" araması yapın ve ardından **günlük analizi** aşağıda gösterildiği gibi:

   ![Günlük analizi Bul](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. Altında **günlük analizi**, bulma ve günlük analizi çalışma alanınızı seçin.

   ![Günlük analizi çalışma alanınızı seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. Altında **Yönetim**, seçin **OMS portalı**.

   ![OMS portalı seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/omsportalpage.png)

4. Giriş sayfanızda seçin **günlük arama**.

   ![Giriş sayfanızda "Günlük arama" seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   -veya-

   ![Menüsünde "Günlük arama" öğesini seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

5. Arama kutusuna, bulmak ve basın istediğiniz bir alan girin **Enter**. Yazmaya başladığınızda, günlük analizi olası eşleşmeler ve kullanabileceğiniz işlemleri gösterir. Daha fazla bilgi edinmek [günlük analizi verileri bulmak üzere nasıl](../log-analytics/log-analytics-log-searches.md).

   Bu örnekte arama olan olaylar için **türü AzureDiagnostics =**.

   ![Sorgu dizesi yazmaya başlayın](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

6. Sol çubuğunda görüntülemek istediğiniz zaman dilimini seçin. Sorgunuz için bir filtre eklemek için **+ Ekle**.

   ![Filtre sorguya ekleme](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

7. Altında **filtreler Ekle**, istediğiniz filtreyi bulabilmek için filtre adı girin. Filtreyi seçin ve **+ Ekle**.

   Değişim Denetimi numarasını bulmak için bu örnek "değişim" sözcüğü için arar ve seçer **event_record_messageProperties_interchangeControlNumber_s** filtre olarak.

   ![Filtreyi seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

9. Sol çubuğunda ve seçin istediğiniz filtre değeri seçin **Uygula**.

   Bu örnek istiyoruz iletilerin değiş tokuş denetim numarası seçer.

   ![Filtre değeri seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

10. Şimdi oluşturmakta olduğunuz sorgu döndür. Sorgunuz seçilen filtre olay ve değeri ile güncelleştirilmiştir. Önceki sonuçlarınızı artık çok filtrelenir.

    ![Sorgunuz filtrelenmiş sonuçlar döndürür](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Sorgunuz gelecekte kullanım için Kaydet

1. Üzerinde sorgudan **günlük arama** sayfasında, **kaydetmek**. Sorgunuz bir ad verin, bir kategori seçin ve **kaydetmek**.

   ![Bir ad ve kategori sorgunuz verin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. Sorgunuz görüntülemek için seçin **Sık Kullanılanlar**.

   !["Sık Kullanılanlar" seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. Altında **kayıtlı aramaları**, sorgunuzu seçin, böylece sonuçlarını görüntüleyebilirsiniz. Farklı sonuçlar bulabilmek için sorguyu güncelleştirmek için sorgu düzenleyin.

   ![Sorgunuz seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-log-analytics"></a>Bulma ve günlük analizi kaydedilmiş sorguları çalıştırma

1. Günlük analizi çalışma alanı giriş sayfanız açın (`https://{your-workspace-name}.portal.mms.microsoft.com`) ve seçin **günlük arama**.

   ![Günlük analizi giriş sayfanızda "Günlük arama" seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   -veya-

   ![Menüsünde "Günlük arama" öğesini seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. Üzerinde **günlük arama** giriş sayfasını, seçin **Sık Kullanılanlar**.

   !["Sık Kullanılanlar" seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. Altında **kayıtlı aramaları**, sorgunuzu seçin, böylece sonuçlarını görüntüleyebilirsiniz. Farklı sonuçlar bulabilmek için sorguyu güncelleştirmek için sorgu düzenleyin.

   ![Sorgunuz seçin](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Sonraki adımlar

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şemaları](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Özel İzleme şemaları](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)