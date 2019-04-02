---
title: Etkileri nasıl çalıştığını anlama
description: Azure İlkesi tanım uyumluluk nasıl yönetildiği ve bildirilen belirleyen çeşitli etkileri vardır.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/29/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: ae9c9c5ed8b951760ddac3034c617a13ebe35006
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58802652"
---
# <a name="understand-azure-policy-effects"></a>Azure İlkesi etkilerini anlama

Azure İlkesi her ilke tanımında, tek bir etkiye sahiptir. Bu etkiyi ilke kuralı eşleşecek şekilde değerlendirildiğinde ne olacağını belirler. Yeni bir kaynak, güncelleştirilmiş bir kaynak veya mevcut bir kaynak için olmaları durumunda etkileri farklı davranır.

Şu anda bir ilke tanımında desteklenen altı etkileri vardır:

- Ekle
- Denetim
- AuditIfNotExists
- Reddet
- Deployıfnotexists
- Devre dışı

## <a name="order-of-evaluation"></a>Değerlendirme sırası

İstekleri oluşturun veya Azure Resource Manager aracılığıyla kaynak güncelleştirme ilkesi tarafından önce değerlendirilir. İlke kaynağına uygulayın ve sonra kaynak her tanımı karşı değerlendirir tüm atamaları listesini oluşturur. İlke isteği uygun kaynak sağlayıcısı için teslim etmeden önce birkaç etkileri işler. Bunun yapılması, kaynak İlkesi tasarlanmış idare denetimleri karşılamadığında gereksiz işleme kaynağı sağlayıcısı tarafından engeller.

- **Devre dışı bırakılmış** önce ilke kuralı değerlendirileceğini belirlemek için denetlenir.
- **Append** ardından değerlendirilir. Beri ekleme isteği değiştirecek, göre ekleme yapılan bir değişikliği denetim engellemek veya tetikleme gelen etkisi reddet.
- **Reddetme** ardından değerlendirilir. Değerlendirerek reddetme denetim önce istenmeyen bir kaynağın çift günlük kaydı engellenir.
- **Denetim** giden kaynak sağlayıcıya isteği önce değerlendirilir.

Kaynak sağlayıcı bir başarı kodu döndürür sonra **AuditIfNotExists** ve **Deployıfnotexists** ek uyumluluk günlük kaydı veya eylem gerekli olup olmadığını belirlemek için değerlendirin.

## <a name="disabled"></a>Devre dışı

Bu etkiyi durumlarda test etmek veya ne zaman ilke tanımı etkisi parametreli için kullanışlıdır. Bu esneklik, bu ilkenin atamalarının tümünü devre dışı bırakmak yerine tek bir atama devre dışı bırakmak mümkün kılar.

## <a name="append"></a>Ekle

Append oluşturma veya güncelleştirme sırasında istenen kaynak için ek alanlar eklemek için kullanılır. Yaygın olarak karşılaşılan örneklerden costCenter gibi kaynaklarda etiket eklemek veya belirtme IP'ler için bir depolama kaynağına izin verilmiyor.

### <a name="append-evaluation"></a>Değerlendirme Ekle

Ekleme isteği, oluşturma veya bir kaynağın güncelleştirilmesi sırasında bir kaynak sağlayıcısı tarafından işlenen önce değerlendirilir. Append kaynağa alanları ekler, **varsa** ilke kuralının koşul karşılanıyorsa. Append etkili bir değer özgün istek farklı bir değerle geçersiz kılarsınız, reddetme etkisi davranır ve isteği reddeder. Yeni bir değer var olan bir diziye eklenecek kullanın **[\*]** diğer sürümü.

Append efekt kullanarak bir ilke tanımı bir değerlendirme döngüsü bir parçası olarak çalıştırdığınızda, zaten mevcut olan kaynakları değişiklik yapmaz. Bunun yerine, bunu karşılayan herhangi bir kaynağa işaretler **varsa** uyumsuz olarak koşul.

### <a name="append-properties"></a>Özellikler ekleme

