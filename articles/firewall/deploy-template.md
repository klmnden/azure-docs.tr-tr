---
title: Azure bir şablon kullanarak Güvenlik Duvarı'nı dağıtma
description: Azure bir şablon kullanarak Güvenlik Duvarı'nı dağıtma
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: article
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: d32e6e29c287d140c28206743e36dc025b26158b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991343"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Azure bir şablon kullanarak Güvenlik Duvarı'nı dağıtma

Bu şablon, bir güvenlik duvarı ve ağ ortamı testi oluşturur. Üç alt ağ ile bir sanal ağın ağ vardır: *AzureFirewallSubnet*, *ServersSubnet*ve *JumpboxSubnet*. ServersSubnet ve JumpboxSubnet'ten her birinin içinde bir tane 2 çekirdekli Windows Server bulunur.

Güvenlik duvarı AzureFirewallSubnet'in içindedir ve www.microsoft.com'a erişim izni veren tek kurallı bir Uygulama Kuralı Koleksiyonu ile yapılandırılmıştır.

Güvenlik duvarı kurallarının uygulandığı güvenlik duvarı üzerinden ServersSubnet'ten gelen ağ trafiğine işaret eden, kullanıcı tanımlı bir yol oluşturulur.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="template-location"></a>Şablonu konumu

Şablon şu konumdadır:

[https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox)

Tanıtımını okuyun ve hazır olduğunuzda dağıtmak tıklatın **azure'a Dağıt**.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İlk Güvenlik Duvarı'nı ve ardından ile oluşturulan kaynakları keşfedin kullanabileceğiniz artık gerekli değilse [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu, güvenlik duvarı ve tüm ilgili kaynakları kaldırmak için komutu.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="next-steps"></a>Sonraki adımlar

Ardından, Azure güvenlik duvarı günlükleri izleyebilirsiniz:

- [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)

