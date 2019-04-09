---
title: Dizi özellikleri Azure kaynakları için yazar ilkeleri
description: Dizi parametreleri oluşturmak, dizi için kuralları dili ifadeleri oluşturma [*] diğer değerlendirmek ve Azure İlkesi tanım kurallarına var olan bir dizi öğeleri eklemek için öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/06/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 38cf6decb8e61768faa9680058f6366e1550ba40
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274732"
---
# <a name="author-policies-for-array-properties-on-azure-resources"></a>Dizi özellikleri Azure kaynakları için yazar ilkeleri

Azure Resource Manager özellikleri, yaygın olarak dize ve Boole değerlerini tanımlanır. Bire çok ilişkisi mevcut olduğunda, karmaşık özellikler, bunun yerine dizileri olarak tanımlanır. Azure İlkesi'nde, diziler, birkaç farklı şekilde kullanılır:

- Türü bir [tanım parametresi](../concepts/definition-structure.md#parameters), birden çok seçenek belirtmelisiniz
- Parçası bir [ilke kuralı](../concepts/definition-structure.md#policy-rule) koşullarını kullanarak **içinde** veya **notIn**
- Veren bir ilke kuralının bir parçası [ \[ \* \] diğer](../concepts/definition-structure.md#understanding-the--alias) gibi belirli senaryoları değerlendirilecek **hiçbiri**, **herhangi**, veya  **Tüm**
- İçinde [efekt ekleme](../concepts/effects.md#append) değiştirin veya varolan bir diziye eklemek için

Bu makalede, Azure İlkesi tarafından her bir kullanım kapsar ve birkaç örnek tanımları sağlar.

## <a name="parameter-arrays"></a>Parametre dizileri

### <a name="define-a-parameter-array"></a>Bir parametre dizisi tanımlayın

Birden fazla değer gerektiğinde bir parametre bir dizi tanımlama ilke esnekliği sağlar.
Bu ilke tanımı parametresi için tek bir konum sağlayan **allowedLocations** ve varsayılan olarak _eastus2_:

```json
"parameters": {
    "allowedLocations": {
        "type": "string",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2"
    }
}
```

Olarak **türü** olduğu _dize_ne zaman bir değere ayarlanabilir yalnızca ilke atama. Bu ilke atanırsa, kapsam dahilindeki kaynaklar yalnızca tek bir Azure bölgesi içinde izin verilir. Çoğu ilke tanımları izin verme gibi onaylı seçeneklerini içeren liste için izin vermeniz _eastus2_, _eastus_, ve _westus2_.

Birden çok seçenek izin vermek için bir ilke tanımı oluşturmak için kullanın _dizi_ **türü**. Aynı ilke şu şekilde yeniden yazılabilir:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2",
        "allowedValues": [
            "eastus2",
            "eastus",
            "westus2"
        ]

    }
}
```

> [!NOTE]
> Bir ilke tanımı kaydedildikten sonra **türü** parametre özelliği değiştirilemez.

Bu yeni parametre tanımında, ilke ataması sırasında birden fazla değer alır. Dizi özelliği ile **allowedValues** ataması sırasında kullanılabilir değerler tanımlı, daha önceden tanımlanmış seçenekler listesine sınırlıdır. Kullanım **allowedValues** isteğe bağlıdır.

### <a name="pass-values-to-a-parameter-array-during-assignment"></a>Atama sırasında bir parametre dizisine değerlerini geçirme

Azure portalından, parametre olarak ilkeyi atarken ne **türü** _dizi_ tek bir metin kutusu olarak görüntülenir. İpucu "kullanır; söyler. değerleri birbirinden ayırmak için. (örneğin, Londra; New York) ". İzin verilen konum değerleri geçirmek için _eastus2_, _eastus_, ve _westus2_ parametre için şu dizeyi kullanın:

`eastus2;eastus;westus2`

Parametre değeri biçimi, Azure CLI, Azure PowerShell veya REST API kullanırken farklıdır. Değerleri, parametre adını içeren bir JSON dizesi geçirilir.

```json
{
    "allowedLocations": {
        "value": [
            "eastus2",
            "eastus",
            "westus2"
        ]
    }
}
```

Bu dize her SDK'sı ile kullanmak için aşağıdaki komutları kullanın:

- Azure CLI: Komut [az ilke ataması oluşturma](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) parametresiyle **params**
- Azure PowerShell: Cmdlet [yeni AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) parametresiyle **PolicyParameter**
- REST API: İçinde _PUT_ [oluşturma](/rest/api/resources/policyassignments/create) istek gövdesi bir parçası olarak bir işlem olarak değerini **properties.parameters** özelliği

## <a name="policy-rules-and-arrays"></a>İlke kuralları ve diziler

### <a name="array-conditions"></a>Dizi koşulları

İlke kuralı [koşullar](../concepts/definition-structure.md#conditions) , bir _dizi_
**türü** parametresinin kullanılabilir ile sınırlı olan `in` ve `notIn`. Koşul ile aşağıdaki ilke tanımı Al `equals` örnek olarak:

```json
{
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "equals": "[parameters('allowedLocations')]"
      }
    },
    "then": {
      "effect": "audit"
    }
  },
  "parameters": {
    "allowedLocations": {
      "type": "Array",
      "metadata": {
        "description": "The list of allowed locations for resources.",
        "displayName": "Allowed locations",
        "strongType": "location"
      }
    }
  }
}
```

Bu ilke tanımı bu hata iletisini gibi bir hata Azure portal müşteri adaylarının aracılığıyla oluşturulmaya çalışılıyor:

- "'{GUID}' İlkesi doğrulama hataları nedeniyle parametreli hale getirilemedi. Lütfen ilke parametrelerinin doğru tanımlanıp olmadığını kontrol edin. İç özel durum 'türü 'Array' dil ifadesi '[parameters('allowedLocations')]' sonucudur değerlendirme beklenen 'String' türü,'."

Beklenen **türü** koşulun `equals` olduğu _dize_. Bu yana **allowedLocations** olarak tanımlanan **türü** _dizi_, ilke altyapısı dil ifadesi değerlendirilir ve hata oluşturur. İle `in` ve `notIn` koşul, ilke altyapısı bekliyor **türü** _dizi_ dil ifadesi içinde. Bu hatayı gidermek için değiştirme `equals` ya da `in` veya `notIn`.

### <a name="evaluating-the--alias"></a>[*] Diğer ad değerlendirme

Sahip diğer adlar **[\*]** kendi adına bağlı belirtmek **türü** olduğu bir _dizi_. Tüm dizi değeri değerlendirme yerine **[\*]** dizinin her öğesi değerlendirilecek mümkün kılar. Bu öğe değerlendirme başına faydalıdır üç senaryo vardır: Hiçbiri, herhangi ve tüm.

İlke altyapısı Tetikleyiciler **etkisi** içinde **ardından** yalnızca **varsa** kural true değerlendirilir.
Bu olgu biçimini bağlamında anlamak önemlidir **[\*]** dizinin her öğesi değerlendirir.

Aşağıdaki senaryoda tablo için örnek ilke kuralı:

```json
"policyRule": {
    "if": {
        "allOf": [
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            <-- Condition (see table below) -->
        ]
    },
    "then": {
        "effect": "[parameters('effectType')]"
    }
}
```

**İpRules** dizi şu şekildedir senaryo için aşağıdaki tabloda:

```json
"ipRules": [
    {
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Her koşul için aşağıdaki örnekte, değiştirin `<field>` ile `"field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value"`.

Şu sonuçlardan koşulu ve örnek ilke kuralı ve yukarıdaki mevcut değerleri dizisi birleşimi sonucudur:

|Koşul |Sonuç |Açıklama |
|-|-|-|
|`{<field>,"notEquals":"127.0.0.1"}` |Hiçbir şey |Bir dizi öğesi yanlış olarak değerlendirilir (127.0.0.1! 127.0.0.1 =) ve true olarak (127.0.0.1! 192.168.1.1 =), bu nedenle **notEquals** koşul _false_ ve etkisi tetiklenmez. |
|`{<field>,"notEquals":"10.0.4.1"}` |İlke etkisi |İki dizi öğeleri doğru olarak değerlendirilmesini (10.0.4.1! = 127.0.0.1 ve 10.0.4.1! 192.168.1.1 =), bu nedenle **notEquals** koşul _true_ ve etkisi tetiklenir. |
|`"not":{<field>,"Equals":"127.0.0.1"}` |İlke etkisi |Bir dizi öğesinin true değerlendirilir (127.0.0.1 127.0.0.1 ==) ve false olarak (127.0.0.1 192.168.1.1 ==), bu nedenle **eşittir** koşul _false_. Mantıksal işleç doğru olarak değerlendirilir (**değil** _false_), etkisi tetiklenir. |
|`"not":{<field>,"Equals":"10.0.4.1"}` |İlke etkisi |İki dizi öğeleri false değerlendirme (10.0.4.1 127.0.0.1 ve 10.0.4.1 == 192.168.1.1 ==), bu nedenle **eşittir** koşul _false_. Mantıksal işleç doğru olarak değerlendirilir (**değil** _false_), etkisi tetiklenir. |
|`"not":{<field>,"notEquals":"127.0.0.1" }` |İlke etkisi |Bir dizi öğesi yanlış olarak değerlendirilir (127.0.0.1! 127.0.0.1 =) ve true olarak (127.0.0.1! = 192.168.1.1), bu nedenle **notEquals** koşul _false_. Mantıksal işleç doğru olarak değerlendirilir (**değil** _false_), etkisi tetiklenir. |
|`"not":{<field>,"notEquals":"10.0.4.1"}` |Hiçbir şey |İki dizi öğeleri doğru olarak değerlendirilmesini (10.0.4.1! = 127.0.0.1 ve 10.0.4.1! 192.168.1.1 =), bu nedenle **notEquals** koşul _true_. Mantıksal işleç false değerlendirilir (**değil** _true_), etkisi tetiklenmez. |
|`{<field>,"Equals":"127.0.0.1"}` |Hiçbir şey |Bir dizi öğesinin true değerlendirilir (127.0.0.1 127.0.0.1 ==) ve false olarak (127.0.0.1 192.168.1.1 ==), bu nedenle **eşittir** koşul _false_ ve etkisi tetiklenmez. |
|`{<field>,"Equals":"10.0.4.1"}` |Hiçbir şey |İki dizi öğeleri false değerlendirme (10.0.4.1 127.0.0.1 ve 10.0.4.1 == 192.168.1.1 ==), bu nedenle **eşittir** koşul _false_ ve etkisi tetiklenmez. |

## <a name="the-append-effect-and-arrays"></a>Append etkisi ve diziler

[Efekt ekleme](../concepts/effects.md#append) if bağlı olarak farklı davranır **details.field** olduğu bir **[\*]** diğer ad veya yok.

- Belirtilmediğinde bir **[\*]** diğer adı ekleme ile tüm dizi değiştirir **değer** özelliği
- Olduğunda bir **[\*]** diğer adı ekleme ekler **değer** varolan özellik dizisi veya yeni bir dizi oluşturur

Daha fazla bilgi için [append örnekleri](../concepts/effects.md#append-examples).

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md)
- Gözden geçirme [İlkesi tanım yapısı](../concepts/definition-structure.md)
- Gözden geçirme [ilke etkilerini anlama](../concepts/effects.md)
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](programmatically-create.md)
- Bilgi edinmek için nasıl [uyumlu olmayan kaynakları Düzelt](remediate-resources.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz