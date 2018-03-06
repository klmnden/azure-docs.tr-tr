---
title: "JSON - Azure Logic Apps ile mantıksal uygulama tanımları oluşturmak | Microsoft Docs"
description: "Parametre ekleme, dizeleri işlemek, parametre eşlemeleri oluşturma ve tarih işlevlerle veri al"
author: ecfan
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
ms.date: 01/31/2018
ms.author: LADocs; estfan
ms.openlocfilehash: d05f7e34cbe670db6733c199e3420c810c304a84
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="build-on-your-logic-app-definition-with-json"></a>Mantıksal uygulama tanımınızı JSON ile derleme

Daha fazla gerçekleştirmek için görevlerle Gelişmiş [Azure Logic Apps](../logic-apps/logic-apps-overview.md), basit ve bildirim temelli bir JSON dil kullanıyorsa, mantıksal uygulama tanımını düzenlemek için kod görünümü kullanabilirsiniz. Henüz yapmadıysanız, ilk gözden [ilk mantıksal uygulamanızı oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md). Ayrıca bkz [tam başvuru için iş akışı tanımlama dili](http://aka.ms/logicappsdocs).

> [!NOTE]
> Yalnızca mantığı uygulamanızın tanımı için kod görünümünde çalışırken parametreleri gibi bazı Azure Logic Apps özellikler kullanılabilir. Parametre değerleri mantıksal uygulamanızı boyunca yeniden olanak tanır. Örneğin, çeşitli eylemler arasında aynı e-posta adresini kullanmak istiyorsanız, bu e-posta adresi parametre olarak tanımlayın.

## <a name="view-and-edit-your-logic-app-definitions-in-json"></a>Görüntüleyip, mantığı uygulama tanımının JSON içinde düzenleyin

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın.

2. Sol menüden **daha fazla hizmet**. **Kurumsal Tümleştirme** altında **Logic Apps**’ı seçin. Mantıksal uygulamanızı seçin.

3. Mantıksal uygulama menüsünden altında **geliştirme araçları**, seçin **mantığı uygulama kod görünümü**.

   Kod Görünüm penceresi açar ve mantıksal uygulama tanımını gösterir.

## <a name="parameters"></a>Parametreler

Parametre değerleri mantıksal uygulamanızı boyunca yeniden sağlar ve genellikle değişebilir değerleri değiştirerek için iyi. Örneğin, birden çok yerde kullanmak istediğiniz bir e-posta adresi varsa, bu e-posta adresi parametre olarak tanımlamanız gerekir. 

Parametreleri de yararlı farklı ortamlarda parametreleri geçersiz gerektiğinde hakkında daha fazla bilgi [dağıtım için parametrelerin](#deployment-parameters) ve [Azure Logic Apps belge için REST API](https://docs.microsoft.com/rest/api/logic).

> [!NOTE]
> Parametreleri yalnızca kod görünümünde kullanılabilir.

İçinde [ilk örnek mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md), yeni gönderileri göründüğünde, bir Web sitesinin RSS akışı e-posta gönderen bir iş akışı oluşturuldu. Akışın URL kodlanmış, olduğundan, bu örnek akışın URL daha kolay geçiş yapabilmeniz sağlayan bir parametreyle sorgu değerini değiştirmek nasıl gösterir.

1. Kod görünümünde Bul `parameters : {}` nesne ve ekleme bir `currentFeedUrl` nesnesi:

   ``` json
     "currentFeedUrl" : {
      "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
   }
   ```

2. İçinde `When_a_feed-item_is_published` eylemi bulma `queries` bölümünde ve sorgu değeriyle `"feedUrl": "#@{parameters('currentFeedUrl')}"`. 

   **Önce**
   ``` json
   }
      "queries": {
          "feedUrl": "https://s.ch9.ms/Feeds/RSS"
       }
   },   
   ```

   **Sonra**
   ``` json
   }
      "queries": {
          "feedUrl": "#@{parameters('currentFeedUrl')}"
       }
   },   
   ```

   İki veya daha fazla katılmak için de kullanabilirsiniz `concat` işlevi. 
   Örneğin, `"@concat('#',parameters('currentFeedUrl'))"` önceki örnekteki gibi aynı şekilde çalışır.

3.  İşiniz bittiğinde **Kaydet**’i seçin. 

Web sitesinin RSS yoluyla farklı bir URL geçirerek akışı değiştirebileceğiniz artık `currentFeedURL` nesnesi.

<a name="deployment-parameters"></a>

## <a name="deployment-parameters-for-different-environments"></a>Farklı ortamlar için dağıtım parametreleri

Genellikle, geliştirme, hazırlama ve üretim ortamlarında dağıtım yaşam döngüleri vardır. Örneğin, aynı mantıksal uygulama tanımını tüm bu ortamlarda ancak farklı veritabanlarını kullanır. Benzer şekilde, aynı tanımın farklı bölgeler arasında yüksek kullanılabilirlik için kullanın. ancak bölgenin veritabanını kullanmak için her mantığını uygulaması örneğini istediğiniz isteyebilirsiniz. 

> [!NOTE] 
> Bu senaryo parametrelerinin alma farklı *çalışma zamanı* burada kullanmanız gereken `trigger()` yerine işlev.

Bir temel tanımı aşağıda verilmiştir:

``` json
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
Fiili olarak `PUT` isteği logic apps için parametre sağlayabilir `uri`. Her bir ortamda için farklı bir değer sağlayabilir `connection` parametresi. Varsayılan değeri artık mevcut olmadığından, bu parametre mantığı uygulama yükü gerektirir:

``` json
{
    "properties": {},
        "definition": {
          /// Use the definition from above here
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

Daha fazla bilgi için bkz: [Azure Logic Apps belge için REST API](https://docs.microsoft.com/rest/api/logic/).

## <a name="process-strings-with-functions"></a>İşlem işlevleriyle dizeleri

Logic Apps Dizelerle çalışmaya yönelik çeşitli işlevleri vardır. Örneğin, bir şirket adı başka bir sistem için bir sıra geçirmek istediğinizi varsayalım. Ancak, karakter kodlama için uygun işleme hakkında emin değilseniz. Bu dizesi base64 kodlaması gerçekleştirebilir, ancak URL'deki çıkışları önlemek için birkaç karakter bunun yerine değiştirebilirsiniz. İlk beş karakter olmayan kullanıldığından Ayrıca, yalnızca bir alt dizesi için şirket adını gerekir. 

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "companyName": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "Request",
      "kind": "Http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Bu adımları Bu örnek için dış içeriden çalışma Bu dize nasıl işlediği açıklanmıştır:

``` 
"uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
```

1. Alma [ `length()` ](../logic-apps/logic-apps-workflow-definition-language.md) şirket adını, böylece elde toplam karakter sayısı.

2. Daha kısa bir dize almak için çıkarma `5`.

3. Artık bir [ `substring()` ](../logic-apps/logic-apps-workflow-definition-language.md). Başlangıç dizininde `5`ve dizenin geri kalanı için gidin.

4. Bu alt dizeyi Dönüştür bir [ `base64()` ](../logic-apps/logic-apps-workflow-definition-language.md) dize.

5. Şimdi [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) tüm `+` ile karakterleri `-` karakter.

6. Son olarak, [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) tüm `/` ile karakterleri `_` karakter.

## <a name="map-list-items-to-property-values-then-use-maps-as-parameters"></a>Liste öğeleri özellik değerlerine eşlemek sonra eşlemeleri parametreleri olarak kullanma

Bir özelliğin değerini bağlı olarak farklı sonuçlar almak için her özellik değerini bir sonuç için eşleşen bir harita oluşturmak, sonra eşlenen bir parametre olarak kullanın. 

Örneğin, bu iş akışı parametreleri ve bu kategorilerin belirli bir URL ile eşleşen bir harita olarak bazı kategorileri tanımlar. İlk olarak, iş akışı makalelerin listesini alır. Ardından, iş akışı harita kategori her makale için eşleşen URL'sini bulmak için kullanır.

*   [ `intersection()` ](../logic-apps/logic-apps-workflow-definition-language.md) İşlevi kategori bilinen tanımlanmış bir kategorisi eşleşip eşleşmediğini denetler.

*   Eşleşen bir kategori edindikten sonra örnek köşeli ayraç kullanarak harita öğesinden çeker: `parameters[...]`

``` json
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

## <a name="get-data-with-date-functions"></a>Date işlevleri ile Veri Al

Yerel olarak desteklemeyen bir veri kaynağından veri almak için *Tetikleyicileri*, kullanabileceğiniz tarih işlevleri süreleri ile çalışmak için ve bunun yerine tarihleri. Örneğin, bu iş akışı adımları nasıl uzun sürüyor, bu deyim bulur içeriden dıştan çalışma:

``` json
"expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
```

1. Gelen `order` eylem, extract `startTime`. 
2. Geçerli zamanı ile elde `utcNow()`.
3. Bir saniye çıkarın:

   [`addseconds(..., -1)`](../logic-apps/logic-apps-workflow-definition-language.md) 

   İsterseniz zaman, diğer birimleri kullanabilirsiniz `minutes` veya `hours`. 

3. Şimdi, bu iki değer karşılaştırabilirsiniz. 

   İlk değer ikinci değer sonra bir saniyeden küçükse sırasını ilk yerleştirilen geçen.

Tarihleri biçimlendirmek için dize biçimlendiricileri kullanabilirsiniz. Örneğin, RFC1123 almak için kullanın [ `utcnow('r')` ](../logic-apps/logic-apps-workflow-definition-language.md). Daha fazla bilgi edinmek [tarih biçimlendirme](../logic-apps/logic-apps-workflow-definition-language.md).

``` json
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder-id"
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


## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre (koşullu deyimler) adımları çalıştırın](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlerini (switch deyimleri) temel alan adımları çalıştırın](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırma ve (döngüler) arasındaki adımları yineleyin](../logic-apps/logic-apps-control-flow-loops.md)
* [Çalıştırmak veya paralel adımları (dal) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplandırılmış eylem durumu (kapsam) temelinde adımları çalıştırın](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* Daha fazla bilgi edinmek [Azure mantıksal uygulamaları için iş akışı tanımlama dili şeması](../logic-apps/logic-apps-workflow-definition-language.md)
* Daha fazla bilgi edinmek [iş akışı eylemleri ve Azure Logic Apps için Tetikleyiciler](../logic-apps/logic-apps-workflow-actions-triggers.md)