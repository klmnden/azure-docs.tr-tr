---
title: Service Fabric Mesh uygulama kapsayıcısında içindeki Azure tabanlı dosyaları birimin bağlayarak durumu Store | Microsoft Docs
description: Azure dosyaları temel birim kapsayıcısında Azure CLI kullanarak Service Fabric Mesh uygulaması içinde bağlayarak durumu depolamayı öğrenin.
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
ms.date: 06/26/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 94a4e17e6285893520a2f6482b32a69b1229e2fa
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090641"
---
# <a name="store-state-by-mounting-azure-files-based-volume-in-service-fabric-mesh-application"></a>Azure dosyaları bağlayarak Store durumunu Service Fabric Mesh uygulaması biriminde tabanlı

Bu makalede, bir Service Fabric Mesh uygulaması kapsayıcı içindeki bir birimin bağlayarak Azure dosyalarında durumunu depolamak gösterilmektedir. Bu örnekte, bir sayaç değeri bir tarayıcıda gösteren bir web sayfası ASP.NET Core hizmetiyle sayacı uygulama vardır. 

`counterService` Perodically sayaç değeri bir dosyadan okur ve dosyayı geri yazma artırır. Dosya, Azure dosya paylaşımı tarafından desteklenen birim üzerinde oluşturulmuş bir klasörde depolanır. 

## <a name="set-up-service-fabric-mesh-cli"></a>Mesh Service Fabric CLI Kurulumu 
Bu görevi tamamlamak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. İzleyerek Azure Service Fabric CLI'sını Mesh uzantısı modülünü yükleme [yönergeleri](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
Azure'da oturum açın ve aboneliğinizi ayarlayın.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-file-share"></a>Dosya paylaşımı oluşturma 
İzleyerek bir Azure dosya paylaşımı oluşturma [yönergeleri](/azure/storage/files/storage-how-to-create-file-share). Depolama hesabı adı, depolama hesabı anahtarını ve dosya paylaşımı adı olarak başvurulan `<storageAccountName>`, `<storageAccountKey>`, ve `<fileShareName>` aşağıdaki yönergeleri.

## <a name="create-resource-group"></a>Kaynak grubu oluşturma
Uygulamayı dağıtmak için bir kaynak grubu oluşturun. Mevcut bir kaynak grubunu kullanın ve bu adımı atlayın. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Uygulama ve aşağıdaki komutu kullanarak ilgili kaynakları oluşturmak ve değerlerini sağlamanız `storageAccountName`, `storageAccountKey` ve `fileShareName` önceki adımdan.

`storageAccountKey` Şablondaki parametresi bir `securestring`. Dağıtım durumunu görüntülenmez ve `az mesh service show` komutları. Aşağıdaki komutta doğru belirtildiğinden emin olun.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json  --parameters "{\"location\": {\"value\": \"eastus\"}, \"fileShareName\": {\"value\": \"<fileShareName>\"}, \"storageAccountName\": {\"value\": \"<storageAccountName>\"}, \"storageAccountKey\": {\"value\": \"<storageAccountKey>\"}}"
```

Önceki komutu kullanarak bir Linux uygulama dağıtır [mesh_rp.linux.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json). Bir Windows uygulamasını dağıtmak istiyorsanız, kullanın [mesh_rp.windows.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.windows.json). Windows kapsayıcı görüntüleri Linux kapsayıcı görüntülerini daha büyüktür ve dağıtmak için daha fazla zaman alabilir.

Birkaç dakika içinde komutunuz ile döndürülmesi gerekir:

`counterApp has been deployed successfully on counterAppNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Uygulamayı açın
Uygulama başarıyla dağıttıktan sonra hizmet uç noktası için genel IP adresini alın ve bir tarayıcıda açın. Bu, saniyede güncelleştirilmesini sayaç değeri ile bir web sayfası görüntülenir.

Dağıtım komutu, hizmet uç noktası genel IP adresini döndürür. İsteğe bağlı olarak, hizmet uç noktası genel IP adresini bulmak için ağ kaynağı sorgulayabilirsiniz. 
 
Bu uygulama için ağ kaynak adı `counterAppNetwork`, aşağıdaki komutu kullanarak hakkında bilgi getirir. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name counterAppNetwork
```

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Uygulama birim kullanmanız mümkün olduğundan emin olun
Uygulama adında bir dosya oluşturur `counter.txt` içinde dosya paylaşımı içinde `counter/counterService` klasör. Bu dosyanın içeriği web sayfasında görüntülenmesini sayacı değerdir.

Azure dosyaları dosya paylaşımına taramayı etkinleştirir herhangi bir aracı kullanarak dosya yüklenebilir. [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) böyle bir araç örneğidir.

## <a name="delete-the-resources"></a>Kaynakları Sil

Önizleme programı için atanan sınırlı kaynak tasarrufu yapmak için sık kaynakları silin. Bu örnek için ilgili kaynakları silmek için Dağıtılmış kaynak grubunu silin.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure dosyaları toplu örnek uygulamayı görünümünde [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Service Fabric kaynak modeli hakkında daha fazla bilgi için bkz: [Service Fabric Mesh kaynak modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md).
