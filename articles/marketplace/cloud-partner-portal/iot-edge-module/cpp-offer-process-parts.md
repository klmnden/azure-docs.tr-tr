---
title: Azure IOT Edge modülü teklifi yayımlama genel bakış | Microsoft Docs
description: IOT Edge modülü yayımlama işlemine genel bakış, Azure Marketi'nde teklif.
services: Azure, Marketplace, Cloud Partner Portal
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 68c58c16d7083182d412adb6ed0d243a3291cc42
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49431842"
---
# <a name="iot-edge-module-offer-publishing-overview"></a>IOT Edge modülü teklifi yayımlama genel bakış

Bu makalede, yayımlama işlemi ve bir IOT Edge modülü teklif anahtar bölümlerini açıklar. Bu makalede yayımlama için teklifinizi yayımlamaya yönelik bir başlangıç noktası olarak kullanın. [Azure Marketi](https://azuremarketplace.microsoft.com).

## <a name="publishing-process"></a>Yayımlama işlemi

IOT Edge modülü teklif yayımlamak için üst düzey adımlar şunlardır:

1. Teklif oluşturma<br> Bu teklif hakkında ayrıntılı bilgi sağlar. Bu bilgiler içerir: pazarlama malzemeleri, destek bilgileri ve varlık belirtimleri teklif açıklaması.

2. İş ve teknik varlıkları oluşturma<br> İş varlıkları (yasal belgeler ve pazarlama malzemeleri) ve ilişkili çözümü (bir Azure Container Registry'de barındırılan IOT Edge modülü görüntüler. teknik varlıkları oluşturma

3. SKU oluşturma<br> Bir teklifle ilgili fiyatlarını oluşturun. Yayımlama planlıyorsanız her görüntü için benzersiz bir SKU gereklidir.

4. Onayla ve teklifi yayımlama <br>Bu teklif ve teknik varlıkları tamamlandıktan sonra teklifi gönderebilirsiniz. Bu gönderi, yayımlama işlemi başlar. Bu işlem sırasında çözümü, doğrulanmış sertifikalı, ardından "Azure Marketi'nde etkin hale gelir" test edilir.

## <a name="parts-of-an-offer"></a>Bir teklif bölümleri

Aşağıdaki makaleleri bir IOT Edge modülü teklif anahtar bölümleri kapsar.

- [Önkoşullar](./cpp-prerequisites.md) <br>Bu makalede, teknik listelenir ve oluşturabilir veya bir IOT Edge modülü yayımlama önce iş gereksinimleri sunar.
- [IOT Edge modülü teknik varlıkları hazırlama](./cpp-create-technical-assets.md) <br>Bu makalede bir IOT Edge modülü için teknik varlıkları hazırlamayı öğrenin. Bu varlıklar, IOT Edge modülü, Azure Marketi'nde yayımlanmadan önce tüm gerekli teknik ölçütlerini karşılamalıdır.
- [IoT Edge modülü teklifi oluşturma](./cpp-create-offer.md) <br>Bu makalede kullanarak yeni bir IOT Edge modülü Teklif girişi oluşturmak için gerekli adımları listeleyen [bulut iş ortağı portalı](https://cloudpartner.azure.com).
- [IOT Edge modülü teklifi yayımlama](./cpp-publish-offer.md)<br> Bu makalede, Azure Market'te yayımlamak için bir teklif göndermek açıklar.

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [teknik ve işle ilgili gereksinimleriniz](./cpp-prerequisites.md) bir IOT Edge modülü, Microsoft Azure Marketi'nde yayımlama.
