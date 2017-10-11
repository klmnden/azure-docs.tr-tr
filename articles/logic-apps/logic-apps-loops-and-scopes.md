---
title: "Döngüler ve kapsamları oluşturun ya da iş akışları - Azure Logic Apps verilerde debatch | Microsoft Docs"
description: "Veri, kapsamları, Grup eylemlere yinelemek için döngüler oluşturun veya Azure Logic Apps içinde daha fazla iş akışlarını başlatmak için veri debatch."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 413a2ba9107ca259ed577825bf0a17ff5622f1ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logic Apps Döngüleri, Kapsamları ve Ayırma
  
Logic Apps, çeşitli yollarla dizilerle, koleksiyonlar, toplu işlemler, iş sağlar ve bir iş akışı içinde döngüye girer.
  
## <a name="foreach-loop-and-arrays"></a>ForEach döngüsü ve diziler
  
Logic Apps, bir veri kümesi üzerinde döngü ve her öğe için bir eylem gerçekleştirmek sağlar.  Bu aracılığıyla mümkündür `foreach` eylem.  Eklenecek belirtebilirsiniz Tasarımcısı'nda bir her döngü için.  Üzerinden yinelemek istediğiniz dizi seçtikten sonra Eylemler eklemeye başlayabilirsiniz.  Şu anda foreach döngüsü başına yalnızca bir eylem için sınırlı olur, ancak bu kısıtlama, önümüzdeki haftalarda kaldırılmış.  Bir kez döngü içinde dizinin her değerinde gerçekleşmesi gerektiğini belirtmek başlayabilirsiniz.

Kod görünümünü kullanıyorsanız, belirleyebileceğiniz bir aşağıdaki gibi her bir döngü için.  Bu örneği olan bir 'microsoft.com' içeren her bir e-posta adresi için bir e-posta gönderir her bir döngü için:

``` json
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')"
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                },
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` Eylem yineleme üzerinden dizi en çok 5000 satır.  Her yineleme paralel olarak varsayılan olarak yürütülür.  

### <a name="sequential-foreach-loops"></a>Sıralı ForEach döngüsü

Sırayla yürütmek foreach döngüsü etkinleştirmek için `Sequential` işlemi seçeneği eklenir.

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a>Döngü kadar
  
  Bir koşul yerine getirilene kadar bir eylem veya dizi eylem gerçekleştirebilir.  Aradığınız yanıt elde edene kadar Bunun en yaygın senaryo bir uç nokta çağırıyor.  Eklenecek belirtebilirsiniz Tasarımcısı'nda bir döngü kadar.  Döngü içinde eylemler eklendikten sonra çıkış koşulu olarak döngü ayarlayabileceğiniz sınırlar.  Döngü döngüleri arasında 1 dakikalık bir gecikmeyle yoktur.
  
  Kod görünümünü kullanıyorsanız, belirleyebileceğiniz bir döngü ister aşağıda kadar.  Bu yanıt gövdesi 'Tamamlandı' değerine sahip kadar bir HTTP uç noktası çağrılmadan bir örnektir.  Ne zaman tamamlanır ya da 
  
  * HTTP yanıtı 'Tamamlandı' durumuna sahip
  * 1 saat boyunca çalıştı
  * 100 kez döngüye
  
  ``` json
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed')",
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn ve debatching

Bazen bir tetikleyici bir dizi debatch ve bir iş akışı öğesi başına başlatmak istediğiniz öğeleri alabilirsiniz.  Bu aracılığıyla gerçekleştirilebilir `spliton` komutu.  Varsayılan olarak, tetikleyici swagger bir dizi bir yükü belirtiyorsa, bir `spliton` eklenir ve bir çalışma öğesi başına başlatın.  SplitOn yalnızca bir tetikleyiciye eklenebilir.  Bu el ile yapılandırılabildiğinden veya tanımı kod görünümünde geçersiz kılındı.  Şu anda SplitOn debatch en fazla 5000 öğeleri dizi.  Sahip olamaz bir `spliton` ve ayrıca zaman uyumlu yanıt desenini uygular.  Adında herhangi bir iş akışının sahip bir `response` ek olarak eylem `spliton` zaman uyumsuz olarak çalışacak ve hemen gönder `202 Accepted` yanıt.  

SplitOn kod görünümünde aşağıdaki örnek olarak belirtilebilir.  Bu öğeleri dizisini alır ve her satırda debatches.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequencey": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Kapsamları

Bir dizi eylem birlikte bir kapsamı kullanarak Grup mümkündür.  Özel durum işleme uygulamak için özellikle yararlıdır.  Tasarımcıda yeni bir kapsam ekleyin ve onu içinde herhangi bir eylem eklemeye başlamak.  Aşağıdaki gibi kod görünümünde kapsamları tanımlayabilirsiniz:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```