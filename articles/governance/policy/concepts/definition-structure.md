---
title: İlke tanım yapısı ayrıntıları
description: Kaynak ilke tanımı hangi etkili olması için zaman ilkelerin hiçbiri uygulanmaz ve açıklayarak, kuruluşunuzdaki kaynaklar için kuralları oluşturmak için Azure İlkesi tarafından nasıl kullanıldığını açıklar.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/13/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 03c7be9112ed22bb43e259fa72581d382a276163
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718188"
---
# <a name="azure-policy-definition-structure"></a>Azure İlkesi tanım yapısı

Kaynak ilke tanımları, Azure İlkesi, kaynaklar için kuralları oluşturmak için kullanılır. Her tanımı kaynak uyumluluğu ve uyumlu olmayan bir kaynak olduğunda gerçekleştirilecek ne efekt açıklar.
Kuralları tanımlayarak daha kolayca kaynaklarınızı yönetmek ve maliyetleri denetleyebilirsiniz. Örneğin, yalnızca belirli türlerdeki sanal makinelere izin verildiğini belirtebilirsiniz. Veya tüm kaynakların belirli bir etikete sahip gerektirebilir. İlkeleri, tüm alt kaynaklar tarafından devralınır. Bir kaynak grubu için bir ilke uygulanırsa, bu kaynak grubundaki tüm kaynaklar için geçerlidir.

Azure İlkesi tarafından kullanılan şema burada bulunabilir: [https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json](https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json)

Bir ilke tanımı oluşturmak için JSON kullanın. İlke tanımı yönelik öğeleri içerir:

- mode
- parametreler
- Görünen ad
- açıklama
- ilke kuralı
  - mantıksal değerlendirme
  - Etkin

Örneğin, aşağıdaki JSON kaynakların dağıtıldığı sınırlayan ilke gösterilmektedir:

