---
title: Hızlı Başlangıç - Azure Container Instances'da bir uygulamayı çalıştırma - PowerShell
description: Bu hızlı başlangıçta, Azure PowerShell ile Azure Container Instances'a dağıtılacak bir Docker kapsayıcı uygulamasını dağıtmak için Azure PowerShell kullanma
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: b8cb84523288f45dfb719d69e4f7d227039598a9
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56806921"
---
# <a name="quickstart-run-a-container-application-in-azure-container-instances-with-azure-powershell"></a>Hızlı Başlangıç: Azure PowerShell ile Azure Container ınstances'da bir kapsayıcı uygulaması çalıştırma

Docker kapsayıcılarınızı kolay ve hızlı bir şekilde Azure'da çalıştırmak için Azure Container Instances hizmetini kullanın. Sanal makine dağıtmanız veya Kubernetes gibi tam kapsamlı bir düzenleme platformu kullanmanız gerekmez. Bu hızlı başlangıçta, Azure portalı kullanarak Azure’da bir Windows kapsayıcı oluşturacak ve bu uygulamayı tam etki alanı adı (FQDN) ile kullanıma sunacaksınız. Tek bir dağıtım komutu yürüttükten birkaç saniye sonra çalışan uygulamaya göz atabilirsiniz:

![Azure Container Instances hizmetine dağıtılmış uygulamanın tarayıcıdaki görüntüsü][qs-powershell-01]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici Azure PowerShell modülünü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Tüm Azure kaynakları gibi Azure kapsayıcı örneklerinin de bir kaynak grubuna dağıtılması gerekir. Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.

İlk olarak, adlı bir kaynak grubu oluşturma *myResourceGroup* içinde *eastus* aşağıdaki konum [yeni AzResourceGroup] [ New-AzResourceGroup] komut:

 ```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Artık bir kaynak grubuna sahip olduğunuza göre Azure'da kapsayıcı çalıştırabilirsiniz. Azure PowerShell ile bir kapsayıcı örneği oluşturmak için bir kaynak grubu adı, kapsayıcı örneği adı ve Docker kapsayıcı görüntüsüne sağlamanız [yeni AzContainerGroup] [ New-AzContainerGroup] cmdlet'i. Bu hızlı başlangıçta, kullandığınız `microsoft/iis:nanoserver` Windows görüntü genel Docker Hub kayıt defterinden. Bu görüntü, Nano Sunucu'da çalıştırmak için Internet Information Services (IIS) paketleri.

Açılacak bir veya daha fazla bağlantı noktası, DNS ad etiketi ya da ikisini birden belirterek kapsayıcılarınızı internete açabilirsiniz. Bu hızlı başlangıçta, böylece IIS genel olarak erişilebilir olduğundan bir DNS ad etiketi içeren bir kapsayıcıya dağıtın.

Bir kapsayıcı örneği başlatmak için aşağıdaki komutu yürütün. `-DnsNameLabel` değeri, örneği oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. "DNS ad etiketi kullanılamıyor" hata iletisiyle karşılaşırsanız farklı bir DNS ad etiketi deneyin.

 ```azurepowershell-interactive
New-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

Birkaç saniye içinde Azure’dan bir yanıt almanız gerekir. Kapsayıcının `ProvisioningState` değeri başlangıçta **Oluşturuluyor** şeklindedir ancak birkaç dakika içinde **Başarılı** olarak değişmesi gerekir. Dağıtım durumunu denetleyin [Get-AzContainerGroup] [ Get-AzContainerGroup] cmdlet:

 ```azurepowershell-interactive
Get-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Kapsayıcının sağlama durumu, tam etki alanı adı (FQDN) ve IP adresi, cmdlet çıkışında görüntülenir:

```console
PS Azure:\> Get-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer


ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            : Always
IpAddress                : 52.226.19.87
DnsNameLabel             : aci-demo-win
Fqdn                     : aci-demo-win.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Windows
Volumes                  :
State                    : Pending
Events                   : {}
```

Kapsayıcının `ProvisioningState` bilgisi **Başarılı** olduğunda tarayıcınızdan `Fqdn` adresine gidin. Aşağıdakine benzer bir web sayfası görüyorsanız kendinizi tebrik edebilirsiniz! Docker kapsayıcısında çalışan bir uygulamayı başarıyla Azure'a dağıttınız.

![Azure Container Instances kullanılarak dağıtılmış IIS tarayıcıda görüntüleniyor][qs-powershell-01]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcıyla işiniz bittiğinde kaldırmak ile [Remove-AzContainerGroup] [ Remove-AzContainerGroup] cmdlet:

 ```azurepowershell-interactive
Remove-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, genel bir Docker Hub deposundaki bir görüntüden bir Azure kapsayıcı örneği oluşturdunuz. Kapsayıcı görüntüsünü oluşturup özel bir Azure kapsayıcı kayıt defterinden dağıtmak istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzResourceGroup]: /powershell/module/az.resources/new-Azresourcegroup
[New-AzContainerGroup]: /powershell/module/az.containerinstance/new-Azcontainergroup
[Get-AzContainerGroup]: /powershell/module/az.containerinstance/get-Azcontainergroup
[Remove-AzContainerGroup]: /powershell/module/az.containerinstance/remove-Azcontainergroup
