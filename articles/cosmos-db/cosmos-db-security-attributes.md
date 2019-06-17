---
title: Azure Cosmos DB için güvenlik öznitelikleri
description: Azure Cosmos DB değerlendirmek için güvenlik öznitelikleri listesi
services: cosmos-db
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: ffeb60f5476a540e3da46a82c240b0dda9aa6be2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66480470"
---
# <a name="security-attributes-for-azure-cosmos-db"></a>Azure Cosmos DB için güvenlik öznitelikleri

Bu makalede, Azure Cosmos DB'ye yerleşik genel güvenlik öznitelikleri belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| (Örneğin, sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) bekleme sırasında şifreleme | Evet | Varsayılan olarak, tüm Cosmos DB veritabanları ve yedeklemeleri şifrelenir; bkz: [Azure Cosmos DB'de veri şifreleme](database-encryption-at-rest.md). Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi desteklenmiyor. |
| (Örneğin, ExpressRoute şifreleme, şifreleme sanal ağ ve VNet-VNet şifreleme) aktarım sırasında şifreleme| Evet | Tüm Azure Cosmos DB verileri aktarım sırasında şifrelenir. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır |  |
| Sütun düzeyinde şifrelemeyi (Azure Veri Hizmetleri)| Evet | Yalnızca tablo API Premium'da. Bu özellik tüm API'leri destekler. Bkz: [Azure Cosmos DB'ye giriş: Tablo API'si](table-introduction.md). |
| Şifrelenmiş API çağrıları| Evet | Azure Cosmos DB için tüm bağlantılar, HTTPS'yi destekleyecek. Azure Cosmos DB, TLS 1.2 bağlantıları da destekliyor, ancak bu henüz zorlanmaz. Müşterilerin kendi tarafında daha düşük düzey TLS kapatırsanız, Cosmos DB'ye bağlanmak için emin olabilirsiniz.  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| vNET ekleme desteği| Evet | Sanal ağ hizmet uç noktası ile bir Azure Cosmos DB hesabı yalnızca bir sanal ağın (VNet) belirli bir alt ağdan erişime izin verecek şekilde yapılandırabilirsiniz. Ayrıca, güvenlik duvarı kuralları ile VNet erişim birleştirebilirsiniz.  Bkz: [erişim Azure Cosmos DB sanal ağlardan](vnet-service-endpoint.md). |
| Ağ yalıtımı ve Firewalling desteği| Evet | Güvenlik Duvarı desteği sayesinde, yalnızca IP adresleri, IP adres aralığı onaylı bir kümesini erişime izin verecek ve/veya Bulut Hizmetleri için Azure Cosmos hesabınıza yapılandırabilirsiniz. Bkz: [yapılandırma IP Güvenlik Duvarı Azure Cosmos DB'de](how-to-configure-firewall.md).|
| Zorlamalı tünel için destek | Evet | Sanal makinelerin bulunduğu sanal ağ üzerindeki istemci tarafındaki yapılandırılabilir.   |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Azure Cosmos DB için gönderilen tüm istekleri günlüğe kaydedilir. [Azure izleme](../azure-monitor/overview.md), Azure ölçümleri, Azure denetim günlükleri desteklenir.  Veri düzlemi istekleri, çalışma zamanı istatistikleri sorgu, sorgu metni ilgili bilgileri günlüğe kaydedebilirsiniz, MongoDB ister. Uyarıları da ayarlayabilirsiniz. |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Veritabanı hesabı düzeyinde Evet; Veri düzlemi düzeyinde Cosmos DB, kaynak belirteçlerine ve anahtar erişimi kullanır. |
| Yetkilendirme| Evet | Ana anahtarları (birincil ve ikincil) ile Azure Cosmos hesabı ve kaynak belirteçleri desteklenmiyor. Okuma/yazma alın veya yalnızca ana anahtarları ile verilere erişimi okuyun. Kaynak belirteçleri, belgeler ve kapsayıcıları gibi kaynakları sınırlı bir süre erişime izin verin. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Günlüğe kaydetme ve denetim Denetim/yönetimini planlama| Evet | Azure etkinlik günlüğü için hesap düzeyinde işlemleri gibi güvenlik duvarları, sanal ağlar, erişim anahtarları ve IAM. |
| Veri günlük kaydı ve denetim düzlemi | Evet | Kapsayıcı düzeyi işlemlerinde gibi oturum izleme, tanılama kapsayıcının sağlama aktarım hızı, ilkeleri ve belgeleri CRUD işlemleri dizin oluşturun. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Hayır  | | 

## <a name="additional-security-attributes-for-cosmos-db"></a>Cosmos DB için ek güvenlik öznitelikleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kaynak kaynak paylaşımı (CORS) | Evet | Bkz: [çıkış noktaları arası kaynak paylaşımı (CORS) yapılandırma](how-to-configure-cross-origin-resource-sharing.md). |
