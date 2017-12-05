---
title: "Hızlı Başlangıç - Azure PowerShell ile özel bir Docker kayıt defteri oluşturun"
description: "PowerShell ile özel bir Docker kapsayıcısı kayıt defteri oluşturmak hızlı bir şekilde öğrenin."
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: tysonn
ms.service: container-registry
ms.devlang: na
ms.topic: quicksart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: d165cc159f7da2c8e7f0be4f0fa7d353997ed5f9
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="create-an-azure-container-registry-using-powershell"></a>PowerShell kullanarak bir Azure kapsayıcı kayıt defteri oluşturma

Azure Container Registry, özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir yönetilen Docker kapsayıcı kayıt defteridir. PowerShell kullanarak Azure kapsayıcı kayıt defteri örneği oluşturma bu kılavuzu ayrıntıları.

Bu Hızlı Başlangıç, Azure PowerShell modülü 3,6 veya sonraki bir sürümü gerektiriyor. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

Ayrıca, Docker yerel olarak yüklü olması gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bir ACR örneği kullanarak oluşturduğunuz [yeni AzureRMContainerRegistry](/powershell/module/containerregistry/New-AzureRMContainerRegistry) komutu.

Kayıt defteri adını **benzersiz olmalıdır**. Aşağıdaki örnekte *myContainerRegistry007* kullanılır. Benzersiz bir değere güncelleştirin.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>ACR için oturum açın

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. İlk olarak, kullanın [Get-AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential) ACR örneği için yönetici kimlik bilgilerini almak için komutu.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Ardından, kullanın [docker oturum açma](https://docs.docker.com/engine/reference/commandline/login/) ACR örneğinde oturum açma için komutu.

```bash
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

Komut tamamlandığında bir “Oturum Açma Başarılı” iletisi döndürür.

## <a name="push-image-to-acr"></a>ACR itme görüntüye

Azure kapsayıcı kayıt defterine görüntü göndermek için ilk görüntüsü olması gerekir. Gerekirse, önceden oluşturulmuş bir görüntü Docker hub'dan çıkarmak için aşağıdaki komutu çalıştırın.

```bash
docker pull microsoft/aci-helloworld
```

Görüntü ACR oturum açma sunucu adıyla etiketlenmelidir. Çalıştırma [Get-AzureRmContainerRegistry](/powershell/module/containerregistry/Get-AzureRmContainerRegistry) ACR örneğinin oturum açma sunucusu adını döndürmek için komutu.

```powershell
Get-AzureRmContainerRegistry | Select Loginserver
```

Kullanarak resim etiketi [docker etiketi](https://docs.docker.com/engine/reference/commandline/tag/) komutu. Değiştir *acrLoginServer* ACR örneğinizi oturum açma sunucusu adı.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Son olarak, [docker itme](https://docs.docker.com/engine/reference/commandline/push/) görüntüleri ACR örneğine göndermek için. Değiştir *acrLoginServer* ACR örneğinizi oturum açma sunucusu adı.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu, ACR örneği ve tüm kapsayıcı görüntüleri kaldırmak için komutu.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç Azure CLI ile Azure kapsayıcı kayıt defteri oluşturuldu. Azure kapsayıcı kayıt defteri Azure kapsayıcı örnekleri ile kullanmak istiyorsanız, Azure kapsayıcı örnekleri Öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure kapsayıcı örnekleri Öğreticisi](../container-instances/container-instances-tutorial-prepare-app.md)