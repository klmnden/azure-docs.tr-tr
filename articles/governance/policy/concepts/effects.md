---
title: Azure İlkesi etkilerini anlama
description: Azure İlkesi tanım uyumluluk nasıl yönetildiği ve bildirilen belirleyen çeşitli etkileri vardır.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 54562401c830232d0a4bf90405cc5a2dbedcd8bc
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055977"
---
# <a name="understand-policy-effects"></a>İlke etkilerini anlama

Azure İlkesi her ilke tanımında ne zaman tarama sırasında belirler tek bir etkisi **varsa** ilke kuralı segmentini taranan kaynak eşleşecek şekilde değerlendirilir. Yeni bir kaynak, güncelleştirilmiş bir kaynak veya mevcut bir kaynak için olmaları durumunda etkileri farklı şekilde davranabilir.

Şu anda bir ilke tanımında desteklenen beş etkileri vardır:

- Ekle
- Denetim
- AuditIfNotExists
- Reddet
- Deployıfnotexists

## <a name="order-of-evaluation"></a>Değerlendirme sırası

Oluşturulacak veya güncelleştirilecek bir kaynak Azure Resource Manager aracılığıyla bir istek yapıldığında, ilke isteği uygun kaynak sağlayıcısı için teslim etmeden önce etkileri birkaç işler.
Bunun yapılması, kaynak İlkesi tasarlanmış idare denetimleri karşılamadığında gereksiz işleme kaynağı sağlayıcısı tarafından engeller. İlke, (eksi işaretini özel durumlar) kapsam tarafından kaynak için geçerlidir ve her tanımı karşı kaynak değerlendirmek hazırlayan bir ilke veya girişim ataması tarafından atanan tüm ilke tanımlarını listesini oluşturur.

- **Append** ilk olarak değerlendirilir. Beri ekleme isteği değiştirecek, göre ekleme yapılan bir değişikliği denetim engellemek veya tetikleme gelen etkisi reddet.
- **Reddetme** ardından değerlendirilir. Değerlendirerek reddetme denetim önce istenmeyen bir kaynağın çift günlük kaydı engellenir.
- **Denetim** giden kaynak sağlayıcıya isteği önce değerlendirilir.

İstek için kaynak sağlayıcısı sağlanır ve kaynak sağlayıcısı başarılı durum kodu döndürür. sonra **AuditIfNotExists** ve **Deployıfnotexists** izleme belirlemek için değerlendirilir Uyumluluk günlük kaydı veya eylem gerekli değildir.

## <a name="append"></a>Ekle

Append oluşturma veya güncelleştirme sırasında istenen kaynak için ek alanlar eklemek için kullanılır. CostCenter gibi kaynaklarda etiket eklemek için kullanışlı olabilir veya belirtme IP'ler için depolama kaynak izin verilmiyor.

### <a name="append-evaluation"></a>Değerlendirme Ekle

Belirtildiği gibi ekleme oluşturulması veya bir kaynağın güncelleştirilmesi sırasında bir kaynak sağlayıcısı tarafından işlenen isteği önce değerlendirilir. Append kaynağa alanları ekler, **varsa** ilke kuralının koşul karşılanıyorsa. Ekleme geçerli bir değer özgün istek farklı bir değerle geçersiz kılarsınız, reddetme etkisi davranır ve isteği reddedebilir.

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

Örnek 2: Birden çok **alan/değer** bir etiket kümesine eklenecek çiftleri.

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

