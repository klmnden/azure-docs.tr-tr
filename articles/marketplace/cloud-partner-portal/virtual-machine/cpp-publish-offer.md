---
title: Bir sanal makine teklifi Azure Market'te yayımlama
description: Listeleri mevcut bir sanal makine yayımlamak için gerekli adımlar Azure Market sunar.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 08/17/2018
ms.author: pabutler
ms.openlocfilehash: 6796c2871cf8a5928beed2ab557cefdfe8ecaae9
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64938606"
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

