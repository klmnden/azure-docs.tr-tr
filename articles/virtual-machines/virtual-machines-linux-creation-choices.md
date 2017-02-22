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
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/03/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 42ee74ac250e6594616652157fe85a9088f4021a
ms.openlocfilehash: 23862762fcf0939ce84859fdae0274421c0bb5fe


---
# <a name="different-ways-to-create-a-linux-vm-including-the-azure-cli-20-preview"></a>Azure CLI 2.0 (Önizleme) dahil olmak üzere Linux VM oluşturmanın farklı yolları
Azure’da size uygun araçları ve iş akışlarını kullanarak bir Linux sanal makine (VM) oluşturma esnekliğine sahipsiniz. Bu makalede bu farklılıklar ve Linux sanal makinelerinizi oluşturmaya yönelik örnekler özetlenmektedir.

## <a name="azure-cli"></a>Azure CLI
Azure’da aşağıdaki CLI sürümlerinden birini kullanarak sanal makineler oluşturabilirsiniz:

- [Azure CLI 1.0](virtual-machines-linux-creation-choices-nodejs.md): Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız
- Azure CLI 2.0 (Önizleme): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI’mız (bu makale)

[Azure CLI 2.0 (Önizleme)](/cli/azure/install-az-cli2) bir npm paketi, distro ile sağlanan paketler veya Docker kapsayıcısı üzerinden çeşitli platformlarda kullanılabilir. Ortamınız için en uygun derlemeyi yükleyin ve [az login](/cli/azure/#login) komutunu kullanarak Azure’da oturum açın

Aşağıdaki örnekler Azure CLI 2.0 (Önizleme) sürümünü kullanır. Gösterilen komutlara ilişkin daha fazla ayrıntı için her bir makaleyi okuyun. Ayrıca [Azure CLI 1.0](virtual-machines-linux-creation-choices-nodejs.md) kullanarak Linux oluşturma seçeneklerine ilişkin örnekleri bulabilirsiniz.

* [Azure CLI 2.0 (Önizleme) kullanarak Linux VM oluşturma](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Bu örnekte `myResourceGroup` adlı bir kaynak grubu oluşturmak için [az group create](/cli/azure/group#create) komutu kullanılmaktadır: 
    
    ```azurecli
    az group create --name myResourceGroup --location westus
    ```
    
  * Bu örnekte Azure Yönetilen Diskler ve `id_rsa.pub` adlı ortak anahtarla en son Debian görüntüsü kullanılarak `myVM` adlı bir VM oluşturmak için [az vm create](/cli/azure/vm#create) komutu kullanılmaktadır:

    ```azurecli
    az vm create \
    --image credativ:Debian:8:latest \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --public-ip-address-dns-name myPublicDNS \
    --resource-group myResourceGroup \
    --location westus \
    --name myVM
    ```

    * Yönetilmeyen diskler kullanmak isterseniz yukarıdaki komuta `--use-unmanaged-disks` bayrağını ekleyin. Sizin için bir depolama hesabı oluşturulur. Daha fazla bilgi için bkz. [Azure Yönetilen Disklere Genel Bakış](../storage/storage-managed-disks-overview.md).

* [Bir Azure şablonu kullanarak güvenli bir Linux VM oluşturma](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Aşağıdaki örnekte GitHub’a depolanmış bir şablon kullanılarak VM oluşturmak için [az group deployment create](/cli/azure/group/deployment#create) komutu kullanılmaktadır:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
    
* [Azure CLI kullanarak eksiksiz bir Linux ortamı oluşturma](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Bir yük dengeleyici ve kullanılabilirlik kümesi içinde birden fazla sanal makine oluşturmayı içerir.

* [Linux VM’ye disk ekleme](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Aşağıdaki örnekte `myVM` adlı var olan sanal makineye `myDataDisk.vhd` adlı 5 GB’lik bir yönetilmeyen disk eklemek için [az vm disk attach-new](/cli/azure/vm/disk#attach-new) komutu kullanılmaktadır:
  
    ```azurecli
    az vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
      --disk-size 5 --vhd https://mystorageaccount.blob.core.windows.net/vhds/myDataDisk.vhd
    ```

## <a name="azure-portal"></a>Azure portalına
[Azure portalı](https://portal.azure.com) sisteminize yüklenecek bir şey olmadığı için sanal makineyi hızlıca oluşturmanıza imkan tanır. VM oluşturmak için Azure portalını kullanma:

* [Azure portalını kullanarak Linux VM oluşturma](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 
* [Azure portalını kullanarak bir diski kullanıma açma](virtual-machines-linux-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="operating-system-and-image-choices"></a>İşletim sistemi ve görüntü seçimi
Bir sanal makine oluşturulurken çalıştırmak istediğiniz işletim sistemini temel alan bir görüntü seçebilirsiniz. Azure ve ortakları, bazıları önyüklü uygulamalar ve araçlarla gelen çok sayıda görüntü sağlar. Veya kendi görüntülerinizden birini karşıya yükleyin ([aşağıdaki bölüme](#use-your-own-image) bakın).

### <a name="azure-images"></a>Azure görüntüleri
Yayımcı, distro sürümü ve derlemelere göre kullanılabilir seçenekleri görmek için [az vm image](/cli/azure/vm/image) komutlarını kullanın.

Kullanılabilir yayımcıların listesi:

```azurecli
az vm image list-publishers --location WestUS
```

Belirli bir yayımcının kullanılabilir ürünlerinin (teklifler) listesi:

```azurecli
az vm image list-offers --publisher-name Canonical --location WestUS
```

Belirli bir teklif için kullanılabilir SKU’ların (distro sürümleri) listesi:

```azurecli
az vm image list-skus --publisher-name Canonical --offer UbuntuServer --location WestUS
```

Belirli bir sürüm için kullanılabilir tüm görüntülerin listesi:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location WestUS
```

Gezinme ve geçerli görüntüleri kullanma ile ilgili daha fazla örnek için bkz. [Azure CLI ile Azure Virtual Machine görüntülerine erişin ve seçin](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

**az vm create** komutu daha yaygın distro’lara ve en son sürümlerine hızlıca erişmek için kullanabileceğiniz bazı diğer adlara sahiptir. Diğer adların kullanılması yayımcı, teklif, SKU ve sürümün bir sanal makine oluşturulan her durumda belirtilmesinden çoğunlukla daha hızlıdır:

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
* [Linux sanal makinesini Resource Manager şablonu olarak yakalama](virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
  
  * Yönetilmeyen diskleri kullanarak mevcut bir sanal makineyi yakalamaya yönelik örnek **az vm** hızlı başlangıç komutları:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm capture --resource-group myResourceGroup --name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [CLI](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile [portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) üzerinden veya [Azure Resource Manager şablonu](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanarak bir Linux VM oluşturun.
* Bir Linux VM oluşturduktan sonra [veri diski ekleyin](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Parola veya SSH anahtarlarını sıfırlama ve kullanıcıları yönetme](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile ilgili hızlı adımlar



<!--HONumber=Feb17_HO2-->


