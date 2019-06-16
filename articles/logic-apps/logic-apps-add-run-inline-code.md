---
title: Ekleme ve kod parçacıkları - Azure Logic Apps çalıştırma
description: Ekleme ve Azure Logic Apps kodunda satır içi kod parçacıkları çalıştırma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: derek1ee, LADocs
ms.topic: article
ms.date: 05/14/2019
ms.openlocfilehash: 0bfa98396ee3afb80b486a5a17959664dfbe603c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65602124"
---
# <a name="add-and-run-code-snippets-by-using-inline-code-in-azure-logic-apps"></a>Ekleme ve kod parçacıkları, Azure Logic Apps'te satır içi kod kullanarak çalıştırma

Mantıksal uygulamanız içinde kod parçası çalıştırmak istediğinizde, yerleşik ekleyebilirsiniz **satır içi kod** mantıksal uygulamanızın iş akışı adımı olarak eylem. Bu senaryo uygun kodu çalıştırmak istediğinizde bu eylemi en iyi şekilde çalışır:

* JavaScript içinde çalışır. Çok yakında daha fazla dil.
* Beş saniye veya daha az çalışan biter.
* Veri işleme boyutu en fazla 50 MB.
* Node.js sürümü 8.11.1 kullanır. Daha fazla bilgi için [standart yerleşik nesneleri](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects). 

  > [!NOTE]
  > Require() işlevi tarafından desteklenmeyen **satır içi kod** JavaScript çalıştırma için eylem.

Bu eylem, kod parçacığı çalıştırır ve çıktıyı bu kod parçacığından adlı bir belirteç döndürür. **sonucu**, hangi mantıksal uygulamanızda sonraki eylemlerde kullanabiliriz. İstediğiniz, kodunuzda bir işlev oluşturmak için diğer senaryolar için deneyin [oluşturma ve bir Azure işlevi çağırma](../logic-apps/logic-apps-azure-functions.md) mantıksal uygulamanızda.

Bu makalede, bir Office 365 Outlook hesabı yeni bir e-posta geldiğinde örnek mantıksal uygulama Tetikleyicileri ulaşır. Kod parçacığının ayıklar ve e-posta gövdesinde görünen herhangi bir e-posta adresi döndürür.

![Örnek genel bakış](./media/logic-apps-add-run-inline-code/inline-code-example-overview.png)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mantıksal uygulama bir tetikleyici de dahil olmak üzere, kod parçacığı eklemek istediğiniz. Mantıksal uygulama yoksa bkz [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

   Bu konudaki örnek mantıksal uygulama bu Office 365 Outlook tetikleyici kullanır: **Yeni bir e-posta geldiğinde**

* Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) mantıksal uygulamanıza bağlı

## <a name="add-inline-code"></a>Satır içi kod ekleyin