Örnek 3: Tek **alan/değer** kullanarak pair bir [diğer](definition-structure.md#aliases) bir diziye sahip **değer** bir depolama hesabında IP kurallarını ayarlamak için.

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

## <a name="deny"></a>Reddet

Reddetme istek başarısız olur ve bir ilke tanımı aracılığıyla istenen standartlara eşleşmiyor kaynak isteğiyle önlemek için kullanılır.

### <a name="deny-evaluation"></a>Değerlendirme Reddet

Ne zaman oluşturulurken veya güncelleştirilirken bir kaynak Reddet önce kaynak sağlayıcısına gönderilen istek engeller. İstek bir 403 (Yasak) döndürülür. Portalda, Yasak bir ilke ataması nedeniyle engellendi dağıtım durumu olarak görüntülenebilir.

Bir değerlendirme döngüsü sırasında kaynaklarla eşleşen ilke tanımları reddetme etkisi ile uyumlu değil olarak işaretlenir ancak herhangi bir eylemi bu kaynak üzerinde gerçekleştirilir.

### <a name="deny-properties"></a>Özellikleri Reddet

Ek özellikleri kullanmak için reddetme etkisinin yok **ardından** ilke tanımının koşul.

### <a name="deny-example"></a>Örnek Reddet

Örnek: reddetme etkisinin kullanma.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Denetim

Denetim etkisiyle, uyumlu olmayan bir kaynak olarak kabul edilir, ancak istek durdurmaz etkinlik günlüğünde uyarı olayı oluşturmak için kullanılır.

### <a name="audit-evaluation"></a>Denetim değerlendirme

Oluşturma sırasında çalıştırmak için en son denetim etkisidir veya güncelleştirme kaynağı önce bir kaynağın kaynak sağlayıcısına gönderilir. Denetim için kaynak isteğiyle ve değerlendirme döngüsü aynı şekilde çalışır ve yürüten bir `Microsoft.Authorization/policies/audit/action` etkinlik günlüğü işlemi. Her iki durumda da, kaynak ile uyumsuz olarak işaretlenir.

### <a name="audit-properties"></a>Denetim Özellikleri

Denetim etkisiyle kullanılmak üzere herhangi bir ek özellik yok **ardından** ilke tanımının koşul.

### <a name="audit-example"></a>Denetim örneği

Örnek: denetim etkisiyle kullanma.

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>AuditIfNotExists

AuditIfNotExists eşleşen bir kaynak üzerinde denetim sağlar **varsa** koşulu, belirtilen bileşenleri yoksa ancak **ayrıntıları** , **ardından** koşul.

### <a name="auditifnotexists-evaluation"></a>AuditIfNotExists değerlendirme

Bir kaynak sağlayıcısı, bir kaynak oluşturma veya güncelleştirme isteği işlediği ve bir başarı durum kodu döndürdü sonra AuditIfNotExists çalıştırır. Etkisi yok ilgili kaynaklar varsa veya kaynaklar tarafından tanımlanan tetiklenir **ExistenceCondition** true olarak değerlendirmez. Etkisi tetiklendiğinde bir `Microsoft.Authorization/policies/audit/action` işlemi etkinlik günlüğüne denetim etkisiyle aynı şekilde yürütülür. Tetiklendiğinde, memnun kaynak **varsa** ile uyumsuz olarak işaretlenmiş kaynak bir durumdur.

### <a name="auditifnotexists-properties"></a>AuditIfNotExists özellikleri

**Ayrıntıları** eşleştirmek için ilgili kaynakları tanımlayan tüm alt AuditIfNotExists etkileri özelliğine sahiptir.

- **Tür** [gerekli]
  - Eşleştirmek için ilgili kaynak türünü belirtir.
  - Başlar altında bir kaynak getirilmeye çalışılırken tarafından **varsa** koşul kaynağı, ardından aynı kaynak grubunda sorgulara **varsa** koşul kaynağı.
- **Ad** (isteğe bağlı)
  - Eşleştirilecek kaynak tam adını belirtir ve belirli bir kaynak belirtilen türdeki tüm kaynakları yerine getirmek ilke neden olur.
- **ResourceGroupName** (isteğe bağlı)
  - İlişkili kaynağın farklı bir kaynak grubundan gelen eşleşen sağlar.
  - Uygulanmaz **türü** altında olan bir kaynağın **varsa** koşul kaynağı.
  - Varsayılan değer **varsa** kaynağın kaynak grubu koşul.
- **ExistenceScope** (isteğe bağlı)
  - İzin verilen değerler _abonelik_ ve _ResourceGroup_.
  - Gelen eşleştirmek için ilgili kaynak almak nereye kapsamını belirler.
  - Uygulanmaz **türü** altında olan bir kaynağın **varsa** koşul kaynağı.
  - İçin _ResourceGroup_, için sınırlar **varsa** koşul kaynağın kaynak grubu veya belirtilen kaynak grubu **ResourceGroupName**.
  - İçin _abonelik_, ilgili kaynak için tüm abonelik sorgular.
  - Varsayılan değer _ResourceGroup_.
- **ExistenceCondition** (isteğe bağlı)
  - Belirtilmezse, kaynağın ilgili **türü** etkisi karşılar ve denetim tetiklemez.
  - Aynı dil için ilke kuralı olarak kullandığı **varsa** koşul, ancak her bir ilgili kaynak karşı ayrı ayrı değerlendirilir.
  - Eşleşen tüm ilgili kaynakları true olarak değerlendirilirse, efekt sağlandığında ve denetim tetiklemez.
  - [Field()] değerlerle denklik denetlenecek kullanabilirsiniz **varsa** koşul.
  - Örneğin, doğrulamak için kullanılabilir üst kaynak (içinde **varsa** koşul) eşleşen ilgili kaynak ile aynı kaynak konumda olduğundan.

### <a name="auditifnotexists-example"></a>AuditIfNotExists örneği

Örnek: kötü amaçlı yazılımdan koruma uzantısını var, ardından eksik olduğunda denetimleri belirlemek için sanal makineleri değerlendirir.

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

Deployıfnotexists da bir kaynak sağlayıcısı oluşturmanız işlediği veya güncelleştirme isteği için bir kaynak ve bir başarı durum kodu döndürdü sonra çalışır. Etkisi yok ilgili kaynaklar varsa veya kaynaklar tarafından tanımlanan tetiklenir **ExistenceCondition** true olarak değerlendirmez. Şablon dağıtımı etkisi tetiklendiğinde yürütülür.

Bir değerlendirme döngüsü sırasında kaynaklarla eşleşen ilke tanımları Deployıfnotexists etkisi ile uyumlu değil olarak işaretlenir ancak herhangi bir eylemi bu kaynak üzerinde gerçekleştirilir.

### <a name="deployifnotexists-properties"></a>Deployıfnotexists özellikleri

**Ayrıntıları** eşleştirme için ilgili kaynakları tanımlayan tüm alt ve yürütmek için şablon dağıtımını Deployıfnotexists etkileri özelliğine sahiptir.

- **Tür** [gerekli]
  - Eşleştirmek için ilgili kaynak türünü belirtir.
  - Başlar altında bir kaynak getirilmeye çalışılırken tarafından **varsa** koşul kaynağı, ardından aynı kaynak grubunda sorgulara **varsa** koşul kaynağı.
- **Ad** (isteğe bağlı)
  - Eşleştirilecek kaynak tam adını belirtir ve belirli bir kaynak belirtilen türdeki tüm kaynakları yerine getirmek ilke neden olur.
- **ResourceGroupName** (isteğe bağlı)
  - İlişkili kaynağın farklı bir kaynak grubundan gelen eşleşen sağlar.
  - Uygulanmaz **türü** altında olan bir kaynağın **varsa** koşul kaynağı.
  - Varsayılan değer **varsa** kaynağın kaynak grubu koşul.
  - Şablon dağıtımı yürütülürse, bu değer kaynak grubunda dağıtılır.
- **ExistenceScope** (isteğe bağlı)
  - İzin verilen değerler _abonelik_ ve _ResourceGroup_.
  - Gelen eşleştirmek için ilgili kaynak almak nereye kapsamını belirler.
  - Uygulanmaz **türü** altında olan bir kaynağın **varsa** koşul kaynağı.
  - İçin _ResourceGroup_, için sınırlar **varsa** koşul kaynağın kaynak grubu veya belirtilen kaynak grubu **ResourceGroupName**.
  - İçin _abonelik_, ilgili kaynak için tüm abonelik sorgular.
  - Varsayılan değer _ResourceGroup_.
- **ExistenceCondition** (isteğe bağlı)
  - Belirtilmezse, kaynağın ilgili **türü** etkisi karşılar ve dağıtım tetiklemez.
  - Aynı dil için ilke kuralı olarak kullandığı **varsa** koşul, ancak her bir ilgili kaynak karşı ayrı ayrı değerlendirilir.
  - Eşleşen tüm ilgili kaynakları true olarak değerlendirilirse, efekt sağlandığında ve dağıtım tetiklemez.
  - [Field()] değerlerle denklik denetlenecek kullanabilirsiniz **varsa** koşul.
  - Örneğin, doğrulamak için kullanılabilir üst kaynak (içinde **varsa** koşul) eşleşen ilgili kaynak ile aynı kaynak konumda olduğundan.
- **roleDefinitionIds** [gerekli]
  - Bu özellik, rol tabanlı erişim denetimine rol kimliği erişilebilir tarafından eşleşen bir dize dizisi içermesi gerekir. Daha fazla bilgi için [düzeltme - ilke tanımı yapılandırma](../how-to/remediate-resources.md#configure-policy-definition).
- **Dağıtım** [gerekli]
  - Bu özellik için geçirilir gibi tam şablon dağıtımı içermelidir `Microsoft.Resources/deployments` API yerleştirin. Daha fazla bilgi için [dağıtımları REST API](/rest/api/resources/deployments).

  > [!NOTE]
  > Tüm işlevler içinde **dağıtım** özelliği, ilke şablonu bileşenleri olarak değerlendirilir. Özel durum **parametreleri** şablona ilkeden değerleri geçirir özelliği. **Değer** bu bölümünde bir şablon parametre adı geçirerek bu değer gerçekleştirmek için kullanılır (bkz _fullDbName_ Deployıfnotexists örnekte).

### <a name="deployifnotexists-example"></a>Deployıfnotexists örneği

Örnek: SQL Server veritabanlarını transparentDataEncryption etkin olup olmadığını belirlemek için değerlendirir. Aksi durumda, bunu etkinleştirmek için bir dağıtım yürütülür.

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
                    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Bir kaynak tarafından birden çok atamaları etkilenebilir. Bu atamaları farklı kapsamlarda veya aynı kapsamda (özel kaynak, kaynak grubu, abonelik veya yönetim grubu) olabilir. Her biri bu atamaları da tanımlanan farklı bir etkiye sahip olma olasılığı yüksektir. Ne olursa olsun, koşul ve etkili (doğrudan veya bir girişimin bir parçası olarak atanan) her ilke için bağımsız olarak değerlendirilir. Örneğin, yalnızca 'westus' reddetme etkisi ile oluşturulması bir abonelik için kaynak konumu kısıtlayan bir koşul İlkesi 1 içeriyor ve İlkesi 2 (aynı abonelikte A) kaynak konumu için kaynak grubu B için yalnızca kısıtlayan bir koşul olabilir 'eastus' denetim etkisi ile oluşturulan her ikisi de olan atanan, sonuçta elde edilen sonucu olacaktır:

- 'Myresourcegroup' kaynak grubunda B zaten kaynak İlkesi 2 için uyumlu, ancak 1 ilkeyle uyumlu olmayan olarak işaretli değildir.
- Kaynak grubu B 'eastus' de bulunan herhangi bir kaynağa 2 ilkeyle uyumlu olmayan olarak işaretlenir ve ayrıca 'westus' değilse 1 İlkesi uyumlu değil olarak işaretlenmiş.
- Bir abonelikte yeni bir kaynak 'westus' 1 ilke tarafından reddedilir değil.
- Bir abonelikte yeni bir kaynak / 'westus' kaynak grubunda B olmayan-2 ilkesindeki uyumlu olarak işaretlenmesini, ancak oluşturulacak (uyumlu ilke 1 ve 2 İlkesi denetim değil Reddet değil).

Hem ilke 1 ve 2 İlkesi vardı efekt, durum, deny değiştirmeniz gerekir:

- Kaynak grubu B 'eastus' de bulunan herhangi bir kaynağa 2 ilkeyle uyumlu olmayan olarak işaretlenir.
- Kaynak grubu B 'westus' de bulunan herhangi bir kaynağa 1 ilkeyle uyumlu olmayan olarak işaretlenir.
- Bir abonelikte yeni bir kaynak 'westus' 1 ilke tarafından reddedilir değil.
- Bir abonelikte yeni bir kaynak / kaynak grubu B izni verilmez bu yana (konumuna hiçbir zaman hem ilke 1 ve 2 İlkesi karşılamak).

Her atama ayrı ayrı değerlendirilir gibi kapsam farklılıkları nedeniyle bir boşluk ile notu için bir kaynak için bir fırsat yoktur. Bu nedenle, sonucunda katmanlama ilkeleri veya ilke çakışma olarak kabul edilir **toplu en kısıtlayıcı**. Diğer bir deyişle, ilke 1 ve 2 ilkesi hem reddetme etkisinin olsaydı oluşturulmasını istediğiniz bir kaynak Yukarıdaki örnek gibi çakışan ve çakışan ilkeler nedeniyle önlenebilir. Hedef kapsamı içinde oluşturulacak kaynağın yine de gerekliyse, doğru ilkeleri doğru kapsamlar etkileşimimiz emin olmak için her atamada dışlamaları gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md)
- Gözden geçirme [İlkesi tanım yapısı](definition-structure.md)
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](../how-to/programmatically-create.md)
- Bilgi edinmek için nasıl [uyumluluk verilerini al](../how-to/getting-compliance-data.md)
- Bulma nasıl [uyumlu olmayan kaynakları Düzelt](../how-to/remediate-resources.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz