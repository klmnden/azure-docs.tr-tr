---
title: Dynamics 365 müşteri katılımı için teknik varlıkları oluşturun | Azure Market
description: Dynamics 365 müşteri katılımı uygulama teklif için teknik varlıkları oluşturun.
services: Dynamics 365 for Customer Engagement, Azure, Marketplace, Cloud Partner Portal, AppSource
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 12/29/2018
ms.author: pabutler
ms.openlocfilehash: eff175264677d6b8ffb885229b5e68b306424335
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64943097"
---
# <a name="create-technical-assets-for-azure-application-offer"></a>Azure uygulama teklif için teknik varlıkları oluşturma

Genellikle, kullanarak çözüm geliştirme [için Dynamics 365 müşteri katılımı uygulamaları için SDK'sı](https://docs.microsoft.com/dynamics365/customer-engagement/developer/get-started-sdk).  Açıklandığı formlar çeşitli çözümler ele [programlama modelleri için Dynamics 365 müşteri katılımı uygulamaları için](https://docs.microsoft.com/dynamics365/customer-engagement/developer/programming-models).  Seçtiğiniz çözüm gereksinimlerinizi en iyi uyduğunu formu.  Bir çözüm geliştirirken, bir dizi genişletilebilirlik seçimleri, çözüm bileşenlerini ve sürüm uyumluluğu gibi çözülmesi gereken sorunlar vardır.  Daha fazla bilgi için [çözümlere giriş](https://docs.microsoft.com/dynamics365/customer-engagement/developer/introduction-solutions).

Dynamics 365 çözümleri için AppSource yayımlanan paket dosyaları dağıtılmış yönetilen uygulamaların çoğu.


## <a name="creating-and-storing-the-package"></a>Oluşturma ve paket depolama

Dynamics 365 müşteri katılımı sunar için oluşturulurken paralel belgeleri bölümünde bulunan [uygulamanızı appsource'ta yayımlama](https://docs.microsoft.com/dynamics365/customer-engagement/developer/publish-app-appsource).  Aşağıdaki konular ayrıntılı çözüm paketi dosyası oluşturma ve Azure depolamaya yükleme yer:

- [4. adım: Uygulamanız için bir AppSource paket oluşturma](https://docs.microsoft.com/dynamics365/customer-engagement/developer/create-package-app-appsource) -yönetilen uygulamanız temsil eder ve içeren bir sıkıştırılmış (ZIP) dosyasının nasıl oluşturulacağı açıklanmaktadır: Çözüm varlıklar klasörü, özel kod DLL, MIME türü bilgileri dosyası, AppSource paketi simgesi, lisans koşulları (HTML) dosyası ve içeriği dosyası (XML).
- [5. adım: Azure depolama alanında AppSource paketinizi Store ve SAS anahtarıyla bir URL oluşturun](https://docs.microsoft.com/dynamics365/customer-engagement/developer/store-appsource-package-azure-storage) -bir AppSource paket dosyası bir Microsoft Azure Blob Depolama hesabında depolamak ve paket dosyası paylaşmak için bir paylaşılan erişim imzası (SAS) anahtarı kullanma açıklanmaktadır. Paket dosyası, Azure depolama konumunuz için sertifika ve AppSource denemeleri ve yayın için alınır.


## <a name="next-steps"></a>Sonraki adımlar

Zaten bunu yapmadıysanız [, Dynamics 365 müşteri katılımı teklif için oluşturma](./cpp-create-offer.md).  Hazır olmanız sonra [teklifinizi yayımlayın](./cpp-publish-offer.md).
