---
title: Azure bir şablon kullanarak Güvenlik Duvarı'nı dağıtma
description: Azure bir şablon kullanarak Güvenlik Duvarı'nı dağıtma
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: article
ms.date: 12/01/2018
ms.author: victorh
ms.openlocfilehash: 86fdbbacf3e8064afe0aaaaebea1d6ef6c25f9d4
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52865844"
---
# <a name="deploy-azure-firewall-using-a-template"></a>Azure bir şablon kullanarak Güvenlik Duvarı'nı dağıtma

[Oluşturma AzureFirewall korumalı alan kurulum şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox) ağ ortamı testi ile bir güvenlik duvarı oluşturur. Bir sanal ağ (VNet) ile üç alt ağıyla: *AzureFirewallSubnet*, *ServersSubnet*, ve *JumpboxSubnet*. *ServersSubnet* ve *JumpboxSubnet* her alt ağa sahip bir tek, iki çekirdekli Windows Server sanal makinesi.

Güvenlik Duvarı yer *AzureFirewallSubnet* alt ağ ve bir uygulama kuralı koleksiyonu erişimine izin veren tek bir kural *www.microsoft.com*.

Kullanıcı tanımlı bir yol, gelen ağ trafiğini işaret *ServersSubnet* alt ağ ve güvenlik duvarında burada güvenlik duvarı kuralları uygulanır.

Azure Güvenlik Duvarı hakkında daha fazla bilgi için bkz: [dağıtma ve Azure Azure portalını kullanarak güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md).

## <a name="use-the-template-to-deploy-azure-firewall"></a>Azure güvenlik duvarı dağıtmak için şablonu kullanın

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

**Yükleme ve şablon kullanarak Azure güvenlik duvarı dağıtma hakkında bilgi için:**

1. Şablon erişim [ https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azurefirewall-sandbox).
   
1. Tanıtımını okuyun ve hazır olduğunuzda dağıtılacağı seçin **azure'a Dağıt**.
   
1. Gerekirse, Azure portalında oturum açın. 

1. Portalında, üzerinde **AzureFirewall bir korumalı alan ayarı oluşturma** sayfasında yazın veya aşağıdaki değerleri seçin:
   
   - **Kaynak grubu**: seçin **Yeni Oluştur**, kaynak grubu için bir ad yazın ve seçin **Tamam**. 
   - **Sanal ağ adı**: yeni bir VNet için bir ad yazın. 
   - **Yönetici kullanıcı adı**: yönetici kullanıcı hesabı için bir kullanıcı adı yazın.
   - **Yönetici parolası**: bir yönetici parolasını yazın. 
   
1. Hüküm ve koşulları okuyun ve ardından **hüküm ve koşulları yukarıda belirtilen kabul ediyorum**.
   
1. **Satın al**'ı seçin.
   
   Kaynakları oluşturmak için birkaç dakika sürer. 
   
1. Güvenlik Duvarı ile oluşturulan kaynakları keşfedin. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olduğunda, kaynak grubu, güvenlik duvarı ve tüm ilgili kaynakları çalıştırarak kaldırabilirsiniz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) PowerShell komutu. Adlı bir kaynak grubunu kaldırmak için *MyResourceGroup*çalıştırın: 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name MyResourceGroup
```
Öğretici izleme güvenlik duvarı oturum devam etmeyi planlıyorsanız kaynak grubu ve güvenlik duvarı henüz kaldırmayın. 

## <a name="next-steps"></a>Sonraki adımlar

Ardından, Azure güvenlik duvarı günlükleri izleyebilirsiniz:

> [!div class="nextstepaction"]
> [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
