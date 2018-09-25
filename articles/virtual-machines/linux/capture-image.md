---
title: Azure CLI kullanarak azure'da bir Linux VM görüntüsü yakalama | Microsoft Docs
description: Azure CLI kullanarak toplu dağıtımları için kullanmak üzere bir Azure VM görüntüsü yakalayın.
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
ms.date: 03/22/2018
ms.author: cynthn
ms.openlocfilehash: 98d98c1337830ce54c7ff96c19812169be129584
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946825"
---
# <a name="how-to-create-an-image-of-a-virtual-machine-or-vhd"></a>Bir sanal makine veya VHD görüntüsü oluşturma

<!-- generalize, image - extended version of the tutorial-->

Bir sanal makine (VM) azure'da birden çok kopyasını oluşturmak için VM veya işletim sistemi VHD'si bir görüntüsünü yakalayın. Bir görüntü oluşturmak için birden çok kez dağıtmak daha güvenli hale getiren bir kişisel hesap bilgilerinizi kaldırmanız. Aşağıdaki adımlarda serbest bırakın ve görüntü oluşturma mevcut bir VM'nin sağlamasını kaldırmak. Bu görüntü, VM'ler, aboneliğinizde arasında herhangi bir kaynak grubu oluşturmak için kullanabilirsiniz.

İstiyorsanız, var olan bir Linux VM yedekleme veya hata ayıklama için bir kopyasını oluşturmak veya bir şirket içi VM'den özelleştirilmiş bir Linux VHD'yi karşıya yükleme, bkz: [karşıya yükleme ve özel disk görüntüsünden Linux VM oluşturma](upload-vhd.md).  

