---
title: "Hızlı Başlangıç - PowerShell ile ilk Azure Container Instances kapsayıcınızı oluşturma"
description: "PowerShell ile bir Windows kapsayıcı örneği oluşturarak Azure Container Instances kullanmaya başlayın."
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: f24619c9ff2667bb96827b1f17bf9ff04b42eaff
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Azure Container Instances’da ilk kapsayıcınızı oluşturma

Azure Container Instances, sanal makine sağlamak veya daha yüksek düzey bir hizmet benimsemek zorunda kalmadan Azure’da Docker kapsayıcıları oluşturmayı ve yönetmeyi kolaylaştırır.

Bu hızlı başlangıç içeriğinde, bir Windows kapsayıcısı oluşturacak ve bu kapsayıcıyı genel IP adresi ile İnternet üzerinden kullanıma sunacaksınız. Bu işlem tek bir komutla tamamlanır. Birkaç dakika içinde, çalışan uygulamayı tarayıcınızda görüntüleyebilirsiniz:

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][qs-powershell-01]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup][New-AzureRmResourceGroup] ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

[New-AzureRmContainerGroup][New-AzureRmContainerGroup] cmdlet’ine bir ad, Docker görüntüsü ve bir Azure kaynak grubu sağlayarak kapsayıcı oluşturabilirsiniz. İsteğe bağlı olarak, kapsayıcıyı genel IP adresi ile İnternet üzerinden kullanıma sunabilirsiniz. Bu durumda, Internet Information Services (IIS) çalıştıran bir Windows Nano Sunucu kullanacağız.

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -IpAddressType Public
```

Birkaç saniye içinde isteğinize yanıt alırsınız. Kapsayıcı başlangıçta **Oluşturuluyor** durumunda olacaktır ancak bir veya iki dakika içinde başlar. [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup] cmdlet’ini kullanarak durumu denetleyebilirsiniz:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Kapsayıcının sağlama durumu ve IP adresi cmdlet çıkışında görüntülenir:

```
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/12345678-1234-1234-1234-12345678abcd/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            :
IpAddress                : 40.71.248.73
Ports                    : {80}
OsType                   : Windows
Volumes                  :
```

Kapsayıcı **ProvisioningState** `Succeeded` durumuna geçtiğinde, sağlanan IP adresini kullanarak tarayıcınızdan kapsayıcıya ulaşabilirsiniz.

![Azure Container Instances kullanılarak dağıtılmış IIS tarayıcıda görüntüleniyor][qs-powershell-01]

## <a name="delete-the-container"></a>Kapsayıcıyı silme

Kapsayıcıyla işiniz bittiğinde [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup] cmdlet’ini kullanarak kapsayıcıyı kaldırabilirsiniz:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Container Instances’da önceden oluşturulmuş bir Windows kapsayıcısı başlattınız. Kapsayıcıyı kendiniz oluşturup Azure Container Registry’yi kullanarak Azure Container Instances’a dağıtmayı denemek istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](./container-instances-tutorial-prepare-app.md)

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png