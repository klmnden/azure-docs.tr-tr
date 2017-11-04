---
title: "Kapsayıcı görüntüleri oluşturmak için Azure Service Fabric | Microsoft Docs"
description: "Kapsayıcı görüntüler için birden çok kapsayıcı Service Fabric uygulaması oluşturmayı öğrenin."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Docker, kapsayıcıları, mikro, Service Fabric, Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 08b3cc4a52c09ee03818b563794ef9b009d12ef4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-container-images-for-service-fabric"></a>Service Fabric için kapsayıcı görüntüleri oluşturma

Bu öğreticide her biri bir Linux Service Fabric kümesi kapsayıcıları kullanmayı gösteren öğretici bir parçasıdır. Bu öğreticide, bir çok kapsayıcı uygulaması Service Fabric ile kullanım için hazırlanır. Sonraki öğreticilerde, bu görüntüleri bir Service Fabric uygulaması bir parçası olarak kullanılır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz: 

> [!div class="checklist"]
> * Kopya Uygulama kaynağı github'dan  
> * Uygulama kaynağından bir kapsayıcı görüntüsü oluşturun
> * Azure kapsayıcı kayıt defteri (ACR) örneğini dağıtma
> * ACR için bir kapsayıcı görüntü etiketi
> * ACR için görüntüyü karşıya yükleme

Bu öğretici serisinde öğrenin nasıl yapılır: 

> [!div class="checklist"]
> * Service Fabric için kapsayıcı görüntüleri oluşturma
> * [Derleme ve Service Fabric uygulaması ile kapsayıcıları çalıştırma](service-fabric-tutorial-package-containers.md)
> * [Service Fabric yük devretme ve ölçeklendirme nasıl işlenir](service-fabric-tutorial-containers-failover.md)

## <a name="prerequisites"></a>Ön koşullar

