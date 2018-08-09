---
title: Azure ayırmaları nelerdir? | Microsoft Docs
description: Azure ayırmaları ve sanal makinelerinizi, SQL veritabanları ve diğer kaynakları maliyetlerini kaydetmek için fiyatlandırma hakkında bilgi edinin.
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
ms.openlocfilehash: 93c11852a11e0bb57a0b92090368298fc14b8c2a
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626313"
---
# <a name="what-are-azure-reservations"></a>Azure ayırmaları nelerdir?

Hesaplama kapasitesi bir yıllık veya üç yıl, sanal makine veya SQL veritabanı için önceden ödeme yaparak tasarruf azure ayırmaları yardımcı olur. Önceden ödeme kullandığınız kaynaklar üzerinde bir indirim almak sağlar. Azure ayırmaları, sanal makine veya SQL veritabanı işlem maliyetleri önemli ölçüde azaltabilirsiniz — Kullandıkça Öde fiyatlarında yüzde 72'ye varan – yıllık veya üç yıllık ön taahhüt. Rezervasyon faturalandırma indirim sağlar ve sanal makine veya SQL veritabanları çalışma zamanı durumunu etkilemez.

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

Bu abonelik türü, Azure müşterilerine bir ayırma satın alabilir:

- Kurumsal sözleşme abonelik teklif türü (MS-AZR-0017P).
- [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) abonelik teklif türü (MS-AZR-003P). Rezervasyon satın almak için abonelikte "Sahip" rolü olmalıdır. Bir kurumsal kayıt rezervasyon satın almak için Kurumsal Yönetici rezervasyon satın alma işlemleri EA portalında etkinleştirmeniz gerekir. Varsayılan olarak bu ayar etkindir.
- Bulut çözümü sağlayıcısı (CSP) iş ortakları, Azure portalını kullanabilir veya [iş ortağı Merkezi](https://docs.microsoft.com/partner-center/azure-reservations) Azure rezervasyon satın almak için. 

SQL veritabanları, Enterprise, Kullandıkça Öde ya da CSP aboneliği türleriyle ilişkili ya da bir Azure ayırma indirimi, yalnızca sanal makineler için geçerlidir.

## <a name="how-is-a-reservation-billed"></a>Rezervasyon nasıl faturalandırılır?

Ayırma, aboneliğe bağlı ödeme yöntemi için ücretlendirilir. Bir kurumsal abonelik varsa, rezervasyon maliyeti parasal taahhüt bakiyeniz çıkarılır. Parasal taahhüt bakiyeniz rezervasyon maliyeti yoksa, faturalandırılırsınız kapasite aşımı. Bir Kullandıkça Öde aboneliği varsa, kredi kartı hesabınızda sahip hemen faturalandırılır. Fatura ile faturalandırılırsınız sonraki faturanızı ücretleri görürsünüz.

## <a name="how-is-the-reservation-discount-applied"></a>Ayırma indirimi nasıl uygulanır?

Ayırma indirimi, rezervasyon satın aldığınızda seçtiğiniz öznitelikleri eşleşen kaynak kullanımı için geçerlidir. Öznitelikler, kapsamı içerir. burada eşleşen VM'ler, SQL veritabanları, veya diğer kaynaklara çalıştırın. Örneğin, Batı ABD bölgesindeki dört standart D2 sanal makineler için bir ayırma indirimi istiyorsanız, sanal makinelerin çalıştığı aboneliği seçin. Kayıt/hesabınızda farklı Aboneliklerde bulunan sanal makineleri çalıştırıyorsanız, kapsam olarak paylaşılan seçin. Paylaşılan kapsam ayırma indirimi, abonelikler arasında uygulanmasına olanak tanır. Rezervasyon satın alma sonra kapsam değiştirebilirsiniz. Daha fazla bilgi için [azure'da ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).

Ayırma indirimi, yalnızca sanal makinelere uygulanır veya SQL veritabanları türleri Kurumsal veya Kullandıkça Öde aboneliği ile ilişkili. Ayırma indirimi, sanal makineleri veya diğer teklif türleri ile bir abonelikte çalışan SQL veritabanları almaz. Kurumsal kayıtları için Kurumsal geliştirme ve Test abonelikleri ayırma avantajlar için uygun değildir.

Sanal makine veya SQL veritabanı faturalandırma ayırmaları etkilemesi daha iyi anlamak için bkz: [ayırma indirimi nasıl uygulandığını anlamanız](billing-understand-vm-reservation-charges.md).

## <a name="what-happens-when-the-reservation-term-expires"></a>Ayırma dönemi sona erdiğinde ne olacak?

Ayırma dönemi sonunda, fatura indirim süresi dolar ve sanal makine, SQL veritabanı veya başka kaynaklara ödeme--, go fiyattan faturalandırılır. Azure ayırmaları otomatik olarak yenilenecek yok. Fatura indirim almaya devam etmek için uygun Hizmetleri ayırma için yeni bir rezervasyon satın almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinelerinizde satın alarak kaydetme başlangıç bir [ayrılmış VM örneği](../virtual-machines/windows/prepay-reserved-vm-instances.md) veya [SQL veritabanı ayrılan kapasite](../sql-database/sql-database-reserved-capacity.md).

Rezervasyonlar hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
- [Ayırma indirimi nasıl uygulanacağını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)
- [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
