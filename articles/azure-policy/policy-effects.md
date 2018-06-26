---
title: Azure ilke etkilerini anlama
description: Uyumluluk nasıl yönetilir ve bildirilen belirleyen çeşitli efektler Azure ilke tanımı var.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 1566cf2b61749121c4eaff5a32b0a940f3341f7e
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751787"
---
# <a name="understanding-policy-effects"></a>İlke etkilerini anlama

Her ilke tanımı Azure ilkesinde ne zaman tarama sırasında olacağını belirler tek bir etkisi **varsa** segment ilke kuralının taranan kaynak eşleşecek şekilde değerlendirildi. Yeni bir kaynak, güncelleştirilmiş bir kaynak veya mevcut bir kaynağı iseler efektlerini farklı davranabilir.

Şu anda bir ilke tanımı'nda desteklenen beş etkileri vardır:

- Ekle
- Denetim
- AuditIfNotExists
- Reddet
- DeployIfNotExists

## <a name="order-of-evaluation"></a>Değerlendirme sırası

İlke oluşturmak veya bir kaynak aracılığıyla Azure Kaynak Yöneticisi'ni güncelleştirmek için bir istek yapıldığında, birkaç isteği uygun kaynak sağlayıcısı için teslim etmeden önce etkileri işler.
Bir kaynak ilkesinin tasarlanmış idare denetimleri karşılamadığında Bunun yapılması bir kaynak sağlayıcısı tarafından gereksiz işleme engeller. İlke kapsamı (eksi Dışlamalar) tarafından kaynağa uygulama ve her tanımı karşı kaynak değerlendirmek için hazırlanırken lütfen bekleyin, bir ilke veya girişimi atama tarafından atanan tüm ilke tanımları listesini oluşturur.

- **Append** ilk olarak değerlendirilir. Beri ekleme isteği alter, göre append yapılan bir değişikliği denetim önleme veya tetikleme gelen etkisi reddet.
- **Reddetme** sonra değerlendirilir. Değerlendirerek reddetme denetim önce istenmeyen bir kaynağın çift günlük engellenir.
- **Denetim** sonra kaynak sağlayıcıya giden istek önce değerlendirilir.

İstek için kaynak sağlayıcısı sağlanır ve kaynak sağlayıcısı başarılı durum kodu döndürür sonra **AuditIfNotExists** ve **DeployIfNotExists** izleme belirlemek için değerlendirilir Uyumluluk günlük veya eylem gerekli değildir.

## <a name="append"></a>Ekle

Append oluşturma veya güncelleştirme sırasında istenen kaynak için ek alanlar eklemek için kullanılır. Etiketler costCenter gibi kaynakları eklemek için faydalı olabilir veya belirtme IP'leri depolama kaynağı için izin verilmiyor.

### <a name="append-evaluation"></a>Değerlendirme ekleme

Belirtildiği gibi append oluşturma veya bir kaynağın güncelleştirilmesi sırasında kaynak sağlayıcısı tarafından işlenen isteği önce değerlendirir. Append kaynağa alan ekler zaman **varsa** ilke kuralı koşulu karşılandı. Append etkisi özgün istekteki bir değeri farklı bir değerle geçersiz kılarsınız, reddetme efekti olarak davranır ve isteğini reddetmek.

Append etkisi kullanarak bir ilke tanımı bir değerlendirme döngüsü bir parçası olarak çalıştırdığınızda, zaten mevcut kaynaklarına değişiklik yapmaz. Bunun yerine, karşılayan herhangi bir kaynağa işaretler **varsa** uyumsuz olarak koşul.

### <a name="append-properties"></a>Özellikler ekleme

