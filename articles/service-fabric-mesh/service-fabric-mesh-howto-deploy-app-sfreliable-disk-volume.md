---
title: Azure Service Fabric Mesh uygulamada yüksek oranda kullanılabilir Service Fabric güvenilir Disk birimi kullanın | Microsoft Docs
description: Azure CLI kullanarak kapsayıcı içinde Service Fabric güvenilir temel alan Disk birimi bağlayarak durumu bir Azure Service Fabric Mesh uygulamada depolamayı öğrenin.
services: service-fabric-mesh
documentationcenter: .net
author: ashishnegi
manager: raunakpandya
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/03/2018
ms.author: asnegi
ms.custom: mvc, devcenter
ms.openlocfilehash: 9f760e7e693334475fb61ba9e5d44df019e78604
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66147488"
---
# <a name="mount-highly-available-service-fabric-reliable-disk-based-volume-in-a-service-fabric-mesh-application"></a>Yüksek oranda kullanılabilir bir Service Fabric Mesh uygulaması Service Fabric güvenilir temel alan Disk biriminde bağlama 
Azure dosya depolama gibi uzak depolama veya Azure Cosmos DB gibi veritabanı kapsayıcı uygulamalar ile kalıcı durumunu genel yöntemini kullanmaktır. Bu önemli okuma ve yazma ağ gecikmesi uzak deposuna artmasına neden olur.

Bu makalede, bir Service Fabric Mesh uygulaması kapsayıcı içindeki bir birimin bağlayarak yüksek oranda kullanılabilir Service Fabric güvenilir Disk durumunu depolamak gösterilmektedir.
Service Fabric güvenilir Disk birimleri yerel okuma için yüksek kullanılabilirlik için Service Fabric kümesi içinde yinelenen yazma işlemleri sağlar. Bu ağ çağrıları kaldırır için okuma ve yazma işlemleri için ağ gecikmesini azaltır. Kapsayıcıyı yeniden başlatır veya başka bir düğüme taşır, yeni bir kapsayıcı örneği eski kümeyle aynı birimde görürsünüz. Bu nedenle verimli ve yüksek oranda kullanılabilir.

Bu örnekte, bir sayaç değeri bir tarayıcıda gösteren bir web sayfası ASP.NET Core hizmetiyle sayacı uygulama vardır.

`counterService` Düzenli aralıklarla bir sayaç değeri bir dosyadan okur ve dosyayı geri yazma artırır. Dosya, Service Fabric güvenilir diskle biriminde takılı bir klasörde depolanır.

## <a name="prerequisites"></a>Önkoşullar

Bu görevi tamamlamak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. Bu makalede Azure CLI kullanmak için emin olun `az --version` en az döndürür `azure-cli (2.0.43)`.  İzleyerek Azure Service Fabric CLI'sını Mesh uzantısı modülü yüklemek (veya güncelleştirmek) [yönergeleri](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure'da oturum açın ve aboneliğinizi ayarlayın.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Uygulamanın dağıtılacağı kaynak grubunu oluşturun. Aşağıdaki komut adlı bir kaynak grubu oluşturur `myResourceGroup` Doğu Amerika Birleşik Devletleri'nde bulunan bir konumda. Kaynak grubu adı aşağıdaki komutu değiştirirseniz, izleyen tüm komutları değiştirmeyi unutmayın.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Aşağıdaki komutu kullanarak bir Linux uygulama dağıtır [counter.sfreliablevolume.linux.json şablon](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/counter/counter.sfreliablevolume.linux.json). Bir Windows uygulaması dağıtmak için [counter.sfreliablevolume.windows.json şablon](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/counter/counter.sfreliablevolume.windows.json). Büyük kapsayıcı görüntülerini dağıtmak için daha uzun süreceğini unutmayın.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/counter/counter.sfreliablevolume.linux.json
```

Ayrıca komut ile dağıtım durumunu görebilirsiniz

```azurecli-interactive
az group deployment show --name counter.sfreliablevolume.linux --resource-group myResourceGroup
```

Kaynak olarak türü olan ağ geçidi kaynağının adını fark `Microsoft.ServiceFabricMesh/gateways`. Bu, uygulamanın genel IP adresi alınırken kullanılır.

## <a name="open-the-application"></a>Uygulamayı açma

Uygulama başarıyla dağıttıktan sonra uygulama için ağ geçidi kaynak IP adresi alın. Yukarıdaki bölüme penceresinde ağ geçidi adı kullanın.
```azurecli-interactive
az mesh gateway show --resource-group myResourceGroup --name counterGateway
```

Çıktı bir özelliği olmalıdır `ipAddress` hizmet uç noktası için genel IP adresi olduğu. Bir tarayıcıda açın. Saniyede güncelleştirilmesini sayaç değeri ile bir web sayfası görüntüler.

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Uygulama birim kullanmanız mümkün olduğundan emin olun

Uygulama adında bir dosya oluşturur `counter.txt` içindeki bir birimin içindeki `counter/counterService` klasör. Bu dosyanın içeriği web sayfasında görüntülenmesini sayacı değerdir.

## <a name="delete-the-resources"></a>Kaynakları Sil

Sık Azure'da artık kullanmadığınız kaynakları silin. Bu örnek için ilgili kaynakları silmek için aşağıdaki komutu kullanarak (kaynak grubuyla ilişkili olan her şeyi birlikte) dağıtmış kaynak grubunu silin:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- Service Fabric güvenilir birim diskini örnek uygulamayı görünümünde [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Service Fabric Kaynak Modeli hakkında daha fazla bilgi için bkz. [Service Fabric Mesh Kaynak Modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh’e genel bakış](service-fabric-mesh-overview.md) makalesini okuyun.
