---
title: Azure sanal makineler için önceden ödeme tarafından paradan tasarruf - Azure | Microsoft Docs
description: Azure ayrılmış sanal makine, sanal makineleri maliyetlerinizi kaydetmek için örnek hakkında bilgi edinin.
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
ms.date: 11/09/2017
ms.author: yashar
ms.openlocfilehash: 9c73102f09e252b449f78603debaf707b3c89c3b
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="save-money-on-virtual-machines-with-reserved-virtual-machine-instances"></a>Ayrılmış sanal makine örnekleri sahip sanal makinelerde paradan tasarruf 
Ayrılmış sanal makine örnekleri, yıllık veya üç yıl kullandığınız sanal makinelerde indirim almak için işlem kapasitesi için önceden ödeme olanak tanır. Sanal makine maliyetlerinizi önemli ölçüde azaltır — yüzde 72 Kullandıkça Öde fiyatları – yıllık veya üç yıllık ön taahhüt ile. Ayrılmış sanal makine örneklerinin bir fatura iskonto ve sanal makineler çalışma zamanı durumunu etkilemez.

Ayrılmış bir sanal makine örnek satın alabileceğiniz [Azure portal](https://aka.ms/reservations). Daha fazla bilgi için bkz: [ayrılmış sanal makine örneğiyle ön'ödeme tasarruf ve sanal makinelere](https://go.microsoft.com/fwlink/?linkid=861721).

## <a name="why-should-i-buy-a-reserved-virtual-machine-instance"></a>Ayrılmış bir sanal makine örnek satın neden?
Uzun bir süre için çalışan sanal makineleri varsa, ayrılmış bir sanal makine örnek satın alma, en iyi etkili fiyat sağlar. Örneğin, bir ayırma Batı ABD bölgesi sürekli olarak standart D2 dört örneklerini çalıştırırsanız, ödeme--Git-, oranlarda ücretlendirilirsiniz. Ayrılmış bir sanal makine örneği için dört VM'ler satın aldığınız durumlarda, sanal makineleri hemen fatura yararlanır. Bunlar artık ödeme--, Git oranlarda sizden ücret kesilir. 

## <a name="what-charges-does-a-reserved-virtual-machine-instance-cover"></a>Hangi ücretleri ayrılmış bir sanal makine örneği ele alınmamaktadır?
Bir ayırma yalnızca, Windows veya Linux sanal makine için sanal makine altyapısı ücretleri kapsar. Bir ayırma ek yazılım, ağ veya depolama ücretleri kapsamaz. Windows sanal makineler için lisans maliyetleri ile Windows kapak [Azure karma avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reserved-virtual-machine-instance"></a>Ayrılmış bir sanal makine örnek satın alma hakkınız kim?
Bu abonelikleri türleri ile Azure müşterilerine ayrılmış bir sanal makine örnek satın alabilirsiniz:
-   Kurumsal Anlaşma abonelik Teklif türü (MS-AZR - 0017P).
-   [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) abonelik Teklif türü (MS-AZR - 003 P).
Ayrılmış örnek satın almak için aboneliğe ilişkin "Sahip" rolünde olması gerekir. Bir kurumsal kayıt ayırmalarını satın almak için kuruluş yöneticisi EA portal ayırma alımlar varsayılan ayar etkin olarak etkinleştirmeniz gerekir.
-   Bulut çözümü sağlayıcısı (CSP) iş ortakları, Azure Yönetim Portalı kullanabilir veya [ortağı Merkezi'nde](https://docs.microsoft.com/partner-center/azure-reservations) ayırmaları satın alın.

## <a name="how-is-a-reserved-virtual-machine-instances-purchase-billed"></a>Ayrılmış sanal makine örnekleri satın alma nasıl faturalandırılır?
Ayırma satın alma, aboneliğe bağlı ödeme yöntemi için ücret kesilir. Bir kurumsal aboneliğiniz varsa ayırma maliyet parasal taahhüt bakiyeniz çıkarılır. Parasal taahhüt bakiyeniz ayırma maliyetini kapsamıyordur, fatura fazla kullanım.
Kullandıkça Öde aboneliğiniz varsa, hesabınızdaki sahip kredi kartı hemen faturalandırılır. Fatura ile faturalandırılır, sonraki faturaya ücretleri konusuna bakın.

## <a name="how-is-the-purchased-reserved-virtual-machine-instance-discount-applied"></a>Satın alınan ayrılmış sanal makine örnek indirim nasıl uygulanır?
Ayrılmış sanal makine örnek indirim ayırma satın aldığınızda seçtiğiniz öznitelikle eşleşen sanal makineler için geçerlidir. Öznitelikler, eşleşen VM'ler çalıştırdığı kapsamını içerir. Örneğin, Batı ABD bölgesi dört standart D2 sanal makineler için ayrılmış VM örnek indirim istiyorsanız, sanal makineleri çalıştırdığı aboneliği seçin. Farklı Aboneliklerde kaydı/hesabınızı içinde çalışan sanal makineler, kapsam olarak paylaşılan seçin. Paylaşılan kapsam abonelikler arasında uygulanacak ayırma indirim sağlar.
Ayrılmış bir VM örnek satın sonra kapsamı değiştirebilirsiniz. Kapsamını değiştirmek için ayırmalarını yönetme konusunda belgelerine bakın.

Ayırma indirim yalnızca enterprise veya Kullandıkça Öde teklif türleri ile Aboneliklerdeki sanal makineler için geçerlidir. Bir abonelikte diğer teklif türleriyle çalışan sanal makineler ayırma indirim almaz. Kurumsal kayıtları için enterprise geliştirme ve Test abonelik için ayrılmış örnek avantajları uygun değil.

Ayırma sanal makine faturalama etkilemesi açıklaması [avantajı faturalama ayırma uygulama anlama](https://go.microsoft.com/fwlink/?linkid=863405).

## <a name="what-happens-when-the-reservation-term-expires"></a>Ayırma terim süresi dolduğunda ne olur?
Ayırma döneminin sonunda fatura indirim süresi dolar ve sanal makine altyapısı ödeme--, Git fiyattan faturalandırılır. Ayırmaları Otomatik yenilemeyi değil. Fatura indirim alınırken devam etmek için yeni bir ayrılmış sanal makine örneği satın almanız gerekir. 

## <a name="sizes-and-regional-availability"></a>Boyutları ve bölgesel kullanılabilirliği
Ayırmalar, bazı özel durumlar dışında çoğu VM boyutları için kullanılabilir:
- Önizleme VM boyutları – Önizleme aşamasındadır herhangi bir boyutta ayrılmış sanal makine örnek satın almak için uygun değildir.
- Bulutları: ayrılmış sanal makine örnekleri satın Azure ABD devlet kurumları, Almanya veya Çin bölgelerde kullanılabilir değil. 
- Yetersiz kota – A ayrılmış VM tek bir abonelik kapsamına örnek vCPU kota abonelik yeni RI için kullanılabilir olması gerekir. Örneğin, hedef abonelik 10 Vcpu'lar D-serisi ailesi için kota sınırı varsa, 11 Standard_D1 örnekleri için ayrılmış bir VM örnek satın alamıyor. Kota denetimi ayırmalarının abonelikte zaten dağıtılan VM'ler içerir. Örneğin, aboneliği D-serisi ailesi için 10 Vcpu'lar kotası varsa. Bu abonelik dağıtılan iki standard_D1 örneği varsa, bu Abonelikteki 10 standard_D1 örnekleri için ayrılmış VM örneği satın alabilirsiniz. 
- Kapasite sınırlamaları – nadir durumlarda, yeni alt ayırmalarını bir bölgede düşük kapasite nedeniyle VM boyutları, Azure sınırları satın.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizi satın alarak kaydetme başlangıç bir [ayrılmış sanal makine örnek](https://go.microsoft.com/fwlink/?linkid=861721). 

Ayrılmış sanal makine örnekleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın.

- [Ayrılmış VM örnekleri ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Ayrılmış sanal makine örneklerini yönetme](billing-manage-reserved-vm-instance.md)
- [Ayrılmış sanal makine örnek indirim nasıl uygulandığını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak](billing-understand-reserved-instance-usage.md)
- [İşletme kaydı için ayrılmış örnek kullanım anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayrılmış örnekler dahil değil](billing-reserved-instance-windows-software-costs.md)
- [Ayrılmış örnekler iş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programı](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun
Yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
