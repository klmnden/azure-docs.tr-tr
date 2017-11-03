---
title: "Hızlı Başlangıç - PowerShell ile ilk Azure kapsayıcı örnekleri kapsayıcı oluşturma"
description: "Azure kapsayıcı örnekleriyle PowerShell ile bir Windows kapsayıcı örneği oluşturarak başlayın."
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: fbd1bee04c5180beda23c04607b313eec9edcab4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Azure Container Instances’da ilk kapsayıcınızı oluşturma

Azure kapsayıcı örnekleri oluşturmak ve sanal makineler sağlamak veya bir üst düzey hizmet benimsemeyi zorunda kalmadan, azure'da Docker kapsayıcıları yönetmek kolay hale getirir.

Bu Hızlı Başlangıç Windows kapsayıcı oluşturmak ve genel bir IP adresi ile Internet'e kullanıma. Bu işlem tek bir komutla tamamlanır. Yalnızca birkaç dakika içinde bu tarayıcınızda görürsünüz:

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][qs-powershell-01]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu Hızlı Başlangıç, Azure PowerShell modülü 4,4 veya sonraki bir sürümü gerektiriyor. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Bir Azure kaynak grubu ile oluşturma [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir ad, bir Docker görüntüsü ve bir Azure kaynak grubu sağlayarak bir kapsayıcı oluşturabilirsiniz [yeni AzureRmContainerGroup] [ New-AzureRmContainerGroup] cmdlet'i. İsteğe bağlı olarak, kapsayıcıyı genel IP adresi ile İnternet üzerinden kullanıma sunabilirsiniz. Bu durumda, Internet Information Services (IIS) çalıştıran bir Windows Nano Server kapsayıcı kullanacağız.

```powershell
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -IpAddressType Public
```

Birkaç saniye içinde isteğinize yanıt elde edersiniz. Kapsayıcı başlangıçta olacak bir **oluşturma** durumu, ancak bir veya iki dakika içinde başlamalıdır. Durum kullanarak denetleyebilirsiniz [Get-AzureRmContainerGroup] [ Get-AzureRmContainerGroup] cmdlet:

```powershell
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Kapsayıcının sağlama durumunu ve IP adresi cmdlet çıktısında görüntülenir:

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

Kapsayıcı **ProvisioningState** taşır `Succeeded`, sağlanan IP adresi kullanarak tarayıcınızda ulaşabilirsiniz.

![Azure kapsayıcı örnekleri kullanılarak dağıtılan IIS tarayıcıda görüntülenen][qs-powershell-01]

## <a name="delete-the-container"></a>Kapsayıcıyı silme

Kapsayıcıyla bittiğinde kullanarak kaldırabilirsiniz [Kaldır AzureRmContainerGroup] [ Remove-AzureRmContainerGroup] cmdlet:

```powershell
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç Azure kapsayıcı örneği önceden oluşturulmuş bir Windows kapsayıcısında başlatıldı. Kendiniz bir kapsayıcı oluşturma ve Azure kapsayıcı kayıt defterini kullanarak Azure kapsayıcı örnekleri için dağıtımı Azure kapsayıcı örnekleri Öğreticisine devam istiyorsanız.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticileri](./container-instances-tutorial-prepare-app.md)

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png