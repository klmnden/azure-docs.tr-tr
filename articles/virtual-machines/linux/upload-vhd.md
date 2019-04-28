---
title: Karşıya yükleme veya Azure CLI ile özel bir Linux VM'yi kopyalama | Microsoft Docs
description: Karşıya yükleme veya özelleştirilmiş bir sanal makine Resource Manager dağıtım modelini ve Azure CLI kullanarak kopyalama
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
ms.date: 10/17/2018
ms.author: cynthn
ms.openlocfilehash: 6483fa8737ee3de6a60c4e4646fefec30ae702b6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61473425"
---
# <a name="create-a-linux-vm-from-a-custom-disk-with-the-azure-cli"></a>Azure CLI ile bir özel diskten Linux VM oluşturma

<!-- rename to create-vm-specialized -->

Bu makalede, özelleştirilmiş bir sanal sabit disk (VHD) karşıya yükleme ve varolan bir VHD'yi Azure'da kopyalamak nasıl gösterir. Yeni oluşturulan VHD'yi sonra yeni Linux sanal makineleri (VM'ler) oluşturmak için kullanılır. Yükleme ve ihtiyaçlarınızı karşılayacak bir Linux distro yapılandırabilir ve ardından yeni bir Azure sanal makine oluşturmak için bu VHD'yi kullanması.

