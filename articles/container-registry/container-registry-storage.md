---
title: Azure kapsayıcı kayıt defterinde görüntü depolama
description: Docker kapsayıcısı görüntülerinizi güvenlik, artıklık ve kapasite dahil olmak üzere Azure kapsayıcı kayıt defterinde nasıl depolandığı ayrıntıları.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 03/21/2018
ms.author: marsma
ms.openlocfilehash: df46712889a3eba54f1a2288ba93c82b21b92deb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="container-image-storage-in-azure-container-registry"></a>Azure kapsayıcı kayıt defterinde kapsayıcı görüntü depolama

Her [temel, standart ve Premium](container-registry-skus.md) Azure kapsayıcı kayıt defteri avantajları Gelişmiş Azure depolama özelliklerinden gibi şifreleme çalışmıyorken-görüntü veri güvenliği ve coğrafi yedeklilik görüntü veri koruması. Aşağıdaki bölümlerde özellikleri ve sınırları görüntü depolama Azure kapsayıcı kayıt defteri (ACR) açıklanmaktadır.

## <a name="encryption-at-rest"></a>Rest, şifreleme

Tüm kapsayıcı görüntüleri, kayıt defterinde bekleyen şifrelenir. Azure otomatik olarak depolamadan önce görüntünün şifreler ve siz veya, uygulama ve hizmetlerinize görüntü çekme olduğunda, üzerinde-çalışma sırasında şifresini çözer.

## <a name="geo-redundant-storage"></a>Coğrafi Olarak Yedekli Depolama

Azure kapsayıcı görüntülerinizi kaybına karşı koruma sağlamak için bir coğrafi olarak yedekli depolama düzenini kullanır. Birden çok coğrafi olarak birbirinden uzak veri merkezleri için otomatik olarak çoğaltır, kapsayıcı görüntülerinizi bölgesel depolama hatası durumunda kendi kaybını önleme Azure kapsayıcı kayıt defteri.

## <a name="geo-replication"></a>Coğrafi çoğaltma

Daha yüksek kullanılabilirlik güvence gerektiren senaryolar için düşünün [coğrafi çoğaltma](container-registry-geo-replication.md) Premium kayıt defterleri özelliğidir. Coğrafi çoğaltma yardımcı durumunda, kayıt defterine erişim kaybolmasına karşı koruma bir *toplam* bölgesel hatası, yalnızca bir depolama hatası. Coğrafi çoğaltma başka avantajları sağlar, çok, ağ Kapat görüntü gibi depolama için daha hızlı iter ve Dağıtılmış Geliştirme ve dağıtım senaryolarında çeker.

## <a name="image-limits"></a>Görüntü sınırları

Aşağıdaki tabloda, Azure kapsayıcı kayıt defterleri için yerinde kapsayıcı görüntü ve depolama sınırları açıklanmaktadır.

| Kaynak | Sınır |
| -------- | :---- |
| Depolar | Sınırsız |
| Görüntüler | Sınırsız |
| Katmanları | Sınırsız |
| Etiketler | Sınırsız|
| Depolama | 5 TB |

Çok yüksek sayıda depoları ve etiketleri kaydınız performansını etkileyebilir. Kullanılmayan depoları, etiketler ve görüntüleri kullanarak düzenli aralıklarla silme [Azure CLI](/cli/azure/acr), ACR [REST API](/rest/api/containerregistry/), veya [Azure portal] [ portal] olarak kayıt defteri bakım alışkanlık parçası. Kayıt defteri kaynakları depoları, görüntüler ve etiketler gibi silinmiş *olamaz* silindikten sonra kurtarılması.

## <a name="storage-cost"></a>Depolama maliyeti

Fiyatlandırma hakkında ayrıntılar için bkz: [Azure kapsayıcı kayıt defteri fiyatlandırma][pricing].

## <a name="next-steps"></a>Sonraki adımlar

Farklı Azure kapsayıcı kayıt defteri SKU'ları (temel, standart, Premium) hakkında daha fazla bilgi için bkz: [Azure kapsayıcı kayıt defteri SKU'ları](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: http://aka.ms/acr/pricing

<!-- LINKS - Internal -->