- Service Fabric için Linux geliştirme ortamı ayarlayın. Yönergeleri izleyerek [burada](service-fabric-get-started-linux.md) Linux ortamınızı ayarlamak için. 
- Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 
- Ayrıca, bir Azure aboneliğine sahip olmasını gerektirir. Ücretsiz deneme sürümü hakkında daha fazla bilgi için Git [burada](https://azure.microsoft.com/free/).

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticide kullanılan örnek bir oylama uygulaması uygulamasıdır. Uygulama bir ön uç web bileşeni ve bir arka uç Redis örneği oluşur. Bileşenleri kapsayıcı görüntülere paketlenir. 

Geliştirme ortamınızı uygulamaya bir kopyasını indirmek için Git kullanın.

```bash
git clone https://github.com/Azure-Samples/service-fabric-dotnet-containers.git

cd service-fabric-dotnet-containers/Linux/container-tutorial/
```

'Kapsayıcı-tutorial' dizini 'azure-oy' adında bir klasör içerir. Bu 'azure-oy' klasörü, ön uç kaynak kodu ve ön uç oluşturmak için bir Dockerfile içerir. 'Kapsayıcı-tutorial' dizin redis görüntü oluşturmak için Dockerfile olan 'redis' dizin de içerir. Bu dizinleri, Bu öğretici kümesi için gerekli varlıkları içerir. 

## <a name="create-container-images"></a>Kapsayıcı görüntüleri oluşturma

'Azure-oy' dizini içinde ön uç web bileşeni için görüntü oluşturmak için aşağıdaki komutu çalıştırın. Bu komut, görüntü oluşturmak için bu dizinde Dockerfile kullanır. 

```bash
docker build -t azure-vote-front .
```

Redis arka uç için görüntü oluşturmak için aşağıdaki komutu çalıştırın içinde 'redis' dizin. Bu komut, görüntü oluşturmak için dizinde Dockerfile kullanır. 

```bash
docker build -t azure-vote-back .
```

Tamamlandığında kullanmak [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) oluşturulan görüntüleri görmek için komutu.

```bash
docker images
```

Dört görüntüleri indirilebilir veya oluşturulan dikkat edin. *Azure oy ön* görüntü, uygulama içerir. Türetilmiş bir *python* Docker hub'a görüntüden. Redis görüntü Docker Hub'ından yüklendi.

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-back              latest              bf9a858a9269        3 seconds ago        107MB
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
redis                        latest              9813a7e8fcc0        2 days ago           107MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB

```

## <a name="deploy-azure-container-registry"></a>Azure kapsayıcı kayıt defteri dağıtın

İlk çalıştırma [az oturum açma](/cli/azure/login) Azure hesabınızda oturum açmak için komutu. 

Ardından, kullanın [az hesabı](/cli/azure/account#set) komutu Azure kapsayıcı kayıt oluşturmak için aboneliğinizi seçin. 

```bash
az account set --subscription <subscription_id>
```

Azure kapsayıcı kayıt defteri dağıtırken, önce bir kaynak grubu gerekir. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Bu örnekte, bir kaynak grubu adında *myResourceGroup* oluşturulan *westus* bölge. Lütfen yakın bir bölgede kaynak grubu seçin. 

```bash
az group create --name myResourceGroup --location westus
```

Azure kapsayıcı kayıt defteri ile oluşturma [az acr oluşturmak](/cli/azure/acr#create) komutu. Bir kapsayıcı kayıt defteri adını **benzersiz olmalıdır**.

```bash
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

Bu öğreticinin geri kalanını, "acrname" yer tutucu olarak seçtiğiniz kapsayıcı kayıt defteri adını kullanıyoruz.

## <a name="log-in-to-your-container-registry"></a>Kapsayıcı kaydınız oturum açın

Görüntüleri göndermeden önce ACR Örneğiniz için oturum açın. Kullanım [az acr oturum açma](/cli/azure/acr?view=azure-cli-latest#az_acr_login) işlemi tamamlamak için komutu. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz bir ad sağlayın.

```bash
az acr login --name <acrName>
```

Komut tamamlandıktan sonra 'Başarılı oturum açma' iletisi döndürür.

## <a name="tag-container-images"></a>Etiket kapsayıcı görüntüleri

Her kapsayıcı görüntü kayıt loginServer adıyla etiketlenmesi gerekiyor. Bu etiket kapsayıcı görüntüleri bir görüntü kayıt defterine Ftp'den zaman yönlendirme için kullanılır.

Geçerli görüntüleri listesini görmek için [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) komutu.

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-back              latest              bf9a858a9269        3 seconds ago        107MB
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
redis                        latest              9813a7e8fcc0        2 days ago           107MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB
```

LoginServer adını almak için aşağıdaki komutu çalıştırın:

```bash
az acr show --name <acrName> --query loginServer --output table
```

Şimdi, etiket *azure oy ön* kapsayıcı kayıt defteri loginServer görüntüsüyle. Ayrıca, ekleme `:v1` sonuna kadar görüntü adı. Bu etiket resim sürümünü gösterir.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Ardından, etiket *azure oy geri* kapsayıcı kayıt defteri loginServer görüntüsüyle. Ayrıca, ekleme `:v1` sonuna kadar görüntü adı. Bu etiket resim sürümünü gösterir.

```bash
docker tag azure-vote-back <acrLoginServer>/azure-vote-back:v1
```

Etiketli sonra 'docker görüntüleri' çalıştırma işlemi doğrulamak için.


Çıktı:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
azure-vote-back                        latest              bf9a858a9269        22 minutes ago      107MB
<acrName>.azurecr.io/azure-vote-back    v1                  bf9a858a9269        22 minutes ago      107MB
azure-vote-front                       latest              052c549a75bf        23 minutes ago      708MB
<acrName>.azurecr.io/azure-vote-front   v1                  052c549a75bf        23 minutes ago      708MB
redis                                  latest              9813a7e8fcc0        2 days ago          107MB
tiangolo/uwsgi-nginx-flask             python3.6           590e17342131        5 days ago          707MB

```

## <a name="push-images-to-registry"></a>Kayıt defteri itme görüntüleri

Anında *azure oy ön* kayıt defterine görüntü. 

Aşağıdaki örneği kullanarak, ortamınızdan loginServer ACR loginServer adını değiştirin.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

Anında *azure oy geri* kayıt defterine görüntü. 

Aşağıdaki örneği kullanarak, ortamınızdan loginServer ACR loginServer adını değiştirin.

```bash
docker push <acrLoginServer>/azure-vote-back:v1
```

Docker itme komutları birkaç tamamlamak için dakika alın.

## <a name="list-images-in-registry"></a>Kayıt defterinde listesi görüntüler

Azure kapsayıcı kaydınız gönderilen görüntüleri listesini döndürmek için kullanın [az acr deposu listesi](/cli/azure/acr/repository#list) komutu. Komut ACR örnek adıyla güncelleştirin.

```bash
az acr repository list --name <acrName> --output table
```

Çıktı:

```bash
Result
----------------
azure-vote-back
azure-vote-front
```

Eğitmen tamamlandığında, kapsayıcı görüntünün bir özel Azure kapsayıcı kayıt defteri örneğinde zamandır depolanmış. Bu görüntü ACR bir Service Fabric kümesi için sonraki öğreticilerde dağıtılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uygulama Github'dan çekilen ve kapsayıcı görüntüleri oluşturulan ve bir kayıt defterine gönderilir. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Kopya Uygulama kaynağı github'dan  
> * Uygulama kaynağından bir kapsayıcı görüntüsü oluşturun
> * Azure kapsayıcı kayıt defteri (ACR) örneğini dağıtma
> * ACR için bir kapsayıcı görüntü etiketi
> * ACR için görüntüyü karşıya yükleme

Yeoman kullanarak bir Service Fabric uygulamasına paketleme kapsayıcıları hakkında bilgi edinmek için sonraki öğretici ilerleyin. 

> [!div class="nextstepaction"]
> [Paket ve bir Service Fabric uygulaması olarak kapsayıcıları dağıtın](service-fabric-tutorial-package-containers.md)
