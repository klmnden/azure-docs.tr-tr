---
title: Ekleme ve Azure işlevleri ile Azure Logic apps'te özel kod çalıştırma | Microsoft Docs
description: Azure işlevleri ile Azure Logic apps'te özel kod parçacıklarını çalıştırmak ve ekleme hakkında bilgi edinin
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/25/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 20ad738541554279ff9fd6dd6babe90a38676c00
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263199"
---
# <a name="add-and-run-custom-code-snippets-in-azure-logic-apps-with-azure-functions"></a>Ekleme ve Azure işlevleri ile Azure Logic apps'te özel kod parçacıkları çalıştırma

Oluşturmak istediğiniz zaman ve logic apps içinde belirli bir soruna yönelik yeterli kod çalıştırmak, kendi işlevleri kullanarak oluşturabileceğiniz [Azure işlevleri](../azure-functions/functions-overview.md). Bu hizmet oluşturmak ve tüm uygulama veya kodunuzu çalıştırmak için bir altyapı oluşturma hakkında endişelenmeden logic apps Node.js veya C# ile yazılan özel kod parçacıklarını çalıştırmak yeteneği sağlar. Azure işlevleri, sunucusuz bulut bilgi işlem sağlar ve aşağıdaki örnekte olduğu gibi görevleri gerçekleştirmek için yararlıdır:

* Mantıksal uygulamanızın davranışını, Node.js veya C# tarafından desteklenen işlevleri ile genişletin.
* Mantıksal uygulama iş akışınızı hesaplamalar gerçekleştirin.
* Gelişmiş biçimlendirme uygulamak veya logic apps alanları işlem.

