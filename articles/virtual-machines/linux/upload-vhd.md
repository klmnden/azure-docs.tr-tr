---
title: Karşıya yükleme veya Azure CLI 2.0 ile özel bir Linux VM'yi kopyalama | Microsoft Docs
description: Karşıya yükleme veya Resource Manager dağıtım modelini ve Azure CLI 2.0 kullanarak özelleştirilmiş sanal makine kopyalama
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/06/2017
ms.author: cynthn
ms.openlocfilehash: 3fb6957cf6af5c09a355b61c7c2440a929d1b837
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736688"
---
# <a name="create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a>Azure CLI 2.0 ile bir özel diskten Linux VM oluşturma

<!-- rename to create-vm-specialized -->

Özelleştirilmiş sanal sabit disk (VHD) veya kopya karşıya yükleme Bu makale bir Azure içinde varolan bir VHD'yi ve özel diskten yeni Linux sanal makineleri (VM) oluşturun. Yükleme ve ihtiyaçlarınızı karşılayacak bir Linux distro yapılandırabilir ve ardından hızlı bir şekilde yeni bir Azure sanal makine oluşturmak için bu VHD'yi kullanması.

Özelleştirilmiş diskinizden birden çok VM oluşturmak istiyorsanız, görüntü, sanal makine veya VHD oluşturmanız gerekir. Daha fazla bilgi için [özel bir Azure CLI kullanarak bir VM görüntüsü oluşturmak](tutorial-custom-images.md).

