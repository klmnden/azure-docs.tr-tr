---
title: "Şema güncelleştirmeleri Haziran-1-2016 - Azure Logic Apps | Microsoft Docs"
description: "Şema sürümü 2016-06-01 Azure Logic Apps için JSON tanımları oluşturma"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/25/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 525df7ddb8cd569bfd361da10d14ae08c1a721e0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a>Şema güncelleştirmeleri Azure Logic Apps için - 1 Haziran 2016

[Şema güncelleştirilmiş](https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json) ve Azure Logic Apps için API sürümü logic apps daha güvenilir ve kullanmayı daha kolay hale kilit iyileştirmeler içerir:

* [Kapsamları](#scopes) grup veya eylemler Eylemler koleksiyonu olarak iç içe sağlar.
* [Koşullar ve döngüler](#conditions-loops) birinci sınıf Eylemler sunulmuştur.
* Eylemler ile çalıştırmak için daha kesin sıralama `runAfter` özelliğini değiştirme`dependsOn`

Mantıksal uygulamalarınızı 1 Ağustos 2015 preview şemadan 1 Haziran 2016 şemasına yükseltmek için [yükseltme bölümü denetleyin](#upgrade-your-schema).

<a name="scopes"></a>

## <a name="scopes"></a>Kapsamlar

Bu şemayı Grup eylem birlikte veya iç içe Eylemler birbirine içinde izin kapsamları içerir. Örneğin, bir koşul başka bir koşul içerebilir. Daha fazla bilgi edinmek [kapsam sözdizimi](../logic-apps/logic-apps-loops-and-scopes.md), ya da bu temel kapsam örnek gözden geçirin:

```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

<a name="conditions-loops"></a>

## <a name="conditions-and-loops-changes"></a>Koşullar ve döngüler değişiklikleri

Önceki şemada, tek bir eylemle ilişkili parametreler sürümleri, koşulları ve döngüleri yoktu. Koşullar ve döngüler şimdi eylem türleri olarak görünmesi için bu sınırlama, bu şemayı kaldırır. Daha fazla bilgi edinmek [döngüler ve kapsamları](../logic-apps/logic-apps-loops-and-scopes.md), veya bir koşul eylemi temel bu örneğin gözden geçirin:

```
{
    "If_trigger_is_some-trigger": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'some-trigger')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_another-trigger": "..."
        }      
    }
}
```

<a name="run-after"></a>

## <a name="runafter-property"></a>'runAfter' özelliği

`runAfter` Özelliğini değiştirir `dependsOn`, Eylemler için çalışma sırasını belirttiğiniz zaman daha yüksek duyarlılık önceki Eylemler durumlarına dayalı sağlanması.

`dependsOn` Özelliği "eylemi çalıştırıldığı ve başarılı olup ile" eşanlamlı, Hayır istediğinizi önceki eylemi başarılı olup üzerinde göre bir eylem yürütmek için kaç kez başarısız oldu veya atlandı önemli. `runAfter` Özelliği nesne sonra çalıştığı tüm eylem adları belirten bir nesne olarak bu esneklik sağlar. Bu özellik ayrıca bir dizi Tetikleyicileri kabul edilebilir durumları tanımlar. Adım A başarılı ve ayrıca sonraki adım B başarılı veya başarısız sonra çalıştırmak istiyorsanız, örneğin, size bu oluşturmak `runAfter` özelliği:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a>Şemanızı yükseltme

Yükseltme için [en son şema](https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json), yalnızca birkaç adım yapması gerekmez. Yükseltme işlemi yükseltme komut dosyası çalıştıran içeren yeni bir mantıksal uygulama kaydetme ve isterseniz, büyük olasılıkla önceki mantıksal uygulama üzerine.

1. Azure portalda mantıksal uygulamanızı açın.

2. Git **genel bakış**. Mantıksal uygulama araç çubuğunda seçin **Şemayı Güncelleştir**.
   
    ![Şemayı Güncelleştir'i seçin][1]
   
    Yükseltilen tanımı, kopyalamak ve gerekirse, bir kaynak tanımı yapıştırma döndürülür. 
    Ancak, biz **tavsiye** seçtiğiniz **Kaydet** tüm bağlantı başvuruları yükseltilmiş mantıksal uygulama geçerli olduğundan emin olmak için.

3. Yükseltme dikey araç çubuğunda seçin **Kaydet**.

4. Durum ve mantığı adını girin. Yükseltilen mantıksal uygulamanızı dağıtmayı tercih **oluşturma**.

5. Yükseltilen mantıksal uygulamanızı beklendiği gibi çalıştığını onaylayın.
   
   > [!NOTE]
   > El ile veya istek tetikleyicisi kullanıyorsanız, geri çağırma URL'si yeni mantıksal uygulamanızı değiştirir. Uçtan uca deneyimi çalıştığından emin olmak için yeni URL sınayın. Önceki URL'leri korumak için var olan mantıksal uygulamanızı kopyalayabilirsiniz.

6. *İsteğe bağlı* yeni şema sürümüyle araç, önceki mantıksal uygulamanızı üzerine yazmayı seçin **kopya**, yanına **Şemayı Güncelleştir**. Bu adım, yalnızca aynı kaynak kimliği tutun veya mantıksal uygulamanızı tetikleyici URL'si istemek istiyorsanız gereklidir.

### <a name="upgrade-tool-notes"></a>Yükseltme Aracı notları

#### <a name="mapping-conditions"></a>Eşleme koşulları

Yükseltilen tanımında true ve false şube Eylemler birlikte gruplandırma kapsamı olarak en iyi çaba araç haline getirir. Özellikle, Tasarımcı desenini `@equals(actions('a').status, 'Skipped')` olarak görünmesi gereken bir `else` eylem. Ancak, aracı tanınmayan desenleri algılarsa, aracı true ve false dal için ayrı koşullar oluşturabilirsiniz. Gerekirse, yükseltme işleminin ardından, Eylemler eşleyebilirsiniz.

#### <a name="foreach-loop-with-condition"></a>'foreach' döngü koşuluna sahip

Yeni şemada filtre eylemi desenini çoğaltmak için kullanabileceğiniz bir `foreach` döngü her öğe, bir koşul ile ancak yükseltme yaptığınızda bu değişiklik otomatik olarak gerçekleştirilmesi. Koşulu yalnızca bir dizi koşul ile eşleşen öğe döndürmek için foreach döngüsü önce bir filtre eylemi olur ve bu diziyi foreach eyleme geçirilir. Bir örnek için bkz: [döngüler ve kapsamları](../logic-apps/logic-apps-loops-and-scopes.md).

#### <a name="resource-tags"></a>Kaynak etiketleri

Yükseltmeden sonra yükseltilen iş akışını sıfırlamanız gerekir böylece kaynak etiketleri kaldırılır.

## <a name="other-changes"></a>Diğer değişiklikler

### <a name="renamed-manual-trigger-to-request-trigger"></a>'İstek' tetikleyicisi 'manual' olarak yeniden adlandırıldı tetikleyiciye

`manual` Tetikleyici türü kullanım ve olarak yeniden adlandırıldı `request` türüyle `http`. Bu değişiklik desen türü için daha fazla tutarlılık oluşturan tetikleyici oluşturmak için kullanılır.

### <a name="new-filter-action"></a>Yeni 'filtre' eylemi

Öğelerinin, daha küçük bir kümesini kadar uzun bir diziye filtrelemek için yeni `filter` türü bir dizi ve koşulu kabul eder, her bir öğe koşulu değerlendirir ve bir dizi koşul toplantı öğeleriyle döndürür.

### <a name="restrictions-for-foreach-and-until-actions"></a>'Foreach' ve 'kadar' Eylemler kısıtlamalar

`foreach` Ve `until` döngü için tek bir eylem kısıtlı.

### <a name="new-trackedproperties-for-actions"></a>Eylemler için yeni 'trackedProperties'

Eylemler şimdi adlı bir ek özellik sahip `trackedProperties`, eşdüzeyi olduğu `runAfter` ve `type` özellikleri. Bu nesne, belirli bir eylem giriş veya iş akışının bir parçası gösterilen Azure tanılama telemetri dahil etmek istediğiniz çıkış belirtir. Örneğin:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Sonraki Adımlar
* [Logic apps için iş akışı tanımları oluşturma](../logic-apps/logic-apps-author-definitions.md)
* [Mantıksal uygulamalar için dağıtım şablonlar oluşturma](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
