---
title: Azure Service Fabric Mesh uygulamada Hizmetleri ölçeklendirme | Microsoft Docs
description: Service Fabric Mesh Azure CLI kullanarak üzerinde çalışan bir uygulama içindeki Hizmetler bağımsız olarak ölçeklendirmeyi öğrenin.
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
ms.date: 08/08/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: e68bcd135c33c7fd83908b8fed0fd098a698fd36
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054751"
---
# <a name="scale-services-within-an-application-running-on-service-fabric-mesh"></a>Service Fabric Mesh üzerinde çalışan bir uygulama içinde Hizmetleri ölçeklendirme

Bu makalede, bir uygulama içinde mikro hizmetler bağımsız olarak ölçeklendirme gösterilmektedir. Bu örnekte, iki mikro görsel nesneler uygulama oluşur: `web` ve `worker`.

`web` Tarayıcıda üçgenler gösteren bir web sayfası ile bir ASP.NET Core uygulaması hizmetidir. Tarayıcı her örneği için bir üçgen görüntüler `worker` hizmeti.

`worker` Hizmeti bu üçgen alana önceden tanımlanmış bir aralık taşır ve üçgeni konumunu gönderir `web` hizmeti. Adresini çözümlemek için DNS kullandığı `web` hizmeti.

## <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama

Bu görevi tamamlamak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. Şu [yönergeleri](service-fabric-mesh-howto-setup-cli.md) izleyerek Azure Service Fabric Mesh CLI uzantısı modülünü yükleyin.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure'da oturum açın ve aboneliğinizi ayarlayın.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Uygulamanın dağıtılacağı kaynak grubunu oluşturun.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application-with-one-worker-service"></a>Bir alt hizmeti içeren uygulama dağıtma

Uygulamanızı kullanarak kaynak grubunu oluşturma `deployment create` komutu.

Aşağıdaki örneği kullanarak bir Linux uygulama dağıtır [mesh_rp.base.linux.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json). Bir Windows uygulaması dağıtmak için [[mesh_rp.base.windows.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.windows.json). Windows kapsayıcı görüntüleri Linux kapsayıcı görüntülerinden büyüktür ve dağıtılması daha uzun sürebilir.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
```

Birkaç dakika sonra komut ile döndürülmesi gerekir:

`visualObjectsApp has been deployed successfully on visualObjectsNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Uygulamayı açma

Dağıtım komutu, hizmet uç noktası genel IP adresini döndürür. Uygulama başarıyla dağıttıktan sonra hizmet uç noktası için genel IP adresini alın ve bir tarayıcıda açın. Bu Üçgen Hareketli olan bir web sayfası görüntülenir.

Bu uygulama için ağ kaynak adı `visualObjectsNetwork`. Aşağıdaki komutu kullanarak, açıklama, konum, kaynak grubu, vb. gibi uygulama hakkında daha fazla bilgi görebilirsiniz:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name visualObjectsNetwork
```

## <a name="scale-worker-service"></a>Ölçek `worker` hizmeti

Ölçek `worker` aşağıdaki komutu kullanarak üç örneğine hizmet. Aşağıdaki örneği kullanarak bir Linux uygulama dağıtır [mesh_rp.scaleout.linux.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json). Bir Windows uygulaması dağıtmak için [mesh_rp.scaleout.windows.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.windows.json). Büyük kapsayıcı görüntülerini dağıtmak için daha uzun süreceğini unutmayın.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```

Uygulama başarıyla dağıttıktan sonra tarayıcı üç taşıma üçgenler ile bir web sayfası görüntülemelidir.

## <a name="delete-the-resources"></a>Kaynakları Sil

Sık Azure'da artık kullanmadığınız kaynakları silin. Bu örnek için ilgili kaynakları silmek için aşağıdaki komutu kullanarak (kaynak grubuyla ilişkili olan her şeyi birlikte) dağıtmış kaynak grubunu silin:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- Görsel nesneler örnek uygulamayı görünümünde [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects).
- Service Fabric kaynak modeli hakkında daha fazla bilgi için bkz: [Service Fabric Mesh kaynak modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md).