Özelleştirilmiş diskinizden birden çok VM oluşturmak için öncelikle görüntüyü VM ya da VHD oluşturun. Daha fazla bilgi için [CLI kullanarak Azure VM'deki özel görüntüsünü oluşturma](tutorial-custom-images.md).

Özel bir disk oluşturmak için iki seçeneğiniz vardır:
* VHD’yi karşıya yükleme
* Mevcut bir Azure VM'yi kopyalama

## <a name="quick-commands"></a>Hızlı komutlar

İle yeni bir VM oluştururken [az vm oluşturma](/cli/azure/vm#az-vm-create) özelleştirilmiş veya özel bir diskten, **ekleme** disk (--ekleme-işletim sistemi diski) bir özel veya Market görüntüsü belirtme yerine (--görüntü). Aşağıdaki örnekte adlı bir VM oluşturur *myVM* adlı Yönetilen disk kullanarak *myManagedDisk* , özelleştirilmiş bir VHD'den oluşturulan:

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a>Gereksinimler
Aşağıdaki adımların tamamlanması gerekir:

* Azure'da kullanım için hazırlanmış bir Linux sanal makine. [VM hazırlama](#prepare-the-vm) bu makalenin Azure Linux, SSH ile sanal makineye bağlanmak gerekli olan aracısını (waagent), yükleme distro özgü bilgileri nasıl bulacağınız konusunda ele alınmaktadır.
* Mevcut bir VHD dosyasını [Azure destekli Linux dağıtım](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (veya [bilgi dağıtımlarla için](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) VHD biçiminde bir sanal diske. VM ve VHD oluşturmak için birden çok araçlar mevcuttur:
  * Yükleme ve yapılandırma [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) veya [KVM](https://www.linux-kvm.org/page/RunningKVM), alma, görüntü biçimi olarak VHD kullanmak için dikkat edin. Gerekirse, [görüntüyü dönüştürme](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) ile `qemu-img convert`.
  * Hyper-V ayrıca kullanabileceğiniz [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) veya [Windows Server 2012/2012 R2'deki](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmiyor. Bir VM oluşturduğunuzda, VHD biçiminde belirtin. Gerekirse, VHDX disk VHD ile dönüştürebilirsiniz [qemu img dönüştürme](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet'i. Azure, bu nedenle karşıya yüklemeden önce statik VHD'ler gibi disklerin dönüştürmeniz gerekir dinamik VHD'ler, karşıya yükleme desteklemez. Gibi araçları kullanın [GO için Azure VHD'nin Utilities](https://github.com/Microsoft/azure-vhd-utils-for-go) bunları Azure'a yükleme işlemi sırasında dinamik diskleri dönüştürme.
> 
> 


* En son sahip olduğunuzdan emin olun [Azure CLI](/cli/azure/install-az-cli2) yüklü ve ile bir Azure hesabında oturum [az login](/cli/azure/reference-index#az-login).

Kendi değerlerinizle örnek parametre adları aşağıdaki örneklerde, aşağıdaki gibi değiştirin *myResourceGroup*, *mystorageaccount*, ve *mydisks*.

<a id="prepimage"> </a>

## <a name="prepare-the-vm"></a>VM’yi hazırlama

Azure, çeşitli Linux dağıtımları destekler (bkz [desteklenen dağıtımı](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Aşağıdaki makaleler, Azure üzerinde desteklenen çeşitli Linux dağıtımları hazırlayın açıklar:

* [CentOS Tabanlı Dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES ve openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Diğerleri: Desteklenmeyen Dağıtımlar](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Ayrıca bkz: [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) Azure için Linux görüntüleri hazırlama hakkında daha fazla genel ipuçları için.

> [!NOTE]
> [Azure platformunun SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) , yalnızca desteklenen dağıtımlarla birini "Desteklenen sürümleri" altında belirtildiği gibi yapılandırma ayrıntılarını ile kullanıldığında, Linux çalıştıran Vm'leri uygulandığı [Azure destekli Linux Dağıtımları](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="option-1-upload-a-vhd"></a>1. seçenek: VHD’yi karşıya yükleme

Sahip olduğunuz bir yerel makinede çalışıyor veya başka bir buluttan dışarı aktardığınız özelleştirilmiş bir VHD'yi karşıya yükleyebilirsiniz. Yeni bir Azure VM oluşturmak için bir VHD kullanmak için bir depolama hesabına VHD yükleyebilirsiniz ve bir VHD'den yönetilen disk oluşturma gerekecektir. Daha fazla bilgi için bkz. [Azure Yönetilen Disklere Genel Bakış](../windows/managed-disks-overview.md).

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Özel diskinizi karşıya yükleme ve sanal makineleri oluşturma önce bir kaynak grubu oluşturmanız gerekir [az grubu oluşturma](/cli/azure/group#az-group-create).

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Özel disk ve VM içeren bir depolama hesabı oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account). Aşağıdaki örnekte adlı bir depolama hesabı oluşturur *mystorageaccount* daha önce oluşturduğunuz kaynak grubunda:

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a>Depolama hesabı anahtarlarını Listele
Azure, her depolama hesabı için iki adet 512 bit erişim tuşu oluşturur. Bu erişim anahtarlarını ne zaman işi izin ver yazma işlemlerini gibi depolama hesabı için kimlik doğrulaması yapılırken kullanılır. Daha fazla bilgi için [Depolama Yönetimi](../../storage/common/storage-account-manage.md#access-keys). 

Erişim anahtarı görüntüleme [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys#az-storage-account-keys-list). Örneğin, görüntülemek için depolama erişim anahtarlarını oluşturduğunuz hesap:

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
Yerel dosya sisteminize mantıksal olarak düzenlemek için farklı bir dizin oluşturma yolla disklerinizi düzenlemek için bir depolama hesabında kapsayıcıları oluşturacaksınız. Bir depolama hesabında çok sayıda kapsayıcı olabilir. Bir kapsayıcı ile [az depolama kapsayıcısı oluşturma](/cli/azure/storage/container#az-storage-container-create).

Aşağıdaki örnekte adlı bir kapsayıcı oluşturur *mydisks*:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-the-vhd"></a>VHD'yi karşıya yükleme
Özel diskinizi karşıya [az storage blob upload](/cli/azure/storage/blob#az-storage-blob-upload). Karşıya yükleme ve sayfa blobu olarak özel diskinizin depolama.

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


İle bir VHD'den yönetilen disk oluşturma [az disk oluşturma](/cli/azure/disk#az-disk-create). Aşağıdaki örnekte adlı bir yönetilen disk oluşturur *myManagedDisk* , adlandırılmış bir depolama hesabı ve kapsayıcı yüklediğiniz VHD'den:

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a>2. seçenek: Mevcut bir VM'yi kopyalama

Ayrıca Azure'da özel bir VM oluşturun ve ardından işletim sistemi diski kopyalayın ve başka bir kopya oluşturmak için yeni bir VM ekleyin. Bu ama isterseniz var olan bir Azure VM, birden çok yeni VM'ler için model olarak kullanmak, oluşturmak, test etmek için iyi bir *görüntü* yerine. Mevcut bir Azure VM'den görüntü oluşturma hakkında daha fazla bilgi için bkz. [CLI kullanarak Azure VM'deki özel görüntüsünü oluşturma](tutorial-custom-images.md).

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

Yönetilen disk oluşturun. Bu örnekte, adlı bir yönetilen disk oluşturacağız *myManagedDisk* burada diskin standart depolama alanında, boyutu 128 GB olarak bizim anlık görüntüden.

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a>Sanal makine oluşturma

İle VM oluşturma [az vm oluşturma](/cli/azure/vm#az-vm-create) ve ekleme (--ekleme-os-disk) yönetilen diski işletim sistemi diski olarak. Aşağıdaki örnekte adlı bir VM oluşturur *myNewVM* , oluşturduğunuz, karşıya yüklenen bir VHD'den yönetilen disk kullanarak:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

Sanal makine kimlik bilgileri ile içine SSH için kaynak sanal makineden başlatabilmeniz gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
Hazır ve özel sanal diskinizin karşıya sonra daha fazla bilgi edinebilirsiniz [Resource Manager şablonları ile](../../azure-resource-manager/resource-group-overview.md). Ayrıca isteyebilirsiniz [veri diski ekleme](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) yeni vm'lere. Erişmesi gereken sanal makinelerinizde çalışan uygulamalarınız varsa, mutlaka [açık bağlantı noktalarını ve uç noktaları](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
