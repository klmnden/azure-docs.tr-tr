---
title: Azure IOT Edge modülü önkoşulları | Microsoft Docs
description: IOT Edge modülü yayımlamak için Önkoşullar.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: pbutlerm
ms.openlocfilehash: a4f1023bdf8a49fccbbda1fd0dc537f83a3acee1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60910345"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>IOT Edge modülü yayımlama önkoşulları

Bu makalede bir IOT Edge modülü teklifini yayımlamanın önkoşulları açıklanır.  Zaten yapmadıysanız, gözden [yayımlama Kılavuzu'nu IOT Edge modülleri](../..//iot-edge-module.md).


## <a name="publishing-prerequisites"></a>Yayımlama önkoşulları

IOT Edge modülü, Azure Marketi'nde içerik yayımlamak için aşağıdaki önkoşulları sağlamanız gerekir:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Erişim [bulut iş ortağı portalı](https://cloudpartner.azure.com/). Daha fazla bilgi için [Azure Market'te ve Appsource'ta yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Sözleşme [Azure Market hükümleri](https://azure.microsoft.com/support/legal/marketplace-terms/)
- IOT Edge modülü teknik varlığınız bir Azure Container Registry'de barındırın.  Daha fazla bilgi için [, IOT Edge modülü teknik varlık hazırlamayı öğrenin](./cpp-create-technical-assets.md)
- IOT Edge modülü meta verilerinizi kullanıma hazır olması. Örneğin, aşağıdaki varlıkları hazırlayın:
    - Bir başlık
    - Bir açıklama (HTML biçiminde)
    - Logo görüntüsü (PNG biçiminde ve sabit görüntü boyutları 40x40px 90x90px, 115x115px, 255x115px dahil olmak üzere)
    - Kullanım ve gizlilik ilkesi koşulları
    - İçeren bir varsayılan modül yapılandırması: yollarını çiftinin istenen özellikleri, createOptions ve ortam değişkenleri.
    - Modül belgeleri
    - Destek kişileri


## <a name="next-steps"></a>Sonraki adımlar

Yapılandırmasını tamamladıktan [IOT Edge modülü teknik varlığınız hazır](./cpp-create-technical-assets.md), hazır olmanız [IOT Edge modülü teklifinizle oluşturmak](./cpp-create-offer.md). 
