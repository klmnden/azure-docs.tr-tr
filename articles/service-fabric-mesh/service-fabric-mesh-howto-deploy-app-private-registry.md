---
title: Bir Service Fabric Mesh uygulaması bir özel kapsayıcı görüntüsünü kayıt defteri için Azure Service Fabric Mesh dağıtma | Microsoft Docs
description: Bir uygulamadan bir özel kapsayıcı görüntüsünü kayıt için Azure CLI kullanarak Service Fabric Mesh dağıtmayı öğrenin.
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
ms.date: 08/20/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: a2791c86512790f36477e562cb82718174df8dc3
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296607"
---
# <a name="deploy-a-service-fabric-mesh-app-from-a-private-container-image-registry"></a>Bir özel kapsayıcı görüntüsünü kayıt defterinden bir Service Fabric Mesh uygulaması dağıtma

Bu makalede, özel kapsayıcı görüntüsünü kayıt defteri kullanan bir Azure Service Fabric Mesh uygulamasının nasıl dağıtılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

### <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama

Bu görevi tamamlamak için Azure CLI'yı yerel bir yüklemesini kullanın. Şu [yönergeleri](service-fabric-mesh-howto-setup-cli.md) izleyerek Azure Service Fabric Mesh CLI uzantısı modülünü yükleyin.

## <a name="install-docker"></a>Docker'ı yükleme

#### <a name="windows-10"></a>Windows 10

Service Fabric Mesh tarafından kullanılan kapsayıcılı Service Fabric uygulamalarını desteklemek için en yeni [Docker Community Edition for Windows][download-docker] sürümünü indirin ve yükleyin.

Yükleme sırasında, istendiğinde **Linux kapsayıcıları yerine Windows kapsayıcılarını kullan**'ı seçin. 

Hyper-V makinenizde etkin değilse Docker yükleyicisi ile etkinleştirebilirsiniz. Bu seçenek sunulursa **Tamam**'a tıklayın.

#### <a name="windows-server-2016"></a>Windows Server 2016