```json
{
    "properties": {
        "mode": "all",
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                },
                "defaultValue": [ "westus2" ]
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

Tüm Azure ilkesi örnekleri altındadır [Azure ilkesi örnekleri](../samples/index.md).

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="mode"></a>Mod

**Modu** olduğu bir Azure Resource Manager özelliği veya bir kaynak sağlayıcısı özelliği ilkenin hedeflediği bağlı olarak yapılandırılmamış.

### <a name="resource-manager-modes"></a>Resource Manager modları

**Modu** hangi kaynak türlerinin bir ilke değerlendirileceğini belirler. İlişkin desteklenen modları şunlardır:

- `all`: kaynak grupları ve tüm kaynak türleri değerlendirin
- `indexed`: etiketlerini ve konumunu destekleyen kaynak türleri yalnızca değerlendirme

Ayarlamanızı öneririz **modu** için `all` çoğu durumda. Portalı kullanarak oluşturulan tüm ilke tanımlarını `all` modu. PowerShell veya Azure CLI kullanıyorsanız, belirtebilmeniz için **modu** parametresi el ile. İlke tanımı içermiyorsa bir **modu** değeri, varsayılan olarak için `all` Azure PowerShell ve çok `null` Azure clı'daki. A `null` modu kullanarak aynı olup `indexed` geriye dönük uyumluluğunu desteklemek için.

`indexed` etiketleri veya konumları zorunlu ilkeleri oluştururken kullanılmalıdır. Gerekli olmasa da, etiketler ve konumları olarak uyumluluk sonuçları uyumlu olmayan gösteren gelen desteklemeyen kaynakları engeller. Özel durum **kaynak grupları**. Konum veya bir kaynak grubu etiketleri takım politikaları ayarlamalıdır **modu** için `all` ve özellikle hedef `Microsoft.Resources/subscriptions/resourceGroups` türü. Bir örnek için bkz. [kaynak grubu etiketleri zorunlu](../samples/enforce-tag-rg.md). Etiketleri destekleyen kaynaklar listesi için bkz. [etiket Azure kaynakları için destek](../../../azure-resource-manager/tag-support.md).

### <a name="resource-provider-modes"></a>Kaynak sağlayıcısı modları

Şu anda desteklenen tek kaynak sağlayıcısı moddur `Microsoft.ContainerService.Data` üzerinde giriş denetleyicisine kuralları yönetmek için [Azure Kubernetes hizmeti](../../../aks/intro-kubernetes.md).

> [!NOTE]
> [Kubernetes için Azure İlkesi](rego-for-aks.md) genel Önizleme aşamasındadır ve yalnızca yerleşik ilke tanımları destekler.

## <a name="parameters"></a>Parametreler

Parametreleri, ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. Düşünme – form üzerinde alanları gibi parametrelerin `name`, `address`, `city`, `state`. Bu parametreleri her zaman aynı kalır, tek tek formu dolduran üzerinde tabanlı ancak bunların değerlerini değiştirin.
Parametreler, ilkeleri oluştururken aynı şekilde çalışır. İlke tanımında parametreler ekleyerek, farklı değerler kullanarak farklı senaryolar için bu ilkeyi yeniden kullanabilirsiniz.

> [!NOTE]
> Parametreler, var olan ve atanmış tanımına eklenebilir. Yeni bir parametre içermelidir **defaultValue** özelliği. Bu, ilke veya girişim mevcut atamaları dolaylı olarak geçersiz yapılmasını önler.

### <a name="parameter-properties"></a>Parametre özellikleri

Bir parametre ilke tanımında kullanılan aşağıdaki özelliklere sahiptir:

- **Ad**: Parametrenin adı. Tarafından kullanılan `parameters` ilke kuralı içinde dağıtım işlevi. Daha fazla bilgi için [bir parametre değeri kullanarak](#using-a-parameter-value).
- `type`: Parametre olup olmadığını belirleyen bir **dize**, **dizi**, **nesne**, **Boole**, **tamsayı**, **float**, veya **datetime**.
- `metadata`: Öncelikli olarak kullanıcı dostu bilgileri görüntülemek için Azure portal tarafından kullanılan alt tanımlar:
  - `description`: Hangi parametre açıklaması için kullanılır. Örnekler, kabul edilebilir değerler sağlamak için kullanılabilir.
  - `displayName`: Parametre için Portalı'nda gösterilen kolay adı.
  - `strongType`: (İsteğe bağlı) Portal üzerinden ilke tanımlarının atamasını yaparken kullanılır. Bağlam kullanan listesini sağlar. Daha fazla bilgi için [strongType](#strongtype).
  - `assignPermissions`: (İsteğe bağlı) Yap _true_ Azure portalında rol ataması sırasında ilke ataması oluşturmak için. Atama kapsamı dışında izin atamak istediğiniz durumlarda bu özellik yararlıdır. Bir rol ataması rol tanımında, ilke (veya rol tanımı girişim ilkelerinde tümünde başına) yoktur. Parametre değeri, geçerli bir kaynak veya kapsamında olması gerekir.
- `defaultValue`: (İsteğe bağlı) Hiçbir değer sağlanmışsa atamadaki parametresinin değerini ayarlar.
  Atanan var olan bir ilke tanımı güncelleştirilirken gereklidir.
- `allowedValues`: (İsteğe bağlı) Atama sırasında parametre kabul eden bir değer dizisi sağlar.

Örneğin, kaynakların dağıtıldığı konumları sınırlamak için bir ilke tanımı tanımlayabilirsiniz. Bu ilke tanımı için bir parametre olabilir **allowedLocations**. Bu parametre her ilke tanımı atama tarafından kabul edilen değerler sınırlamak için kullanılacak. Kullanımını **strongType** Portalı aracılığıyla atanabilecek tamamlarken Gelişmiş bir deneyim sağlar:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": [ "westus2" ],
        "allowedValues": [
            "eastus2",
            "westus2",
            "westus"
        ]
    }
}
```

### <a name="using-a-parameter-value"></a>Bir parametre değeri kullanma

