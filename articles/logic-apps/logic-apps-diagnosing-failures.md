---
title: "Hataları - Azure Logic Apps tanılama | Microsoft Docs"
description: "Logic apps nerede başarısız olduğunu anlamak için yaygın yolları"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 814e6f93088cdd96b0a663d2a7494b5a11470d99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="diagnose-logic-app-failures"></a>Mantıksal uygulama hatalarını tanılama
Sorunları veya hatalar logic apps ile karşılaşırsanız, var olan birkaç yaklaşımlar burada hataları'ten gelen daha iyi anlamanıza yardımcı olabilir.  

## <a name="azure-portal-tools"></a>Azure portal araçları
Azure portalında her mantıksal uygulama her adımında tanılamak için birçok araçlar sağlar.

### <a name="trigger-history"></a>Tetikleyici geçmişi

Her mantıksal uygulama en az bir tetikleyici sahiptir. Uygulamaları tetikleme olmayan fark ederseniz, tetikleyici geçmişi daha fazla bilgi için ilk arayın. Tetikleyici geçmişi mantığı app'ss ana dikey penceresinde erişebilir.

![Tetikleyici geçmişi bulma][1]

Tetikleyici geçmişi mantıksal uygulamanızı yapılan tüm tetikleyici girişimleri listeler. Ayrıntılara, özellikle incelemek için her bir tetikleyici denemesi, tüm girişleri veya tetikleyici girişimi oluşturulan çıkışları tıklatabilirsiniz. Başarısız Tetikleyicileri bulursanız, tetikleyici girişimi seçip **çıkışları** gözden için bağlantı oluşturulan hata iletileri, örneğin, geçerli olmayan FTP kimlik bilgileri.

Görebilirsiniz farklı durumlar şunlardır:

* **Atlanan**. Uç nokta verileri denetlemek için sorgulanan ve hiçbir veri yoktu bir yanıt aldı.
* **Başarılı bir şekilde**. Tetikleyici verileri kullanılabilir bir yanıt aldı. Bu durum, el ile bir tetikleyici, yineleme tetikleyici ya da bir yoklama tetikleyici neden olabilir. Bu durum genellikle tarafından eşlik **Fired** durumu, bir koşul veya SplitOn komutu memnun değildi kod görünümünde olup olmadığını olmayabilir ancak.
* **Başarısız**. Bir hata oluştu.

#### <a name="start-a-trigger-manually"></a>Bir tetikleyici el ile başlatın

Hemen bir sonraki tekrarı beklemeden kullanılabilir bir tetikleyici denetlemek için mantıksal uygulama istiyorsanız, **Tetik Seç** onay zorlamak için ana dikey penceresinde. Örneğin, Dropbox tetikleyiciyle bu bağlantıya tıkladıklarında, Dropbox yeni dosyaları hemen yoklamak iş akışı neden olur.

### <a name="run-history"></a>çalıştırma geçmişi

Her Mazotlu tetikleyici çalıştırılmasıyla sonuçlanır. İş akışı sırasında neler olduğunu anlamanıza yardımcı olabilecek birçok ayrıntılarını içeren ana dikey penceresinden çalışma bilgilere erişebilir.

![Çalıştırma geçmişi bulma][2]

Bir farklı çalıştır aşağıdaki durumlardan birini görüntüler:

* **Başarılı bir şekilde**. Tüm eylemleri başarılı oldu. Bir hata oluştu, bu hata oluştu. daha sonra iş akışında bir eylem tarafından işlendi. Diğer bir deyişle, hata, başarısız bir eylemden sonra çalışacak şekilde ayarlanmış bir eylem tarafından işlendi.
* **Başarısız**. En az bir eylemin daha sonra iş akışında bir eylem tarafından işlenmedi bir hata oluştu.
* **İptal**. İş akışının çalıştığı ancak iptal isteği aldı.
* **Çalışan**. İş akışı şu anda çalışıyor. Bu durum, daraltılmış iş akışları için ya da geçerli fiyatlandırma planı nedeniyle oluşabilir. Ayrıntılar için bkz [eylem sınırları Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/app-service/plans/). Tanılama (çalıştırma geçmişi altında görüntülenen grafiklerin) yapılandırma gerçekleşecek herhangi bir kısıtlama olayı hakkında bilgi de sağlayabilirsiniz.

Çalışma geçmişinize bakıldığında, daha fazla ayrıntı için ayrıntıya inebilir.  

#### <a name="trigger-outputs"></a>Tetikleyici çıkarır

Tetikleyici çıkışları tetikleyiciyle gelen verileri görüntüleyin. Bu çıktılar tüm özellikleri beklendiği gibi döndürülen olup olmadığını belirlemenize yardımcı olabilir.

> [!NOTE]
> Anlamadığınız herhangi bir içerik görürseniz, nasıl Azure Logic Apps öğrenin [farklı içerik türlerini işleme](../logic-apps/logic-apps-content-type.md).
> 

![Tetikleyici çıkışı örnekleri][3]

#### <a name="action-inputs-and-outputs"></a>Eylem girişleri ve çıkışları

Giriş ve bir eylem alınan çıkış inebilir. Bu verilerin boyutunu ve çıkışları şeklini anlamak için ve ayrıca oluşturulmuş olabilir herhangi bir hata iletisi bulmak için yararlıdır.

![Eylem girişleri ve çıkışları][4]

## <a name="debug-workflow-runtime"></a>İş akışı çalışma zamanı hata ayıklama

Giriş, çıkış ve Tetikleyicileri bir çalışma izleme ile birlikte hata ayıklamaya yardımcı bir iş akışı için bazı adımlar ekleyebilirsiniz. 
[RequestBin](http://requestb.in) bir iş akışında bir adım olarak ekleyebileceğiniz güçlü bir araçtır. RequestBin kullanarak tam boyutunu, Şekil ve bir HTTP istek biçimini belirlemek için HTTP isteği Inspector ayarlayabilirsiniz. Bir RequestBin oluşturun ve bir mantıksal uygulama HTTP POST eylemiyle, örneğin test etmek istediğiniz gövde içerik, ifade veya başka bir adım çıkış URL'sini yapıştırın. Mantıksal uygulama çalıştırın sonra isteği Logic Apps altyapısı oluşturulan zaman nasıl oluşturulduğu görmek için RequestBin yenileyebilirsiniz.

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
