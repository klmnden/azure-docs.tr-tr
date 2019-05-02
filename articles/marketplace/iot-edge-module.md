---
title: Azure IOT Edge modülleri
description: IOT Edge modülü sunan Azure Marketi'nde uygulama ve hizmet yayımcılar için.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, IoT Edge module offer
author: qianw211
manager: pabutler
ms.service: marketplace
ms.topic: article
ms.date: 09/22/2018
ms.author: qianw211
ms.openlocfilehash: 9f4ad704de83e5971b5bc10083aefeec5d28374b
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64937845"
---
# <a name="iot-edge-modules"></a>IoT Edge modülleri

[Azure IOT Edge](https://azure.microsoft.com/services/iot-edge/) platform, Azure bulut tarafından desteklenir.  Bu platform, kullanıcıların doğrudan IOT cihazlarında çalıştırmak için bulut iş yüklerini dağıtmaya olanak tanır.  IOT Edge modülü, çevrimdışı iş yüklerini çalıştırmak ve veri analizi yerel olarak yapın. Bu teklif türü, bant genişliği, yerel ve hassas veriler korunmasına yardımcı olur ve düşük gecikmeli yanıt süresi sunar.  Artık, önceden oluşturulmuş bu iş yüklerinin yararlanmak için seçeneğiniz de vardır. Şimdiye kadar yalnızca birinci taraf Microsoft çözümleri birkaç kullanılabilir.  Kendi özel IOT çözümlerinin oluşturmamızı zaman ve kaynak yatırım gerekiyordu.

Karşınızda tarafından [IOT Edge modülleri Azure Market'te](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1), şimdi yayımcıların çözümleri IOT dinleyicilere tanıtabilecekleri ve kullanıma sunmak için tek bir hedef vardır. IOT geliştiriciler, sonuçta bulun ve kendi çözümünüzü geliştirmeyi hızlandırmak için özellikleri satın alın.  

## <a name="key-benefits-of-iot-edge-modules-in-azure-marketplace"></a>IOT Edge modülleri Azure Marketi'nde önemli avantajları:

| **Yayımcıları için**    | **(IOT geliştiriciler) müşteriler için**  |
| :------------------- | :-------------------|
| Milyonlarca IOT Edge çözümleri oluşturma ve dağıtma isteyen geliştiriciler ulaşın.  | Güvenli ve test edilmiş bileşenlerini kullanma güvenle IOT Edge çözümünü oluşturun. |
| Bir kez yayınlayın ve kapsayıcıları destekleyen herhangi bir IOT Edge donanım üzerinde çalıştırın. | Bularak ve belirli gereksinimleri için 1. ve 3. taraf IOT Edge modüllerini dağıtmak pazara ulaşma süresini azaltın. |
| Esnek faturalandırma seçenekleri ile gelir elde edin <ul> <li> Ücretsiz ve kendi lisansınızı getirin (BYOL). </li> </ul> | Faturalama modelleri dillerinden satın alma işlemleri yapın. <ul> <li> Ücretsiz ve kendi lisansınızı getirin (BYOL). </li> </ul> |

## <a name="what-is-an-iot-edge-module"></a>IOT Edge Modülü nedir?

Azure IOT Edge dağıtma ve iş mantığı kenarda modülleri şeklinde yönetmenize olanak sağlar. Azure IOT Edge modülleri, IOT Edge tarafından yönetilen en küçük hesaplama birimleri ve Microsoft Hizmetleri (örneğin, Azure Stream Analytics), 3. taraf hizmetleri veya kendi çözümünüze özgü kodla içerebilir. IOT Edge modülleri hakkında daha fazla bilgi için bkz: [anlamak Azure IOT Edge modülleri](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules).

**Bir kapsayıcı Teklif türü ve bir IOT Edge modülü Teklif türü arasındaki fark nedir?**

IOT Edge modülü Teklif türü, kapsayıcı bir IOT Edge cihaz üzerinde çalışan belirli türüdür. IOT Edge bağlamında çalışacak şekilde varsayılan yapılandırma ayarlarıyla birlikte gelir ve isteğe bağlı olarak IOT Edge çalışma zamanı ile tümleştirilmesi için SDK'sı IOT Edge modülünü kullanır.

## <a name="publishing-your-iot-edge-module"></a>Yayımlama, IOT Edge Modülü

**Doğru mağazada seçme**

IOT Edge modülleri, yalnızca Azure Market'te yayımlanan, AppSource geçerli değildir.  Farklılıklar ve hedef kitlesi vitrinler arasında hakkında daha fazla bilgi için bkz. [çözümünüz için yayımlama seçeneği belirleme](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type).
 
**Faturalandırma seçenekleri**

Market'in şu anda desteklediği **ücretsiz** ve **kendi lisansını getir (KLG)** faturalandırma seçenekleriyle IOT Edge modülleri için.
 
**Yayımlama seçenekleri**

Her durumda, IOT Edge modülleri seçmelisiniz **Transact** seçeneği yayımlama.  Bkz: [yayımlama seçeneğini](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) Yayımlama seçenekleri hakkında daha fazla bilgi.  

## <a name="eligibility-criteria"></a>Uygunluk ölçütleri

Microsoft Azure Marketi sözleşmeleri ve ilkeleri tüm koşulları IOT Edge modülü teklifler için geçerlidir.  Ayrıca, ön koşullar ve IOT Edge modülleri için teknik gereksinimleri vardır.  

**Ön koşullar**

IOT Edge modülü, Azure Marketi'nde içerik yayımlamak için aşağıdaki önkoşulları sağlamanız gerekir:

- Bulut iş ortağı Portalı'nı (CPP) erişim. Daha fazla bilgi için [Azure Market'te ve Appsource'ta yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Bir Azure Container Registry'de, IOT Edge modülü barındırın. 
- IOT Edge modülü meta verilerinizi (kapsamlı olmayan listesi gibi) hazır vardır: 
    - Bir başlık
    - Bir açıklama (HTML biçiminde)
    - Logo görüntüsü (PNG biçiminde ve sabit görüntü boyutları 40x40px 90x90px, 115x115px, 255x115px dahil olmak üzere)
    - Kullanım ve gizlilik ilkesi koşulları
    - Varsayılan modül yapılandırması (yol, çiftinin istenen özelliklerini, createOptions, ortam değişkenleri)
    - Belgeler
    - Destek kişileri

**Teknik gereksinimler**

Bunun almak ve Sertifikalı Azure Market'te yayımlanmış bir IOT Edge modülü için birincil teknik gereksinimleri bölümünde detayları [teknik varlıkları, IOT Edge modülü hazırlama](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-technical-assets).  

## <a name="documentation-and-resources"></a>Belgeleri ve kaynakları

[IOT Edge modülü teklif oluşturma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-offer) -– bulut yayımlama portalında yeni bir IOT Edge modülü yayımlama adımları sunar.

## <a name="next-steps"></a>Sonraki adımlar

Zaten yapmadıysanız,

- Kaydetmeniz [Microsoft iş ortağı ağı](https://partner.microsoft.com/membership).
- Oluşturma bir [Microsoft Account](https://account.microsoft.com/account/) (teklifleri Azure Marketi transact için gerekli; diğerleri için önerilir).
- Gönderme [Market kayıt formunda](https://azuremarketplace.microsoft.com/sell/signup).

Kayıtlı ve yeni bir teklif oluşturur veya mevcut bir proje üzerinde çalışmaya,

- Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/) oluşturmak veya teklifiniz tamamlayın.
- Bkz: [genel bakış yayımlama IOT Edge modülü teklif](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-offer-process-parts) bir IOT Edge modülü teklifi yayımlama hakkında bilgi için.
