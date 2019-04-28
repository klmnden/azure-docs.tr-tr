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
ms.date: 11/06/2018
ms.author: pbutlerm
ms.openlocfilehash: 5b4d4471d9c77b5d13dfd5f8c2e9394b1c2d2a87
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60910710"
---
# <a name="iot-edge-module-offer-publishing-overview"></a>IOT Edge modülü teklifi yayımlama genel bakış

<table> <tr> <td>Bu bölümde, Microsoft yeni bir Azure IOT Edge modülü teklif yayımlamak açıklanmaktadır <a href="https://azuremarketplace.microsoft.com">Azure Marketi</a>. IOT Edge modülü bir IOT Edge Cihazınızda çalıştırmak için yaptığı Docker ile uyumlu bir kapsayıcıdır. Azure IOT Edge modülleri, IOT Edge tarafından yönetilen bir hesaplama birimi en küçük olan ve Azure hizmetlerine veya özel bir çözüm kod içerebilir. </td> <td><img src="./media/iotedge-icon1.png"  alt="Azure IoT Edge module icon" /></td> </tr> </table>

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
- [IoT Edge modülü teklifi yayınlama](./cpp-publish-offer.md)<br> Bu makalede, Azure Market'te yayımlamak için bir teklif göndermek açıklar.

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [teknik ve işle ilgili gereksinimleriniz](./cpp-prerequisites.md) bir IOT Edge modülü, Microsoft Azure Marketi'nde yayımlama.
