---
title: "Bir şablon olarak kullanmak üzere bir Azure Linux VM yakalama | Microsoft Docs"
description: "Yakalama ve görüntüyü bir Linux tabanlı Azure sanal makinesinin Azure Resource Manager dağıtım modeli kullanılarak oluşturulmuş (VM) generalize öğrenin."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: b1164fbd816eea5189786850f096438e32f8f802
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Azure üzerinde çalışan Linux sanal makine yakalama
Generalize ve Resource Manager dağıtım modelinde, Azure Linux sanal makine (VM) yakalamak için bu makaledeki adımları izleyin. VM generalize, kişisel hesap bilgilerini kaldırın ve bir görüntü olarak kullanılacak VM hazırlayın. Ardından VHD'ler, bağlı veri diskleri için işletim sistemi için genelleştirilmiş bir sanal sabit disk (VHD) görüntü yakalama ve [Resource Manager şablonu](../../azure-resource-manager/resource-group-overview.md) yeni VM dağıtımı için. Bu makalede yönetilmeyen diskleri kullanan bir VM için Azure CLI 1.0 ile bir VM görüntüsü yakalama ayrıntılarını verir. Ayrıca [Azure CLI 2.0 ile Azure yönetilen diskleri kullanarak bir VM yakalama](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Yönetilen diskleri Azure platformu tarafından işlenir ve hazırlık veya konum depolamaya gerektirmez. Daha fazla bilgi için bkz. [Azure Yönetilen Disklere Genel Bakış](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Görüntü kullanarak VM'ler oluşturmak için her yeni VM için ağ kaynakları ayarlamak ve yakalanan VHD yansımalarını dağıtmak için şablonu (JavaScript nesne gösterimi veya JSON, dosyası) kullanın. Bu şekilde, geçerli yazılım yapılandırması, benzer şekilde Azure Marketi'nde görüntüleri kullanmak'yle bir VM'yi çoğaltabilirsiniz.

