---
title: 'Hızlı Başlangıç: PowerShell ile Azure’da özel bir Docker kayıt defteri oluşturun'
description: PowerShell ile Azure’da hızlıca özel bir Docker kapsayıcısı kayıt defteri oluşturmayı öğrenin.
services: container-registry
author: marsma
manager: jeconnoc
ms.service: container-registry
ms.topic: quickstart
ms.date: 05/08/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: e6c3330519692eb829803af2582b711be2fb3efe
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43092887"
---
# <a name="quickstart-create-an-azure-container-registry-using-powershell"></a>Hızlı başlangıç: PowerShell kullanarak Azure Container Registry oluşturma

Azure Container Registry, Docker kapsayıcı görüntülerini oluşturmak, depolamak ve sunmak için kullanılan özel bir yönetilen Docker kapsayıcı kayıt defteridir. Bu hızlı başlangıçta PowerShell kullanarak Azure Container Registry oluşturmayı öğreneceksiniz. Kayıt defterini oluşturduktan sonra, ona bir kapsayıcı görüntüsü göndereceksiniz ve ardından kapsayıcıyı kayıt defterinizden Azure Container Instances’a (ACI) dağıtacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç, Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Yüklü sürümünüzü belirlemek için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

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

## <a name="log-in-to-registry"></a>Kayıt defterinde oturum açma

Kapsayıcı görüntülerini gönderip çekmeden önce kayıt defterinizde oturum açmalısınız. Önce, kayıt defterinin yönetici kimlik bilgilerini almak için [Get-AzureRmContainerRegistryCredential][Get-AzureRmContainerRegistryCredential] komutunu kullanın:

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Ardından, oturum açmak için [docker login][docker-login] komutunu çalıştırın:

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

Başarılı bir oturum açma işlemi `Login Succeeded` döndürür:

```console
PS Azure:\> $creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
Login Succeeded
```

## <a name="push-image-to-registry"></a>Kayıt defterine görüntü gönderme

Kayıt defterinde oturum açtığınıza göre, artık ona kapsayıcı görüntüleri gönderebilirsiniz. Kayıt defterinize gönderebileceğiniz bir görüntü almak için, Docker Hub’dan [aci-helloworld][aci-helloworld-image] ortak görüntüsünü çekin. [aci-helloworld][aci-helloworld-github] görüntüsü, Azure Container Instances logosunu gösteren statik bir HTML sayfası sunan küçük bir Node.js uygulamasıdır.

```powershell
docker pull microsoft/aci-helloworld
```

Görüntü çekildikten sonra çıkış aşağıdakine benzer:

```console
PS Azure:\> docker pull microsoft/aci-helloworld
Using default tag: latest
latest: Pulling from microsoft/aci-helloworld
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
3be0618266da: Pull complete
9e232827e52f: Pull complete
b53c152f538f: Pull complete
Digest: sha256:a3b2eb140e6881ca2c4df4d9c97bedda7468a5c17240d7c5d30a32850a2bc573
Status: Downloaded newer image for microsoft/aci-helloworld:latest
```

Azure kapsayıcı kayıt defterinize görüntü gönderebilmeniz için, kayıt defterinizin tam etki alanı adını (FQDN) görüntüye etiketlemeniz gerekir. Azure kapsayıcı kayıt defterlerinin FQDN’si *\<registry-name\>.azurecr.io* biçimindedir.

Bir değişkeni tam görüntü etiketiyle doldurun. Oturum açma sunucusunu, depo adını ("aci-helloworld") ve görüntü sürümünü ("v1") ekleyin:

```powershell
$image = $registry.LoginServer + "/aci-helloworld:v1"
```

Şimdi görüntüyü [docker tag][docker-tag] ile etiketleyin:

```powershell
docker tag microsoft/aci-helloworld $image
```

Ve son olarak, görüntüyü kayıt defterinize [docker push][docker-push] ile gönderin:

```powershell
docker push $image
```

Docker istemcisi görüntünüzü gönderdiğinde, çıkış aşağıdakine benzer:

```console
PS Azure:\> docker push $image
The push refers to repository [myContainerRegistry007.azurecr.io/aci-helloworld]
31ba1ebd9cf5: Pushed
cd07853fe8be: Pushed
73f25249687f: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:565dba8ce20ca1a311c2d9485089d7ddc935dd50140510050345a1b0ea4ffa6e size: 1576
```

Tebrikler! Az önce kayıt defterinize ilk kapsayıcı görüntünüzü gönderdiniz.

## <a name="deploy-image-to-aci"></a>Görüntüyü ACI’ya dağıtma

