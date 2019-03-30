---
title: Red Hat Enterprise Linux planları - Azure rezervasyon satın alın | Microsoft Docs
description: Red Hat kullanımınız için ön ödeme ve, Kullandıkça Öde maliyetlerinden tasarruf hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/18/2019
ms.author: yashar
ms.openlocfilehash: e90241d9da140ca33d1aae8ffc5636f8eb2fbc74
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58653148"
---
# <a name="prepay-for-red-hat-enterprise-linux-plans-from-azure-reservations"></a>Azure ayırmalardan Red Hat Enterprise Linux planları için ön ödeme

Red Hat kullanımınız için ön ödeme ve, Kullandıkça Öde maliyetlerinden tasarruf edin. İndirimleri, Red Hat ölçümleri ve sanal makine kullanımı değil, yalnızca geçerli. Sanal makineler, ayrı olarak daha fazla tasarruf etmek için rezervasyon satın alabilirsiniz.

Red Hat yazılım planlarını Azure portalında satın alabilirsiniz. Bir plan satın almak için:

- En az bir kuruluş veya Kullandıkça Öde aboneliğine sahip rolünde olması gerekir.
- Kurumsal abonelikler için **ayrılmış örnekleri ekleme** içinde etkinleştirilmelidir [EA portal](https://ea.azure.com). Veya bu ayarı devre dışıysa, aboneliğini bir EA yönetici olması gerekir.
- Bulut çözümü sağlayıcısı (CSP) programı için yönetim aracılarını veya satış aracılarının Red Hat planları satın alabilirsiniz.

## <a name="buy-a-red-hat-software-plan"></a>Red Hat yazılım planı satın alın

1. Git [ayırmaları](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) Azure portalında.
1. Seçin **Ekle** ve Red Hat Linux'ı seçin.
1. Gerekli alanları doldurun. Ne satın öznitelikleri eşleşen tüm Red Hat Linux VM indirim alır. Kapsam ve seçilen miktar indirim almak dağıtımları gerçek sayısını bağlıdır.

    | Alan      | Açıklama|
    |:------------|:--------------|
    |Ad        |Bu satın alma adı.|
    |Abonelik|Bu plan için ödeme yapmak üzere kullanılan abonelik. Aboneliğinizin ödeme yöntemini, ön maliyet ayırma için ücretlendirilir. Kurumsal Anlaşma abonelik türü olmalıdır (sayılar sunar: MS-AZR-0017P veya MS-AZR - 0148 P) ya da Kullandıkça Öde (sayılar sunar: MS-AZR-0003P veya MS-AZR - 0023 P). Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.|
    |Kapsam       |Kapsam bir abonelik veya birden çok abonelik (paylaşılan kapsamı) ele. Seçerseniz: <ul><li>Bu abonelikte Red Hat Linux kullanımı tek bir abonelik - planı indirim uygulanır. </li><li>Paylaşılan - planı indirim Red Hat Linux kullanımı herhangi bir abonelik, fatura bağlamı içinde uygulanır. Kurumsal müşteriler için Paylaşılan kapsam kayıt ve kayıt içinde tüm abonelikleri içerir. Kullandıkça Öde müşterileri için paylaşılan tüm Kullandıkça Öde abonelikleri Hesap Yöneticisi tarafından oluşturulan kapsamdır.</li></ul>|
    |Plan     |Red Hat Linux planı seçin. Red Hat Linux Kurumsal Yazılım ayırma indirimi nasıl uygulanacağını anlayın ne satın almak tanımlama konusunda yardım için bkz.|
    |VM boyutu     |Red Hat Linux fiyatlandırma, VM Vcpu sayısına bağlıdır. Red Hat Linux Vm'lerinizi Vcpu sayısını temsil eden bir seçenek belirleyin.|
    |Dönem        |Bir yıl veya üç yıl.|
    |Miktar    |Bu Red Hat Linux plan için satın alma sanal makine sayısı. Fatura indirim almak Red Hat Linux örnekleri çalışan sayısını miktarıdır.|
1. **Satın al**'ı seçin.
1. Seçin **bu rezervasyonu görüntüle** satın alma işleminizi durumunu görmek için.

Ayırma indirimi, tüm çalışan Red Hat sanal ayırma eşleşen makinelere otomatik olarak uygulanır. İndirim yalnızca Red Hat ölçüm için geçerlidir. Bu planı tarafından VM işlem ücretlerini kapsamaz.

## <a name="discount-applies-to-different-vm-sizes"></a>İndirimi, farklı VM boyutları için geçerlidir.

Ayrılmış VM örnekleri gibi Red Hat Linux planları örneği boyutu esnekliği sunar. Başka bir deyişle, satın aldığınız Red Hat planından farklı bir boyutta bir VM dağıtımı yaparken bile, indirim uygulanır. Red Hat Linux Kurumsal Yazılım ayırma indirimi nasıl uygulanacağını anlayın daha fazla bilgi için bkz.

## <a name="cancellation-and-exchanges-not-allowed"></a>İptal ve değişimleri izin verilmiyor

İptal edemezsiniz veya satın aldığınız bir Red Hat planı exchange. Kullanımınızı denetleyerek doğru planı satın aldığınızdan emin olun. Red Hat Linux Kurumsal Yazılım ayırma indirimi nasıl uygulanacağını anlayın ne satın almak tanımlama konusunda yardım için bkz.

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyon yönetme konusunda bilgi almak için bkz: [Azure'un ayırmaları](../../billing/billing-manage-reserved-vm-instance.md).

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](../../billing/billing-save-compute-costs-reservations.md)
- [Azure'da ayırmalarını yönetme](../../billing/billing-manage-reserved-vm-instance.md)
- Red Hat ayırma indirimi nasıl uygulanacağını anlama
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](../../billing/billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](../../billing/billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.