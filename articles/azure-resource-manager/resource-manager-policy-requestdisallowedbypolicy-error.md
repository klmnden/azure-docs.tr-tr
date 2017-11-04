---
title: "Azure kaynak İlkesi RequestDisallowedByPolicy hatasıyla | Microsoft Docs"
description: "RequestDisallowedByPolicy hatanın nedenini açıklar."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: c950fe5444d4eacf3568d355a8dc9c5699645229
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Azure kaynak ilkesiyle RequestDisallowedByPolicy hata

RequestDisallowedByPolicy hatanın nedeni, bu makalede, ayrıca bu hata için çözüm sağlar.

## <a name="symptom"></a>Belirti

Dağıtım sırasında bir eylem yapmak çalıştığınızda alabileceğiniz bir **RequestDisallowedByPolicy** tamamlanmasını engellediğini hata. Aşağıdaki örnek, hatayı gösterir:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Sorun giderme

Dağıtımınızı engellenen İlkesi hakkındaki ayrıntıları almak için aşağıdaki yöntemlerden birini kullanın:

### <a name="method-1"></a>Yöntem 1

Bu ilke tanımlayıcısı olarak PowerShell'de sağlamak `Id` dağıtımınızı engellenen İlkesi hakkındaki ayrıntıları almak için parametre.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a>Yöntem 2 

Azure CLI 2. 0'ilke tanımı adını sağlayın: 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Çözüm

Güvenlik veya uyumluluk, BT departmanınızın genel IP adresleri, ağ güvenlik grupları, kullanıcı tanımlı yollar veya yönlendirme tabloları oluşturma yasaklar kaynak İlkesi zorlayabilir. Hata iletisinde **Belirtiler** bölümünde gösterilir adlı bir ilke **regionPolicyDefinition**. İlkeniz, farklı bir ad olabilir.
Bu sorunu gidermek için kaynak ilkelerini gözden geçirmek için BT departmanınıza çalışır ve bu ilkeleri ile uyumlu istenen eylemi gerçekleştirmek nasıl belirleyin.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kaynak ilkesine genel bakış](resource-manager-policy.md)
- [Portal üzerinden ilke atamalarını görüntülemek](resource-manager-policy-portal.md#view-policy-assignments)
