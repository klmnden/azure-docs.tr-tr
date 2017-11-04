---
title: "Azure CLI 1.0 ile bir Azure şablonu kullanarak bir Linux VM oluşturma | Microsoft Docs"
description: "Azure CLI 1.0 ve Azure Resource Manager şablonu kullanarak Azure'da bir Linux VM oluşturma."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: v-livech
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 33d4aaa78fcdf3bd9e2e236606f2d3049f464a8a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-linux-vm-using-the-azure-cli-10-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu Azure CLI 1.0 kullanarak bir Linux VM oluşturma
Bu makalede hızlı bir şekilde Azure CLI 1.0 ve Azure Resource Manager şablonu kullanarak bir Linux sanal makine dağıtma gösterilir. Bu makale için şunlar gereklidir:

* bir Azure hesabı ([ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/)).
* [Azure CLI 1.0](../../cli-install-nodejs.md) oturum açarken `azure login`.
* Azure CLI'si, `azure config mode arm` Azure Resource Manager modunda *olmalıdır*.

[Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)’ı kullanarak da hızlı bir şekilde Linux VM şablonu dağıtabilirsiniz.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#quick-command-summary) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](create-ssh-secured-vm-from-template.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız

## <a name="quick-command-summary"></a>Hızlı Komut Özeti
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Ayrıntılı Kılavuz
Şablonlar, kullanıcı adları ve ana bilgisayar adları gibi çalıştırma sırasında özelleştirmek istediğiniz ayarlarla Azure'da VM'ler oluşturmanızı sağlar. Bu makalede; SSH'ye açık, 22 numaralı bağlantı noktasına sahip bir ağ güvenlik grubu (NSG) ile birlikte Ubuntu VM'yi kullanarak bir Azure şablonu başlatıyoruz.

Azure Resource Manager şablonları, bir defalık basit görevler (örneğin, bu makalede yapıldığı gibi bir Ubuntu VM'nin başlatılması) için kullanılabilen JSON dosyalarıdır.  Azure Şablonları; test, geliştirme veya üretim dağıtım yığını gibi tüm ortamların karmaşık Azure yapılandırmalarını oluşturmak için de kullanılabilir.

## <a name="create-the-linux-vm"></a>Linux VM’i oluşturma
Aşağıdaki kod örneği [bu Azure Resource Manager şablonunu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) kullanarak bir kaynak grubu oluşturmak ve aynı zamanda bir SSH güvenlikli Linux VM dağıtmak için `azure group create` çağrısının nasıl gerçekleştireceğini açıklar. Örneğinizde ortamınıza özel adları kullanmanız gerektiğini unutmayın. Bu örnekte *myResourceGroup* kaynak grubu adı olarak ve *myVM* VM adı.

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Çıktı aşağıdaki çıktı bloğu gibi görünmelidir:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Bu örnekte, `--template-uri` parametresi kullanılarak bir VM dağıtıldı.  Ayrıca şablon dosyasının yolu ile birlikte `--template-file` parametresini bağımsız değişken şeklinde kullanarak bir şablonu indirebilir, yerel olarak oluşturabilir ve şablonun geçişini sağlayabilirsiniz. Azure CLI sizden şablon için gerekli parametreleri isteyecektir.

## <a name="next-steps"></a>Sonraki adımlar
Daha sonra dağıtılacak uygulama altyapılarını keşfetmek için [şablon galerisi](https://azure.microsoft.com/documentation/templates/)nde arama yapın.

