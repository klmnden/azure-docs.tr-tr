---
title: "Karşıya yükleme veya Azure CLI 2.0 ile özel bir Linux VM Kopyala | Microsoft Docs"
description: "Karşıya yükleme veya Resource Manager dağıtım modeli ve Azure CLI 2.0 kullanarak özelleştirilmiş bir sanal makine kopyalama"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
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
ms.openlocfilehash: 49e6f4a1222d11d096577265e3e7c7a30169bf81
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a>Azure CLI 2.0 ile özel diskten bir Linux VM oluşturma

<!-- rename to create-vm-specialized -->

Bu makalede, özelleştirilmiş sanal sabit disk (VHD) veya kopya yüklemeyi gösteren bir Azure varolan bir VHD'yi ve özel diskten yeni Linux sanal makineleri (VM'ler) oluşturun. Yükleme ve Linux distro gereksinimlerinize yapılandırmak ve hızlı bir şekilde yeni bir Azure sanal makine oluşturmak için bu VHD kullanın.

Özelleştirilmiş diskinizden birden çok VM oluşturmak istiyorsanız, görüntü, VM veya VHD oluşturmanız gerekir. Daha fazla bilgi için bkz: [Azure CLI kullanarak bir VM'nin özel bir görüntü oluşturun](tutorial-custom-images.md).

İki seçeneğiniz vardır:
* [Bir VHD’yi karşıya yükleme](#option-1-upload-a-specialized-vhd)
* [Mevcut bir Azure VM'yi kopyalama](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a>Hızlı komutlar

Kullanarak yeni bir VM oluştururken [az vm oluşturma](/cli/azure/vm#az_vm_create) özelleştirilmiş veya özelleştirilmiş bir diskten, **attach** disk (--attach-os-disk) yerine bir özel veya Market görüntüsü belirtme (--görüntü). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM* adlı Yönetilen diski kullanarak *myManagedDisk* özelleştirilmiş VHD'den oluşturuldu:

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a>Gereksinimler
Aşağıdaki adımları tamamlamak için gerekir:

* Azure kullanıma hazır bir Linux sanal makine. [VM hazırlama](#prepare-the-vm) bu makalenin Azure Linux Azure düzgün çalışması VM ve, SSH kullanarak bağlanabilmesi gerekli olan Aracı (waagent) yükleme distro belirli bilgileri bulmak nasıl ele alınmaktadır.
* Varolan bir VHD dosyasının [Linux Azure destekli dağıtım](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (veya bkz [desteklenmeyen dağıtımlarla bilgi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) VHD biçiminde bir sanal disk için. Bir VM ve VHD oluşturmak için birden çok araç mevcuttur:
  * Yükleme ve yapılandırma [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) veya [KVM](http://www.linux-kvm.org/page/RunningKVM), alma, resim biçimi olarak VHD kullanmaya dikkat edin. Gerekirse, [bir görüntüyü dönüştürme](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) kullanarak **qemu img Dönüştür**.
  * Hyper-V de kullanabilirsiniz [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) veya [Windows Server 2012/2012 R2 üzerinde](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmiyor. Bir VM oluşturduğunuzda, VHD biçiminde belirtin. Gerekirse, VHD kullanarak VHDX diskleri dönüştürebilirsiniz [qemu img Dönüştür](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet'i. Ayrıca, Azure karşıya yüklemeden önce statik VHD'ler gibi diskleri dönüştürmeniz gerekir böylece dinamik VHD yüklemeyi desteklemez. Araçları gibi kullanabilir [Git için Azure VHD yardımcı programları](https://github.com/Microsoft/azure-vhd-utils-for-go) Azure'a karşıya yükleme işlemi sırasında dinamik diskleri dönüştürme.
> 
> 


* En son sahip olduğunuzdan emin olun [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *mystorageaccount*, ve *mydisks*.

<a id="prepimage"> </a>

## <a name="prepare-the-vm"></a>VM’yi hazırlama

Azure çeşitli Linux dağıtımları destekler (bkz [destekli dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Aşağıdaki makaleler Azure üzerinde desteklenen çeşitli Linux dağıtımları hazırlanma konusunda size kılavuzluk eder:

* [CentOS tabanlı dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Diğer - desteklenmeyen dağıtımlarla](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Ayrıca bkz. [Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes) için Azure Linux görüntüleri hazırlama hakkında daha fazla genel ipuçları için.

> [!NOTE]
> [Azure platformu SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) içinde yalnızca doğrulanan dağıtımları birini 'Sürümleri desteklenir' altında belirtildiği gibi yapılandırma ayrıntıları ile kullanıldığında, Linux çalıştıran Vm'leri uygulandığı [Azure-Endorsed dağıtımları Linux'ta](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="option-1-upload-a-vhd"></a>Seçenek 1: bir VHD yüklemek

Yerel makinede çalışıyor olması veya başka bir buluttan dışarı özelleştirilmiş bir VHD'yi yükleyebilirsiniz. Yeni bir Azure VM oluşturmak üzere VHD'yi kullanmak için bir depolama hesabına VHD'nin yüklenmesi ve yönetilen bir disk VHD'den oluşturmanız gerekir. 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Özel diskinizin karşıya yükleme ve sanal makineleri oluşturma önce ilk sahip bir kaynak grubu oluşturmak ihtiyacınız [az grubu oluşturma](/cli/azure/group#az_group_create).

Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* konumu: [Azure yönetilen diskleri genel bakış](../windows/managed-disks-overview.md)
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Özel disk ve VM'lerin için depolama hesabı oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#az_storage_account_create). 

Aşağıdaki örnek adlı bir depolama hesabı oluşturur *mystorageaccount* daha önce oluşturduğunuz kaynak grubunda:

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a>Depolama hesabı anahtarlarını Listele
Azure her depolama hesabı için iki 512 bit erişim tuşu oluşturur. Bu erişim anahtarlarını yazma işlemleri gerçekleştirme gibi depolama hesabı, kimlik doğrulaması yapılırken kullanılır. Daha fazla bilgi edinin [depolama burada erişimi yönetme](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). İle erişim tuşları görüntüleme [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys#az_storage_account_keys_list).

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
Not **key1** depolama hesabınız sonraki adımlarda ile etkileşim kurmak için kullanacağınız.

### <a name="create-a-storage-container"></a>Depolama kapsayıcısı oluşturma
Yerel dosya sisteminde mantıksal olarak düzenlemek için farklı dizin oluşturmak aynı şekilde, disklerinizi düzenlemek için bir depolama hesabı kapsayıcılara oluşturun. Bir depolama hesabı kapsayıcıların herhangi bir sayı içerebilir. İle bir kapsayıcı oluşturmak [az depolama kapsayıcısı oluşturmak](/cli/azure/storage/container#az_storage_container_create).

Aşağıdaki örnek adlı bir kapsayıcı oluşturur *mydisks*:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-the-vhd"></a>VHD'nin yüklenmesi
Şimdi özel diskiniz ile karşıya [az depolama blob karşıya yükleme](/cli/azure/storage/blob#az_storage_blob_upload). Karşıya yükleme ve özel diskinizin bir sayfa blob'u olarak depolar.

Erişim anahtarınız, yerel bilgisayarınızda önceki adımı ve özel disk yolu oluşturulan kapsayıcı belirtin:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
VHD karşıya biraz zaman alabilir.

### <a name="create-a-managed-disk"></a>Yönetilen bir disk oluşturma


Bir yönetilen VHD kullanımından disketi [az disketi](/cli/azure/disk#az_disk_create). Aşağıdaki örnek, adında yönetilen bir disk oluşturur *myManagedDisk* adlı depolama hesabı ve kapsayıcı için karşıya VHD'den:

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a>Seçenek 2: mevcut bir VM'yi kopyalama

Ayrıca Azure'da özelleştirilmiş VM oluşturun ve ardından işletim sistemi diski kopyalayın ve başka bir kopya oluşturmak için yeni bir VM'e ekleyin. Bu test etmek için uygundur, ancak mevcut bir Azure VM'i model olarak birden çok yeni VM'ler için kullanmak istiyorsanız, gerçekten oluşturmalısınız bir **görüntü** yerine. Var olan bir Azure sanal makineden bir görüntü oluşturma hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak bir VM'nin özel bir görüntü oluşturun](tutorial-custom-images.md)

### <a name="create-a-snapshot"></a>Bir anlık görüntü oluşturma

Bu örnek, adlandırılmış bir VM'nin bir anlık görüntü oluşturur *myVM* kaynak grubunda *myResourceGroup* ve adında bir anlık görüntü oluşturur *osDiskSnapshot*.

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-the-managed-disk"></a>Yönetilen disketi oluşturma

Yeni bir yönetilen disk anlık görüntüden oluşturun.

Anlık görüntü Kimliğini alın. Bu örnekte, anlık görüntü adlı *osDiskSnapshot* ve olarak *myResourceGroup* kaynak grubu.

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

Yönetilen diski oluşturun. Bu örnekte, adında yönetilen bir disk oluşturacağız *myManagedDisk* bizim anlık görüntüden 128 GB boyutunda standart depolama olmasıdır.

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a>Sanal makine oluşturma

Şimdi, VM oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create) ve ekleme (--attach-os-disk) işletim sistemi diski olarak yönetilen disk. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myNewVM* karşıya yüklenen VHD'den oluşturulan yönetilen diski kullanarak:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

Kaynak VM kimlik bilgilerini kullanarak VM içine SSH için görüyor olmalısınız. 

## <a name="next-steps"></a>Sonraki adımlar
Hazır ve özel sanal diskinizin karşıya sonra daha fazla bilgi edinebilirsiniz [Resource Manager ve şablonlar kullanarak](../../azure-resource-manager/resource-group-overview.md). Ayrıca isteyebilirsiniz [bir veri diski Ekle](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) yeni Vm'leriniz için. Erişmesi gereken Vm'leriniz çalışan uygulamalar varsa, emin olun [açık bağlantı noktalarını ve uç noktaları](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

