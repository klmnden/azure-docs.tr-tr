---
title: "Döngüler ve kapsamları oluşturun ya da iş akışları - Azure Logic Apps verilerde debatch | Microsoft Docs"
description: "Veri, kapsamları, Grup eylemlere yinelemek için döngüler oluşturun veya Azure Logic Apps içinde daha fazla iş akışlarını başlatmak için veri debatch."
services: logic-apps
documentationcenter: .net,nodejs,java
author: ecfan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: estfan; LADocs
ms.openlocfilehash: 24a19ef7d93b4b0d006dfb0aed38f88d3d821d99
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logic Apps Döngüleri, Kapsamları ve Ayırma
  
Logic Apps, çeşitli yollarla dizilerle, koleksiyonlar, toplu işlemler, iş sağlar ve bir iş akışı içinde döngüye girer.
  
## <a name="foreach-loop-and-arrays"></a>ForEach döngüsü ve diziler
  
Logic Apps, bir veri kümesi üzerinde döngü ve her öğe için bir eylem gerçekleştirmek sağlar.  Bir koleksiyon döngü aracılığıyla olası `foreach` eylem.  Tasarımcıda eklediğiniz bir her döngü için.  Üzerinden yinelemek istediğiniz dizi seçtikten sonra Eylemler eklemeye başlayabilirsiniz.  Foreach döngüsü başına birden çok eylem ekleyebilirsiniz.  Bir kez döngü içinde dizinin her değerinde gerçekleşmesi gerektiğini belirtmek başlayabilirsiniz.

  Bu örnek için 'microsoft.com' içeren her bir e-posta adresine bir e-posta gönderir. Kod görünümünü kullanıyorsanız, belirleyebileceğiniz bir aşağıdaki örnekteki gibi her bir döngü için:

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
  
  A `foreach` eylem binlerce varlığın ile diziler üzerinden yineleme.  Yineleme paralel olarak varsayılan olarak yürütün.  Bkz: [sınırlarını ve yapılandırmasını](logic-apps-limits-and-config.md) dizi ve eşzamanlılık sınırları hakkında ayrıntılı bilgi için.

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
  
  Bir koşul yerine getirilene kadar bir eylem veya dizi eylem gerçekleştirebilir.  Kullanmak için en yaygın senaryo bir aradığınız yanıt elde edene kadar döngü bir uç nokta çağırmak kadar.  Eklenecek belirtebilirsiniz Tasarımcısı'nda bir döngü kadar.  Döngü içinde eylemler eklendikten sonra çıkış koşulu olarak döngü ayarlayabileceğiniz sınırlar.
  
  Yanıt gövdesi 'Tamamlandı' değerine sahip kadar bu örnek bir HTTP uç noktası çağırır.  Ne zaman tamamlar ya da: 
  
  * HTTP yanıtı 'Tamamlandı' durumuna sahip
  * Bir saat için çalıştı
  * 100 kez döngüye
  
  Kod görünümünü kullanıyorsanız, belirleyebileceğiniz bir döngü aşağıdaki örnekteki gibi kadar:
  
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

Bazen bir tetikleyici bir dizi debatch ve bir iş akışı öğesi başına başlatmak istediğiniz öğeleri alabilirsiniz.  Bu debatching aracılığıyla gerçekleştirilebilir `spliton` komutu.  Varsayılan olarak, tetikleyici swagger bir dizi bir yükü belirtiyorsa, bir `spliton` eklenir. `spliton` Komut dizideki her öğe bir farklı çalıştır başlatır.  SplitOn yalnızca el ile yapılandırılmış veya geçersiz bir tetikleyici eklenebilir. Sahip olamaz bir `spliton` ve ayrıca zaman uyumlu yanıt desenini uygular.  Adında herhangi bir iş akışının sahip bir `response` ek olarak eylem `spliton` zaman uyumsuz olarak çalışır ve hemen gönderir `202 Accepted` yanıt.  

  Bu örnek öğeleri dizisini alır ve her satırda debatches. SplitOn kod görünümünde aşağıdaki örnek olarak belirtilebilir:

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

## <a name="scopes"></a>Kapsamlar

Bir dizi eylem birlikte bir kapsamı kullanarak Grup mümkündür.  Kapsam, özel durum işleme uygulamak için faydalıdır.  Tasarımcıda yeni bir kapsam ekleyin ve onu içinde herhangi bir eylem eklemeye başlamak.  Aşağıdaki örneğe benzer kod görünümünde kapsamları tanımlayabilirsiniz:


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
