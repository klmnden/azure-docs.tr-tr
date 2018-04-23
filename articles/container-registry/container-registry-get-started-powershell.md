---
title: 'Hızlı Başlangıç: PowerShell ile Azure’da özel bir Docker kayıt defteri oluşturun'
description: PowerShell ile hızlıca özel bir Docker kapsayıcısı kayıt defteri oluşturmayı öğrenin.
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ae21fc7ab016071d075324bf813243cef58dcd04
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="quickstart-create-an-azure-container-registry-using-powershell"></a>Hızlı başlangıç: PowerShell kullanarak Azure Container Registry oluşturma

Azure Container Registry, özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir yönetilen Docker kapsayıcı kayıt defteridir. Bu kılavuzda PowerShell kullanarak bir Azure Container Registry örneği oluşturma, kayıt defterine bir kapsayıcı görüntüsü gönderme ve son olarak kapsayıcıyı kayıt defterinizden Azure Container Instances’a (ACI) dağıtma hakkında ayrıntılı bilgi verilmektedir.

Bu hızlı başlangıç, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker [Mac][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
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

```powershell
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

Bu komut tamamlandığında `Login Succeeded` döndürülür. `--password-stdin` parametresinin kullanılmasını öneren bir güvenlik uyarısı da görebilirsiniz. Bunun kullanımı bu makalenin kapsamında olmasa da bu en iyi yöntemin izlenmesi önerilir. Daha fazla bilgi edinmek için [docker login][docker-login] komut başvurusuna bakın.

## <a name="push-image-to-acr"></a>Görüntüyü ACR’ye gönderme

Azure Container kayıt defterine görüntü gönderebilmeniz için önce bir görüntünüz olmalıdır. Gerekirse aşağıdaki komutu çalıştırarak Docker Hub’dan önceden oluşturulmuş bir görüntüyü çekin.

```powershell
docker pull microsoft/aci-helloworld
```

Görüntünün, ACR oturum açma sunucusu adıyla etiketlenmiş olması gerekir. Bunu yapmak için [docker tag][docker-tag] komutunu kullanın.

```powershell
$image = $registry.LoginServer + "/aci-helloworld:v1"
docker tag microsoft/aci-helloworld $image
```

Son olarak, [docker push][docker-push] komutunu kullanarak görüntüyü ACR’ye gönderin.

```powershell
docker push $image
```

## <a name="deploy-image-to-aci"></a>Görüntüyü ACI’ya dağıtma
Görüntüyü Azure Container Instances’a (ACI) bir kapsayıcı örneği olarak dağıtmak için öncelikle kayıt defteri kimlik bilgisini bir PSCredential’a dönüştürün.

```powershell
$secpasswd = ConvertTo-SecureString $creds.Password -AsPlainText -Force
$pscred = New-Object System.Management.Automation.PSCredential($creds.Username, $secpasswd)
```

1 CPU çekirdeği ve 1 GB bellek ile kapsayıcı kayıt defterinden kapsayıcı görüntünüzü dağıtmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name mycontainer -Image $image -Cpu 1 -MemoryInGB 1 -IpAddressType public -Port 80 -RegistryCredential $pscred
```

Azure Resource Manager’dan kapsayıcınızın ayrıntılarını içeren ilk yanıtı almanız gerekir. Kapsayıcınızın durumunu izlemek ve ne zaman çalıştığını denetleyip görmek için [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup] komutunu tekrarlayın. Bu işlemin bir dakikadan kısa sürmesi gerekir.

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
```

Örnek çıktı: `Succeeded`

## <a name="view-the-application"></a>Uygulamayı görüntüleme
ACI dağıtımı başarılı olduktan sonra [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup] komutuyla kapsayıcının genel IP adresini alın:

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).IpAddress
```

Örnek çıktı: `"13.72.74.222"`

Çalışan uygulamayı görmek için, sık kullandığınız tarayıcıda genel IP adresine gidin. Şuna benzer şekilde görünecektir:

![Tarayıcıdaki Merhaba Dünya uygulaması][qs-portal-15]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değillerse [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup] komutunu kullanarak kaynak grubunu, Azure Container Registry’yi ve tüm Azure Container Örneklerini kaldırabilirsiniz.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure CLI ile bir Azure Container Registry oluşturdunuz ve Azure Container Instances’da bir örneğini başlattınız. ACI’ya daha ayrıntılı bir bakış için Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup

<!-- IMAGES> -->
[qs-portal-15]: ./media/container-registry-get-started-portal/qs-portal-15.png
