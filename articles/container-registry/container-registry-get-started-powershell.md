---
title: "Hızlı Başlangıç: PowerShell ile Azure’da özel bir Docker kayıt defteri oluşturun"
description: "PowerShell ile hızlıca özel bir Docker kapsayıcısı kayıt defteri oluşturmayı öğrenin."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 80b5055dee35cd6efe62ee949c05aef386a3ba14
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="create-an-azure-container-registry-using-powershell"></a>PowerShell kullanarak Azure Container Registry oluşturma

Azure Container Registry, özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir yönetilen Docker kapsayıcı kayıt defteridir. Bu kılavuzda, PowerShell kullanarak bir Azure Container Registry örneği oluşturma hakkındaki ayrıntılar yer alır.

Bu hızlı başlangıç, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker [Mac][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

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

[New-AzureRMContainerRegistry](/powershell/module/containerregistry/New-AzureRMContainerRegistry) komutunu kullanarak bir ACR örneği oluşturun.

Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Aşağıdaki örnekte *myContainerRegistry007* komutu kullanılmıştır. Bunu benzersiz bir değerle güncelleştirin.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>ACR üzerinde oturum açın

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. Önce, ACR örneğinin yönetici kimlik bilgilerini öğrenmek için [Get-AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential) komutunu kullanın.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Sonra, ACR örneğinde oturum açmak için [docker login][docker-login] komutunu kullanın.

```bash
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

Bu komut tamamlandığında `Login Succeeded` döndürülür. `--password-stdin` parametresinin kullanılmasını öneren bir güvenlik uyarısı da görebilirsiniz. Bunun kullanımı bu makalenin kapsamında olmasa da bu en iyi yöntemin izlenmesi önerilir. Daha fazla bilgi edinmek için [docker login][docker-login] komut başvurusuna bakın.

## <a name="push-image-to-acr"></a>Görüntüyü ACR’ye gönderme

Azure Container kayıt defterine görüntü gönderebilmeniz için önce bir görüntünüz olmalıdır. Gerekirse aşağıdaki komutu çalıştırarak Docker Hub’dan önceden oluşturulmuş bir görüntüyü çekin.

```bash
docker pull microsoft/aci-helloworld
```

Görüntünün, ACR oturum açma sunucusu adıyla etiketlenmiş olması gerekir. ACR örneğinin oturum açma sunucusu adını döndürmek için, [Get-AzureRmContainerRegistry](/powershell/module/containerregistry/Get-AzureRmContainerRegistry) komutunu çalıştırın.

```powershell
Get-AzureRmContainerRegistry | Select Loginserver
```

Görüntüyü [docker tag][docker-tag] komutunu kullanarak etiketleyin. *acrLoginServer* değerini ACR örneğinizin sunucu adıyla değiştirin.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Son olarak, [docker push][docker-push] komutunu kullanarak görüntüleri ACR örneğine gönderin. *acrLoginServer* değerini ACR örneğinizin sunucu adıyla değiştirin.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değillerse [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu, ACR örneğini ve tüm kapsayıcı görüntülerini kaldırabilirsiniz.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure CLI ile bir Azure Container Registry oluşturdunuz. Azure Container Registry’yi Azure Container Instances ile kullanmak istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/