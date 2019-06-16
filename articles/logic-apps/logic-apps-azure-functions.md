---
title: Ekleme ve Azure Logic Apps'ten Azure işlevleri çağırma
description: Ekleme ve logic apps'ten Azure işlevleri çalıştırma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.topic: article
ms.date: 06/04/2019
ms.reviewer: klam, LADocs
ms.openlocfilehash: 524b927ec0966199c51cdee93e920d7b847139ae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66495085"
---
# <a name="call-azure-functions-from-azure-logic-apps"></a>Azure Logic Apps'ten Azure işlevleri çağırma

Logic apps içinde belirli bir işi yapan kodu çalıştırmak istediğinizde, kendi işlevi kullanarak oluşturabileceğiniz [Azure işlevleri](../azure-functions/functions-overview.md). Bu hizmet, Node.js, oluşturmanıza yardımcı olur. C#, ve F# tam uygulama veya kod çalıştırmak için altyapı oluşturmak zorunda kalmamak için çalışır. Ayrıca [logic apps'ten çağrı Azure işlevleri içindeki](#call-logic-app). Azure işlevleri, sunucusuz bilgi işlem bulutta sağlar ve aşağıdaki örnekte olduğu gibi görevleri gerçekleştirmek için yararlıdır:

* Node.js veya C# işlevleri ile mantıksal uygulamanızın davranışını genişletin.
* Mantıksal uygulama iş akışınızı hesaplamalar gerçekleştirin.
* Gelişmiş biçimlendirme uygulamak veya logic apps alanları işlem.

Azure işlevleri'ni oluşturmadan kod parçacıklarını çalıştırmak için bilgi nasıl [ekleme ve satır içi kod çalıştırma](../logic-apps/logic-apps-add-run-inline-code.md).

