---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 82187b05a398c066f9da94c57cbe8a59a6ba3275
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145785"
---
## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.


## <a name="get-the-managed-image"></a>Yönetilen bir görüntü al

Kullanarak bir kaynak grubu mevcut görüntülerin listesini görebilirsiniz [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage). Görüntü adı ve hangi kaynak grubunda öğrendikten sonra olduğundan, kullanabileceğiniz `Get-AzImage` yeniden görüntü nesnesini alın ve daha sonra kullanmak üzere bir değişkende depolayın. Bu örnek adlı bir görüntü alır *Myımage* "myResourceGroup" kaynak grubu ve bir değişkene atar *$managedImage*. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Bir görüntü Galerisi oluşturma 

Bir görüntü Galerisine görüntü paylaşımına etkinleştirmek için kullanılan birincil kaynaktır. Galeri adı için izin verilen karakterler büyük veya küçük harf, rakam, nokta ve dönemleri olur. Galeri adı kısa çizgi içeremez. Galeri adları, abonelik içinde benzersiz olmalıdır. 

Kullanarak bir görüntü Galerisi oluşturma [yeni AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery). Aşağıdaki örnekte adlı bir galeridir oluşturur *myGallery* içinde *myGalleryRG* kaynak grubu.

```azurepowershell-interactive
$resourceGroup = New-AzResourceGroup `
   -Name 'myGalleryRG' `
   -Location 'West Central US'  
$gallery = New-AzGallery `
   -GalleryName 'myGallery' `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -Description 'Shared Image Gallery for my organization'  
```
   
## <a name="create-an-image-definition"></a>Bir görüntü tanımı oluşturun 

Resimler için mantıksal bir gruplandırmasını görüntü tanımları oluşturun. Bunlar, bunların içinde oluşturulan görüntü sürümleri hakkında bilgi yönetmek için kullanılır. Görüntü tanımı adları büyük veya küçük harf, rakam, nokta, kısa çizgi ve dönemleri meydana gelebilir. Bir görüntü tanımı için belirtebileceğiniz değerler hakkında daha fazla bilgi için bkz. [görüntü tanımları](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Kullanarak görüntü tanımı oluşturabilir [yeni AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). Bu örnekte, Galeri görüntüsü adlı *myGalleryImage*.

```azurepowershell-interactive
$galleryImage = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```


## <a name="create-an-image-version"></a>Görüntü sürümü oluşturma

Görüntü sürümü kullanarak bir yönetilen görüntüsünü oluşturma [yeni AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). 

Görüntü sürümü için izin verilen karakter, sayı ve dönemleri ' dir. Sayı 32-bit tamsayı aralığında olmalıdır. Biçim: *MajorVersion*. *MinorVersion*. *Düzeltme Eki*.

Bu örnekte, görüntü sürümü olan *1.0.0* ve her ikisi de çoğaltılır *Batı Orta ABD* ve *Orta Güney ABD* veri merkezleri. Çoğaltma için hedef bölgeler seçerken, ayrıca eklemek zorunda olmadığını unutmayın *kaynak* çoğaltma için hedef bölgede.


```azurepowershell-interactive
$region1 = @{Name='South Central US';ReplicaCount=1}
$region2 = @{Name='West Central US';ReplicaCount=2}
$targetRegions = @($region1,$region2)
$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -TargetRegion $targetRegions  `
   -Source $managedImage.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-01-01' `
   -asJob 
```

Uygulamanın, biz ilerleme durumunu izleyebilmek bir işi oluşturduk şekilde görüntünün hedef bölgeler tümüne biraz sürebilir. İşinin ilerleme durumunu görmek için şunu yazın `$job.State`.

```azurepowershell-interactive
$job.State
```

> [!NOTE]
> Görüntü sürümü yerleşik ve başka bir görüntü sürümünü oluşturmak için aynı yönetilen görüntüsünü kullanabilmeniz için önce çoğaltılmış tamamen tamamlanmasını beklemeniz gerekir. 
>
> Görüntü sürümünüzde da depolayabilirsiniz [bölgesel olarak yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) ekleyerek `-StorageAccountType Standard_ZRS` oluşturduğunuzda görüntü sürümü.
>