> [!TIP]
> Yedekleme veya hata ayıklama için özel durumu ile bir kopyasını, varolan bir Linux VM oluşturmak istiyorsanız, bkz: [Azure üzerinde çalışan Linux sanal makine bir kopyasını oluşturmak](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ve bir şirket içi VM Linux VHD'den karşıya yüklemek istiyorsanız, bkz: [karşıya yükleme ve özel disk görüntüsünden bir Linux VM oluşturma](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#before-you-begin) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız

## <a name="before-you-begin"></a>Başlamadan önce
Aşağıdaki önkoşulları karşıladığından emin olun:

* **Azure VM Resource Manager dağıtım modelinde oluşturulan** -bir Linux VM oluşturmadıysanız, kullanabileceğiniz [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), veya [Resource Manager şablonları ](create-ssh-secured-vm-from-template.md). 
  
    VM gerektiği gibi yapılandırın. Örneğin, [veri diski Ekle](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)güncelleştirmeleri uygulamak ve uygulamaları yükleyin. 
* **Azure CLI** -yükleme [Azure CLI](../../cli-install-nodejs.md) yerel bir bilgisayarda.

## <a name="step-1-remove-the-azure-linux-agent"></a>1. adım: Azure Linux Aracısı'nı kaldırma
Öncelikle çalıştırın **waagent** komutunu **deprovision** Linux VM'de parametresi. Bu komut dosyaları ve VM genelleme için hazır hale getirmek için verileri siler. Ayrıntılar için bkz [Azure Linux Aracısı Kullanıcı Kılavuzu](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

1. Bir SSH istemcisi kullanarak Linux VM'NİZDE bağlayın.
2. SSH penceresinde aşağıdaki komutu yazın:
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > Yalnızca resim olarak yakalamasına düşündüğünüz bir VM'de bu komutu çalıştırın. Görüntü tüm hassas bilgilerin temizlenmiş veya yeniden dağıtım için uygun garanti etmez.
 
3. Tür **y** devam etmek için. Ekleyebileceğiniz **-force** bu onay adım önlemek için parametre.
4. Komut tamamlandığında yazın **çıkmak**. Bu adım, SSH istemcisi kapatır.

## <a name="step-2-capture-the-vm"></a>2. adım: VM yakalama
Generalize ve VM yakalama için Azure CLI kullanın. Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında **myResourceGroup**, **myVnet**, ve **myVM**.

1. Azure CLI yerel bilgisayarınızdan açın ve [Azure aboneliğinizde oturum açma](../../xplat-cli-connect.md). 
2. Kaynak Yöneticisi modunda olduğundan emin olun.
   
    ```azurecli
    azure config mode arm
    ```
3. Zaten aşağıdaki komutu kullanarak sağlaması kaldırılıyor. sağlaması VM kapatın:
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. VM şu komutla generalize:
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. Şimdi Çalıştır **azure vm yakalama** VM yakalar komutu. Aşağıdaki örnekte, VHD ile yakalanan görüntü adları ile başlayan **MyVHDNamePrefix**ve **-t** seçeneği, şablonun yolunu belirtir **MyTemplate.json**. 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > Görüntü VHD dosyaları, varsayılan olarak özgün VM kullanılan aynı depolama hesabındaki oluşturulmasına. Kullanım *aynı depolama hesabındaki* görüntüden oluşturduğunuz yeni vm'leri VHD'lerin depolanmasını. 

6. Yakalanan görüntüye konumunu bulmak için JSON şablonunu bir metin düzenleyicisinde açın. İçinde **storageProfile**, bulma **URI** , **görüntü** bulunan **sistem** kapsayıcı. Örneğin, işletim sistemi disk görüntüsü URI'sini benzer.`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>3. adım: yakalanan görüntüden bir VM oluşturma
Şimdi görüntünün bir Linux VM oluşturmak için sahip bir şablon kullanın. Bu adımlar, Azure CLI ve yeni bir sanal ağ oluşturmak için yakalanan JSON dosyası şablonu nasıl kullanılacağını gösterir.

### <a name="create-network-resources"></a>Ağ kaynakları oluşturun
Şablonu kullanmak için önce yeni VM için bir sanal ağ ve NIC ayarlamanız gerekir. VM görüntüsü depolandığı konumda bu kaynakları için bir kaynak grubu oluşturma öneririz. Çalıştırma komutları aşağıdaki değiştirerek adları, kaynaklarınızı ve uygun bir Azure konumuna (Bu komutlarda "centralus") için benzer:

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-the-id-of-the-nic"></a>NIC kimliğini Al
Görüntüden VM yakalama sırasında kaydedilen JSON kullanarak dağıtmak için NIC kimliği gerekir. Bunu, aşağıdaki komutu çalıştırarak alın:

```azurecli
azure network nic show myResourceGroup1 myNIC
```

**Kimliği** çıktıda benzer`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`

### <a name="create-a-vm"></a>VM oluşturma
Şimdi, yakalanan VM görüntüsünü oluşturmak için aşağıdaki komutu çalıştırın. Kullanım **-f** parametresi kaydettiğiniz şablon JSON dosyasının yolunu belirtin.

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

Komut çıktısında, yeni bir VM adı, yönetici kullanıcı adı ve parola ve daha önce oluşturduğunuz NIC kimliğini sağlamanız istenir.

```bash
info:    Executing command group deployment create
info:    Supply values for the following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

Aşağıdaki örnek, başarılı bir dağıtım için gördüğünüz gösterir:

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment to complete
data:    DeploymentName     : MyDeployment
data:    ResourceGroupName  : MyResourceGroup1
data:    ProvisioningState  : Succeeded
data:    Timestamp          : xxxxxxx
data:    Mode               : Incremental
data:    Name                Type          Value

data:    ------------------  ------------  -------------------------------------

data:    vmName              String        myNewVM

data:    vmSize              String        Standard_D1

data:    adminUserName       String        myAdminuser

data:    adminPassword       SecureString  undefined

data:    networkInterfaceId  String        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
info:    group deployment create command OK
```

### <a name="verify-the-deployment"></a>Dağıtımı doğrulama
Şimdi SSH dağıtım ve yeni VM kullanarak başlangıç doğrulamak için oluşturduğunuz sanal makine için. SSH bağlanmak için aşağıdaki komutu çalıştırarak oluşturulan VM IP adresini bulun:

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

Genel IP adresi komut çıktısında listelenir. Varsayılan olarak Linux VM'ye SSH bağlantı noktası 22 tarafından bağlanır.

## <a name="create-additional-vms"></a>Ek VM oluşturma
Şablon ve yakalanan görüntü önceki bölümde yer alan adımları kullanarak ek sanal makineleri dağıtmak için kullanın. Hızlı Başlatma şablonunu kullanarak veya çalıştıran VM'ler görüntüsünü oluşturmak için diğer seçenekleri dahil **azure vm oluşturma** komutu.

### <a name="use-the-captured-template"></a>Yakalanan şablonu kullanın
Yakalanan görüntü ve şablonu kullanmak için (önceki bölümde ayrıntılı) aşağıdaki adımları izleyin:

* VM görüntüsü VM VHD barındıran aynı depolama hesabı olduğundan emin olun.
* Şablon JSON dosyasını kopyalayın ve işletim sistemi diski yeni VM'nin VHD (veya VHD) için benzersiz bir ad belirtin. Örneğin, **storageProfile**altında **vhd**, **URI**, için benzersiz bir ad belirtin **osDisk** VHD, benzer`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Bir NIC aynı veya farklı bir sanal ağ oluşturun.
* Değiştirilen şablon JSON dosyasını kullanarak, sanal ağı kümesi kaynak grubundaki bir dağıtım oluşturun.

### <a name="use-a-quickstart-template"></a>Hızlı Başlatma şablonunu kullanma
Otomatik olarak bir VM görüntüsünü oluşturduğunuzda kümesi ağ istiyorsanız, bu kaynakları bir şablonda belirtebilirsiniz. Örneğin, [101 vm gelen kullanıcı görüntüsü şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) github'dan. Bu şablon bir VM özel görüntünüzü ve gerekli sanal ağ, genel IP adresi ve NIC kaynakları oluşturur. Azure portalında şablonu kullanarak bir anlatım için bkz: [Resource Manager şablonu kullanarak özel bir görüntüden sanal makine oluşturmak nasıl](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Azure vm komutu oluşturun
Genellikle bir VM görüntüsünü oluşturmak için Resource Manager şablonu kullanmak en kolay yoludur. Ancak, VM oluşturabilirsiniz *imperatively* kullanarak **azure vm oluşturma** komutunu **-Q** (**--görüntü urn**) parametre. Bu yöntemi kullanırsanız, ayrıca geçirdiğiniz **-d** (**--işletim sistemi disk vhd**) parametresini kullanarak yeni bir VM için işletim sistemi .vhd dosyası konumunu belirtin. Bu dosya görüntü VHD dosyasının depolandığı depolama hesabını VHD'ler kapsayıcısında olması gerekir. Yeni VM için VHD komutu otomatik olarak kopyalar **VHD'ler** kapsayıcı.

Çalıştırmadan önce **azure vm oluşturma** görüntüsüyle aşağıdaki adımları tamamlayın:

1. Bir kaynak grubu oluşturun veya varolan bir kaynak grubu dağıtımı için belirleyin.
2. Yeni VM için genel bir IP adresi kaynağı ve NIC kaynağı oluşturun. CLI kullanarak bir sanal ağ, genel IP adresi ve NIC oluşturma adımları için bu makalenin önceki bölümlerinde bkz. (**azure vm oluşturma** bir NIC de oluşturabilirsiniz, ancak bir sanal ağ ve alt ağ için ek parametreler gerekir.)

Daha sonra yeni işletim sistemi VHD dosyasının hem mevcut görüntü URI'ler geçirmeden bir komutu çalıştırın. Bu örnekte, bir boyut Doğu ABD bölgesinde Standard_A1 VM oluşturulur.

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

Ek komut seçeneklerini çalıştırmak `azure help vm create`.

## <a name="next-steps"></a>Sonraki adımlar
Vm'leriniz CLI ile yönetmek için görevleri görmek [dağıtma ve Azure Resource Manager şablonları ve Azure CLI kullanarak sanal makineleri yönetme](create-ssh-secured-vm-from-template.md).

