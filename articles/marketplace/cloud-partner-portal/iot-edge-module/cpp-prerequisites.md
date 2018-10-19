---
title: Azure IOT Edge modülü önkoşulları | Microsoft Docs
description: IOT Edge modülü yayımlamak için Önkoşullar.
services: Azure, Marketplace, Cloud Partner Portal,
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
ms.openlocfilehash: 7271a97dc7ab9de1840d809ded0ba1c2940ed83f
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49431874"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>IOT Edge modülü yayımlama önkoşulları

Bu makalede bir IOT Edge modülü teklifini yayımlamanın önkoşulları açıklanır.

IOT Edge modülleri ve bir modül, Azure Market'te yayımlama avantajları hakkında daha fazla bilgi için bkz: [yayımlama Kılavuzu'nu IOT Edge modülleri](https://docs.microsoft.com/azure/marketplace/iot-edge-module).

## <a name="publishing-prerequisites"></a>Yayımlama önkoşulları

IOT Edge modülü, Azure Marketi'nde içerik yayımlamak için aşağıdaki önkoşulları sağlamanız gerekir:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Erişim [bulut iş ortağı portalı](https://cloudpartner.azure.com/). Daha fazla bilgi için [Azure Market'te ve Appsource'ta yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Sözleşme [Azure Market hükümleri](https://azure.microsoft.com/support/legal/marketplace-terms/)
- IOT Edge modülü teknik varlığınız bir Azure Container Registry'de barındırın.  Daha fazla bilgi için [, IOT Edge modülü teknik varlık hazırlamayı öğrenin](./cpp-create-technical-assets.md)
- IOT Edge modülü meta verilerinizi kullanıma hazır olması. Örneğin, (kapsamlı olmayan liste):
    - Bir başlık
    - Bir açıklama (HTML biçiminde)
    - Logo görüntüsü (PNG biçiminde ve sabit görüntü boyutları 40x40px 90x90px, 115x115px, 255x115px dahil olmak üzere)
    - Kullanım ve gizlilik ilkesi koşulları
    - İçeren bir varsayılan modül yapılandırması: yollarını çiftinin istenen özellikleri, createOptions ve ortam değişkenleri.
    - Belgeler
    - Destek kişileri

## <a name="next-steps"></a>Sonraki adımlar

- [IOT Edge modülü teknik varlık hazırlama](./cpp-create-technical-assets.md)
- [IOT Edge modülü teklifinizi oluşturun](./cpp-create-offer.md)
