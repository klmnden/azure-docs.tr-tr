---
title: Ekleme ve Azure Logic Apps ile Azure işlevleri'nde kod çalıştırma
description: Ekleme ve Azure Logic Apps ile Azure işlevleri'nde kod çalıştırma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.topic: article
ms.date: 08/20/2018
ms.reviewer: klam, LADocs
ms.openlocfilehash: 9b304f2d4d2e498701be5977decf202cb0fa995b
ms.sourcegitcommit: d73c46af1465c7fd879b5a97ddc45c38ec3f5c0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65922060"
---
# <a name="add-and-run-code-by-using-azure-functions-in-azure-logic-apps"></a>Ekleme ve Azure Logic Apps Azure işlevleri'ni kullanarak kod çalıştırma

Logic apps içinde belirli bir işi yapan kodu çalıştırmak istediğinizde, kendi işlevleri ile oluşturabileceğiniz [Azure işlevleri](../azure-functions/functions-overview.md). Bu hizmet, Node.js, oluşturmanıza yardımcı olur. C#, ve F# bütün bir uygulama veya kodunuzu çalıştırmak için altyapı oluşturmak zorunda kalmamak için kod. Ayrıca [logic apps'ten çağrı Azure işlevleri içindeki](#call-logic-app).
Azure işlevleri, sunucusuz bilgi işlem bulutta sağlar ve aşağıdaki örnekte olduğu gibi görevleri gerçekleştirmek için yararlıdır:

* Node.js veya C# işlevleri ile mantıksal uygulamanızın davranışını genişletin.
* Mantıksal uygulama iş akışınızı hesaplamalar gerçekleştirin.
* Gelişmiş biçimlendirme uygulamak veya logic apps alanları işlem.

