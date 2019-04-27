---
title: Azure CLI kullanarak azure'da bir Linux VM görüntüsü yakalama | Microsoft Docs
description: Yığın dağıtımları için Azure CLI kullanarak bir Azure VM görüntüsü yakalayın.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/08/2018
ms.author: cynthn
ms.openlocfilehash: 5022d765b5dfa4f1f973b7fb4370d5314bb887b8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60542887"
---
# <a name="how-to-create-an-image-of-a-virtual-machine-or-vhd"></a>Bir sanal makine veya VHD görüntüsü oluşturma

<!-- generalize, image - extended version of the tutorial-->

Azure'da kullanım için bir sanal makine (VM) birden çok kopyasını oluşturmak için VM veya işletim sistemi VHD'si, görüntü yakalama. Dağıtım için bir görüntü oluşturmak için kişisel hesap bilgilerinizi kaldırmak gerekir. Aşağıdaki adımlarda bunu serbest bırakın ve görüntü oluşturma mevcut bir VM'nin sağlamasını kaldırmak. Bu görüntü, VM'ler, aboneliğinizde arasında herhangi bir kaynak grubu oluşturmak için kullanabilirsiniz.

Yedekleme veya hata ayıklama için var olan bir Linux sanal makinenizin bir kopya oluşturmak için veya bir şirket içi VM'den özelleştirilmiş bir Linux VHD'yi karşıya yüklemek için bkz: [karşıya yükleme ve özel disk görüntüsünden Linux VM oluşturma](upload-vhd.md).  

