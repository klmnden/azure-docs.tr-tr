---
title: "Azure’da Linux VM oluşturmanın farklı yolları | Microsoft Azure"
description: "Azure üzerinde bir Linux sanal makine oluşturmanın farklı yollarını her bir yöntemin araç ve öğreticilerinin bağlantılarıyla birlikte listeler."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: b2f93579eb1c7a69d0dbd1b0ef112aed9b2168c3
ms.contentlocale: tr-tr
ms.lasthandoff: 05/31/2017


---
# <a name="different-ways-to-create-a-linux-vm"></a>Linux VM oluşturmak için farklı yöntemler
Azure’da size uygun araçları ve iş akışlarını kullanarak bir Linux sanal makine (VM) oluşturma esnekliğine sahipsiniz. Bu makalede bu farklılıklar ve Azure CLI 2.0 dahil olmak üzere Linux VM'lerinizi oluşturmaya yönelik örnekler özetlenmektedir. [Azure CLI 1.0](creation-choices-nodejs.md) dahil oluşturma seçeneklerini de görüntüleyebilirsiniz.

[Azure CLI 2.0](/cli/azure/install-az-cli2) bir npm paketi, distro ile sağlanan paketler veya Docker kapsayıcısı üzerinden çeşitli platformlarda kullanılabilir. Ortamınız için en uygun derlemeyi yükleyin ve [az login](/cli/azure/#login) komutunu kullanarak Azure’da oturum açın

* [Azure CLI 2.0 ile Linux VM oluşturma](quick-create-cli.md)
  
  * [az group create](/cli/azure/group#create) ile *myResourceGroup* adlı bir kaynak grubu oluşturun: 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * [az vm create](/cli/azure/vm#create) komutu ile en son *UbuntuLTS* görüntüsünü kullanan *myVM* adlı bir VM oluşturun ve *~/.ssh* içinde henüz yoksa SSH anahtarları oluşturun:

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [Azure şablonu ile Linux VM oluşturma](create-ssh-secured-vm-from-template.md)
  
  * Aşağıdaki örnekte, GitHub’da depolanmış bir şablondan VM oluşturmak için [az group deployment create](/cli/azure/group/deployment#create) komutu kullanılmaktadır:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [Linux VM oluşturma ve cloud-init ile özelleştirme](tutorial-automate-vm-deployment.md)

* [Birden fazla Linux VM üzerinde yük dengeli ve yüksek oranda kullanılabilir bir uygulama oluşturma](tutorial-load-balancer.md)


## <a name="azure-portal"></a>Azure portalına
[Azure portalı](https://portal.azure.com) sisteminize yüklenecek bir şey olmadığı için sanal makineyi hızlıca oluşturmanıza imkan tanır. VM oluşturmak için Azure portalını kullanma:

* [Azure portalını kullanarak Linux VM oluşturma](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a>İşletim sistemi ve görüntü seçimi
Bir sanal makine oluşturulurken çalıştırmak istediğiniz işletim sistemini temel alan bir görüntü seçebilirsiniz. Azure ve ortakları, bazıları önyüklü uygulamalar ve araçlarla gelen çok sayıda görüntü sağlar. Veya kendi görüntülerinizden birini karşıya yükleyin ([aşağıdaki bölüme](#use-your-own-image) bakın).

### <a name="azure-images"></a>Azure görüntüleri
Yayımcı, distro sürümü ve derlemelere göre kullanılabilir seçenekleri görmek için [az vm image](/cli/azure/vm/image) komutlarını kullanın.

Kullanılabilir yayımcıların listesi:

```azurecli
az vm image list-publishers --location eastus
```

Belirli bir yayımcının kullanılabilir ürünlerinin (teklifler) listesi:

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

Belirli bir teklif için kullanılabilir SKU’ların (distro sürümleri) listesi:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

Belirli bir sürüm için kullanılabilir tüm görüntülerin listesi:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

Gezinme ve geçerli görüntüleri kullanma ile ilgili daha fazla örnek için bkz. [Azure CLI ile Azure Virtual Machine görüntülerine erişin ve seçin](cli-ps-findimage.md).

[az vm create](/cli/azure/vm#create) komutu daha yaygın distro’lara ve en son sürümlerine hızlıca erişmek için kullanabileceğiniz bazı diğer adlara sahiptir. Diğer adların kullanılması yayımcı, teklif, SKU ve sürümün bir sanal makine oluşturulan her durumda belirtilmesinden çoğunlukla daha hızlıdır:

| Diğer ad | Yayımcı | Sunduğu | SKU | Sürüm |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |en son |
| CoreOS |CoreOS |CoreOS |Dengeli |en son |
| Debian |credativ |Debian |8 |en son |
| openSUSE |SUSE |openSUSE |13.2 |en son |
| RHEL |RedHat |RHEL |7.2 |en son |
| SLES |SLES |SLES |12 SP1 |en son |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |en son |

### <a name="use-your-own-image"></a>Kendi görüntünüzü kullanma
Belirli özelleştirmelere ihtiyaç duyuyorsanız, mevcut bir Azure VM’yi yakalayarak bu VM’yi temel alan bir görüntü kullanabilirsiniz. Şirket içinde oluşturduğunuz bir görüntüyü de yükleyebilirsiniz. Desteklenen distro’lar ve kendi görüntünüzü kullanma ile ilgili daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure destekli dağıtımlar](endorsed-distros.md)
* [Desteklenmeyen dağıtımlarla ilgili bilgiler](create-upload-generic.md)
* [Mevcut bir Azure VM’den görüntü oluşturma](tutorial-custom-images.md).
  
  * Mevcut bir Azure VM’den görüntü oluşturmaya yönelik hızlı başlangıç örnek komutları:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [CLI](quick-create-portal.md) ile, [portal](quick-create-cli.md) üzerinden veya [Azure Resource Manager şablonu](../windows/cli-deploy-templates.md) kullanarak bir Linux VM oluşturun.
* Linux VM oluşturduktan sonra [Azure diskleri ve depolama hakkında bilgi edinin](tutorial-manage-disks.md).
* [Parola veya SSH anahtarlarını sıfırlama ve kullanıcıları yönetme](using-vmaccess-extension.md) ile ilgili hızlı adımlar.

