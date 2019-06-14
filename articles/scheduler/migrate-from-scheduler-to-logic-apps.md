---
title: Azure Zamanlayıcı ' Azure Logic Apps'e geçirme
description: Devre dışı bırakılıyor, Azure zamanlayıcı işleri Azure Logic Apps ile nasıl değiştirebileceğiniz öğrenin
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/20/2018
ms.openlocfilehash: 25ed66fd75301475542dbac8e8a01670ee37563c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60531683"
---
# <a name="migrate-azure-scheduler-jobs-to-azure-logic-apps"></a>Azure Logic Apps için Azure zamanlayıcı işlerini geçirme

> [!IMPORTANT]
> Azure Logic Apps, Azure Scheduler, devre dışı bırakılıyor yerini alıyor. İşleri zamanlamak için bunun yerine Azure Logic Apps'e taşıma için bu makaleyi izleyin.

Bu makalede, Azure Scheduler ile değil, Azure Logic Apps ile otomatik iş akışları oluşturarak tek seferlik ve yinelenen işleri nasıl zamanlayabilirsiniz gösterilmektedir. Logic Apps ile zamanlanmış işleri oluşturduğunuzda, bu avantajlar alın:

* Kavramı hakkında endişelenmek zorunda olmadığınız bir *iş koleksiyonu* her mantıksal uygulama ayrı bir Azure kaynağı olduğundan.

* Tek bir mantıksal uygulama'yı kullanarak, birden çok tek seferlik iş çalıştırabilirsiniz.

* Azure Logic Apps hizmetinin saat dilimini ve günışığından (DST) destekler.

