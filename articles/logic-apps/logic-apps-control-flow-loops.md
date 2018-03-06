---
title: "Döngüler - işlem dizileri veya yineleme Eylemler - Azure Logic Apps | Microsoft Docs"
description: "İşlem \"için her\" dizilerle döngüler veya logic apps içinde belirli koşulların kadar yineleme eylemleri"
services: logic-apps
keywords: "her döngü için"
documentationcenter: 
author: ecfan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: estfan; LADocs
ms.openlocfilehash: f634b1004fef2eb65c6b8134088ceead47c91890
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="loops-process-arrays-or-repeat-actions-until-a-condition-is-met"></a>Döngüler: diziler işlem veya Eylemler bir koşul yerine getirilene kadar yineleme

Mantıksal uygulama diziler üzerinden yineleme yapmak için kullanabileceğiniz bir ["Foreach" döngüsü](#foreach-loop) veya [sıralı "Foreach" döngüsü](#sequential-foreach-loop). Sıralı bir "Foreach" döngü Döngülerde birer birer çalıştırırken standart "Foreach" döngü Döngülerde paralel olarak çalıştırın. "Foreach" döngüler çalıştırmak tek mantıksal uygulama içinde işleyebilir dizi öğe maksimum sayısı için bkz: [sınırlarını ve yapılandırmasını](../logic-apps/logic-apps-limits-and-config.md). 

> [!TIP] 
> Bir dizi alan ve her bir dizi öğesi için bir iş akışını çalıştırmak istediğiniz bir tetikleyici varsa *debatch* bu diziyle [ **SplitOn** tetiklemek özelliği](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch). 
  
Bir koşula uyulup uyulmadığını veya bazı durumu değişti kadar Eylemler yinelemek için kullanın bir ["Kadar" döngü](#until-loop). Mantıksal uygulamanızı ilk döngü içinde tüm eylemleri gerçekleştirir ve koşul son adımı olarak denetler. Koşul karşılandığında, döngü durur. Aksi takdirde, döngü tekrarlar. En fazla sayısını "Kadar" için çalıştırın, tek bir mantıksal uygulama Döngülerde görmek [sınırlarını ve yapılandırmasını](../logic-apps/logic-apps-limits-and-config.md). 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Hakkındaki temel bilgileri [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>"Foreach" döngüsü

Dizideki her öğe için Eylemler yinelemek için mantığı uygulama akışınızda "Foreach" döngüsü kullanın. Birden çok eylem "Foreach" döngüde içerebilir ve birbirlerine içinde "Foreach" döngüler yerleştirebilirsiniz. Varsayılan olarak, standart "Foreach" döngü Döngülerde paralel olarak çalışır. "Bu Foreach döngüsü çalıştırabilirsiniz" paralel üst sınırını döngüleri için bkz: [sınırları ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md).

> [!NOTE] 
> "Foreach" döngüsü yalnızca bir dizi ile çalışır ve Döngüdeki eylemlerini kullanın `@item()` dizideki her öğe işlemek için başvuru. Bir dizi değil veri belirtirseniz, mantığı uygulama iş akışı başarısız olur. 

Örneğin, bu mantıksal uygulama, günlük özet bir Web sitesinin RSS akışı gönderir. Uygulama bir e-posta gönderen her yeni öğesi bulunamadı "Foreach" döngü kullanır.

1. [Bu örnek mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) bir Outlook.com veya Office 365 Outlook hesapla.

2. RSS arasında tetiklemek ve gönderme e-posta eylemi, "Foreach" döngü ekleyin. 

   Adımları arasında döngü eklemek için döngü eklemek istediğiniz işaretçiyi üzerinde oku taşıyın. 
   Seçin **artı** (**+**) görünür, ardından **Ekle bir her**.

   !["Foreach" döngü adımlar arasındaki ekleyin](media/logic-apps-control-flow-loops/add-for-each-loop.png)

3. Şimdi döngü oluşturun. Altında **bir çıktı önceki adımları seçin** sonra **dinamik içerik eklemek** listesi görüntülenir, seçin **bağlantılar akış** RSS tetikleyici çıktısını olan dizi. 

   ![Dinamik içerik listeden seçin](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > Seçebileceğiniz *yalnızca* dizi önceki adımda çıkarır.

   Seçilen dizi şimdi burada görünür:

   ![Dizi seçin](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. Her bir dizi öğede bir eylem gerçekleştirmek üzere sürükleyin **bir e-posta Gönder** eyleme **her** döngü. 

   Mantıksal uygulamanız bu örnek şöyle görünebilir:

   ![Adımları "Foreach" döngü ekleme](media/logic-apps-control-flow-loops/for-each-loop-with-step.png)

5. Mantıksal uygulamanızı kaydedin. El ile tasarımcı araç çubuğunda, mantıksal uygulamanızı test etmeyi **çalıştırmak**.

<a name="for-each-json"></a>

## <a name="foreach-loop-definition-json"></a>"Foreach" döngü tanımı (JSON)

Mantıksal uygulamanız için kod görünümünde çalışıyorsanız, tanımlayabilirsiniz `Foreach` mantığı uygulamanızın JSON tanımında döngü bunun yerine, örneğin:

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

Varsayılan olarak, her bir dizi öğesi için paralel "Foreach" döngü her döngü çalıştırır. Her döngü sıralı olarak çalışacak şekilde ayarlanmış **sıralı** "Foreach" döngü seçeneği.

1. Döngünün sağ üst köşedeki, seçin **üç nokta** (**...** ) > **Ayarları**.

   !["Foreach" döngüsü seçin "..." > "Ayarlar"](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

2. Aç **sıralı** ayarını seçin **Bitti**.

   !["Foreach" döngünün sıralı ayarı](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

Ayrıca ayarlayabilirsiniz **operationOptions** parametresi `Sequential` mantığı uygulamanızın JSON tanımında. Örneğin:

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
  
Bir koşula uyulup uyulmadığını veya bazı durumu değişti kadar Eylemler yinelemek için mantığı uygulama akışınızda "Kadar" döngü kullanın. "Kadar" döngü burada kullanabileceğiniz bazı ortak kullanım durumları şunlardır:

* İstediğiniz yanıt elde edene kadar bir uç nokta çağırın.
* Bir veritabanında bir kayıt oluşturmak, kayıt Onaylandı, belirli bir alana kadar bekleyin ve işleme devam. 

> [!NOTE]
> "Döngüler"Foreach"döngüler veya diğer döngü"Kadar"içeremez kadar".

Örneğin, değişkenin değeri eşittir 10 kadar 8: 00'de her gün, bu mantıksal uygulama bir değişken artırır. Ardından, mantıksal uygulama geçerli değeri soran bir e-posta gönderir. Bu örnekte Office 365 Outlook kullansa da, Logic Apps tarafından desteklenen herhangi bir e-posta sağlayıcıyı kullanabilirsiniz ([bağlayıcılar listesi burada gözden](https://docs.microsoft.com/connectors/)). Başka bir e-posta hesabı kullanıyorsanız genel adımlar aynıdır, ancak kullanıcı arabirimi biraz farklı olabilir. 

1. Boş bir mantıksal uygulama oluşturma. Mantıksal Uygulama Tasarımcısı'nda "recurrence" için arama ve bu Tetikleyici seçin: **çizelgesi - yineleme** 

   !["Zamanlama – Recurrence" tetikleyicisi ekleyin](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

2. Zaman aralığı, sıklığı ve günün saatini ayarlayarak tetiği harekete belirtin. Saati ayarlamak için seçin **Gelişmiş Seçenekleri Göster**.

   !["Zamanlama – Recurrence" tetikleyicisi ekleyin](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Özellik | Değer |
   | -------- | ----- |
   | **Aralık** | 1 | 
   | **Sıklık** | Gün |
   | **Şu saatlerde** | 8 |
   ||| 

3. Tetikleyici altında seçin **yeni adım** > **Eylem Ekle**. "Değişkenleri" araması yapın ve sonra bu eylem seçin: **değişkenleri - Initialize değişkeni**

   !["Değişken - Initialize değişkeni" ekleyebilirsiniz eylemi](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

4. Bu değerler, değişkenle ayarlayın:

   ![Değişken özelliklerini ayarlama](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Özellik | Değer | Açıklama |
   | -------- | ----- | ----------- |
   | **Ad** | Sınır | Değişken adı | 
   | **Tür** | Tamsayı | Değişkenin veri türü | 
   | **Değer** | 0 | Değişken değeri başlıyor | 
   |||| 

5. Altında **Initialize değişkeni** eylemi seçin **yeni adım** > **daha fazla**. Bu döngü seçin: **kadar Ekle**

   !["Kadar yapmak" döngü ekleme](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

6. Döngünün çıkış koşulu seçerek yapı **sınırı** değişken ve **eşittir** işleci. Girin **10** karşılaştırma değeri olarak.

   ![Döngü durdurmak için çıkış koşulu oluştur](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

7. Döngünün içinde seçin **Eylem Ekle**. "Değişkenleri" araması yapın ve ardından bu eylemi ekleyin: **değişkenleri - artışı değişkeni**

   ![Değişken artırma için Eylem Ekle](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

8. İçin **adı**seçin **sınırı** değişkeni. İçin **değeri**, "1" girin. 

   ![1 "Sınırı" artışla](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

9. Altında ancak döngü dışında e-posta gönderen bir eylem ekleyin. İstenirse, e-posta hesabınızda oturum açın.

   ![E-posta gönderir Eylem Ekle](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

10. E-postanın özellikleri ayarlayın. Ekleme **sınırı** konusuyla değişken. Böylece, değişkenin geçerli değeri, belirtilen, örneğin koşulunu doğrulayabilirsiniz:

    ![E-posta özelliklerini ayarlama](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

    | Özellik | Değer | Açıklama |
    | -------- | ----- | ----------- | 
    | **Alıcı** | *<email-address@domain>* | Alıcının e-posta adresi. Test etmek için kendi e-posta adresi kullanın. | 
    | **Konu** | "Sınırı" için geçerli değer **sınırı** | E-posta konusunu belirtin. Bu örnekte, eklediğinizden emin olun **sınırı** değişkeni. | 
    | **Gövde** | <*e-posta içeriği*> | Göndermek istediğiniz e-posta iletisi içeriği belirtin. Bu örnek için istediğiniz herhangi bir metni girin. | 
    |||| 

11. Mantıksal uygulamanızı kaydedin. El ile tasarımcı araç çubuğunda, mantıksal uygulamanızı test etmeyi **çalıştırmak**.

    Mantığınızı çalışmaya başladıktan sonra belirttiğiniz içeriği içeren bir e-posta alın:

    ![Alınan e-posta](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Sonsuz döngüler engelle

Bir "Kadar" döngü bu koşulların herhangi biri görülüyorsa yürütme Durdur varsayılan sınırlamaları vardır:

| Özellik | Varsayılan değer | Açıklama | 
| -------- | ------------- | ----------- | 
| **Sayısı** | 60 | Döngüden çıkılıp önce çalıştırılan döngüler maksimum sayısı. Varsayılan değer 60 döngüleri ' dir. | 
| **Zaman aşımı** | PT1H | Döngü önce bir döngü çalıştırmak için en uzun süreyi çıkar. Varsayılan bir saat ve ISO 8601 biçiminde belirtilir. <p>Zaman aşımı değeri her döngü döngüsü için değerlendirilir. Döngü içinde herhangi bir işlem zaman aşımı sınırdan daha uzun sürerse, geçerli döngüsü bitmez ancak sınırı koşul değil çünkü bir sonraki döngüyü başlamıyor. | 
|||| 

Bu varsayılan sınırları değiştirmek için tercih **Gelişmiş Seçenekleri Göster** döngü eylem şeklinde.

<a name="until-json"></a>

## <a name="until-definition-json"></a>"Kadar" tanımı (JSON)

Mantıksal uygulamanız için kod görünümünde çalışıyorsanız, tanımlayabilirsiniz bir `Until` mantığı uygulamanızın JSON tanımında döngü bunun yerine, örneğin:

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

Başka bir örnekte, bir kaynak oluşturan ve HTTP yanıt gövdesi "Tamamlandı" durumundaki döndürdüğünde durdurur bir HTTP uç noktası bu "Kadar" döngü çağırır. Sonsuz döngüler önlemek için aşağıdaki koşullardan herhangi biri görülüyorsa döngü ayrıca durdurur:

* Döngü 10 kez belirtildiği gibi çalıştırıldı `count` özniteliği. Varsayılan değer 60 katıdır. 
* Döngü belirtildiği gibi iki saat çalıştırmak denedi `timeout` ISO 8601 biçiminde özniteliği. Varsayılan bir saattir.
  
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
* Gönderme veya özellikler ve öneri, oy [Azure Logic Apps kullanıcı geri bildirim sitesi](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre (koşullu deyimler) adımları çalıştırın](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlerini (switch deyimleri) temel alan adımları çalıştırın](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırmak veya paralel adımları (dal) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplandırılmış eylem durumu (kapsam) temelinde adımları çalıştırın](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)