Aşağıdaki parametrelerle ilke kuralında başvuru `parameters` dağıtım değer işlev sözdizimi:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

Bu örnek başvuran **allowedLocations** içinde devremenin parametre [parametre özellikleri](#parameter-properties).

### <a name="strongtype"></a>strongType

İçinde `metadata` kullanabileceğiniz özelliği **strongType** çoklu seçim listesini Azure portalındaki seçenekleri sağlamak için. İzin verilen değerler için **strongType** şu anda içerir:

- `location`
- `resourceTypes`
- `storageSkus`
- `vmSKUs`
- `existingResourceGroups`
- `omsWorkspace`
- `Microsoft.EventHub/Namespaces/EventHubs`
- `Microsoft.EventHub/Namespaces/EventHubs/AuthorizationRules`
- `Microsoft.EventHub/Namespaces/AuthorizationRules`
- `Microsoft.RecoveryServices/vaults`
- `Microsoft.RecoveryServices/vaults/backupPolicies`

## <a name="definition-location"></a>Tanım konumu

Oluşturulurken bir girişim veya tanımını konumu belirtmek gereklidir. Tanım konumu, bir yönetim grubu veya abonelik olmalıdır. Bu konum için girişim veya atanabilir kapsamı belirler. Kaynaklar doğrudan üyesi veya tanım konumunu hiyerarşi içinde alt atamanın hedef olmalıdır.

Tanım konumu c: ise

- **Abonelik** - yalnızca bu Abonelikteki kaynakların ilke atanabilir.
- **Yönetim grubu** - yalnızca alt Yönetim grupları ve alt abonelik içindeki kaynaklara ilke atanabilir. Konum, ilke tanımı birden fazla aboneliğe uygulayın planlıyorsanız, bu Aboneliklerdeki içeren yönetim grubu olması gerekir.

## <a name="display-name-and-description"></a>Görünen ad ve açıklama

Kullandığınız **displayName** ve **açıklama** ilke tanımını ve kullanıldığında için bağlam sağlar. **displayName** en fazla uzunluğu _128_ karakter ve **açıklama** uzunluğu en fazla _512_ karakter.

## <a name="policy-rule"></a>İlke kuralı

İlke kuralı oluşan **varsa** ve **ardından** engeller. İçinde **varsa** bloğu ne zaman ilkelerin hiçbiri uygulanmaz belirten bir veya daha fazla koşulları tanımlayın. Senaryo ilkesi için tam olarak tanımlamak için bu koşullar için mantıksal işleçler uygulayabilirsiniz.

İçinde **ardından** bloğu gerçekleşen etkisi tanımladığınız zaman **varsa** koşullar yerine.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists | disabled"
    }
}
```

### <a name="logical-operators"></a>Mantıksal işleçler

Desteklenen mantıksal işleçler şunlardır:

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

**Değil** söz dizimi koşul sonucunu tersine çevirir. **Tümü** sözdizimi (benzer şekilde mantıksal **ve** işlemi) tüm koşulların true olmasını gerektirir. **Herhangi** sözdizimi (benzer şekilde mantıksal **veya** işlemi) bir veya daha fazla koşul true olmasını gerektirir.

Mantıksal işleçler iç içe yerleştirebilirsiniz. Aşağıdaki örnekte gösterildiği bir **değil** içinde iç içe işlem bir **tümü** işlemi.

```json
"if": {
    "allOf": [{
            "not": {
                "field": "tags",
                "containsKey": "application"
            }
        },
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        }
    ]
},
```

### <a name="conditions"></a>Koşullar

Bir koşulu değerlendirir olup olmadığını bir **alan** veya **değer** erişimci belirli ölçütleri karşılayan. Desteklenen koşullar şunlardır:

- `"equals": "value"`
- `"notEquals": "value"`
- `"like": "value"`
- `"notLike": "value"`
- `"match": "value"`
- `"matchInsensitively": "value"`
- `"notMatch": "value"`
- `"notMatchInsensitively": "value"`
- `"contains": "value"`
- `"notContains": "value"`
- `"in": ["value1","value2"]`
- `"notIn": ["value1","value2"]`
- `"containsKey": "keyName"`
- `"notContainsKey": "keyName"`
- `"less": "value"`
- `"lessOrEquals": "value"`
- `"greater": "value"`
- `"greaterOrEquals": "value"`
- `"exists": "bool"`

Kullanırken **gibi** ve **notLike** koşullar, sağladığınız bir joker karakter `*` değerindeki değişikliği belirtir.
Birden fazla joker karakter değeri olmamalıdır `*`.

Kullanırken **eşleşen** ve **notMatch** koşulları sağlayan `#` bir rakam, eşleştirilecek `?` bir harfi için `.` herhangi bir karakter ve aynı diğer herhangi bir karakterle eşleştirmek için Bu gerçek bir karakter.
**eşleşen** ve **notMatch** büyük küçük harfe duyarlıdır. Büyük küçük harf duyarsız alternatifleri kullanılabilir **matchInsensitively** ve **notMatchInsensitively**. Örnekler için bkz [birkaç adı desenlerinin izin](../samples/allow-multiple-name-patterns.md).

