---
title: "Etiketler için Azure kaynak ilkeleri | Microsoft Docs"
description: "Etiketler kaynaklardaki yönetmek için kaynak ilkeleri örnekleri sağlar"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: tomfitz
ms.openlocfilehash: 469bd8d637337e5900ea84c6bfaf88064695fb7e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="apply-resource-policies-for-tags"></a>Kaynak etiketleri için geçerlidir

Bu konu, etiketleri kaynaklarına tutarlı kullanımını sağlamak için uygulayabilirsiniz ortak ilke kuralları sağlar.

Bir kaynak grubuna veya aboneliğe mevcut kaynaklarla bir etiket ilkesi uygulamak firmalarda geriye dönük İlkesi kaynaklarla için geçerli değildir. Bu kaynaklar ilkelerini zorlamak için var olan kaynaklar için bir güncelleştirme tetikler. Bu makalede, bir güncelleştirme tetiklemek için bir PowerShell örnek içerir.

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a>Bir kaynak grubundaki tüm kaynakların bir etiketi/değer sahip olduğundan emin olun

Ortak gerekli bir kaynak grubundaki tüm kaynakların bir özel etiket ve değerine sahip değildir. Bu gereksinime genellikle maliyetleri izlemek için bölümünüzün gereklidir. Aşağıdaki koşullar karşılanmalıdır:

* Gerekli etiket ve değer etiketi olmayan yeni ve güncelleştirilmiş kaynaklara eklenir.
* Tüm mevcut kaynaklardan değeri ve gerekli etiketi kaldırılamaz.

Bu gereksinim, bir kaynak grubuna iki yerleşik ilkeleri uygulayarak gerçekleştirmek.

| Kimlik | Açıklama |
| ---- | ---- |
| 2a0e14a6-b0a6-4fab-991a-187a4f81c498 | Kullanıcı tarafından belirtilmediğinde gerekli bir etiket ve varsayılan değerini geçerlidir. |
| 1e30110a-5ceb-460c-a204-c1c3969c6d62 | Gerekli bir etiket ve değerini zorlar. |

### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell betiğini iki yerleşik ilke tanımları bir kaynak grubuna atar. Komut dosyasını çalıştırmadan önce tüm gerekli etiketleri kaynak grubuna atayın. Kaynak grubunda bulunan her bir etiketin grubundaki kaynaklar için gereklidir. Aboneliğinizdeki tüm kaynak grupları atamak için sağlıyor mu `-Name` kaynak gruplarını alırken parametresi.

```powershell
$appendpolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '2a0e14a6-b0a6-4fab-991a-187a4f81c498'}
$denypolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '1e30110a-5ceb-460c-a204-c1c3969c6d62'}

$rgs = Get-AzureRMResourceGroup -Name ExampleGroup

foreach($rg in $rgs)
{
    $tags = $rg.Tags
    foreach($key in $tags.Keys){
        $key 
        $tags[$key]
        New-AzureRmPolicyAssignment -Name ("append"+$key+"tag") -PolicyDefinition $appendpolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
        New-AzureRmPolicyAssignment -Name ("denywithout"+$key+"tag") -PolicyDefinition $denypolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
    }
}
```

İlkeleri atadıktan sonra eklediğiniz etiketi ilkelerini zorlamak için var olan tüm kaynaklar için bir güncelleştirme tetikleyebilir. Aşağıdaki komut dosyası kaynaklardaki varolan herhangi bir etiket korur:

```powershell
$group = Get-AzureRmResourceGroup -Name "ExampleGroup" 

$resources = Find-AzureRmResource -ResourceGroupName $group.ResourceGroupName 

foreach($r in $resources)
{
    try{
        $r | Set-AzureRmResource -Tags ($a=if($r.Tags -eq $NULL) { @{}} else {$r.Tags}) -Force -UsePatchSemantics
    }
    catch{
        Write-Host  $r.ResourceId + "can't be updated"
    }
}
```

## <a name="require-tags-for-a-resource-type"></a>Bir kaynak türü için etiketler gerektirir
Aşağıdaki örnek, bir uygulama etiketi yalnızca bir belirtilen kaynak türü (Bu durumda, depolama hesapları için) istemek için mantıksal işleçler iç içe gösterilmektedir.

```json
{
    "if": {
        "allOf": [
          {
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
    "then": {
        "effect": "audit"
    }
}
```

## <a name="require-tag"></a>Etiket gerektirir
Aşağıdaki ilke (herhangi bir değer uygulanabilir) "costCenter" anahtarı içeren bir etiket yok istekleri engeller:

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* (Yukarıdaki örneklerde gösterildiği gibi) bir ilke kuralı tanımladıktan sonra ilke tanımı oluşturun ve bir kapsama atamanız gerekir. Kapsamı bir abonelik, kaynak grubu veya kaynak olabilir. Portal üzerinden ilkeler atamak için bkz: [atamak ve kaynak ilkelerini yönetmek için kullanım Azure portal](resource-manager-policy-portal.md). REST API'si, PowerShell veya Azure CLI aracılığıyla ilkeleri atamak için bkz: [atayın ve komut dosyası aracılığıyla ilkelerini yönetme](resource-manager-policy-create-assign.md).
* Kaynak ilkelerini giriş için bkz: [kaynak ilkesine genel bakış](resource-manager-policy.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

