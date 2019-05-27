---
title: RequestDisallowedByPolicy hatası ile bir Azure kaynak ilkesinden | Microsoft Docs
description: RequestDisallowedByPolicy hatası nedenini açıklar.
services: azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: c160fe39b02d8adf6c12e3736307cf7f9688b0c5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66128446"
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Azure kaynak İlkesi ile RequestDisallowedByPolicy hatası

RequestDisallowedByPolicy hatası nedenini bu makalede, ayrıca bu hata için çözüm sağlar.

## <a name="symptom"></a>Belirti

Dağıtım sırasında alabileceğiniz bir **RequestDisallowedByPolicy** kaynaklar oluşturmanızı engeller hata. Aşağıdaki örnekte, hatayı gösterir:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Sorun giderme

Dağıtımınızı engellenen ilke ayrıntılarını almak için yöntemlerden birini kullanın:

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell'de, bu ilke tanımlayıcısı olarak sağlamak `Id` dağıtımınızı engellenen ilke ayrıntılarını almak için parametre.

```powershell
(Get-AzPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>Azure CLI'si

Azure CLI, ilke tanımı adını sağlayın:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Çözüm

Güvenlik veya uyumluluk için abonelik yöneticileri kaynakları nasıl dağıtılacağını sınırlama ilkeleri atayabilirsiniz. Örneğin, aboneliğiniz, genel IP adresleri, ağ güvenlik grupları, kullanıcı tanımlı yollar oluşturma engelleyen bir ilke veya rota tabloları. Hata iletisinde **belirtileri** bölüm ilkesinin adını gösterir.
Bu sorunu çözmek için kaynak ilkelerini gözden geçirin ve bu ilkeleri ile uyumlu kaynakları dağıtma konusunda bilgi.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure İlkesi nedir?](../governance/policy/overview.md)
- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)
