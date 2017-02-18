---
title: "Linux VM oluşturmanın farklı yolları | Microsoft Belgeleri"
description: "Azure üzerinde bir Linux sanal makine oluşturmanın farklı yollarını her bir yöntemin araç ve öğreticilerinin bağlantılarıyla birlikte listeler."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 1cbc3534aa0415a14d8b955dbcd291d2200c276e
ms.openlocfilehash: 2ffc8d1f8ec924914c624f882bf35a08f0dd5888


---
# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Azure’da Linux sanal makine oluşturmanın farklı yolları
Azure’da size uygun araçları ve iş akışlarını kullanarak bir Linux sanal makine (VM) oluşturma esnekliğine sahipsiniz. Bu makalede bu farklılıklar ve Linux sanal makinelerinizi oluşturmaya yönelik örnekler özetlenmektedir.

## <a name="azure-cli"></a>Azure CLI
Azure’da aşağıdaki CLI sürümlerinden birini kullanarak sanal makineler oluşturabilirsiniz:

- Azure CLI 1.0: Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız (bu makale)
- [Azure CLI 2.0 (Önizleme)](virtual-machines-linux-creation-choices.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI’mız

Azure CLI 1.0 bir npm paketi, distro ile sağlanan paketler veya Docker kapsayıcısı üzerinden çeşitli platformlarda kullanılabilir. [Azure CLI yükleme ve yapılandırma](../xplat-cli-install.md) hakkında daha fazla bilgi alabilirsiniz. Aşağıdaki öğretiler Azure CLI 1.0 kullanma ile ilgili örnekler sunar. Gösterilen CLI hızlı başlatma komutlarına ilişkin daha fazla ayrıntı için her bir makaleyi okuyun:

* [Geliştirme ve test için Azure CLI üzerinden bir Linux VM oluşturma](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Aşağıdaki örnekte `azure_id_rsa.pub` adlı bir ortak anahtar kullanılarak CoreOS VM oluşturulmaktadır:
    
    ```azurecli
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
      --image-urn CoreOS
    ```
* [Bir Azure şablonu kullanarak güvenli bir Linux VM oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Aşağıdaki örnekte GitHub’a depolanmış bir şablon kullanılarak bir VM oluşturulmuştur:
    
    ```azurecli
    azure group create --name myResourceGroup --location WestUS 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```
* [Azure CLI kullanarak eksiksiz bir Linux ortamı oluşturma](virtual-machines-linux-create-cli-complete-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Bir yük dengeleyici ve kullanılabilirlik kümesi içinde birden fazla sanal makine oluşturmayı içerir.
* [Linux VM’ye disk ekleme](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Aşağıdaki örnek `TestVM` adlı var olan bir sanal makineye 5 Gb disk ekler:
    
    ```azurecli
    azure vm disk attach-new --resource-group myResourceGroup  --vm-name myVM \
      --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure portal
[Azure portalı](https://portal.azure.com) sisteminize yüklenecek bir şey olmadığı için sanal makineyi hızlıca oluşturmanıza imkan tanır. VM oluşturmak için Azure portalını kullanma:

* [Azure portalını kullanarak Linux VM oluşturma](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 
* [Azure portalını kullanarak bir diski kullanıma açma](virtual-machines-linux-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="operating-system-and-image-choices"></a>İşletim sistemi ve görüntü seçimi
Bir sanal makine oluşturulurken çalıştırmak istediğiniz işletim sistemini temel alan bir görüntü seçebilirsiniz. Azure ve ortakları, bazıları önyüklü uygulamalar ve araçlarla gelen çok sayıda görüntü sağlar. Veya kendi görüntülerinizden birini karşıya yükleyin ([aşağıdaki bölüme](#use-your-own-image) bakın).

### <a name="azure-images"></a>Azure görüntüleri
Yayım, distro sürümü ve derlemelere göre kullanılabilen seçenekleri görmek için `azure vm image` CLI komutlarını kullanın.

Kullanılabilir yayımcıları aşağıdaki gibi listeleyin:

```azurecli
azure vm image list-publishers --location WestUS
```

Belirli bir yayımcının kullanılabilir ürünlerini (teklifler) aşağıdaki gibi listeleyin:

```azurecli
azure vm image list-offers --location WestUS --publisher Canonical
```

Belirli bir teklif için kullanılabilir SKU’ları (distro sürümleri) aşağıdaki gibi listeleyin:

```azurecli
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Belirli bir sürüm için kullanılabilir tüm görüntüleri aşağıdaki gibi listeleyin:

```azurecli
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Gezinme ve geçerli görüntüleri kullanma ile ilgili daha fazla örnek için bkz. [Azure CLI ile Azure Virtual Machine görüntülerine erişin ve seçin](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

`azure vm quick-create` ve `azure vm create` komutları daha yaygın distro’lara ve en son sürümlerine hızlıca erişmek için kullanabileceğiniz bazı diğer adlara sahiptir. Diğer adların kullanılması yayımcı, teklif, SKU ve sürümün bir sanal makine oluşturulan her durumda belirtilmesinden çoğunlukla daha hızlıdır:

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
Belirli özelleştirmelere ihtiyaç duyuyorsanız, mevcut bir Azure VM’i temel alan bir görüntü kullanabilirsiniz, bunun için söz konusu VM’i *yakalayabilirsiniz*. Şirket içinde oluşturduğunuz bir görüntüyü de yükleyebilirsiniz. Desteklenen distro’lar ve kendi görüntünüzü kullanma ile ilgili daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure destekli dağıtımlar](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Desteklenmeyen dağıtımlarla ilgili bilgiler](virtual-machines-linux-create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Özel disk görüntüsünden Linux VM yükleme ve oluşturma](virtual-machines-linux-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Linux sanal makinesini Resource Manager şablonu olarak yakalama](virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
  
  * Var olan bir sanal makineyi yakalamaya yönelik hızlı başlangıç örnek komutları:
    
    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --vm-name myVM
    azure vm generalize --resource-group myResourceGroup --vm-name myVM
    azure vm capture --resource-group myResourceGroup --vm-name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [CLI](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile [portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) üzerinden veya [Azure Resource Manager şablonu](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanarak bir Linux VM oluşturun.
* Bir Linux VM oluşturduktan sonra [veri diski ekleyin](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Parola veya SSH anahtarlarını sıfırlama ve kullanıcıları yönetme](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile ilgili hızlı adımlar




<!--HONumber=Jan17_HO3-->


