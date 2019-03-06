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
ms.openlocfilehash: 3a702668418ddf17aa9091317393264458ff4f8b
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57408947"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>Hızlı Başlangıç: Azure PowerShell kullanarak bir özel kapsayıcı kayıt defteri oluşturma

Azure Container Registry, Docker kapsayıcı görüntülerini oluşturmak, depolamak ve sunmak için kullanılan özel bir yönetilen Docker kapsayıcı kayıt defteridir. Bu hızlı başlangıçta PowerShell kullanarak Azure Container Registry oluşturmayı öğreneceksiniz. Sonra kayıt defterine bir kapsayıcı görüntüsü göndermeye Docker komutlarını kullanın ve son olarak çekmek ve görüntüyü kayıt defterinizden çalıştırın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu hızlı başlangıçta, Azure PowerShell modülünü gerektirir. Yüklü sürümünüzü belirlemek için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-az-ps).

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker, [macOS][docker-mac], [Windows][docker-windows] ve [Linux][docker-linux] sistemleri için paketler sağlar.

Azure Cloud Shell gerekli tüm Docker bileşenlerini (`dockerd` daemon) içermediğinden, bu hızlı başlangıçta Cloud Shell’i kullanamazsınız.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure aboneliğinizde oturum açın [Connect AzAccount] [ Connect-AzAccount] komutunu ve izleyin ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Azure ile kimlik doğrulaması yapıldıktan sonra bir kaynak grubu oluşturun [yeni AzResourceGroup][New-AzResourceGroup]. Kaynak grubu, Azure kaynaklarınızın dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Ardından, yeni kaynak grubunuz ile kapsayıcı kayıt defteri oluşturun [yeni AzContainerRegistry] [ New-AzContainerRegistry] komutu.

Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Aşağıdaki örnek, "myContainerRegistry007" adlı bir kayıt defteri oluşturur. Aşağıdaki komutta *myContainerRegistry007* değerini değiştirin ve kayıt defterini oluşturmak için komutu çalıştırın:

```powershell
$registry = New-AzContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

Bu hızlı başlangıçta oluşturduğunuz bir *temel* maliyet açısından iyileştirilmiş bir seçenek Azure Container Registry hakkında öğrenme geliştiriciler için kayıt defteri. Kullanılabilir hizmet katmanları hakkında daha fazla bilgi için bkz: [kapsayıcı kayıt defteri SKU'ları][container-registry-skus].

## <a name="log-in-to-registry"></a>Kayıt defterinde oturum açma

Kapsayıcı görüntülerini gönderip çekmeden önce kayıt defterinizde oturum açmalısınız. Üretim senaryolarında, bir bireysel kimlik veya hizmet sorumlusu için kapsayıcı kayıt defteri erişimini kullanır, ancak bu hızlı başlangıcı kısa tutmak için kullanarak kayıt defterinizde yönetici kullanıcıyı etkinleştirin [Get-AzContainerRegistryCredential] [ Get-AzContainerRegistryCredential] komutu:

```powershell
$creds = Get-AzContainerRegistryCredential -Registry $registry
```

Ardından, oturum açmak için [docker login][docker-login] komutunu çalıştırın:

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

Bu komut tamamlandığında `Login Succeeded` döndürülür.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde kaynaklarla çalışmak, bu hızlı başlangıçta oluşturduğunuz, kullanın [Remove-AzResourceGroup] [ Remove-AzResourceGroup] kaynak grubunu, kapsayıcı kayıt defteri ve kapsayıcı görüntülerini kaldırmak için komutu depolanan:

```powershell
Remove-AzResourceGroup -Name myResourceGroup
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
[Connect-AzAccount]: /powershell/module/az.accounts/connect-azaccount
[Get-AzContainerRegistryCredential]: /powershell/module/az.containerregistry/get-azcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzContainerRegistry]: /powershell/module/az.containerregistry/New-AzContainerRegistry
[New-AzResourceGroup]: /powershell/module/az.resources/new-azresourcegroup
[Remove-AzResourceGroup]: /powershell/module/az.resources/remove-azresourcegroup
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
