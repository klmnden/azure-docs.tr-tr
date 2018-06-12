---
title: Hataları - Azure Logic Apps tanılamak ve gidermek | Microsoft Docs
description: Logic apps neden ve nasıl başarısız anlama
services: logic-apps
documentationcenter: ''
author: jeffhollan
manager: jeconnoc
editor: ''
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 10/15/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 5af99821305fe6daab8a213d0351c5a1c5936461
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298799"
---
# <a name="troubleshoot-and-diagnose-logic-app-failures"></a>Mantıksal uygulama hataları tanılamak ve gidermek

Mantıksal uygulamanızı tanılamanıza ve sorunları, uygulamanızda hata ayıklama yardımcı olabilecek bilgiler oluşturur. Azure Portalı aracılığıyla iş akışındaki her adım gözden geçirerek bir mantıksal uygulama tanılayabilirsiniz. Veya bir iş akışı çalışma zamanı hata ayıklama için bazı adımlar ekleyebilirsiniz.

## <a name="review-trigger-history"></a>Gözden geçirme tetikleyici geçmişi

Her mantıksal uygulama tetikleyicisi'yle başlatır. Tetikleyici harekete değil, ilk tetikleyici geçmişini denetleyin. Bu geçmiş, tüm mantıksal uygulamanızı yapılan tetikleyici girişimlerinin ve girişleri ve çıkışları her tetikleyici girişimi için ayrıntılarını listeler.

1. Tetikleyici, logic app menünüzde harekete olup olmadığını denetlemek için seçin **genel bakış**. Altında **tetikleyici geçmişi**, tetikleyici durumunu gözden geçirin.

   > [!TIP]
   > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

   ![Gözden geçirme tetikleyici geçmişi](./media/logic-apps-diagnosing-failures/logic-app-trigger-history-overview.png)

   > [!TIP]
   > * Beklediğiniz veri bulamazsanız, seçmeyi deneyin **yenileme** araç çubuğunda.
   > * Listenin birçok denemeleri tetikleyebilir ve istediğiniz girişi bulunamıyor gösteriyorsa, listenin filtrelemeyi deneyin.

   Bir tetikleyici girişimi için olası durumlar şunlardır:

   | Durum | Açıklama | 
   | ------ | ----------- | 
   | **Başarılı oldu** | Tetikleyici uç noktayı kullanıma ve kullanılabilir veri bulunamadı. Genellikle, bir "Fired" durumu da bu durum görüntülenir. Tetikleyici tanımı bir koşula sahip olmayabilir, varsa veya `SplitOn` karşılanmadığı komutu. <p>Bu durum, el ile tetikleyici, yineleme tetikleyici veya yoklama tetikleyici uygulayabilirsiniz. Bir tetikleyici başarılı bir şekilde çalıştırabilirsiniz ancak eylemleri işlenmemiş hataları oluşturduğunuzda Çalıştır yine başarısız olabilir. | 
   | **Atlandı** | Uç nokta tetikleyici işaretli, ancak hiçbir veri bulunamadı. | 
   | **Başarısız oldu** | Bir hata oluştu. Başarısız bir tetikleyici olabilecek oluşturulan hata iletilerini gözden geçirmek için Bu tetikleyici girişim seçin ve seçin **çıkışları**. Örneğin, geçerli olmayan girişleri görebilirsiniz. | 
   ||| 

   Aynı saat ve tarihi mantıksal uygulamanızı birden çok öğe bulduğunda gerçekleşen, birden çok tetikleyici girişlerle olabilir. 
   Her tetikleyici ateşlenir Logic Apps altyapısı, iş akışınızı çalıştırmak için bir mantıksal uygulama örneği oluşturur. Hiçbir iş akışı bir farklı çalıştır başlatmadan önce beklenecek sahip olması varsayılan olarak, her örneğinin paralel olarak çalışır.

   > [!TIP]
   > Tetikleyici için bir sonraki tekrarı beklemeden yeniden denetle. Genel Bakış araç çubuğunda seçin **tetikleyici çalıştırmak**ve bir onay zorlar Tetikleyici seçin. Ya da seçin **çalıştırmak** Logic Apps Designer araç.

3. Bir tetikleyici girişimi ayrıntılarını altında incelemek için **tetikleyici geçmişi**, bu tetikleyici girişim seçin. 

   ![Bir tetikleyici girişimi seçin](./media/logic-apps-diagnosing-failures/logic-app-trigger-history.png)

