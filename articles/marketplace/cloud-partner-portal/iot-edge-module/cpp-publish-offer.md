---
title: Azure IOT Edge modülü teklifi yayımlama | Microsoft Docs
description: Bir IOT Edge modülü teklifini yayımlamanın nasıl.
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
ms.openlocfilehash: 670a2ce205ba5e64418eccc41add36cbecc28212
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49431756"
---
# <a name="publish-iot-edge-module-offer"></a>IOT Edge modülü teklifi yayımlama

 Şirket bilgileri sağlayarak yeni bir teklif oluşturduktan sonra **yeni teklif** sayfasında teklif yayımlayabilirsiniz. Seçin **Yayımla** yayımlama işlemini başlatmak için.

Aşağıdaki diyagramda "Canlı gitmek" bir teklif için yayımlama işlemi ana adımları gösterir.

![IOT Edge modülü için yayımlama adımları sunar.](./media/iot-edge-module-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Yayımlama adımları ayrıntılı bir açıklaması

Aşağıdaki tabloda her adımı tamamlamak için bir zaman tahmin (maksimum) olan her yayımlama adımlarını açıklar.
<!-- P2: we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Yayımlama Adım**           | **saat**    | **Açıklama**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Önkoşulları doğrulama         | 15 dakika   | Bilgi sunan ve ayarlar doğrulanır sunar.                        |
| Sertifika                  | 1 hafta | Teklif, Azure sertifika ekibi tarafından analiz edilir. Bu adım, virüsler, kötü amaçlı yazılım, emniyet uyumluluk ve güvenlik sorunları için tarama gerçekleştirir. Ayrıca bu IOT Edge modülü teklif tüm uygunluk ölçütlerini karşıladığını doğrular (bkz [önkoşulları](./cpp-prerequisites.md) ve [teknik varlıklarınızı hazırlama](./cpp-create-technical-assets.md)). Bir sorun bulunursa geri bildirim sağlanır. |
| Paketleme | 1 saat  | Teklife ilişkin teknik varlıkları müşteri kullanılmak üzere hazırlanmıştır ve müşteri adayı sistemleri yapılandırılır ve kurulumu. |
|  Yayımcı oturum kapatma             |  -        | Son yayımcı gözden geçirme ve teklif Canlı geçmeden önce onay. Teklifinizi (adımlarda teklif bilgi) seçili Aboneliklerdeki tüm gereksinimleri karşıladığından emin doğrulamak için dağıtabilirsiniz.  Seçin **Go Live** için teklifinizi sonraki adıma geçebilirsiniz. |
| Paketleme                 | 1 saat | Sonlandırılmış bir teklifi Market'te üretim sistemlerine ve bölgelerde çoğaltılır. | 
| Canlı                           | 4 gün |Teklif serbest, gerekli bölgelerde çoğaltılır ve genel kullanıma sunulan. |

Yayımlama işleminin tamamlanması 10 iş günü için izin ve teklif yayımlanır. Yayımlama işlemini tamamladıktan sonra IOT Edge modülü teklifinizi listelenir [Microsoft Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Marketi'nde var olan bir IOT Edge modülü teklifi güncelleştirme](./cpp-update-existing-offer.md)
