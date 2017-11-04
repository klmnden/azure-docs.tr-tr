---
title: "JSON - Azure Logic Apps ile iş akışları tanımlama | Microsoft Docs"
description: "İş akışı tanımları JSON'de logic apps için yazma"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 03/29/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 7f9e5a10066df8a464c285273e77a85c0d562ebb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a>JSON kullanarak logic apps için iş akışı tanımları oluşturma

İş akışı tanımları için oluşturabileceğiniz [Azure Logic Apps](logic-apps-what-are-logic-apps.md) basit ve bildirim temelli JSON dili. Henüz yapmadıysanız, ilk gözden [mantığı Uygulama Tasarımcısı ile ilk mantıksal uygulamanızı oluşturmak nasıl](logic-apps-create-a-logic-app.md). Ayrıca bkz [tam başvuru için iş akışı tanımlama dili](http://aka.ms/logicappsdocs).

## <a name="repeat-steps-over-a-list"></a>Bir liste arasındaki adımları yineleyin

10.000 öğelerine sahip bir dizi yinelemek ve her öğe için bir eylem gerçekleştirmek için kullanmanız [foreach türü](logic-apps-loops-and-scopes.md).

## <a name="handle-failures-if-something-goes-wrong"></a>Bir sorun yaşanırsa hataları işleme

Genellikle, dahil etmek istediğiniz bir *düzeltme adım* — yürütür bazı mantığı *ve yalnızca,* biri veya birkaçı çağrılarınızı başarısız. Bu örnek verileri çeşitli yerlerden alır, ancak çağrısı başarısız olursa, böylece biz daha sonra bu hata izleyebilirsiniz ileti yere POSTALAMA istiyoruz:  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "postToErrorMessageQueue": {
      "type": "ApiConnection",
      "inputs": "...",
      "runAfter": {
        "readData": [
          "Failed"
        ]
      }
    }
  },
  "outputs": {}
}
```

Belirtmek için `postToErrorMessageQueue` sonra yalnızca çalışan `readData` sahip `Failed`, kullanın `runAfter` olası değerler listesini belirtmek için özellik, örneğin, böylece `runAfter` olabilir `["Succeeded", "Failed"]`.

Bu örnek şimdi hata işlemesi nedeniyle son olarak, biz artık Çalıştır işaretlemek `Failed`. Bu örnekte bu hata işleme için adım eklediğimiz çünkü Çalıştır sahip `Succeeded` rağmen tek bir adımda `Failed`.

## <a name="execute-two-or-more-steps-in-parallel"></a>İki veya daha fazla adım Paralel yürütme

Paralel olarak birden çok eylem çalıştırmak için `runAfter` özelliği çalışma zamanında eşdeğer olmalıdır. 

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "kind": "http",
      "type": "Request"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

Bu örnekte, her ikisi de `branch1` ve `branch2` çalışacak şekilde ayarlanmış `readData`. Sonuç olarak, her iki dalları paralel olarak çalıştırın. Her iki dalları için zaman damgası aynıdır.

![Paralel](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a>İki paralel dalları katılma

Öğelerine ekleyerek paralel olarak çalışacak şekilde ayarlanmış iki eylem katılabilirsiniz `runAfter` özelliği önceki örnekte olduğu gibi.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {}
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "join": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "branch1": [
          "Succeeded"
        ],
        "branch2": [
          "Succeeded"
        ]
      }
    }
  },
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "Http",
      "inputs": {
        "schema": {}
      }
    }
  },
  "contentVersion": "1.0.0.0",
  "outputs": {}
}
```

![Paralel](media/logic-apps-author-definitions/join.png)

## <a name="map-list-items-to-a-different-configuration"></a>Liste öğeleri için farklı bir yapılandırma eşleme

