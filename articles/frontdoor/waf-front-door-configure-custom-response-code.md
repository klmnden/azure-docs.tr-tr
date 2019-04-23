---
title: Web uygulaması güvenlik duvarı için özel bir yanıt Azure ön Kapıda yapılandırın
description: Web uygulaması Güvenlik Duvarı (WAF), bir istek engellediğinde bir özel yanıt kodu ve şu iletiyle yapılandırmayı öğrenin.
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: tyao;kumud
ms.openlocfilehash: 2d16893420f27caf4f8b00dc32069e3296d7c236
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795755"
---
# <a name="configure-a-custom-response-for-azure-web-application-firewall"></a>Azure web uygulaması güvenlik duvarı için özel bir yanıt yapılandırın

Varsayılan olarak, Azure web uygulaması Güvenlik Duvarı (WAF) ile Azure ön kapısı eşleşen kural nedeniyle, bir istek engellediğinde 403 durum koduyla döndürür **istek engellendi** ileti. Bu makalede, bir istek WAF tarafından engellendiğinde özel yanıt durum kodu ve yanıt iletisi yapılandırma açıklanır.

## <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizle oturum açmak için bu sayfadaki yönergeleri izleyin ve Az PowerShell modülünü yükleyin.

### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Oturum açma için etkileşimli bir iletişim kutusu ile Azure'a bağlanma
```
Connect-AzAccount
Install-Module -Name Az
```
PowerShellGet yüklü geçerli sürümü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell'i yeniden açın.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 
### <a name="install-azfrontdoor-module"></a>Az.FrontDoor modülünü yükleme 

```
Install-Module -Name Az.FrontDoor
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure'da, bir kaynak grubu için ilgili kaynakları ayırın. Bu örnekte, bir kaynak grubu kullanarak oluşturduğunuz [yeni AzResourceGroup](/powershell/module/Az.resources/new-Azresourcegroup).

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupWAF
```

## <a name="create-a-new-waf-policy-with-custom-response"></a>Özel yanıtı ile yeni bir WAF ilkesi oluşturma 

405 ve ileti için ayarlanan özel yanıt durum kodu ile yeni bir WAF ilkesi oluşturma örneği aşağıda verilmiştir **bloke edilir.** kullanarak [yeni AzFrontDoorFireWallPolicy](/powershell/module/az.frontdoor/new-azfrontdoorfirewallPolicy).

```azurepowershell
# WAF policy setting
New-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 405 `
-CustomBlockResponseBody "<html><head><title>You are blocked.</title></head><body></body></html>"
```

Özel yanıt kodu veya yanıt gövdesi ayarlarını var olan bir WAF ilkenin kullanarak [kümesi AzFrontDoor](/powershell/module/az.frontdoor/set-azfrontdoor).

```azurepowershell
# modify WAF response code
Set-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 403
```

```azurepowershell
# modify WAF response body
Set-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-CustomBlockResponseBody "<html><head><title> Forbidden</title></head><body></body></html>"
```

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [ön kapısı](front-door-overview.md)