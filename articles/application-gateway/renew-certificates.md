---
title: Bir Azure uygulama ağ geçidi Sertifikayı Yenile
description: Bir uygulama ağ geçidi dinleyicisi ile ilişkili bir sertifikayı yenilemek öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 05/17/2018
ms.author: victorh
ms.openlocfilehash: b125f707e8de17764701e981736a53492e5e756c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="renew-application-gateway-certificates"></a>Uygulama ağ geçidi sertifikaları yenile

Belirli bir noktada, uygulama ağ geçidi SSL şifrelemesi için yapılandırılmışsa, sertifikaları yenilemek gerekir.

Azure portalında veya Azure PowerShell kullanarak bir dinleyici ile ilişkili bir sertifikayı yenilemek:

## <a name="azure-portal"></a>Azure portalına

Portaldan bir dinleyici sertifikayı yenilemek için uygulama ağ geçidi dinleyicileri gidin. Yenilenmesi ve ardından gereken bir sertifikası var olan dinleyiciyi tıklatın **yenileme veya düzenleme Seçili sertifika**.

![Sertifikayı Yenile](media/renew-certificate/ssl-cert.png)

Yeni PFX sertifikanızı karşıya yükleme, bir ad verin, parolayı yazın ve ardından **kaydetmek**.

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell kullanarak sertifikanızı yenilemek için aşağıdaki cmdlet'i kullanın:

```PowerShell
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

## <a name="next-steps"></a>Sonraki adımlar

SSL boşaltma Azure uygulama ağ geçidi ile yapılandırma hakkında bilgi edinmek için [SSL boşaltma yapılandırın](application-gateway-ssl-portal.md)
