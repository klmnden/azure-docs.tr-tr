---
title: Bir Azure uygulama ağ geçidi Sertifikayı Yenile
description: Bir uygulama ağ geçidi dinleyicisi ile ilişkili bir sertifikayı yenilemek öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 05/18/2018
ms.author: victorh
ms.openlocfilehash: b44a57fe8ebcc985d3ab66ea04936a1558d00863
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598275"
---
# <a name="renew-application-gateway-certificates"></a>Uygulama ağ geçidi sertifikaları yenile

Belirli bir noktada, uygulama ağ geçidi SSL şifrelemesi için yapılandırılmışsa, sertifikaları yenilemek gerekir.

Ya da Azure portal, Azure PowerShell veya Azure CLI kullanarak bir dinleyici ile ilişkili bir sertifikayı yenilemek:

## <a name="azure-portal"></a>Azure portalına

Portaldan bir dinleyici sertifikayı yenilemek için uygulama ağ geçidi dinleyicileri gidin. Yenilenmesi ve ardından gereken bir sertifikası var olan dinleyiciyi tıklatın **yenileme veya düzenleme Seçili sertifika**.

![Sertifikayı Yenile](media/renew-certificate/ssl-cert.png)

Yeni PFX sertifikanızı karşıya yükleme, bir ad verin, parolayı yazın ve ardından **kaydetmek**.

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell kullanarak sertifikanızı yenilemek için aşağıdaki cmdlet'i kullanın:

```azurepowershell-interactive
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-azureRmApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password
```
## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Sonraki adımlar

SSL boşaltma Azure uygulama ağ geçidi ile yapılandırma hakkında bilgi edinmek için [SSL boşaltma yapılandırın](application-gateway-ssl-portal.md)
