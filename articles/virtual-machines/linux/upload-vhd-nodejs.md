---
title: Azure CLI 1.0 ile özel bir Linux görüntüyü karşıya yüklemeden | Microsoft Docs
description: Oluşturun ve bir sanal sabit disk (VHD) için Azure Resource Manager dağıtım modeli ve Azure CLI 1.0 kullanarak özel bir Linux görüntü ile yükleyin.
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: iainfou
ms.openlocfilehash: 6eb0cae2b70e0cbb9a4fb5fcab3a58d566d0f4d9
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30915319"
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-the-azure-cli-10"></a>Karşıya yükleme ve Azure CLI 1.0 kullanarak özel bir disk görüntüsünü bir Linux VM oluşturma
Bu makalede Resource Manager dağıtım modelini kullanarak Azure'a bir sanal sabit disk (VHD) yükleyin ve Linux VM'ler bu özel görüntüsünü oluşturmak nasıl gösterir. Bu işlevsellik, yükleme ve Linux distro gereksinimlerinize yapılandırmak ve hızlı bir şekilde Azure sanal makineleri (VM'ler) oluşturmak için bu VHD kullanmak olanak sağlar.


## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#quick-commands) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız


## <a name="quick-commands"></a>Hızlı komutlar
Bir VM için Azure karşıya yüklemek için hızlı bir şekilde, aşağıdaki bölümde ayrıntıları temel görevi gerekiyorsa komutları. Her adım, belgenin geri kalanında bulunabilir bilgi ve içerik daha ayrıntılı [burada başlangıç](#requirements).

Bilgisayarınızda yüklü olduğundan emin olun [Azure CLI 1.0](../../cli-install-nodejs.md) oturum açmış ve Resource Manager modunu kullanma:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil `myResourceGroup`, `mystorageaccount`, ve `myimages`.

İlk olarak, bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu oluşturur `myResourceGroup` içinde `WestUs` konumu:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

Sanal diskleri tutmak için bir depolama hesabı oluşturun. Aşağıdaki örnek adlı bir depolama hesabı oluşturur `mystorageaccount`:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Depolama hesabınız için erişim anahtarlarını listeler. Not `key1`:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Depolama hesabınızda aldığınız depolama anahtarı kullanarak bir kapsayıcı oluşturun. Aşağıdaki örnek adlı bir kapsayıcı oluşturur `myimages` depolama anahtarı değerini kullanarak `key1`:

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Son olarak, VHD'yi oluşturduğunuz kapsayıcıya karşıya yükleyin. VHD altında yerel yolunu belirtin `/path/to/disk/mydisk.vhd`:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Karşıya yüklenen sanal diskten bir VM artık oluşturabilirsiniz [Resource Manager şablonu kullanarak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). URI diskinize belirterek CLI de kullanabilirsiniz (`--image-urn`). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur `myVM` sanal diski kullanarak daha önce karşıya:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Hedef depolama hesabının sanal diskinizin karşıya burada ile aynı olması gerekir. Belirtmeniz gerekir ya da yanıt ister gerektirdiği tüm ek parametreleri için `azure vm create` sanal ağ, genel IP adresi, kullanıcı adı ve SSH anahtarları gibi komutu. Daha fazla bilgi edinebilirsiniz [kullanılabilir CLI Resource Manager parametreleri](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Gereksinimler
Aşağıdaki adımları tamamlamak için gerekir:

* **Bir .vhd dosyası yüklü Linux işletim sistemi** -yüklemek bir [Linux Azure destekli dağıtım](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (veya bkz [desteklenmeyen dağıtımlarla bilgi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) VHD biçiminde bir sanal disk için . Bir VM ve VHD oluşturmak için birden çok araç mevcuttur:
  * Yükleme ve yapılandırma [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) veya [KVM](http://www.linux-kvm.org/page/RunningKVM), alma, resim biçimi olarak VHD kullanmaya dikkat edin. Gerekirse, [bir görüntüyü dönüştürme](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) kullanarak `qemu-img convert`.
  * Hyper-V de kullanabilirsiniz [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) veya [Windows Server 2012/2012 R2 üzerinde](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmiyor. Bir VM oluşturduğunuzda, VHD biçiminde belirtin. Gerekirse, VHD kullanarak VHDX diskleri dönüştürebilirsiniz [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet'i. Ayrıca, Azure karşıya yüklemeden önce statik VHD'ler gibi diskleri dönüştürmeniz gerekir böylece dinamik VHD yüklemeyi desteklemez. Araçları gibi kullanabilir [Git için Azure VHD yardımcı programları](https://github.com/Microsoft/azure-vhd-utils-for-go) Azure'a karşıya yükleme işlemi sırasında dinamik diskleri dönüştürme.
> 
> 

* Özel görüntüden oluşturulan VM'ler görüntü olarak aynı depolama hesabındaki bulunmalıdır
  * Bir depolama hesabı ve özel görüntü ve oluşturulan VM'ler tutmak için kapsayıcı oluşturma
  * Tüm Vm'lerinizi oluşturduktan sonra görüntünüzü güvenle silebilirsiniz

Bilgisayarınızda yüklü olduğundan emin olun [Azure CLI 1.0](../../cli-install-nodejs.md) oturum açmış ve Resource Manager modunu kullanma:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil `myResourceGroup`, `mystorageaccount`, ve `myimages`.

<a id="prepimage"> </a>

## <a name="prepare-the-image-to-be-uploaded"></a>Karşıya yüklenecek görüntüsünü hazırlama
Azure çeşitli Linux dağıtımları destekler (bkz [destekli dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Aşağıdaki makaleler Azure üzerinde desteklenen çeşitli Linux dağıtımları hazırlanma konusunda size kılavuzluk eder:

* **[CentOS tabanlı dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Diğer - desteklenmeyen dağıtımlarla](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Ayrıca bkz. **[Linux yükleme notları](create-upload-generic.md#general-linux-installation-notes)** için Azure Linux görüntüleri hazırlama hakkında daha fazla genel ipuçları için.

> [!NOTE]
> [Azure platformu SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) içinde yalnızca doğrulanan dağıtımları birini 'Sürümleri desteklenir' altında belirtildiği gibi yapılandırma ayrıntıları ile kullanıldığında, Linux çalıştıran Vm'leri uygulandığı [Azure-Endorsed dağıtımları Linux'ta](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grupları, mantıksal olarak birlikte sanal ağ ve depolama gibi sanal makinelerinizi desteklemek için tüm Azure kaynaklarına duruma getirin. Daha fazla bilgi edinin [Azure kaynak gruplarını burada](../../azure-resource-manager/resource-group-overview.md). Özel disk görüntünüzü karşıya yükleme ve sanal makineleri oluşturma önce ilk olarak bir kaynak grubu oluşturmanız gerekir. 

Aşağıdaki örnek, bir kaynak grubu oluşturur `myResourceGroup` içinde `WestUS` konumu:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Sanal makineleri bir depolama hesabındaki sayfa blobları olarak depolanır. Daha fazla bilgi edinin [Azure blob depolama burada](../../storage/common/storage-introduction.md#blob-storage). Özel disk görüntüsü ve VM'ler için bir depolama hesabı oluşturun. Özel disk görüntüden oluşturduğunuz herhangi bir VM, görüntü aynı depolama hesabındaki olması gerekir.

Aşağıdaki örnek adlı bir depolama hesabı oluşturur `mystorageaccount` daha önce oluşturduğunuz kaynak grubunda:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Depolama hesabı anahtarlarını Listele
Azure her depolama hesabı için iki 512 bit erişim tuşu oluşturur. Bu erişim anahtarları, depolama hesabına gibi doğrulanırken, yazma işlemleri gerçekleştirmek için kullanılır. Daha fazla bilgi edinin [depolama burada erişimi yönetme](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Erişim tuşları ile görüntüleyebilirsiniz `azure storage account keys list` komutu.

Oluşturduğunuz depolama hesabının erişim anahtarlarını görüntüleyin:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
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
Yerel dosya sisteminde mantıksal olarak düzenlemek için farklı dizin oluşturmak aynı şekilde, sanal diskleri ve görüntüleri düzenlemek için bir depolama hesabı kapsayıcılara oluşturun. Bir depolama hesabı kapsayıcıların herhangi bir sayı içerebilir. 

Aşağıdaki örnek adlı bir kapsayıcı oluşturur `myimages`, önceki adımda elde erişim tuşu belirtme (`key1`):

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>VHD karşıya yükle
Şimdi gerçekten özel disk görüntünüzü karşıya yükleyebilirsiniz. Olarak VM'ler tarafından kullanılan tüm sanal diskler ile karşıya yükleme ve özel disk görüntünüzü sayfa blob'u olarak depolar.

Erişim anahtarınız, yerel bilgisayarınızda önceki adımı ve özel disk görüntüsünün yolunu oluşturulan kapsayıcı belirtin:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Özel görüntüden VM oluşturma
VM'ler, özel bir disk görüntüsünü oluşturduğunuzda, disk görüntü URI'si belirtin. Özel disk görüntünüzü depolandığı hedef depolama hesabı eşleşmeleri emin olun. Azure CLI veya Resource Manager JSON şablonunu kullanarak, VM oluşturabilirsiniz.

### <a name="create-a-vm-using-the-azure-cli"></a>Azure CLI kullanarak bir VM oluşturma
Belirttiğiniz `--image-urn` parametresiyle `azure vm create` özel disk görüntüsüne işaret edecek şekilde komutu. Emin `--storage-account-name` özel disk görüntüsünün depolandığı depolama hesabını eşleşir. Vm'leriniz depolamak için özel disk görüntüsü olarak aynı kapsayıcı kullanmak zorunda değil. Özel disk görüntülerinizi karşıya yüklemeden önce önceki adımlarda aynı şekilde herhangi bir ek kapsayıcıdaki oluşturduğunuzdan emin olun.

Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur `myVM` özel disk görüntüsünden:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Yine de belirtmeniz gerekiyorsa veya yanıt ister gerektirdiği tüm ek parametreleri için `azure vm create` sanal ağ, genel IP adresi, kullanıcı adı ve SSH anahtarları gibi komutu. Daha fazla bilgi edinin [kullanılabilir CLI Resource Manager parametreleri](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Bir JSON şablonu kullanarak bir VM oluşturma
Azure Resource Manager şablonları oluşturmak istediğiniz ortamını tanımlayan JavaScript nesne gösterimi (JSON) dosyalarıdır. Şablonlar, işlem veya ağ gibi farklı kaynak sağlayıcıları için ayrılmıştır. Var olan şablonları kullanın ya da kendi yazma. Daha fazla bilgi edinin [Resource Manager ve şablonlar kullanarak](../../azure-resource-manager/resource-group-overview.md).

İçinde `Microsoft.Compute/virtualMachines` sağlayıcı şablonunuzun elinizde bir `storageProfile` VM için yapılandırma ayrıntılarını içeren düğümü. Düzenlemek için iki ana parametreler `image` ve `vhd` özel disk görüntünüzü ve yeni VM sanal diske işaret URI. Aşağıdaki özel disk görüntüsü kullanmak için JSON örneği gösterilmektedir:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Kullanabileceğiniz [özel bir görüntüden bir VM oluşturmak için bu mevcut şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) veya ilgili bilgileri okuyun [kendi Azure Resource Manager şablonları oluşturma](../../azure-resource-manager/resource-group-authoring-templates.md). 

Yapılandırılmış bir şablonu oluşturduktan sonra kullanarak, Vm'lerde oluşturma `azure group deployment create` komutu. JSON şablonunuzla URI'sini belirtin `--template-uri` parametre:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Yerel olarak bilgisayarınızda depolanan bir JSON dosyası varsa, kullanabileceğiniz `--template-file` parametresi bunun yerine:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Sonraki adımlar
Hazır ve özel sanal diskinizin karşıya sonra daha fazla bilgi edinebilirsiniz [Resource Manager ve şablonlar kullanarak](../../azure-resource-manager/resource-group-overview.md). Ayrıca isteyebilirsiniz [bir veri diski Ekle](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) yeni Vm'leriniz için. Erişmesi gereken Vm'leriniz çalışan uygulamalar varsa, emin olun [açık bağlantı noktalarını ve uç noktaları](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