> [!NOTE]
> Logic Apps ve Azure işlevleri arasında tümleştirme şu anda etkin yuvaları ile çalışmaz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Azure işlevleri, birlikte Azure işleviniz için bir kapsayıcı bir Azure işlev uygulaması. Bir işlev uygulaması yoksa [ilk işlev uygulamanızı oluşturmak](../azure-functions/functions-create-first-azure-function.md). Daha sonra işlevinizi mantıksal uygulamanızı dışında ya da Azure portalında oluşturabilirsiniz veya [öğesinden sonra mantıksal uygulamanızın içinde](#create-function-designer) mantıksal Uygulama Tasarımcısı'nda.

* Mevcut veya yeni olup olmadığını logic apps ile çalışırken aynı gereksinimleri işlev uygulaması ve işlevleri için geçerlidir:

  * İşlev uygulaması ve mantıksal uygulama, aynı Azure aboneliği kullanmanız gerekir.

  * Yeni işlev uygulamaları .NET veya JavaScript çalışma zamanı yığını olarak kullanmanız gerekir. Var olan işlev uygulamaları için yeni bir işlev eklediğinizde, seçebilirsiniz C# veya JavaScript.

  * İşlevinizi kullanan **HTTP tetikleyicisi** şablonu.

    HTTP tetikleyici şablonu olan içeriği kabul edebilir `application/json` mantıksal uygulamanızdan türü. Bir Azure işlevi mantıksal uygulamanıza eklediğinizde, mantıksal Uygulama Tasarımcısı, Azure aboneliğinizde bu şablondan oluşturulan özel işlevler gösterir.

  * İşlevinizi tanımladınız sürece özel yollar kullanmayan bir [Openapı tanımı](../azure-functions/functions-openapi-definition.md) (eski adıyla bir [Swagger dosyası](https://swagger.io/)).

  * İşleviniz için bir Openapı tanımı varsa, Logic Apps Tasarımcısı'nda daha zengin bir size zaman deneyimi iş işlevi parametrelere sahip. Mantıksal uygulamanızı bulun ve erişim Openapı tanımlarıyla sahip işlevlerin önce [aşağıdaki adımları izleyerek işlev uygulamanızı ayarlama](#function-swagger).

* İşlev eklemek istediğiniz mantıksal uygulama da dahil olmak üzere bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantıksal uygulamanızı ilk adımı olarak

  İşlevleri çalıştırma eylemleri eklemeden önce mantıksal uygulamanızın bir tetikleyici ile başlamalıdır. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="function-swagger"></a>

## <a name="find-functions-that-have-openapi-descriptions"></a>Openapı açıklamaları işlevleri Bul

Logic Apps Tasarımcısı'nda işlev parametreleri ile çalışırken daha zengin bir deneyim [Openapı tanımı oluşturma](../azure-functions/functions-openapi-definition.md), eski adıyla bir [Swagger dosyası](https://swagger.io/), işleviniz için. Mantıksal uygulamanızı bulun ve Swagger açıklamaları işlevleri kullanmak için işlev uygulamanızı ayarlamak için aşağıdaki adımları izleyin:

1. İşlev uygulamanızı etkin bir şekilde çalıştığından emin olun.

1. İşlev uygulamanız ayarlanan [çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) böylece tüm kaynaklar, aşağıdaki adımları izleyerek izin verilir:

   1. Gelen **işlev uygulamaları** listesinde, işlev uygulamanızı seçin. Sağ bölmede seçin **Platform özellikleri** > **CORS**.

      ![İşlev uygulamanızı seçin > "Platform özellikleri" > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors.png)

   1. Altında **CORS**, yıldız işareti ekleyin ( **`*`** ) joker karakter, ancak tüm diğer kaynakları listeden kaldırın ve seçin **Kaydet**.

      ![Ayarlama "CORS * joker karakteri için" * "](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

## <a name="access-property-values-inside-http-requests"></a>HTTP isteklerini içinde erişim özellik değerleri

Web kancası İşlevler, girdi olarak HTTP isteklerini kabul etmek ve diğer işlevler için bu istekleri geçirin. Örneğin, Logic Apps olsa da [DateTime değerlerini dönüştürme işlevleri](../logic-apps/workflow-definition-language-functions-reference.md), bu temel örnek JavaScript işlevi, işleve geçirilen ve üzerinde işlem yapabileceğiniz bir istek nesnesi içinde bir özelliği nasıl erişebileceğiniz gösterir. Bu özellik değeri. Nesnelerin özelliklerine erişmek için bu örnekte [nokta (.) işleci](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Property_accessors):

```javascript
function convertToDateString(request, response){
   var data = request.body;
   response = {
      body: data.date.ToDateString();
   }
}
```

Bu işlev içinde neler aşağıda verilmiştir:

1. İşlev oluşturur bir `data` değişkeni ve atar `body` içinde nesne `request` Bu değişken için nesne. İşlev başvurmak için nokta (.) işlecini kullanır `body` içinde nesne `request` nesnesi:

   ```javascript
   var data = request.body;
   ```

1. İşlevi artık erişebilirsiniz `date` özelliği aracılığıyla `data` değişkeni ve DateTime türü özellik değerini DateString yazın çağırarak dönüştürme `ToDateString()` işlevi. İşlev ayrıca sonucunu döndürür `body` işlevin yanıt özelliği:

   ```javascript
   body: data.date.ToDateString();
   ```

Azure işlevinizi oluşturduğunuza göre nasıl adımlarını izleyin [logic Apps'e işlevler eklemek](#add-function-logic-app).

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Logic apps içinde işlevler oluşturma

Mantıksal uygulamanız içinde Logic Apps Tasarımcısı'nı kullanarak başlayarak bir Azure işlev oluşturabilmeniz için önce işlevleriniz için kapsayıcı olan bir Azure işlev uygulaması olmalıdır. Bir işlev uygulamasına sahip değilseniz, bu işlev uygulaması ilk oluşturun. Bkz: [Azure portalında ilk işlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md).

1. İçinde [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın.

1. Oluşturma ve işlevinizi eklemek için senaryonuz için geçerli adımı izleyin:

   * Mantıksal uygulamanızın iş akışında son adımı altında seçin **yeni adım**.

   * Mantıksal uygulamanızın iş akışında mevcut adımlar arasındaki okun üzerine fareyi hareket ettirin seçin artı (+) oturum açın ve ardından **Eylem Ekle**.

1. Arama kutusuna filtreniz olarak "azure işlevleri" girin. Eylem listesinden şu eylemi seçin: **Azure işlevleri - Azure işlevi seçin**

   !["Azure işlevleri" bulun](./media/logic-apps-azure-functions/find-azure-functions-action.png)

1. İşlev uygulamaları listesinden işlevi uygulamanızı seçin. Eylem listesini açıldıktan sonra şu eylemi seçin: **Azure işlevleri - yeni bir işlev oluşturma**

   ![İşlev uygulamanızı seçin](./media/logic-apps-azure-functions/select-function-app-create-function.png)

1. İşlev tanımı Düzenleyicisi'nde, işlevinizi tanımlayın:

   1. İçinde **işlev adı** kutusunda, işleviniz için bir ad sağlayın.

   1. İçinde **kod** kutusunda, eklemek istediğiniz yük ve yanıt dahil, bir işlev şablonu kodunuza işlevinizi çalışmayı tamamladıktan sonra mantıksal uygulamanızı döndürdü.

      ![İşlevinizi tanımlayın](./media/logic-apps-azure-functions/function-definition.png)

      Şablonun kodda  *`context` nesne* aracılığıyla, mantıksal uygulamanın gönderdiği ileti başvurduğu **istek gövdesi** sonraki bir adımda alan. Erişim için `context` nesnenin özelliklerinden, işlevin içindeki bu sözdizimini kullanın:

      `context.body.<property-name>`

      Örneğin, başvuru için `content` özelliği içinde `context` nesne, bu sözdizimini kullanın: 

      `context.body.content`

      Ayrıca şablon kodunu içeren bir `input` değerini depolayan değişken `data` işlevinizi değeri işlemleri gerçekleştirebilmesi için parametre. JavaScript işlevleri içinde `data` değişkeni, ayrıca bir kısayol `context.body`.

      > [!NOTE]
      > `body` Özelliği burada uygulandığı `context` nesne ve aynı olmayan **gövdesi** eylem belirtecinden çıkış, hangi işlevinize de geçebilir.

   1. İşiniz bittiğinde **Oluştur**’u seçin.

1. İçinde **istek gövdesi** kutusunda, işlevinizin giriş, JavaScript nesne gösterimi (JSON) nesnesi olarak biçimlendirilmelidir, sağlayın.

   Bu giriş *bağlam nesnesi* veya mantıksal uygulamanızı işlevinize gönderen bir ileti. Tıkladığınızda **istek gövdesi** alan, dinamik içerik listesi görüntülenir önceki adımlardan bir çıkış belirteçleri seçebilmeniz. Bu örnek bağlamı yükü adlı bir özellik içerdiğini belirtir `content` olan **gelen** e-posta tetikleyicisi değerinden belirteci:

   !["İstek gövdesi" örnek - bağlam nesnesi yükü](./media/logic-apps-azure-functions/function-request-body-example.png)

   Burada, nesne içeriğini, doğrudan JSON yükü için eklenen için bağlam nesnesini bir dize olarak cast değil. Ancak, bağlam nesnesini bir dize, bir JSON nesnesi veya bir JSON dizisi gönderen bir JSON belirteç bulunmadığında hata alırsınız. Bu nedenle, bu örnekte kullanılan **alınma zamanı** belirteci bunun yerine, bağlam nesnesini bir dize olarak çift tırnak işareti eklenerek çevirebilirsiniz:  

   ![Dize olarak atama nesnesi](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

1. İstek üstbilgileri veya sorgu parametreleri kullanılacak yöntemi gibi diğer ayrıntılarını belirtmek için açık **yeni parametre Ekle** listesinde ve istediğiniz seçenekleri seçin.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Logic Apps'e var olan işlevleri ekleyin

Mantıksal uygulamalarınızı mevcut Azure işlevleri çağırmak için Logic App Tasarımcısı'nda Azure işlevleri gibi diğer herhangi bir eylem ekleyebilirsiniz.

1. İçinde [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın.

1. İşlev eklemek istediğiniz adımı altında seçin **yeni adım**seçip **Eylem Ekle**.

1. Arama kutusuna filtreniz olarak "azure işlevleri" girin. Eylem listesinden şu eylemi seçin: **Azure işlevleri - Azure işlevi seçin**

   !["Azure işlevleri" bulun](./media/logic-apps-azure-functions/find-azure-functions-action.png)

1. İşlev uygulamaları listesinden işlevi uygulamanızı seçin. İşlevler listesi göründükten sonra işlevinizi seçin.

   ![İşlev uygulaması ve Azure işlevi seçin](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   API tanımı (Swagger açıklamaları) varsa ve işlevler [mantıksal uygulamanızı bulabilir ve bu işlevlere erişmek için ayarlanan](#function-swagger), seçebileceğiniz **Swagger eylemleri**:

   ![İşlev uygulaması, "Swagger eylemleri" seçin "ve Azure işlevinizin](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

1. İçinde **istek gövdesi** kutusunda, işlevinizin giriş, JavaScript nesne gösterimi (JSON) nesnesi olarak biçimlendirilmelidir, sağlayın.

   Bu giriş *bağlam nesnesi* veya mantıksal uygulamanızı işlevinize gönderen bir ileti. Tıkladığınızda **istek gövdesi** alan, dinamik içerik listesinden görünür önceki adımlardan çıkış belirteçleri seçebilirsiniz. Bu örnek bağlamı yükü adlı bir özellik içerdiğini belirtir `content` olan **gelen** e-posta tetikleyicisi değerinden belirteci:

   !["İstek gövdesi" örnek - bağlam nesnesi yükü](./media/logic-apps-azure-functions/function-request-body-example.png)

   Burada, nesne içeriğini, doğrudan JSON yükü için eklenen için bağlam nesnesini bir dize olarak cast değil. Ancak, bağlam nesnesini bir dize, bir JSON nesnesi veya bir JSON dizisi gönderen bir JSON belirteç bulunmadığında hata alırsınız. Bu nedenle, bu örnekte kullanılan **alınma zamanı** belirteci bunun yerine, bağlam nesnesini bir dize olarak çift tırnak işareti eklenerek çevirebilirsiniz:

   ![Dize olarak atama nesnesi](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

1. İstek üstbilgileri veya sorgu parametreleri kullanılacak yöntemi gibi diğer ayrıntılarını belirtmek için açık **yeni parametre Ekle** listesinde ve istediğiniz seçenekleri seçin.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-azure-functions"></a>Mantıksal uygulamaları Azure işlevleri çağırma

Bir Azure işlevi içinde bir mantıksal uygulamadan tetiklemek istediğinizde, mantıksal uygulama bir çağrılabilir uç noktası sağlayan bir tetikleyici ile başlamalıdır. Örneğin, mantıksal uygulama ile başlayabilirsiniz **HTTP**, **istek**, **Azure kuyrukları**, veya **Event Grid** tetikleyici. İçinde işlevinizi tetikleyicinin URL'sine HTTP POST isteği gönderin ve istediğiniz işlemek için bu mantıksal uygulama yükü içerir. Daha fazla bilgi için [çağrı, tetikleyici veya iç içe mantıksal uygulamalar](../logic-apps/logic-apps-http-endpoint.md).

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
