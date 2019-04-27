---
title: Web API'ları ve REST API'leri oluşturmak için Azure Logic Apps | Microsoft Docs
description: Web API'ları ve REST API'leri için sistem tümleştirmeleri Azure Logic apps'te, API'leri, hizmetleri ve sistemleri çağırmak için oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.topic: article
ms.assetid: bd229179-7199-4aab-bae0-1baf072c7659
ms.date: 05/26/2017
ms.openlocfilehash: 620ede672d71338abeff5198fd5f94e92dc193d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60681888"
---
# <a name="create-custom-apis-you-can-call-from-azure-logic-apps"></a>Azure mantıksal uygulamalardan arayabileceğiniz özel API'ler oluşturma

Azure Logic Apps sunmasına karşın [100'den fazla yerleşik bağlayıcı](../connectors/apis-list.md) mantıksal uygulama iş akışlarınızla kullanabilirsiniz, API, sistemler ve bağlayıcı olarak kullanılamayan Hizmetleri çağırmak isteyebilirsiniz. Eylemler ve tetikleyiciler logic apps içinde kullanılacak sağlayan kendi Apı'lerinize oluşturabilirsiniz. Mantıksal uygulama iş akışlarından çağırabileceğiniz kendi API'leri oluşturmak için neden isteyebileceğiniz diğer nedenler şunlardır:

* Geçerli sistem tümleştirme ve veri tümleştirmesi iş akışlarınızı genişletin.
* Müşterilerin kişisel veya profesyonel görevlerini yönetmek için hizmetinizi kullanmak yardımcı olur.
* Erişim, bulunabilirlik ve kullanım hizmetiniz için genişletin.

Temel olarak, web takılabilir arabirimleri için REST kullanma API bağlayıcıları olan [Swagger meta veri biçimi](https://swagger.io/specification/) belgeleri ve kendi veri takas biçimi olarak JSON. Bağlayıcıları bir HTTP uç noktaları iletişim kuran bir REST API'leri olduğundan, .NET, Java ve Node.js gibi herhangi bir dilde bağlayıcılar oluşturmak için kullanabilirsiniz. Apı'leriniz üzerinde barındırabilirsiniz [Azure App Service](../app-service/overview.md), bir platform-a sağlayan en iyi, kolay ve en iyi yollarından biri, API barındırmak için sunan hizmet olarak (PaaS). 

Logic apps ile çalışmak özel API'ler için API'nizi sağlayabilir [ *eylemleri* ](./logic-apps-overview.md#logic-app-concepts) mantıksal uygulama iş akışlarınızla belirli görevler gerçekleştirir. API'nizi gibi de davranabilir bir [ *tetikleyici* ](./logic-apps-overview.md#logic-app-concepts) yeni veriler veya bir olay belirtilen bir koşulu karşıladığında bir mantıksal uygulama iş akışı başlar. Bu konuda, eylemleri ve Tetikleyicileri sağlamak için API'nizi istediğiniz davranışına göre API'nizi oluşturmak için izlemeniz gereken ortak deseni açıklar.

Apı'lerinizi barındırmak [Azure App Service](../app-service/overview.md), bir platform-a sağlayan yüksek düzeyde ölçeklenebilir, kolay API'sini barındıran sunan hizmet olarak (PaaS).

> [!TIP] 
> Apı'lerinizi web uygulamaları olarak dağıtabilmenize karşın olarak derleme, barındırma ve API'leri bulutta ve şirket içi bağlandığınızda işinizi kolaylaştırabilir ve API apps, API dağıtımı göz önünde bulundurun. Apı'lerinizi herhangi bir kod değişikliği--yalnızca kodunuzu API uygulamasına dağıtma yok. Örneğin, bu dillerden ile oluşturulan API uygulamaları oluşturmayı öğrenin: 
> 
> * [ASP.NET](../app-service/app-service-web-get-started-dotnet.md). 
> * [Java](../app-service/app-service-web-get-started-java.md)
> * [Node.js](../app-service/app-service-web-get-started-nodejs.md)
> * [PHP](../app-service/app-service-web-get-started-php.md)
> * [Python](../app-service/containers/quickstart-python.md)
> * [Ruby](../app-service/containers/quickstart-ruby.md)
>
> Logic apps için oluşturulan API uygulaması örnekleri için ziyaret [Azure Logic Apps GitHub deposu](https://github.com/logicappsio) veya [blog](https://aka.ms/logicappsblog).

## <a name="how-do-custom-apis-differ-from-custom-connectors"></a>Özel API'leri nasıl özel bağlayıcılar arasından farklıdır?

Özel API'ler ve [özel Bağlayıcılar](../logic-apps/custom-connector-overview.md) olan web takılabilir arabirimleri için REST kullanma API'lerini [Swagger meta veri biçimi](https://swagger.io/specification/) belgeleri ve kendi veri takas biçimi olarak JSON. Ve bu API'ler ve Bağlayıcılarla HTTP uç noktaları iletişim kuran bir REST API'leri olduğundan, özel API'ler ve Bağlayıcılarla oluşturmak için .NET, Java ve Node.js gibi herhangi bir dil kullanabilirsiniz.

Özel API'ler, bağlayıcılar olmayan API'leri çağırmak ve HTTP + Swagger, Azure API yönetimi ve uygulama hizmetleri ile çağırabileceğiniz uç noktaları sağlar olanak tanır. Özel bağlayıcılar, özel API'ler gibi çalışır ancak bu öznitelikler de:

* Azure Logic Apps Bağlayıcısı kaynakları olarak kayıtlı.
* Logic Apps Tasarımcısı'nda Microsoft tarafından yönetilen bağlayıcılar ile birlikte simgeler görünür.
* Bağlayıcılar yazarlar ve mantıksal uygulamaların dağıtıldığı bölgede aynı Azure Active Directory kiracısına ve Azure aboneliğine sahip bir mantıksal uygulama kullanıcıları için kullanılabilir.

Ayrıca, Microsoft sertifikası için kayıtlı bağlayıcılar belirleyebilirsiniz. Bu işlem, kayıtlı bağlayıcılar genel kullanıma ölçütlere uyan ve bu bağlayıcıları Microsoft Flow ve Microsoft Powerapps'te kullanıcıların kullanımına doğrular.

Özel bağlayıcılar hakkında daha fazla bilgi için bkz. 

* [Özel bağlayıcılara genel bakış](../logic-apps/custom-connector-overview.md)
* [Web API'lerinden özel bağlayıcılar oluşturma](../logic-apps/custom-connector-build-web-api-app-tutorial.md)
* [Azure Logic Apps'te özel bağlayıcıları kaydetme](../logic-apps/logic-apps-custom-connector-register.md)

## <a name="helpful-tools"></a>Yararlı araçları

Ayrıca API sahip olduğunda özel bir API logic apps ile en iyi şekilde çalışır. bir [Swagger belgesinin](https://swagger.io/specification/) API'nin işlemlerini ve parametrelerini açıklar.
Birçok kitaplıkları ister [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle), Swagger dosyası sizin için otomatik olarak oluşturur. Swagger dosyası görünen adları, özellik türleri ve benzeri için açıklama eklemek için de kullanabilirsiniz [TRex](https://github.com/nihaue/TRex) böylece Swagger dosyanızı logic apps ile de çalışır.

<a name="actions"></a>

## <a name="action-patterns"></a>Eylem desenleri

Logic apps görevleri gerçekleştirmek için özel API'nizi sağlamalıdır [ *eylemleri*](./logic-apps-overview.md#logic-app-concepts). Her bir işlemde API'niz için bir eylem eşler. Temel bir eylem HTTP istekleri ve HTTP yanıtlarını döndürür kabul eden bir denetleyicisidir. Örneğin, bir mantıksal uygulama, web uygulamanızı veya API uygulaması için bir HTTP isteği gönderir. Uygulamanızı daha sonra mantıksal uygulama işleyebilen içeriği ile birlikte bir HTTP yanıtı döndürür.

Standart bir eylem için bir HTTP istek yöntemi API'nizi yazabilir ve bu yöntem bir Swagger dosyasında açıklar. Ardından API'niz ile doğrudan çağırabilir bir [HTTP eylemi](../connectors/connectors-native-http.md) veya [da HTTP + Swagger](../connectors/connectors-native-http-swagger.md) eylem. Varsayılan olarak, içinde yanıt verilmesi gereken [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md). 

![Standart eylem deseni](./media/logic-apps-create-api-app/standard-action.png)

<a name="pattern-overview"></a> API'nizi uzun soluklu görevlerin tamamlarken bekleyin bir mantıksal uygulama yapmak için API'NİZİN izleyebilirsiniz [yoklama zaman uyumsuz desen](#async-pattern) veya [zaman uyumsuz Web kancası deseni](#webhook-actions) bu konuda açıklanan. Benzetme her zaman bu desenleri farklı davranışları görselleştirmenize yardımcı olan bir pastane öğesinden özel pasta sıralama işlemi düşünün. Yoklama deseni pastayı hazır olup olmadığını denetlemek için her 20 dakikada pastane çağırdığınız davranışı yansıtır. Web kancası deseni pastayı hazır olduğunda bunlar çağırabilirsiniz burada pastane telefon numaranızı ister davranışı yansıtır.

Örnekler için ziyaret [Logic Apps GitHub deposu](https://github.com/logicappsio). Ayrıca, daha fazla bilgi edinin [eylemleri için kullanım ölçümü](logic-apps-pricing.md).

<a name="async-pattern"></a>

### <a name="perform-long-running-tasks-with-the-polling-action-pattern"></a>Uzun süre çalışan görevleri yoklama eylemi deseni

API'nizi uzun süre çalışan görevler gerçekleştirmek için [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md), yoklama zaman uyumsuz desen kullanabilirsiniz. Bu düzen ayrı iş parçacığı ancak canlı bir etkin bağlantı Logic Apps altyapısı iş, API sahiptir. Bu şekilde, mantıksal uygulama, zaman aşımına uğramaz veya çalışma API'nizi tamamlanmadan önce iş akışındaki bir sonraki adımla devam edin.

Genel Düzen aşağıdaki gibidir:

1. Altyapı API isteği kabul ve çalışmaya bilir emin olun.
2. Altyapı sonraki istekleri için iş durumunu yaptığında, API'nizi görev tamamlandığında bilmeniz altyapısı sağlar.
3. Mantıksal uygulama iş akışı devam edebilmeniz için ilgili veri Altyapısı'na dönün.

<a name="bakery-polling-action"></a> Artık önceki pastane benzerliği yoklama desen uygulamak ve pastane ve sipariş özel pasta için teslim çağırmanızı düşünün. Pasta yapma işlemi sürüyor ve pastane pastayı üzerinde çalışırken telefonda beklemek istemiyorsanız. Pastane siparişinizi onaylar ve, her 20 dakikada pastayı ait durum için çağrı sahiptir. 20 dakika geçmesi sonra pastane diyebilirsiniz, ama, pasta bitti değildir ve sizin başka bir programda 20 dakika çağırmalıdır bunlar size. Bu geri yönlü işlemi çağırabilir ve pastane siparişinizi hazırdır ve sunar, pasta söyler kadar devam eder. 

Şimdi bu yoklama düzeni yeniden eşleyin. Logic Apps altyapısı, pasta müşteri temsil ederken pastane özel API'nizi temsil eder. Altyapısı bir isteği ile API'nizi çağırdığında, API'nizi istek onaylar ve altyapı iş durumunu kontrol edebilirsiniz zaman aralığı ile yanıt verdiği. Altyapısı işlemi yapılır API'nize yanıt verene kadar iş durumunu kontrol etme devam eder ve sonra iş akışı devam eder, mantıksal uygulama için verileri döndürür. 

![Yoklama eylemi deseni](./media/logic-apps-create-api-app/custom-api-async-action-pattern.png)

Takip etmek, API'niz için belirli adımlar şunlardır API'nin açısından açıklanmaktadır:

1. API'nizi iş başlatmak için bir HTTP isteği aldığında, hemen bir HTTP geri `202 ACCEPTED` yanıtıyla `location` daha sonra bu adımda açıklandığı gibi başlığı. Bu yanıt, API'nizi isteği alındı, istek Yükü (veri giriş) kabul bildiğinizden Logic Apps altyapısı sağlar ve şu anda işleniyor. 
   
   `202 ACCEPTED` Yanıt bu üst bilgileri içermelidir:
   
   * *Gerekli*: A `location` nereden Logic Apps altyapısı kontrol API'NİZİN iş durumu bir URL mutlak yolu belirten üst bilgisi

   * *İsteğe bağlı*: A `retry-after` altyapısı denetlemeden önce beklemesi gereken saniye sayısını belirten üst bilgisi `location` iş durumu için URL. 

     Varsayılan olarak, her 20 saniyede altyapısı denetler. Farklı bir zaman aralığı belirtmek için dahil `retry-after` üstbilgi ve sonraki yoklama kadar saniye sayısı.

2. Belirtilen süre geçtikten sonra yoklamalar Logic Apps altyapısı `location` iş durumunu denetlemek için URL. API'nizi, bu denetimleri gerçekleştirmek ve bu yanıtlar döndürür:
   
   * Bir HTTP işlemi yapıldığında, iade `200 OK` yanıt, yanıt yükünde (bir sonraki adım için giriş) yanı sıra.

   * İş hala işliyorsa, başka bir HTTP dönüş `202 ACCEPTED` yanıt, özgün yanıt olarak aynı üst bilgileri ile.

API'nizi bu düzeni uygularken, iş durumunu kontrol etme devam etmek için mantıksal uygulama iş akışı tanımında bir şey yapmanız gerekmez. Ne zaman altyapısı alır bir HTTP `202 ACCEPTED` yanıt ve geçerli bir `location` üst bilgi, alt yapısı zaman uyumsuz desen uyar ve denetimleri `location` API'nizi 202 yanıt dönene kadar başlığı.

> [!TIP]
> Bu örnek zaman uyumsuz desen için gözden [zaman uyumsuz denetleyici yanıt örneğini github'da](https://github.com/logicappsio/LogicAppsAsyncResponseSample).

<a name="webhook-actions"></a>

### <a name="perform-long-running-tasks-with-the-webhook-action-pattern"></a>Web kancası eylemi deseni ile uzun süre çalışan görevleri

Alternatif olarak, Web kancası desen, uzun soluklu görevlerin ve zaman uyumsuz işlenmesi için kullanabilirsiniz. Bu düzen bir "geri" için beklemesini mantıksal uygulama işleme iş akışı devam etmeden önce tamamlanması, API'sinden sahiptir. Bir olay meydana geldiğinde, bir URL'ye bir ileti gönderen bir HTTP POST aramasıdır. 

<a name="bakery-webhook-action"></a> Artık Web kancasını deseni önceki pastane benzerliği uygulayın ve pastane ve sipariş özel pasta için teslim çağırmanızı düşünün. Pasta yapma işlemi sürüyor ve pastane pastayı üzerinde çalışırken telefonda beklemek istemiyorsanız. Siparişinizi pastane onaylar, ancak pastayı tamamlandığında bunların çağırabilirsiniz bu kez, bunları telefon numaranızı sağlar. Bu kez, pastane siparişinizi ne zaman hazır ve sunar, pasta söyler.

Biz bu Web kancası düzeni yeniden eşlediğinizde, Logic Apps altyapısı, pasta müşteri temsil ederken pastane özel API'nizi temsil eder. Altyapısı isteğiyle bir API çağırır ve bir "geri" URL içerir.
İş tamamlandığında, API'nizi altyapısı bildirir ve ardından iş akışı devam eder, mantıksal uygulama için veri döndürmek için URL'yi kullanır. 

Bu düzen için iki uç nokta denetleyicinizde ayarlayın: `subscribe` ve `unsubscribe`

*  `subscribe` Uç noktası: Yürütme iş akışı eylemi, API'NİZİN ulaştığında çağrıları Logic Apps altyapısı `subscribe` uç noktası. Bu adım, mantıksal uygulamanın API'nizi depolayan bir geri çağırma URL'si oluşturun ve ardından iş tamamlandığında, API'nizi geri çağırmadan bekleyin neden olur. API'nizi sonra URL'sine HTTP POST ile geri çağırır ve mantıksal uygulama için giriş olarak herhangi bir üst bilgileri ve döndürülen içeriğin geçirir.

* `unsubscribe` Uç noktası: Mantıksal uygulama çalıştırması iptal edilirse, çağrıları Logic Apps altyapısı `unsubscribe` uç noktası. API'nizi geri çağırma URL'si kaydını ve gereken tüm işlemleri durdur.

![Web kancası eylemi deseni](./media/logic-apps-create-api-app/custom-api-webhook-action-pattern.png)

> [!NOTE]
> Şu anda, mantıksal Uygulama Tasarımcısı Swagger aracılığıyla bulan Web kancası uç noktalarını desteklemiyor. Bu düzen için eklemeniz gerekir. Bu nedenle bir [ **Web kancası** eylem](../connectors/connectors-native-webhook.md) ve URL üst bilgileri ve isteğinizin gövdesi belirtin. Ayrıca bkz: [iş akışı eylemleri ve Tetikleyicileri](logic-apps-workflow-actions-triggers.md#apiconnection-webhook-action). Geri arama URL'si geçirmek için kullanabileceğiniz `@listCallbackUrl()` gerektiği şekilde herhangi bir önceki alanları iş akışı işlevi.

> [!TIP]
> Örnek Web kancası düzeni için bu gözden [GitHub Web kancası tetikleyici örnek](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

<a name="triggers"></a>

## <a name="trigger-patterns"></a>Tetikleyici desenleri

Özel API'nizi olarak davranıp bir [ *tetikleyici* ](./logic-apps-overview.md#logic-app-concepts) yeni veriler veya bir olay belirtilen bir koşulu karşıladığında bir mantıksal uygulama başlar. Bu tetikleyiciyi ya da düzenli olarak kontrol edin veya bekleyip, yeni verileri veya olay, hizmet uç noktasında dinler. Belirtilen koşulu karşılıyorsa, yeni verileri veya bir olay tetiklenir ve bu tetikleyiciyi dinleyen mantıksal uygulama başlatılır. Logic apps bu şekilde başlatmak için API'NİZİN izleyebilirsiniz [ *yoklama tetikleyici* ](#polling-triggers) veya [ *Web kancası tetikleyici* ](#webhook-triggers) deseni. Bu desenleri için karşılıkları benzerdir [yoklama eylemi](#async-pattern) ve [Web kancası eylemleri](#webhook-actions). Ayrıca, daha fazla bilgi edinin [Tetikleyiciler için kullanım ölçümü](logic-apps-pricing.md).

<a name="polling-triggers"></a>

### <a name="check-for-new-data-or-events-regularly-with-the-polling-trigger-pattern"></a>Yeni veri ya da düzenli aralıklarla yoklama tetikleyici desen ile olayları denetle

A *yoklama tetikleyici* gibi davranan [eylem yoklama](#async-pattern) bu konuda daha önce açıklanan. Logic Apps altyapısı, düzenli aralıklarla çağırır ve tetikleyici uç nokta yeni verileri veya olay için denetler. Yeni veri ya da belirtilen koşulunu sağlayan bir olay altyapısı bulursa, tetikleyici. Sonra altyapı, girdi verilerini işleyen bir mantıksal uygulama örneği oluşturur. 

![Tetikleyici deseni yoklama](./media/logic-apps-create-api-app/custom-api-polling-trigger-pattern.png)

> [!NOTE]
> Her yoklama isteği, bile hiçbir mantıksal uygulama örneği oluşturulduğunda bir eylem yürütme olarak sayar. Birden çok kez aynı veri işlemeyi engellemek için tetikleyici zaten okuyan ve mantıksal Uygulama'ya geçirilen verileri temizlemeniz gerekir.

API'nin açısından açıklanan yoklama tetikleyici, belirli adımlar şunlardır:

| Yeni verileri veya olay bulundu?  | API yanıtı | 
| ------------------------- | ------------ |
| Bulundu | Bir HTTP dönüş `200 OK` durumu ile yanıt yükünde (sonraki adım için giriş). <br/>Bu yanıt, bir mantıksal uygulama örneği oluşturur ve iş akışı başlatır. | 
| Bulunamadı | Bir HTTP dönüş `202 ACCEPTED` durumu ile bir `location` başlığı ve bir `retry-after` başlığı. <br/>Tetikleyici, `location` üstbilgi de içermelidir bir `triggerState` , genellikle bir "zaman damgası." sorgu parametresi API'nizi bu tanımlayıcı, mantıksal uygulama tetiklendi son süreyi izlemek için kullanabilirsiniz. | 
||| 

Örneğin, hizmetiniz için yeni dosyalar düzenli aralıklarla kontrol etmek için bu davranışlara sahip bir yoklama tetikleyici oluşturabilir:

| İstek içerir `triggerState`? | API yanıtı | 
| -------------------------------- | -------------| 
| Hayır | Bir HTTP dönüş `202 ACCEPTED` durumu yanı sıra `location` üstbilgiyle `triggerState` geçerli zamana Ayarla ve `retry-after` aralık 15 saniye. | 
| Evet | Hizmetiniz sonra eklenen dosyalar için denetleme `DateTime` için `triggerState`. | 
||| 

| Bulunan dosyaların sayısı | API yanıtı | 
| --------------------- | -------------| 
| Tek bir dosya | Bir HTTP dönüş `200 OK` güncelleştirme durumu ve içerik yükü `triggerState` için `DateTime` döndürülen dosyası ve kümesi `retry-after` aralık 15 saniye. | 
| Birden çok dosya | Bir dosyayı bir saat ve bir HTTP dönüş `200 OK` durumu, güncelleştirme `triggerState`ve `retry-after` 0 saniyeye aralığı. </br>Daha fazla veri bulunduğunu ve altyapı hemen URL'den istek verilerini bilmeniz altyapısı adımları izin `location` başlığı. | 
| Dosya yok | Bir HTTP dönüş `202 ACCEPTED` durumu değişmez `triggerState`ve `retry-after` aralık 15 saniye. | 
||| 

> [!TIP]
> Örneğin bu gözden tetikleyici desen, yoklama [yoklama tetikleyici denetleyici örneği github'daki](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/PollTriggerController.cs).

<a name="webhook-triggers"></a>

### <a name="wait-and-listen-for-new-data-or-events-with-the-webhook-trigger-pattern"></a>Bekleyin ve yeni verileri veya Web kancası tetikleyici desen ile olayları dinleme

Bir Web kancası tetikleyici bir *anında iletme tetikleyici* bekler ve yeni veri ya da, hizmet uç noktasında olayları dinler. Belirtilen koşulu karşılıyorsa, yeni verileri veya bir olay tetiklenir ve ardından verileri girdi olarak işler bir mantıksal uygulama örneği oluşturur.
Web kancası Tetikleyicileri gibi davranacak [Web kancası eylemleri](#webhook-actions) daha önce bu konuda açıklanan ve ile ayarlayın `subscribe` ve `unsubscribe` uç noktaları. 

* `subscribe` Uç noktası: Ekleme ve bir Web kancası tetikleyici mantıksal uygulamanızı kaydedin, çağrıları Logic Apps altyapısı `subscribe` uç noktası. Bu adım, API'nizi depolayan bir geri çağırma URL'si oluşturmak mantıksal uygulama neden olur. Yeni veri ya da belirtilen koşulu karşılayan bir olay olduğunda API'NİZİN URL'sine HTTP POST ile geri çağırır. Üst bilgiler ve içerik yükü mantıksal uygulama için giriş olarak geçirin.

* `unsubscribe` Uç noktası: Web kancası tetikleyicisine veya tüm mantıksal uygulama silinirse, çağrıları Logic Apps altyapısı `unsubscribe` uç noktası. API'nizi geri çağırma URL'si kaydını ve gereken tüm işlemleri durdur.

![Web kancası tetikleyici deseni](./media/logic-apps-create-api-app/custom-api-webhook-trigger-pattern.png)

> [!NOTE]
> Şu anda, mantıksal Uygulama Tasarımcısı Swagger aracılığıyla bulan Web kancası uç noktalarını desteklemiyor. Bu düzen için eklemeniz gerekir. Bu nedenle bir [ **Web kancası** tetikleyici](../connectors/connectors-native-webhook.md) ve URL üst bilgileri ve isteğinizin gövdesi belirtin. Ayrıca bkz: [HTTPWebhook tetikleyici](logic-apps-workflow-actions-triggers.md#httpwebhook-trigger). Geri arama URL'si geçirmek için kullanabileceğiniz `@listCallbackUrl()` gerektiği şekilde herhangi bir önceki alanları iş akışı işlevi.
>
> Birden çok kez aynı veri işlemeyi engellemek için tetikleyici zaten okuyan ve mantıksal Uygulama'ya geçirilen verileri temizlemeniz gerekir.

> [!TIP]
> Örnek Web kancası düzeni için bu gözden [github Web kancası tetikleyici denetleyici örneği](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

## <a name="secure-calls-to-your-apis-from-logic-apps"></a>Mantıksal uygulamalardan Apı'lerinizi güvenli çağrılar

Böylece güvenli bir şekilde mantıksal uygulamalardan arayabileceğiniz özel Apı'lerinizi oluşturduktan sonra kimlik doğrulaması API'leri için ayarlayın. Bilgi [mantıksal uygulamalardan özel API'lere giden çağrıların güvenliğini sağlama](../logic-apps/logic-apps-custom-api-authentication.md).

## <a name="deploy-and-call-your-apis"></a>API'leri dağıtma ve çağırma

Kimlik doğrulamayı ayarladıktan sonra dağıtım Apı'leriniz için ayarlayın. Bilgi [dağıtmak ve logic apps'ten özel API'leri çağırmak nasıl](../logic-apps/logic-apps-custom-api-host-deploy-call.md).

## <a name="publish-custom-apis-to-azure"></a>Özel API'ler Azure'a yayımlama

Özel Apı'lerinizi diğer Azure Logic Apps kullanıcılar için kullanılabilir yapmak için güvenlik özellikleri ekleyebilir ve bunları mantıksal uygulama bağlayıcı olarak kaydedin. Daha fazla bilgi için bkz. [Özel bağlayıcılara genel bakış](../logic-apps/custom-connector-overview.md). 

Özel Apı'lerinizi Logic Apps, Microsoft Flow ve Microsoft Powerapps'teki tüm kullanıcılar için kullanılabilir kılmak için güvenlik özellikleri ekleyebilir, Apı'lerinizi mantıksal uygulama bağlayıcı olarak kaydedin ve gerekir bağlayıcılarınızı için aday gösterin [Microsoft Azure sertifikası programı](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/). 

## <a name="get-support"></a>Destek alın

* Özel API'ler ile ilgili belirli Yardım almak için başvurun [ customapishelp@microsoft.com ](mailto:customapishelp@microsoft.com).

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

* Logic Apps’in geliştirilmesine yardımcı olmak için, [Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın. 

## <a name="next-steps"></a>Sonraki adımlar

* [Hataları ve özel durumları işleme](../logic-apps/logic-apps-exception-handling.md)
* [Çağrı, tetikleyici veya iç içe mantıksal uygulamalar ile HTTP uç noktaları](../logic-apps/logic-apps-http-endpoint.md)
* [Eylemler ve Tetikleyiciler için kullanım ölçümü](../logic-apps/logic-apps-pricing.md)
