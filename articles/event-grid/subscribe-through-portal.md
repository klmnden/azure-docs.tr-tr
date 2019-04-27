---
title: Portal üzerinden Azure Event Grid abonelikleri
description: Portal üzerinden Event Grid abonelikleri oluşturma işlemini açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: spelluru
ms.openlocfilehash: b54bc52a2feaf4646d801265ddb273c2c86158ee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60823561"
---
# <a name="subscribe-to-events-through-portal"></a>İçin portal aracılığıyla olaylara abone olma

Bu makalede, portal üzerinden Event Grid abonelikleri oluşturma açıklanır.

## <a name="create-event-subscriptions"></a>Olay abonelikleri oluşturma

İçin desteklenen bir Event Grid aboneliği oluşturmak için [olay kaynakları](event-sources.md), aşağıdaki adımları kullanın. Bu makalede bir Azure aboneliği için Event Grid aboneliği oluşturmak nasıl gösterir.

1. **Tüm Hizmetler**’i seçin.

   ![Tüm hizmetleri seçin](./media/subscribe-through-portal/select-all-services.png)

1. Arama **Event Grid abonelikleri** ve kullanılabilir seçenekler arasından seçin.

   ![Arama](./media/subscribe-through-portal/search.png)

1. **+ Olay Aboneliği**'ni seçin.

   ![Abonelik ekleme](./media/subscribe-through-portal/add-subscription.png)

1. Tür, oluşturmak istediğiniz aboneliği seçin. Örneğin, bir Azure aboneliği için olaylara abone olmak için seçin **Azure abonelikleri** ve hedef aboneliği.

   ![Azure aboneliği seçin](./media/subscribe-through-portal/azure-subscription.png)

1. Bu olay kaynağı için tüm olay türlerine abone olmak için tutmak **tüm olay türlerine abone** teslim seçeneği. Aksi takdirde, bu abonelik için olay türlerini seçin.

   ![Olay türlerini seçin](./media/subscribe-through-portal/select-event-types.png)

1. Olayları ve bir abonelik adı işleme için uç nokta gibi bir olay aboneliği hakkında ek ayrıntılar sağlayın.

   ![Abonelik ayrıntılarını sağlayın](./media/subscribe-through-portal/provide-subscription-details.png)

1. Geçersiz yazımın ardından gerçekleşen etkinleştirin ve yeniden deneme ilkeleri özelleştirmek için **ek özellikler**.

   ![Ek özellikleri seçin](./media/subscribe-through-portal/select-additional-features.png)

1. Kapsayıcı teslim olmayan olayları depolamak için kullanarak ve yeniden deneme nasıl gönderileceğini seçin.

   ![Geçersiz yazımın ardından gerçekleşen ve yeniden etkinleştirme](./media/subscribe-through-portal/set-deadletter-retry.png)

1. İşiniz bittiğinde **Oluştur**’u seçin.

## <a name="create-subscription-on-resource"></a>Kaynak abonelik oluşturma

Bazı olay kaynakları bu kaynak için portal arabirimi aracılığıyla bir olay aboneliği oluşturma desteği. Olay kaynağını seçin ve Ara **olayları** sol bölmesinde.

![Abonelik ayrıntılarını sağlayın](./media/subscribe-through-portal/resource-events.png)

Portal bu kaynak için ilgili olay aboneliği oluşturmak için seçenekler sunar.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden deneme hakkında bilgi için [Event Grid iletiyi teslim ve yeniden deneme](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid ile hızla çalışmaya başlamak için bkz: [Azure Event Grid ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md).
