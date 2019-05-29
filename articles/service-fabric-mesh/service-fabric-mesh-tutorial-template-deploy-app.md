---
title: Öğretici - Azure Service Fabric Mesh’e uygulama dağıtma | Microsoft Docs
description: Bu öğreticide, şablon kullanarak Service Fabric Mesh'e uygulama dağıtmayı öğreneceksiniz.
services: service-fabric-mesh
documentationcenter: .net
author: dkkapur
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/11/2019
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: ce063d8a256cbf2507e19d459aafe13150eccce7
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306941"
---
# <a name="tutorial-deploy-an-application-to-service-fabric-mesh-using-a-template"></a>Öğretici: Şablon kullanarak Service Fabric Mesh’e uygulama dağıtma

Bu öğretici, bir dizinin birinci bölümüdür. Şablon kullanarak bir Azure Service Fabric Mesh uygulaması dağıtmayı öğreneceksiniz.  Bu uygulama ASP.NET web ön uç hizmeti ve bir ASP.NET Core Web API'si arka uç hizmetinden oluştur ve bu bileşenler Docker Hub'da bulunur.  Docker Hub'dan iki kapsayıcı görüntüsü çekip bunları kendi özel kayıt defterinize göndereceksiniz. Ardından uygulama için bir Azure RM şablonu oluşturacak ve uygulamayı kapsayıcı kayıt defterinizden Service Fabric Mesh'e dağıtacaksınız. İşlem tamamlandığında Service Fabric Mesh'te çalışan basit bir Yapılacaklar Listesi uygulamasına sahip olacaksınız.

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Özel bir Azure Container Registry örneği oluşturma
> * Kayıt defterine kapsayıcı görüntüsü gönderme
> * RM şablonunu ve parametre dosyalarını oluşturma
> * Service Fabric Mesh’e uygulama dağıtma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Şablon kullanarak Service Fabric Mesh’e uygulama dağıtma
> * [Service Fabric Mesh’te çalışan bir uygulamadaki hizmetleri ölçeklendirme](service-fabric-mesh-tutorial-template-scale-services.md)
> * [Service Fabric Mesh’te çalışan bir uygulamayı yükseltme](service-fabric-mesh-tutorial-template-upgrade-app.md)
> * [Uygulamayı kaldırma](service-fabric-mesh-tutorial-template-remove-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.

* [Docker'ı yükleyin](service-fabric-mesh-howto-setup-developer-environment-sdk.md#install-docker)

* [Azure CLI ve Service Fabric Mesh CLI’sini yerel olarak yükleyin](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Service Fabric Mesh uygulamanızdaki hizmetlerle ilişkilendirilmiş olan kapsayıcı görüntülerinin bir kapsayıcı kayıt defterinde depolanması gerekir.  Bu öğreticide özel bir Azure Container Registry (ACR) örneği kullanılır. 

ACR örneği oluşturmak için aşağıdaki adımları kullanın.  Kurulu bir ACR örneğiniz varsa bu adımı atlayabilirsiniz.

### <a name="sign-in-to-azure"></a>Oturum açın: Azure

Azure'da oturum açın ve etkin aboneliğinizi ayarlayın.

```azurecli
az login
az account set --subscription "<subscriptionName>"
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki komutu kullanarak *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturun.

```azurecli
az group create --name myResourceGroup --location eastus
```

### <a name="create-the-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

`az acr create` komutunu kullanarak bir ACR örneği oluşturun. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Aşağıdaki örnekte *myContainerRegistry* adı kullanılmıştır. Kayıt defterinin kullanılmakta olduğunu belirten bir hata alırsanız farklı bir ad seçin.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry --sku Basic
```

Kayıt defteri oluşturulduğunda aşağıdakilere benzer bir çıkış görürsünüz:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2018-09-13T19:43:57.388203+00:00",
  "id": "/subscriptions/<subscription>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry",
  "location": "eastus",
  "loginServer": "mycontainerregistry.azurecr.io",
  "name": "myContainerRegistry",
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

## <a name="push-the-images-to-azure-container-registry"></a>Azure Container Registry’ye görüntüleri gönderme

Bu öğreticide örnek olarak Yapılacaklar Listesi örnek uygulaması kullanılmıştır.  [WebFrontEnd](https://hub.docker.com/r/seabreeze/azure-mesh-todo-webfrontend/) ve [ToDoService](https://hub.docker.com/r/seabreeze/azure-mesh-todo-service/) hizmetlerine ait kapsayıcı görüntüleri Docker Hub'da mevcuttur. Bkz: [bir Service Fabric Mesh uygulaması oluşturma](service-fabric-mesh-tutorial-create-dotnetcore.md) uygulamayı Visual Studio'da derleme hakkında bilgi için. Service Fabric Mesh, Windows veya Linux Docker kapsayıcılarıyla çalışabilir.  Linux kapsayıcılarıyla çalışıyorsanız Docker'da **Switch to Linux containers** (Linux kapsayıcılarına geç) öğesini seçin.  Windows kapsayıcılarıyla çalışıyorsanız Docker'da **Switch to Windows containers** (Windows kapsayıcılarına geç) öğesini seçin.

ACR örneğine görüntü gönderebilmeniz için önce bir kapsayıcı görüntünüz olmalıdır. Yerel kapsayıcı görüntünüz yoksa [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanarak [WebFrontEnd](https://hub.docker.com/r/seabreeze/azure-mesh-todo-webfrontend/) ve [ToDoService](https://hub.docker.com/r/seabreeze/azure-mesh-todo-service/) görüntülerini Docker Hub'dan çekin.

Şu Windows görüntülerini çekin:

```bash
docker pull seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709
docker pull seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709
```

Kayıt defterinize görüntü gönderebilmeniz için, ACR oturum açma sunucunuzun tam adını etiketlemeniz gerekir.

ACR örneğinizin tam oturum açma sunucusu adını almak için aşağıdaki komutu çalıştırın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table

AcrLoginServer
---------------------------------
mycontainerregistry.azurecr.io
```

Şimdi [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) komutunu kullanarak Docker görüntünüzü etiketleyin. Aşağıdaki örnek seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709 ve seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709 görüntülerini etiketler. Farklı görüntüler kullanıyorsanız aşağıdaki komutta yer alan görüntü adlarını uygun şekilde değiştirin.

```bash
docker tag seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709 mycontainerregistry.azurecr.io/seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709
docker tag seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709 mycontainerregistry.azurecr.io/seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709
```

Azure Container Registry'ye oturum açın.

```azurecli
az acr login -n myContainerRegistry
```

Görüntüyü [docker push](https://docs.docker.com/engine/reference/commandline/push/) komutuyla ACR örneğine gönderin:

```bash
docker push mycontainerregistry.azurecr.io/seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709
docker push mycontainerregistry.azurecr.io/seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709
```

### <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

ACR örneğinizdeki depoları görmek için şu komutu çalıştırın:

```azurecli
az acr repository list --name myContainerRegistry --output table

Result
-------------------------------
seabreeze/azure-mesh-todo-webfrontend
seabreeze/azure-mesh-todo-service
```

Aşağıdaki örnekte, **azure-mesh-todo-service** deposunda yer alan etiketlerin listesi verilmiştir.

```azurecli
az acr repository show-tags --name myContainerRegistry --repository seabreeze/azure-mesh-todo-service --output table

Result
--------
1.0-nanoserver-1709
```

Yukarıdaki çıkış, özel kapsayıcı kayıt defterinde `azure-mesh-todo-service:1.0-nanoserver-1709` görüntüsünün bulunduğunu onaylar.  Ayrıca `azure-mesh-todo-webfrontend:1.0-nanoserver-1709` görüntüsünün bulunduğunu da doğrular.

## <a name="retrieve-credentials-for-the-registry"></a>Kayıt defteri kimlik bilgilerini alma

> [!IMPORTANT]
> Üretim senaryolarında ACR örneğinde yönetici kullanıcının etkinleştirilmesi önerilmez. Burada kolaylık sağlama açısından yapılmıştır. Üretim senaryolarında kullanıcı ve sistem kimlik doğrulaması için [hizmet sorumlusu](https://docs.microsoft.com/azure/container-registry/container-registry-auth-service-principal) kullanmanız önerilir.

Kayıt defterinden şablon kullanılarak oluşturulmuş olan bir kapsayıcı örneğini dağıtmak için dağıtım sırasında kayıt defteri kimlik bilgilerini sağlamanız gerekir. Öncelikle aşağıdaki komutu kullanarak kayıt defterinizde yönetici kullanıcıyı etkinleştirin:

```azurecli
az acr update --name myContainerRegistry --admin-enabled true
```

Ardından aşağıdaki komutları kullanarak kayıt defteri oturum açma sunucu adı, kullanıcı adı ve parola değerlerini alın:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
az acr credential show --name myContainerRegistry --query username
az acr credential show --name myContainerRegistry --query "passwords[0].value"
```

Bir sonraki bölümde RM şablonunu ve parametreler dosyalarını oluştururken döndürülen ACR oturum açma sunucu adı, kullanıcı adı ve parola değerlerini kullanın.

## <a name="download-and-explore-the-template-and-parameters-files"></a>Şablonu ve parametreler dosyalarını indirin ve keşfedin

Service Fabric Mesh uygulaması, Azure Resource Manager (RM) şablonlarını kullanarak dağıtıp yönetebileceğiniz bir Azure kaynağıdır. Azure çözümlerinizi dağıtma ve yönetme kavramlarına aşina değilseniz bkz. [Azure Resource Manager'a genel bakış](/azure/azure-resource-manager/resource-group-overview) ve [RM Şablonlarının yapısını ve söz dizimini anlama](/azure/azure-resource-manager/resource-group-authoring-templates).

Bu öğreticide örnek olarak Yapılacaklar Listesi örneği kullanılmıştır.  Yeni şablon ve parametre dosyası oluşturmak yerine [mesh_rp.windows.json deployment template](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) ve [mesh_rp.windows.parameter.json parameters](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) dosyalarını indirin.

### <a name="parameters"></a>Parametreler
Şablonunuzda, uygulama dağıtıldıktan sonra değişeceğini tahmin ettiğiniz değerler olduğunda veya dağıtım bazında değiştirme seçeneğine sahip olmak istediğinizde (diğer dağıtımlar için bu şablonu yeniden kullanmayı planlıyorsanız) en iyi uygulama, değerlerin parametrelerini oluşturmaktır. Bunu yapmanın yolu, dağıtım şablonunuzun en üst kısmında bir "parametreler" bölümü oluşturmak ve dağıtım şablonunun sonraki bölümlerinde başvurulacak parametre adlarını ve özelliklerini burada belirtmektir. Her parametre tanımı *type*, *defaultValue* ve isteğe bağlı *description* içeren bir *metadata* bölümüne sahiptir.

Parametreler bölümü dağıtım şablonunuzun en üstünde, *resources* bölümünün hemen öncesinde tanımlanır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
      ...
    },
    "resources": [
```

[mesh_rp.windows.json dağıtım şablonunda](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) şu parametreler bildirilmiştir: location, registryPassword, registryUserName, registryServer, frontEndImage, serviceImage, frontEndCpu, serviceCpu, frontEndMemory, serviceMemory, frontEndReplicaCount, serviceReplicaCount.  Parametrelerin açıklamaları dağıtım şablonu dosyasında mevcuttur.

Bu parametreler [mesh_rp.windows.parameter.json parametreler](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) dosyasında kullanılır.  Ayrı bir parametre dosyası kullandığınızda dağıtım şablonunu güncelleştirmeye gerek kalmadan parametre değerlerini dağıtımlar arasında değiştirebilirsiniz.

### <a name="overview-of-the-application-and-services"></a>Uygulama ve hizmetlere genel bakış

Hizmetler, şablonda uygulama kaynağının özellikleri olarak belirtilir.  Uygulamalar, şablonda bir kaynak olarak bildirilen özel bir ağa dağıtılır.  Hizmetler, verileri kalıcı hale getirmek için şablonda kaynaklar olarak bildirilen birimleri kullanabilir.  Her hizmet için işletim sistemi türü, kod paketleri, çoğaltma sayısı ve ağ bilgileri hizmet özelliği olarak belirtilir.  Her kod paketi için kapsayıcı görüntüsü, uç noktalar, bellek ve CPU kaynakları ile görüntü deposu kimlik bilgileri belirtilir. Birden fazla hizmete sahip bir Service Fabric Mesh uygulaması şablonu genel hatlarıyla şu şekilde görünür:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
  },
  "resources": [
    {
      "apiVersion": "2018-09-01-preview",
      "name": "MyMeshApplication",
      "type": "Microsoft.ServiceFabricMesh/applications",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/networks/MeshAppNetwork",
        "Microsoft.ServiceFabricMesh/volumes/ServiceAVolume"
      ],
      "properties": {
        "services": [
          {
            "name": "ServiceA",
            "properties": {
              "description": "ServiceA description.",
              "osType": "Linux",
              "codePackages": [
                {
                  "name": "ServiceA",
                  "image": "[parameters('frontEndImage')]",
                  "volumeRefs": [
                    {
                      "name": "[resourceId('Microsoft.ServiceFabricMesh/volumes', 'ServiceAVolume')]",
                      "destinationPath": "/app/data"
                    }
                  ],
                  "endpoints": [
                    {
                      "name": "ServiceAListener",
                      "port": 20001
                    }
                  ],
                  "resources": {
                    "requests": {
                      "cpu": "[parameters('frontEndCpu')]",
                      "memoryInGB": "[parameters('frontEndMemory')]"
                    }
                  },
                  "imageRegistryCredential": {
                    "server": "[parameters('registryServer')]",
                    "username": "[parameters('registryUserName')]",
                    "password": "[parameters('registryPassword')]"
                  }
                }
              ],
              "replicaCount": "[parameters('frontEndReplicaCount')]",
              "networkRefs": [
                {
                  "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'MeshAppNetwork')]"
                }
              ]
            }
          },
          {
            "name": "ServiceB",
            ...
          }
        ],
        "description": "Application description."
      }
    },
    {
      "apiVersion": "2018-07-01-preview",
      "name": "MeshAppNetwork",
      "type": "Microsoft.ServiceFabricMesh/networks",
      "location": "[parameters('location')]",
      "dependsOn": [],
      "properties": {
        "description": "MeshAppNetwork description.",
        "addressPrefix": "10.0.0.4/22",
        "ingressConfig": {
          "layer4": [
            {
              "name": "ServiceAIngress",
              "publicPort": "20001",
              "applicationName": "MyMeshApplication",
              "serviceName": "ServiceA",
              "endpointName": "ServiceAListener"
            }
          ]
        }
      }
    },
    {
      "apiVersion": "2018-09-01-preview",
      "name": "ServiceAVolume",
      "type": "Microsoft.ServiceFabricMesh/volumes",
      "location": "[parameters('location')]",
      "dependsOn": [],
      "properties": {
        "description": "Azure Files storage volume for counter App.",
        "provider": "SFAzureFile",
        "azureFileParameters": {
          "shareName": "[parameters('fileShareName')]",
          "accountName": "[parameters('storageAccountName')]",
          "accountKey": "[parameters('storageAccountKey')]"
        }
      }
    }
  ]
}
```

Yapılacaklar Listesi uygulamasına özgü değerler için [mesh_rp.windows.json dağıtım şablonu](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) dosyasını inceleyin.

## <a name="deploy-the-application-to-service-fabric-mesh"></a>Uygulamayı Service Fabric Mesh’e dağıtma
Aşağıdaki komutu kullanarak uygulamayı ve ilgili kaynakları oluşturabilirsiniz. [Kayıt defteri kimlik bilgilerini alma](#retrieve-credentials-for-the-registry) adımındaki kimlik bilgilerini kullanın.

Parametreler dosyasında şu parametre değerlerini güncelleştirin:

|Parametre|Değer|
|---|---|
|konum|Uygulamanın dağıtılacağı bölge.  Örneğin: "eastus".|
|registryPassword|[Kayıt defteri kimlik bilgilerini alma](#retrieve-credentials-for-the-registry) bölümünde aldığınız parola. Bu şablon parametresi güvenli bir dizedir ve dağıtım durumunda veya `az mesh service show` komutlarında görüntülenmez.|
|registryUserName|[Kayıt defteri kimlik bilgilerini alma](#retrieve-credentials-for-the-registry) bölümünde aldığınız kullanıcı adı.|
|registryServer|[Kayıt defteri kimlik bilgilerini alma](#retrieve-credentials-for-the-registry) bölümünde aldığınız kayıt defteri sunucusu adı.|
|frontEndImage|Ön uç hizmetinin kapsayıcı görüntüsü.  Örneğin, `<myregistry>.azurecr.io/seabreeze/azure-mesh-todo-webfrontend:1.0-nanoserver-1709`.|
|serviceImage|Arka uç hizmetinin kapsayıcı görüntüsü.  Örneğin, `<myregistry>.azurecr.io/seabreeze/azure-mesh-todo-service:1.0-nanoserver-1709`.|

Uygulamayı dağıtmak için şu komutu çalıştırın:

```azurecli
az mesh deployment create --resource-group myResourceGroup --template-file c:\temp\mesh_rp.windows.json --parameters c:\temp\mesh_rp.windows.parameters.json
```

Bu komut, aşağıda gösterilen bir JSON parçacığı oluşturur. Altında ```outputs``` JSON çıkışını kopyalama bölümünü ```publicIPAddress``` özelliği.

```json
"outputs": {
    "publicIPAddress": {
    "type": "String",
    "value": "40.83.78.216"
    }
}
```

Bu bilgiler geldiği ```outputs``` ARM şablonu bölümünde. Bu bölümde, aşağıda gösterildiği gibi ağ geçidi kaynağının genel IP adresini Getir başvuruyor. 

```json
  "outputs": {
    "publicIPAddress": {
      "value": "[reference('todolistappGateway').ipAddress]",
      "type": "string"
    }
  }
```

## <a name="open-the-application"></a>Uygulamayı açma

Uygulama başarıyla dağıtıldıktan sonra, hizmet uç noktası genel IP adresini alın. Dağıtım komutu hizmet uç noktasının genel IP adresini döndürür. İsteğe bağlı olarak hizmet uç noktasının genel IP adresini bulmak için ağ kaynağını da sorgulayabilirsiniz. Bu uygulamanın ağ kaynağı adı `todolistappNetwork` şeklindedir ve aşağıdaki komutu kullanarak daha fazla bilgiye ulaşabilirsiniz. 

```azurecli
az mesh gateway show --resource-group myResourceGroup --name todolistappGateway
```

Web tarayıcısında IP adresine gidin.

## <a name="check-application-status"></a>Uygulama durumunu denetleme

app show komutunu kullanarak uygulamanın durumunu denetleyebilirsiniz. Dağıtılan uygulamanın adı "todolistapp" şeklindedir, bu adı kullanarak ayrıntılara ulaşabilirsiniz.

```azurecli
az mesh app show --resource-group myResourceGroup --name todolistapp
```

`az mesh code-package-log get` komutunu kullanarak dağıtılan uygulamanın günlüklerini inceleyin:
```azurecli
az mesh code-package-log get --resource-group myResourceGroup --application-name todolistapp --service-name WebFrontEnd --replica-name 0 --code-package-name WebFrontEnd
```

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Özel kapsayıcı kayıt defteri oluşturma
> * Kayıt defterine kapsayıcı görüntüsü gönderme
> * Şablon ve parametre dosyalarını oluşturma
> * Service Fabric Mesh’e uygulama dağıtma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Service Fabric Mesh’te çalışan bir uygulamayı ölçeklendirme](service-fabric-mesh-tutorial-template-scale-services.md)
