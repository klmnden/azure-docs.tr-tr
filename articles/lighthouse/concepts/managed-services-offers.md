---
title: Yönetilen Azure Marketi'ndeki teklif Hizmetleri
description: Yönetilen hizmet sağlayıcıları kaynak yönetimi teklifleri müşteriler Azure Marketi'nde satış tekliflerini izin Hizmetleri.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: a6fcf5f1d0ac194d60f834fb8d26db019c538410
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809879"
---
# <a name="managed-services-offers-in-azure-marketplace"></a>Yönetilen Azure Marketi'ndeki teklif Hizmetleri

Bu makalede yeni **yönetilen Hizmetler** Teklif türü içinde [Azure Marketi](https://azuremarketplace.microsoft.com). Yönetilen hizmetleri sunar, müşterilere temsilci Azure resource management ile kaynak Yönetimi Hizmetleri sunmak için izin. Bu tekliflerin kullanılabilir tüm olası müşterilere veya yalnızca bir veya daha fazla belirli müşterilere yapabilirsiniz. Müşteriler, doğrudan bu yönetilen Hizmetleri ile ilişkili maliyetler için fatura olduğundan, Microsoft tarafından hiçbir Ücretlerle vardır.

## <a name="understand-managed-services-offers"></a>Yönetilen hizmetler tekliflerini anlama

Azure kaynak yönetimi temsilcisi için yönetilen hizmetler sunar müşteri sayısını artırma kolaylaştırabilir. Bir müşteri satın alan Azure marketi'ndeki teklif sonra bunların hangi abonelikler ve/veya kaynak grupları eklendi, böylece kuruluşunuzdaki kullanıcılar, müşteri içinden yönetim görevleri gerçekleştirebilir belirtilen gerektiğini belirtin mümkün olacaktır, Kuruluşunuzun kiracısı.

Bundan sonra başka bir eylem müşteri veya hizmet sağlayıcısına göre müşteri çözümü eklemek için gereklidir. Teklife tanımladığınızda olmasıdır [bulut iş ortağı portalı](https://cloudpartner.azure.com/), Azure AD kullanıcıları, grupları belirten bir bildirim oluşturmak ve Azure'ı kullanarak müşteri kaynaklarına erişimi olacak hizmet ilkeleri kaynak temsilcisi yönetimi. rolleri yanı sıra, kendi erişim düzeyini tanımlar. Bir dizi bireysel kullanıcı veya uygulama hesapları yerine bir Azure AD grubu için izinler atayarak, ekleyebilir veya bireysel kullanıcılar, erişim gereksinimlerinizin değişmesi kaldırılır.

## <a name="public-and-private-offers"></a>Genel ve özel teklifler

Her yönetilen Hizmetleri teklifi, bir veya daha fazla plan içerir. Bu planlar, özel veya genel olabilir.

Belirli müşterilerin teklifinizi sınırlandırmak istiyorsanız, özel bir plan yayımlayabilirsiniz. Bunu yaptığınızda, plan için belirli yalnızca satın alınabileceği] abonelik sağladığınız kimlik. Daha fazla bilgi için bkz. [özel teklifler](https://docs.microsoft.com/azure/marketplace/private-offers).

Genel planları hizmetlerinizi yeni müşterilere tanıtın olanak tanır. Yalnızca müşterinin kiracısına sınırlı erişim gerektirdiğinde bu genellikle daha uygundur. İsterseniz, kuruluş ek erişim vermek bir müşteriyle ilişki kurduktan sonra yalnızca o müşteriye ait ya da yeni bir özel planı yayımlayarak ya da bunu yapabilirsiniz [ekleme daha fazla Azure'ı kullanarak erişim için bunları Resource Manager şablonları](../how-to/onboard-customer.md).

Bir plan genel olarak yayımlandıktan sonra özel değiştiremezsiniz olduğunu aklınızda bulundurun. Ayrıca, bunu yapmayı tercih ederseniz planı tamamen satış durdurabilirsiniz, ancak belirli müşterilere veya hatta müşteriler, belirli bir sayıda planın genel kullanılabilirlik kısıtlayamazsınız.

Uygunsa, genel ve özel planları aynı teklife dahil edebilirsiniz.

## <a name="publish-managed-service-offers"></a>Yönetilen hizmet teklifleri yayımlama

Yönetilen hizmetler teklifi yayımlama öğrenmek için bkz. [yönetilen Hizmetleri teklifi Azure Marketinde yayımlama](../how-to/publish-managed-services-offers.md). Bulut iş ortağı portalını kullanarak Azure Marketi'nde yayımlama hakkında genel bilgi için bkz. [Azure Market ve Appsource'ta yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide) ve [Azure yönetmek ve AppSource Market teklifleri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure kaynak yönetimi temsilcisi](azure-delegated-resource-management.md) ve [kiracılar arası yönetim deneyimleri](cross-tenant-management-experience.md).
- [Yönetilen hizmetler teklifleri yayımlama](../how-to/publish-managed-services-offers.md) Azure Market'te.
