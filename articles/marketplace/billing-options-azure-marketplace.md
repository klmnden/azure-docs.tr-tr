---
title: Azure Marketi'nde faturalandırma seçenekleri | Azure
description: Yayımcılar Azure Marketi faturalandırma seçenekleri.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/05/2018
ms.author: ellacroi
ms.openlocfilehash: d0dbf603c600639679e121723556a7f3ceb17e37
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158990"
---
# <a name="billing-options-in-the-azure-marketplace"></a>Azure Marketi'nde faturalandırma seçenekleri

Bu makalede kullanılabilen faturalandırma seçenekleri [Azure Marketi](https://azuremarketplace.microsoft.com).

## <a name="commercial-considerations-in-the-marketplace"></a>Market'te ticari konuları
Aşağıdaki liste türleri için gelir Market paylaşmadığından: 
*   Liste
*   Deneme
*   Kendi lisansını getir (KLG) faturalandırma modeliyle transact

Market'te vitrinler katılmak için faturalandırılan ek ücretler kullanmadığınız.

Daha fazla bilgi için [Microsoft Azure Marketi katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies).  

## <a name="pay-as-you-go-and-byol-billing-options"></a>Kullandıkça Öde ve faturalandırma seçenekleriyle KLG
Gelir lisans, kullanım tabanlı yazılım yayımlama bir seçenek olarak Kullandıkça Öde faturalandırma modeli kullandığınız zaman içinde bir % 80/sizinle Microsoft arasındaki bölme % 20 paylaşılmaz. Tek bir teklif her iki Kullandıkça Öde ve faturalama modelleri KLG kullanarak fiyatını. İki faturalandırma modelimiz teklif düzeyinde ayrı SKU'ları bir arada. Bulut iş ortağı Portalı'nda teklifinizi faturalandırma modelleri yapılandırın. 

Aşağıdaki örnekleri dikkate alın:
*   Kullandıkça Öde seçeneği etkinleştirirseniz, aşağıdaki sonucu vardır:
    | Lisansınızı maliyeti | saat başına 1,00 $ |
    |:--- |:--- |
    | Azure kullanım maliyeti (D1/1-çekirdek) | saat başına 0.14 |
    | *Müşteri, Microsoft tarafından faturalandırılır* | *saat başına 1,14* |

    Microsoft, bu senaryoda, yayımlanmış VM görüntülerinden birini kullanmak için saat başına 1,14 düzenler:
    | Microsoft faturalar | saat başına 1,14 |
    |:--- |:--- |
    | Microsoft, lisans maliyeti 80 oranında öder | saat başına 0,80 $ |
    | Microsoft, lisans maliyeti %20 tutar. | saat başına $0,20 |
    | Microsoft Azure kullanım maliyeti tutar. | saat başına 0.14 |

*   KLG seçeneği etkinleştirirseniz, aşağıdaki sonucu vardır:
    | Lisansınızı maliyeti | Lisans ücreti anlaşma ve sizin tarafınızdan faturalandırılır |
    |:--- |:--- |
    | Azure kullanım maliyeti (D1/1-çekirdek) | saat başına 0.14 |
    | *Müşteri, Microsoft tarafından faturalandırılır* | *saat başına 0.14* |

    Microsoft, bu senaryoda, yayımlanmış VM görüntülerinden birini kullanmak için saat başına 0.14 düzenler: 
    | Microsoft faturalar | saat başına 0.14 |
    |:--- |:--- |
    | Microsoft Azure kullanım maliyeti tutar. | saat başına 0.14 |
    | Microsoft, lisans maliyeti %0 tutar. | saat başına 0,00 ABD Doları |

## <a name="single-billing-and-payment-methods"></a>Tek bir faturalandırma ve ödeme yöntemleri
Seçeneği Market'te yayımlama Transact kullanmanın önemli bir avantajı lisanslama maliyetleri ve Azure kullanım müşterinize doğrudan tek faturalandırılır olmasıdır.

Bu senaryoda Microsoft düzenler ve sizin adınıza toplar. Microsoft faturalama, müşteri kendi tedarik ilişki oluşturmak için gereksinimini ortadan kaldırır. Tek bir fatura zamandan ve kaynaklardan tasarruf edebilirsiniz. Ayrıca yardımcı fatura toplama işlemini yerine satışı giriş üzerinde odaklanın. 

## <a name="enterprise-agreement"></a>Kurumsal Anlaşma  
Microsoft Kurumsal anlaşması müşterisiyseniz, Microsoft ürünleri için ödeme yapmayı Kurumsal anlaşmanızı kullanabilirsiniz. Azure kullanımınız için de dahil olmak üzere ürünler için fatura. Ödeme Kurumsal sözleşmenizi kullanarak, yazılım lisans ve bulut Hizmetleri için en az üç yıl kuruluşlar için tasarlanmıştır. Bir yapmak yerine ödemeler kullanıma yayılır. Peşin ödeme yaparak. Bir Kullandıkça Öde yayımlama seçeneğini kullanırsanız, yazılım lisansı maliyetleri faturalandırması üç aylık Kurumsal Anlaşma fazla kullanım fatura döngüsü izler.  

### <a name="monetary-commitment"></a>Parasal taahhüt
Kurumsal Sözleşme müşterisi iseniz, Azure sözleşmenize ekleyebilirsiniz. Azure için önden parasal taahhütte bulunarak Azure sözleşmenize ekleyin. Parasal taahhüt yıl boyunca kullanılır. Taahhüdünüz, Azure hizmetlerini herhangi bir birleşimini içerir.

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [Azure Market'te ve Appsource'ta yayımlama Kılavuzu](./marketplace-publishers-guide.md).