4. Girdi ve tetikleyici oluşturulan herhangi bir çıkış gözden geçirin. Tetikleyici çıkışları tetikleyiciyle gelen verileri görüntüleyin. Bu çıktılar tüm özellikleri beklendiği gibi döndürülen olup olmadığını belirlemenize yardımcı olabilir.

   > [!NOTE]
   > Anlamadığınız herhangi bir içerik bulursanız, nasıl Azure Logic Apps öğrenin [farklı içerik türlerini işleme](../logic-apps/logic-apps-content-type.md).

   ![Tetikleyici çıkarır](./media/logic-apps-diagnosing-failures/trigger-outputs.png)

## <a name="review-run-history"></a>Çalıştırma geçmişini gözden geçirme

Her Mazotlu tetikleyici çalıştırmak bir iş akışı başlatır. Çalıştırılan, durum her adımı için iş akışı artı girişleri ve çıkışları her adımı için dahil olmak üzere sırasında ne gözden geçirebilirsiniz.

1. Mantıksal uygulama menüsünden **Genel Bakış**'ı seçin. Altında **çalıştıran geçmişi**, çalıştırması Mazotlu tetikleyici için gözden geçirin.

   > [!TIP]
   > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

   ![Gözden geçirme geçmişi çalıştırır](./media/logic-apps-diagnosing-failures/logic-app-runs-history-overview.png)

   > [!TIP]
   > * Beklediğiniz veri bulamazsanız, seçmeyi deneyin **yenileme** araç çubuğunda.
   > * Liste çok sayıda çalıştırır gösterir ve istediğiniz girişi bulunamıyor, listenin filtrelemeyi deneyin.

   Bir çalıştırma için olası durumlar şunlardır:

   | Durum | Açıklama | 
   | ------ | ----------- | 
   | **Başarılı oldu** | Tüm eylemleri başarılı oldu. <p>Hataları belirli bir eylemi oluştuysa, bu hata aşağıdaki bir eylem iş akışı içinde işlenir. | 
   | **Başarısız oldu** | En az bir eylemi başarısız oldu ve herhangi bir sonraki eylem iş akışı içinde hata işlemek üzere ayarlanmış. | 
   | **İptal edildi** | İş akışının çalıştığı ancak iptal isteği aldı. | 
   | **Çalışan** | İş akışı şu anda çalışıyor. <p>Bu durum, daraltılmış iş akışları için ya da geçerli fiyatlandırma planı nedeniyle gerçekleşebilir. Daha fazla bilgi için bkz: [eylem sınırları Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/logic-apps/). Ayarlarsanız [tanılama günlükleri](../logic-apps/logic-apps-monitor-your-logic-apps.md), gerçekleşecek herhangi bir kısıtlama olayı hakkında bilgi elde edebilirsiniz. | 
   ||| 

2. Her adım belirli çalıştırmada ayrıntılarını gözden geçirin. Altında **çalıştıran geçmişi**, incelemek istediğiniz Çalıştır'ı seçin.

   ![Gözden geçirme geçmişi çalıştırır](./media/logic-apps-diagnosing-failures/logic-app-run-history.png)

   Çalıştırması kendisini başarılı veya başarısız çalıştırma Ayrıntıları görünümü her adım olup olmadığını gösterir ve olup bunlar başarılı veya başarısız.

   ![Bir mantıksal uygulama çalıştırmasının ayrıntılarını görüntüleme](./media/logic-apps-diagnosing-failures/logic-app-run-details.png)

3. Böylece şekli genişletir ve ayrıntıları gösterir girişleri ve çıkışları belirli bir adımı için herhangi bir hata iletisi incelemek için bu adım seçin. Örneğin:

   ![Adım ayrıntıları görüntüleme](./media/logic-apps-diagnosing-failures/logic-app-run-details-expanded.png)

## <a name="perform-runtime-debugging"></a>Çalışma zamanı hata ayıklama gerçekleştirmek

Hata ayıklamaya yardımcı olmak için tanılama geçmişi çalıştırır ve tetikleyici gözden geçirme ile birlikte bir iş akışı adımlarını ekleyebilirsiniz. Örneğin, kullandığınız adımlar ekleyebilirsiniz [RequestBin](http://requestb.in) hizmet HTTP isteklerini inceleyin ve bunların tam boyutu, Şekil ve biçimini belirler.

1. Özel ve yalnızca tarayıcısında görüntülenebilen yapabileceğiniz bir RequestBin oluşturun.

2. Örneğin, test etmek istediğiniz gövde içerik, ifade veya başka bir adım çıkış sahip bir HTTP POST eylem mantıksal uygulamanızı ekleyin.

3. URL, RequestBin için HTTP POST eyleme yapıştırın.

4. Bir istek Logic Apps altyapısı oluşturulan zaman nasıl biçimlendirildiğinden gözden geçirmek için mantıksal uygulama çalıştırın ve, RequestBin yenileyin.

## <a name="next-steps"></a>Sonraki adımlar

[Mantıksal uygulamanızı izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md)