---
title: "CLI 2.0 kullanarak azure'da bir Linux VM bir görüntüsünü yakalama | Microsoft Docs"
description: "Azure CLI 2.0 kullanarak yığın dağıtımları için kullanmak üzere bir Azure VM görüntüsünü yakalayın."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 15ad240ea9b635cd7995bfae403a93e0b392850a
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="how-to-create-an-image-of-a-virtual-machine-or-vhd"></a>Bir sanal makine ya da VHD görüntüsü oluşturma

<!-- generalize, image - extended version of the tutorial-->

Bir sanal makine Azure'da kullanmak için (VM) birden çok kopyasını oluşturmak için VM veya işletim sistemi VHD bir görüntüsünü yakalayın. Bir görüntü oluşturmak için birden çok kez dağıtmak daha güvenli hale getiren kişisel hesap bilgileri kaldırmanız. Aşağıdaki adımlarda, mevcut bir VM'yi yetkisini kaldırma, ayırması ve görüntü oluşturma. Bu görüntü, VM'ler aboneliğinizi içinde arasında herhangi bir kaynak grubu oluşturmak için kullanabilirsiniz.

İstiyorsanız, var olan bir Linux VM için yedekleme veya hata ayıklama, bir kopyasını oluşturun veya bir şirket içi VM özelleştirilmiş bir Linux VHD'den karşıya yükleme, bkz: [karşıya yükleme ve özel disk görüntüsünden bir Linux VM oluşturma](upload-vhd.md).  

Aynı zamanda **Packer** özel yapılandırma oluşturmak için. Packer kullanma hakkında daha fazla bilgi için bkz: [Packer Linux sanal makine görüntülerini oluşturmak için nasıl kullanılacağını](build-image-with-packer.md).


## <a name="before-you-begin"></a>Başlamadan önce
Aşağıdaki önkoşulları karşıladığından emin olun:

* Azure yönetilen diskleri kullanılarak Resource Manager dağıtım modelinde oluşturulmuş VM'deki gerekir. Bir Linux VM oluşturmadıysanız, kullanabileceğiniz [portal](quick-create-portal.md), [Azure CLI](quick-create-cli.md), veya [Resource Manager şablonları](create-ssh-secured-vm-from-template.md). VM gerektiği gibi yapılandırın. Örneğin, [veri diski Ekle](add-disk.md)güncelleştirmeleri uygulamak ve uygulamaları yükleyin. 

