---
title: "Azure CLI 2.0 ile özel bir Linux disk yüklemek | Microsoft Docs"
description: "Oluşturma ve bir sanal sabit disk (VHD) için Azure Resource Manager dağıtım modeli ve Azure CLI 2.0 kullanarak yükleme"
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
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 258c2a5bbce1f15c78690cb01dc9b66fef4bb8f5
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a>Karşıya yükleme ve Azure CLI 2.0 ile özel diskten bir Linux VM oluşturma
Bu makalede Azure CLI 2.0 ile Azure depolama hesabı için bir sanal sabit disk (VHD) karşıya yükleyin ve bu özel diskten Linux VM'ler oluşturmak gösterilmektedir. Bu adımları [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile de gerçekleştirebilirsiniz. Bu işlevsellik, yükleme ve Linux distro gereksinimlerinize yapılandırmak ve hızlı bir şekilde Azure sanal makineleri (VM'ler) oluşturmak için bu VHD kullanmak olanak sağlar.

Bu konuda depolama hesapları için son VHD'leri kullanır, ancak aşağıdaki adımları kullanarak da yapabilirsiniz [yönetilen diskleri](upload-vhd.md). 

## <a name="quick-commands"></a>Hızlı komutlar
Azure için bir VHD yüklemek için hızlı bir şekilde, aşağıdaki bölümde ayrıntıları temel görevi gerekiyorsa komutları. Her adım, belgenin geri kalanında bulunabilir bilgi ve içerik daha ayrıntılı [burada başlangıç](#requirements).

En son sahip olduğunuzdan emin olun [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil `myResourceGroup`, `mystorageaccount`, ve `mydisks`.

Öncelikle [az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu oluşturur `myResourceGroup` içinde `WestUs` konumu:

```azurecli
az group create --name myResourceGroup --location westus
```

Sanal diskleri tutmak için depolama hesabı oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#az_storage_account_create). Aşağıdaki örnek adlı bir depolama hesabı oluşturur `mystorageaccount`:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Depolama hesabınız için erişim anahtarları listesinde [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys#az_storage_account_keys_list). Not `key1`:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

Aldığınız ile depolama anahtarı kullanarak depolama hesabınıza içinde bir kapsayıcı oluşturmak [az depolama kapsayıcısı oluşturmak](/cli/azure/storage/container#az_storage_container_create). Aşağıdaki örnek adlı bir kapsayıcı oluşturur `mydisks` depolama anahtarı değerini kullanarak `key1`:

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

Son olarak, oluşturduğunuz kapsayıcısı, VHD'yi karşıya [az depolama blob karşıya yükleme](/cli/azure/storage/blob#az_storage_blob_upload). VHD altında yerel yolunu belirtin `/path/to/disk/mydisk.vhd`:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

Diskinize URI'sini belirtin (`--image`) ile [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur `myVM` sanal diski kullanarak daha önce karşıya:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

Hedef depolama hesabının sanal diskinizin karşıya burada ile aynı olması gerekir. Belirtmeniz gerekir ya da yanıt ister gerektirdiği tüm ek parametreleri için **az vm oluşturma** sanal ağ, genel IP adresi, kullanıcı adı ve SSH anahtarları gibi komutu. Daha fazla bilgi edinebilirsiniz [kullanılabilir CLI Resource Manager parametreleri](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Gereksinimler
Aşağıdaki adımları tamamlamak için gerekir:

* **Bir .vhd dosyası yüklü Linux işletim sistemi** -yüklemek bir [Linux Azure destekli dağıtım](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (veya bkz [desteklenmeyen dağıtımlarla bilgi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) VHD biçiminde bir sanal disk için . Bir VM ve VHD oluşturmak için birden çok araç mevcuttur:
  * Yükleme ve yapılandırma [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) veya [KVM](http://www.linux-kvm.org/page/RunningKVM), alma, resim biçimi olarak VHD kullanmaya dikkat edin. Gerekirse, [bir görüntüyü dönüştürme](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) kullanarak `qemu-img convert`.
  * Hyper-V de kullanabilirsiniz [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) veya [Windows Server 2012/2012 R2 üzerinde](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmiyor. Bir VM oluşturduğunuzda, VHD biçiminde belirtin. Gerekirse, VHD kullanarak VHDX diskleri dönüştürebilirsiniz [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet'i. Ayrıca, Azure karşıya yüklemeden önce statik VHD'ler gibi diskleri dönüştürmeniz gerekir böylece dinamik VHD yüklemeyi desteklemez. Araçları gibi kullanabilir [Git için Azure VHD yardımcı programları](https://github.com/Microsoft/azure-vhd-utils-for-go) Azure'a karşıya yükleme işlemi sırasında dinamik diskleri dönüştürme.
> 
> 

* Özel diskinizden oluşturulan VM'ler disk aynı depolama hesabındaki bulunmalıdır
  * Bir depolama hesabı ve özel disk ve oluşturulan VM'ler tutmak için kapsayıcı oluşturma
  * Tüm Vm'lerinizi oluşturduktan sonra disk güvenle silebilirsiniz

En son sahip olduğunuzdan emin olun [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil `myResourceGroup`, `mystorageaccount`, ve `mydisks`.

<a id="prepimage"> </a>

## <a name="prepare-the-disk-to-be-uploaded"></a>Karşıya yüklenecek disk hazırlama
Azure çeşitli Linux dağıtımları destekler (bkz [destekli dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Aşağıdaki makaleler Azure üzerinde desteklenen çeşitli Linux dağıtımları hazırlanma konusunda size kılavuzluk eder:

* **[CentOS tabanlı dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Diğer - desteklenmeyen dağıtımlarla](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Ayrıca bkz.  **[Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes)**  için Azure Linux görüntüleri hazırlama hakkında daha fazla genel ipuçları için.

> [!NOTE]
> [Azure platformu SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) içinde yalnızca doğrulanan dağıtımları birini 'Sürümleri desteklenir' altında belirtildiği gibi yapılandırma ayrıntıları ile kullanıldığında, Linux çalıştıran Vm'leri uygulandığı [Azure-Endorsed dağıtımları Linux'ta](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grupları, mantıksal olarak birlikte sanal ağ ve depolama gibi sanal makinelerinizi desteklemek için tüm Azure kaynaklarına duruma getirin. Daha fazla bilgi kaynak grupları için bkz: [kaynak gruplarını genel bakış](../../azure-resource-manager/resource-group-overview.md). Özel diskinizin karşıya yükleme ve sanal makineleri oluşturma önce ilk sahip bir kaynak grubu oluşturmak ihtiyacınız [az grubu oluşturma](/cli/azure/group#az_group_create).

Aşağıdaki örnek, bir kaynak grubu oluşturur `myResourceGroup` içinde `westus` konumu:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Özel disk ve VM'lerin için depolama hesabı oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#az_storage_account_create). Herhangi bir VM yönetilmeyen disklerle, bu diski aynı depolama hesabındaki olması gerekiyor, özel disk oluşturun. 

Aşağıdaki örnek adlı bir depolama hesabı oluşturur `mystorageaccount` daha önce oluşturduğunuz kaynak grubunda:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a>Depolama hesabı anahtarlarını Listele
Azure her depolama hesabı için iki 512 bit erişim tuşu oluşturur. Bu erişim anahtarları, depolama hesabına gibi doğrulanırken, yazma işlemleri gerçekleştirmek için kullanılır. Daha fazla bilgi edinin [depolama burada erişimi yönetme](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). İle erişim tuşları görüntüleme [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys#az_storage_account_keys_list).

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
Not `key1` depolama hesabınız sonraki adımlarda ile etkileşim kurmak için kullanacağınız.

## <a name="create-a-storage-container"></a>Depolama kapsayıcısı oluşturma
Yerel dosya sisteminde mantıksal olarak düzenlemek için farklı dizin oluşturmak aynı şekilde, disklerinizi düzenlemek için bir depolama hesabı kapsayıcılara oluşturun. Bir depolama hesabı kapsayıcıların herhangi bir sayı içerebilir. İle bir kapsayıcı oluşturmak [az depolama kapsayıcısı oluşturmak](/cli/azure/storage/container#az_storage_container_create).

Aşağıdaki örnek adlı bir kapsayıcı oluşturur `mydisks`:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a>VHD karşıya yükle
Şimdi özel diskiniz ile karşıya [az depolama blob karşıya yükleme](/cli/azure/storage/blob#az_storage_blob_upload). Karşıya yükleme ve özel diskinizin bir sayfa blob'u olarak depolar.

Erişim anahtarınız, yerel bilgisayarınızda önceki adımı ve özel disk yolu oluşturulan kapsayıcı belirtin:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-the-vm"></a>Sanal makine oluşturma
Yönetilmeyen disklerle bir VM oluşturmak için URI diskinize belirtin (`--image`) ile [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur `myVM` sanal diski kullanarak daha önce karşıya:

Belirttiğiniz `--image` parametresiyle [az vm oluşturma](/cli/azure/vm#az_vm_create) özel diskinize yönlendirin. Emin `--storage-account` özel diskinizin depolandığı depolama hesabını eşleşir. Aynı kapsayıcı, sanal makineleri depolamak için özel disk olarak kullanmak zorunda değil. Özel diskinizin karşıya yüklemeden önce önceki adımlarda aynı şekilde herhangi bir ek kapsayıcıdaki oluşturduğunuzdan emin olun.

Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur `myVM` özel diskinizden:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

Yine de belirtmeniz gerekiyorsa veya yanıt ister gerektirdiği tüm ek parametreleri için **az vm oluşturma** kullanıcı adı ve SSH anahtarları gibi komutu.


## <a name="resource-manager-template"></a>Resource Manager şablonu
Azure Resource Manager şablonları oluşturmak istediğiniz ortamını tanımlayan JavaScript nesne gösterimi (JSON) dosyalarıdır. Şablonlar, işlem veya ağ gibi farklı kaynak sağlayıcıları için ayrılmıştır. Var olan şablonları kullanın ya da kendi yazma. Daha fazla bilgi edinin [Resource Manager ve şablonlar kullanarak](../../azure-resource-manager/resource-group-overview.md).

İçinde `Microsoft.Compute/virtualMachines` sağlayıcı şablonunuzun elinizde bir `storageProfile` VM için yapılandırma ayrıntılarını içeren düğümü. Düzenlemek için iki ana parametreler `image` ve `vhd` özel diskinizin ve yeni VM sanal diske işaret URI. Özel bir disk kullanarak JSON örneği gösterilmektedir:

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

Kullanabileceğiniz [özel bir görüntüden bir VM oluşturmak için bu mevcut şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) veya ilgili bilgileri okuyun [kendi Azure Resource Manager şablonları oluşturma](../../azure-resource-manager/resource-group-authoring-templates.md). 

Yapılandırılmış bir şablonu oluşturduktan sonra kullanma [az grup dağıtımı oluşturmak](/cli/azure/group/deployment#az_group_deployment_create) Vm'leriniz oluşturmak için. JSON şablonunuzla URI'sini belirtin `--template-uri` parametre:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

Yerel olarak bilgisayarınızda depolanan bir JSON dosyası varsa, kullanabileceğiniz `--template-file` parametresi bunun yerine:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Sonraki adımlar
Hazır ve özel sanal diskinizin karşıya sonra daha fazla bilgi edinebilirsiniz [Resource Manager ve şablonlar kullanarak](../../azure-resource-manager/resource-group-overview.md). Ayrıca isteyebilirsiniz [bir veri diski Ekle](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) yeni Vm'leriniz için. Erişmesi gereken Vm'leriniz çalışan uygulamalar varsa, emin olun [açık bağlantı noktalarını ve uç noktaları](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

