---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/27/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: cae108a1d4226e8c0fe39f9cd1cedc1e6a024ffc
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465436"
---
## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.

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
1. İşiniz bittiğinde seçin **gözden geçir + Oluştur**.
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

## <a name="share-the-gallery"></a>Galeri paylaşın

Görüntü Galerisi düzeyinde erişim paylaştığınız öneririz. Aşağıda, yeni oluşturduğunuz galeri Paylaşımı aracılığıyla açıklanmaktadır.

1. [Azure portalı](https://portal.azure.com) açın.
1. Soldaki menüde **kaynak grupları**. 
1. Kaynak grupları listesinde seçin **myGalleryRG**. Kaynak grubunuz için bir dikey pencere açılır.
1. Sol tarafındaki menüde **myGalleryRG** sayfasında **erişim denetimi (IAM)** . 
1. Altında **bir rol ataması Ekle**seçin **Ekle**. **Bir rol ataması Ekle** bölmesi açılır. 
1. Altında **rol**seçin **okuyucu**.
1. Altında **erişim Ata**, varsayılan değerini bırakın **Azure AD kullanıcı, Grup veya hizmet sorumlusu**.
1. Altında **seçin**, davet etmek istediğiniz kişinin e-posta adresini yazın.
1. Kullanıcı, kuruluşunuz dışında ise, bir ileti görürsünüz **bu kullanıcının Microsoft ile işbirliği yapmasına tanıyan bir e-posta gönderilir.** E-posta adresiyle kullanıcı seçin ve ardından **Kaydet**.

Kullanıcı kuruluşunuz dışında ise, bunlar kuruluşa katılma bir e-posta davetiyesi alırsınız. Kullanıcının daveti kabul etmesi gerekir ve ardından galeri ve tüm görüntü tanımları ve sürümler kaynakların listelerinde görmeye devam.

