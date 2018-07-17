---
title: Azure Service Fabric Mesh uygulamada Hizmetleri ölçeklendirme | Microsoft Docs
description: Service Fabric Mesh Azure CLI kullanarak üzerinde çalışan bir uygulama içindeki Hizmetler bağımsız olarak ölçeklendirmeyi öğrenin.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: c0350b767b65aee0c4611bb8fa6f635a651d33dc
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076553"
---
# <a name="scale-services-within-an-application-running-on-service-fabric-mesh"></a>Service Fabric Mesh üzerinde çalışan bir uygulama içinde Hizmetleri ölçeklendirme

Bu makalede, bir uygulama içinde mikro hizmetler bağımsız olarak ölçeklendirme gösterilmektedir. Bu örnekte, iki mikro görsel nesneler uygulama oluşur; `web` ve `worker`. 

`web` Tarayıcıda üçgenler gösteren bir web sayfası ile bir ASP.NET Core uygulaması hizmetidir. Tarayıcı her örneği için bir üçgen görüntüler `worker` hizmeti. 

`worker` Hizmeti bu üçgen alana önceden tanımlanmış bir aralık taşır ve üçgeni konumunu gönderir `web` hizmeti. Adresini çözümlemek için DNS kullandığı `web` hizmeti.

## <a name="set-up-service-fabric-mesh-cli"></a>Mesh Service Fabric CLI Kurulumu 
Bu görevi tamamlamak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. İzleyerek Azure Service Fabric CLI'sını Mesh uzantısı modülünü yükleme [yönergeleri](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
Azure'da oturum açın ve aboneliğinizi ayarlayın.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma
Uygulamayı dağıtmak için bir kaynak grubu oluşturun. Mevcut bir kaynak grubunu kullanın ve bu adımı atlayın. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application-with-one-worker-service"></a>Bir alt hizmeti içeren uygulama dağıtma
Uygulamanızı kullanarak kaynak grubunu oluşturma `deployment create` komutu.

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```
Önceki komutu kullanarak bir Linux dağıtır [mesh_rp.base.linux.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json). Bir Windows uygulamasını dağıtmak istiyorsanız, kullanın [mesh_rp.base.windows.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.windows.json). Windows kapsayıcı görüntüleri Linux kapsayıcı görüntülerini daha büyüktür ve dağıtmak için daha fazla zaman alabilir.

Birkaç dakika içinde komutunuz ile döndürülmesi gerekir:

`visualObjectsApp has been deployed successfully on visualObjectsNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Uygulamayı açın
Uygulama başarıyla dağıttıktan sonra hizmet uç noktası için genel IP adresini alın ve bir tarayıcıda açın. Bir üçgen alanı taşıma ile bir web sayfası görüntülemelidir.

Dağıtım komutu, hizmet uç noktası genel IP adresini döndürür. Ayrıca, hizmet uç noktası genel IP adresini bulmak için ağ kaynağı sorgulayabilirsiniz.
 
Bu uygulama için ağ kaynak adı `visualObjectsNetwork`, aşağıdaki komutu kullanarak hakkında bilgi getirir. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name visualObjectsNetwork
```

## <a name="scale-worker-service"></a>Ölçek `worker` hizmeti

Ölçek `worker` aşağıdaki komutu kullanarak üç örneğine hizmet. 

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```
Önceki komutu kullanarak bir Linux dağıtır [mesh_rp.scaleout.linux.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json). Bir Windows uygulamasını dağıtmak istiyorsanız, kullanın [mesh_rp.scaleout.windows.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.windows.json). Windows kapsayıcı görüntüleri Linux kapsayıcı görüntülerini daha büyüktür ve dağıtmak için daha fazla zaman alabilir.

Uygulama başarıyla dağıttıktan sonra tarayıcı alanı taşıma üç üçgenler ile bir web sayfası görüntüleme.

## <a name="delete-the-resources"></a>Kaynakları Sil

Önizleme programı için atanan sınırlı kaynak tasarrufu yapmak için sık kaynakları silin. Bu örnek için ilgili kaynakları silmek için Dağıtılmış kaynak grubunu silin.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Sonraki adımlar
- Görsel nesneler örnek uygulamayı görünümünde [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects).
- Service Fabric kaynak modeli hakkında daha fazla bilgi için bkz: [Service Fabric Mesh kaynak modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md).