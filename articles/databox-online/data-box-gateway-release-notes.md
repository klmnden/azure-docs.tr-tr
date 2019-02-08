---
title: Azure Data Box ağ geçidi önizlemesi sürüm notları | Microsoft Docs
description: Azure veri kutusu önizleme sürümünü çalıştıran ağ geçidi için açık kritik sorunlar ve çözümleri açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 02/07/2019
ms.author: alkohli
ms.openlocfilehash: 0265de5b224e62d188fe6e3b9322d5c2e3f77fa1
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883143"
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
| **1.** | Başka bir aracı (AzCopy) tarafından karşıya yüklenen dosya yenilenir ve ardından dosya boyutu artar/genişleten bir şekilde güncelleştirilmesi bu sürümde, şu hatayı dikkate alınır: *400. hata: InvalidBlobOrBlock (belirtilen blobu veya blok içeriği geçersiz.)*|
| **2.** |Bu sürümde bir hata nedeniyle hata kodunu 110 örneklerini görebileceğiniz *error.xml* tanınmayan öğesi adları ile. | 
| **3.** |Bu sürümde bir hata nedeniyle, belirli dosyaları karşıya yükleme sırasında hata kodu 2003 örneklerini görebilirsiniz. | 
| **4.** |Bu sürümde, aynı anda yalnızca bir paylaşım yenileyebilirsiniz. | 


## <a name="known-issues-in-preview-release"></a>Önizleme sürümündeki bilinen sorunlar

Aşağıdaki tabloda, önizleme sürümünü çalıştıran, veri kutusu ağ geçidi için bilinen sorunların bir Özet sağlar.

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamaları |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önceki Önizleme sürümleri bu sürüme güncelleştirilemez oluşturulan veri kutusu ağ geçidi cihazı. |Sanal disk görüntüleri yeni sürümü karşıdan yüklemek ve yapılandırmak ve yeni cihazlara dağıtabilirsiniz. Daha fazla bilgi için Git [Azure veri kutusu ağ geçidi dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md). |
| **2.** |Sağlanan veri diski |Bir veri diskinin belirli bir belirtilen boyutta bir kez sağladığınız ve oluşturulan karşılık gelen veri kutusu ağ geçidi, veri diski küçültmeye gerekir değil. Cihazdaki tüm yerel verilerin kaybı disk sonuçları daraltmak çalışıyor. | |
| **3.** |Yeniden Adlandır |Nesneleri yeniden adlandırma desteklenmiyor. |Bu özellik, akışınız için önemliyse, Microsoft Support başvurun. |
| **4.** |Kopyala| Salt okunur bir dosya cihaza kopyalanırsa, salt okunur özelliği korunmaz. | |
| **5.** |Dosya türleri | Aşağıdaki Linux dosya türlerinde desteklenmez: karakter dosyaları, dosyaları engelleme, yuva, Kanallar, simgesel bağlantılar.  |Bu dosyaları kopyalama NFS oluşturulmakta 0 uzunluklu dosyalar sonuçlarında paylaşın. Bu dosyalar bir hata durumunda kalır ve ayrıca bildirilen *error.xml*. |
| **6.** |Silme | NFS paylaşımını silinirse, bu sürümde bir hata nedeniyle, paylaşım ardından silinemez. Paylaşım durumu *silme*.  |Desteklenmeyen dosya adını kullanarak paylaşıma yalnızca bu gerçekleşir. |
| **7.** |Yenile | İzinler ve erişim denetim listeleri (ACL'ler), bir yenileme işlemi arasında korunmaz.  | |
| **8.** |Kopyala | Veri kopyalama hatasıyla başarısız oluyor:  Bir dosya sistemi sınırlaması nedeniyle istenen işlem tamamlanamadı.  |128 KB (ReFS için üst sınır) dosyası ile ilişkili diğer veri Stream (REKLAM) aştığında, bu hata oluşur.  |
| **9.** |Simgesel bağlantılar |Sembolik bağlantılar desteklenmez.  |Sembolik bağlantılar dizinler için hiçbir zaman çevrimdışı olarak işaretlenmiş dizinlerde neden. Sonuç olarak, gri arası dizinleri çevrimdışı olduğunu ve tüm ilişkili içeriği tamamen Azure'da yüklenen gösterir dizinlerde göremeyebilirsiniz. |
| **10.** |Paylaşımlar |Dosya değişikliği hatalarında karşıya yüklemek için sayfa BLOB'ları ile var olan bir kapsayıcı bir blok blobu paylaşımına (veya tersi) yenileme yol açar.  |Bu davranış, bu adımları izlediğinizde görülür: <li> Cihazda bir blok blobu paylaşımı oluşturun. </li><li> Paylaşım, sayfa BLOB'ları olan bir var olan bulut kapsayıcısı ile ilişkilendirin.</li><li>Bu paylaşım yenileyin. </li><li>Bazı bulut sayfa Blobları olarak depolanır yenilenmiş dosyaları değiştirin.</li> Karşıya yükleme hataları görülür. |
| **11.** |Çevrimiçi Yardım |Azure portalı Yardım bağlantıları belgelerinin bağlantısı yok.|Yardım bağlantıları genel kullanım sürümünde çalışır. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure veri kutusu ağ geçidi dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md).