Daha fazla bilgi için bkz: [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md) veya bu hızlı başlangıçta ilk mantıksal uygulamanızı oluşturmayı deneyin: [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Mantıksal uygulamanızı HTTP isteği göndererek tetiklemek için bir aracı gibi kullanın [Postman masaüstü uygulaması](https://www.getpostman.com/apps).

## <a name="schedule-one-time-jobs"></a>Tek seferlik işleri zamanlama

Yalnızca tek bir mantıksal uygulama oluşturarak, tek seferlik birden çok iş çalıştırabilirsiniz. 

### <a name="create-your-logic-app"></a>Mantıksal uygulamanızı oluşturma

1. İçinde [Azure portalında](https://portal.azure.com), Logic Apps Tasarımcısı'nda boş bir mantıksal uygulama oluşturun. 

   Temel adımlarını izleyin [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Arama kutusuna filtreniz olarak "http isteği," girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Bir HTTP isteği alındığında** 

   !["İstek" tetikleyici ekleme](./media/migrate-from-scheduler-to-logic-apps/request-trigger.png)

1. İstek tetikleyicisi için Logic Apps Tasarımcısı'nin gelen istek girişleri yapısını anlamanıza yardımcı olur ve çıkışlar, iş akışında seçmenizi kolaylaştırır bir JSON şema, isteğe bağlı olarak sağlayabilir.

   Bir şema belirtmek için şemada girin **istek gövdesi JSON şeması** kutusunda, örneğin: 

   ![İstek şeması](./media/migrate-from-scheduler-to-logic-apps/request-schema.png)

   Bir şema yok, ancak bir örnek yük JSON biçiminde varsa, bir şema bu yükü oluşturabilir.

   1. İstek tetikleyicisinde seçin **şema oluşturmak için örnek yük kullanma**.

   1. Altında **girin veya yapıştırın örnek JSON yükü**, örnek yükünüzü sağlayın ve ardından **Bitti**, örneğin:

      ![Örnek yük](./media/migrate-from-scheduler-to-logic-apps/sample-payload.png)

1. Tetikleyici altında seçin **sonraki adım**. 

1. Arama kutusuna "filtreniz olarak Geciktir" girin. Eylemler listesinde şu eylemi seçin: **Geciktir:**

   Bu eylem, belirtilen tarih ve saate kadar mantıksal uygulama iş akışınızı duraklatır.

   !["Geciktir" Eylem Ekle](./media/migrate-from-scheduler-to-logic-apps/delay-until.png)

1. Mantıksal uygulama iş akışını başlatmak istediğiniz zaman için zaman damgasını girin. 

   İçine tıkladığınızda **zaman damgası** kutusu, dinamik içerik listesi görünür olan tetikleyiciyi isteğe bağlı olarak bir çıkış seçebilmeniz için.

   !["Geciktir" ayrıntılarını sağlayın](./media/migrate-from-scheduler-to-logic-apps/delay-until-details.png)

1. Seçerek çalıştırmak istediğiniz herhangi bir eylem ekleme [~ 200'den fazla bağlayıcı](../connectors/apis-list.md). 

   Örneğin, bir URL isteği gönderen bir HTTP eylem veya depolama kuyrukları, Service Bus kuyrukları ve Service Bus konuları ile iş eylemleri içerebilir: 

   ![HTTP eylemi](./media/migrate-from-scheduler-to-logic-apps/request-http-action.png)

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

   ![Mantıksal uygulamanızı kaydetme](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

   Mantıksal uygulamanızı ilk kez kaydederken, uç nokta URL'si mantıksal uygulamanızın istek tetikleyicisi için görünür **HTTP POST URL'si** kutusu. 
   Mantıksal uygulamanızı çağırın ve mantıksal uygulamanız için işleme girişleri göndermek istediğinizde, bu URL'yi çağrı hedefi olarak kullanın.

   ![İstek tetikleyicisi uç nokta URL'si Kaydet](./media/migrate-from-scheduler-to-logic-apps/request-endpoint-url.png)

1. Kopyalayın ve daha sonra mantıksal uygulamanızı tetikleyen el ile bir istek göndermek için bu uç nokta URL'si kaydedin. 

## <a name="start-a-one-time-job"></a>Bir kerelik iş Başlat

El ile çalıştırmak veya tek seferlik bir iş tetiklemek için mantıksal uygulamanızın istek tetikleyicisi için uç nokta URL'si için bir çağrı gönderin. Bu çağrı, daha önce bir şema belirterek açıklanan göndermek için yük veya girdi belirtin. 

Örneğin, Postman uygulamasını kullanarak, ayarlarla bu örneğe benzer bir POST isteği oluşturun ve ardından **Gönder** istekte bulunmak için.

| İstek yöntemi | URL'si | Gövde | Üst bilgiler |
|----------------|-----|------|---------| 
| **POST** | <*uç nokta URL'si*> | **Ham** <p>**JSON(application/json)** <p>İçinde **ham** kutusuna, istekte göndermek istediğiniz yük girin. <p>**Not**: Bu ayarı otomatik olarak yapılandırır **üstbilgileri** değerleri. | **Anahtar**: İçerik türü <br>**Değer**: application/json
 |||| 

![Mantıksal uygulamanızı el ile tetiklemek için isteği gönder](./media/migrate-from-scheduler-to-logic-apps/postman-send-post-request.png)

Çağrı gönderdikten sonra mantıksal uygulamanızı yanıttan altında görünür **ham** kutusuna **gövdesi** sekmesi. 

<a name="workflow-run-id"></a>

> [!IMPORTANT]
>
> Daha sonra işi iptal etmek istiyorsanız seçin **üstbilgileri** sekmesi. Bulup kopyalayabilirsiniz **x-ms-iş akışı-Çalıştır-ID** yanıt üstbilgi değeri. 
>
> ![Yanıt](./media/migrate-from-scheduler-to-logic-apps/postman-response.png)

## <a name="cancel-a-one-time-job"></a>Tek seferlik bir işi iptal et

Logic Apps'te, her bir kerelik iş örneğini çalıştıran tek bir mantıksal uygulama yürütür. Tek seferlik bir işi iptal etmek için kullanabileceğiniz [- iş akışı çalıştırmaları iptal etme](https://docs.microsoft.com/rest/api/logic/workflowruns/cancel) Logic Apps REST API'de. Tetikleyici için bir çağrı gönderdiğinizde sağlamak [iş akışı çalıştırma kimliği](#workflow-run-id).

## <a name="schedule-recurring-jobs"></a>Yinelenen işleri zamanlama

### <a name="create-your-logic-app"></a>Mantıksal uygulamanızı oluşturma

1. İçinde [Azure portalında](https://portal.azure.com), Logic Apps Tasarımcısı'nda boş bir mantıksal uygulama oluşturun. 

   Temel adımlarını izleyin [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Arama kutusuna filtreniz olarak "yinelenme" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Yineleme** 

   !["Yinelenme" tetikleyicisini ekleyin](./media/migrate-from-scheduler-to-logic-apps/recurrence-trigger.png)

1. Daha gelişmiş bir zamanlama ayarlamak istiyorsanız ayarlayın.

   ![Gelişmiş zamanlama](./media/migrate-from-scheduler-to-logic-apps/recurrence-advanced-schedule.png)

   Gelişmiş zamanlama seçenekleri hakkında daha fazla bilgi için bkz: [oluşturma ve çalıştırma yinelenen görevleri ve Azure Logic Apps ile iş akışları](../connectors/connectors-native-recurrence.md)

1. Eklemek istediğiniz seçerek diğer eylemleri [200'den fazla bağlayıcı](../connectors/apis-list.md). Tetikleyici altında seçin **sonraki adım**. Bulun ve istediğiniz eylemi seçin.

   Örneğin, bir URL isteği gönderen bir HTTP eylem veya depolama kuyrukları, Service Bus kuyrukları ve Service Bus konuları ile iş eylemleri içerebilir: 

   ![HTTP eylemi](./media/migrate-from-scheduler-to-logic-apps/recurrence-http-action.png)

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

   ![Mantıksal uygulamanızı kaydetme](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

## <a name="advanced-setup"></a>Gelişmiş Kurulum

İşlerinizi özelleştirebileceğiniz diğer yollar şunlardır.

### <a name="retry-policy"></a>Yeniden deneme ilkesi

Bir eylem aralıklı hatalar meydana geldiğinde mantıksal uygulamanız yeniden dener şeklini denetlemek için ayarlayabileceğiniz [yeniden deneme ilkesi](../logic-apps/logic-apps-exception-handling.md#retry-policies) her eylemin ayarlarında, örneğin:

1. Eylemin açın ( **...** ) seçin ve menü **ayarları**.

   ![Eylem ayarları](./media/migrate-from-scheduler-to-logic-apps/action-settings.png)

1. İstediğiniz yeniden deneme İlkesi'ni seçin. Her ilke hakkında daha fazla bilgi için bkz: [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies).

   ![Yeniden deneme ilkesi seçin](./media/migrate-from-scheduler-to-logic-apps/retry-policy.png)

## <a name="handle-exceptions-and-errors"></a>Özel durumları ve hataları işleme

Azure Scheduler'da çalıştırmak varsayılan eylem başarısız olursa, hata durumu ele alan diğer eylem çalıştırabilirsiniz. Azure Logic Apps'te, aynı görevi gerçekleştirebilirsiniz.

1. Logic Apps Tasarımcısı'nda eylemi işlemek için işaretçinizi adımlar arasındaki okun üzerine getirin ve seçmek istediğiniz ve **parallel dal Ekle**. 

   ![Paralel dal Ekle](./media/migrate-from-scheduler-to-logic-apps/add-parallel-branch.png)

1. Bul ve bunun yerine alternatif bir eylem olarak çalıştırmak istediğiniz eylemi seçin.

   ![Paralel bir eylem ekleme](./media/migrate-from-scheduler-to-logic-apps/add-parallel-action.png)

1. Alternatif bir eylem üzerinde açın ( **...** ) seçin ve menü **sonrasında çalıştırmayı Yapılandır**.

   ![Sonrasında çalıştırmayı Yapılandır](./media/migrate-from-scheduler-to-logic-apps/configure-run-after.png)

1. Kutusunu temizleyin **başarılı** özelliği. Bu özellikler'i seçin: **başarısız oldu**, **atlandı**, ve **zaman aşımına uğradı**

   !["Sonra Çalıştır" özelliklerini ayarlama](./media/migrate-from-scheduler-to-logic-apps/select-run-after-properties.png)

1. İşiniz bittiğinde **Bitti**'yi seçin.

Özel durum işleme hakkında daha fazla bilgi için bkz: [hataları ve özel durumları - RunAfter özelliği](../logic-apps/logic-apps-exception-handling.md#catch-and-handle-failures-with-the-runafter-property).

## <a name="faq"></a>SSS

<a name="retire-date"></a> 

**Q**: Azure Zamanlayıcı'yı ne zaman emekli? <br>
**A**: Azure Zamanlayıcı, 30 Eylül 2019 üzerinde devre dışı bırakmak için zamanlandı.

**Q**: Hizmet kaldırdıktan sonra Zamanlayıcı İş koleksiyonları ve işlerine ne olur? <br>
**A**: Tüm Scheduler iş koleksiyonları ve işleri sistemden silinir.

**Q**: Yedekleme veya Zamanlayıcı İşlerim Logic Apps'e geçiş yapmadan önce herhangi bir görevi gerçekleştirmek var mı? <br>
**A**: En iyi uygulama, her zaman çalışmanızı yedekleyin. Oluşturduğunuz logic apps silmeden veya Zamanlayıcı işlerinizi devre dışı bırakma önce beklendiği gibi çalıştığından emin olun. 

**Q**: Bana İşlerim Zamanlayıcıdan Logic Apps'e geçirme yardımcı olabilecek bir aracı var mı? <br>
**A**: Her bir zamanlayıcı iş benzersiz olduğundan her kuruluşa uyacak bir aracı yok. Ancak, çeşitli betikleri için ihtiyaçlarınız için değiştirmeniz amacıyla kullanılabilir. Betik kullanılabilirlik için daha sonra tekrar deneyin.

**Q**: Destek Zamanlayıcı İşlerim geçirmek için nereden alabilirim? <br>
**A**: Destek almak için bazı yollar şunlardır: 

**Azure portal**

Azure aboneliğiniz, ücretli bir destek planınız varsa, Azure portalında bir teknik destek talebi oluşturabilirsiniz. Aksi takdirde, farklı destek seçeneği seçebilirsiniz.

1. Üzerinde [Azure portalında](https://portal.azure.com) ana menüsünde, select **Yardım + Destek**.

1. Altında **Destek**seçin **yeni destek isteği**. İsteğiniz bu ayrıntıları sağlayın:

   | Ayar | Değer |
   |---------|-------|
   | **Sorun türü** | **Teknik** | 
   | **Abonelik** | <*Azure aboneliği Sihirbazı*> | 
   | **Hizmet** | Altında **izleme ve Yönetim**seçin **Zamanlayıcı**. | 
   ||| 

1. İstediğiniz Destek seçeneğini belirleyin. Ücretli bir destek planınız varsa seçin **sonraki**.

**Topluluk**

* [Azure Logic Apps Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-scheduler)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps ile düzenli olarak çalıştırılan görevler ve iş akışları oluşturma](../connectors/connectors-native-recurrence.md)
* [Öğretici: Zamanlama tabanlı mantıksal uygulama ile trafiği denetleme](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)