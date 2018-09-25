---
title: include dosyası
description: include dosyası
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 2902fe53e7f3ffb86dc727ebcdc2200ba02ac203
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048458"
---
Paylaşılan resim galerileri, görüntü tanımları ve yansıma sürümü herhangi bir işlemi gerçekleştirirken sorun yaşarsanız, başarısız olan komutu hata ayıklama modunda yeniden çalıştırın. Hata ayıklama modu etkin olduğu geçirerek **-hata ayıklama** CLI ile anahtar ve **-hata ayıklama** geçiş PowerShell ile. Hataları gidermek için bu belgede, hata bulunan sonra izleyin.


## <a name="unable-to-create-a-shared-image-gallery"></a>Paylaşılan görüntü Galerisi oluşturulamadı

Olası nedenler:

*Galeri adı geçersiz.*

Galeri adı için izin verilen karakterler büyük veya küçük harf, rakam, nokta ve dönemleri olur. Galeri adı tire içinde olamaz. Galeri adı değiştirin ve yeniden deneyin. 

*Galeri adı, abonelik içinde benzersiz değil.*

Başka bir galeri adı belirleyin ve yeniden deneyin.


## <a name="unable-to-create-an-image-definition"></a>Bir görüntü tanımı oluşturulamadı 

Olası nedenler:

*görüntü tanımı adı geçersiz.*

Görüntü tanımı için izin verilen karakterler büyük veya küçük harf, rakam, nokta, tire ve nokta olur. Görüntü tanımı adını değiştirin ve yeniden deneyin.

*Bir görüntü tanımı oluşturmak için zorunlu özelliklerini doldurulmaz.*

Ad, yayımcı, teklif, sku ve işletim sistemi türü gibi özellikleri zorunludur. Tüm özellikleri gönderildiğini doğrulayın.

Emin olun **OSType**, Linux veya Windows, görüntüyü tanımının aynıdır, görüntü sürümü oluşturmak için kullandığınız görüntü kaynağı yönetilen olarak. 


## <a name="unable-to-create-an-image-version"></a>Görüntü sürümü oluşturulamadı 

Olası nedenler:

*Görüntü sürümü adı geçersiz.*

Görüntü sürümü için izin verilen karakter, sayı ve dönemleri ' dir. Sayı 32-bit tamsayı aralığında olmalıdır. Biçim: *MajorVersion.MinorVersion.Patch*. Görüntü sürümü adı değiştirin ve yeniden deneyin.

*Görüntü sürümü oluşturulduğu kaynak yönetilen görüntüsü bulunamadı.* 

Kaynak görüntü var ve görüntü sürümü ile aynı bölgede olup olmadığını denetleyin.

*Yönetilen bir görüntü bitti değil hala uygulanmakta.*

Kaynak yönetilen bir görüntü sağlama durumu olduğundan emin olun **başarılı**.

*Kaynak bölgesi henüz desteklenmiyor.*

İstenen kaynak bölgede desteklenip desteklenmediğini görmek için aşağıdaki tabloyu kullanın:
<br>

| Galeride veya "kaynak bölge" oluşturun   | Rersion için ya da "hedef bölgede" çoğaltın |
|----------------------------------------|-------------------------------------------|
| Batı Orta ABD                        | Tüm Azure genel bulut bölgeleri            |
| Orta Güney ABD                       |                                           |
| Doğu ABD 2                              |                                           |
| Güneydoğu Asya                         |                                           |
| Batı Avrupa                            |                                           |

<br>

Hedef bölge listesi, görüntü sürümü Kaynak bölgesi içermesi gerekir. Kaynak bölgesi görüntü sürümünüzü çoğaltmak için Azure istediğiniz hedef bölgeler listesinden dahil emin olun.

*Tamamlanmamış tüm hedef bölgelere çoğaltma.*

Kullanım **--ReplicationStatus genişletin** tüm belirtilen hedef bölgelere çoğaltma tamamlandıysa, kontrol etmek için bayrak. Aksi durumda, işin tamamlanması 6 saat kadar bekleyin. Başarısız olursa yeniden oluşturun ve görüntü sürümü çoğaltmak için komutu çalıştırın. Çok sayıda görüntü sürümü çoğaltılmakta olan hedef bölgeler varsa, çoğaltma aşamalardaki yapmayı düşünün.

## <a name="unable-to-create-a-vm-or-a-scale-set"></a>Bir VM veya ölçeği oluşturulamıyor ayarlayın 

Olası nedenler:

*Çalışan bir sanal makine veya sanal makine ölçek kümesi oluşturma, görüntü sürümü için okuma erişimi yok.*

Abonelik sahibiyle iletişime geçin ve görüntü sürümü veya üstü için kaynaklar (örneğin, paylaşılan görüntü Galerisi ya da görüntü tanımı) okuma erişimi vermek aracılığıyla isteyin [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles) (RBAC). 

*Görüntü sürümü bulunamadı.*

Görüntü sürümü hedef bölgeler listesinden bir sanal makine veya sanal makine ölçek kümesinde oluşturmaya çalıştığınız bölgenin eklendiğini doğrulayın. Bölge zaten hedef bölgeler listesinden ise, çoğaltma işi tamamladığını doğrulayın. Kullanabileceğiniz **- ReplicationStatus** tüm belirtilen hedef bölgelere çoğaltma tamamlandıysa, kontrol etmek için bayrak. 

*Sanal makine veya sanal makine ölçek oluşturulması uzun sürüyorsa ayarlayın.*

Doğrulayın **OSType** görüntüsü VM oluşturmaya çalıştığınız veya sanal makine ölçek kümesi aynı sürümde **OSType** görüntü sürümünü oluşturmak için kullanılan görüntü kaynağını yönetilen. 

## <a name="unable-to-share-resources"></a>Kaynakları paylaşılamıyor

Paylaşılan görüntü Galerisi, görüntü tanımı ve resmi sürüm kaynakları abonelikler arasında Paylaşımı kullanılarak etkinleştirilir [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles) (RBAC). 

## <a name="replication-is-slow"></a>Çoğaltma yavaş

Kullanım **--ReplicationStatus genişletin** tüm belirtilen hedef bölgelere çoğaltma tamamlandıysa, kontrol etmek için bayrak. Aksi durumda, işin tamamlanması 6 saat kadar bekleyin. Başarısız olursa yeniden oluşturun ve görüntü sürümü çoğaltmak için komutu tetikleyin. Çok sayıda görüntü sürümü çoğaltılmakta olan hedef bölgeler varsa, çoğaltma aşamalardaki yapmayı düşünün.

## <a name="azure-limits-and-quotas"></a>Azure limitleri ve kotaları 

[Azure limitleri ve kotaları](https://docs.microsoft.com/azure/azure-subscription-service-limits) tüm paylaşılan görüntü Galerisi, görüntü tanımı ve resmi sürüm kaynakları için geçerlidir. Abonelikleriniz için sınırlar içinde olduğundan emin olun. 



