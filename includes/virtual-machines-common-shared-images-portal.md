---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/29/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 9b9b6d08fd14a850838590ce003e889e8e652c7c
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148059"
---
## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

> [!NOTE]
> Önizleme sırasında paylaşılan resim galerileri kullanmak için kayıtlı, yeniden kaydetmeniz gerekebilir `Microsoft.Compute` sağlayıcısı. Açık [Cloud Shell](https://shell.azure.com/bash) ve türü: `az provider register -n Microsoft.Compute`

## <a name="create-an-image-gallery"></a>Bir görüntü Galerisi oluşturma

Bir görüntü Galerisine görüntü paylaşımına etkinleştirmek için kullanılan birincil kaynaktır. Galeri adı için izin verilen karakterler büyük veya küçük harf, rakam, nokta ve dönemleri olur. Galeri adı kısa çizgi içeremez.  Galeri adları, abonelik içinde benzersiz olmalıdır. 

Aşağıdaki örnekte adlı bir galeridir oluşturur *myGallery* içinde *myGalleryRG* kaynak grubu.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. Türü kullanmak **paylaşılan görüntü Galerisi** seçip arama kutusuna **paylaşılan görüntü Galerisi** sonuçları.
1. İçinde **paylaşılan görüntü Galerisi** sayfasında **Oluştur**.
1. Doğru aboneliği seçin.
1. İçinde **kaynak grubu**seçin **Yeni Oluştur** ve türü *myGalleryRG* adı.
1. İçinde **adı**, türü *myGallery* galeri adı.
1. İçin varsayılan değeri bırakın **bölge**.
1. Gibi galeri kısa bir açıklamasını yazın *görüntü Galerim test etmek için.* ve ardından **gözden geçir + Oluştur**.
1. Doğrulama denetimini geçtikten seçin **Oluştur**.
1. Dağıtım tamamlandığında seçin **kaynağa Git**.
   
## <a name="create-an-image-definition"></a>Bir görüntü tanımı oluşturun 

Resimler için mantıksal bir gruplandırmasını görüntü tanımları oluşturun. Bunlar, bunların içinde oluşturulan görüntü sürümleri hakkında bilgi yönetmek için kullanılır. Görüntü tanımı adları büyük veya küçük harf, rakam, nokta, kısa çizgi ve dönemleri meydana gelebilir. Bir görüntü tanımı için belirtebileceğiniz değerler hakkında daha fazla bilgi için bkz. [görüntü tanımları](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Galeriniz içinde galeri görüntüsü tanımı oluşturun. Bu örnekte, Galeri görüntüsü adlı *myImageDefinition*.

1. Seçin sayfasında, yeni bir görüntü Galerisi, **yeni bir görüntü tanımı eklemek** sayfanın üst. 
1. İçin **görüntü tanımı adı**, türü *myImageDefinition*.
1. İçin **işletim sistemi**, uygulamanızın kaynak görüntüyü temel alarak doğru seçeneğini belirleyin.
1. İçin **yayımcı**, türü *myPublisher*. 
1. İçin **teklif**, türü *myOffer*.
1. İçin **SKU**, türü *mySKU*.
1. Emin olun **Evet** ABD için seçilen **etkinleştirme** seçip **gözden geçir + Oluştur**.
1. Görüntü tanımı doğrulama denetimini geçtikten seçin **Oluştur**.
1. Dağıtım tamamlandığında seçin **kaynağa Git**.



## <a name="create-an-image-version"></a>Görüntü sürümü oluşturma

Görüntü sürümü, yönetilen bir görüntüden oluşturun. Bu örnekte, görüntü sürümü olan *1.0.0* ve her ikisi de çoğaltılır *Batı Orta ABD* ve *Orta Güney ABD* veri merkezleri. Çoğaltma için hedef bölgeler seçerken, ayrıca eklemek zorunda olmadığını unutmayın *kaynak* çoğaltma için hedef bölgede.

Görüntü sürümü için izin verilen karakter, sayı ve dönemleri ' dir. Sayı 32-bit tamsayı aralığında olmalıdır. Biçim: *MajorVersion*. *MinorVersion*. *Düzeltme Eki*.

1. Görüntü tanımınız için sayfasında seçin **Ekle sürüm** sayfanın üst.
1. İçinde **bölge**, yönetilen, görüntünün depolandığı bölgeyi seçin. Yansıma sürümü öğesinden oluşturulan aynı bölgede yönetilen bir görüntü olarak oluşturulması gerekir.
1. İçin **adı**, türü *1.0.0*. Görüntü sürümü adı izlemelidir *ana*. *küçük*. *Düzeltme Eki* tam sayılar kullanılarak biçimlendirilecek. 
1. İçinde **kaynak görüntüsü**, açılan listeden, kaynak yönetilen bir görüntü seçin.
1. İçinde **son dışında tut**, varsayılan değerini bırakın *Hayır*.
1. İçin **sonu yaşam tarihi**, birkaç ay sonra takvimden bir tarih seçin.
1. İçinde **çoğaltma**, bırakın **varsayılan yineleme sayısı** 1 olarak. Kaynak bölgeye çoğaltmak, bu nedenle ilk çoğaltmayı varsayılan olarak bırakın ve sonra olacak şekilde ikinci bir çoğaltma bölge seçip için ihtiyaç duyduğunuz *Doğu ABD*.
1. İşiniz bittiğinde **gözden geçir + Oluştur**. Azure yapılandırmasını doğrular.
1. Görüntü sürümü doğrulama başarılı olduğunda seçin **Oluştur**.
1. Dağıtım tamamlandığında seçin **kaynağa Git**.

Bu görüntünün hedef bölgeler tümüne biraz sürebilir.
