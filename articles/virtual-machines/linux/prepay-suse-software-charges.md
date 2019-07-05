---
title: Yazılım planları - Azure ayırmaları ön ödeme
description: Nasıl, yazılım planlarını, Kullandıkça Öde maliyetlerinden tasarruf için ön ödeme öğrenin.
author: bandersmsft
manager: yashesvi
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: c8e9d07bf01536d7865bd79f667a937037d96837
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67489890"
---
# <a name="prepay-for-azure-software-plans"></a>Azure yazılım planları için ön ödeme yapma

Azure, SUSE ve RedHat yazılım kullanımı için ön ödeme, Kullandıkça Öde maliyetlerinden tasarruf. İndirimleri, SUSE ve RedHat oranları ve sanal makine kullanımı değil, yalnızca geçerli. Ek tasarruf için ayrı ayrı sanal makineler için ayırma satın alabilirsiniz.

Azure portalında, SUSE ve RedHat yazılım planları satın alabilirsiniz. Bir plan satın almak için:

- En az bir işletme veya Kullandıkça Öde fiyatlandırması ile tek bir abonelik için sahip rolü olmalıdır.
- Kurumsal abonelikler için **ayrılmış örnekleri ekleme** seçeneği etkinleştirilmelidir [EA portal](https://ea.azure.com/). Ayarı devre dışıysa, abonelik için EA yönetici olmanız gerekir.
- Bulut çözümü sağlayıcısı (CSP) programı için yönetim aracılarını veya satış aracılarının yazılım planları satın alabilirsiniz.

## <a name="buy-a-software-plan"></a>Yazılım planı satın alın

1. Azure portalında oturum açın ve gidin [ayırmaları](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
2. Tıklayın **Ekle** ve ardından satın almak istediğiniz yazılım planı seçin.
Gerekli alanları doldurun. Herhangi bir SUSE Linux VM veya RedHat ne satın öznitelikleri eşleşen VM indirim alır. Kapsam ve seçilen miktar indirim almak dağıtımları gerçek sayısını bağlıdır.
3. Bir abonelik seçin. Bu plan için ödeme yapmayı kullanılır.
Abonelik ödeme yöntemi, ön maliyet ayırma için ücretlendirilir. Kurumsal Anlaşma abonelik türü olmalıdır (sayılar sunar: MS-AZR-0017P veya MS-AZR - 0148 P) ya da Kullandıkça Öde fiyatlandırması ile tek tek anlaşma (sayılar sunar: MS-AZR-0003P veya MS-AZR-0023P).
    - Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir.
    - İçin tek bir aboneliği Kullandıkça Öde fiyatlandırması ile ücretleri aboneliğin kredi kartı veya fatura ödeme yöntemi için faturalandırılır.
4. Bir kapsam seçin. Kapsam bir abonelik veya birden çok abonelik (paylaşılan kapsamı) ele.
    - Abonelik kullanımı eşleşen tek bir abonelik - planı indirim uygulanır.
    - Paylaşılan - planı indirimi, eşleşen herhangi bir abonelikte fatura Bağlamınızı durumlarda uygulanır. Kurumsal müşteriler için fatura bağlamı kayıt ve kayıt içinde tüm abonelikleri içerir. Kullandıkça Öde fiyatlandırma müşterilerle tek plan için faturalama Hesap Yöneticisi tarafından oluşturulan Kullandıkça Öde fiyatlandırma abonelikleri ile tek tek tüm planlar bağlamdır.
5. VM boyutu ve resim türü seçmek için bir ürün seçin. Yalnızca seçilen VM boyutuna indirim uygulanır.
6. Bir yıllık veya üç yıllık bir terim seçin.
7. Fatura indirim almak ön ödemeli VM örneği sayısı bir miktar seçin.
8. Sepet, gözden geçirme ve satın alma için ürün ekleyin.

Ayırma indirimi, önceden ödeme yaparsınız yazılım ölçer için otomatik olarak uygulanır. VM işlem ücretlerini planı kapsamında değildir. VM ayırmaları ayrı olarak satın alabilirsiniz.

## <a name="discount-applies-to-different-suse-vm-sizes"></a>İndirimi, farklı SUSE VM boyutları için geçerlidir.

Ayrılmış VM örnekleri gibi SUSE Linux planları örneği boyutu esnekliği sunar. Satın aldığınız SUSE planından farklı bir boyutta bir sanal makine dağıttığınızda, indirim uygulanır. Daha fazla bilgi için [yazılım planı indirim nasıl uygulandığını anlamanız](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="redhat-plan-discount"></a>RedHat-planı indirim

Planlar yalnızca Red Hat Enterprise Linux sanal makineler için kullanılabilir. İndirim, RedHat Enterprise Linux SAP HANA Vm'lerinde veya RedHat Enterprise Linux SAP Business Apps Vm'leri için geçerli değildir.

Satın alım zamanında seçtiğiniz VM boyutuna RedHat planı indirimleri uygulanır. RHEL planları para iadesi veya satın almanızdan sonra akar.


## <a name="cancellation-and-exchanges-not-allowed"></a>İptal ve değişimleri izin verilmiyor

İptal edemezsiniz veya satın aldığınız bir SUSE veya RedHat planı exchange. Kullanımınızı denetleyerek doğru planı satın aldığınızdan emin olun. Senaryonuzda satın almak daha fazla yardım için bkz: [yazılım planı indirim nasıl uygulandığını anlamanız](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyon yönetme konusunda bilgi almak için bkz: [Azure'un ayırmaları](../../billing/billing-manage-reserved-vm-instance.md).

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](../../billing/billing-save-compute-costs-reservations.md)
- [Azure'da ayırmalarını yönetme](../../billing/billing-manage-reserved-vm-instance.md)
- [SUSE ayırma indirimi nasıl uygulanacağını anlama](../../billing/billing-understand-suse-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](../../billing/billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](../../billing/billing-understand-reserved-instance-usage-ea.md)
