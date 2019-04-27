---
title: Bir sanal makine teklifi Azure Market'te yayımlama | Microsoft Docs
description: Listeleri mevcut bir sanal makine yayımlamak için gerekli adımlar Azure Market sunar.
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
ms.topic: article
ms.date: 08/17/2018
ms.author: pbutlerm
ms.openlocfilehash: 3cf6a3d9bcb9470fd3a6bd4fef964c1966adfa1a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60844260"
---
# <a name="publish-a-virtual-machine-offer"></a>Bir sanal makine teklifi yayımlama

 Teklif Portalı'nda tanımlandığı ve ilgili teknik varlıkları oluşturulan sonra son adım, teklif yayımlama için gönderme sağlamaktır. Aşağıdaki diyagram, "Canlı Git" yayımlama işlemi ana adımları gösterir:

![Sanal makine için yayımlama adımları sunar.](./media/publishvm_013.png)

Aşağıdaki tabloda, adımları açıklar ve bunların tamamlanmasını en uzun süreyi tahmin sağlar:
<!-- we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Yayımlama Adım**           | **saat**    | **Açıklama**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Önkoşulları doğrulama         | 15 dakika   | Bilgi sunan ve ayarlar doğrulanır sunar.                        |
| Sürücü doğrulama (isteğe bağlı) test etme | 2 saat | Test Sürüşü etkinleştirmek için seçtiyseniz, Microsoft Test Sürüşü yapılandırma, dağıtım ve seçili bölgeler arasında çoğaltma doğrular. |
| Sertifika                  | 3 gün | Teklif, Azure sertifika ekibi tarafından analiz edilir. Bu adım, virüsler, kötü amaçlı yazılım, emniyet uyumluluk ve güvenlik sorunları için tarama gerçekleştirir. Bir sorun bulunursa geri bildirim sağlanır. |
| Sağlama                   | 4 gün   | VM teklifi Market'te üretim sistemleri çoğaltılır.               |
| Paketleme ve müşteri adayı oluşturma kaydı | \< 1 saat  | Teklife ilişkin teknik varlıkları müşteri kullanılmak üzere hazırlanmıştır ve müşteri adayı sistemleri yapılandırılır ve kurulumu. |
|  Yayımcı oturumu kapatma             |  -        | Son yayımcı gözden geçirme ve teklif Canlı geçmeden önce onay. Teklifinizi (adımlarda teklif bilgi) seçili Aboneliklerdeki tüm gereksinimleri karşıladığından emin doğrulamak için dağıtabilirsiniz.  |
| Sağlama                   | 4 gün | Sonlandırılmış VM teklifi Market'te üretim sistemlerine ve bölgelerde çoğaltılır. | 
| Canlı                           | 4 gün | VM teklifi serbest, gerekli bölgelerde çoğaltılır ve genel kullanıma sunulan. |
|  |  |

Bu işlemin tamamlanması için 16 güne kadar bekleyin.  Bu yayımlama adımları olduktan sonra VM teklifinizi listelenir [Microsoft Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/). 