Hyper-V rolleri etkin değilse PowerShell'i yönetici olarak açın ve aşağıdaki komutu çalıştırarak Hyper-V'yi etkinleştirdikten sonra bilgisayarınızı yeniden başlatın. Daha fazla bilgi için bkz. [Windows Server için Docker Enterprise Edition][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Bilgisayarınızı yeniden başlatın.

Docker'ı yüklemek için PowerShell'i yönetici olarak açın ve aşağıdaki komutu çalıştırın:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure'da oturum açın ve etkin aboneliği ayarlayın.

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-a-container-registry-and-push-an-image-to-it"></a>Kapsayıcı kayıt defteri oluşturmak ve görüntüyü iletin

Bir Azure kapsayıcı kayıt defteri oluşturmak için aşağıdaki adımları kullanın.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın *myResourceGroup* içinde *eastus* konumu.

```azurecli
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bir Azure container registry (ACR) örneği kullanarak oluşturma `az acr create` komutu. Kayıt defteri adı Azure'da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Aşağıdaki örnekte, adı *myContainerRegistry007* kullanılır. Kayıt defteri adı kullanımda olan bir hata alırsanız, farklı bir ad seçin. Her yerde ad kullanımı `<acrName>` bu Yönergelerdeki görünür.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Kayıt defteri oluşturulduğunda, aşağıdakine benzer bir çıktı görürsünüz:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="push-the-image-to-azure-container-registry"></a>Azure Container Registry'ye görüntü gönderme

Bir Azure container registry (ACR) görüntü gönderebilmeniz için öncelikle bir kapsayıcı görüntüsü olması gerekir. Henüz yerel kapsayıcı görüntünüz yoksa, çekme aşağıdaki komutu çalıştırarak görüntüyü Docker hub'dan (docker simgesi sağ tıklatıp seçerek Linux görüntüleri ile çalışmak için Docker geçmeniz gerekebilir **Linuxkapsayıcılarıiçingeçiş**).

```bash
docker pull seabreeze/azure-mesh-helloworld:1.1-alpine
```

Kayıt defterinize görüntü gönderebilmeniz için, ACR oturum açma sunucunuzun tam adını etiketlemeniz gerekir.

ACR örneğinizin tam oturum açma sunucusu adını almak için aşağıdaki komutu çalıştırın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Döndürülen tam oturum açma sunucu adını için olarak anılacaktır `<acrLoginServer>` bu makalenin geri kalanında.

Şimdi, docker görüntüsünü kullanarak etiket [docker tag] [ docker-tag] komutu. Aşağıdaki komutta `<acrLoginServer>` oturum açma sunucusu adıyla bildirilen yukarıdaki komutu. Aşağıdaki örnek seabreeze/azure-kafes etiketleri-helloworld:1.1-alpine görüntü. Farklı bir görüntü kullanıyorsanız, aşağıdaki komutta görüntü adı değiştirin.

```bash
docker tag seabreeze/azure-mesh-helloworld:1.1-alpine <acrLoginServer>/seabreeze/azure-mesh-helloworld:1.1-alpine
```

Örneğin, `docker tag seabreeze/azure-mesh-helloworld:1.1-alpine myContainerRegistry007.azurecr.io/seabreeze/azure-mesh-helloworld:1.1-alpine`

Azure Container Registry'ye oturum açın.

```bash
az acr login -n <acrName>
```

Örneğin, `az acr login -n myContainerRegistry007`

Aşağıdaki komutla azure container registry'ye görüntü gönderme:

```bash
docker push <acrLoginServer>/seabreeze/azure-mesh-helloworld:1.1-alpine
```

Örneğin, `docker push myContainerRegistry007.azurecr.io/seabreeze/azure-mesh-helloworld:1.1-alpine`

### <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Aşağıdaki örnek, kayıt defterindeki depoları listeler. Aşağıdaki örneklerde azure kullandığınız varsayılmıştır-mesh-helloworld:1.1-alpine görüntü. Farklı bir görüntü kullanıyorsanız, azure ağı helloworld görüntü kullanıldığı adını değiştirin.

```azurecli
az acr repository list --name <acrName> --output table
```
Örneğin, `az acr repository list --name myContainerRegistry007 --output table`

Çıktı:

```bash
Result
-------------------------------
seabreeze/azure-mesh-helloworld
```

Aşağıdaki örnekte yer alan etiketlerin listesi **azure kafes helloworld** depo.

```azurecli
az acr repository show-tags --name <acrName> --repository seabreeze/azure-mesh-helloworld --output table
```

Örneğin, `az acr repository show-tags --name myContainerRegistry007 --repository seabreeze/azure-mesh-helloworld --output table`

Çıktı:

```bash
Result
--------
1.1-alpine
```

Önceki çıkış varlığını doğrular `azure-mesh-helloworld:1.1-alpine` özel kapsayıcı kayıt defterinde.

## <a name="retrieve-credentials-for-the-registry"></a>Kayıt defteri kimlik bilgilerini alma

> [!IMPORTANT]
> Bir Azure kapsayıcı kayıt defterindeki yönetici kullanıcı etkinleştiriliyor üretim senaryoları için önerilmez. Burada, bu gösteride kısa tutmak için gerçekleştirilir. Üretim senaryolarında kullanılması bir [hizmet sorumlusu](https://docs.microsoft.com/azure/container-registry/container-registry-auth-service-principal) üretim senaryolarında hem kullanıcı hem de sistem kimlik doğrulaması için.

Oluşturulan kayıt defterinden bir kapsayıcı örneği dağıtmak için dağıtım sırasında kimlik bilgilerini sağlamanız gerekir. Aşağıdaki komutu kullanarak kayıt defterinizde yönetici kullanıcıyı etkinleştirin:

```azurecli-interactive
az acr update --name <acrName> --admin-enabled true
```

Örneğin, `az acr update --name myContainerRegistry007 --admin-enabled true`

Aşağıdaki komutları kullanarak kayıt defteri sunucusu adı, kullanıcı adını ve parolasını alın:

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
az acr credential show --name <acrName> --query username
az acr credential show --name <acrName> --query "passwords[0].value"
```

Önceki komut tarafından sağlanan değerler olarak başvurulan `<acrLoginServer>`, `<acrUserName>`, ve `<acrPassword>` aşağıda.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Uygulama ve aşağıdaki komutu kullanarak ilgili kaynakları oluşturmak ve önceki adımdan gelen kimlik bilgilerini sağlayın.

`registry-password` Parametredir şablondaki güvenli bir dize. Dağıtım durumunu görüntülenmez ve `az mesh service show` komutları.

Bash konsolu kullanıyorsanız, aşağıdaki komutu çalıştırın:

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}, \"registry-server\": {\"value\": \"<acrLoginServer>\"}, \"registry-username\": {\"value\": \"<acrUserName>\"}, \"registry-password\": {\"value\": \"<acrPassword>\"}}"
```

PowerShell konsolu kullanıyorsanız, aşağıdaki komutu çalıştırın:

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{'location': {'value': 'eastus'}, 'registry-server': {'value': '<acrLoginServer>'}, 'registry-username': {'value': '<acrUserName>'}, 'registry-password': {'value': '<acrPassword>'}}"
```

Birkaç dakika içinde görmeniz gerekir:

`helloWorldPrivateRegistryApp has been deployed successfully on helloWorldPrivateRegistryNetwork with public ip address <IP Address>`

## <a name="open-the-application"></a>Uygulamayı açma

Uygulama başarıyla dağıttıktan sonra hizmet uç noktası için genel IP adresini alın ve bir tarayıcıda açın. Bu, Service Fabric Mesh logosu ile bir web sayfası görüntülenir.

Dağıtım komutu, hizmet uç noktası genel IP adresini döndürür. İsteğe bağlı olarak, hizmet uç noktası genel IP adresini bulmak için ağ kaynağı sorgulayabilirsiniz. 
 
Bu uygulama için ağ kaynak adı `helloWorldPrivateRegistryNetwork`, aşağıdaki komutu kullanarak hakkında bilgi getirir. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name helloWorldPrivateRegistryNetwork
```

## <a name="delete-the-resources"></a>Kaynakları Sil

Sık Azure'da artık kullanmadığınız kaynakları silin. Bu örnek için ilgili kaynakları silmek için aşağıdaki komutu kullanarak (kaynak grubuyla ilişkili olan her şeyi birlikte) dağıtmış kaynak grubunu silin:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

Bu alıştırma için Linux görüntüleri çalışmak ve Windows görüntülerle yeniden çalışma için docker simgesine sağ tıklayıp gitmeniz gerekiyor Docker geçtiyseniz **Windows kapsayıcılarına geç**

## <a name="next-steps"></a>Sonraki adımlar

- Hello World örnek uygulamasını görünümünde [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld).
- Service Fabric kaynak modeli hakkında daha fazla bilgi için bkz: [Service Fabric Mesh kaynak modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md).

[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/