Bir ekleme yalnızca etkisi bir **ayrıntıları** gerekli olan bir dizi. Olarak **ayrıntıları** bir dizi ya da tek bir sürebilir **alan/değer** bölümleri veya katları çifti. Başvurmak [tanım yapısı](definition-structure.md#fields) için kabul edilebilir alanların listesi.

### <a name="append-examples"></a>Append örnekleri

Örnek 1: Tek **alan/değer** bir etiket eklemek için çifti.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "tags.myTag",
        "value": "myTagValue"
    }]
}
```

Örnek 2: İki **alan/değer** bir etiket kümesine eklenecek çiftleri.

```json
"then": {
    "effect": "append",
    "details": [{
            "field": "tags.myTag",
            "value": "myTagValue"
        },
        {
            "field": "tags.myOtherTag",
            "value": "myOtherTagValue"
        }
    ]
}
```

Örnek 3: Tek **alan/değer** olmayan bir kullanarak pair **[\*]**
[diğer](definition-structure.md#aliases) bir diziye sahip **değer** IP kurallarını ayarlamak için bir Depolama hesabı. Zaman olmayan **[\*]** diğer bir dizi olan etkisi ekler **değer** tüm dizi. Dizi zaten varsa, reddetme olayı çakışma gerçekleşir.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
        "value": [{
            "action": "Allow",
            "value": "134.5.0.0/21"
        }]
    }]
}
```

