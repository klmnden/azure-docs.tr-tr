---
title: Azure Deniz ve bulut çözümü sağlayıcısı programı
description: Azure'ı kullanarak temsilci kaynak yönetimi, güvenlik göz önünde bulundurun ve erişim denetimi önemlidir.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 09552c66d2dc841cff8b5cb52e26dc657568e08f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809963"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure Deniz ve bulut çözümü sağlayıcısı programı

Size bir [CSP (bulut çözümü sağlayıcısı)](https://docs.microsoft.com/partner-center/csp-overview) iş ortağı, kullanarak müşterilerinizin CSP programı aracılığıyla oluşturulan Azure abonelikleri erişebilir [yönetmek adına (AOBO)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO) işlevselliği. Bu erişim, doğrudan destek, yapılandırma ve müşterilerinizin Aboneliklerini yönetmek sağlar.

AOBO mekanizması müşteri ortamlarında tam erişim verir. Temsil edilen Azure kaynak yönetimi AOBO birlikte kullanarak, kullanıcılarınız için daha ayrıntılı izinler etkinleştirerek gereksiz erişim azaltmanıza izin vererek güvenliği artırmaya yardımcı olur. 

## <a name="administer-on-behalf-of-aobo"></a>Adına Yönetim (AOBO) yönetme

AOBO, herhangi bir kullanıcı ile [yönetim Aracısı](https://docs.microsoft.com/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) Kiracı rolünde CSP programı aracılığıyla oluşturulan Azure abonelikleri AOBO erişimi olacaktır. Tüm Müşteri abonelikleri erişmesi gereken tüm kullanıcılar bu grubun bir üyesi olması gerekir. AOBO esnekliğine sahip farklı müşteriler iş ayrı grupları oluşturmak için ya da grupları veya kullanıcıları için farklı roller etkinleştirmek için izin vermez.

![Kiracı yönetim AOBO kullanma](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Temsil edilen Azure kaynak yönetimi

Kaynak Yönetimi temsilcisi Azure kullanarak, farklı gruplar farklı müşteriler veya rolleri için aşağıdaki diyagramda gösterildiği gibi atayabilirsiniz. Kullanıcılar uygun düzeyini temsil edilen Azure kaynak yönetimi üzerinden erişime sahip olacağından yönetim Aracısı rolüne sahip (ve bu nedenle tam AOBO erişime sahip) kullanıcı sayısını azaltabilirsiniz. Bu, gereksiz müşterilerinizin kaynaklara erişimi sınırlayan tarafından daha fazla güvenlik sağlar. Bu ayrıca, uygun ölçekte birden çok müşteriyi yönetiyorsanız daha fazla esneklik sağlar.

Ekleme CSP programı aracılığıyla oluşturulan bir aboneliği içinde açıklanan adımları takip eden [yerleşik kaynak yönetimi için Azure aboneliğine temsilci](../how-to/onboard-customer.md). Kiracınızda yönetici aracı rolü olan herhangi bir kullanıcı bu ekleme gerçekleştirebilirsiniz.

CSP program oluşturulmuş abonelikler için unutmayın, destek istekleri yalnızca hizmet sağlayıcısının Kiracı yönetici aracı rolde olan kullanıcılar tarafından oluşturulabilir. Temsil edilen Azure kaynak yönetimi eklenen kullanıcılar atanan kaynaklar için destek istekleri bu Aboneliklerde açmak mümkün olmayacaktır.

![Kiracı yönetim AOBO ile Azure kaynak yönetimi temsilcisi](../media/csp-2.jpg)

## <a name="partner-admin-link"></a>İş ortağı yönetim bağlantı

Eklenen aboneliklerinizi, etki müşterilerle yaşadığımız izlemek için Microsoft iş ortağı ağı (MPN) Kimliğiniz ilişkilendirebilirsiniz.

Varsa, [yönetilen hizmetler teklifi Azure Marketinde yayımlama](../how-to/publish-managed-services-offers.md), MPN Kimliğinizi, yayımcı profilinizle ilişkili ve teklife otomatik olarak ilişkilendirilir. Bu teklif aracılığıyla Azure kaynakları tarafından üretilen gelir ardından kuruluşunuza öznitelikli. Raporlama sistemleri iş ortağı merkezi veya MPN gibi ortağında attribution ortak yönetici bağlantısı (PAL) olarak görünür.

Varsa, [müşterilerin Azure için Azure Resource Manager şablonlarını kullanarak kaynak yönetimi temsilcisi](../how-to/onboard-customer.md)tanıma, müşterilerle yaşadığımız etkisi almak için MPN Kimliğinizi ilişkilendirebilirsiniz ancak gerekir el ile yapmak için. Daha fazla bilgi için bkz. [şekilde Azure hesaplarınızı için bir iş ortağı kimliği Bağla](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started). 

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [kiracılar arası yönetim deneyimleri](cross-tenant-management-experience.md).
- Hakkında bilgi edinin [bulut çözümü sağlayıcısı programı](https://docs.microsoft.com/partner-center/csp-overview).
