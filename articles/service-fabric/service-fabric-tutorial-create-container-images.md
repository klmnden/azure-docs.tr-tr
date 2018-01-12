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
ms.openlocfilehash: 9ea5be818cfc104c243ce31cc0e2d0f10135259f
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
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

## <a name="prerequisites"></a>Önkoşullar

- Service Fabric için Linux geliştirme ortamı ayarlayın. Yönergeleri izleyerek [burada](service-fabric-get-started-linux.md) Linux ortamınızı ayarlamak için. 
- Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 
- Ayrıca, bir Azure aboneliğine sahip olmasını gerektirir. Ücretsiz deneme sürümü hakkında daha fazla bilgi için Git [burada](https://azure.microsoft.com/free/).

## <a name="get-application-code"></a>Uygulama kodunu alma

Bu öğreticide kullanılan örnek bir oylama uygulaması uygulamasıdır. Uygulama bir ön uç web bileşeni ve bir arka uç Redis örneği oluşur. Bileşenleri kapsayıcı görüntülere paketlenir. 

Geliştirme ortamınızı uygulamaya bir kopyasını indirmek için Git kullanın.

```bash
git clone https://github.com/Azure-Samples/service-fabric-containers.git

cd service-fabric-containers/Linux/container-tutorial/
```

Çözüm iki klasör içerir ve bir ' docker-compse.yml' dosyası. 'Azure-oy' klasörü görüntü oluşturmak için kullanılan Dockerfile birlikte Python ön uç hizmeti içerir. 'Oylama' dizini kümeye dağıtılan Service Fabric uygulama paketini içerir. Bu dizinleri, Bu öğretici için gerekli varlıkları içerir.  

## <a name="create-container-images"></a>Kapsayıcı görüntüleri oluşturma

İçinde **azure oy** dizin, ön uç web bileşeni için görüntü oluşturmak için aşağıdaki komutu çalıştırın. Bu komut, görüntü oluşturmak için bu dizinde Dockerfile kullanır. 

```bash
docker build -t azure-vote-front .
```

Bu komut, tüm gerekli bağımlılıkları Docker hub'dan çekebilir gerektiği biraz zaman alabilir. Tamamlandığında kullanmak [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) oluşturulan görüntüleri görmek için komutu.

```bash
docker images
```

İki görüntü indirilebilir veya oluşturulan dikkat edin. *Azure oy ön* görüntü, uygulama içerir. Türetilmiş bir *python* Docker hub'a görüntüden.

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB

```

## <a name="deploy-azure-container-registry"></a>Azure kapsayıcı kayıt defteri dağıtın

İlk çalıştırma **az oturum açma** Azure hesabınızda oturum açmak için komutu. 

```bash
az login
```

Ardından, kullanın **az hesabı** komutu Azure kapsayıcı kayıt oluşturmak için aboneliğinizi seçin. Abonelik kimliği, Azure aboneliğinizin < ABONELİK_KİMLİĞİ > yerine girmeniz gerekir. 

```bash
az account set --subscription <subscription_id>
```

Azure kapsayıcı kayıt defteri dağıtırken, önce bir kaynak grubu gerekir. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

**az group create** komutuyla bir kaynak grubu oluşturun. Bu örnekte, bir kaynak grubu adında *myResourceGroup* oluşturulan *westus* bölge.

```bash
az group create --name <myResourceGroup> --location westus
```

Azure kapsayıcı kayıt defteri ile oluşturma **az acr oluşturmak** komutu. Değiştir \<acrName > aboneliğinizi altında oluşturmak istediğiniz kapsayıcı kayıt defteri adı. Bu ad benzersiz ve alfasayısal olmalıdır. 

```bash
az acr create --resource-group <myResourceGroup> --name <acrName> --sku Basic --admin-enabled true
```

Bu öğreticinin geri kalanını, "acrName" yer tutucu olarak seçtiğiniz kapsayıcı kayıt defteri adını kullanıyoruz. Lütfen bu değeri not edin. 

## <a name="log-in-to-your-container-registry"></a>Kapsayıcı kaydınız oturum açın

Görüntüleri göndermeden önce ACR Örneğiniz için oturum açın. Kullanım **az acr oturum açma** işlemi tamamlamak için komutu. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz bir ad sağlayın.

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
azure-vote-front             latest              052c549a75bf        About a minute ago   708MB
tiangolo/uwsgi-nginx-flask   python3.6           590e17342131        5 days ago           707MB
```

LoginServer adını almak için aşağıdaki komutu çalıştırın:

```bash
az acr show --name <acrName> --query loginServer --output table
```

Aşağıdaki sonuçları içeren bir tablo çıkarır. Bu sonuç etiketi için kullanılacak, **azure oy ön** sonraki adımda kapsayıcı kayıt defterine Ftp'den önce görüntü.

```bash
Result
------------------
<acrName>.azurecr.io
```

Şimdi, etiket *azure oy ön* kapsayıcı kaydınız loginServer görüntüsüyle. Ayrıca, ekleme `:v1` sonuna kadar görüntü adı. Bu etiket resim sürümünü gösterir.

```bash
docker tag azure-vote-front <acrName>.azurecr.io/azure-vote-front:v1
```

Etiketli sonra 'docker görüntüleri' çalıştırma işlemi doğrulamak için.


Çıktı:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                       latest              052c549a75bf        23 minutes ago      708MB
<acrName>.azurecr.io/azure-vote-front   v1                  052c549a75bf       23 minutes ago      708MB
tiangolo/uwsgi-nginx-flask             python3.6           590e17342131        5 days ago          707MB

```

## <a name="push-images-to-registry"></a>Kayıt defteri itme görüntüleri

Anında *azure oy ön* kayıt defterine görüntü. 

Aşağıdaki örneği kullanarak, ortamınızdan loginServer ACR loginServer adını değiştirin.

```bash
docker push <acrName>.azurecr.io/azure-vote-front:v1
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