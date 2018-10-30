---
title: Eylemler yineleme veya dizi - Azure Logic Apps işlem döngüler ekleme | Microsoft Docs
description: İş akışı eylemi yineleyin veya Azure Logic Apps dizilerde işlem döngü oluşturma
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 5ba5e5abef4ebdc58c44cbe7f5ba584efe8abfc7
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233115"
---
# <a name="create-loops-that-repeat-workflow-actions-or-process-arrays-in-azure-logic-apps"></a>İş akışı eylemi yineleyin veya Azure Logic Apps dizilerde işlem döngü oluşturma

Mantıksal uygulamanızı diziler üzerinden yineleme yapmak için kullanabileceğiniz bir ["Foreach" döngüsünü](#foreach-loop) veya [sıralı "Foreach" döngü](#sequential-foreach-loop). Sıralı bir "Foreach" döngü yinelemesi teker teker çalıştırırken yinelemeler için standart bir "Foreach" döngüsünü paralel olarak çalıştırın. En fazla sayısı "Foreach" döngüler, tek bir mantıksal uygulama çalıştırması işleyebilir dizi öğeleri için bkz: [limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md). 

> [!TIP] 
> Bir dizi alır ve her dizi öğesi için bir iş akışını çalıştırmak istediğiniz bir tetikleyici varsa *debatch* ile bu diziyi [ **SplitOn** özellik tetikleyicisi](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch). 
  
Bir koşul karşılandığında veya bazı durumu değişti kadar Eylemler yinelemek için kullanmak bir ["Kadar" döngü](#until-loop). Mantıksal uygulamanızı ilk döngü içinde tüm eylemleri gerçekleştirir ve ardından koşul son adım olarak denetler. Koşul karşılanırsa, döngü durdurur. Aksi takdirde, döngü tekrarlanır. En fazla sayısını "Kadar" için tek bir mantıksal uygulama için çalıştırma, Döngülerde görmek [limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md). 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>"Foreach" döngü

Bir dizideki her öğe için Eylemler yinelemek için mantıksal uygulama iş akışınızı "Foreach" döngüsünü kullanın. Birden fazla eylem "Foreach" döngüde içerebilir ve "Foreach" döngüler birbirine içinde iç içe yerleştirebilirsiniz. Varsayılan olarak, standart "Foreach" döngü Döngülerde paralel olarak çalışır. "Döngüler çalıştırabilir, Foreach" sayısının paralel döngüler için bkz: [limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md).

> [!NOTE] 
> "Foreach" döngü yalnızca bir dizi ile çalışır ve eylemlerini döngüsünde kullanın `@item()` dizideki her bir öğeyi işleyemedi, başvuru. Bir dizi içinde olmayan veriler belirlediğiniz mantıksal uygulama iş akışı başarısız olur. 

Örneğin, bu mantıksal uygulama, günlük özetini bir Web sitesinin RSS akışı gönderir. Uygulama bir e-posta gönderen her yeni öğe bulundu "Foreach" döngü kullanır.

1. [Bu örnek mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) ile bir Outlook.com veya Office 365 Outlook hesabı.

2. RSS arasında tetikleyin ve gönderme e-posta eylemi, "Foreach" döngüsünü ekleyin. 

   Adımları arasındaki bir döngüyü eklemek için döngü eklemek istediğiniz okun üzerine işaretçiyi taşıyın. 
   Seçin **artı** (**+**) görünür, ardından **Ekle bir her**.

   ![Adımlar arasındaki bir "Foreach" döngüsünü ekleyin](media/logic-apps-control-flow-loops/add-for-each-loop.png)

3. Şimdi döngünün oluşturun. Altında **önceki adımlardan bir çıkış seçin** sonra **dinamik içerik Ekle** listesi görüntülenirse, seçin **akış bağlantıları** RSS tetikleyicisi çıktısını dizisi. 

   ![Dinamik içerik listesinden seçin](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > Seçebileceğiniz *yalnızca* dizinin önceki adımda çıkarır.

   Seçilen dizi artık burada görünür:

   ![Dizi seçin](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. Dizideki tüm öğeler bir eylem gerçekleştirmek için sürükleyin **bir e-posta** eylemlere **her** döngü. 

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
        "runAfter": {},
    }
},
```

<a name="sequential-foreach-loop"></a>

## <a name="foreach-loop-sequential"></a>"Foreach" döngü: sıralı

Varsayılan olarak, her döngü "Foreach" döngü içinde dizideki tüm öğeler için paralel çalışır. Her döngü sıralı olarak çalıştırmak için ayarlanmış **ardışık** "Foreach" döngünüzü seçeneği.

1. Döngünün sağ üst köşedeki, seçin **üç nokta** (**...** ) > **Ayarları**.

   !["Foreach" döngüsünü üzerinde seçin "..." > "Ayarlar"](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

2. Açma **ardışık** ayarını seçin **Bitti**.

   !["Foreach" döngünün sıralı ayarı](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

Ayrıca **operationOptions** parametresi `Sequential` mantıksal uygulamanızın JSON tanımında. Örneğin:

``` json
"actions": {
    "myForEachLoopName": {
        "type": "Foreach",
        "actions": {
            "Send_an_email": {               
            }
        },
        "foreach": "@triggerBody()?['links']",
        "runAfter": {},
        "operationOptions": "Sequential"
    }
},
```

<a name="until-loop"></a>

## <a name="until-loop"></a>Döngü "Kadar"
  
Bir koşul karşılandığında veya bazı durumu değişti kadar Eylemler yinelemek için mantıksal uygulama iş akışınızı bir "Kadar" döngüsünü kullanın. Bir "Kadar" döngüsünü kullanabileceğiniz bazı yaygın kullanım örnekleri şunlardır:

* İstediğiniz yanıt elde edene kadar bir uç nokta çağırın.
* Bir kaydı bir veritabanı oluşturma, kayıt Onaylandı, belirli bir alana kadar bekleyin ve işleme devam. 

Örneğin, değişkenin değeri eşittir 10 kadar 8: 00'da her gün, bu mantıksal uygulama bir değişkeni artırır. Ardından, mantıksal uygulamanın geçerli değer olduğunu bildiren bir e-posta gönderir. Bu örnekte Office 365 Outlook kullansa da, Logic Apps tarafından desteklenen herhangi bir e-posta sağlayıcısı kullanabilirsiniz ([buradaki bağlayıcı listesini inceleyin](https://docs.microsoft.com/connectors/)). Başka bir e-posta hesabı kullanıyorsanız genel adımlar aynıdır, ancak kullanıcı arabirimi biraz farklı olabilir. 

1. Boş bir mantıksal uygulama oluşturma. Logic Apps Tasarımcısı'nda "yinelenme" için arama yapın ve şu tetikleyiciyi seçin: **zamanlama - yinelenme** 

   !["Zamanlama – yinelenme" tetikleyicisini ekleyin](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

2. Zaman aralığı, sıklığı ve günün saatini ayarlayarak tetikleyici belirtin. Saat koymak için **Gelişmiş Seçenekleri Göster**.

   !["Zamanlama – yinelenme" tetikleyicisini ekleyin](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Özellik | Değer |
   | -------- | ----- |
   | **Aralık** | 1 | 
   | **Sıklık** | Gün |
   | **Şu saatlerde** | 8 |
   ||| 

3. Tetikleyici altında seçin **yeni adım** > **Eylem Ekle**. "Değişkenler" için arama yapın ve ardından şu eylemi seçin: **değişkenler - değişken Başlat**

   !["Değişkenler - değişken Başlat" ekleme eylemi](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

4. Değişkeninizin şu değerlerle ayarlayın:

   ![Değişken özelliklerini ayarlama](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Özellik | Değer | Açıklama |
   | -------- | ----- | ----------- |
   | **Ad** | Sınır | Değişken adı | 
   | **Tür** | Tamsayı | Değişkenin veri türü | 
   | **Değer** | 0 | Değişkeninizin değeri başlıyor | 
   |||| 

5. Altında **değişken Başlat** eylemi seçin **yeni adım** > **daha fazla**. Bu döngü seçin: **do until Ekle**

   !["Do until" döngü Ekle](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

6. Döngünün çıkış koşulu seçerek yapı **sınırı** değişkeni ve **eşittir** işleci. Girin **10** karşılaştırma değeri.

   ![Döngü durdurmak için çıkış koşulu oluşturma](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

7. Döngünün içinde seçin **Eylem Ekle**. "Değişkenler" için arama yapın ve ardından bu eylemi ekleyin: **değişkenler - değişken artırma**

   ![Değişken artırma için eylem ekleme](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

8. İçin **adı**seçin **sınırı** değişkeni. İçin **değer**, "1" girin. 

   !["Sınır" artışla 1](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

9. Altında ancak döngü dışında e-posta gönderen bir eylem ekleyin. İstenirse, e-posta hesabınızda oturum açın.

   ![E-posta gönderen bir eylem ekleme](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

10. E-postanın özelliklerini ayarlayın. Ekleme **sınırı** konuya değişken. Bu şekilde, değişkenin geçerli değeri, belirtilen, örneğin koşulunu doğrulayabilirsiniz:

    ![E-posta özelliklerini ayarlama](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

    | Özellik | Değer | Açıklama |
    | -------- | ----- | ----------- | 
    | **Alıcı** | *<email-address@domain>* | Alıcının e-posta adresi. Test için kendi e-posta adresinizi kullanın. | 
    | **Konu** | Geçerli değer "Sınırın" **sınırı** | E-posta konusunu belirtin. Bu örnekte, eklediğinizden emin olun **sınırı** değişkeni. | 
    | **Gövde** | <*e-posta içeriği*> | E-posta ileti göndermek istediğiniz içeriği belirtin. Bu örnekte, istediğiniz herhangi bir metni girin. | 
    |||| 

11. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda mantıksal uygulamanızı el ile test etmeyi **çalıştırma**.

    Mantığınızı çalışmaya başladıktan sonra bir e-posta, belirttiğiniz içeriğe sahip olursunuz:

    ![Alınan e-posta](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Sonsuz döngüler engelle

Bir "Kadar" döngü, Bu koşullardan herhangi biri varsa, yürütmeyi durdurun varsayılan sınırlara sahiptir:

| Özellik | Varsayılan değer | Açıklama | 
| -------- | ------------- | ----------- | 
| **Sayısı** | 60 | Döngüden çıkılıp önce çalıştırılan döngüler maksimum sayısı. 60 döngüleri varsayılandır. | 
| **zaman aşımı** | PT1H | Döngü önce bir döngüsü çalıştırmak için en uzun süreyi çıkar. Varsayılan bir saattir ve ISO 8601 biçiminde belirtilir. <p>Zaman aşımı değeri her döngü döngüsü için değerlendirilir. Döngü içinde herhangi bir işlem zaman aşımı sınırından daha uzun sürerse, geçerli döngüsü alıkoymaz, ancak bir sonraki döngüde sınır koşulu karşılanmamış çünkü başlamaz. | 
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
        },
    }
},
```

Başka bir örnekte, bir kaynak oluşturur ve HTTP yanıt gövdesinde döndürür "Completed" durumu ile sona erer bir HTTP uç noktasını bu "Kadar" döngü çağırır. Sonsuz döngüler önlemek amacıyla, Bu koşullardan herhangi biri varsa döngüyü da durdurur:

* Döngü 10 kez belirtildiği gibi çalıştırıldı `count` özniteliği. Varsayılan değer 60 katıdır. 
* Döngü iki saat tarafından belirlenen çalıştırmayı denedi `timeout` ISO 8601 biçimli öznitelik. Varsayılan bir saattir.
  
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
},
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Gönderin veya özellikleri ve önerileri oylamak için [Azure Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre (koşullu deyimler) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlere (switch deyimleri) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırın veya paralel adımları (dallar) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplandırılmış eylem durumu (kapsamları) temelinde adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