Örnek 4: Tek **alan/değer** kullanarak pair bir **[\*]** [diğer](definition-structure.md#aliases) dizisiyle **değeri** bir depolama hesabında IP kurallarını ayarlamak için. Kullanarak **[\*]** diğer etkisi ekler **değer** potansiyel olarak önceden var olan bir dizi. Dizi olmayan henüz mevcut oluşturulur.

```json
"then": {
    "effect": "append",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]",
        "value": {
            "value": "40.40.40.40",
            "action": "Allow"
        }
    }]
}
```

## <a name="deny"></a>Reddet

Reddetme istek başarısız olur ve bir ilke tanımı ile tanımlanan standartları eşleşmiyor kaynak isteğiyle önlemek için kullanılır.

### <a name="deny-evaluation"></a>Değerlendirme Reddet

Ne zaman oluşturulurken veya güncelleştirilirken bir eşleşen kaynak Reddet isteği kaynak sağlayıcıya gönderilmeden önce engeller. İstek olarak döndürülen bir `403 (Forbidden)`. Portalda, Yasak bir ilke ataması tarafından engellendi dağıtım durumu olarak görüntülenebilir.

Var olan kaynakların değerlendirmesi sırasında bir reddetme ilke tanımı eşleşen kaynak uyumlu değil olarak işaretlenir.

### <a name="deny-properties"></a>Özellikleri Reddet

Ek özellikleri kullanmak için reddetme etkisinin yok **ardından** ilke tanımının koşul.

### <a name="deny-example"></a>Örnek Reddet

Örnek: Reddetme etkisinin kullanma.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Denetim

Denetim etkinlik günlüğünde, uyumlu olmayan bir kaynak değerlendirirken uyarı olayı oluşturmak için kullanılır, ancak istek bitmez.

### <a name="audit-evaluation"></a>Denetim değerlendirme

Denetim İlkesi tarafından oluşturulması veya bir kaynağın güncelleştirilmesi sırasında kullanıma son etkisidir. İlke, ardından kaynak kaynak sağlayıcısına gönderir. Denetim için kaynak isteğiyle ve değerlendirme döngüsü aynı şekilde çalışır. İlke ekler bir `Microsoft.Authorization/policies/audit/action` işlemi etkinlik günlüğüne ve kaynak uyumlu değil olarak işaretler.

### <a name="audit-properties"></a>Denetim Özellikleri

Ek özellikleri kullanmak için bir denetim etkisi yoktur **ardından** ilke tanımının koşul.

### <a name="audit-example"></a>Denetim örneği

Örnek: Denetim etkisiyle kullanma.

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>AuditIfNotExists

AuditIfNotExists sağlayan eşleşen kaynak denetim **varsa** koşulu, belirtilen bileşenleri yok ancak **ayrıntıları** , **ardından** koşul.

### <a name="auditifnotexists-evaluation"></a>AuditIfNotExists değerlendirme

Bir kaynak sağlayıcısı oluşturma veya güncelleştirme kaynak isteğiyle işlediği ve bir başarı durum kodu döndürdü sonra AuditIfNotExists çalıştırır. Hiçbir ilgili kaynaklar varsa veya kaynaklar tarafından tanımlanan denetim gerçekleşir **ExistenceCondition** doğru olarak değerlendirilebilmesi yok. İlke ekler bir `Microsoft.Authorization/policies/audit/action` işlemi etkinlik denetim etkin aynı şekilde oturum. Tetiklendiğinde, memnun kaynak **varsa** ile uyumsuz olarak işaretlenmiş kaynak bir durumdur.

### <a name="auditifnotexists-properties"></a>AuditIfNotExists özellikleri

**Ayrıntıları** eşleştirmek için ilgili kaynakları tanımlayan tüm alt AuditIfNotExists etkileri özelliğine sahiptir.

- **Tür** [gerekli]
  - Eşleştirmek için ilgili kaynak türünü belirtir.
  - Varsa **details.type** altında bir kaynak türü **varsa** koşul kaynağı, bu kaynaklar için ilke sorgular **türü** değerlendirilen kaynak kapsamında. Aksi durumda, ilke sorguları değerlendirilen kaynak ile aynı kaynak grubunda.
- **Ad** (isteğe bağlı)
  - Eşleştirilecek kaynak tam adını belirtir ve belirli bir kaynak belirtilen türdeki tüm kaynakları yerine getirmek ilke neden olur.
  - Koşul zaman değerleri **if.field.type** ve **then.details.type** , eşleşen **adı** olur _gerekli_ ve olmalıdır`[field('name')]`. Ancak, bir [denetim](#audit) efekt bunun yerine sayılacağı.
- **ResourceGroupName** (isteğe bağlı)
  - İlişkili kaynağın farklı bir kaynak grubundan gelen eşleşen sağlar.
  - Varsa geçerli değildir **türü** altında olan bir kaynağın **varsa** koşul kaynağı.
  - Varsayılan değer **varsa** kaynağın kaynak grubu koşul.
- **ExistenceScope** (isteğe bağlı)
  - İzin verilen değerler _abonelik_ ve _ResourceGroup_.
  - Gelen eşleştirmek için ilgili kaynak almak nereye kapsamını belirler.
  - Varsa geçerli değildir **türü** altında olan bir kaynağın **varsa** koşul kaynağı.
  - İçin _ResourceGroup_, için sınırlar **varsa** koşul kaynağın kaynak grubu veya belirtilen kaynak grubu **ResourceGroupName**.
  - İçin _abonelik_, ilgili kaynak için tüm abonelik sorgular.
  - Varsayılan değer _ResourceGroup_.
- **ExistenceCondition** (isteğe bağlı)
  - Belirtilmezse, kaynağın ilgili **türü** etkisi karşılar ve denetim tetiklemediğini.
  - Aynı dil için ilke kuralı olarak kullandığı **varsa** koşul, ancak her bir ilgili kaynak karşı ayrı ayrı değerlendirilir.
  - Eşleşen tüm ilgili kaynakları true olarak değerlendirilirse, efekt sağlandığında ve denetim tetiklemediğini.
  - [Field()] değerlerle denklik denetlenecek kullanabilirsiniz **varsa** koşul.
  - Örneğin, doğrulamak için kullanılabilir üst kaynak (içinde **varsa** koşul) eşleşen ilgili kaynak ile aynı kaynak konumda olduğundan.

### <a name="auditifnotexists-example"></a>AuditIfNotExists örneği

Örnek: Sanal makineler, kötü amaçlı yazılımdan koruma uzantısını var, ardından eksik olduğunda denetimleri belirlemek için değerlendirir.

```json
{
    "if": {
        "field": "type",
        "equals": "Microsoft.Compute/virtualMachines"
    },
    "then": {
        "effect": "auditIfNotExists",
        "details": {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "existenceCondition": {
                "allOf": [{
                        "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                        "equals": "Microsoft.Azure.Security"
                    },
                    {
                        "field": "Microsoft.Compute/virtualMachines/extensions/type",
                        "equals": "IaaSAntimalware"
                    }
                ]
            }
        }
    }
}
```

## <a name="deployifnotexists"></a>Deployıfnotexists

Koşul karşılandığında AuditIfNotExists benzeyen bir şablon dağıtımı Deployıfnotexists yürütür.

> [!NOTE]
> [İç içe şablonlar](../../../azure-resource-manager/resource-group-linked-templates.md#nested-template) ile desteklenen **Deployıfnotexists**, ancak [bağlı şablonları](../../../azure-resource-manager/resource-group-linked-templates.md) şu anda desteklenmiyor.

### <a name="deployifnotexists-evaluation"></a>Deployıfnotexists değerlendirme

Bir kaynak sağlayıcısı oluşturma veya güncelleştirme kaynak isteğiyle işlediği ve bir başarı durum kodu döndürdü sonra Deployıfnotexists çalıştırır. Hiçbir ilgili kaynaklar varsa veya kaynaklar tarafından tanımlanan bir şablon dağıtımı gerçekleşir **ExistenceCondition** doğru olarak değerlendirilebilmesi yok.

Bir değerlendirme döngüsü sırasında kaynaklarla eşleşen ilke tanımları Deployıfnotexists etkisi ile uyumlu değil olarak işaretlenir ancak bu kaynak üzerinde hiçbir işlem yapılmaz.

### <a name="deployifnotexists-properties"></a>Deployıfnotexists özellikleri

**Ayrıntıları** eşleştirme için ilgili kaynakları tanımlayan tüm alt ve yürütmek için şablon dağıtımını Deployıfnotexists etkileri özelliğine sahiptir.

- **Tür** [gerekli]
  - Eşleştirmek için ilgili kaynak türünü belirtir.
  - Başlar altında bir kaynak getirilmeye çalışılırken tarafından **varsa** koşul kaynağı, ardından aynı kaynak grubunda sorgulara **varsa** koşul kaynağı.
- **Ad** (isteğe bağlı)
  - Eşleştirilecek kaynak tam adını belirtir ve belirli bir kaynak belirtilen türdeki tüm kaynakları yerine getirmek ilke neden olur.
  - Koşul zaman değerleri **if.field.type** ve **then.details.type** , eşleşen **adı** olur _gerekli_ ve olmalıdır`[field('name')]`.
- **ResourceGroupName** (isteğe bağlı)
  - İlişkili kaynağın farklı bir kaynak grubundan gelen eşleşen sağlar.
  - Varsa geçerli değildir **türü** altında olan bir kaynağın **varsa** koşul kaynağı.
  - Varsayılan değer **varsa** kaynağın kaynak grubu koşul.
  - Şablon dağıtımı yürütülürse, bu değer kaynak grubunda dağıtılır.
- **ExistenceScope** (isteğe bağlı)
  - İzin verilen değerler _abonelik_ ve _ResourceGroup_.
  - Gelen eşleştirmek için ilgili kaynak almak nereye kapsamını belirler.
  - Varsa geçerli değildir **türü** altında olan bir kaynağın **varsa** koşul kaynağı.
  - İçin _ResourceGroup_, için sınırlar **varsa** koşul kaynağın kaynak grubu veya belirtilen kaynak grubu **ResourceGroupName**.
  - İçin _abonelik_, ilgili kaynak için tüm abonelik sorgular.
  - Varsayılan değer _ResourceGroup_.
- **ExistenceCondition** (isteğe bağlı)
  - Belirtilmezse, kaynağın ilgili **türü** etkisi karşılar ve dağıtımı tetikleyecek değil.
  - Aynı dil için ilke kuralı olarak kullandığı **varsa** koşul, ancak her bir ilgili kaynak karşı ayrı ayrı değerlendirilir.
  - Eşleşen tüm ilgili kaynakları true olarak değerlendirilirse, efekt sağlandığında ve dağıtımı tetikleyecek değil.
  - [Field()] değerlerle denklik denetlenecek kullanabilirsiniz **varsa** koşul.
  - Örneğin, doğrulamak için kullanılabilir üst kaynak (içinde **varsa** koşul) eşleşen ilgili kaynak ile aynı kaynak konumda olduğundan.
- **roleDefinitionIds** [gerekli]
  - Bu özellik, rol tabanlı erişim denetimine rol kimliği erişilebilir tarafından eşleşen bir dize dizisi içermesi gerekir. Daha fazla bilgi için [düzeltme - ilke tanımı yapılandırma](../how-to/remediate-resources.md#configure-policy-definition).
- **DeploymentScope** (isteğe bağlı)
  - İzin verilen değerler _abonelik_ ve _ResourceGroup_.
  - Tetiklenmesi için dağıtım türünü ayarlar. _Abonelik_ gösteren bir [abonelik düzeyinde dağıtım](../../../azure-resource-manager/deploy-to-subscription.md), _ResourceGroup_ bir kaynak grubuna bir dağıtım gösterir.
  - A _konumu_ özelliği içinde belirtilmelidir _dağıtım_ abonelik düzeyi dağıtımları kullanırken.
  - Varsayılan değer _ResourceGroup_.
- **Dağıtım** [gerekli]
  - Bu özellik için geçirilir gibi tam şablon dağıtımı içermelidir `Microsoft.Resources/deployments` API yerleştirin. Daha fazla bilgi için [dağıtımları REST API](/rest/api/resources/deployments).

  > [!NOTE]
  > Tüm işlevler içinde **dağıtım** özelliği, ilke şablonu bileşenleri olarak değerlendirilir. Özel durum **parametreleri** şablona ilkeden değerleri geçirir özelliği. **Değer** bu bölümünde bir şablon parametre adı geçirerek bu değer gerçekleştirmek için kullanılır (bkz _fullDbName_ Deployıfnotexists örnekte).

### <a name="deployifnotexists-example"></a>Deployıfnotexists örneği

Örnek: SQL Server veritabanlarını transparentDataEncryption etkin olup olmadığını belirlemek için değerlendirir. Aksi durumda, etkinleştirmek için bir dağıtım yürütülür.

```json
"if": {
    "field": "type",
    "equals": "Microsoft.Sql/servers/databases"
},
"then": {
    "effect": "DeployIfNotExists",
    "details": {
        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
        "name": "current",
        "roleDefinitionIds": [
            "/subscription/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
            "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
        ],
        "existenceCondition": {
            "field": "Microsoft.Sql/transparentDataEncryption.status",
            "equals": "Enabled"
        },
        "deployment": {
            "properties": {
                "mode": "incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "fullDbName": {
                            "type": "string"
                        }
                    },
                    "resources": [{
                        "name": "[concat(parameters('fullDbName'), '/current')]",
                        "type": "Microsoft.Sql/servers/databases/transparentDataEncryption",
                        "apiVersion": "2014-04-01",
                        "properties": {
                            "status": "Enabled"
                        }
                    }]
                },
                "parameters": {
                    "fullDbName": {
                        "value": "[field('fullName')]"
                    }
                }
            }
        }
    }
}
```

## <a name="layering-policies"></a>Katmanlama ilkeleri

Bir kaynak tarafından birkaç atamaları etkilenebilir. Bu atamaları, aynı kapsamda veya farklı kapsamlardaki olabilir. Her biri bu atamaları da tanımlanan farklı bir etkiye sahip olma olasılığı yüksektir. Her ilke için geçerli ve koşul bağımsız olarak değerlendirilir. Örneğin:

- İlke 1
  - Kaynak konumu 'westus' için sınırlar
  - Bir abonelik için atanan
  - Reddetme etkisi
- İlke 2
  - 'Myresourcegroup' kaynak konumuna kısıtlar
  - Bir Abonelikteki kaynak grubu B atanan
  - Denetim etkisi
  
Bu kurulum, aşağıdaki sonucu neden olur:

- 'Myresourcegroup' kaynak grubunda B zaten bulunan herhangi bir kaynağa uyumlu İlkesi 2 ve 1 ilkeyle uyumlu olmayan
- 2 ilkeyle uyumlu olmayan ve 1 değilse 'westus' ilkeye uyumlu olmayan herhangi bir kaynak zaten kaynak grubundaki B 'eastus' içinde değil
- Yeni bir kaynak değil 'westus', abonelik a 1 ilke tarafından reddedildi
- Oluşturulan ve 2 ilkesindeki uyumlu olmayan yeni bir kaynak bir aboneliği ve 'westus' kaynak grubunda B

Hem ilke 1 ve 2 İlkesi vardı efekt, durum değişikliklerini reddet:

- Kaynak grubu B 'eastus' de bulunan herhangi bir kaynak İlkesi 2 uyumlu değil
- Kaynak grubu B 'westus' de bulunan herhangi bir kaynak İlkesi 1 uyumlu değil
- Yeni bir kaynak değil 'westus', abonelik a 1 ilke tarafından reddedildi
- Abonelik a B kaynak grubundaki yeni bir kaynak engellendi

Her atama ayrı ayrı değerlendirilir. Bu nedenle, hiç bir kaynak için bir fırsat boşluk aracılığıyla irsaliyesi için kapsam farklarını öğesinden. Sonucunda katmanlama ilkeleri veya ilke çakışma olarak kabul edilir **toplu en kısıtlayıcı**. Örnek olarak, her iki ilke 1 ve 2 reddetme etkisinin aksine olsaydı çakışan ve çakışan ilkeleri tarafından kaynak engellenebilir. Kaynağın yine de gerekliyse, gözden geçirme hedef kapsamda oluşturulan doğru kapsamlar doğru ilkelerini doğrulamak için her atamada Dışlamalar etkiliyor olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md)
- Gözden geçirme [İlkesi tanım yapısı](definition-structure.md)
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](../how-to/programmatically-create.md)
- Bilgi edinmek için nasıl [uyumluluk verilerini al](../how-to/getting-compliance-data.md)
- Bilgi edinmek için nasıl [uyumlu olmayan kaynakları Düzelt](../how-to/remediate-resources.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz
