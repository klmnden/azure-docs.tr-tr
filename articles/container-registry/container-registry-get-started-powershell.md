---
title: Hızlı Başlangıç - Azure - PowerShell özel Docker kayıt defteri oluşturma
description: PowerShell ile Azure’da hızlıca özel bir Docker kapsayıcısı kayıt defteri oluşturmayı öğrenin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: b8ff8e671d51a148177e66b30225dd7536a48028
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55299752"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>Hızlı Başlangıç: Azure PowerShell kullanarak bir özel kapsayıcı kayıt defteri oluşturma

Azure Container Registry, Docker kapsayıcı görüntülerini oluşturmak, depolamak ve sunmak için kullanılan özel bir yönetilen Docker kapsayıcı kayıt defteridir. Bu hızlı başlangıçta PowerShell kullanarak Azure Container Registry oluşturmayı öğreneceksiniz. Sonra kayıt defterine bir kapsayıcı görüntüsü göndermeye Docker komutlarını kullanın ve son olarak çekmek ve görüntüyü kayıt defterinizden çalıştırın.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç, Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Yüklü sürümünüzü belirlemek için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/azurerm/install-azurerm-ps).

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker, [macOS][docker-mac], [Windows][docker-windows] ve [Linux][docker-linux] sistemleri için paketler sağlar.

Azure Cloud Shell gerekli tüm Docker bileşenlerini (`dockerd` daemon) içermediğinden, bu hızlı başlangıçta Cloud Shell’i kullanamazsınız.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Connect-AzureRmAccount][Connect-AzureRmAccount] komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri uygulayın.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Azure ile kimliğiniz doğrulandıktan sonra [New-AzureRmResourceGroup][New-AzureRmResourceGroup] ile bir kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarınızın dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Ardından, [New-AzureRMContainerRegistry][New-AzureRMContainerRegistry] komutuyla yeni kaynak grubunuzda bir kapsayıcı kayıt defteri oluşturun.

Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Aşağıdaki örnek, "myContainerRegistry007" adlı bir kayıt defteri oluşturur. Aşağıdaki komutta *myContainerRegistry007* değerini değiştirin ve kayıt defterini oluşturmak için komutu çalıştırın:

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

Bu hızlı başlangıçta oluşturduğunuz bir *temel* maliyet açısından iyileştirilmiş bir seçenek Azure Container Registry hakkında öğrenme geliştiriciler için kayıt defteri. Kullanılabilir hizmet katmanları hakkında daha fazla bilgi için bkz: [kapsayıcı kayıt defteri SKU'ları][container-registry-skus].

## <a name="log-in-to-registry"></a>Kayıt defterinde oturum açma

Kapsayıcı görüntülerini gönderip çekmeden önce kayıt defterinizde oturum açmalısınız. Üretim senaryolarında, bir bireysel kimlik veya hizmet sorumlusu için kapsayıcı kayıt defteri erişimini kullanır, ancak bu hızlı başlangıcı kısa tutmak için kullanarak kayıt defterinizde yönetici kullanıcıyı etkinleştirin [Get-AzureRmContainerRegistryCredential] [ Get-AzureRmContainerRegistryCredential] komutu:

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Ardından, oturum açmak için [docker login][docker-login] komutunu çalıştırın:

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

Bu komut tamamlandığında `Login Succeeded` döndürülür.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde kaynaklarla çalışmak, bu hızlı başlangıçta oluşturduğunuz, kullanın [Remove-AzureRmResourceGroup] [ Remove-AzureRmResourceGroup] kaynak grubunu, kapsayıcı kayıt defteri ve kapsayıcı kaldırmak için komutu depolanan görüntüler:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure PowerShell ile bir Azure Container Registry oluşturdunuz, bir kapsayıcı görüntüsü gönderdiniz çekilir ve kayıt defterinden görüntü çalıştı. ACR derin göz atmak için Azure Container Registry öğreticilere geçin.

> [!div class="nextstepaction"]
> [Azure Container Registry öğreticileri][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzureRmAccount]: /powershell/module/azurerm.profile/connect-azurermaccount
[Get-AzureRmContainerRegistryCredential]: /powershell/module/azurerm.containerregistry/get-azurermcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzureRMContainerRegistry]: /powershell/module/azurerm.containerregistry/New-AzureRMContainerRegistry
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md