### <a name="fields"></a>Alanlar

Koşullara alanları kullanılarak oluşturulur. Bir alan, kaynak isteği yükü özelliklerinde eşleştirir ve kaynak durumunu açıklar.

Aşağıdaki alanları desteklenir:

- `name`
- `fullName`
  - Kaynağın tam adını döndürür. Bir kaynağın tam adını, tüm üst kaynak adları (örneğin "myServer/Veritabanım") tarafından başına kaynak addır.
- `kind`
- `type`
- `location`
  - Kullanım **genel** konum bağımsız olan kaynaklar için. Bir örnek için bkz. [örnekleri - izin verilen Konumlar](../samples/allowed-locations.md).
- `identity.type`
  - Türünü döndüren [yönetilen kimliği](../../../active-directory/managed-identities-azure-resources/overview.md) kaynakta etkinleştirilmemiş.
- `tags`
- `tags['<tagName>']`
  - Bu ayracı sözdizimi, kısa çizgi, nokta veya boşluk gibi noktalama sahip etiket adları destekler.
  - Burada **\<tagName\>** doğrulamak için bir koşul için etiket adıdır.
  - Örnekler: `tags['Acct.CostCenter']` burada **Acct.CostCenter** etiketin adı.
- `tags['''<tagName>''']`
  - Bu köşeli ayraç söz dizimi ile çift kesme kaçış tarafından kesme sahip etiket adları destekler.
  - Burada **'\<tagName\>'** doğrulamak için bir koşul için etiket adıdır.
  - Örnek: `tags['''My.Apostrophe.Tag''']` burada **'\<tagName\>'** etiketin adı.
