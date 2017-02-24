---
title: "Azure şablonu kullanarak bir Linux VM oluşturma | Microsoft Belgeleri"
description: "Azure Resource Manager şablonu kullanarak Azure’da bir Linux VM oluşturun."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/24/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: ea1274dd53a93f00fa251ed03684b17b58b009c2


---
# <a name="create-a-linux-vm-using-an-azure-template"></a>Bir Azure şablonu kullanarak bir Linux VM oluşturma
Bu makalede, Azure’da bir Azure Şablonu kullanarak nasıl hızlı bir şekilde Linux Sanal Makine dağıtacağınız gösterilmiştir.  Bu makale için şunlar gereklidir:

* bir Azure hesabı ([ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/)).
* `azure login` ile oturum açılmış [Azure CLI'si](../xplat-cli-install.md).
* Azure CLI'si, `azure config mode arm` Azure Resource Manager modunda *olmalıdır*.

[Azure portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)’ı kullanarak da hızlı bir şekilde Linux VM şablonu dağıtabilirsiniz.

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
Aşağıdaki kod örneği [bu Azure Resource Manager şablonunu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) kullanarak bir kaynak grubu oluşturmak ve aynı zamanda bir SSH güvenlikli Linux VM dağıtmak için `azure group create` çağrısının nasıl gerçekleştireceğini açıklar. Örneğinizde ortamınıza özel adları kullanmanız gerektiğini unutmayın. Bu örnekte, kaynak grubu adı olarak `myResourceGroup`, VM adı olarak ise `myVM` kullanılmıştır.

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




<!--HONumber=Nov16_HO2-->


