---
title: include dosyası
description: include dosyası
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 04/30/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 7e4ca54d8f97646192d19d5923bee24a906e8df7
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149696"
---
## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/bash](https://shell.azure.com/bash) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede örneği tamamlamak için bir genelleştirilmiş VM'nin yönetilen görüntüsü mevcut olmalıdır. Daha fazla bilgi için [Öğreticisi: Azure CLI 2.0 ile Azure VM'deki özel görüntüsünü oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). 


## <a name="create-an-image-gallery"></a>Bir görüntü Galerisi oluşturma 

Bir görüntü Galerisine görüntü paylaşımına etkinleştirmek için kullanılan birincil kaynaktır. Galeri adı için izin verilen karakterler büyük veya küçük harf, rakam, nokta ve dönemleri olur. Galeri adı kısa çizgi içeremez.   Galeri adları, abonelik içinde benzersiz olmalıdır. 

Kullanarak bir görüntü Galerisi oluşturma [az sig oluşturma](/cli/azure/sig#az-sig-create). Aşağıdaki örnekte adlı bir galeridir oluşturur *myGallery* içinde *myGalleryRG*.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create -g myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>Bir görüntü tanımı oluşturun

Resimler için mantıksal bir gruplandırmasını görüntü tanımları oluşturun. Bunlar, bunların içinde oluşturulan görüntü sürümleri hakkında bilgi yönetmek için kullanılır. Görüntü tanımı adları büyük veya küçük harf, rakam, nokta, tire ve nokta oluşur. Bir görüntü tanımı için belirtebileceğiniz değerler hakkında daha fazla bilgi için bkz. [görüntü tanımları](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#image-definitions).

Galeri kullanarak azure'da ilk görüntü tanımı oluşturma [az sig görüntü tanımı oluşturma](/cli/azure/sig/image-definition#az-sig-image-definition-create).

```azurecli-interactive 
az sig image-definition create \
   -g myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku 16.04-LTS \
   --os-type Linux 
```


## <a name="create-an-image-version"></a>Görüntü sürümü oluşturma 

Görüntünün sürümü kullanarak gerektiği gibi oluşturma [az görüntü Galerisi oluşturma-görüntü-version](/cli/azure/sig/image-version#az-sig-image-version-create). Görüntü sürümü oluşturmak için temel olarak kullanmak için yönetilen bir görüntü kimliği geçirin gerekecektir. Kullanabileceğiniz [az görüntü listesi](/cli/azure/image?view#az-image-list) bir kaynak grubunda bulunan görüntüleri hakkında daha fazla bilgi edinmek için. 

Görüntü sürümü için izin verilen karakter, sayı ve dönemleri ' dir. Sayı 32-bit tamsayı aralığında olmalıdır. Biçim: *MajorVersion*. *MinorVersion*. *Düzeltme Eki*.

Bu örnekte, görüntümüzü sürümüdür *1.0.0* ve 2 çoğaltma oluşturmak için kullanacağız *Batı Orta ABD* bölge, 1 yinelemede *Orta Güney ABD* bölge ve 1 yinelemedeki *Doğu ABD 2* bölge.


```azurecli-interactive 
az sig image-version create \
   -g myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "WestCentralUS" "SouthCentralUS=1" "EastUS2=1" \
   --replica-count 2 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

> [!NOTE]
> Görüntü sürümü yerleşik ve başka bir görüntü sürümünü oluşturmak için aynı yönetilen görüntüsünü kullanabilmeniz için önce çoğaltılmış tamamen tamamlanmasını beklemeniz gerekir.