* Ayrıca son sahip olmanız gerekir [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmanız [az oturum açma](/cli/azure/reference-index#az_login).

## <a name="quick-commands"></a>Hızlı komutlar

Değerlendirme veya azure'da VM öğrenmeye, test, bu konu, Basitleştirilmiş bir sürümünü için bkz: [Azure CLI kullanarak bir VM'nin özel bir görüntü oluşturun](tutorial-custom-images.md).


## <a name="step-1-deprovision-the-vm"></a>1. adım: VM yetkisini kaldırma
Makine belirli dosyaları ve verileri silmek için Azure VM Aracısı'nı kullanarak VM'yi sağlamayı sonlandırın. Kullanım `waagent` komutunu *-deprovision + kullanıcı* kaynağınız Linux VM parametresi. Daha fazla bilgi için bkz. [Azure Linux Aracısı kullanıcı kılavuzu](../windows/agent-user-guide.md).

1. Bir SSH istemcisi kullanarak Linux VM'NİZDE bağlayın.
2. SSH penceresinde aşağıdaki komutu yazın:
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > Yalnızca resim olarak yakalamasına düşündüğünüz bir VM'de bu komutu çalıştırın. Görüntü tüm hassas bilgilerin temizlenmiş veya yeniden dağıtım için uygun garanti etmez. *+ Kullanıcı* parametresi son sağlanan kullanıcı hesabının da kaldırır. VM'yi hesap kimlik bilgilerini tutmak istiyorsanız, yalnızca kullanmak *-deprovision* kullanıcı hesabı bırakın için.
 
3. Tür **y** devam etmek için. Ekleyebileceğiniz **-force** bu onay adım önlemek için parametre.
4. Komut tamamlandığında yazın **çıkmak**. Bu adım, SSH istemcisi kapatır.

## <a name="step-2-create-vm-image"></a>2. adım: VM görüntüsü oluşturma
VM genelleştirilmiş olarak işaretler ve görüntü yakalamak için Azure CLI 2.0 kullanın. Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *myVnet*, ve *myVM*.

1. İle sağlaması kaldırılıyor. sağlaması VM serbest bırakma [az vm serbest bırakma](/cli//azure/vm#deallocate). Aşağıdaki örnek adlı VM kaldırır *myVM* kaynak grubunda adlı *myResourceGroup*:
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. VM ile genelleştirilmiş olarak işaretle [az vm generalize](/cli//azure/vm#generalize). Aşağıdaki örnek işaretleri adlı VM *myVM* kaynak grubunda adlı *myResourceGroup* genelleştirilmiş olarak:
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. Şimdi ile VM kaynak görüntüsü oluşturma [az görüntü oluşturma](/cli/azure/image#az_image_create). Aşağıdaki örnek adlı bir resim oluşturur *myImage* kaynak grubunda adlı *myResourceGroup* adlı VM kaynak kullanarak *myVM*:
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > Görüntü, kaynak VM ile aynı kaynak grubunda oluşturulur. Bu görüntü aboneliğinizden içinde herhangi bir kaynak grubunda sanal makineleri oluşturabilirsiniz. Yönetim açısından bakıldığında, belirli bir kaynak grubunun VM kaynakları ve görüntüleri oluşturmak isteyebilirsiniz.

## <a name="step-3-create-a-vm-from-the-captured-image"></a>3. adım: yakalanan görüntüden bir VM oluşturma
Oluşturduğunuz ile görüntü kullanarak bir VM oluşturma [az vm oluşturma](/cli/azure/vm#az_vm_create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVMDeployed* adlı görüntüden *myImage*:

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-the-vm-in-another-resource-group"></a>Başka bir kaynak grubunda VM oluşturma 

Aboneliğinizi içinde herhangi bir kaynak grubunda bir görüntüden sanal makineleri oluşturabilirsiniz. Farklı bir kaynak grubu görüntünün bir VM oluşturmak için görüntünüze tam kaynak kimliği belirtin. Kullanım [az resim listesi](/cli/azure/image#az_image_list) görüntüleri listesini görüntülemek için. Çıktı aşağıdaki örneğe benzer:

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

Aşağıdaki örnek kullanır [az vm oluşturma](/cli/azure/vm#az_vm_create) görüntü kaynak kimliği belirterek bir VM kaynak görüntü daha farklı bir kaynak grubu oluşturmak için:

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-the-deployment"></a>4. adım: dağıtımı doğrulama

Şimdi SSH dağıtım ve yeni VM kullanarak başlangıç doğrulamak için oluşturduğunuz sanal makine için. SSH bağlanmak için IP adresi veya FQDN ile VM Bul [az vm Göster](/cli/azure/vm#az_vm_show):

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Sonraki adımlar
Kaynak VM görüntüden birden çok VM oluşturabilirsiniz. Görüntüye değişiklik yapmanız gerekirse: 

- Bir VM görüntünüze oluşturun.
- Tüm güncelleştirmeler veya yapılandırma değişikliklerini yapın.
- Yeniden sağlamayı sonlandırın, serbest bırakma, generalize ve görüntü oluşturma adımlarını izleyin.
- Bu yeni görüntüyü gelecekteki dağıtımlar için kullanın. İsterseniz, orijinal görüntüyü silin.

Vm'leriniz CLI ile yönetme ile ilgili daha fazla bilgi için bkz: [Azure CLI 2.0](/cli/azure).