Ayrıca [logic apps'ten çağrı Azure işlevleri içindeki](#call-logic-app).

## <a name="prerequisites"></a>Önkoşullar

Bu makalede takip etmek için gereksinim duyduğunuz öğeleri şunlardır:

* Henüz Azure aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* İşlev eklemek istediğiniz mantıksal uygulama

  Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* A [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantıksal uygulamanızı ilk adımı olarak 

  İşlevleri çalıştırmak için Eylemler eklemeden önce mantıksal uygulamanızın bir tetikleyici ile başlamalıdır.

* Azure işlevleri ve Azure işleviniz için bir kapsayıcı bir Azure işlev uygulaması. Bir işlev uygulaması yoksa, şunları yapmalısınız [ilk işlev uygulamanızı oluşturmak](../azure-functions/functions-create-first-azure-function.md). İşlevinizi ya da sonra oluşturma [ayrı ayrı mantıksal uygulamanızı dışında](#create-function-external), veya [gelen mantıksal uygulamanız içinde](#create-function-designer) mantıksal Uygulama Tasarımcısı'nda.

  Yeni ve mevcut Azure işlev uygulamaları hem de işlevleri, logic apps ile çalışmak için aynı gereksinimleri vardır:

  * İşlev uygulamanızın mantıksal uygulamanızı Azure aboneliğine ait olmalıdır.

  * İşlevinizi kullanmalısınız **Genel Web kancası** ya da işlev şablonu **JavaScript** veya **C#**. Bu şablon olan içeriği kabul edebilir `application/json` mantıksal uygulamanızdan türü. Bu şablonlar, bulmak ve görüntülemek için logic apps bu işlevleri eklediğinizde, bu şablonları ile oluşturduğunuz özel işlevler mantıksal Uygulama Tasarımcısı da yardımcı olur.

  * Bu maddeyi işlevi şablonun **modu** özelliği **Web kancası** ve **Web kancası türü** özelliği **genel JSON**.

    1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a>’da oturum açın.
    2. Ana Azure menüsünde **işlev uygulamaları**. 
    3. İçinde **işlev uygulamaları** listesinde, işlev uygulamanızı seçin, işlevinizi genişletin ve seçin **tümleştir**. 
    4. Denetleme, şablonun **modu** özelliği **Web kancası** ve **Web kancası türü** özelliği **genel JSON**. 

  * İşleviniz varsa bir [API tanımı](../azure-functions/functions-openapi-definition.md), eski adıyla bir [Swagger dosyası](http://swagger.io/), Logic Apps Tasarımcısı'nda işlev parametrelerini çalışmak için daha zengin bir deneyim sunar. 
  Mantıksal uygulamanızı bulun ve erişim Swagger açıklamaları olan işlevler önce [aşağıdaki adımları izleyerek işlev uygulamanızı ayarlama](#function-swagger).

<a name="create-function-external"></a>

## <a name="create-functions-outside-logic-apps"></a>İşlevleri dış mantıksal uygulamalar oluşturma

İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, mantıksal uygulamanızı aynı Azure aboneliğinde olması gerekir ve ardından Azure işlevinizi oluşturma, Azure işlev uygulaması oluşturma. Azure işlevleri'ne yeniyseniz, bilgi nasıl [Azure portalında ilk işlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md), ancak bu gereksinimler ekleyebilir ve çağrı logic apps'ten Azure işlevleri'ni oluşturmak için dikkat edin.

* Seçtiğinizden emin olun **Genel Web kancası** ya da işlev şablonu **JavaScript** veya **C#**.

  ![Genel Web kancası - JavaScript ya da C#](./media/logic-apps-azure-functions/generic-webhook.png)

* Azure işlevinizi oluşturduktan sonra bu maddeyi şablonun **modu** ve **Web kancası türü** özellikleri doğru şekilde ayarlanır.

  1. İçinde **işlev uygulamaları** listesinde, işlevinizi genişletin ve seçin **tümleştir**. 

  2. Denetleyin, şablonun **modu** özelliği **Web kancası** ve **Web kancası türü** özelliği **genel JSON**. 

     ![İşlev şablonunun "Tümleştirme" özellikleri](./media/logic-apps-azure-functions/function-integrate-properties.png)

<a name="function-swagger"></a>

* İsteğe bağlı olarak, varsa, [bir API tanımı oluşturmayı](../azure-functions/functions-openapi-definition.md), eski adıyla bir [Swagger dosyası](http://swagger.io/), işleviniz için Logic Apps Tasarımcısı'nda işlev parametreleri ile çalışırken daha zengin bir deneyim elde edebilirsiniz. Mantıksal uygulamanızı bulun ve Swagger açıklamaları olan işlevler erişmek için işlev uygulamanızı ayarlamak için:

  * İşlev uygulamanızı etkin bir şekilde çalıştığından emin olun.

  * İşlev uygulamanız ayarlanan [çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) tüm kaynaklara izin verilen şekilde:

    1. Başlangıç **işlev uygulamaları** listesinde, işlev uygulamanızı seçin > **Platform özellikleri** > **CORS**.

       ![İşlev uygulamanızı seçin > "Platform özellikleri" > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors.png)

    2. Altında **CORS**, ekleme `*` joker karakter, ancak tüm diğer kaynakları listeden kaldırın ve seçin **Kaydet**.

       ![İşlev uygulamanızı seçin > "Platform özellikleri" > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

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

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın. 

2. Oluşturma ve işlev eklemek istediğiniz adımı altında seçin **yeni adım** > **Eylem Ekle**. 

3. Arama kutusuna filtreniz olarak "azure işlevleri" girin.
Eylem listesinden şu eylemi seçin: **Azure işlevleri - Azure işlevi seçin** 

   !["Azure işlevleri" bulun](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. İşlev uygulamaları listesinden işlevi uygulamanızı seçin. Eylemleri açılır liste sonra şu eylemi seçin: **Azure işlevleri - yeni işlev oluşturma**

   ![İşlev uygulamanızı seçin](./media/logic-apps-azure-functions/select-function-app-create-function.png)

5. İşlev tanımı Düzenleyicisi'nde, işlevinizi tanımlayın:

   1. İçinde **işlev adı** kutusunda, işleviniz için bir ad sağlayın. 

   2. İçinde **kod** kutusunda, eklemek istediğiniz yük ve yanıt dahil olmak üzere bu şablon, işlev kodunuzun işlevinizi çalışmayı tamamladıktan sonra mantıksal uygulamanıza döndürdü. 
   Şablon kod bağlamı nesneye ileti ve mantıksal uygulamanızı işlevinize, örneğin geçirir içerik açıklar:

      ![İşlevinizi tanımlayın](./media/logic-apps-azure-functions/function-definition.png)

      İşlevinizi içinde bu söz dizimini kullanarak bağlam nesnesi özelliklerinde başvurabilirsiniz:

      ```text
      context.<token-name>.<property-name>
      ```
      Bu örnekte, kullanacağınız sözdizimi aşağıdadır:

      ```text
      context.body.content
      ```

   3. İşiniz bittiğinde **Oluştur**’u seçin.

6. İçinde **istek gövdesi** kutusunda, hangi biçimlendirilmelidir işlevinizin giriş olarak geçirmek için bağlam nesnesini JavaScript nesne gösterimi (JSON) belirtin. Tıkladığınızda **istek gövdesi** kutusu, dinamik içerik listesi açılır belirteçleri kullanılabilir özellikleri için önceki adımlardan seçebilmeniz için. 

   Bu örnekte nesnesinde geçirir **gövdesi** e-posta tetikleyici belirteç:  

   !["İstek gövdesi" örnek - bağlam nesnesi yükü](./media/logic-apps-azure-functions/function-request-body-example.png)

   Bağlam nesnesini içeriği bağlı olarak, mantıksal Uygulama Tasarımcısı sonra satır içi düzenleme yapabileceğiniz bir işlev şablonu oluşturur. 
   Logic Apps, giriş bağlam nesnesini temel alan değişkenleri de oluşturur.

   İçeriği doğrudan için JSON yükü eklendikten şekilde bu örnekte, bir dize olarak cast bağlam nesnesi değil. 
   Ancak, nesne bir dize, bir JSON nesnesi veya bir JSON dizisi olmalıdır, bir JSON belirteci değilse bir hata alırsınız. 
   Dize olarak bağlam nesnesi için çift tırnak işareti, örneğin ekleyin:

   ![Dize olarak atama nesnesi](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

7. İstek üstbilgileri veya sorgu parametreleri kullanılacak yöntemi gibi diğer ayrıntılarını belirtmek için seçin **Gelişmiş Seçenekleri Göster**.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Logic Apps'e var olan işlevleri ekleyin

Mantıksal uygulamalarınızı mevcut Azure işlevleri çağırmak için Logic App Tasarımcısı'nda Azure işlevleri gibi diğer herhangi bir eylem ekleyebilirsiniz. 

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın. 

2. İşlev eklemek istediğiniz adımı altında seçin **yeni adım** > **Eylem Ekle**. 

3. Arama kutusuna filtreniz olarak "azure işlevleri" girin.
Eylem listesinden şu eylemi seçin: **Azure işlevleri - Azure işlevi seçin** 

   !["Azure işlevleri" bulun](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. İşlev uygulamaları listesinden işlevi uygulamanızı seçin. İşlevler listesi göründükten sonra işlevinizi seçin. 

   ![İşlev uygulaması ve Azure işlevi seçin](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   API tanımı (Swagger açıklamaları) sahip olan ve olmayan işlevler için [mantıksal uygulamanızı bulabilir ve bu işlevlere erişmek için ayarlanan](#function-swagger), seçebileceğiniz **Swagger eylemleri**:

   ![İşlev uygulaması, "Swagger eylemleri" seçin "ve Azure işlevinizin](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

5. İçinde **istek gövdesi** kutusunda, hangi biçimlendirilmelidir işlevinizin giriş olarak geçirmek için bağlam nesnesini JavaScript nesne gösterimi (JSON) belirtin. Bu bağlam nesnesinin ileti ve işlevinize, mantıksal uygulamanın gönderdiği içeriklere açıklar. 

   Tıkladığınızda **istek gövdesi** kutusu, dinamik içerik listesi açılır belirteçleri kullanılabilir özellikleri için önceki adımlardan seçebilmeniz için. 
   Bu örnekte nesnesinde geçirir **gövdesi** e-posta tetikleyici belirteç:

   !["İstek gövdesi" örnek - bağlam nesnesi yükü](./media/logic-apps-azure-functions/function-request-body-example.png)

   İçeriği doğrudan için JSON yükü eklendikten şekilde bu örnekte, bir dize olarak cast bağlam nesnesi değil. 
   Ancak, nesne bir dize, bir JSON nesnesi veya bir JSON dizisi olmalıdır, bir JSON belirteci değilse bir hata alırsınız. 
   Dize olarak bağlam nesnesi için çift tırnak işareti, örneğin ekleyin:

   ![Dize olarak atama nesnesi](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

6. İstek üstbilgileri veya sorgu parametreleri kullanılacak yöntemi gibi diğer ayrıntılarını belirtmek için seçin **Gelişmiş Seçenekleri Göster**.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-functions"></a>Mantıksal uygulamaları çağırma işlevleri

Bir Azure işlevi içinde bir mantıksal uygulamadan tetiklemek için bu mantıksal uygulama çağrılabilir uç nokta olmalıdır ya da daha açık belirtmek gerekirse bir **istek** tetikleyici. Ardından, işlevinizin bir HTTP POST isteği URL'sini için gönderen **isteği** tetikleyin ve işlemek için bu mantıksal uygulama istediğiniz yükü içerir. Daha fazla bilgi için [çağrı, tetikleyici veya iç içe mantıksal uygulamalar](../logic-apps/logic-apps-http-endpoint.md). 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