Görüntü artık kayıt defterinizde olduğuna göre, Azure’da çalıştığını görmek için bir kapsayıcıyı doğrudan Azure Container Instances’a dağıtın.

İlk olarak, kayıt defteri kimlik bilgilerini *PSCredential*’a dönüştürün. Kapsayıcı örneğini oluşturmak için kullandığınız `New-AzureRmContainerGroup` komutu bu biçimde olmasını gerektirir.

```powershell
$secpasswd = ConvertTo-SecureString $creds.Password -AsPlainText -Force
$pscred = New-Object System.Management.Automation.PSCredential($creds.Username, $secpasswd)
```

Ayrıca, kapsayıcınızın DNS ad etiketi oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır. Bir değişkeni oluşturulan bir adla doldurmak için aşağıdaki komutu yürütün:

```powershell
$dnsname = "aci-demo-" + (Get-Random -Maximum 9999)
```

Son olarak, bir kapsayıcıyı kayıt defterinizdeki görüntüden 1 CPU çekirdek ve 1 GB bellek ile dağıtmak için [New-AzureRmContainerGroup][New-AzureRmContainerGroup] komutunu çalıştırın:

```powershell
New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name "mycontainer" -Image $image -RegistryCredential $pscred -Cpu 1 -MemoryInGB 1 -DnsNameLabel $dnsname
```

Azure ’dan kapsayıcınızın ayrıntılarını içeren ilk yanıtı almanız gerekir ve durumu ilk olarak “Beklemede” olur:

```console
PS Azure:\> New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name "mycontainer" -Image $image -RegistryCredential $pscred -Cpu 1 -MemoryInGB 1 -DnsNameLabel $dnsname
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials : {myContainerRegistry007}
RestartPolicy            : Always
IpAddress                : 40.117.255.198
DnsNameLabel             : aci-demo-8751
Fqdn                     : aci-demo-8751.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Linux
Volumes                  :
State                    : Pending
Events                   : {}
```

Durumunu izlemek ve ne zaman çalıştığını belirlemek için [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup] komutunu birkaç kez çalıştırın. Kapsayıcının başlaması en fazla bir dakika sürmelidir.

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
```

Burada, kapsayıcının ProvisioningState durumunun ilk olarak *Oluşturuluyor* olduğunu ve ardından sorunsuz bir şekilde çalışmaya başladığında *Başarılı* durumuna geçtiğini görebilirsiniz:

```console
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
Creating
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
Succeeded
```

## <a name="view-running-application"></a>Çalışan uygulamayı görüntüleme

ACI’ye dağıtım başarılı olduğunda ve kapsayıcınız sorunsuz bir şekilde çalıştığında, uygulamanın Azure’da çalıştığını görmek için tarayıcınızda tam etki alanı adına (FQDN) gidin.

[Get-AzureRmContainerGroup][Get-AzureRmContainerGroup] ile kapsayıcınızın FQDN’sini alın:


```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).Fqdn
```

Komutun çıkışı, kapsayıcı örneğinizin FQDN'sidir:

```console
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).Fqdn
aci-demo-8571.eastus.azurecontainer.io
```

Hazır olduğuna göre tarayıcınızda FQDN’ye gidin:

![Tarayıcıdaki Merhaba Dünya uygulaması][qs-psh-01-running-app]

Tebrikler! Azure’da uygulama çalıştıran ve doğrudan özel Azure kapsayıcı kayıt defterinizdeki bir kapsayıcı görüntüsünden dağıtılmış olan bir kapsayıcınız var.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta oluşturduğunuz kaynaklarla işiniz bittiğinde, kaynak grubunu, kapsayıcı kayıt defterini ve kapsayıcı örneğini kaldırmak için [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup] komutunu kullanın:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure CLI ile bir Azure Container Registry oluşturdunuz ve Azure Container Instances’da bir örneğini başlattınız. ACI’ya daha ayrıntılı bir bakış için Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- LINKS - external -->
[aci-helloworld-github]: https://github.com/Azure-Samples/aci-helloworld
[aci-helloworld-image]: https://hub.docker.com/r/microsoft/aci-helloworld/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzureRmAccount]: /powershell/module/azurerm.profile/connect-azurermaccount
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Get-AzureRmContainerRegistryCredential]: /powershell/module/azurerm.containerregistry/get-azurermcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[New-AzureRMContainerRegistry]: /powershell/module/azurerm.containerregistry/New-AzureRMContainerRegistry
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup

<!-- IMAGES> -->
[qs-psh-01-running-app]: ./media/container-registry-get-started-powershell/qs-psh-01-running-app.png
