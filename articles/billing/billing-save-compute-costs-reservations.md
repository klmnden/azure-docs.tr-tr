---
title: Azure Ayırmaları nedir? | Microsoft Docs
description: Sanal makinelerinizi, SQL veritabanları ve diğer kaynak maliyetleri kaydetmek için Azure ayırmaları ve fiyatlandırma hakkında bilgi edinin.
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
ms.date: 08/08/2018
ms.author: yashar
ms.openlocfilehash: 82b23f46acc94fefccc871583657200b90a33f05
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43303571"
---
# <a name="what-are-azure-reservations"></a>Azure Ayırmaları nedir?

Bir yıllık veya üç yıl, sanal makine, SQL veritabanı için önceden ödeme yaparak tasarruf azure ayırmaları yardımcı olur, kapasite veya diğer Azure kaynakları işlem. Önceden ödeme kullandığınız kaynaklar üzerinde bir indirim almak sağlar. Rezervasyonlar önemli ölçüde en fazla %72 Kullandıkça Öde fiyatlarında, sanal makine, SQL veritabanı işlem ya da diğer kaynak maliyetleri azaltabilir. Rezervasyon faturalandırma indirim sağlar ve kaynaklarınızın çalışma zamanı durumunu etkilemez.

Rezervasyon satın alabileceğiniz [Azure portalında](https://aka.ms/reservations). Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

- [Azure ayrılmış VM örnekleri ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL veritabanı'nın ayrılmış kapasite ile SQL veritabanı bilgi işlem kaynakları için ön ödeme](../sql-database/sql-database-reserved-capacity.md)

## <a name="why-should-i-buy-a-reservation"></a>Rezervasyon satın neden?

Sanal makine ya da uzun süreler için çalışan SQL veritabanları varsa, rezervasyon satın alma, en uygun maliyetli seçeneği sunar. Örneğin, dört örnek ayırma olmadan bir hizmetin sürekli olarak çalıştırırsanız, Kullandıkça Öde fiyatları üzerinden ücretlendirilir. Bu kaynaklar için bir ayırma satın alırsanız, ayırma indirimini hemen erişin. Kaynakları artık Kullandıkça Öde fiyatları üzerinden ücretlendirilir.

## <a name="what-charges-does-a-reservation-cover"></a>Ne ayırma kapak mu?

- Ayrılmış sanal makine örneği: Bir ayırma, yalnızca sanal makine işlem maliyetlerini kapsar. Bu, ek yazılım, ağ ve depolama ücretleri ele alınmamıştır.
- SQL veritabanı sanal çekirdek ayrılmış: yalnızca bir ayırma ile işlem maliyetleri dahildir. Lisans ayrı olarak faturalandırılır.

Windows sanal makineler ve SQL veritabanı için lisanslama maliyetleri kapsayan [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Rezervasyon satın almak uygun kimdir?

Bu abonelik türü, Azure müşterilerine rezervasyon satın alabilirsiniz:

- Kurumsal sözleşme abonelik teklif türü (MS-AZR-0017P).
- [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) abonelik teklif türü (MS-AZR-003P). Rezervasyon satın almak için abonelikte "Sahip" rolü olmalıdır.
- Bulut çözümü sağlayıcısı (CSP) iş ortakları, Azure portalını kullanabilir veya [iş ortağı Merkezi](https://docs.microsoft.com/partner-center/azure-reservations) Azure rezervasyon satın almak için.

Ayırma indirimi, yalnızca Enterprise, Kullandıkça Öde ya da CSP aboneliği türleriyle ilişkili kaynaklar için geçerlidir.

## <a name="how-is-a-reservation-billed"></a>Rezervasyon nasıl faturalandırılır?

Ayırma, aboneliğe bağlı ödeme yöntemi için ücretlendirilir. Bir kurumsal abonelik varsa, rezervasyon maliyeti parasal taahhüt bakiyeniz çıkarılır. Parasal taahhüt bakiyeniz rezervasyon maliyeti yoksa, faturalandırılırsınız kapasite aşımı. Bir Kullandıkça Öde aboneliği varsa, kredi kartı hesabınızda sahip hemen faturalandırılır. Fatura ile faturalandırılırsınız sonraki faturanızı ücretleri görürsünüz.

## <a name="how-is-the-reservation-discount-applied"></a>Ayırma indirimi nasıl uygulanır?

Ayırma indirimi, rezervasyon satın aldığınızda seçtiğiniz öznitelikleri eşleşen kaynak kullanımı için geçerlidir. Öznitelikler, kapsamı içerir. burada eşleşen VM'ler, SQL veritabanları, veya diğer kaynaklara çalıştırın. Örneğin, Batı ABD bölgesindeki dört standart D2 sanal makineler için bir ayırma indirimi istiyorsanız, sanal makinelerin çalıştığı aboneliği seçin. Kayıt/hesabınızda farklı Aboneliklerde bulunan sanal makineleri çalıştırıyorsanız, kapsam olarak paylaşılan seçin. Paylaşılan kapsam ayırma indirimi, abonelikler arasında uygulanmasına olanak tanır. Rezervasyon satın alma sonra kapsam değiştirebilirsiniz. Daha fazla bilgi için [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).

Ayırma indirimi, yalnızca Enterprise, Kullandıkça Öde ya da CSP aboneliği türleriyle ilişkili kaynaklar için geçerlidir. Ayırma indirimi çalıştıran diğer teklif türleri ile bir Abonelikteki kaynakları almaz. Kurumsal kayıtları için Kurumsal geliştirme ve Test abonelikleri ayırma avantajlar için uygun değildir.

Ayırmalar, faturalandırma nasıl etkilediğini daha iyi anlamak için aşağıdaki konulara bakın:

-  [Azure ayrılmış VM örnekleri indirim anlama](billing-understand-vm-reservation-charges.md)
- [Azure ayırma indirimi anlama](billing-understand-vm-reservation-charges.md)
- Azure ayırma indirimi ve kullanım için SUSE anlama

## <a name="what-happens-when-the-reservation-term-expires"></a>Ayırma dönemi sona erdiğinde ne olacak?

Ayırma dönemi sonunda, fatura indirim süresi dolar ve sanal makine, SQL veritabanı veya başka bir kaynağa ödeme--, go fiyattan faturalandırılır. Azure ayırmaları otomatik olarak yenilenecek yok. Fatura indirim almaya devam etmek için uygun hizmetler ve yazılım için yeni bir rezervasyon satın almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinelerinizde satın alarak kaydetme başlangıç bir [ayrılmış VM örneği](../virtual-machines/windows/prepay-reserved-vm-instances.md) veya [SQL veritabanı ayrılan kapasite](../sql-database/sql-database-reserved-capacity.md).

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)
- [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
