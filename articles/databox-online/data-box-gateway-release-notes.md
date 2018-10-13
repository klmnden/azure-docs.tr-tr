---
title: Azure veri kutusu ağ geçidi Preivew sürüm notları | Microsoft Docs
description: Azure veri kutusu önizleme sürümünü çalıştıran ağ geçidi için açık kritik sorunlar ve çözümleri açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: f5e19d59dfddc3be849700f3678519179b5b39ba
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49164578"
---
# <a name="azure-data-box-gateway-preview-release-notes"></a>Azure Data Box ağ geçidi önizlemesi sürüm notları

## <a name="overview"></a>Genel Bakış

Aşağıdaki sürüm notları, kritik açık sorunlar ve çözümlenen sorunlar için Microsoft Azure veri kutusu ağ geçidinin önizleme sürümünü tanımlar.

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça eklenir. Veri kutusu ağ geçidi dağıtmadan önce bu sürüm notlarında yer alan bilgileri dikkatle gözden geçirin.

Önizleme sürümü yazılım sürümüne karşılık gelen **veri kutusu ağ geçidi Preview sürüm 2.0**.

## <a name="issues-fixed-in-preview-release"></a>Önizleme sürümünde giderilen sorunlar

Aşağıdaki tabloda, bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Sorun |
| --- | --- |
| 1 | Bu sürümde, başka bir aracı (AzCopy) tarafından karşıya yüklenen dosya yenilenir ve ardından bir şekilde güncelleştirilmesi artar/dosya boyutu genişleten sonra aşağıdaki hata gözlemlenen: *hata 400: InvalidBlobOrBlock (belirtilen blob veya blok içeriği geçersiz.)*|


## <a name="known-issues-in-preview-release"></a>Önizleme sürümündeki bilinen sorunlar

Aşağıdaki tabloda, önizleme sürümünü çalıştıran, veri kutusu ağ geçidi için bilinen sorunların bir Özet sağlar.

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamaları |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önceki Önizleme sürümleri bu sürüme güncelleştirilemez oluşturulan veri kutusu ağ geçidi cihazı. |Sanal disk görüntüleri yeni sürümü karşıdan yüklemek ve yapılandırmak ve yeni cihazlara dağıtabilirsiniz. Daha fazla bilgi için Git [Azure veri kutusu ağ geçidi dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md). |
| **2.** |Sağlanan veri diski |Bir veri diskinin belirli bir belirtilen boyutta bir kez sağladığınız ve oluşturulan karşılık gelen veri kutusu ağ geçidi, veri diski küçültmeye gerekir değil. Cihazdaki tüm yerel verilerin kaybı disk sonuçları daraltmak çalışıyor. | |
| **3.** |Yenile |Bu sürümde, aynı anda yalnızca bir paylaşım yenileyebilirsiniz. | |
| **4.** |Yeniden Adlandır |Nesneleri yeniden adlandırma desteklenmiyor. |Bu özellik, akışınız için önemliyse, Microsoft Support başvurun. |
| **5.** |Kopyala| Salt okunur bir dosya cihaza kopyalanırsa, salt okunur özelliği korunmaz. | |
| **6.** |Günlükler| Bu sürümde bir hata nedeniyle hata kodunu 110 örneklerini görebileceğiniz *error.xml* tanınmayan öğesi adları ile. | |
| **7.** |Karşıya Yükle | Bu sürümde bir hata nedeniyle, belirli dosyaları karşıya yükleme sırasında hata kodu 2003 örneklerini görebilirsiniz. | |
| **8.** |Dosya türleri | Aşağıdaki Linux dosya türlerinde desteklenmez: karakter dosyaları, dosyaları engelleme, yuva, Kanallar, simgesel bağlantılar.  |Bu dosyaları kopyalama NFS oluşturulmakta 0 uzunluklu dosyalar sonuçlarında paylaşın. Bu dosyalar bir hata durumunda kalır ve ayrıca bildirilen *error.xml*. |
| **9.** |Silme | NFS paylaşımını silinirse, bu sürümde bir hata nedeniyle, paylaşım ardından silinemez. Paylaşım durumu *silme*.  |Desteklenmeyen dosya adını kullanarak paylaşıma yalnızca bu gerçekleşir. |
| **10.** |Yenile | İzinler ve erişim denetim listeleri (ACL'ler), bir yenileme işlemi arasında korunmaz.  | |
| **11.** |Çevrimiçi Yardım |Azure portalı Yardım bağlantıları belgelerinin bağlantısı yok.|Yardım bağlantıları genel kullanım sürümünde çalışır. |



## <a name="next-steps"></a>Sonraki adımlar

- [Azure veri kutusu ağ geçidi dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md).