Azure işlevleri'ni oluşturmadan kod parçacıklarını çalıştırmak için bilgi nasıl [ekleme ve satır içi kod çalıştırma](../logic-apps/logic-apps-add-run-inline-code.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Azure işlevleri ve Azure işleviniz için bir kapsayıcı bir Azure işlev uygulaması. Bir işlev uygulaması yoksa [ilk işlev uygulamanızı oluşturmak](../azure-functions/functions-create-first-azure-function.md). İşlevinizi ya da sonra oluşturma [ayrı ayrı mantıksal uygulamanızı dışında](#create-function-external), veya [gelen mantıksal uygulamanız içinde](#create-function-designer) mantıksal Uygulama Tasarımcısı'nda.

  Mevcut ve yeni işlev uygulaması ve işlevleri, logic apps ile çalışmak için aynı gereksinimleri vardır:

  * İşlev uygulamanızın mantıksal uygulamanızı aynı Azure aboneliğinde olması gerekir.

  * İşlevinize HTTP tetikleyicisi, örneğin kullanır, **HTTP tetikleyicisi** işlev şablonu için **JavaScript** veya **C#**. 

    HTTP tetikleyici şablonu olan içeriği kabul edebilir `application/json` mantıksal uygulamanızdan türü. 
    Bir Azure işlevi mantıksal uygulamanıza eklediğinizde, mantıksal Uygulama Tasarımcısı, Azure aboneliğinizde bu şablondan oluşturulan özel işlevler gösterir. 

  * İşlevinizi tanımladınız sürece özel yollar kullanmayan bir [Openapı tanımı](../azure-functions/functions-openapi-definition.md), eski adıyla bir [Swagger dosyası](https://swagger.io/). 
  
  * İşleviniz için bir Openapı tanımı tanımladıysanız, Logic Apps Tasarımcısı'nda, işlev parametreleri ile çalışmak için daha zengin bir deneyim sunar. Mantıksal uygulamanızı bulun ve erişim Openapı tanımlarıyla sahip işlevlerin önce [aşağıdaki adımları izleyerek işlev uygulamanızı ayarlama](#function-swagger).

* İşlev eklemek istediğiniz mantıksal uygulama da dahil olmak üzere bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantıksal uygulamanızı ilk adımı olarak 

  İşlevleri çalıştırmak üzere eylemleri eklemeden önce mantıksal uygulamanızın bir tetikleyici ile başlamalıdır.

  Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

> [!NOTE]
> Yuvalar (Önizleme) etkinleştirildiğinde işlevler ile logic apps tümleştirmesi çalışmaz.

<a name="create-function-external"></a>

## <a name="create-functions-outside-logic-apps"></a>İşlevleri dış mantıksal uygulamalar oluşturma

İçinde [Azure portalında](https://portal.azure.com), mantıksal uygulamanızı aynı Azure aboneliğinde olması gerekir ve ardından Azure işlevinizi oluşturma, Azure işlev uygulaması oluşturma.
Azure işlevleri'ni oluşturmaya yeni başlıyorsanız öğrenin nasıl [Azure portalında ilk işlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md), ancak mantıksal uygulamalardan arayabileceğiniz işlevleri oluşturmak için bu gereksinimleri dikkate alın:

* Seçtiğinizden emin olun **HTTP tetikleyicisi** ya da işlev şablonu **JavaScript** veya **C#**.

  ![HTTP tetikleyicisi - JavaScript ya da C#](./media/logic-apps-azure-functions/http-trigger-function.png)

<a name="function-swagger"></a>

* İsteğe bağlı olarak, varsa, [bir API tanımı oluşturmayı](../azure-functions/functions-openapi-definition.md), eski adıyla bir [Swagger dosyası](https://swagger.io/), işleviniz için Logic Apps Tasarımcısı'nda işlev parametreleri ile çalışırken daha zengin bir deneyim elde edebilirsiniz. Mantıksal uygulamanızı bulun ve Swagger açıklamaları işlevleri kullanmak için işlev uygulamanızı ayarlamak için aşağıdaki adımları izleyin:

  1. İşlev uygulamanızı etkin bir şekilde çalıştığından emin olun.

  2. İşlev uygulamanız ayarlanan [çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) için aşağıdaki adımları izleyerek tüm kaynaklara izin verilir:

     1. Gelen **işlev uygulamaları** listesinde, işlev uygulamanızı seçin > **Platform özellikleri** > **CORS**.

        ![İşlev uygulamanızı seçin > "Platform özellikleri" > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors.png)

     2. Altında **CORS**, ekleme `*` joker karakter, ancak tüm diğer kaynakları listeden kaldırın ve seçin **Kaydet**.

        ![Ayarlama "CORS * joker karakteri için" * "](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

### <a name="access-property-values-inside-http-requests"></a>HTTP isteklerini içinde erişim özellik değerleri

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

2. İşlevi artık erişebilirsiniz `date` özelliği aracılığıyla `data` değişkeni ve DateTime türü özellik değerini DateString yazın çağırarak dönüştürme `ToDateString()` işlevi. İşlev ayrıca sonucunu döndürür `body` işlevin yanıt özelliği: 

   ```javascript
   body: data.date.ToDateString();
   ```

Azure işlevinizi oluşturduğunuza göre nasıl adımlarını izleyin [logic Apps'e işlevler eklemek](#add-function-logic-app).

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Logic apps içinde işlevler oluşturma

Mantıksal Uygulama Tasarımcısı'nda mantıksal uygulama içinde başlayarak bir Azure işlev oluşturabilmeniz için önce işlevleriniz için kapsayıcı olan bir Azure işlev uygulaması olmalıdır. Bir işlev uygulamasına sahip değilseniz, bu işlev uygulaması ilk oluşturun. Bkz: [Azure portalında ilk işlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md). 

1. İçinde [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın. 

2. Oluşturma ve işlevinizi eklemek için senaryonuz için geçerli adımı izleyin:

   * Mantıksal uygulamanızın iş akışında son adımı altında seçin **yeni adım**.

   * Mantıksal uygulamanızın iş akışında mevcut adımlar arasındaki okun üzerine fareyi hareket ettirin seçin artı (+) oturum açın ve ardından **Eylem Ekle**.

3. Arama kutusuna filtreniz olarak "azure işlevleri" girin.
Eylem listesinden şu eylemi seçin: **Azure işlevleri - Azure işlevi seçin** 

   !["Azure işlevleri" bulun](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. İşlev uygulamaları listesinden işlevi uygulamanızı seçin. Eylem listesini açıldıktan sonra şu eylemi seçin: **Azure işlevleri - yeni bir işlev oluşturma**

   ![İşlev uygulamanızı seçin](./media/logic-apps-azure-functions/select-function-app-create-function.png)

5. İşlev tanımı Düzenleyicisi'nde, işlevinizi tanımlayın:

   1. İçinde **işlev adı** kutusunda, işleviniz için bir ad sağlayın. 

   2. İçinde **kod** kutusunda, eklemek istediğiniz yük ve yanıt dahil, bir işlev şablonu kodunuza işlevinizi çalışmayı tamamladıktan sonra mantıksal uygulamanızı döndürdü. 

      ![İşlevinizi tanımlayın](./media/logic-apps-azure-functions/function-definition.png)

      Şablonun kodda  *`context` nesne* aracılığıyla, mantıksal uygulamanın gönderdiği ileti başvurduğu **istek gövdesi** sonraki bir adımda alan. 
      Erişim için `context` nesnenin özelliklerinden, işlevin içindeki bu sözdizimini kullanın: 

      `context.body.<property-name>`

      Örneğin, başvuru için `content` özelliği içinde `context` nesne, bu sözdizimini kullanın: 

      `context.body.content`

      Ayrıca şablon kodunu içeren bir `input` değerini depolayan değişken `data` işlevinizi değeri işlemleri gerçekleştirebilmesi için parametre. 
      JavaScript işlevleri içinde `data` değişkeni, ayrıca bir kısayol `context.body`.

      > [!NOTE]
      > `body` Özelliği burada uygulandığı `context` nesne ve aynı olmayan **gövdesi** eylem belirtecinden çıkış, hangi işlevinize de geçebilir. 
 
   3. İşiniz bittiğinde **Oluştur**’u seçin.

6. İçinde **istek gövdesi** kutusunda, işlevinizin giriş, JavaScript nesne gösterimi (JSON) nesnesi olarak biçimlendirilmelidir, sağlayın. 

   Bu giriş *bağlam nesnesi* veya mantıksal uygulamanızı işlevinize gönderen bir ileti. Tıkladığınızda **istek gövdesi** alan, dinamik içerik listesi görüntülenir önceki adımlardan bir çıkış belirteçleri seçebilmeniz. Bu örnek bağlamı yükü adlı bir özellik içerdiğini belirtir `content` olan **gelen** e-posta tetikleyicisi değerinden belirteci:

   !["İstek gövdesi" örnek - bağlam nesnesi yükü](./media/logic-apps-azure-functions/function-request-body-example.png)

   Burada, nesne içeriğini, doğrudan JSON yükü için eklenen için bağlam nesnesini bir dize olarak cast değil. Ancak, bağlam nesnesini bir dize, bir JSON nesnesi veya bir JSON dizisi gönderen bir JSON belirteç bulunmadığında hata alırsınız. Bu nedenle, bu örnekte kullanılan **alınma zamanı** belirteci bunun yerine, bağlam nesnesini bir dize olarak çift tırnak işareti eklenerek çevirebilirsiniz:  

   ![Dize olarak atama nesnesi](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

7. İstek üstbilgileri veya sorgu parametreleri kullanılacak yöntemi gibi diğer ayrıntılarını belirtmek için seçin **Gelişmiş Seçenekleri Göster**.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Logic Apps'e var olan işlevleri ekleyin

Mantıksal uygulamalarınızı mevcut Azure işlevleri çağırmak için Logic App Tasarımcısı'nda Azure işlevleri gibi diğer herhangi bir eylem ekleyebilirsiniz. 

1. İçinde [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın. 

2. İşlev eklemek istediğiniz adımı altında seçin **yeni adım** > **Eylem Ekle**. 

3. Arama kutusuna filtreniz olarak "azure işlevleri" girin.
Eylem listesinden şu eylemi seçin: **Azure işlevleri - Azure işlevi seçin** 

   !["Azure işlevleri" bulun](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. İşlev uygulamaları listesinden işlevi uygulamanızı seçin. İşlevler listesi göründükten sonra işlevinizi seçin. 

   ![İşlev uygulaması ve Azure işlevi seçin](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   API tanımı (Swagger açıklamaları) varsa ve işlevler [mantıksal uygulamanızı bulabilir ve bu işlevlere erişmek için ayarlanan](#function-swagger), seçebileceğiniz **Swagger eylemleri**:

   ![İşlev uygulaması, "Swagger eylemleri" seçin "ve Azure işlevinizin](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

5. İçinde **istek gövdesi** kutusunda, işlevinizin giriş, JavaScript nesne gösterimi (JSON) nesnesi olarak biçimlendirilmelidir, sağlayın. 

   Bu giriş *bağlam nesnesi* veya mantıksal uygulamanızı işlevinize gönderen bir ileti. Tıkladığınızda **istek gövdesi** alan, dinamik içerik listesi görüntülenir önceki adımlardan bir çıkış belirteçleri seçebilmeniz. Bu örnek bağlamı yükü adlı bir özellik içerdiğini belirtir `content` olan **gelen** e-posta tetikleyicisi değerinden belirteci:

   !["İstek gövdesi" örnek - bağlam nesnesi yükü](./media/logic-apps-azure-functions/function-request-body-example.png)

   Burada, nesne içeriğini, doğrudan JSON yükü için eklenen için bağlam nesnesini bir dize olarak cast değil. Ancak, bağlam nesnesini bir dize, bir JSON nesnesi veya bir JSON dizisi gönderen bir JSON belirteç bulunmadığında hata alırsınız. Bu nedenle, bu örnekte kullanılan **alınma zamanı** belirteci bunun yerine, bağlam nesnesini bir dize olarak çift tırnak işareti eklenerek çevirebilirsiniz: 

   ![Dize olarak atama nesnesi](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

6. İstek üstbilgileri veya sorgu parametreleri kullanılacak yöntemi gibi diğer ayrıntılarını belirtmek için seçin **Gelişmiş Seçenekleri Göster**.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-functions"></a>Mantıksal uygulamaları çağırma işlevleri

Bir Azure işlevi içinde bir mantıksal uygulamadan tetiklemek istediğinizde, mantıksal uygulama bir çağrılabilir uç noktası sağlayan bir tetikleyici ile başlamalıdır. Örneğin, mantıksal uygulama ile başlayabilirsiniz **HTTP**, **istek**, **Azure kuyrukları**, veya **Event Grid** tetikleyici. İçinde işlevinizi tetikleyicinin URL'sine HTTP POST isteği gönderin ve istediğiniz işlemek için bu mantıksal uygulama yükü içerir. Daha fazla bilgi için [çağrı, tetikleyici veya iç içe mantıksal uygulamalar](../logic-apps/logic-apps-http-endpoint.md). 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