Ayrıca **Packer** özel yapılandırmanızı oluşturmak için. Packer kullanarak daha fazla bilgi için bkz: [Packer Azure'da Linux sanal makine görüntüleri oluşturmak için nasıl kullanılacağını](build-image-with-packer.md).


## <a name="before-you-begin"></a>Başlamadan önce
Aşağıdaki önkoşulları karşıladığından emin olun:

* Azure yönetilen diskleri kullanarak Resource Manager dağıtım modelinde oluşturulan VM ihtiyacınız vardır. Bir Linux VM oluşturmadıysanız, kullanabileceğiniz [portalı](quick-create-portal.md), [Azure CLI](quick-create-cli.md), veya [Resource Manager şablonları](create-ssh-secured-vm-from-template.md). VM, gerektiği şekilde yapılandırın. Örneğin, [veri diskleri ekleme](add-disk.md)güncelleştirmelerini uygulamak ve uygulamaları yükleyin. 

* Ayrıca en son gerekir [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açma [az login](/cli/azure/reference-index#az_login).

## <a name="quick-commands"></a>Hızlı komutlar

Test için bu konu, Basitleştirilmiş bir sürümünü değerlendirmek veya azure'da sanal makineler öğrenmeye bkz [özel bir Azure CLI kullanarak bir VM görüntüsü oluşturmak](tutorial-custom-images.md).


## <a name="step-1-deprovision-the-vm"></a>1. adım: sanal Makinenin sağlamasını kaldırma
Size, makine belirli dosyaları ve verileri silmek için Azure VM Aracısı'nı kullanarak, VM'nin sağlamasını kaldırın. Kullanım `waagent` komutunu *-sağlamayı kaldırma + kullanıcı* kaynak Linux VM'NİZİN parametresi. Daha fazla bilgi için bkz. [Azure Linux Aracısı kullanıcı kılavuzu](../extensions/agent-linux.md).

1. Bir SSH istemcisi kullanarak Linux VM'nize bağlanın.
2. SSH penceresinde aşağıdaki komutu yazın:
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > Yalnızca bir VM'de bir görüntü olarak yakalama istiyorsanız şu komutu çalıştırın. Görüntü tüm önemli bilgileri temizlenir veya yeniden dağıtım için uygun garanti etmez. *+ Kullanıcı* parametresi, son sağlanan kullanıcı hesabı da kaldırır. Hesap kimlik bilgilerini VM tutmak istiyorsanız, kullanmanız yeterlidir *-sağlamayı kaldırma* kullanıcı hesabının makineyi bırakabilirsiniz.
 
3. Tür **y** devam etmek için. Ekleyebileceğiniz **-force** bu doğrulama adımı önlemek için parametre.
4. Komut tamamlandıktan sonra yazın **çıkmak**. Bu adım, SSH istemcisi kapanır.

## <a name="step-2-create-vm-image"></a>2. adım: Sanal makine görüntüsü oluşturma
VM genelleştirilmiş olarak işaretleme ve görüntü yakalamak için Azure CLI'yı kullanın. Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *myVnet*, ve *myVM*.

1. İle sağlaması VM'yi serbest bırakın [az vm deallocate](/cli//azure/vm#deallocate). Aşağıdaki örnekte adlı VM serbest bırakılır *myVM* adlı kaynak grubunda *myResourceGroup*:
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. VM genelleştirilmiş olarak işaretleme [az vm generalize](/cli//azure/vm#generalize). Aşağıdaki örnekte adlı VM işaretler *myVM* adlı kaynak grubunda *myResourceGroup* genelleştirilmiş olarak:
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. Şimdi VM kaynağının ile görüntü oluşturma [az görüntü oluşturma](/cli/azure/image#az_image_create). Aşağıdaki örnekte adlı bir görüntü oluşturur *Myımage* adlı kaynak grubunda *myResourceGroup* adlı VM kaynağını kullanarak *myVM*:
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > Görüntü, kaynak VM ile aynı kaynak grubunda oluşturulur. Bu görüntüden, aboneliğiniz kapsamındaki herhangi bir kaynak grubunda VM'ler oluşturabilirsiniz. Yönetim açısından bakıldığında, VM kaynakları ve resimler için belirli bir kaynak grubu oluşturmak isteyebilirsiniz.
   >
   > Görüntünüzü bölge dayanıklı depolama birimine depolamak istediğiniz desteklediği bir bölgede oluşturmanız gerekirse [kullanılabilirlik](../../availability-zones/az-overview.md) ve `--zone-resilient true` parametresi.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>3. adım: yakalanan görüntüden VM oluşturma
İle oluşturulan görüntüyü kullanarak bir VM oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnekte adlı bir VM oluşturur *myVMDeployed* adlı görüntüden *Myımage*:

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-the-vm-in-another-resource-group"></a>Başka bir kaynak grubunda VM oluşturma 

Aboneliğinizde herhangi bir kaynak grubunda bir görüntüden VM'ler oluşturabilirsiniz. Resimden farklı bir kaynak grubunda bir VM oluşturmak için görüntüye tam kaynak Kimliğini belirtin. Kullanım [az görüntü listesi](/cli/azure/image#az_image_list) görüntülerin listesini görüntülemek için. Çıktı aşağıdaki örneğe benzer:

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

Aşağıdaki örnekte [az vm oluşturma](/cli/azure/vm#az_vm_create) görüntü kaynak kimliği'ni belirleyerek bir kaynak görüntüden farklı bir kaynak grubundaki bir VM oluşturmak için:

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-the-deployment"></a>4. adım: dağıtımı doğrulama

Artık, yeni VM kullanmaya başlayın ve dağıtımı doğrulamak için oluşturduğunuz sanal makineye SSH. IP adresi veya FQDN'si ile sanal makinenize SSH bağlanmak için bulma [az vm show](/cli/azure/vm#az_vm_show):

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Sonraki adımlar
Kaynak sanal makine görüntüsünden birden çok VM oluşturabilirsiniz. Görüntünüzü için değişiklik yapmanız gerekiyorsa: 

- Görüntünüze bir VM oluşturun.
- Herhangi bir güncelleştirme veya yapılandırma değişikliklerini yapın.
- Yeniden sağlamasını kaldırma ve serbest bırakın, generalize ve görüntü oluşturma adımlarını izleyin.
- Bu yeni görüntüyü gelecekteki dağıtımlar için kullanın. İsterseniz, orijinal görüntüyü silin.

Vm'lerinizi CLI ile yönetme ile ilgili daha fazla bilgi için bkz: [Azure CLI](/cli/azure).
