---
title: SUSE Linux planları - Azure rezervasyon satın alın | Microsoft Docs
description: SUSE kullanımınız için ön ödeme ve, Kullandıkça Öde maliyetlerinden tasarruf hakkında bilgi edinin.
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
ms.date: 08/29/2018
ms.author: yashar
ms.openlocfilehash: 9c3976a5fa98049de03f2a65b71f1fc927947142
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307833"
---
# <a name="prepay-for-suse-software-plans-from-azure-reservations"></a>Azure ayırmalardan SUSE yazılım planları için ön ödeme

SUSE kullanımınız için ön ödeme ve, Kullandıkça Öde maliyetlerinden tasarruf edin. İndirimler yalnızca SUSE ölçümleri ve değil, sanal makine kullanımı için geçerlidir. Sanal makineler, ayrı olarak daha fazla tasarruf etmek için rezervasyon satın alabilirsiniz.

Azure portalında SUSE yazılım planları satın alabilirsiniz. Bir plan satın almak için:

- En az bir kuruluş veya Kullandıkça Öde aboneliğine sahip rolünde olması gerekir.
- Kurumsal abonelikler için rezervasyon satın alma işlemleri içinde etkinleştirilmelidir [EA portal](https://ea.azure.com).
- Bulut çözümü sağlayıcısı (CSP) programı için yönetim aracılarını veya satış aracılarının SUSE planları satın alabilirsiniz.

## <a name="buy-a-suse-software-plan"></a>SUSE yazılım planı satın alın

1. Git [ayırmaları](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) Azure portalında.
1. Seçin **Ekle** ve SUSE Linux'ı seçin.
1. Gerekli alanları doldurun. Tüm SUSE Linux VM ne satın öznitelikleri eşleşen indirim alır. Kapsam ve seçilen miktar indirim almak dağıtımları gerçek sayısını bağlıdır.

    | Alan      | Açıklama|
    |:------------|:--------------|
    |Ad        |Bu satın alma adı.|
    |Abonelik|Bu plan için ödeme yapmak üzere kullanılan abonelik. Aboneliğinizin ödeme yöntemini, ön maliyet ayırma için ücretlendirilir. Abonelik, kurumsal anlaşma (teklif numarası: MS-AZR-0017P) veya Kullandıkça Öde (teklif numarası: MS-AZR-0003P) türündedir. Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.|
    |Kapsam       |Kapsam bir abonelik veya birden çok abonelik (paylaşılan kapsamı) ele. Seçerseniz: <ul><li>Bu abonelikte SUSE Linux kullanımı tek bir abonelik - planı indirim uygulanır. </li><li>Paylaşılan - planı indirim SUSE Linux kullanımı herhangi bir abonelik, fatura bağlamı içinde uygulanır. Kurumsal müşteriler için Paylaşılan kapsam kayıt ve kayıt (geliştirme ve test abonelikleri) hariç tüm aboneliklere dahildir. Kullandıkça Öde müşterileri için paylaşılan tüm Kullandıkça Öde abonelikleri Hesap Yöneticisi tarafından oluşturulan kapsamdır.</li></ul>|
    |Yazılım planı     |SUSE Linux planı seçin. Satın almak hangi tanımlama konusunda yardım için bkz: [SUSE Linux Enterprise yazılım ayırma indirimi nasıl uygulandığını anlamanız](../../billing/billing-understand-suse-reservation-charges.md).|
    |VM boyutu     |SUSE Linux fiyatlandırma, VM Vcpu sayısına bağlıdır. SUSE Linux vm'lerde Vcpu sayısını temsil eden bir seçenek belirleyin.|
    |Sözleşme Dönemi        |Bir yıl veya üç yıl.|
    |Miktar    |Bu SUSE Linux plan için satın alıyorsunuz sanal makine sayısı. Fatura indirim almak SUSE Linux örnekleri çalışan sayısını miktarıdır.|
1. **Satın al**'ı seçin.
1. Seçin **bu rezervasyonu görüntüle** satın alma işleminizi durumunu görmek için.

Ayırma indirimi, tüm çalışan SUSE sanal ayırma eşleşen makinelere otomatik olarak uygulanır. İndirim yalnızca SUSE ölçüm için geçerlidir. Bu planı tarafından VM işlem ücretlerini kapsamaz.

## <a name="discount-applies-to-different-vm-sizes-with-instance-size-flexibility"></a>İndirim örneği boyutu esnekliği ile farklı VM boyutları için geçerlidir.

Ayrılmış VM örnekleri gibi SUSE Linux planları örneği boyutu esnekliği sunar. Başka bir deyişle, satın aldığınız SUSE planından farklı bir boyutta bir VM dağıtımı yaparken bile, indirim uygulanır. Daha fazla bilgi için [SUSE Linux Enterprise yazılım ayırma indirimi nasıl uygulandığını anlamanız](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="cancellation-and-exchanges-not-allowed"></a>İptal ve değişimleri izin verilmiyor

İptal edemezsiniz veya satın aldığınız bir SUSE planı exchange. Doğru planı satın emin olmak için kullanımınızı kontrol edin. Satın almak hangi tanımlama konusunda yardım için bkz: [SUSE Linux Enterprise yazılım ayırma indirimi nasıl uygulandığını anlamanız](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyon yönetme konusunda bilgi almak için bkz: [Azure'un ayırmaları](../../billing/billing-manage-reserved-vm-instance.md).

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](../../billing/billing-save-compute-costs-reservations.md)
- [Azure'da ayırmalarını yönetme](../../billing/billing-manage-reserved-vm-instance.md)
- [SUSE ayırma indirimi nasıl uygulanacağını anlama](../../billing/billing-understand-suse-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](../../billing/billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](../../billing/billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.