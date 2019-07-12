---
title: Azure CLI ile özel bir Linux disk karşıya yükle | Microsoft Docs
description: Oluşturma ve Resource Manager dağıtım modelini ve Azure CLI kullanarak Azure'da bir sanal sabit disk (VHD) yükleme
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 9614614782179f9160aebdc4deca88f067778060
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708364"
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-the-azure-cli"></a>Azure CLI ile bir özel diskten Linux VM oluşturma ve karşıya yükleme

Bu makalede Azure CLI ile Azure depolama hesabınız için bir sanal sabit disk (VHD) karşıya yükleyin ve bu özel diskten Linux Vm'leri oluşturma gösterilmektedir. Bu işlev, yüklemek ve bir Linux distro gereksinimlerinize yapılandırın ve ardından Azure sanal makineleri (VM) hızlı bir şekilde oluşturmak için bu VHD'yi kullanması sağlar.

Bu konuda depolama hesapları için son VHD'ler kullanır, ancak bu adımları kullanarak da yapabilirsiniz [yönetilen diskler](upload-vhd.md). 

## <a name="quick-commands"></a>Hızlı komutlar
Hızlı bir şekilde, aşağıdaki bölümde ayrıntıları temel görevi gerekirse Azure'a VHD yükleme komutları. Bilgi ve içerik her adım, belgenin geri kalanında bulunabilir ayrıntılı [burada başlangıç](#requirements).

