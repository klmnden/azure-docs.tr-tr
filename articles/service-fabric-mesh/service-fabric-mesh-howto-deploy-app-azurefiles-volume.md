---
title: Store durumu bir Azure Service Fabric Mesh uygulaması içinde bir kapsayıcı içinde temel Azure dosyaları birim bağlayarak | Microsoft Docs
description: Azure CLI kullanarak kapsayıcı içindeki bir Azure tabanlı dosyaları birimin bağlayarak durumu bir Azure Service Fabric Mesh uygulamada depolamayı öğrenin.
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
ms.date: 08/09/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 3b350deff2883761af6a3a2b3c5c9ef22235bde0
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42055155"
---
# <a name="store-state-in-an-azure-service-fabric-mesh-application-by-mounting-an-azure-files-based-volume-inside-the-container"></a>Azure dosyaları bağlayarak bir Azure Service Fabric Mesh uygulaması Store durumda birim kapsayıcı içinde temel

Bu makalede, bir Service Fabric Mesh uygulaması kapsayıcı içindeki bir birimin bağlayarak Azure dosyalarında durumunu depolamak gösterilmektedir. Bu örnekte, bir sayaç değeri bir tarayıcıda gösteren bir web sayfası ASP.NET Core hizmetiyle sayacı uygulama vardır. 

`counterService` Düzenli aralıklarla bir sayaç değeri bir dosyadan okur ve dosyayı geri yazma artırır. Dosya, Azure dosya paylaşımı tarafından desteklenen birim üzerinde oluşturulmuş bir klasörde depolanır.

## <a name="prerequisites"></a>Önkoşullar

Bu görevi tamamlamak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. Bu makalede Azure CLI kullanmak için emin olun `az --version` en az döndürür `azure-cli (2.0.43)`.  İzleyerek Azure Service Fabric CLI'sını Mesh uzantısı modülü yüklemek (veya güncelleştirmek) [yönergeleri](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure'da oturum açın ve aboneliğinizi ayarlayın.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

İzleyerek bir Azure dosya paylaşımı oluşturma [yönergeleri](/azure/storage/files/storage-how-to-create-file-share). Depolama hesabı adı, depolama hesabı anahtarını ve dosya paylaşımı adı olarak başvurulan `<storageAccountName>`, `<storageAccountKey>`, ve `<fileShareName>` aşağıdaki yönergeleri. Bu değerler, Azure portalında kullanılabilir:
* <storageAccountName> -Altında **depolama hesapları**, dosya paylaşımı oluştururken kullanılan depolama hesabının adıdır.
* <storageAccountKey> -Altında depolama hesabınızı seçin **depolama hesapları** seçip **erişim anahtarları** ve değerin altında **key1**.
* <fileShareName> -Altında depolama hesabınızı seçin **depolama hesapları** seçip **dosyaları**. Kullanılacak adı, dosya paylaşımını oluşturduğunuz adıdır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Uygulamanın dağıtılacağı kaynak grubunu oluşturun. Aşağıdaki komut adlı bir kaynak grubu oluşturur `myResourceGroup` Doğu Amerika Birleşik Devletleri'nde bulunan bir konumda.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Uygulama ve aşağıdaki komutu kullanarak ilgili kaynakları oluşturmak ve değerlerini sağlamanız `storageAccountName`, `storageAccountKey` ve `fileShareName` önceki gelen [dosya paylaşımı oluşturma](#create-a-file-share) adım.

`storageAccountKey` Parametredir şablondaki güvenli bir dize. Dağıtım durumunu görüntülenmez ve `az mesh service show` komutları. Aşağıdaki komutta doğru belirtildiğinden emin olun.

Aşağıdaki komutu kullanarak bir Linux uygulama dağıtır [mesh_rp.linux.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json). Bir Windows uygulaması dağıtmak için [mesh_rp.windows.json şablon](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.windows.json). Büyük kapsayıcı görüntülerini dağıtmak için daha uzun süreceğini unutmayın.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json  --parameters "{\"location\": {\"value\": \"eastus\"}, \"fileShareName\": {\"value\": \"<fileShareName>\"}, \"storageAccountName\": {\"value\": \"<storageAccountName>\"}, \"storageAccountKey\": {\"value\": \"<storageAccountKey>\"}}"
```

Birkaç dakika sonra komut ile döndürmelidir `counterApp has been deployed successfully on counterAppNetwork with public ip address <IP Address>`

## <a name="open-the-application"></a>Uygulamayı açma

Dağıtım komutu, hizmet uç noktası genel IP adresini döndürür. Uygulama başarıyla dağıttıktan sonra hizmet uç noktası için genel IP adresini alın ve bir tarayıcıda açın. Saniyede güncelleştirilmesini sayaç değeri ile bir web sayfası görüntüler.

Bu uygulama için ağ kaynak adı `counterAppNetwork`. Aşağıdaki komutu kullanarak, açıklama, konum, kaynak grubu, vb. gibi uygulama hakkında daha fazla bilgi görebilirsiniz:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name counterAppNetwork
```

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Uygulama birim kullanmanız mümkün olduğundan emin olun

Uygulama adında bir dosya oluşturur `counter.txt` içinde dosya paylaşımı içinde `counter/counterService` klasör. Bu dosyanın içeriği web sayfasında görüntülenmesini sayacı değerdir.

Bir Azure dosyaları dosya paylaşımı gibi taramayı etkinleştirir herhangi bir aracı kullanarak dosyayı karşıdan yüklenebilir [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).

## <a name="delete-the-resources"></a>Kaynakları Sil

Sık Azure'da artık kullanmadığınız kaynakları silin. Bu örnek için ilgili kaynakları silmek için aşağıdaki komutu kullanarak (kaynak grubuyla ilişkili olan her şeyi birlikte) dağıtmış kaynak grubunu silin:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure dosyaları toplu örnek uygulamayı görünümünde [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Service Fabric kaynak modeli hakkında daha fazla bilgi için bkz: [Service Fabric Mesh kaynak modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md).