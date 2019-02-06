---
title: Azure Ayırmaları nedir? | Microsoft Docs
description: Sanal makinelerinizde, SQL veritabanları, Azure Cosmos DB ve diğer kaynak maliyetleri kaydetmek için Azure ayırmaları ve fiyatlandırma hakkında bilgi edinin.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: banders
ms.openlocfilehash: 14b6125bd1467efcd6120bc367a7042ad29552bb
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55744509"
---
# <a name="what-are-azure-reservations"></a>Azure Ayırmaları nedir?

Azure ayırmaları bir yıllık veya üç yıl, sanal makine, SQL veritabanı işlem kapasitesi, Azure Cosmos DB, aktarım hızı veya diğer Azure kaynakları için önceden ödeme yaparak tasarruf etmenize yardımcı olur. Önceden ödeme kullandığınız kaynaklar üzerinde bir indirim almak sağlar. Rezervasyonlar, sanal makine, SQL veritabanı işlem, Azure Cosmos DB, önemli ölçüde azaltabilir ya da diğer kaynak en fazla %72 Kullandıkça Öde fiyatlarında maliyetlerini. Rezervasyon faturalandırma indirim sağlar ve kaynaklarınızın çalışma zamanı durumunu etkilemez.

Rezervasyon satın alabileceğiniz [Azure portalında](https://aka.ms/reservations). Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure Cosmos DB ayrılmış kapasite ile Azure Cosmos DB kaynaklarını için ön ödeme](../cosmos-db/cosmos-db-reserved-capacity.md)

## <a name="why-should-i-buy-a-reservation"></a>Rezervasyon satın neden?

Sanal makineleri, Azure Cosmos DB veya uzun süreler için çalışan SQL veritabanları varsa, rezervasyon satın alma, en uygun maliyetli bir seçenek sağlar. Örneğin, dört örnek ayırma olmadan bir hizmetin sürekli olarak çalıştırırsanız, Kullandıkça Öde fiyatları üzerinden ücretlendirilir. Bu kaynaklar için bir ayırma satın alırsanız, ayırma indirimini hemen erişin. Kaynakları artık Kullandıkça Öde fiyatları üzerinden ücretlendirilir.

## <a name="what-charges-does-a-reservation-cover"></a>Ne ayırma kapak mu?

- Ayrılmış sanal makine örneği: Ayırma, yalnızca sanal makine işlem maliyetlerini kapsar. Bu, ek yazılım, ağ ve depolama ücretleri ele alınmamıştır.
- SQL veritabanı sanal çekirdek ayrılmıştır: Ayırma ile yalnızca işlem maliyetleri dahildir. Lisans ayrı olarak faturalandırılır.
- Azure Cosmos DB, kapasite ayrılmıştır: Kaynaklarınız için sağlanan aktarım hızı bir ayırma kapsar, depolama ve ağ ücretleri kapsamaz.

Windows sanal makineler ve SQL veritabanı için lisanslama maliyetleri kapsayan [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Rezervasyon satın almak uygun kimdir?

Bu abonelik türü, Azure müşterilerine rezervasyon satın alabilirsiniz:

- Kurumsal Anlaşma abonelik Teklif türü (MS-AZR - 0017P veya MS-AZR - 0148P).
- Kullandıkça Öde aboneliği Teklif türü (MS-AZR - 003P veya MS-AZR - 0023P).
- Bulut çözümü sağlayıcısı (CSP) iş ortakları, Azure portalını kullanabilir veya [iş ortağı Merkezi](https://docs.microsoft.com/partner-center/azure-reservations) Azure rezervasyon satın almak için.

Ayırma indirimi, yalnızca Enterprise, Kullandıkça Öde ya da CSP aboneliği türleriyle ilişkili kaynaklar için geçerlidir.

 Bir plan satın almak için:

- En az bir kuruluş veya Kullandıkça Öde aboneliğine sahip rolünde olması gerekir.
- Kurumsal abonelikler için **ayrılmış örnekleri ekleme** içinde etkinleştirilmelidir [EA portal](https://ea.azure.com). Veya bu ayarı devre dışıysa, aboneliğini bir EA yönetici olması gerekir.

## <a name="how-is-a-reservation-billed"></a>Rezervasyon nasıl faturalandırılır?

Ayırma, aboneliğe bağlı ödeme yöntemi için ücretlendirilir. Bir kurumsal abonelik varsa, rezervasyon maliyeti parasal taahhüt bakiyeniz çıkarılır. Parasal taahhüt bakiyeniz rezervasyon maliyeti yoksa, faturalandırılırsınız kapasite aşımı. Bir Kullandıkça Öde aboneliği varsa, kredi kartı hesabınızda sahip hemen faturalandırılır. Fatura ile faturalandırılırsınız sonraki faturanızı ücretleri görürsünüz.

## <a name="how-is-the-reservation-discount-applied"></a>Ayırma indirimi nasıl uygulanır?

Ayırma indirimi, rezervasyon satın aldığınızda seçtiğiniz öznitelikleri eşleşen kaynak kullanımı için geçerlidir. Öznitelikler, eşleşen VM'ler, SQL veritabanları, Azure Cosmos DB veya diğer kaynaklara çalıştırdığı kapsamı içerir. Örneğin, Batı ABD bölgesindeki dört standart D2 sanal makineler için bir ayırma indirimi istiyorsanız, sanal makinelerin çalıştığı aboneliği seçin. Kayıt/hesabınızda farklı Aboneliklerde bulunan sanal makineleri çalıştırıyorsanız, kapsam olarak paylaşılan seçin. Paylaşılan kapsam ayırma indirimi, abonelikler arasında uygulanmasına olanak tanır. Rezervasyon satın alma sonra kapsam değiştirebilirsiniz. Daha fazla bilgi için [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).

Ayırma indirimi, yalnızca Enterprise, Kullandıkça Öde ya da CSP aboneliği türleriyle ilişkili kaynaklar için geçerlidir. Ayırma indirimi çalıştıran diğer teklif türleri ile bir Abonelikteki kaynakları almaz.

Ayırmalar, faturalandırma nasıl etkilediğini daha iyi anlamak için aşağıdaki konulara bakın:

-  [Azure ayrılmış VM örnekleri indirim anlama](billing-understand-vm-reservation-charges.md)
- [Azure ayırma indirimi anlama](billing-understand-vm-reservation-charges.md)
- [Azure Cosmos DB ayırma indirimi anlama](billing-understand-cosmosdb-reservation-charges.md)
- [Azure ayırma indirimi ve kullanım için SUSE anlama](billing-understand-suse-reservation-charges.md)

## <a name="what-happens-when-the-reservation-term-expires"></a>Ayırma dönemi sona erdiğinde ne olacak?

Ayırma dönemi sonunda, fatura indirim süresi dolar ve sanal makine, SQL veritabanı, Azure Cosmos DB veya diğer kaynak ödeme--, go fiyattan faturalandırılır. Azure ayırmaları otomatik olarak yenilenecek yok. Fatura indirim almaya devam etmek için uygun hizmetler ve yazılım için yeni bir rezervasyon satın almanız gerekir.

## <a name="discount-applies-to-different-sizes-with-instance-size-flexibility"></a>İndirim için farklı boyutlarda örneği boyutu esnekliği ile geçerlidir.

Rezervasyon satın aldığınızda, indirim diğer örnekleriyle aynı boyut grubu içinde öznitelikleri uygulayabilirsiniz. İndirim Karşılama esnekliği, ayırma ve rezervasyon satın aldığınızda, çekme özniteliklerinin türüne bağlıdır.

- Ayrılmış VM örnekleri: Ne zaman satın aldığınız ayırma seçerseniz **için en iyi duruma getirilmiş**: **örnek boyutu esneklik**, indirim kapsamı seçtiğiniz VM boyutuna bağlıdır. Ayırma aynı boyutu seri grubu içindeki sanal makineler (VM) boyutlarına uygulayabilirsiniz. Daha fazla bilgi için [ayrılmış VM örnekleri ile sanal makine boyutu esneklik](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- SUSE Linux Enterprise yazılım planı: İndirim kapsamı SUSE yazılımı çalıştıracağınız sanal makinelerin Vcpu bağlıdır. Daha fazla bilgi için [SUSE Linux Enterprise yazılım planı indirim nasıl uygulandığını anlamanız](billing-understand-suse-reservation-charges.md).
- SQL veritabanı, kapasite ayrılmıştır: İndirim kapsamı seçtiğiniz performans katmanına bağlıdır. Daha fazla bilgi için [bir Azure ayırma indirimi nasıl uygulandığını anlamanız](billing-understand-reservation-charges.md).
- Azure Cosmos DB, kapasite ayrılmıştır: İndirim kapsamı, sağlanan aktarım hızına bağlıdır. Daha fazla bilgi için [bir Azure Cosmos DB ayırma indirimi nasıl uygulandığını anlamanız](billing-understand-cosmosdb-reservation-charges.md).

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinelerinizde satın alarak kaydetme başlangıç bir [ayrılmış VM örneği](../virtual-machines/windows/prepay-reserved-vm-instances.md), [SQL veritabanı ayrılan kapasite](../sql-database/sql-database-reserved-capacity.md), veya [Azure Cosmos DB ayrılan kapasite](../cosmos-db/cosmos-db-reserved-capacity.md).

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)
- [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
