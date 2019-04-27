---
title: Azure Container Registry'de resim depolama
description: Docker kapsayıcı görüntülerinizi Azure Container Registry'de güvenlik, yedeklilik ve kapasite de dahil olmak üzere nasıl depolandığı ayrıntıları.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/21/2018
ms.author: danlep
ms.openlocfilehash: 55c84907ab41f6da9d7a0989c68a1c1f90c5e424
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60827281"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Kapsayıcı görüntüsü Azure Container Registry depolamada

Her [temel, standart ve Premium](container-registry-skus.md) bekleyen şifreleme görüntü veri güvenliği ve coğrafi olarak yedeklilik görüntü veri koruma için Gelişmiş Azure depolama Özelliği Azure kapsayıcı kayıt defteri avantajları ister. Aşağıdaki bölümlerde, hem özellikleri hem de Azure Container Registry (ACR) resim depolama sınırları açıklanmaktadır.

## <a name="encryption-at-rest"></a>Bekleyen şifreleme

Tüm kapsayıcı görüntülerini kullanarak kayıt bekleme durumundayken şifrelenir. Azure otomatik olarak bir görüntü depolamadan önce şifreler ve siz veya uygulama ve hizmetlerinizin görüntü çekme, üzerinde halindeyken şifresini çözer.

## <a name="geo-redundant-storage"></a>Coğrafi olarak yedekli depolama

Azure, kapsayıcı görüntülerinizi kaybına karşı koruma sağlamak için coğrafi olarak yedekli depolama düzeni kullanır. Birden çok coğrafi olarak uzak veri merkezlerinin otomatik olarak çoğaltır, kapsayıcı görüntülerinizi bölgesel depolama hatası durumunda kullanıcıların kaybını önleme Azure Container kayıt defteri.

## <a name="geo-replication"></a>Coğrafi çoğaltma

Daha yüksek kullanılabilirlik garantisi gerektiren senaryolar için düşünün kullanarak [coğrafi çoğaltma](container-registry-geo-replication.md) Premium kayıt defterlerinin özelliğidir. Coğrafi çoğaltma, durumunda, kayıt defteri erişimini kaybetme karşı korumanıza yardımcı olur bir *toplam* bölgesel hatalara, yalnızca bir depolama hatası. Coğrafi çoğaltma diğer avantajları sağlar, çok, ağa yakın görüntüsü gibi depolama için daha hızlı gönderim ve Dağıtılmış Geliştirme ve dağıtım senaryolarında çeker.

## <a name="image-limits"></a>Görüntü sınırları

Aşağıdaki tablo, kapsayıcı görüntüsü ve depolama limitleri Azure kapsayıcısı kayıt defterleri için yerinde açıklar.

| Kaynak | Sınır |
| -------- | :---- |
| Depolar | Sınırsız |
| Görüntüler | Sınırsız |
| Katmanları | Sınırsız |
| Etiketler | Sınırsız|
| Depolama | 5 TB |

Çok yüksek sayıda depoları ve etiketleri, kayıt defterinizin performansını etkileyebilir. Kayıt defteri bakım yordamının bir parçası olarak düzenli aralıklarla kullanılmayan depoları, etiketler ve görüntüleri silin. Kayıt defteri kaynakları depoları, görüntüleri ve etiketleri gibi silinmiş *olamaz* , silindikten sonra kurtarılamaz. Kayıt defteri kaynakları silme hakkında daha fazla bilgi için bkz. [Sil kapsayıcı görüntülerini Azure Container Registry'de](container-registry-delete.md).

## <a name="storage-cost"></a>Depolama maliyeti

Fiyatlandırma hakkında tam Ayrıntılar için bkz [Azure Container Registry fiyatlandırma][pricing].

## <a name="next-steps"></a>Sonraki adımlar

Farklı Azure Container Registry SKU'ları (temel, standart, Premium) hakkında daha fazla bilgi için bkz: [Azure Container Registry SKU'ları](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: https://aka.ms/acr/pricing

<!-- LINKS - Internal -->
