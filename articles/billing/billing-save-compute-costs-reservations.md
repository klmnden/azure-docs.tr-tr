---
title: Azure ayrılmış örnekler nelerdir? | Microsoft Docs
description: Azure ayrılmış VM örnekleri ve sanal makineleri giderlerinden tasarruf edersiniz ve en iyi etkili fiyatı almak için fiyatlandırma VM hakkında bilgi edinin.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: yashar
ms.openlocfilehash: 12fc88596b4283b6f809575328ab801704cc1c8d
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064066"
---
# <a name="what-are-azure-reserved-vm-instances"></a>Azure ayrılmış VM örnekleri nelerdir?
[Azure ayrılmış VM örnekleri](https://azure.microsoft.com/pricing/reserved-vm-instances) kullandığınız yıllık veya üç yıl hesaplama kapasitesi sağlayarak sanal makinelerde indirim almak için önceden ödeme tarafından para Kaydet yardımcı olur. Azure ayrılmış örnekler, sanal makine maliyetlerini önemli ölçüde azaltabilirsiniz — yüzde 72 Kullandıkça Öde fiyatları – yıllık veya üç yıllık ön taahhüt ile. Ayrılmış örnekler fatura indirim sağlayın ve sanal makinelerinizi çalışma zamanı durumunu etkilemez.

Ayrılmış örnek (RI) satın alabileceğiniz [Azure portal](https://aka.ms/reservations). Daha fazla bilgi için bkz: [ön ödeme sanal makineler için ve ayrılmış örnekler ile tasarruf](https://go.microsoft.com/fwlink/?linkid=861721).

## <a name="why-should-i-buy-a-reserved-instance"></a>Ayrılmış örnek satın neden?
Uzun bir süre için çalışan sanal makineleri varsa, ayrılmış örnek satın alma en düşük maliyetli seçeneği sunar. Örneğin, ayrılmış örnek Batı ABD bölgesinde sürekli olarak standart bir D2 VM dört örneklerini çalıştırırsanız, Kullandıkça Öde oranlarda ücretlendirilirsiniz. Ayrılmış örnek için dört VM'ler satın aldığınız durumlarda, sanal makineleri hemen fatura yararlanır. Bunlar artık Kullandıkça Öde oranlarda sizden ücret kesilir. 

## <a name="what-charges-does-a-reserved-instance-cover"></a>Ne ücretlendirilen ayrılmış örnek kapak mu?
Ayrılmış örnek yalnızca, Windows veya Linux sanal makine için sanal makine altyapısı ücretleri kapsar. Ayrılmış örnek ek yazılım, ağ veya depolama ücretleri kapsamaz. Windows sanal makineler için lisans maliyetleri ile Windows kapak [Azure karma avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reserved-instance"></a>Ayrılmış örnek satın alma hakkınız kim?
Bu abonelikleri türleri ile Azure müşterilerine ayrılmış örnek satın alabilirsiniz:
-   Kurumsal Anlaşma abonelik Teklif türü (MS-AZR - 0017P).
-   [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) abonelik Teklif türü (MS-AZR - 003 P). Ayrılmış örnek satın almak için aboneliğe ilişkin "Sahip" rolünde olması gerekir. Ayrılmış örnekler bir işletme kaydı, satın aldığınız için kuruluş yöneticisi EA portal ayrılmış örnek alımlar etkinleştirmeniz gerekir. Varsayılan olarak, bu ayar etkinleştirilir.
-   Bulut çözümü sağlayıcısı (CSP) iş ortakları, Azure Portal kullanabilir veya [ortağı Merkezi'nde](https://docs.microsoft.com/partner-center/azure-reservations) ayrılmış örnekler satın alın.

## <a name="how-is-a-reserved-instance-purchase-billed"></a>Ayrılmış örnek satın alma nasıl faturalandırılır?
Ayrılmış örnek satın alma, aboneliğe bağlı ödeme yöntemine doludur. Kurumsal aboneliği varsa, ayrılmış örnek maliyet parasal taahhüt bakiyeniz çıkarılır. Ayrılmış örnek maliyetini parasal taahhüt bakiyeniz kapsamaz etseniz bile fatura fazla kullanım.
Kullandıkça Öde aboneliğiniz varsa, hesabınızdaki sahip kredi kartı hemen faturalandırılır. Fatura ile faturalandırılır, sonraki faturaya ücretleri konusuna bakın.

## <a name="how-is-the-purchased-reserved-instance-discount-applied"></a>Satın alınan ayrılmış örnek indirim nasıl uygulanır?
Ayrılmış örnek indirim ayrılmış örnek satın aldığınızda seçtiğiniz öznitelikle eşleşen sanal makineler için geçerlidir. Öznitelikler, eşleşen VM'ler çalıştırdığı kapsamını içerir. Örneğin, Batı ABD bölgesi dört standart D2 sanal makineler için ayrılmış örnek indirim istiyorsanız, sanal makineleri çalıştırdığı aboneliği seçin. Farklı Aboneliklerde kaydı/hesabınızı içinde çalışan sanal makineler, kapsam olarak paylaşılan seçin. Paylaşılan kapsam abonelikler arasında uygulanacak ayrılmış örnek indirim sağlar. Ayrılmış örnek satın sonra kapsamı değiştirebilirsiniz. Kapsamını değiştirmek için bkz: [yönetmek azure'da ayrılmış örnekler](billing-manage-reserved-vm-instance.md).

Ayrılmış örnek indirim yalnızca enterprise veya Kullandıkça Öde aboneliğine türleri ile ilişkilendirilen sanal makineleri için geçerlidir. Bir abonelikte diğer teklif türleriyle çalışan sanal makineler, ayrılmış örnek indirim almaz. Kurumsal kayıtları için enterprise geliştirme ve Test abonelik için ayrılmış örnek avantajları uygun değil.

Daha iyi anlamak için sanal makine faturalama ayrılmış örnek etkiler bkz [ayrılmış örnek indirim nasıl uygulandığını anlamak](billing-understand-vm-reservation-charges.md).

## <a name="what-happens-when-the-reserved-instance-term-expires"></a>Ayrılmış örnek terim süresi dolduğunda ne olur?
Ayrılmış örnek döneminin sonunda fatura indirim süresi dolar ve sanal makine altyapısı ödeme--, Git fiyattan faturalandırılır. Azure ayrılmış örnekler Otomatik yenilemeyi değil. Fatura indirim alınırken devam etmek için yeni bir ayrılmış örnek satın almanız gerekir. 

## <a name="sizes-and-regional-availability"></a>Boyutları ve bölgesel kullanılabilirliği
Ayrılmış örnekler, bazı özel durumlar dışında çoğu VM boyutları için kullanılabilir:
- Önizleme-herhangi bir VM dizisi veya önizlemede boyutu VM'ler ayrılmış örnek satın almak için uygun değildir.
- Bulutları: ayrılmış örnekler satın Azure ABD devlet kurumları, Almanya veya Çin bölgelerde kullanılabilir değil. 
- Yetersiz kota – tek bir abonelik kapsamlıdır ayrılmış bir örnek vCPU kota abonelik yeni RI için kullanılabilir olması gerekir. Örneğin, hedef abonelik D-serisi için 10 Vcpu'lar kota sınırı varsa, 11 Standard_D1 örnekleri için ayrılmış bir örnek satın alamıyor. Ayrılmış örnekler için kota denetimi abonelikte zaten dağıtılan VM'ler içerir. Örneğin, abonelik için D-serisi 10 Vcpu'lar kotası varsa ve dağıtılan iki standard_D1 örneğe sahip, bu Abonelikteki 10 standard_D1 örnekleri için ayrılmış bir örnek satın alabilirsiniz. 
- Nadir durumlarda, bir bölgede düşük kapasite nedeniyle yeni bir ayrılmış örneklerinin VM alt için satın alma boyutları Azure sınırları kapasite – kısıtlamaları.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizi satın alarak kaydetme başlangıç bir [Azure ayrılmış örnek](https://go.microsoft.com/fwlink/?linkid=861721). 

Ayrılmış örnekler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayrılmış VM örnekleri nelerdir?](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure'da ayrılmış örnekler yönetme](billing-manage-reserved-vm-instance.md)
- [Ayrılmış örnek indirim nasıl uygulandığını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak](billing-understand-reserved-instance-usage.md)
- [İşletme kaydı için ayrılmış örnek kullanım anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayrılmış örnekler dahil değil](billing-reserved-instance-windows-software-costs.md)
- [Ayrılmış örnekler iş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programı](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
