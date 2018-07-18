---
title: Azure Service Fabric Mesh için özel bir kayıt defterinden uygulama dağıtma | Microsoft Docs
description: Service Fabric Mesh Azure CLI kullanarak için bir özel kapsayıcı kayıt defteri kullanan bir uygulamayı dağıtmayı öğrenin.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/16/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: af92d3c6ea881d00ec687a5560bf4db35aa431c5
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089495"
---
# <a name="deploy-a-service-fabric-mesh-app-from-a-private-container-image-registry"></a>Bir özel kapsayıcı görüntüsünü kayıt defterinden bir Service Fabric Mesh uygulaması dağıtma

Bu makalede, özel kapsayıcı görüntüsünü kayıt defteri kullanan bir Azure Service Fabric Mesh uygulamasının nasıl dağıtılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

### <a name="set-up-service-fabric-mesh-cli"></a>Mesh Service Fabric CLI Kurulumu 
Bu görevi tamamlamak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. İzleyerek Azure Service Fabric CLI'sını Mesh uzantısı modülünü yükleme [yönergeleri](service-fabric-mesh-howto-setup-cli.md).

### <a name="install-docker"></a>Docker'ı yükleyin

Service Fabric Mesh tarafından kullanılan kapsayıcılı bir Service Fabric uygulamaları desteklemek için Docker'ı yükleyin.

### <a name="windows-10"></a>Windows 10

En son sürümünü indirip [Windows için Docker Community Edition][download-docker]. 

Yükleme sırasında seçin **kullanım Windows kapsayıcıları Linux kapsayıcıları yerine** sorulduğunda. Sonra oturumunuzu kapatıp yeniden oturum gerekecektir. Hyper-V, önceden etkinleştirmediyseniz oturum geri sonra Hyper-V'yi etkinleştirmek için istenebilir. Hyper-V etkinleştirin ve bilgisayarınızı yeniden başlatın.

Bilgisayarınız yeniden başlatıldıktan sonra Docker etkinleştirmek isteyip istemediğinizi sorar **kapsayıcıları** özelliği, etkinleştirmek ve bilgisayarınızı yeniden başlatın.

### <a name="windows-server-2016"></a>Windows Server 2016

Docker'ı yüklemek için aşağıdaki PowerShell komutlarını kullanın. Daha fazla bilgi için [Docker Enterprise Edition için Windows Server][download-docker-server].

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Bilgisayarınızı yeniden başlatın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure'da oturum açın ve etkin bir aboneliğiniz ayarlayın:

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-a-container-registry-and-push-an-image-to-it"></a>Kapsayıcı kayıt defteri oluşturmak ve görüntüyü iletin

Yönergeleri izleyerek bir Azure Container Registry oluşturma [Azure CLI ile azure'da özel Docker kayıt defteri oluşturma](../container-registry/container-registry-get-started-azure-cli.md). En fazla adımları [ACR oturum açma](../container-registry/container-registry-get-started-azure-cli.md#log-in-to-acr) adım. 

### <a name="push-image-to-acr"></a>Görüntüyü ACR’ye gönderme

Azure Container kayıt defterine görüntü gönderebilmeniz için önce bir görüntünüz olmalıdır. Henüz yerel kapsayıcı görüntünüz yoksa Docker Hub’dan mevcut bir görüntüyü çekmek için aşağıdaki komutu çalıştırın.

```bash
docker pull seabreeze/azure-mesh-helloworld:1.1-alpine
```

Kayıt defterinize görüntü gönderebilmeniz için, ACR oturum açma sunucunuzun tam adını etiketlemeniz gerekir. ACR örneğinizin tam oturum açma sunucusu adını öğrenmek için aşağıdaki komutu çalıştırın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Görüntüyü [docker tag][docker-tag] komutunu kullanarak etiketleyin. `<acrLoginServer>` değerini, ACR örneğinizin sunucu adıyla değiştirin.

```bash
docker tag seabreeze/azure-mesh-helloworld:1.1-alpine <acrLoginServer>/azure-mesh-helloworld:1.1-alpine
```
### <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Aşağıdaki örnekte, bir kayıt defterinde yer alan depolar listelenmektedir:

```azurecli
az acr repository list --name <acrName> --output table
```

Çıktı:

```bash
Result
----------------
azure-mesh-helloworld
```

Aşağıdaki örnekte yer alan etiketlerin listesi **azure kafes helloworld** depo.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-mesh-helloworld --output table
```

Çıktı:

```bash
Result
--------
1.1-alpine
```
Önceki çıkış varlığını doğrular `azure-mesh-helloworld:1.1-alpine` özel kapsayıcı kayıt defterinde.

## <a name="retrieve-credentials-for-the-registry"></a>Kayıt defteri kimlik bilgilerini alma

Oluşturulan kayıt defterinden bir kapsayıcı örneği dağıtmak için dağıtım sırasında kimlik bilgileri sağlanmalıdır. Aşağıdaki komutu kullanarak kayıt defterinizde yönetici kullanıcıyı etkinleştirin:

```azurecli-interactive
az acr update --name <acrName> --admin-enabled true
```

Aşağıdaki komutları kullanarak kayıt defteri sunucusu adı, kullanıcı adını ve parolasını alın:

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
az acr credential show --name <acrName> --query username
az acr credential show --name <acrName> --query "passwords[0].value"
```

Önceki komut tarafından sağlanan değerler olarak başvurulan `<acrLoginServer>`, `<acrUserName>`, ve `<acrPassword>` aşağıdaki komutta.


## <a name="deploy-the-template"></a>Şablonu dağıtma

Uygulama ve aşağıdaki komutu kullanarak ilgili kaynakları oluşturmak ve önceki adımdan gelen kimlik bilgilerini sağlayın.

`registry-password` Şablondaki parametresi bir `securestring`. Dağıtım durumunu görüntülenmez ve `az mesh service show` komutları. Aşağıdaki komutta doğru belirtildiğinden emin olun.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}, \"registry-server\": {\"value\": \"<acrLoginServer>\"}, \"registry-username\": {\"value\": \"<acrUserName>\"}, \"registry-password\": {\"value\": \"<acrPassword>\"}}" 
```

Birkaç dakika içinde komutunuz ile döndürülmesi gerekir:

`helloWorldPrivateRegistryApp has been deployed successfully on helloWorldPrivateRegistryNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Uygulamayı açın
Uygulama başarıyla dağıttıktan sonra hizmet uç noktası için genel IP adresini alın ve bir tarayıcıda açın. Bu, Service Fabric Mesh logosu ile bir web sayfası görüntülenir.

Dağıtım komutu, hizmet uç noktası genel IP adresini döndürür. İsteğe bağlı olarak, hizmet uç noktası genel IP adresini bulmak için ağ kaynağı sorgulayabilirsiniz. 
 
Bu uygulama için ağ kaynak adı `helloWorldPrivateRegistryNetwork`, aşağıdaki komutu kullanarak hakkında bilgi getirir. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name helloWorldPrivateRegistryNetwork
```

## <a name="delete-the-resources"></a>Kaynakları Sil

Önizleme programı için atanan sınırlı kaynak tasarrufu yapmak için sık kaynakları silin. Bu örnek için ilgili kaynakları silmek için Dağıtılmış kaynak grubunu silin.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Sonraki adımlar
- Hello World örnek uygulamasını görünümünde [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld).
- Service Fabric kaynak modeli hakkında daha fazla bilgi için bkz: [Service Fabric Mesh kaynak modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md).

[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/