En son sahip olduğunuzdan emin olun [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil `myResourceGroup`, `mystorageaccount`, ve `mydisks`.

Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek `WestUs` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location westus
```

Sanal diskleri tutmak için depolama hesabı oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account). Aşağıdaki örnekte adlı bir depolama hesabı oluşturur `mystorageaccount`:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Depolama hesabınızın erişim anahtarlarını Listele [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys). Not `key1`:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

İle elde ettiğiniz depolama anahtarını kullanarak, depolama hesabında bir kapsayıcı oluşturmak [az depolama kapsayıcısı oluşturma](/cli/azure/storage/container). Aşağıdaki örnekte adlı bir kapsayıcı oluşturur `mydisks` depolama anahtar değerini kullanarak `key1`:

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

Son olarak, oluşturduğunuz kapsayıcıya VHD'nizi karşıya [az storage blob upload](/cli/azure/storage/blob). VHD'nizi yerel yol belirtin `/path/to/disk/mydisk.vhd`:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

Disk için bir URI belirtin (`--image`) ile [az vm oluşturma](/cli/azure/vm). Aşağıdaki örnekte adlı bir VM oluşturur `myVM` sanal diski kullanarak daha önce yüklenmiş:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

Hedef depolama hesabını, sanal diskinizin yüklediğiniz aynı olması gerekir. Belirtmeniz gerekir veya yanıt ister gerektirdiği tüm ek parametreleri için **az vm oluşturma** sanal ağ, genel IP adresi, kullanıcı adı ve SSH anahtarları gibi komutu. Daha fazla bilgi edinebilirsiniz [kullanılabilir CLI Resource Manager parametreleri](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Gereksinimler
Aşağıdaki adımları tamamlamak için gerekir:

* **Linux işletim sistemi yüklü bir .vhd dosyasına** -yüklemek bir [Azure destekli Linux dağıtım](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (veya [bilgi dağıtımlarla için](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) VHD biçiminde bir sanal diske . VM ve VHD oluşturmak için birden çok araçlar mevcuttur:
  * Yükleme ve yapılandırma [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) veya [KVM](https://www.linux-kvm.org/page/RunningKVM), alma, görüntü biçimi olarak VHD kullanmak için dikkat edin. Gerekirse, [görüntüyü dönüştürme](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) kullanarak `qemu-img convert`.
  * Hyper-V ayrıca kullanabileceğiniz [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) veya [Windows Server 2012/2012 R2'deki](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmiyor. Bir VM oluşturduğunuzda, VHD biçiminde belirtin. Gerekirse, VHD kullanarak VHDX diskler dönüştürülebilir [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet'i. Ayrıca, Azure gibi disklerin statik Vhd'lere karşıya yüklemeden önce dönüştürmeniz gerekmez dinamik VHD'ler, karşıya yükleme desteklemez. Gibi araçları kullanın [GO için Azure VHD'nin Utilities](https://github.com/Microsoft/azure-vhd-utils-for-go) Azure'a yükleme işlemi sırasında dinamik diskleri dönüştürme.
> 
> 

* Özel diskinizden oluşturulan sanal makinelerin disk ile aynı depolama hesabındaki bulunmalıdır
  * Bir depolama hesabı ve kapsayıcı özel disk ve oluşturulan VM'ler için oluşturma
  * Tüm Vm'leriniz oluşturduktan sonra disk güvenli bir şekilde silebilirsiniz

En son sahip olduğunuzdan emin olun [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil `myResourceGroup`, `mystorageaccount`, ve `mydisks`.

<a id="prepimage"></a>

## <a name="prepare-the-disk-to-be-uploaded"></a>Karşıya yüklenecek diski hazırlama
Azure, çeşitli Linux dağıtımları destekler (bkz [desteklenen dağıtımı](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Aşağıdaki makaleler Azure üzerinde desteklenen çeşitli Linux dağıtımları hazırlama size kılavuzluk eder:

* **[CentOS tabanlı dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Diğer - dağıtımlarla](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Ayrıca bkz: **[Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes)** Azure için Linux görüntüleri hazırlama hakkında daha fazla genel ipuçları için.

> [!NOTE]
> [Azure platformunun SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) , yalnızca desteklenen dağıtımlarla birini 'Sürümleri desteklenir' altında belirtildiği gibi yapılandırma ayrıntılarını ile kullanıldığında, Linux çalıştıran Vm'leri uygulandığı [Azure destekli Linux Dağıtımları](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grupları mantıksal olarak sanal ağ ve depolama gibi sanal makinelerinizi desteklemek için tüm Azure kaynaklarını bir araya getirin. Daha fazla bilgi kaynak grupları için bkz: [kaynak gruplarına genel bakış](../../azure-resource-manager/resource-group-overview.md). Özel diskinizi karşıya yükleme ve sanal makineleri oluşturma önce ilk ile bir kaynak grubu oluşturmak için ihtiyacınız [az grubu oluşturma](/cli/azure/group).

Aşağıdaki örnek `westus` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Özel disk ve VM içeren bir depolama hesabı oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account). Bu disk aynı depolama hesabındaki olacak şekilde özel disk gereksiniminizi kaynağından oluşturduğunuz yönetilmeyen disklere sahip makineler. 

Aşağıdaki örnekte adlı bir depolama hesabı oluşturur `mystorageaccount` daha önce oluşturduğunuz kaynak grubunda:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a>Depolama hesabı anahtarlarını Listele
Azure, her depolama hesabı için iki adet 512 bit erişim tuşu oluşturur. Bu erişim anahtarlarını gibi depolama hesabı için kimlik doğrulaması yapılırken, yazma işlemleri gerçekleştirmek için kullanılır. Daha fazla bilgi edinin [burada Depolama Yönetimi](../../storage/common/storage-account-manage.md#access-keys). Erişim anahtarı görüntüleme [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys).

Oluşturduğunuz depolama hesabının erişim anahtarlarını görüntüleyin:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
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
Not `key1` sonraki adımlarda depolama hesabınızla etkileşim kurmak için kullanacağınız.

## <a name="create-a-storage-container"></a>Bir depolama kapsayıcısı oluşturma
Yerel dosya sisteminize mantıksal olarak düzenlemek için farklı bir dizin oluşturma yolla disklerinizi düzenlemek için bir depolama hesabında bir kapsayıcı oluşturun. Bir depolama hesabı herhangi bir sayıda kapsayıcı içerebilir. Bir kapsayıcı ile [az depolama kapsayıcısı oluşturma](/cli/azure/storage/container).

Aşağıdaki örnekte adlı bir kapsayıcı oluşturur `mydisks`:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a>VHD'yi karşıya yükleme
Şimdi, özel bir disk ile karşıya [az storage blob upload](/cli/azure/storage/blob). Size karşıya yükleme ve sayfa blobu olarak özel diskinizin depolama.

Erişim anahtarınızı, önceki adımda ve özel disk yolu yerel bilgisayarınızda oluşturduğunuz kapsayıcıya belirtin:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-the-vm"></a>Sanal makine oluşturma
Yönetilmeyen disklerle bir VM oluşturmak için disk URI'si belirtin (`--image`) ile [az vm oluşturma](/cli/azure/vm). Aşağıdaki örnekte adlı bir VM oluşturur `myVM` sanal diski kullanarak daha önce yüklenmiş:

Belirttiğiniz `--image` parametresiyle [az vm oluşturma](/cli/azure/vm) özel diskinize yönlendirin. Emin `--storage-account` özel diskinizi depolandığı depolama hesabını eşleşir. Aynı kapsayıcı Vm'lerinizi depolamak için özel bir disk olarak kullanmak zorunda değil. Özel diskinizi karşıya yüklemeden önce önceki adımları aynı şekilde ek tüm kapsayıcıları oluşturduğunuzdan emin olun.

Aşağıdaki örnekte adlı bir VM oluşturur `myVM` özel diskinizden:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

Yine de belirtmeniz gerekir veya yanıt ister gerektirdiği tüm ek parametreleri için **az vm oluşturma** kullanıcı adı ve SSH anahtarları gibi komutu.


## <a name="resource-manager-template"></a>Resource Manager şablonu
Azure Resource Manager şablonları, oluşturmak istediğiniz ortamı tanımlayan JavaScript nesne gösterimi (JSON) dosyalarıdır. Şablonları, işlem veya ağ gibi farklı kaynak sağlayıcıları için ayrılmıştır. Var olan şablonları kullanın veya kendinizinkini yazabilirsiniz. Daha fazla bilgi edinin [Resource Manager şablonları ile](../../azure-resource-manager/resource-group-overview.md).

İçinde `Microsoft.Compute/virtualMachines` şablonunuzu sağlayıcısı, sahip olduğunuz bir `storageProfile` VM'niz için yapılandırma ayrıntılarını içeren bir düğüm. Düzenlemek için iki ana parametreler `image` ve `vhd` özel diskinizi ve yeni sanal makinenizin sanal diski işaret bir URI'leri. Özel bir disk kullanmak için JSON örneği aşağıda gösterilmiştir:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Kullanabileceğiniz [özel bir görüntüden bir VM oluşturmak için bu mevcut şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) veya okuma hakkında [kendi Azure Resource Manager şablonları oluşturmaya](../../azure-resource-manager/resource-group-authoring-templates.md). 

Yapılandırılmış bir şablonu oluşturduktan sonra kullanma [az grubu dağıtım oluşturma](/cli/azure/group/deployment) Vm'lerinizi oluşturmak için. Bir URI ile JSON şablonunuzun `--template-uri` parametresi:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

Yerel olarak bilgisayarınızda depolanan bir JSON dosyası varsa, kullanabileceğiniz `--template-file` parametresi yerine:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Sonraki adımlar
Hazır ve özel sanal diskinizin karşıya sonra daha fazla bilgi edinebilirsiniz [Resource Manager şablonları ile](../../azure-resource-manager/resource-group-overview.md). Ayrıca isteyebilirsiniz [veri diski ekleme](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) yeni vm'lere. Erişmesi gereken sanal makinelerinizde çalışan uygulamalarınız varsa, mutlaka [açık bağlantı noktalarını ve uç noktaları](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

