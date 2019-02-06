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
ms.date: 01/18/2019
ms.author: yashar
ms.openlocfilehash: 4f70a34febcf0b39d051053a6ddd9abe5c9a6726
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745988"
---
# <a name="prepay-for-suse-software-plans-from-azure-reservations"></a>Azure ayırmalardan SUSE yazılım planları için ön ödeme

SUSE kullanımınız için ön ödeme ve, Kullandıkça Öde maliyetlerinden tasarruf edin. İndirimler yalnızca SUSE ölçümleri ve değil, sanal makine kullanımı için geçerlidir. Sanal makineler, ayrı olarak daha fazla tasarruf etmek için rezervasyon satın alabilirsiniz.

Azure portalında SUSE yazılım planları satın alabilirsiniz. Bir plan satın almak için:

- En az bir kuruluş veya Kullandıkça Öde aboneliğine sahip rolünde olması gerekir.
- Kurumsal abonelikler için **ayrılmış örnekleri ekleme** içinde etkinleştirilmelidir [EA portal](https://ea.azure.com). Veya bu ayarı devre dışıysa, aboneliğini bir EA yönetici olması gerekir.
- Bulut çözümü sağlayıcısı (CSP) programı için yönetim aracılarını veya satış aracılarının SUSE planları satın alabilirsiniz.

## <a name="buy-a-suse-software-plan"></a>SUSE yazılım planı satın alın

1. Git [ayırmaları](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) Azure portalında.
1. Seçin **Ekle** ve SUSE Linux'ı seçin.
1. Gerekli alanları doldurun. Tüm SUSE Linux VM ne satın öznitelikleri eşleşen indirim alır. Kapsam ve seçilen miktar indirim almak dağıtımları gerçek sayısını bağlıdır.

    | Alan      | Açıklama|
    |:------------|:--------------|
    |Ad        |Bu satın alma adı.|
    |Abonelik|Bu plan için ödeme yapmak üzere kullanılan abonelik. Aboneliğinizin ödeme yöntemini, ön maliyet ayırma için ücretlendirilir. Kurumsal Anlaşma abonelik türü olmalıdır (sayılar sunar: MS-AZR-0017P veya MS-AZR - 0148 P) ya da Kullandıkça Öde (sayılar sunar: MS-AZR-0003P veya MS-AZR - 0023 P). Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.|
    |Kapsam       |Kapsam bir abonelik veya birden çok abonelik (paylaşılan kapsamı) ele. Seçerseniz: <ul><li>Bu abonelikte SUSE Linux kullanımı tek bir abonelik - planı indirim uygulanır. </li><li>Paylaşılan - planı indirim SUSE Linux kullanımı herhangi bir abonelik, fatura bağlamı içinde uygulanır. Kurumsal müşteriler için Paylaşılan kapsam kayıt ve kayıt içinde tüm abonelikleri içerir. Kullandıkça Öde müşterileri için paylaşılan tüm Kullandıkça Öde abonelikleri Hesap Yöneticisi tarafından oluşturulan kapsamdır.</li></ul>|
    |Yazılım planı     |SUSE Linux planı seçin. Satın almanız gereken planı belirleme konusunda yardım için bkz. [SUSE Linux Enterprise yazılım ayırma indiriminin nasıl uygulandığını anlama](../../billing/billing-understand-suse-reservation-charges.md).|
    |VM boyutu     |SUSE Linux fiyatlandırma, VM Vcpu sayısına bağlıdır. SUSE Linux vm'lerde Vcpu sayısını temsil eden bir seçenek belirleyin.|
    |Sözleşme Dönemi        |Bir yıl veya üç yıl.|
    |Miktar    |Bu SUSE Linux plan için satın alıyorsunuz sanal makine sayısı. Fatura indirim almak SUSE Linux örnekleri çalışan sayısını miktarıdır.|
1. **Satın al**'ı seçin.
1. Seçin **bu rezervasyonu görüntüle** satın alma işleminizi durumunu görmek için.

Ayırma indirimi, tüm çalışan SUSE sanal ayırma eşleşen makinelere otomatik olarak uygulanır. İndirim yalnızca SUSE ölçüm için geçerlidir. Bu planı tarafından VM işlem ücretlerini kapsamaz.

## <a name="discount-applies-to-different-vm-sizes-with-instance-size-flexibility"></a>İndirim örneği boyutu esnekliği ile farklı VM boyutları için geçerlidir.

Ayrılmış VM örnekleri gibi SUSE Linux planları örneği boyutu esnekliği sunar. Başka bir deyişle, satın aldığınız SUSE planından farklı bir boyutta bir VM dağıtımı yaparken bile, indirim uygulanır. Daha fazla bilgi için [SUSE Linux Enterprise yazılım ayırma indirimi nasıl uygulandığını anlamanız](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="cancellation-and-exchanges-not-allowed"></a>İptal ve değişimleri izin verilmiyor

Satın aldığınız SUSE planını iptal edemez veya değiştiremezsiniz. Kullanımınızı denetleyerek doğru planı satın aldığınızdan emin olun. Satın almanız gereken planı belirleme konusunda yardım için bkz. [SUSE Linux Enterprise yazılım ayırma indiriminin nasıl uygulandığını anlama](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyon yönetme konusunda bilgi almak için bkz: [Azure'un ayırmaları](../../billing/billing-manage-reserved-vm-instance.md).

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](../../billing/billing-save-compute-costs-reservations.md)
- [Azure'da ayırmalarını yönetme](../../billing/billing-manage-reserved-vm-instance.md)
- [SUSE ayırma indirimi nasıl uygulanacağını anlama](../../billing/billing-understand-suse-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](../../billing/billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](../../billing/billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).