---
title: "Azure CLI ile özel VM görüntüleri oluşturma | Microsoft Docs"
description: "Öğretici - Azure CLI kullanarak özel bir VM görüntüsü oluşturun."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: e73494ff4827b74cbb42b2b0f1f9738c78960e23
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-the-cli"></a>Azure CLI kullanarak bir VM'nin özel bir görüntü oluşturun

Özel resimler gibi Market görüntülerini olsa da, bunları kendiniz oluşturmanız. Özel resimler, uygulamalar, uygulama yapılandırmaları ve diğer işletim sistemi yapılandırmalarını önceden gibi önyükleme yapılandırmaları için kullanılabilir. Bu öğreticide, kendi özel görüntünüzü bir Azure sanal makine oluşturun. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Yetkisini kaldırma ve sanal makineleri generalize
> * Özel görüntü oluşturma
> * Özel bir görüntüden bir VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listesi
> * Bir görüntü Sil


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki adımlar mevcut bir VM'yi almak ve yeni VM örnekleri oluşturmak için kullanabileceğiniz yeniden kullanılabilir bir özel görüntü açmak nasıl ayrıntılı olarak açıklanmaktadır.

Örneğin bu öğreticiyi tamamlamak için var olan bir sanal makine olması gerekir. Gerekirse, bu [komut dosyası örneği](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) sizin için bir tane oluşturabilirsiniz. Çalışma öğretici aracılığıyla değiştirdiğinizde VM ve kaynak grubu adları gerektiğinde.

## <a name="create-a-custom-image"></a>Özel görüntü oluşturma

Bir sanal makinenin bir görüntü oluşturmak için VM sağlama kaldırmayı, serbest bırakma ve genelleştirilmiş gibi VM kaynak işaretleme hazırlamanız gerekir. VM hazır olduktan sonra bir görüntü oluşturabilirsiniz.

### <a name="deprovision-the-vm"></a>VM yetkisini kaldırma 

Sağlamayı kaldırmayı VM makineye özgü bilgiler kaldırarak genelleştirir. Bu Genelleştirme tek bir görüntüden birçok VM dağıtmak mümkün kılar. Sağlama kaldırma sırasında ana bilgisayar adı sıfırlanır *localhost.localdomain*. SSH ana bilgisayar anahtarları, ad sunucusu yapılandırmaları, kök parolasını ve önbelleğe alınan DHCP kiralarını de silinir.

VM yetkisini kaldırma için Azure VM Aracısı (waagent) kullanın. Azure VM Aracısı VM üzerinde yüklü ve sağlama ve Azure yapı denetleyicisi ile etkileşim yönetir. Daha fazla bilgi için bkz: [Azure Linux Aracısı Kullanıcı Kılavuzu](agent-user-guide.md).

SSH kullanarak VM'nize bağlanmak ve VM yetkisini kaldırma için komutu çalıştırın. İle `+user` bağımsız değişken, son sağlanan kullanıcı hesabı ve tüm ilişkili veriler de silinir. Örnek IP adresinin, VM ortak IP adresiyle değiştirin.

SSH VM.
```bash
ssh azureuser@52.174.34.95
```
VM sağlamayı sonlandırın.

```bash
sudo waagent -deprovision+user -force
```
SSH oturumu kapatın.

```bash
exit
```

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Deallocate ve VM genelleştirilmiş olarak işaretle

Bir görüntü oluşturmak için VM serbest gerekir. Kullanarak VM serbest bırakma [az vm serbest bırakma](/cli//azure/vm#deallocate). 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

Son olarak, VM durumu ile genelleştirilmiş olarak ayarlamak [az vm generalize](/cli//azure/vm#generalize) VM genelleştirilmiş Azure platformu bilmesi için. Yalnızca genelleştirilmiş bir sanal makineden bir görüntü oluşturabilirsiniz.
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-the-image"></a>Görüntü oluşturma

VM görüntüsü kullanarak oluşturabileceğiniz artık [az görüntü oluşturma](/cli//azure/image#create). Aşağıdaki örnek adlı bir resim oluşturur *myImage* adlı bir VM'den *myVM*.
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-the-image"></a>Sanal makineleri görüntüden oluşturma

Bir görüntü sahip olduğunuza göre görüntü kullanarak bir veya daha fazla yeni VM'ler oluşturabilirsiniz [az vm oluşturma](/cli/azure/vm#create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVMfromImage* adlı görüntüden *myImage*.

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a>Görüntü Yönetimi 

Genel görüntü yönetim görevleri ve bunları Azure CLI kullanarak tamamlamak nasıl bazı örnekleri aşağıda verilmiştir.

Tablo biçiminde ada göre tüm görüntüleri listeleyin.

```azurecli-interactive 
az image list \
    --resource-group myResourceGroup
```

Görüntüyü silin. Bu örnek adlı görüntü siler *myOldImage* gelen *myResourceGroup*.

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel bir VM görüntüsü oluşturuldu. Şunları öğrendiniz:

> [!div class="checklist"]
> * Yetkisini kaldırma ve sanal makineleri generalize
> * Özel görüntü oluşturma
> * Özel bir görüntüden bir VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listesi
> * Bir görüntü Sil

Yüksek oranda kullanılabilir sanal makineler hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Yüksek oranda kullanılabilir sanal makineleri oluşturmak](tutorial-availability-sets.md).