1. Henüz kaydolmadıysanız, [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın.

1. Tasarımcıda ekleme **satır içi kod** mantıksal uygulamanızın iş akışında istediğiniz konumda eylem.

   * İş akışınızı sonunda eylem eklemek için **yeni adım**.

   * Var olan adımlar arasında bir eylem eklemek için bu adımları bağlanan okun üzerine fare işaretçisini taşıyın. Artı işaretini seçin ( **+** ) seçip **Eylem Ekle**.

   Bu örnek ekler **satır içi kod** eylemi Office 365 Outlook tetikleyicinin altında.

   ![Yeni adım Ekle](./media/logic-apps-add-run-inline-code/add-new-step.png)

1. Altında **eylem seçin**, arama kutusuna filtreniz olarak "satır içi kod" girin. Eylem listesinden şu eylemi seçin: **JavaScript kodu yürütme**

   !["JavaScript kodu yürütme" seçin](./media/logic-apps-add-run-inline-code/select-inline-code-action.png)

   Eylem Tasarımcısı'nda görünür ve bir dönüş ifadesi'dahil olmak üzere bazı varsayılan örnek kodunu içerir.

   ![Varsayılan örnek kod ile satır içi kod eylemi](./media/logic-apps-add-run-inline-code/inline-code-action-default.png)

1. İçinde **kod** kutusuna örnek kodu silin ve çalıştırmak istediğiniz kodu girin. Bir yöntem içinde ancak yöntem imzası tanımlama olmadan koyabilirsiniz kod yazın. 

   Tanınan bir anahtar sözcüğü yazın, kullanılabilir sözcüklerden örneğin seçebilmeniz için otomatik tamamlama listesi görünür:

   ![Anahtar sözcüğü otomatik tamamlama listesi](./media/logic-apps-add-run-inline-code/auto-complete.png)

   Bu örnek kod parçacığı ilk olarak depolayan bir değişken oluşturur bir *normal ifade*, giriş metninde eşleştirmeyi için bir desen belirtir. Kodu daha sonra tetikleyici için e-posta gövdesi verilerini depolayan bir değişken oluşturur.

   ![Değişken oluşturma](./media/logic-apps-add-run-inline-code/save-email-body-variable.png)

   Tetikleyici ve Eylemler önceki sonuçlardan başvuru kolaylaştırmak için imlecinizi iç ederken dinamik içerik listesi görünür **kod** kutusu. Bu örnekte, liste tetikleyiciden kullanılabilir sonuçları gösterir dahil olmak üzere **gövdesi** belirteci artık seçebilirsiniz.

   Seçtikten sonra **gövdesi** belirteç, satır içi kod eylemi belirtece çözümlenen bir `workflowContext` e-postanın başvuran nesne `Body` özellik değeri:

   ![Sonucu seçin](./media/logic-apps-add-run-inline-code/inline-code-example-select-outputs.png)

   İçinde **kod** kutusunda parçacığınızı salt okunur kullanabilirsiniz `workflowContext` giriş olarak nesnesi. Bu nesne, tetikleyici ve önceki eylemlerin iş sonuçları, kod erişim vermek alt özellikleri vardır.
   Daha fazla bilgi için bu konunun ilerleyen bölümlerinde bu bölümüne bakın: [Tetikleyici ve eylem sonuçlarını, kodunuzda başvuru](#workflowcontext).

   > [!NOTE]
   >
   > Kod parçacığınızı nokta (.) işlecini kullanan eylem adları başvuruyorsa, bu eylem adları için eklemelisiniz [ **eylemleri** parametre](#add-parameters). Bu başvuruları da örneğin eylem adları köşeli ayraç ([]) ve tırnak işaretleri ile almalısınız:
   >
   > `// Correct`</br> 
   > `workflowContext.actions["my.action.name"].body`</br>
   >
   > `// Incorrect`</br>
   > `workflowContext.actions.my.action.name.body`

   Satır içi kod eylemi gerektirmeyen bir `return` deyimi, ancak sonuçları bir `return` deyimi, sonraki eylemleri başvuru için kullanılabilir **sonucu** belirteci. 
   Örneğin, kod parçacığının çağırarak sonucunu döndürür `match()` işlevi, hangi bulduğu eşleşen normal ifade karşı e-posta gövdesindeki. **Compose** eylem kullandığı **sonucu** belirteci başvurmak için satır içi sonuçlardan kod eylemi ve tek bir sonuç oluşturur.

   ![Tamamlanmış mantıksal uygulama](./media/logic-apps-add-run-inline-code/inline-code-complete-example.png)

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

<a name="workflowcontext"></a>

### <a name="reference-trigger-and-action-results-in-your-code"></a>Kodunuzda başvuru tetikleyici ve eylem sonuçları

`workflowContext` Nesnesinde içerir, bu yapı `actions`, `trigger`, ve `workflow` alt:

```json
{
   "workflowContext": {
      "actions": {
         "<action-name-1>": @actions('<action-name-1>'),
         "<action-name-2>": @actions('<action-name-2>')
      },
      "trigger": {
         @trigger()
      },
      "workflow": {
         @workflow()
      }
   }
}
```

Bu tablo, bu alt özellikler hakkında daha fazla bilgi içerir:

| Özellik | Tür | Açıklama |
|----------|------|-------|
| `actions` | Nesne koleksiyonu | Sonuç nesnelerini kod parçacığınız çalışmadan önce çalıştırmak Eylemlerdeki. Her bir nesnenin sahip bir *anahtar-değer* burada anahtarı bir eylemin adı ve değeri çağırmakla eşdeğerdir çifti [actions() işlevi](../logic-apps/workflow-definition-language-functions-reference.md#actions) ile `@actions('<action-name>')`. Eylemin adı boşluk değiştirir temel iş akışı tanımında kullanılan eylem adının aynısını kullanır ("") eylem adı alt çizgi (_). Bu nesne, eylem özellik değerlerini çalıştırmak geçerli iş akışı örneğinden erişim sağlar. |
| `trigger` | Object | Arama için sonuç nesnesi tetikleyici ve eşdeğer [trigger() işlevi](../logic-apps/workflow-definition-language-functions-reference.md#trigger). Bu nesne, geçerli iş akışı örneği çalıştırmak tetikleyicisi özellik değerlerine erişim sağlar. |
| `workflow` | Object | Arama eşdeğerdir ve iş akışı nesnesini [akışı() işlevi](../logic-apps/workflow-definition-language-functions-reference.md#workflow). Bu nesne erişim çalıştırmak geçerli iş akışı örneğinden iş akışı adı, çalıştırma kimliği ve vb. gibi iş akışı özellik değerlerini sağlar. |
|||

Bu konunun örnekte `workflowContext` nesne kodunuzu erişebileceği şu özelliklere sahiptir:

```json
{
   "workflowContext": {
      "trigger": {
         "name": "When_a_new_email_arrives",
         "inputs": {
            "host": {
               "connection": {
                  "name": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/office365"
               }
            },
            "method": "get",
            "path": "/Mail/OnNewEmail",
            "queries": {
               "includeAttachments": "False"
            }
         },
         "outputs": {
            "headers": {
               "Pragma": "no-cache",
               "Content-Type": "application/json; charset=utf-8",
               "Expires": "-1",
               "Content-Length": "962095"
            },
            "body": {
               "Id": "AAMkADY0NGZhNjdhLTRmZTQtNGFhOC1iYjFlLTk0MjZlZjczMWRhNgBGAAAAAABmZwxUQtCGTqSPpjjMQeD",
               "DateTimeReceived": "2019-03-28T19:42:16+00:00",
               "HasAttachment": false,
               "Subject": "Hello World",
               "BodyPreview": "Hello World",
               "Importance": 1,
               "ConversationId": "AAQkADY0NGZhNjdhLTRmZTQtNGFhOC1iYjFlLTk0MjZlZjczMWRhNgAQ",
               "IsRead": false,
               "IsHtml": true,
               "Body": "Hello World",
               "From": "<sender>@<domain>.com",
               "To": "<recipient-2>@<domain>.com;<recipient-2>@<domain>.com",
               "Cc": null,
               "Bcc": null,
               "Attachments": []
            }
         },
         "startTime": "2019-05-03T14:30:45.971564Z",
         "endTime": "2019-05-03T14:30:50.1746874Z",
         "scheduledTime": "2019-05-03T14:30:45.8778117Z",
         "trackingId": "1cd5ffbd-f989-4df5-a96a-6e9ce31d03c5",
         "clientTrackingId": "08586447130394969981639729333CU06",
         "originHistoryName": "08586447130394969981639729333CU06",
         "code": "OK",
         "status": "Succeeded"
      },
      "workflow": {
         "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<logic-app-workflow-name>",
         "name": "<logic-app-workflow-name>",
         "type": "Microsoft.Logic/workflows",
         "location": "<Azure-region>",
         "run": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<logic-app-workflow-name>/runs/08586453954668694173655267965CU00",
            "name": "08586453954668694173655267965CU00",
            "type": "Microsoft.Logic/workflows/runs"
         }
      }
   }
}
```

<a name="add-parameters"></a>

## <a name="add-parameters"></a>Parametre ekleme

Bazı durumlarda, açıkça gerektiren gerekebilir **satır içi kod** eylem sonuçlardan ekleyerek kod bağımlılıkları olarak başvuran tetikleyicisi veya belirli eylemler içeriyorsa **tetikleyici** veya **Eylemleri** parametreleri. Bu seçenek, çalışma zamanında başvurulan sonuçları bulunduğu olmayan senaryolar için kullanışlıdır.

> [!TIP]
> Kodunuzu yeniden kullanmayı planlıyorsanız, özellikler için başvuru kullanarak eklemek **kod** tetikleyici veya eylem özel bağımlılıklar eklemek yerine kodunuzu çözümlenen belirteci başvuruları içeren kutusu.

Örneğin, başvuran kod olduğunu varsayalım **SelectedOption** sonucunda **onay e-posta Gönder** Office 365 Outlook Bağlayıcısı için eylem. Oluşturma zamanı, Logic Apps altyapısı, herhangi bir tetikleyici başvurulan veya eylem sonuçlarını ve bu sonuçları otomatik olarak dahil olup olmadığını belirlemek için kodunuzu analiz eder. Çalışma zamanında, başvurulan tetikleyici veya eylem sonucu belirtilen kullanılabilir olmayan bir hata almalısınız `workflowContext` ekleyebilirsiniz, tetikleyici veya eylemi açık bir bağımlılık olarak nesnesi. Bu örnekte, eklediğiniz **eylemleri** parametresi belirleyen **satır içi kod** eylem sonucu açıkça dahil **onay e-posta Gönder** eylem.

Bu parametre eklemek için açık **yeni parametre Ekle** listelemek ve istediğiniz parametreleri seçin:

   ![Parametre ekleme](./media/logic-apps-add-run-inline-code/inline-code-action-add-parameters.png)

   | Parametre | Açıklama |
   |-----------|-------------|
   | **Eylemler** | Önceki Eylemlerdeki sonuçları içerir. Bkz: [eylem sonuçlarında](#action-results). |
   | **Tetikleyici** | Tetikleyici sonuçlarını içerir. Bkz: [INCLUDE tetikleyici sonuçları](#trigger-results). |
   |||

<a name="trigger-results"></a>

### <a name="include-trigger-results"></a>Tetikleyici sonuçları dahil et

Seçerseniz **Tetikleyicileri**, sonuçlarını tetikleyici dahil etmek için olup olmadığını istenir.

* Gelen **tetikleyici** listesinden **Evet**.

<a name="action-results"></a>

### <a name="include-action-results"></a>Eylem sonuçlarını içerir.

Seçerseniz **eylemleri**, istenir, eklemek istediğiniz eylemleri için. Ancak, Eylemler ekleme başlamadan önce mantıksal uygulamanın temel iş akışı tanımı içinde görünen eylem adı sürümü gerekir.

* Bu özellik, değişkenleri, döngüler ve yineleme dizinler desteklemiyor.

* Mantıksal uygulamanızın iş akışı tanımı adları alt çizgi (_), bir alanı kullanın.

* Nokta işleci (.) eylem adları için örneğin bu işleçler şunlardır:

  `My.Action.Name`

1. Tasarımcı araç çubuğunda **kod görünümü**, içinde arama ve `actions` özniteliği eylem adı.

   Örneğin, `Send_approval_email_` JSON adı **onay e-posta Gönder** eylem.

   ![Eylem adı, JSON'da bulun](./media/logic-apps-add-run-inline-code/find-action-name-json.png)

1. Kod Görünümü araç çubuğunda, Tasarımcı görünümü dönmek için **Tasarımcısı**.

1. İlk eylem eklemek için **eylemleri öğesi - 1** kutusu, eylem JSON adı girin.

   ![İlk eylem girin](./media/logic-apps-add-run-inline-code/add-action-parameter.png)

1. Başka bir eylem eklemek için **Yeni Öğe Ekle**.

## <a name="reference"></a>Başvuru

Hakkında daha fazla bilgi için **yürütme JavaScript kodunu** eylemin yapısı ve mantıksal uygulamanızın iş akışı tanımlama dili kullanarak temel iş akışı tanımı sözdiziminde bkz bu eylemin [bölümü başvurusu ](../logic-apps/logic-apps-workflow-actions-triggers.md#run-javascript-code).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure Logic Apps için bağlayıcılar](../connectors/apis-list.md)
