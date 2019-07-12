---
title: Azure kaynak yönetimi - Azure Deniz temsilcisi.
description: Yönetilen hizmet sağlayıcıları kaynak yönetimi teklifleri müşteriler Azure Marketi'nde satış tekliflerini izin Hizmetleri.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: cec6453cdf339e82420a1b12af6c8e60526fdc03
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809970"
---
# <a name="azure-delegated-resource-management"></a>Temsil edilen Azure kaynak yönetimi

Temsil edilen Azure kaynak yönetimi, Azure Deniz'in temel bileşenlerinden biridir. İle temsil edilen Azure kaynak yönetimi, hizmet sağlayıcıları ile çevikliği ve duyarlık uygun ölçekte temsilci kaynaklarını yönetme sırasında müşteri katılımı ve ekleme deneyimleri basitleştirebilir.

## <a name="what-is-azure-delegated-resource-management"></a>Azure nedir, kaynak yönetimi temsilcisi?

Başka bir kiracıda oturum bir kiracıdaki kaynakların mantıksal bir projeksiyon temsil edilen Azure kaynak yönetimi sağlar. Bu yetkili kullanıcıların bir Azure Active Directory (Azure AD) kiracısı içinde farklı bir Azure AD genelinde yönetim işlemlerini gerçekleştirmenize olanak tanır, müşterilerine ait kiracılar. Hizmet sağlayıcıları kendi Azure AD kiracısına oturum açabilir ve temsilci müşteri aboneliği ve kaynak gruplarını çalışma yetkisi vardır. Bu, bunları her bir müşterinin Kiracı için oturum açmak zorunda kalmadan, müşterileri adına yönetim işlemlerini gerçekleştirmenize olanak tanır.

> [!NOTE]
> Temsil edilen Azure kaynak yönetimi basitleştirmek için kendi kiracılar arası Management birden çok Azure AD kiracılarıyla olan kuruluş içinde de kullanılabilir.

İle temsil edilen Azure kaynak yönetimi, müşterinin Kiracı içinde bir hesap olması veya bir müşterinin Kiracı ortak sahibi olan yetkili kullanıcıların doğrudan bir müşteri aboneliğinde bağlamında çalışabilir. Ayrıca [görüntüleme ve tüm yetkilendirilmiş müşteri abonelikleri yeni yönetme **müşterilerimin** sayfa](../how-to/view-manage-customers.md) Azure portalında.

[Kiracılar arası yönetim deneyimi](cross-tenant-management-experience.md) daha verimli bir şekilde Azure Yönetim Hizmetleri ile yardımcı ister Azure İlkesi, Azure Güvenlik Merkezi ve daha fazlası. Tüm hizmet sağlayıcısı etkinlik, hizmet sağlayıcısının hem müşterinin kiracıda depolanır ve Etkinlik günlüğünü izlenir. Başka bir deyişle müşteri ve hizmet sağlayıcısı herhangi bir değişiklik ile ilişkili kullanıcı kolayca belirleyebilirsiniz.

Bir müşteri Azure ile yerleşik temsilci zaman kaynak yönetimi, bunlar yeni erişim kurcalayabileceğimi **hizmeti sağlayıcıları** yapabileceği Azure portalında sayfası [onaylayın ve bunların sunar ve hizmet sağlayıcıları, yönetin ve kaynaklara temsilci](../how-to/view-manage-service-providers.md). Müşteri, bir hizmet sağlayıcısı için erişimi iptal etmek hiç olmadığı kadar istiyorsa, bunlar burada herhangi bir zamanda bunu yapabilirsiniz.

Yapabilecekleriniz [yeni yönetilen hizmetler Teklif türü Azure Marketi'nde yayımlayabileceğiniz](../how-to/publish-managed-services-offers.md) kolayca müşterilerin Azure kaynak yönetimi temsilcisi. Alternatif olarak, [Azure Resource Manager şablonları dağıtarak ekleme işlemini tamamlamaya](../how-to/onboard-customer.md).

## <a name="how-azure-delegated-resource-management-works"></a>Azure kaynak yönetimi için nasıl temsilci çalışır

Yüksek düzeyde, işte Azure kaynak yönetimi works nasıl temsilci:

1. Bir hizmet sağlayıcısı olarak, grupları, hizmet sorumluları veya kullanıcılar müşterinin Azure kaynaklarını yönetmek için gereken erişim (roller) belirleyin. Hizmet sağlayıcısının Kiracı kimliği kullanılarak tanımlanmış bir teklif için gerekli erişim yanı sıra erişim tanımını içeren **Principalıd** eşlenen kiracınızdan kimlikleri [yerleşik  **roleDefinition** değerleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) (Katkıda bulunan, sanal makine katkıda bulunan, okuyucu, vb.).
2. Bu erişim belirtin ve yerleşik Azure müşteriye temsilci iki yoldan biriyle kaynak yönetimi:
   - [Bir Azure Market'te yönetilen hizmetler teklifi yayımlama](../how-to/publish-managed-services-offers.md) (özel veya genel), müşterinin kabul eder
   - [Müşterinin kiracısına bir Azure Resource Manager şablonu dağıtma](../how-to/onboard-customer.md) bir veya daha fazla belirli bir abonelik veya kaynak grupları
3. Müşteri eklendi sonra yetkili kullanıcıların hizmet sağlayıcısı kiracınızda oturum açın ve tanımladığınız erişimi göre belirli müşteri kapsamda yönetim görevlerini gerçekleştirebilirsiniz.

## <a name="support-for-azure-delegated-resource-management"></a>Kaynak yönetimi için Azure destek temsilcisi

İlgili yardıma ihtiyacınız olursa Azure kaynak yönetimi temsilcisi, Azure portalında bir destek isteği açabilirsiniz. İçin **sorun türü**, seçin **teknik**. Bir abonelik seçin ve ardından **kaynak yönetimi temsilcisi** (altında **izleme ve Yönetim**).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [kiracılar arası yönetim deneyimleri](cross-tenant-management-experience.md).
- Hakkında bilgi edinin [yönetilen hizmetler Azure Marketi'ndeki teklif](managed-services-offers.md).