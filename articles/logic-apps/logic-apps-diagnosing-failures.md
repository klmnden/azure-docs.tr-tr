---
title: Sorun giderme ve tanılama hataları - Azure Logic Apps | Microsoft Docs
description: Azure Logic apps'te iş akışı hatalarını tanılama ve giderme hakkında bilgi edinin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.topic: article
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.date: 10/15/2017
ms.openlocfilehash: 62a74364939fffb6e06f51f1c0cabb6cce8c10e1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60999823"
---
# <a name="troubleshoot-and-diagnose-workflow-failures-in-azure-logic-apps"></a>Azure Logic apps'te iş akışı hatalarını tanılama ve giderme

Mantıksal uygulamanızı tanılayın ve sorunlarını uygulamanızda hata ayıklama yardımcı olabilecek bilgiler oluşturur. Mantıksal uygulama, Azure Portalı aracılığıyla iş akışındaki her adımı inceleyerek tanılayabilirsiniz. Veya bir iş akışı çalışma zamanı hata ayıklama için bazı adımlar ekleyebilirsiniz.

## <a name="review-trigger-history"></a>Tetikleyici geçmişi gözden geçirme

Her mantıksal uygulama tetikleyicisi ile başlatılır. İlk tetikleyici etkinleşmez, tetikleyici geçmişini denetleyin. Bu geçmiş, tüm mantıksal uygulamanızı yapılan tetikleyici denemeleri ve giriş ve çıkışları tetikleyici girişimleri ayrıntılarını listeler.

1. Tetikleyici, mantıksal uygulama menüsünde harekete olup olmadığını denetlemek için seçin **genel bakış**. Altında **tetikleyici geçmişi**, tetikleyicinin durumunu gözden geçirin.

   > [!TIP]
   > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

   ![Tetikleyici geçmişi gözden geçirme](./media/logic-apps-diagnosing-failures/logic-app-trigger-history-overview.png)

   > [!TIP]
   > * Beklediğiniz verileri bulamazsanız, seçmeyi deneyin **Yenile** araç.
   > * Liste çok girişimleri tetikler ve girişi bulunamıyor gösterir, listeyi filtreleme deneyin.

   Bir tetikleyici girişimi için olası durumlar şunlardır:

   | Durum | Açıklama | 
   | ------ | ----------- | 
   | **Başarılı oldu** | Tetikleyici, uç noktayı kullanıma ve kullanılabilir veri bulunamadı. Genellikle, bir "Fired" durumu da yanı sıra bu durum görüntülenir. Tetikleyici tanımında bir koşula sahip olmayabilir, varsa veya `SplitOn` karşılanmadığı komutu. <p>Bu durum, el ile tetikleyici, yineleme tetikleyicisi veya yoklama tetikleyici uygulayabilirsiniz. Tetikleyici başarıyla çalıştırabilirsiniz, ancak çalışma eylemleri işlenmeyen bir hata oluşturduğunuzda yine de başarısız olabilir. | 
   | **Atlandı** | Uç nokta tetikleyici işaretli, ancak hiçbir veri bulunamadı. | 
   | **Başarısız** | Bir hata oluştu. Başarısız bir tetikleyicinin herhangi oluşturulan hata iletileri gözden geçirmek için Bu tetikleyici girişim seçip **çıkışları**. Örneğin, geçerli olmayan girişler bulabilirsiniz. | 
   ||| 

   Aynı saat ve tarihi mantıksal uygulamanız birden çok öğe bulduğunda gerçekleşen, birden çok tetikleyici girdilere sahip olabilir. 
   Her zaman tetikleme tetiklendiğinde, Logic Apps altyapısı iş akışınızı çalıştırmak için bir mantıksal uygulama örneği oluşturur. Hiçbir iş akışı bir Farklı Çalıştır'ı başlatmadan önce beklenecek sahip olacak şekilde varsayılan olarak, her örneği paralel olarak çalışır.

   > [!TIP]
   > Tetikleyici, sonraki yineleme için beklemenize gerek kalmadan yeniden denetlemek. Genel Bakış araç çubuğunda **tetikleyicisi**ve bir onay zorlar tetikleyicisini seçin. Ya da seçin **çalıştırma** Logic Apps Tasarımcısı araç çubuğunda.

3. Altında bir tetikleyici girişimi ayrıntılarını incelemek için **tetikleyici geçmişi**, bu tetikleyici girişim seçin. 

   ![Bir tetikleyici girişimi seçin](./media/logic-apps-diagnosing-failures/logic-app-trigger-history.png)

4. Giriş ve tetikleyici oluşturulan tüm çıkışları gözden geçirin. Çıkışlarına tetikleyiciden gelen verileri görüntüleyin. Bu çıkışları tüm özellikler beklenen şekilde döndürülüp döndürülmediğini belirlemenize yardımcı olabilir.

   > [!NOTE]
   > Azure Logic Apps anlamadığınız herhangi bir içerik bulursanız, bilgi [farklı içerik türlerini işleme](../logic-apps/logic-apps-content-type.md).

   ![Tetikleyici çıkışı](./media/logic-apps-diagnosing-failures/trigger-outputs.png)

