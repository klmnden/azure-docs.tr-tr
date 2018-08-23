---
title: Azure Service Fabric şablon kullanarak Mesh için uygulama dağıtma | Microsoft Docs
description: Azure CLI kullanarak bir şablondan bir .NET Core uygulaması için Service Fabric Mesh dağıtmayı öğrenin.
services: service-fabric-mesh
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
ms.date: 07/12/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 356e8019f9a4a64cb1c1c113d0f44990aa4e0f95
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42057609"
---
# <a name="deploy-a-service-fabric-mesh-application-to-service-fabric-mesh-using-a-template"></a>Bir şablon kullanarak Service Fabric Mesh için Service Fabric Mesh uygulaması dağıtma
Bu makalede, bir şablonu kullanarak Service Fabric Mesh için bir .NET Core uygulamasının nasıl dağıtılacağı gösterilmektedir. Bitirdiğinizde, oylama ön uç, Oylama sonuçlarını kümedeki bir arka uç hizmetine kaydeden bir ASP.NET Core web uygulaması vardır. Ön uç, arka uç hizmetinin adresini çözümlemek için DNS kullanır.

## <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama 
Bu görevi tamamlamak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. Şu [yönergeleri](service-fabric-mesh-howto-setup-cli.md) izleyerek Azure Service Fabric Mesh CLI uzantısı modülünü yükleyin.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
Azure'da oturum açın ve aboneliğinizi ayarlayın.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma
Uygulamanın dağıtılacağı kaynak grubunu oluşturun. Mevcut bir kaynak grubunu kullanabilir ve bu adımı atlayabilirsiniz. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application"></a>Uygulamayı dağıtma
Uygulamanızı kullanarak kaynak grubunu oluşturma `deployment create` komutu.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.windows.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
```

Önceki komutu kullanarak bir Windows uygulaması dağıtır [mesh_rp.windows.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.windows.json). Bir Linux uygulaması dağıtmak istiyorsanız, kullanın [mesh_rp.linux.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.linux.json). Windows kapsayıcı görüntüleri Linux kapsayıcı görüntülerinden büyüktür ve dağıtılması daha uzun sürebilir.

Birkaç dakika içinde komutunuz ile döndürülmesi gerekir:

`VotingApp has been deployed successfully on VotingAppNetwork with public ip address <IP address>` 

## <a name="open-the-application"></a>Uygulamayı açma
Uygulama başarıyla dağıttıktan sonra hizmet uç noktası için genel IP adresini alın ve bir tarayıcıda açın. Bunu, web sayfası görüntülenir. 

![Oylama uygulaması](./media/service-fabric-mesh-howto-deploy-app-template/VotingApplication.png)

Şimdi uygulamayı oylama seçeneklerini ekleyin ve üzerinde oy verin veya oylama seçeneklerini silin.

Dağıtım komutu, hizmet uç noktası genel IP adresini döndürür. İsteğe bağlı olarak, hizmet uç noktası genel IP adresini bulmak için ağ kaynağı sorgulayabilirsiniz. 

Bu uygulama için ağ kaynak adı `VotingAppNetwork`, aşağıdaki komutu kullanarak hakkında bilgi getirir. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name VotingAppNetwork
```

## <a name="check-the-application-details"></a>Uygulama ayrıntılarını denetleme
`app show` komutunu kullanarak uygulamanın durumunu denetleyebilirsiniz. Dağıtılmış uygulama için uygulama adı "VotingApp", bu nedenle ayrıntılarını getirir. 

```azurecli-interactive
az mesh app show --resource-group myResourceGroup --name VotingApp
```

## <a name="list-the-deployed-applications"></a>Dağıtılan uygulamalar listesi
Aboneliğinize dağıttığınız uygulamaların listesini almak için "app list" komutunu kullanabilirsiniz. 

```azurecli-interactive
az mesh app list -o table
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Ne zaman uygulama artık ihtiyacınız ve onu ilgili kaynakları, bunları içeren kaynak grubunu silin. 

```azurecli-interactive
az group delete --resource-group myResourceGroup  
``` 

## <a name="next-steps"></a>Sonraki adımlar
- Voting örnek uygulamasını görünümünde [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md).


