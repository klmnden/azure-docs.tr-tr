---
title: Eylemler yineleme veya dizi - Azure Logic Apps işlem döngüler ekleme | Microsoft Docs
description: İş akışı eylemi yineleyin veya Azure Logic Apps dizilerde işlem döngü oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
manager: jeconnoc
ms.date: 01/05/2019
ms.topic: article
ms.openlocfilehash: 339d4270dc1803879607663e9e2db4a86591ec76
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59523010"
---
# <a name="create-loops-that-repeat-workflow-actions-or-process-arrays-in-azure-logic-apps"></a>İş akışı eylemi yineleyin veya Azure Logic Apps dizilerde işlem döngü oluşturma

Mantıksal uygulamanızda bir dizi işlemek için oluşturabileceğiniz bir ["Foreach" döngüsünü](#foreach-loop). Bu döngü dizideki her öğe üzerinde bir veya daha fazla eylemleri yineler. "Foreach" döngü dizi öğesi sayısına yönelik sınırlar işleme için bkz: [limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md). 

Bir koşul veya bir durum değişikliklerini kadar Eylemler yinelemek için oluşturabileceğiniz bir ["Kadar" döngü](#until-loop). Mantıksal uygulamanızı ilk tüm eylemler döngünün içinde çalışır ve ardından koşul veya durumunu denetler. Koşul karşılanırsa, döngü durdurur. Aksi takdirde, döngü tekrarlanır. Bir mantıksal uygulama çalıştırması, Döngülerde "Kadar" sayısı üst sınırı için bkz: [limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md). 

> [!TIP]
> Bir dizi alır ve her dizi öğesi için bir iş akışını çalıştırmak istediğiniz bir tetikleyici varsa *debatch* ile bu diziyi [ **SplitOn** özellik tetikleyicisi](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch). 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>"Foreach" döngü

"Foreach döngüsü" yalnızca diziler üzerinde çalışan ve her dizi öğesi bir veya daha fazla Eylemler tekrarlar. Yinelemelerde "Foreach" döngüsünü paralel olarak çalıştırın. Bir yineleme teker teker ayarıyla ancak çalıştırabilirsiniz bir [sıralı "Foreach" döngü](#sequential-foreach-loop). 

"Foreach" döngüler kullandığınızda bazı noktalar şunlardır:

* İç içe geçmiş Döngülerde yinelemeler her zaman sırayla, paralel olarak çalışır. Paralel iç içe döngü öğeleri için işlemleri çalıştırmak için oluşturma ve [alt mantıksal uygulamayı çağırın](../logic-apps/logic-apps-http-endpoint.md).

* Her döngü yinelemesinin sırasında değişkenlerde işlemlerden tahmin edilebilir sonuçlar almak için bu döngü sırayla çalışır. Örneğin, eşzamanlı olarak çalışan döngü sona erer, artırma, azaltma ve ekleme için değişken işlemleri tahmin edilebilir sonuçlar döndürür. Ancak, eşzamanlı olarak çalışan Döngüdeki her bir yineleme sırasında bu işlemleri öngörülemeyen sonuçlara döndürebilir. 

* "Foreach" eylemi döngü kullanın [`@item()`](../logic-apps/workflow-definition-language-functions-reference.md#item) 
Başvuru ve dizideki her öğe işlemek için ifade. Bir dizi içinde olmayan veriler belirlediğiniz mantıksal uygulama iş akışı başarısız olur. 

Bu örnek mantıksal uygulama bir Web sitesinin RSS akışındaki günlük özetini gönderir. Uygulama, her yeni öğe için bir e-posta gönderen bir "Foreach" döngüsü kullanır.

1. [Bu örnek mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) ile bir Outlook.com veya Office 365 Outlook hesabı.

2. RSS arasında tetikleyin ve gönderme e-posta eylemi, "Foreach" döngüsünü ekleyin. 

   1. Adımlar arasında döngü eklemek için işaretçinizi Bu adımlar arasında okun üzerine getirin. 
   Seçin **artı** (**+**) seçip görüntülenen **Eylem Ekle**.

      !["Eylem Ekle"'i seçin](media/logic-apps-control-flow-loops/add-for-each-loop.png)

   1. Arama kutusunun altındaki seçin **tüm**. Arama kutusuna filtreniz olarak "for each" yazın. Eylem listesinden şu eylemi seçin: **Her - denetim için**

      !["For each" döngüsü ekleyin](media/logic-apps-control-flow-loops/select-for-each.png)

3. Şimdi döngünün oluşturun. Altında **önceki adımlardan bir çıkış seçin** sonra **dinamik içerik Ekle** listesi görüntülenirse, seçin **akış bağlantıları** RSS tetikleyicisi çıktısını dizisi. 

   ![Dinamik içerik listesinden seçin](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > Seçebileceğiniz *yalnızca* dizinin önceki adımda çıkarır.

   Seçilen dizi artık burada görünür:

   ![Dizi seçin](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. Dizideki tüm öğeler bir eylemi çalıştırmak için sürükleyin **bir e-posta** döngü eylemlere. 

   Mantıksal uygulamanız şu örnekteki gibi bir şey benzeyebilir:

   ![Adımlar "Foreach" döngüsünü ekleyin](media/logic-apps-control-flow-loops/for-each-loop-with-step.png)

5. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda mantıksal uygulamanızı el ile test etmeyi **çalıştırma**.

<a name="for-each-json"></a>

## <a name="foreach-loop-definition-json"></a>"Foreach" döngü tanımının (JSON)

Mantıksal uygulamanız için kod görünümde çalışıyorsanız, tanımlayabileceğiniz `Foreach` mantıksal uygulamanızın JSON tanımında döngü bunun yerine, örneğin:

``` json
"actions": {
   "myForEachLoopName": {
      "type": "Foreach",
      "actions": {
         "Send_an_email": {
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "@{item()}",
                  "Subject": "New CNN post @{triggerBody()?['publishDate']}",
                  "To": "me@contoso.com"
               },
               "host": {
                  "api": {
                     "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/office365"
                  },
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            },
            "runAfter": {}
         }
      },
      "foreach": "@triggerBody()?['links']",
      "runAfter": {}
   }
}
```

<a name="sequential-foreach-loop"></a>

## <a name="foreach-loop-sequential"></a>"Foreach" döngü: Sıralı

Varsayılan olarak, Döngülerde "Foreach" döngüsünü paralel olarak çalıştırın. Döngünün her döngü sırayla çalışır ayarlayın **ardışık** seçeneği. Döngüler veya değişkenleri döngüler iç içe olduğunda "Foreach" döngüler tahmin edilebilir sonuçlar burada beklediğiniz sırayla çalıştırılmalıdır. 

1. Döngünün sağ üst köşedeki, seçin **üç nokta** (**...** ) > **Ayarları**.

   !["Foreach" döngüsünü üzerinde seçin "..." > "Ayarlar"](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

1. Altında **eşzamanlılık denetimi**, kapatma **eşzamanlılık denetimi** ayarını **üzerinde**. Taşıma **paralellik derecesi** kaydırıcısını **1**ve **Bitti**.

   ![Eşzamanlılık denetimi açın](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

Mantıksal uygulamanızın JSON tanımı ile çalışıyorsanız, kullanabileceğiniz `Sequential` ekleyerek seçeneği `operationOptions` parametresi, örneğin:

``` json
"actions": {
   "myForEachLoopName": {
      "type": "Foreach",
      "actions": {
         "Send_an_email": { }
      },
      "foreach": "@triggerBody()?['links']",
      "runAfter": {},
      "operationOptions": "Sequential"
   }
}
```

<a name="until-loop"></a>

## <a name="until-loop"></a>Döngü "Kadar"
  
Çalıştırın ve eylemleri bir koşul veya bir durum değişikliklerini kadar yinelemek için bu eylemlerin bir "Kadar" döngüde yerleştirin. Mantıksal uygulamanızı ilk tüm eylemler döngünün içinde çalışır ve ardından koşul veya durumunu denetler. Koşul karşılanırsa, döngü durdurur. Aksi takdirde, döngü tekrarlanır.

Bir "Kadar" döngüsünü kullanabileceğiniz bazı yaygın senaryolar şunlardır:

* İstediğiniz yanıt elde edene kadar bir uç nokta çağırın.

* Bir kaydı bir veritabanı oluşturun. Kayıt Onaylandı, belirli bir alana kadar bekleyin. İşleme devam edin. 

8: 00'da her gün başlayarak, değişkenin değeri eşittir 10 kadar bu örnek mantıksal uygulama, bir değişkeni artırır. Mantıksal uygulama, ardından geçerli değer olduğunu bildiren bir e-posta gönderir. 

> [!NOTE]
> Bu adımları Office 365 Outlook kullanıyoruz, ancak Logic Apps destekleyen herhangi bir e-posta sağlayıcısı kullanabilirsiniz. 
> [Buradaki bağlayıcı listesini denetleyin](https://docs.microsoft.com/connectors/). Başka bir e-posta hesabı kullanırsanız genel adımlar aynı olacaktır ancak kullanıcı Arabirimi biraz farklı görünebilir. 

1. Boş bir mantıksal uygulama oluşturma. Logic Apps Tasarımcısı'nda arama kutusunun altındaki seçin **tüm**. "Yinelenme" arayın. 
   Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Yinelenme - zamanlama**

   !["– Zamanlama yinelenme" tetikleyicisi Ekle](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

1. Zaman aralığı, sıklığı ve günün saatini ayarlayarak tetikleyici belirtin. Saat koymak için **Gelişmiş Seçenekleri Göster**.

   ![Yinelenme Zamanlaması ' ayarlayın](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Özellik | Değer |
   | -------- | ----- |
   | **Aralık** | 1 | 
   | **Sıklık** | Gün |
   | **Şu saatlerde** | 8 |
   ||| 

1. Tetikleyici altında seçin **yeni adım**. 
   "Değişkenler" için arama yapın ve şu eylemi seçin: **Değişken - değişken Başlat**

   !["Değişken - değişken Başlat" eylemini ekleme](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

1. Değişkeninizin şu değerlerle ayarlayın:

   ![Değişken özelliklerini ayarlama](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Özellik | Değer | Açıklama |
   | -------- | ----- | ----------- |
   | **Ad** | Sınır | Değişken adı | 
   | **Tür** | Tamsayı | Değişkenin veri türü | 
   | **Değer** | 0 | Değişkeninizin değeri başlıyor | 
   |||| 

1. Altında **değişken Başlat** eylemi seçin **yeni adım**. 

1. Arama kutusunun altındaki seçin **tüm**. "Kadar için" arama yapın ve şu eylemi seçin: **-Kadar denetimi**

   ![Döngü "Kadar" Ekle](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

1. Döngünün çıkış koşulu seçerek yapı **sınırı** değişkeni ve **eşittir** işleci. 
   Girin **10** karşılaştırma değeri.

   ![Döngü durdurmak için çıkış koşulu oluşturma](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

1. Döngünün içinde seçin **Eylem Ekle**. 

1. Arama kutusunun altındaki seçin **tüm**. "Değişkenler" için arama yapın ve şu eylemi seçin: **Artış değişkeni - değişkenleri**

   ![Değişken artırma için eylem ekleme](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

1. İçin **adı**seçin **sınırı** değişkeni. İçin **değer**, "1" girin. 

     !["Sınır" artışla 1](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

1. Dışında ve bir döngünün altında seçin **yeni adım**. 

1. Arama kutusunun altındaki seçin **tüm**. 
     Bul ve örneğin e-posta gönderen bir eylem ekleyin: 

     ![E-posta gönderen bir eylem ekleme](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

1. İstenirse, e-posta hesabınızda oturum açın.

1. E-posta eylemin özelliklerini ayarlayın. Ekleme **sınırı** konuya değişken. Bu şekilde, değişkenin geçerli değeri, belirtilen, örneğin koşulunu doğrulayabilirsiniz:

      ![E-posta özelliklerini ayarlama](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

      | Özellik | Değer | Açıklama |
      | -------- | ----- | ----------- | 
      | **Alıcı** | *\<e-posta adresi\@etki alanı >* | Alıcının e-posta adresi. Test için kendi e-posta adresinizi kullanın. | 
      | **Konu** | Geçerli değer "Sınırın" **sınırı** | E-posta konusunu belirtin. Bu örnekte, eklediğinizden emin olun **sınırı** değişkeni. | 
      | **Gövde** | <*email-content*> | E-posta ileti göndermek istediğiniz içeriği belirtin. Bu örnekte, istediğiniz herhangi bir metni girin. | 
      |||| 

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda mantıksal uygulamanızı el ile test etmeyi **çalıştırma**.

      Mantığınızı çalışmaya başladıktan sonra bir e-posta, belirttiğiniz içeriğe sahip olursunuz:

      ![Alınan e-posta](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Sonsuz döngüler engelle

Bir "Kadar" döngü, Bu koşullardan herhangi biri varsa, yürütmeyi durdurun varsayılan sınırlara sahiptir:

| Özellik | Varsayılan değer | Açıklama | 
| -------- | ------------- | ----------- | 
| **Sayısı** | 60 | Döngüden çıkılıp önce çalıştırılan döngüler en yüksek sayısı. 60 döngüleri varsayılandır. | 
| **zaman aşımı** | PT1H | Bir döngü döngü önce çalıştırılacak çoğu süreyi çıkar. Varsayılan bir saattir ve ISO 8601 biçiminde belirtilir. <p>Zaman aşımı değeri her döngü döngüsü için değerlendirilir. Geçerli döngü, döngü içinde herhangi bir işlem zaman aşımı sınırından daha uzun sürerse, bitmez. Ancak, sınır koşulu karşılanmamış çünkü bir sonraki döngüde başlamaz. | 
|||| 

Bu varsayılan sınırları değiştirmek için seçin **Gelişmiş Seçenekleri Göster** döngü eylem şeklinde.

<a name="until-json"></a>

## <a name="until-definition-json"></a>Tanımı "Kadar" (JSON)

Mantıksal uygulamanız için kod görünümde çalışıyorsanız, tanımlayabileceğiniz bir `Until` mantıksal uygulamanızın JSON tanımında döngü bunun yerine, örneğin:

``` json
"actions": {
   "Initialize_variable": {
      // Definition for initialize variable action
   },
   "Send_an_email": {
      // Definition for send email action
   },
   "Until": {
      "type": "Until",
      "actions": {
         "Increment_variable": {
            "type": "IncrementVariable",
            "inputs": {
               "name": "Limit",
               "value": 1
            },
            "runAfter": {}
         }
      },
      "expression": "@equals(variables('Limit'), 10)",
      // To prevent endless loops, an "Until" loop 
      // includes these default limits that stop the loop. 
      "limit": { 
         "count": 60,
         "timeout": "PT1H"
      },
      "runAfter": {
         "Initialize_variable": [
            "Succeeded"
         ]
      }
   }
}
```

Bu örnekte "kadar" döngü bir kaynak oluşturan bir HTTP uç noktası çağırır. HTTP yanıt gövdesi ile döndürüldüğünde döngü durur `Completed` durumu. Sonsuz döngüler önlemek amacıyla, Bu koşullardan herhangi biri varsa döngüyü da durdurur:

* Döngü 10 kez belirtildiği gibi çalıştı `count` özniteliği. Varsayılan değer 60 katıdır. 

* Döngü belirtildiği gibi iki saat çalıştı `timeout` ISO 8601 biçimli öznitelik. Varsayılan bir saattir.
  
``` json
"actions": {
   "myUntilLoopName": {
      "type": "Until",
      "actions": {
         "Create_new_resource": {
            "type": "Http",
            "inputs": {
               "body": {
                  "resourceId": "@triggerBody()"
               },
               "url": "https://domain.com/provisionResource/create-resource",
               "body": {
                  "resourceId": "@triggerBody()"
               }
            },
            "runAfter": {},
            "type": "ApiConnection"
         }
      },
      "expression": "@equals(triggerBody(), 'Completed')",
      "limit": {
         "count": 10,
         "timeout": "PT2H"
      },
      "runAfter": {}
   }
}
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Gönderin veya özellikleri ve önerileri oylamak için [Azure Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre (koşullu deyimler) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlere (switch deyimleri) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırın veya paralel adımları (dallar) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplandırılmış eylem durumu (kapsamları) temelinde adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
