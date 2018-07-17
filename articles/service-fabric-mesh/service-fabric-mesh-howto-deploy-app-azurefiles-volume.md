---
title: Azure dosyaları toplu kullanan bir Service Fabric Mesh uygulaması dağıtma | Microsoft Docs
description: Service Fabric Mesh Azure CLI kullanarak için Azure dosya birimi kullanan bir uygulamayı dağıtmayı öğrenin.
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
ms.openlocfilehash: fb9740efaa9a1033d6602180f5750f6fb550c048
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076497"
---
# <a name="deploy-a-service-fabric-mesh-application-that-uses-the-azure-files-volume"></a>Azure dosya birimi kullanan Service Fabric Mesh uygulaması dağıtma
Bu örnek, Azure Service Fabric Mesh çalıştıran bir kapsayıcıdaki depolama birimleri kullanımını gösterir. Bu örnek bir parçası olarak:

- Bir dosya paylaşımı ile [Azure dosyaları](/azure/storage/files/storage-files-introduction) 
- Biz dağıtacaksınız bir kapsayıcı örneği için bir birim olarak paylaşma başvurusu
  - Kapsayıcı başlatıldığında, bu paylaşım olarak kapsayıcı içindeki belirli bir konuma bağlar
- Kapsayıcı içinde çalışan kodu bir metin dosyası bu konuma yazar.
- Dosyanın doğru birim yedekleyen paylaşımda yazılır doğrulayın

## <a name="example-json-templates"></a>Örnek JSON şablonları

Linux: [https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.linux.json](https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.linux.json)

Windows: [https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.windows.json](https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.windows.json)

## <a name="create-the-azure-files-file-share"></a>Azure dosyaları dosya paylaşımı oluşturma

Bölümündeki yönergeleri [Azure dosyaları belgeleri](/azure/storage/files/storage-how-to-create-file-share) kullanmak için uygulamayı için dosya paylaşımı oluşturmak için.

## <a name="set-up-service-fabric-mesh-cli"></a>Mesh Service Fabric CLI Kurulumu
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Bu adımları tamamlamak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI Sürüm 2.0.35 yüklemeniz gerekir ya da daha sonra. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI'ın en son sürüme yükseltin veya yüklemek için [Azure CLI 2.0 yükleme] [azure CLI yükleme] bakın.

Azure Service Fabric CLI'sını Mesh uzantısı modülünü yükleyin. Önizleme için Kafes Azure Service Fabric CLI, Azure CLI'yı bir uzantısı olarak yazılır.

```azurecli-interactive
az extension add --source https://sfmeshcli.blob.core.windows.net/cli/mesh-0.8.1-py2.py3-none-any.whl
```

## <a name="log-in-to-azure"></a>Azure'da oturum açma
Azure'da oturum açın ve aboneliğinizi ayarlayın.

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma
(Bu örnekte dağıtmak için RG) bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu kullanın ve bu adımı atlayın. Yalnızca önizleme kullanılabilir `eastus` konumu.

```azurecli-interactive
az group create --name <resourceGroupName> --location eastus
```

## <a name="deploy-the-template"></a>Şablonu dağıtma
Aşağıdaki komutlardan birini kullanarak ilgili kaynakları ve uygulama oluşturun.

Linux için:

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.linux.json
```

Windows için:

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://seabreezequickstart.blob.core.windows.net/templates/azurefiles-volume/sbz_rp.windows.json
```

Dosya paylaşım adı, hesap adı ve hesap anahtarını birim sağlar Azure dosya paylaşımı için girmek için istemleri izleyin. Bir dakika ya da bunu, komutunuz ile döndürmelidir `"provisioningState": "Succeeded"`.

Parola parametresi şablondaki önemlidir `string` türü kullanım kolaylığı. Düz metin ve dağıtım durumu ekranında görüntülenir.

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Uygulama birim kullanmanız mümkün olduğundan emin olun
Uygulama adında bir dosya oluşturur _data.txt_ dosya paylaşımında (Bunu zaten mevcut değilse). Bu dosyanın içeriği uygulama tarafından her 30 saniyede artırılır bir sayıdır. Örnek düzgün çalıştığını doğrulamak için açın _data.txt_ düzenli aralıklarla dosya ve sayı güncelleştirilmiş olduğunu doğrulayın.

Azure dosyaları dosya paylaşımına taramayı etkinleştirir herhangi bir aracı kullanarak dosya yüklenebilir. [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) böyle bir araç örneğidir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure dosyaları toplu örnek uygulamayı görünümünde [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/azurefiles-volume).
- Service Fabric kaynak modeli hakkında daha fazla bilgi için bkz: [Service Fabric Mesh kaynak modeli](service-fabric-mesh-service-fabric-resources.md).
- Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md).