- özellik diğer adları - bir listesi için bkz [diğer adlar](#aliases).

> [!NOTE]
> `tags.<tagName>`, `tags[tagName]`, ve `tags[tag.with.dots]` etiketlerini alana bildirme hala kabul edilebilir yöntemlerdir. Ancak, tercih edilen ifade yukarıda listelenen olanlardır.

#### <a name="use-tags-with-parameters"></a>Parametrelerle etiketleri kullanma

Bir parametre değeri, bir etiket alanı geçirilebilir. Bir etiket alanı için bir parametre geçirerek, ilke ataması sırasında ilke tanımı'nın esnekliği artırır.

Aşağıdaki örnekte, `concat` değerini adlı etiket için etiket alanı arama oluşturmak için kullanılan **tagName** parametresi. Bu etiketi yoksa, **ekleme** etkisi denetlenen kaynakları üst kaynak grubunda ayarlamak için aynı adlı etiketi değerini kullanarak etiket eklemek için kullanılan `resourcegroup()` arama işlevi.

```json
{
    "if": {
        "field": "[concat('tags[', parameters('tagName'), ']')]",
        "exists": "false"
    },
    "then": {
        "effect": "append",
        "details": [{
            "field": "[concat('tags[', parameters('tagName'), ']')]",
            "value": "[resourcegroup().tags[parameters('tagName')]]"
        }]
    }
}
```

### <a name="value"></a>Value

Koşullar de biçimlendirilmiş kullanarak **değer**. **değer** koşullar karşı denetler [parametreleri](#parameters), [şablon işlevleri desteklenen](#policy-functions), ya da değişmez.
**değer** desteklenen herhangi biri ile eşleştirilmiş [koşul](#conditions).

> [!WARNING]
> Sonucu bir _şablon işlevi_ bir hata ilkesi değerlendirmesi başarısız olur. Başarısız bir değerlendirme bir örtük olarak **Reddet**. Daha fazla bilgi için [şablon hatalarını önleme](#avoiding-template-failures).

#### <a name="value-examples"></a>Değer örnekleri

Bu ilke kuralı örnekte **değer** sonucunu karşılaştırmak için `resourceGroup()` işlevi ve döndürülen **adı** özelliğini bir **gibi** koşulu`*netrg`. Kural olmayan herhangi bir kaynağa engellediği `Microsoft.Network/*` **türü** adı biten içinde herhangi bir kaynak grubunda `*netrg`.

```json
{
    "if": {
        "allOf": [{
                "value": "[resourceGroup().name]",
                "like": "*netrg"
            },
            {
                "field": "type",
                "notLike": "Microsoft.Network/*"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

Bu ilke kuralı örnekte **değer** birden çok sonuç, iç içe geçmiş denetlemek için işlevleri **eşittir** `true`. Kural, en az üç etiket yok herhangi bir kaynağa reddeder.

```json
{
    "mode": "indexed",
    "policyRule": {
        "if": {
            "value": "[less(length(field('tags')), 3)]",
            "equals": true
        },
        "then": {
            "effect": "deny"
        }
    }
}
```

#### <a name="avoiding-template-failures"></a>Şablon hatalarını önleme

Kullanımını _şablon işlevleri_ içinde **değer** için birçok karmaşık iç içe geçmiş işlevler sağlar. Sonucu bir _şablon işlevi_ bir hata ilkesi değerlendirmesi başarısız olur. Başarısız bir değerlendirme bir örtük olarak **Reddet**. Örneği bir **değer** belirli senaryolarda başarısız olur:

```json
{
    "policyRule": {
        "if": {
            "value": "[substring(field('name'), 0, 3)]",
            "equals": "abc"
        },
        "then": {
            "effect": "audit"
        }
    }
}
```

Örnek ilke kuralı kullanımlar yukarıda [substring()](../../../azure-resource-manager/resource-group-template-functions-string.md#substring) ilk üç karakterine Karşılaştırılacak **adı** için **abc**. Varsa **adı** üç karakterden daha kısa `substring()` işlevi hatayla sonuçlanır. İlke olacak bu hataya neden olur bir **Reddet** efekt.

Bunun yerine, [if()](../../../azure-resource-manager/resource-group-template-functions-logical.md#if) işlevi olmadığını denetlemek için ilk üç karakterine **adı** eşit **abc** izin olmadan bir **adı** kısa hataya neden olan üç karakter:

```json
{
    "policyRule": {
        "if": {
            "value": "[if(greaterOrEquals(length(field('name')), 3), substring(field('name'), 0, 3), 'not starting with abc')]",
            "equals": "abc"
        },
        "then": {
            "effect": "audit"
        }
    }
}
```

Düzeltilmiş ilke kuralı ile `if()` denetler **adı** alınmaya çalışılırken önce bir `substring()` Üçten az karakter içeren bir değeri. Varsa **adı** "abc ile başlamıyor" değeri yerine döndürdü ve karşılaştırıldığında çok kısa **abc**. İle başlamaz kısa bir ada sahip bir kaynak **abc** hala ilke kuralı başarısız olur, ancak artık değerlendirmesi sırasında bir hataya neden olur.

### <a name="effect"></a>Etki

Azure İlkesi etkisi aşağıdaki türlerini destekler:

- **Reddetme**: istek başarısız olur ve etkinlik günlüğüne bir olay oluşturur
- **Denetim**: etkinlik günlüğünde uyarı olayı oluşturur, ancak istek başarısız değil
- **Append**: dizi alanları isteği ekler
- **AuditIfNotExists**: bir kaynak mevcut değilse denetim sağlar
- **Deployıfnotexists**: zaten mevcut değilse bir kaynak dağıtır.
- **Devre dışı bırakılmış**: uyumluluk İlkesi kuralı için kaynakları değerlendirmez
- **EnforceRegoPolicy**: Açık İlke Aracısı kullandılar denetleyicisi Azure Kubernetes hizmeti (Önizleme) yapılandırır.

İçin **ekleme**, aşağıdaki ayrıntıları sağlamanız gerekir:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

Değer bir dize veya bir JSON biçimi nesnesi olabilir.

**AuditIfNotExists** ve **Deployıfnotexists** ilgili kaynağın var olup olmadığını değerlendirmek ve bir kural uygulayın. Kaynak kural eşleşmiyorsa etkisi uygulanır. Örneğin, tüm sanal ağları için Ağ İzleyicisi dağıtıldığını gerektirebilir. Daha fazla bilgi için [uzantı mevcut değilse denetim](../samples/audit-ext-not-exist.md) örnek.

**Deployıfnotexists** etkisi gerektirir **Roledefinitionıd** özelliğinde **ayrıntıları** ilke kuralı kısmı. Daha fazla bilgi için [düzeltme - ilke tanımı yapılandırma](../how-to/remediate-resources.md#configure-policy-definition).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscription/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

Değerlendirme, özellikler ve örnekler de sırasını her etkisi hakkında tüm ayrıntılar için bkz. [anlama Azure İlkesi etkileri](effects.md).

### <a name="policy-functions"></a>İlke işlevleri

Tüm [Resource Manager şablonu işlevleri](../../../azure-resource-manager/resource-group-template-functions.md) aşağıdaki işlevleri ve kullanıcı tanımlı işlevler dışında bir ilke kuralı içinde kullanmak kullanılabilir:

- copyındex)
- deployment()
- Liste *
- newGuid()
- pickZones()
- providers()
- Reference()
- resourceId()
- variables()

Aşağıdaki işlevleri, ilke kuralında kullanabilirsiniz, ancak bir Azure Resource Manager şablonu kullanımda farklı kullanılabilir:

- addDays (dateTime, numberOfDaysToAdd)
  - **dateTime**: [gerekli] dize - dize Evrensel ISO 8601 tarih saat biçiminde ' yyyy-aa-ddTHH:mm:ss.fffffffZ'
  - **numberOfDaysToAdd**: [gerekli] tamsayı - eklenecek gün sayısı
- utcNow() - aksine, bir Resource Manager şablonu, bu defaultValue dışında kullanılabilir.
  - Geçerli tarih ve saat Evrensel ISO 8601 tarih saat biçiminde şekilde ayarlanan bir dize döndürür ' yyyy-aa-ddTHH:mm:ss.fffffffZ'

Ayrıca, `field` işlevi ilke kuralları için kullanılabilir. `field` ile kullanılır **AuditIfNotExists** ve **Deployıfnotexists** değerlendirilmekte kaynak başvurusu alanlarında. Bu kullanım örneği görülebilir [Deployıfnotexists örnek](effects.md#deployifnotexists-example).

#### <a name="policy-function-example"></a>İlke işlevi örneği

Bu ilke kuralı örnekte `resourceGroup` almak için kaynak işlevi **adı** özelliği bir araya geldiğinde, `concat` oluşturmak için dizi ve nesne işlevi bir `like` başlatmak için kaynak adı zorlar durumu kaynak grubu adı ile.

```json
{
    "if": {
        "not": {
            "field": "name",
            "like": "[concat(resourceGroup().name,'*')]"
        }
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="aliases"></a>Diğer adlar

Bir kaynak türü için belirli özelliklerine erişmek için özelliği diğer adları kullanın. Diğer adlar, kaynak üzerinde bir özellik için hangi değerleri veya koşullara izin sınırlamak sağlar. Belirtilen kaynak türü için farklı bir API sürümlerinde yolları her diğer adın eşlenir. İlke değerlendirmesi sırasında ilke altyapısı bu API sürümü için özellik yolu alır.

Diğer adlar listesini her zaman artmaktadır. Hangi diğer adlar şu anda Azure İlkesi tarafından desteklenen bulmak için aşağıdaki yöntemlerden birini kullanın:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzAccount if not using Cloud Shell

  # Use Get-AzPolicyAlias to list available providers
  Get-AzPolicyAlias -ListAvailable

  # Use Get-AzPolicyAlias to list aliases for a Namespace (such as Azure Automation -- Microsoft.Automation)
  Get-AzPolicyAlias -NamespaceMatch 'automation'
  ```

- Azure CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # List namespaces
  az provider list --query [*].namespace

  # Get Azure Policy aliases for a specific Namespace (such as Azure Automation -- Microsoft.Automation)
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST API / ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

### <a name="understanding-the--alias"></a>[*] Diğer anlama

Birkaç kullanılabilir diğer adlarına sahip bir 'normal' bir ad ve başka görüntülenen bir sürümüne sahip **[\*]** eklediniz. Örneğin:

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

'Normal' diğer adı alanı, tek bir değer temsil eder. Bu alan tüm değerler kümesini tam olarak tanımlandığı gibi artık ve az olmalıdır tam eşleşme karşılaştırma senaryoları için kullanılır.

**[\*]** Diğer adı dizideki her öğenin değerini ve her öğenin belirli özelliklerini karşı Karşılaştırılacak mümkün kılar. Bu yaklaşım, öğe özelliklerini karşılaştırma mümkün kılar 'Hiçbiri ' ise 'varsa' veya ' tüm'ın ' senaryoları. Kullanarak **ipRules [\*]** , örnek, doğrulamak her _eylem_ olduğu _Reddet_, ancak kaç kuralları var veya hangi IP hakkındaendişelenmenizegerekyok_değer_ olduğu. Bu örnek kural eşleşme için denetler **ipRules [\*] .value** için **10.0.4.1** ve uygular **effectType** yalnızca en az bir eşleşme bulamazsa:

```json
"policyRule": {
    "if": {
        "allOf": [
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
                "notEquals": "10.0.4.1"
            }
        ]
    },
    "then": {
        "effect": "[parameters('effectType')]"
    }
}
```

Daha fazla bilgi için [değerlendirme [\*] diğer adı](../how-to/author-policies-for-arrays.md#evaluating-the--alias).

## <a name="initiatives"></a>Girişimler

Girişim atamaları ve yönetim grubu tek bir öğe olarak çalışmak için basitleştirmek için çeşitli ilgili ilke tanımlarını olanak sağlar. Örneğin, tek bir girişim etiketleme ilgili ilke tanımlarını gruplandırabilirsiniz. Tek tek her ilke atamak yerine, girişim uygulayın.

Aşağıdaki örnek iki etiketi işlemeye yönelik bir girişim oluşturma işlemini gösterir: `costCenter` ve `productName`. Varsayılan etiket değeri uygulamak için iki yerleşik ilkeleri kullanır.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [{
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md).
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](../how-to/programmatically-create.md).
- Bilgi edinmek için nasıl [uyumluluk verilerini alma](../how-to/getting-compliance-data.md).
- Bilgi edinmek için nasıl [uyumlu olmayan kaynakları düzeltme](../how-to/remediate-resources.md).
- Bir yönetim grubu olan gözden geçirme [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md).