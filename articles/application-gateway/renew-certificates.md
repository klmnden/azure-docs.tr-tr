---
title: Azure Application Gateway sertifikasını yenileme
description: Bir uygulama ağ geçidi dinleyicisi ile ilişkili bir sertifikayı yenilemek öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 8/15/2018
ms.author: victorh
ms.openlocfilehash: 48bd548ec977d2dc4dd3b5b2f34df04562a6e918
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42056977"
---
# <a name="renew-application-gateway-certificates"></a>Application Gateway sertifikaları yenile

Belirli bir noktada, application gateway SSL şifrelemesi için yapılandırılmışsa, sertifikalarını yenilemesi gerekir.

Ya da Azure portalı, Azure PowerShell veya Azure CLI kullanarak bir dinleyici ile ilişkili bir sertifika yenileme yapabilir:

## <a name="azure-portal"></a>Azure portal

Portaldan bir dinleyici sertifikayı yenilemek için uygulama ağ geçidi dinleyicilere gidin. Yenilenmesi ve ardından gereken bir sertifikaya sahip bir dinleyici tıklayın **Yenile veya Düzenle seçilen sertifika**.

![Sertifikayı yenile](media/renew-certificate/ssl-cert.png)

Yeni bir PFX sertifikasını karşıya yükleyin, bir ad verin, parolayı yazın ve ardından **Kaydet**.

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell kullanarak sertifikanızı yenilemek için aşağıdaki betiği kullanın:

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

Set-AzureRmApplicationGateway -ApplicationGateway $appgw
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

SSL boşaltma Azure Application Gateway ile yapılandırma konusunda bilgi için bkz: [SSL yük boşaltma yapılandırın](application-gateway-ssl-portal.md)
