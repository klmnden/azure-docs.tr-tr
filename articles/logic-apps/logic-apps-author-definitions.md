---
title: Oluşturmak, düzenlemek veya JSON için mantıksal uygulama tanımları - Azure Logic Apps genişletin | Microsoft Docs
description: Yazar ve JSON için mantıksal uygulama tanımları Azure Logic apps'te genişletin.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.topic: article
ms.date: 01/01/2018
ms.openlocfilehash: daeb900abc3f24a408fc1b5f6e989e5181f2a463
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60427063"
---
# <a name="create-edit-or-extend-json-for-logic-app-definitions-in-azure-logic-apps"></a>Oluşturmak, düzenlemek veya JSON için mantıksal uygulama tanımları Azure Logic apps'te genişletin.

Kurumsal tümleştirme çözümleri ile oluşturduğunuzda, otomatik iş akışları [Azure Logic Apps](../logic-apps/logic-apps-overview.md), temel alınan mantıksal uygulama tanımları basit ve bildirim temelli JavaScript nesne gösterimi (JSON) ile birlikte kullanmak [ İş akışı tanımı dili (WDL) şeması](../logic-apps/logic-apps-workflow-definition-language.md) açıklaması ve doğrulama. Bu biçimler mantıksal uygulama tanımları okumanız ve anlamanız kod hakkında pek fazla bilmeden kolaylaştırır. Oluşturma ve logic apps dağıtımı otomatik hale getirmek istediğinizde, mantıksal uygulama tanımları olarak dahil edebileceğiniz [Azure kaynaklarını](../azure-resource-manager/resource-group-overview.md) içinde [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-overview.md#template-deployment). Oluşturma, yönetme ve logic apps dağıtmak için daha sonra [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.logicapp), [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md), veya [Azure Logic Apps REST API'lerini](https://docs.microsoft.com/rest/api/logic/).

Mantıksal uygulama tanımları json'da çalışmak için Azure portalında veya Visual Studio çalışırken kod görünüm düzenleyicisini açmak veya tanımını istediğiniz herhangi bir düzenleyiciye kopyalayın. Logic apps kullanmaya yeni başladıysanız gözden [ilk mantıksal uygulamanızı oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md).

> [!NOTE]
> Mantıksal uygulama tanımları parametre ve birden çok tetikleyici tanımlama gibi bazı Azure Logic Apps özellikleri, yalnızca JSON biçiminde, Logic Apps Tasarımcısı kullanılabilir.
> Bu nedenle bu görevler için kod görünümü veya başka bir düzenleyicide çalışmalıdır.

## <a name="edit-json---azure-portal"></a>JSON - Azure Portalı'nı Düzenle

1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a> oturum açın.

2. Sol menüden **tüm hizmetleri**. Arama kutusuna "logic apps" bulun ve ardından sonuçları, mantıksal uygulamanızı seçin.

3. Mantıksal uygulama menüsünde, altında **geliştirme araçları**seçin **mantıksal uygulama kod görünümü**.

   Kod Görünümü Düzenleyicisi açılır ve mantıksal uygulama tanımınızı JSON biçiminde gösterir.

## <a name="edit-json---visual-studio"></a>JSON - Visual Studio Düzenle

Visual Studio'da mantıksal uygulama tanımınızı üzerinde çalışmadan önce emin olun [gerekli araçlar yüklü](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#prerequisites). Visual Studio ile mantıksal uygulama oluşturmak için gözden [hızlı başlangıç: Azure Logic Apps - Visual Studio ile görev ve işlemleri otomatik hale getirmek](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

Visual Studio'da oluşturulan ve dağıtılan veya doğrudan Azure portalından Azure Resource Manager projeleri Visual Studio'dan logic apps açabilirsiniz.

1. Visual Studio çözümünü açmak veya [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) mantıksal uygulamanızı içeren bir proje.

2. Bulma ve açma görünür, varsayılan olarak, mantıksal uygulamanızın tanımını bir [Resource Manager şablonu](../azure-resource-manager/resource-group-overview.md#template-deployment), adlandırılmış **LogicApp.json**. Bu farklı ortamlar için dağıtım şablonu özelleştirme ve kullanabilirsiniz.

3. Mantıksal uygulama tanımını ve şablon için kısayol menüsünü açın. **Mantıksal Uygulama Tasarımcısı ile Aç**’ı seçin.

   ![Visual Studio çözümünde açık mantıksal uygulama](./media/logic-apps-author-definitions/open-logic-app-designer.png)

4. Tasarımcının en altında seçin **kod görünümü**. 

   Kod Görünümü Düzenleyicisi açılır ve mantıksal uygulama tanımınızı JSON biçiminde gösterir.

5. Kod Görünümü Düzenleyicisi altındaki Tasarımcı görünümü dönmek için **tasarım**.

## <a name="parameters"></a>Parametreler

Parametreler, mantıksal uygulamanızın tamamında değerleri yeniden kullanmanıza olanak tanır ve sık sık değişebilir değerler değiştirmek için uygundur. Örneğin, birden çok yerde kullanmak istediğiniz bir e-posta adresi varsa, bu e-posta adresi bir parametre olarak tanımlamanız gerekir.

Parametreleri de faydalı farklı ortamlarda parametrelerini geçersiz kıl gerektiğinde daha fazla bilgi edinin [dağıtım parametrelerini](#deployment-parameters) ve [Azure Logic Apps belgeleri için REST API](https://docs.microsoft.com/rest/api/logic).

> [!NOTE]
> Parametreleri yalnızca kod Görünümü'nde kullanılabilir.

İçinde [ilk örnek mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md), yeni gönderiler göründüğünde e-posta gönderen bir Web sitesinin RSS akışında bir iş akışı oluşturulur. Akışın URL kodlanmış, olduğundan bu örnek, akışın URL'si daha kolay değiştirebilmeniz sorgu değerini bir parametreyle değiştirmek gösterilmektedir.

1. Kod Görünümü'nde bulun `parameters : {}` nesne ve ekleme bir `currentFeedUrl` nesnesi:

   ``` json
   "currentFeedUrl" : {
      "type" : "string",
      "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
   }
   ```

2. İçinde `When_a_feed-item_is_published` eylemi bulma `queries` bölümünde ve sorgu değeriyle değiştirin `"feedUrl": "#@{parameters('currentFeedUrl')}"`.

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

   İki veya daha fazla dizeleri birleştirmek için de kullanabilirsiniz `concat` işlevi. 
   Örneğin, `"@concat('#',parameters('currentFeedUrl'))"` önceki örnekteki gibi aynı şekilde çalışır.

3.  İşiniz bittiğinde **Kaydet**’i seçin.

Web sitesinin RSS akışı farklı bir URL aracılığıyla geçirerek değiştirebilirsiniz artık `currentFeedURL` nesne.

<a name="deployment-parameters"></a>

## <a name="deployment-parameters-for-different-environments"></a>Farklı ortamlar için dağıtım parametreleri

Genellikle, geliştirme, hazırlama ve üretim ortamlarında dağıtım yaşam döngülerine sahip. Örneğin, aynı mantıksal uygulama tanımını tüm bu ortamlarda ancak farklı veritabanlarındaki kullanın. Benzer şekilde, aynı tanımın farklı bölgeler arasında yüksek kullanılabilirlik için kullanın. ancak istediğiniz her mantıksal uygulama örneği bölgenin veritabanı kullanmak isteyebilirsiniz.

> [!NOTE]
> Bu senaryo parametrelerinin fotoğrafını çekmenizi farklı *çalışma zamanı* burada kullanmanız gerektiğini `trigger()` işlevini.

Bir temel tanımı aşağıda verilmiştir:

``` json
{
    "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
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
Fiili olarak `PUT` logic apps için istek parametresi sağlayabilirsiniz `uri`. Her ortamda için farklı bir değer sağlayabilirsiniz `connection` parametresi. Varsayılan değeri artık mevcut olmadığından, mantıksal uygulama yükü bu parametre gerektirir:

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

Daha fazla bilgi için bkz. [Azure Logic Apps belgeleri için REST API](https://docs.microsoft.com/rest/api/logic/).

## <a name="process-strings-with-functions"></a>İşlem işlevleri ile dizeler

Logic Apps dizelerle çeşitli işlevleri vardır. Örneğin, bir şirket adı, bir sıra başka bir sisteme geçirmek istediğiniz varsayalım. Ancak, karakter kodlaması için uygun işleme hakkında emin değilseniz. Bu dizesine base64 kodlaması gerçekleştirebilir ancak kaçış karakterleri URL'ye önlemek için birkaç karakter yerine değiştirebilirsiniz. İlk beş karakteri değil kullanıldığından ayrıca yalnızca bir alt dizesi için şirket adı gerekir.

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
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
        "uri": "https://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Bu adımlar, bu örnek için dış içeriden çalışma Bu dize, nasıl işlediğini açıklar:

```
"uri": "https://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').companyName,5,sub(length(parameters('order').companyName), 5) )),'+','-') ,'/' ,'_' )}"
```

1. Alma [ `length()` ](../logic-apps/logic-apps-workflow-definition-language.md) için şirket adı, bu nedenle aldığınız toplam karakter sayısı.

2. Daha kısa bir dize almak için çıkarma `5`.

3. Artık bir [ `substring()` ](../logic-apps/logic-apps-workflow-definition-language.md). Başlangıç dizini `5`ve dizenin geri kalanı için gidin.

4. Dönüştürmek için bu alt dizenin bir [ `base64()` ](../logic-apps/logic-apps-workflow-definition-language.md) dize.

5. Artık [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) tüm `+` ile karakterleri `-` karakter.

6. Son olarak, [ `replace()` ](../logic-apps/logic-apps-workflow-definition-language.md) tüm `/` ile karakterleri `_` karakter.

## <a name="map-list-items-to-property-values-then-use-maps-as-parameters"></a>Liste öğeleri için özellik değerlerini eşleştirmek ve haritalar parametreleri olarak kullanma

Farklı sonuçlar elde etmek için bir özelliğinin değerini alarak, bir sonuç için her bir özellik değeri ile eşleşen bir harita oluşturun ve ardından eşleştiren bir parametre olarak kullanın.

Örneğin, bu iş akışı parametreleri ve belirli bir URL'ye sahip kategoriler eşleşen bir harita olarak bazı kategorileri tanımlar. İlk olarak, iş akışı makalelerin listesini alır. Daha sonra iş akışı, her bir makaleyi kategorisi eşleşen URL'sini bulmak için eşlemesini kullanır.

*   [ `intersection()` ](../logic-apps/logic-apps-workflow-definition-language.md) İşlevi kategorisi bilinen tanımlanmış bir kategori eşleşip eşleşmediğini denetler.

*   Örnek, eşleşen bir kategori aldıktan sonra köşeli ayraçlar kullanarak eşleme maddeden çeker: `parameters[...]`

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
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
        "science": "https://www.nasa.gov",
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
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=https://feeds.wired.com/wired/index"
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

## <a name="get-data-with-date-functions"></a>Tarih işlevleri ile veri alma

Yerel olarak desteklemeyen bir veri kaynağından veri almanın *Tetikleyicileri*, kullanabileceğiniz tarih işlevleri süreleri ile çalışmak için ve bunun yerine tarihleri. Örneğin, bu iş akışının adımları nasıl uzun sürüyor, bu ifade bulana içeriden dıştan çalışma:

``` json
"expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
```

1. Gelen `order` eylemi, extract `startTime`.
2. Geçerli saati ile alma `utcNow()`.
3. Bir saniye çıkarın:

   [`addseconds(..., -1)`](../logic-apps/logic-apps-workflow-definition-language.md) 

   Diğer zaman birimleri gibi kullanabileceğiniz `minutes` veya `hours`.

3. Şimdi, bu iki değerden karşılaştırabilirsiniz. 

   İlk değer, ikinci değer ve ardından bir saniyeden daha az ise ilk siparişi veren bu yana geçen süreyi.

Tarihleri biçimlendirmek için dize biçimlendiricileri kullanabilirsiniz. Örneğin, RFC1123 almak için kullanın [ `utcnow('r')` ](../logic-apps/logic-apps-workflow-definition-language.md). Daha fazla bilgi edinin [tarih biçimlendirme](../logic-apps/logic-apps-workflow-definition-language.md).

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
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
        "uri": "https://www.example.com/?id=@{parameters('order').id}"
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
            "uri": "https://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
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

* [Bir koşula göre (koşullu deyimler) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlere (switch deyimleri) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırma ve yineleme adımları (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Çalıştırın veya paralel adımları (dallar) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
* [Gruplandırılmış eylem durumu (kapsamları) temelinde adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
* Daha fazla bilgi edinin [Azure Logic Apps iş akışı tanımı dil şeması](../logic-apps/logic-apps-workflow-definition-language.md)
* Daha fazla bilgi edinin [iş akışı eylemleri ve Azure Logic Apps için Tetikleyiciler](../logic-apps/logic-apps-workflow-actions-triggers.md)