İki seçeneğiniz vardır:
* [Bir VHD’yi karşıya yükleme](#option-1-upload-a-specialized-vhd)
* [Mevcut bir Azure VM'yi kopyalama](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a>Hızlı komutlar

Kullanarak yeni bir VM oluştururken [az vm oluşturma](/cli/azure/vm#az_vm_create) özelleştirilmiş veya özel bir diskten, **ekleme** disk (--ekleme-işletim sistemi diski) bir özel veya Market görüntüsü belirtmek yerine (--görüntü). Aşağıdaki örnekte adlı bir VM oluşturur *myVM* adlı Yönetilen disk kullanarak *myManagedDisk* , özelleştirilmiş bir VHD'den oluşturulan:

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a>Gereksinimler
Aşağıdaki adımları tamamlamak için gerekir:

* Azure'da kullanım için hazırlanmış bir Linux sanal makine. [VM hazırlama](#prepare-the-vm) nasıl Azure Linux VM azure'da düzgün çalışması için ve, SSH kullanarak bağlanabilmesi gerekli olan aracısını (waagent) yükleme distro belirli bilgileri bulmak bu makalenin kapsar.
* Mevcut bir VHD dosyasını [Azure destekli Linux dağıtım](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (veya [bilgi dağıtımlarla için](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) VHD biçiminde bir sanal diske. VM ve VHD oluşturmak için birden çok araçlar mevcuttur:
  * Yükleme ve yapılandırma [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) veya [KVM](http://www.linux-kvm.org/page/RunningKVM), alma, görüntü biçimi olarak VHD kullanmak için dikkat edin. Gerekirse, [görüntüyü dönüştürme](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) kullanarak **qemu img dönüştürme**.
  * Hyper-V ayrıca kullanabileceğiniz [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) veya [Windows Server 2012/2012 R2'deki](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmiyor. Bir VM oluşturduğunuzda, VHD biçiminde belirtin. Gerekirse, VHD kullanarak VHDX diskler dönüştürülebilir [qemu img dönüştürme](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet'i. Ayrıca, Azure gibi disklerin statik Vhd'lere karşıya yüklemeden önce dönüştürmeniz gerekmez dinamik VHD'ler, karşıya yükleme desteklemez. Gibi araçları kullanın [GO için Azure VHD'nin Utilities](https://github.com/Microsoft/azure-vhd-utils-for-go) Azure'a yükleme işlemi sırasında dinamik diskleri dönüştürme.
> 
> 


* En son sahip olduğunuzdan emin olun [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index#az_login).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *mystorageaccount*, ve *mydisks*.

<a id="prepimage"> </a>

## <a name="prepare-the-vm"></a>VM’yi hazırlama

Azure, çeşitli Linux dağıtımları destekler (bkz [desteklenen dağıtımı](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Aşağıdaki makaleler Azure üzerinde desteklenen çeşitli Linux dağıtımları hazırlama size kılavuzluk eder:

* [CentOS tabanlı dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Diğer - dağıtımlarla](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Ayrıca bkz: [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Azure için Linux görüntüleri hazırlama hakkında daha fazla genel ipuçları için.

> [!NOTE]
> [Azure platformunun SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) , yalnızca desteklenen dağıtımlarla birini 'Sürümleri desteklenir' altında belirtildiği gibi yapılandırma ayrıntılarını ile kullanıldığında, Linux çalıştıran Vm'leri uygulandığı [Azure destekli Linux Dağıtımları](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="option-1-upload-a-vhd"></a>Seçenek 1: bir VHD'yi karşıya yükleme

Sahip olduğunuz bir yerel makinede çalışıyor veya başka bir buluttan dışarı aktardığınız özelleştirilmiş bir VHD'yi karşıya yükleyebilirsiniz. Yeni bir Azure VM oluşturmak için VHD kullanmak için bir depolama hesabına VHD yükleyebilirsiniz ve bir VHD'den yönetilen disk oluşturma gerekir. 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Özel diskinizi karşıya yükleme ve sanal makineleri oluşturma önce ilk ile bir kaynak grubu oluşturmak için ihtiyacınız [az grubu oluşturma](/cli/azure/group#az_group_create).

Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* konumu: [Azure yönetilen disklere genel bakış](../windows/managed-disks-overview.md)
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Özel disk ve VM içeren bir depolama hesabı oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#az_storage_account_create). 

Aşağıdaki örnekte adlı bir depolama hesabı oluşturur *mystorageaccount* daha önce oluşturduğunuz kaynak grubunda:

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a>Depolama hesabı anahtarlarını Listele
Azure, her depolama hesabı için iki adet 512 bit erişim tuşu oluşturur. Bu erişim anahtarlarını yazma işlemleri gerçekleştirme gibi depolama hesabı için kimlik doğrulaması yapılırken kullanılır. Daha fazla bilgi edinin [burada Depolama Yönetimi](../../storage/common/storage-account-manage.md#access-keys). Erişim anahtarı görüntüleme [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys#az_storage_account_keys_list).

Oluşturduğunuz depolama hesabının erişim anahtarlarını görüntüleyin:

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

Çıktı şuna benzer olacaktır:

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
Not **key1** sonraki adımlarda depolama hesabınızla etkileşim kurmak için kullanacağınız.

### <a name="create-a-storage-container"></a>Bir depolama kapsayıcısı oluşturma
Yerel dosya sisteminize mantıksal olarak düzenlemek için farklı bir dizin oluşturma yolla disklerinizi düzenlemek için bir depolama hesabında bir kapsayıcı oluşturun. Bir depolama hesabı herhangi bir sayıda kapsayıcı içerebilir. Bir kapsayıcı ile [az depolama kapsayıcısı oluşturma](/cli/azure/storage/container#az_storage_container_create).

Aşağıdaki örnekte adlı bir kapsayıcı oluşturur *mydisks*:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-the-vhd"></a>VHD'yi karşıya yükleme
Şimdi, özel bir disk ile karşıya [az storage blob upload](/cli/azure/storage/blob#az_storage_blob_upload). Size karşıya yükleme ve sayfa blobu olarak özel diskinizin depolama.

Erişim anahtarınızı, önceki adımda ve özel disk yolu yerel bilgisayarınızda oluşturduğunuz kapsayıcıya belirtin:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
VHD'yi karşıya biraz sürebilir.

### <a name="create-a-managed-disk"></a>Yönetilen disk oluşturma


VHD kullanarak yönetilen disk oluşturma [az disk oluşturma](/cli/azure/disk#az_disk_create). Aşağıdaki örnekte adlı bir yönetilen disk oluşturur *myManagedDisk* , adlandırılmış bir depolama hesabı ve kapsayıcı yüklediğiniz VHD'den:

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a>Seçenek 2: var olan bir VM'yi kopyalama

Ayrıca özelleştirilmiş sanal makine oluşturacak ve ardından işletim sistemi diski kopyalayın ve başka bir kopya oluşturmak için yeni bir VM ekleyin. Bu test etmek için uygundur, ancak mevcut bir Azure VM'yi model olarak birden çok yeni VM'ler için kullanmak istiyorsanız, gerçekten oluşturmalısınız bir **görüntü** yerine. Mevcut bir Azure VM'den görüntü oluşturma hakkında daha fazla bilgi için bkz. [özel bir Azure CLI kullanarak bir VM görüntüsü oluşturma](tutorial-custom-images.md)

### <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

Bu örnek adlı bir sanal makine anlık görüntüsünü oluşturur *myVM* kaynak grubundaki *myResourceGroup* ve adlı bir anlık görüntüsünü oluşturur *osDiskSnapshot*.

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-the-managed-disk"></a>Yönetilen disk oluşturma

Yeni bir yönetilen disk anlık görüntüden oluşturun.

Anlık görüntünün Kimliğini alın. Bu örnekte, anlık görüntü adlı *osDiskSnapshot* ve durumda *myResourceGroup* kaynak grubu.

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

Yönetilen disk oluşturun. Bu örnekte, adlı bir yönetilen disk oluşturacağız *myManagedDisk* bizim anlık görüntüden 128 GB boyutundaki standart depolama alanında olmasıdır.

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a>Sanal makine oluşturma

Şimdi ile VM oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create) ve ekleme (--ekleme-os-disk) yönetilen diski işletim sistemi diski olarak. Aşağıdaki örnekte adlı bir VM oluşturur *myNewVM* oluşturulan, karşıya yüklenen bir VHD'den yönetilen disk kullanarak:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

SSH kimlik bilgilerini kullanarak kaynak sanal makineden sanal makine içine mümkün olması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
Hazır ve özel sanal diskinizin karşıya sonra daha fazla bilgi edinebilirsiniz [Resource Manager şablonları ile](../../azure-resource-manager/resource-group-overview.md). Ayrıca isteyebilirsiniz [veri diski ekleme](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) yeni vm'lere. Erişmesi gereken sanal makinelerinizde çalışan uygulamalarınız varsa, mutlaka [açık bağlantı noktalarını ve uç noktaları](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

