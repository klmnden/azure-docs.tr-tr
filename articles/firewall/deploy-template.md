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
ms.openlocfilehash: 1a732e22d72c36afe11030e42bae529baa35df1a
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991549"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Azure bir şablon kullanarak Güvenlik Duvarı'nı dağıtma

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

Azure güvenlik duvarı makalelerdeki örneklerde, Azure güvenlik duvarı genel önizlemeye sunuldu zaten etkinleştirdiyseniz varsayılmaktadır. Daha fazla bilgi için [Azure güvenlik duvarı genel önizlemesini etkinleştirme](public-preview.md).

Bu şablon, bir güvenlik duvarı ve ağ ortamı testi oluşturur. Üç alt ağ ile bir sanal ağın ağ vardır: *AzureFirewallSubnet*, *ServersSubnet*ve *JumpboxSubnet*. ServersSubnet ve JumpboxSubnet bir 2 çekirdekli Windows Server olması.

Güvenlik Duvarı AzureFirewallSubnet içinde olan ve bir uygulama kuralı koleksiyonu www.microsoft.com erişim sağlayan tek bir kural ile yapılandırılır.

Kullanıcı tanımlı bir yolun güvenlik duvarı kuralları nereye uygulanacağını ağ trafiğinin güvenlik duvarı üzerinden ServersSubnet işaret eden oluşturulur.

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

- [Öğretici: Azure Güvenlik Duvarı İzleme günlükleri](./tutorial-diagnostics.md)

