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
ms.date: 03/09/2018
ms.author: genli
ms.openlocfilehash: 5a9efa6b807e933726104e7af315589ede5d9b74
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Azure kaynak ilkesiyle RequestDisallowedByPolicy hata

RequestDisallowedByPolicy hatanın nedeni, bu makalede, ayrıca bu hata için çözüm sağlar.

## <a name="symptom"></a>Belirti

Dağıtım sırasında alabileceğiniz bir **RequestDisallowedByPolicy** kaynaklar oluşturmanızı engeller hata. Aşağıdaki örnek, hatayı gösterir:

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

### <a name="powershell"></a>PowerShell

Bu ilke tanımlayıcısı olarak PowerShell'de sağlamak `Id` dağıtımınızı engellenen İlkesi hakkındaki ayrıntıları almak için parametre.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>Azure CLI

Azure CLI 2. 0'ilke tanımı adını sağlayın:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Çözüm

Güvenlik veya uyumluluk için abonelik yöneticilerinizi kaynakları nasıl dağıtıldığını sınırlama ilkeleri atayabilirsiniz. Örneğin, aboneliğiniz genel IP adresleri, ağ güvenlik grupları, kullanıcı tanımlı yollar oluşturma önleyen bir ilke, veya olabilir tabloları rota. Hata iletisinde **Belirtiler** bölüm ilkesinin adını gösterir.
Bu sorunu gidermek için kaynak ilkelerini gözden geçirin ve bu ilkelerle kaynakları dağıtma belirleyin.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure ilke nedir?](../azure-policy/azure-policy-introduction.md)
- [Uyumluluğu zorlamak üzere ilkeleri oluşturun ve yönetin](../azure-policy/create-manage-policy.md)