Bir ekleme yalnızca etkisi bir **ayrıntıları** gerekli olan dizi. Olarak **ayrıntıları** bir dizi ya da tek bir sürebilir **alan/değer** çifti veya çarpan. Başvurmak [ilke tanımı](policy-definition.md#fields) için kabul edilebilir bir alanlar listesi.

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

Örnek 2: Birden çok **alan/değer** etiketleri kümesini eklenecek çiftleri.

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

Örnek 3: Tek **alan/değer** kullanarak eşleştirin bir [diğer](policy-definition.md#aliases) bir diziye sahip **değeri** bir depolama hesabında IP kuralları ayarlamak için.

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

Reddetme bir ilke tanımı yoluyla istenen standartları eşleşmiyor ve isteği başarısız bir kaynak isteği önlemek için kullanılır.

### <a name="deny-evaluation"></a>Değerlendirme Reddet

Ne zaman oluştururken veya güncelleştirirken bir kaynak Reddet önce kaynak sağlayıcıya gönderilen istek engeller. İstek 403 (Yasak) döndürülür. Portalın içinde Yasak ilke ataması nedeniyle engellendi dağıtım durumuna olarak görüntülenebilir.

Bir değerlendirme döngüsü sırasında kaynakları eşleşen ilke tanımları reddetme etkisi ile uyumsuz olarak işaretlenmiş, ancak hiçbir eylem, bu kaynağa gerçekleştirilir.

### <a name="deny-properties"></a>Özellikler Reddet

Reddetme etkisi kullanmak için herhangi bir ek özellik yoktur **sonra** ilke tanımı koşulu.

### <a name="deny-example"></a>Örnek Reddet

Örnek: reddetme etkisi kullanma.

```json
"then": {
    "effect": "deny"
}
```

## <a name="audit"></a>Denetim

Denetim etkin uyarı olayı Denetim günlüğüne uyumlu olmayan bir kaynak değerlendirilir, ancak istek durdurmaz oluşturmak için kullanılır.

### <a name="audit-evaluation"></a>Denetim değerlendirme

Oluşturma sırasında çalıştırmak için en son denetim etkin olduğu veya kaynak sağlayıcısı için gönderilen bir kaynağın kaynak önce güncelleştirme. Denetim bir kaynak isteği ve değerlendirme döngüsü için aynı şekilde çalışır ve yürüten bir `Microsoft.Authorization/policies/audit/action` etkinlik günlüğü işlemi. Her iki durumda da kaynak olarak uyumsuz olarak işaretlenir.

### <a name="audit-properties"></a>Denetim Özellikleri

Denetim etkisi kullanmak için herhangi bir ek özellik yoktur **sonra** ilke tanımı koşulu.

### <a name="audit-example"></a>Denetim örneği

Örnek: denetim etkisi kullanma.

```json
"then": {
    "effect": "audit"
}
```

## <a name="auditifnotexists"></a>AuditIfNotExists

AuditIfNotExists etkinleştirir eşleşen bir kaynak denetimi **varsa** koşul, belirtilen bileşenleri yok ancak **ayrıntıları** , **sonra** koşulu.

### <a name="auditifnotexists-evaluation"></a>AuditIfNotExists değerlendirme

Bir kaynak sağlayıcısı oluşturma veya güncelleştirme isteği bir kaynağa işlediği ve başarı durum kodunu döndürdü sonra AuditIfNotExists çalışır. Etkisi ilgili hiçbir kaynak ya da kaynaklar tarafından tanımlanmışsa tetiklenir **ExistenceCondition** true olarak değerlendirmez. Etkisi tetiklendiğinde bir `Microsoft.Authorization/policies/audit/action` etkinlik günlüğü işlemi denetim etkisi aynı şekilde gerçekleştirilir. Tetiklendiğinde, karşılanan kaynak **varsa** ile uyumsuz olarak işaretlenmiş kaynak bir durumdur.

### <a name="auditifnotexists-properties"></a>AuditIfNotExists özellikleri

**Ayrıntıları** AuditIfNotExists etkileri özelliğine eşleştirmek için ilgili kaynakları tanımlayan tüm alt özellikleri.

- **Tür** [gerekli]
  - Eşleştirilecek ilgili kaynak türünü belirtir.
  - Başlatır kaynağı altına getirmek deneyerek **varsa** koşul kaynak sonra aynı kaynak grubunda sorgulara **varsa** koşul kaynak.
- **Ad** (isteğe bağlı)
  - Eşleştirilecek kaynak tam adını belirtir ve belirli bir kaynak belirtilen türe ait tüm kaynakları yerine getirmek ilke neden olur.
- **ResourceGroupName** (isteğe bağlı)
  - Farklı kaynak grubundan gelmesini ilgili kaynak eşleşen sağlar.
  - Uygulanmaz **türü** altında olacak bir kaynaktır **varsa** koşul kaynak.
  - Varsayılan değer **varsa** kaynağın kaynak grubu koşul.
- **ExistenceScope** (isteğe bağlı)
  - İzin verilen değerler _abonelik_ ve _ResourceGroup_.
  - Gelen eşleştirmek için ilgili kaynağı getirmek nereye kapsamını ayarlar.
  - Uygulanmaz **türü** altında olacak bir kaynaktır **varsa** koşul kaynak.
  - İçin _ResourceGroup_, için sınırlandırır **varsa** koşul kaynağın kaynak grubu veya belirtilen kaynak grubu **ResourceGroupName**.
  - İçin _abonelik_, tüm abonelik ilişkili kaynak için sorgular.
  - Varsayılan değer _ResourceGroup_.
- **ExistenceCondition** (isteğe bağlı)
  - Belirtilmezse, tüm kaynak ilgili **türü** etkisi karşılayan ve denetim tetiklemez.
  - Aynı dil için ilke kuralı kullanan **varsa** koşul, ancak her ilgili kaynak karşı ayrı olarak değerlendirilir.
  - Herhangi bir eşleşen ilgili kaynak doğru olarak değerlendirir, etkisi memnuniyetini ve denetim tetiklemez.
  - [Field()] değerlerle eşdeğer denetlemek için kullanabilirsiniz **varsa** koşulu.
  - Örneğin, doğrulamak için kullanılabilecek üst kaynak (içinde **varsa** koşulu) eşleşen ilgili kaynak ile aynı kaynak konumda olan.

### <a name="auditifnotexists-example"></a>AuditIfNotExists örneği

Örnek: kötü amaçlı yazılımdan koruma uzantısı mevcut durumunda eksik olduğunda denetimleri belirlemek için sanal makineleri değerlendirir.

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

## <a name="deployifnotexists"></a>DeployIfNotExists

Koşul karşılandığında AuditIfNotExists benzeyen bir şablon dağıtımı DeployIfNotExists yürütür.

### <a name="deployifnotexists-evaluation"></a>DeployIfNotExists değerlendirme

DeployIfNotExists Ayrıca kaynak sağlayıcısı oluşturmanız işlediği veya güncelleştirme kaynağa istemek ve başarı durum kodunu döndürdü sonra çalışır. Etkisi ilgili hiçbir kaynak ya da kaynaklar tarafından tanımlanmışsa tetiklenir **ExistenceCondition** true olarak değerlendirmez. Şablon dağıtımı etkisi tetiklendiğinde yürütülür.

Bir değerlendirme döngüsü sırasında kaynakları eşleşen ilke tanımları DeployIfNotExists etkisi ile uyumsuz olarak işaretlenmiş, ancak hiçbir eylem, bu kaynağa gerçekleştirilir.

### <a name="deployifnotexists-properties"></a>DeployIfNotExists özellikleri

**Ayrıntıları** eşleşmesi için ilgili kaynakları tanımlayan tüm alt özellikleri ve yürütmek için şablon dağıtımı DeployIfNotExists etkileri özelliğine sahiptir.

- **Tür** [gerekli]
  - Eşleştirilecek ilgili kaynak türünü belirtir.
  - Başlatır kaynağı altına getirmek deneyerek **varsa** koşul kaynak sonra aynı kaynak grubunda sorgulara **varsa** koşul kaynak.
- **Ad** (isteğe bağlı)
  - Eşleştirilecek kaynak tam adını belirtir ve belirli bir kaynak belirtilen türe ait tüm kaynakları yerine getirmek ilke neden olur.
- **ResourceGroupName** (isteğe bağlı)
  - Farklı kaynak grubundan gelmesini ilgili kaynak eşleşen sağlar.
  - Uygulanmaz **türü** altında olacak bir kaynaktır **varsa** koşul kaynak.
  - Varsayılan değer **varsa** kaynağın kaynak grubu koşul.
  - Şablon dağıtımı yürütülürse, bu değer kaynak grubunda dağıtılır.
- **ExistenceScope** (isteğe bağlı)
  - İzin verilen değerler _abonelik_ ve _ResourceGroup_.
  - Gelen eşleştirmek için ilgili kaynağı getirmek nereye kapsamını ayarlar.
  - Uygulanmaz **türü** altında olacak bir kaynaktır **varsa** koşul kaynak.
  - İçin _ResourceGroup_, için sınırlandırır **varsa** koşul kaynağın kaynak grubu veya belirtilen kaynak grubu **ResourceGroupName**.
  - İçin _abonelik_, tüm abonelik ilişkili kaynak için sorgular.
  - Varsayılan değer _ResourceGroup_.
- **ExistenceCondition** (isteğe bağlı)
  - Belirtilmezse, tüm kaynak ilgili **türü** etkisi karşılayan ve dağıtım tetiklemez.
  - Aynı dil için ilke kuralı kullanan **varsa** koşul, ancak her ilgili kaynak karşı ayrı olarak değerlendirilir.
  - Herhangi bir eşleşen ilgili kaynak doğru olarak değerlendirir, etkisi memnuniyetini ve dağıtım tetiklemez.
  - [Field()] değerlerle eşdeğer denetlemek için kullanabilirsiniz **varsa** koşulu.
  - Örneğin, doğrulamak için kullanılabilecek üst kaynak (içinde **varsa** koşulu) eşleşen ilgili kaynak ile aynı kaynak konumda olan.
- **Dağıtım** [gerekli]
  - Bu özellik için aktarılabilecek gibi tam şablon dağıtımı içermelidir `Microsoft.Resources/deployments` API yerleştirin. Daha fazla bilgi için bkz: [dağıtımları REST API](/rest/api/resources/deployments).

  > [!NOTE]
  > İçindeki tüm işlevleri **dağıtım** özelliği, ilke şablonu bileşenleri olarak değerlendirilir. Özel durum **parametreleri** değerleri ilkesinden şablona Geçiren özelliği. **Değeri** Bu bölümde bir şablon altındaki parametre adı geçirme bu değer yapmak için kullanılır (bkz _fullDbName_ DeployIfNotExists örnekte).

### <a name="deployifnotexists-example"></a>DeployIfNotExists örneği

Örnek: transparentDataEncryption etkin olup olmadığını belirlemek için SQL Server veritabanlarını değerlendirir. Değilse, bunu etkinleştirmek için bir dağıtım sonra yürütülür.

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

Bir kaynak tarafından birden çok atamaları etkilenebilir. Bu atamaları aynı kapsamda (belirli bir kaynak, kaynak grubu, abonelik veya yönetim grubu) veya farklı kapsamlar olabilir. Her bu atamaları ayrıca tanımlanan farklı bir etkiye sahip olasılığı yüksektir. Ne olursa olsun, koşul ve etkili (doğrudan veya bir girişimi parçası olarak atanır) her ilke için bağımsız olarak değerlendirildiği. Örneğin, yalnızca reddetme etkisi olmadan 'westus' oluşturulması bir abonelik için kaynak konumu kısıtlayan bir koşul 1 ilkesi varsa ve 2 İlkesi (Bu abonelik A) kaynak konuma kaynak grubu B için yalnızca kısıtlayan bir koşulu olması 'eastus' denetim etkisi ile oluşturulan her ikisi de olan atanan, sonuçta elde edilen sonucu olacaktır::

- 'Eastus' kaynak grubunda B zaten kaynak İlkesi 2 uyumlu ancak uyumsuz İlkesi 1 olarak işaretli değildir.
- Herhangi bir kaynak zaten gruptaki kaynak B değil 'eastus' 2 ilkeyle uyumlu olmayan olarak işaretlenir ve ayrıca değil 'westus' değilse 1 İlkesi uyumlu işaretlenmesi.
- Bir Abonelikteki tüm yeni kaynak 'westus' 1 İlkesi tarafından engellenir değil.
- Bir Abonelikteki tüm yeni kaynak / 'westus' kaynak grubunda B olarak 2 İlkesi uyumlu olmayan işaretli, ancak oluşturulacak (ilke 1 ve 2 İlkesi denetleme ve değil reddetme uyumludur).

İlke 1 ve 2 İlkesi vardı efekt, reddetme için durum değiştirir:

- Herhangi bir kaynak zaten gruptaki kaynak B değil 'eastus' 2 ilkeyle uyumlu olmayan olarak işaretlenir.
- Herhangi bir kaynak zaten gruptaki kaynak B değil 'westus' 1 ilkeyle uyumlu olmayan olarak işaretlenir.
- Bir Abonelikteki tüm yeni kaynak 'westus' 1 İlkesi tarafından engellenir değil.
- Bir Abonelikteki tüm yeni kaynak / kaynak grubu B izni verilmez (konumuna hiçbir zaman İlkesi 1 ve 2 İlkesi karşılamak bu yana).

Her bir atama tek tek değerlendirilmesi gibi bir boşluk kapsam farklılıkları nedeniyle aracılığıyla notu için bir kaynak için bir fırsat değildir. Bu nedenle, katmanlama ilkeleri veya ilke çakışma net sonucu olarak kabul edilir **toplu en kısıtlayıcı**. Diğer bir deyişle, ilke 1 ve 2 İlkesi reddetme etkin olsaydı oluşturulmasını istediğiniz bir kaynağa yukarıdaki örnekte gibi çakışan ve çakışan ilkeleri nedeniyle önlenebilir. Hedef kapsamı içinde oluşturulacak kaynak hala gerekiyorsa, üzerinde sağ ilkeleri doğru kapsamlar etkileyen emin olmak için her bir atama dışarıda bırakılacak gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

İlke tanımı etkileri daha derin bir anlayış sahip olduğunuza göre ilke örnekleri gözden geçirin:

- [Azure İlkesi örnekleri](json-samples.md) sayfasındaki diğer örnekleri inceleyin.