## <a name="review-run-history"></a>Çalıştırma geçmişini gözden geçirme

Bir iş akışı çalıştırma her başlatılan bir tetikleyici başlatır. Çalışan, iş akışı yanı sıra giriş ve çıkışları her adım için her bir adımın durumunu da dahil olmak üzere sırasında ne olduğunu gözden geçirebilirsiniz.

1. Mantıksal uygulama menüsünden **Genel Bakış**'ı seçin. Altında **çalıştırma geçmişi**, başlatılan bir tetikleyici çalıştırmasını gözden geçirin.

   > [!TIP]
   > Mantıksal uygulama menüsü görünmüyorsa Azure panosuna dönüp mantıksal uygulamanızı yeniden açmayı deneyin.

   ![Çalıştırma geçmişi gözden geçirme](./media/logic-apps-diagnosing-failures/logic-app-runs-history-overview.png)

   > [!TIP]
   > * Beklediğiniz verileri bulamazsanız, seçmeyi deneyin **Yenile** araç.
   > * Birçok çalıştırmaları listesi gösterir ve girişi bulunamıyor, listeyi filtreleme deneyin.

   Bir çalıştırma için olası durumlar şunlardır:

   | Durum | Açıklama | 
   | ------ | ----------- | 
   | **Başarılı oldu** | Tüm eylemleri başarılı oldu. <p>Belirli bir eylem içinde herhangi bir hata oluştuysa, bu hata aşağıdaki bir eylem iş akışı içinde işlenir. | 
   | **Başarısız** | En az bir eylem başarısız oldu ve iş akışındaki sonraki eylem yok hatayı işlemek için ayarlanmış. | 
   | **İptal edildi** | İş akışının çalıştığı ancak bir iptal isteği aldı. | 
   | **Çalıştıran** | İş akışı şu anda çalışıyor. <p>Bu durum, daraltılmış iş akışları için ya da geçerli fiyatlandırma planı nedeniyle gerçekleşebilir. Daha fazla bilgi için [eylem sınırları Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/logic-apps/). Ayarlarsanız [tanılama günlüğünü](../logic-apps/logic-apps-monitor-your-logic-apps.md), aynı zamanda gerçekleşen herhangi bir kısıtlama olayları hakkında bilgi edinebilirsiniz. | 
   ||| 

2. Her adımda belirli bir çalıştırma ayrıntılarını gözden geçirin. Altında **çalıştırma geçmişi**, incelemek istediğiniz Çalıştır'ı seçin.

   ![Çalıştırma geçmişi gözden geçirme](./media/logic-apps-diagnosing-failures/logic-app-run-history.png)

   Çalıştırmaya kendisini başarılı veya başarısız çalıştırma Ayrıntıları görünümünde her adım olup olmadığını gösterir ve olup bunların başarılı veya başarısız oldu.

   ![Bir mantıksal uygulama çalıştırmasının ayrıntılarını görüntüleme](./media/logic-apps-diagnosing-failures/logic-app-run-details.png)

3. Girişler, çıkışlar ve belirli bir adıma herhangi bir hata iletisi incelemek için Şekil genişletir ve Ayrıntılar gösterilir böylece bu adımı seçin. Örneğin:

   ![Adım ayrıntıları görüntüleme](./media/logic-apps-diagnosing-failures/logic-app-run-details-expanded.png)

## <a name="perform-runtime-debugging"></a>Çalışma zamanı hata ayıklama gerçekleştirin

Hata ayıklamaya yardımcı olmak için tanılama çalıştırma geçmişi ve tetikleyici gözden geçirme ile birlikte bir iş akışı adımlarını ekleyebilirsiniz. Örneğin, kullandığınız adımlar ekleyebilirsiniz [Web kancası Tester](https://webhook.site/) HTTP istekleri incelemek ve bunların tam bir boyut, Şekil ve biçimini belirler hizmet.

1. Ziyaret [Web kancası Tester](https://webhook.site/) ve oluşturulan benzersiz URL'yi kopyalayın

2. Mantıksal uygulamanızı, örneğin, test etmek istediğiniz gövde içeriği, bir ifade veya başka bir adım çıkış ile bir HTTP POST eylem ekleme.

3. URL'yi, Web kancası test edici için HTTP POST eylemlere yapıştırın.

4. Logic Apps altyapısı oluşturulmuş bir istek nasıl biçimlendirildiğini gözden geçirmek için mantıksal uygulamayı çalıştırın ve Web kancası Tester Ayrıntılar için bkz.

## <a name="next-steps"></a>Sonraki adımlar

[Mantıksal uygulamanızı izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md)
