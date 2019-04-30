---
title: Azure kapsayıcılar görüntü teklifi yayımlama | Microsoft Docs
description: Nasıl bir Azure kapsayıcı teklif yayımlanır.
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
ms.date: 11/01/2018
ms.author: pbutlerm
ms.openlocfilehash: 7533d1a133c9c474bc39f0f64c5f1a8183ab30f8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61472892"
---
# <a name="publish-container-offer"></a>Kapsayıcı teklifini yayımlama

 Kullanarak yeni bir teklif oluşturduktan sonra **yeni teklif** sayfasında teklif yayımlayabilirsiniz. Seçin **Yayımla** yayımlama işlemini başlatmak için.

Aşağıdaki diyagramda "Canlı gitmek" bir teklif için yayımlama işlemi ana adımları gösterir.

![Kapsayıcı teklif için yayımlama adımları](./media/offer-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Yayımlama adımları ayrıntılı bir açıklaması

Aşağıdaki tabloda her yayımlama adımlarını açıklar. Her adımı tamamlamak için tahmini bir saat de verilir.


|  **Yayımlama Adım**           | **saat**    | **Açıklama**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Önkoşulları doğrulama         | 15 dakika   | Bilgi sunan ve ayarlar doğrulanır sunar.                        |
| Sertifika                  | 1 hafta | Teklif, Azure sertifika ekibi tarafından analiz edilir. Bu teklif, virüsler, kötü amaçlı yazılım, emniyet uyumluluk ve güvenlik sorunları için taranır. Bu teklif, tüm uygunluk ölçütlerini karşıladığını görmek için denetlenir. Daha fazla bilgi için [önkoşulları](./cpp-prerequisites.md) ve [teknik varlıklarınızı hazırlama](./cpp-create-technical-assets.md). Bir sorun bulunursa geri bildirim'ın sağlanır. |
| Paketleme | 1 saat  | Teklife ilişkin teknik varlıkları müşteri kullanılmak üzere hazırlanmıştır ve müşteri adayı sistemleri yapılandırılır ve kurulumu. |
|  Yayımcı oturum kapatma             |  -        | Son yayımcı gözden geçirme ve teklif Canlı geçmeden önce onay. Teklifinizi (adımlarda teklif bilgi) seçili Aboneliklerdeki tüm gereksinimleri karşıladığından emin doğrulamak için dağıtabilirsiniz.  Seçin **Go Live** için teklifinizi sonraki adıma geçebilirsiniz. |
| Paketleme                 | 1 saat | Tamamlanmış teklif Market üretim sistemlerine ve bölgelerde çoğaltılır. | 
| Canlı                           | 4 gün |Teklif serbest, gerekli bölgelerde çoğaltılır ve genel kullanıma sunulan. |

Yayımlama işleminin tamamlanması 10 iş günü için izin ve teklif yayımlanır. Yayımlama işlemini tamamladıktan sonra kapsayıcı teklifinizi listelenir [Microsoft Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Marketi'nde var olan bir kapsayıcı teklifi güncelleştirme](./cpp-update-existing-offer.md)