Ardından, bir özellik değeri temel alınarak farklı içerik almak istiyoruz diyelim. Parametre olarak değerleri hedeflere haritasını oluşturabilir:  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "specialCategories": {
      "defaultValue": [
        "science",
        "google",
        "microsoft",
        "robots",
        "NSA"
      ],
      "type": "Array"
    },
    "destinationMap": {
      "defaultValue": {
        "science": "http://www.nasa.gov",
        "microsoft": "https://www.microsoft.com/en-us/default.aspx",
        "google": "https://www.google.com",
        "robots": "https://en.wikipedia.org/wiki/Robot",
        "NSA": "https://www.nsa.gov/"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "http"
    }
  },
  "actions": {
    "getArticles": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
      }
    },
    "forEachArticle": {
      "type": "foreach",
      "foreach": "@body('getArticles').responseData.feed.entries",
      "actions": {
        "ifGreater": {
          "type": "if",
          "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)",
          "actions": {
            "getSpecialPage": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
              }
            }
          }
        }
      },
      "runAfter": {
        "getArticles": [
          "Succeeded"
        ]
      }
    }
  }
}
```

Bu durumda, biz ilk makalelerin listesini alın. İkinci adım parametre olarak tanımlandı kategoriye göre içeriği almak için URL aramak için bir harita kullanır.

Burada dikkat edilecek bazı süresi: 

*   [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) İşlevi kategori bilinen tanımlı kategorilerden birini eşleşip eşleşmediğini denetler.

*   Biz kategori aldıktan sonra biz köşeli ayraç kullanarak harita öğesinden çekebilir:`parameters[...]`

## <a name="process-strings"></a>İşlem dizeleri

Dizeleri işlemek için çeşitli işlevleri kullanabilirsiniz. Örneğin, bir sisteme geçirmek için istiyoruz bir dize sahip olduğumuz ancak biz karakter kodlama için uygun işleme hakkında emin değilseniz varsayalım. Bir seçenektir base64 için bu dizesini kodlayın. Ancak, bir URL kaçış önlemek için sizi birkaç karakterlerini değiştirmek için adımıdır. 

İlk beş karakter olmayan kullanıldığından de sipariş adını alt dizeyi istiyoruz.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "orderer": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Çalışmasını içinde için dışında:

1. Alma [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) için sipariş eden'ın adı, böylece biz geri alma toplam karakter sayısı.

2. Daha kısa bir dize istiyoruz çünkü 5 çıkarın.

3. Aslında, ele [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring). Biz dizininde Başlat `5` ve dizenin geri kalanı gidin.

4. Bu alt dizeyi Dönüştür bir [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) dize.

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)tüm `+` ile karakterleri `-` karakter.

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)tüm `/` ile karakterleri `_` karakter.

## <a name="work-with-date-times"></a>Tarih süreleri ile çalışma

Özellikle doğal olarak desteklemeyen bir veri kaynağından veri almasına izin verirken tarih kez yararlı olabilir *Tetikleyicileri*. Tarih Saatler ne kadar çeşitli adımları kaplayan bulmak için de kullanabilirsiniz.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

Bu örnekte, biz ayıklamak `startTime` önceki adımdan. Biz geçerli saati almak ve bir ikinci çıkarma sonra:

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

İsterseniz zaman, diğer birimleri kullanabilirsiniz `minutes` veya `hours`. Son olarak, biz bu iki değer karşılaştırabilirsiniz. İlk değer ikinci değer sonra bir saniyeden küçükse sırasını ilk yerleştirilen geçen.

Tarihleri biçimlendirmek için dize biçimlendiricileri kullanabilirsiniz. Örneğin, RFC1123 almak için kullanırız [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). Tarih biçimlendirme hakkında bilgi edinmek için [iş akışı tanımlama dili](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).

## <a name="deployment-parameters-for-different-environments"></a>Farklı ortamlar için dağıtım parametreleri

Genellikle, bir geliştirme ortamı, hazırlık ortamı ve bir üretim ortamında dağıtım yaşam döngüleri vardır. Örneğin, aynı tanımın tüm bu ortamlarda ancak farklı veritabanlarını kullanır. Benzer şekilde, aynı tanımın farklı bölgeler arasında yüksek kullanılabilirlik için kullanın, ancak her logic app örneği bölgenin veritabanı ile iletişim kurmak istediğiniz isteyebilirsiniz.
Bu senaryo parametrelerinin alma farklı *çalışma zamanı* Burada bunun yerine, kullanmanız gereken `trigger()` önceki örnekte olduğu gibi işlev.

Bu örnek gibi temel bir tanımıyla başlatabilirsiniz:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "request": {
          "type": "request",
          "kind": "http"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Fiili olarak `PUT` isteği logic apps için parametre sağlayabilir `uri`. Varsayılan değeri artık mevcut olmadığından, bu parametre mantığı uygulama yükü gerektirir:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

Her bir ortamda için farklı bir değer sağlayabilir `connection` parametresi. 

Oluşturma ve mantıksal uygulamaları yönetmek için tüm seçenekleri için bkz: [REST API belgeleri](https://msdn.microsoft.com/library/azure/mt643787.aspx). 