Ayrıca **Packer** özel yapılandırmanızı oluşturmak için. Daha fazla bilgi için [Packer Azure'da Linux sanal makine görüntüleri oluşturmak için nasıl kullanılacağını](build-image-with-packer.md).

Görüntüyü oluşturmadan önce aşağıdaki öğeler gerekir:

* Azure yönetilen diskler kullanan Resource Manager dağıtım modelinde oluşturulan VM. Bir Linux VM henüz oluşturmadıysanız, kullanabileceğiniz [portalı](quick-create-portal.md), [Azure CLI](quick-create-cli.md), veya [Resource Manager şablonları](create-ssh-secured-vm-from-template.md). VM, gerektiği şekilde yapılandırın. Örneğin, [veri diskleri ekleme](add-disk.md)güncelleştirmelerini uygulamak ve uygulamaları yükleyin. 

* En son [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı ile oturum açması [az login](/cli/azure/reference-index#az-login).

## <a name="quick-commands"></a>Hızlı komutlar

Bu makale basitleştirilmiş bir sürümünü ve test etme, değerlendirme veya azure'da sanal makineler öğrenmeye bkz [CLI kullanarak Azure VM'deki özel görüntüsünü oluşturma](tutorial-custom-images.md).


## <a name="step-1-deprovision-the-vm"></a>1. Adım: VM’nin sağlamasını kaldırma
İlk sanal makineye özgü dosyaları ve verileri silmek için Azure VM aracısını kullanarak sağlamasını. Kullanım `waagent` komutunu `-deprovision+user` kaynak Linux VM'NİZİN parametresi. Daha fazla bilgi için bkz. [Azure Linux Aracısı kullanıcı kılavuzu](../extensions/agent-linux.md).

1. Bir SSH istemcisi kullanarak Linux VM'nize bağlanın.
2. SSH penceresinde aşağıdaki komutu girin:
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > Yalnızca bir VM'de bir görüntü olarak yakalama bu komutu çalıştırın. Bu komut, görüntünün tüm hassas bilgilerin temizlenmiş veya yeniden dağıtım için uygun garanti etmez. `+user` Parametresi, son sağlanan kullanıcı hesabı da kaldırır. VM kullanıcı hesabı kimlik bilgileri korumak için yalnızca kullanın `-deprovision`.
 
3. Girin **y** devam etmek için. Ekleyebileceğiniz `-force` bu doğrulama adımı önlemek için parametre.
4. Komut tamamlandıktan sonra girin **çıkmak** SSH İstemcisi'ni kapatın.

## <a name="step-2-create-vm-image"></a>2. Adım: VM görüntüsü oluşturma
VM genelleştirilmiş olarak işaretleme ve görüntü yakalamak için Azure CLI'yı kullanın. Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *myVnet*, ve *myVM*.

1. İle sağlaması VM'yi serbest bırakın [az vm deallocate](/cli/azure/vm). Aşağıdaki örnekte adlı VM serbest bırakılır *myVM* adlı kaynak grubunda *myResourceGroup*.
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. VM genelleştirilmiş olarak işaretleme [az vm generalize](/cli/azure/vm). Aşağıdaki örnekte adlı VM işaretler *myVM* adlı kaynak grubunda *myResourceGroup* genelleştirilmiş olarak.
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. Kaynak VM görüntüsü oluşturma [az görüntü oluşturma](/cli/azure/image#az-image-create). Aşağıdaki örnekte adlı bir görüntü oluşturur *Myımage* adlı kaynak grubunda *myResourceGroup* adlı VM kaynağını kullanarak *myVM*.
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > Görüntü, kaynak VM ile aynı kaynak grubunda oluşturulur. Bu görüntüden, aboneliğiniz kapsamındaki herhangi bir kaynak grubunda VM'ler oluşturabilirsiniz. Yönetim açısından bakıldığında, VM kaynakları ve resimler için belirli bir kaynak grubu oluşturmak isteyebilirsiniz.
   >
   > Görüntünüzü bölge dayanıklı depolama birimine depolamak istediğiniz desteklediği bir bölgede oluşturmanız gerekirse [kullanılabilirlik](../../availability-zones/az-overview.md) ve `--zone-resilient true` parametresi.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>3. Adım: Yakalanan görüntüden VM oluşturma
İle oluşturduğunuz görüntüsünü kullanarak VM oluşturma [az vm oluşturma](/cli/azure/vm). Aşağıdaki örnekte adlı bir VM oluşturur *myVMDeployed* adlı görüntüden *Myımage*.

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-the-vm-in-another-resource-group"></a>Başka bir kaynak grubunda VM oluşturma 

Aboneliğinizde herhangi bir kaynak grubunda bir görüntüden VM'ler oluşturabilirsiniz. Resimden farklı bir kaynak grubunda bir VM oluşturmak için görüntüye tam kaynak Kimliğini belirtin. Kullanım [az görüntü listesi](/cli/azure/image#az-image-list) görüntülerin listesini görüntülemek için. Çıktı aşağıdaki örneğe benzerdir.

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

Aşağıdaki örnekte [az vm oluşturma](/cli/azure/vm#az-vm-create) görüntü kaynak kimliği belirtilerek bir kaynak grubunda kaynak görüntüyü dışındaki bir VM oluşturmak için

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-the-deployment"></a>4. Adım: Dağıtımı doğrulama

Yeni VM kullanmaya başlayın ve dağıtımı doğrulamak için oluşturduğunuz sanal makineye SSH. IP adresi veya FQDN'si ile sanal makinenize SSH bağlanmak için bulma [az vm show](/cli/azure/vm#az-vm-show).

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Sonraki adımlar
Kaynak sanal makine görüntüsünden birden çok VM oluşturabilirsiniz. Görüntüye değişiklik yapmak için: 

- Görüntünüze bir VM oluşturun.
- Herhangi bir güncelleştirme veya yapılandırma değişikliklerini yapın.
- Yeniden sağlamasını kaldırma ve serbest bırakın, generalize ve görüntü oluşturma adımlarını izleyin.
- Bu yeni görüntüyü gelecekteki dağıtımlar için kullanın. Özgün resmin silebilirsiniz.

Vm'lerinizi CLI ile yönetme ile ilgili daha fazla bilgi için bkz: [Azure CLI](/cli/azure).
