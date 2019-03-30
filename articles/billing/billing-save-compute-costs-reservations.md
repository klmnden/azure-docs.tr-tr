---
title: Azure Ayırmaları nedir? | Microsoft Docs
description: Sanal makinelerinizde, SQL veritabanları, Azure Cosmos DB ve diğer kaynak maliyetleri kaydetmek için Azure ayırmaları ve fiyatlandırma hakkında bilgi edinin.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: banders
ms.openlocfilehash: 1349a05e1dd235c7b375335ae2c9fed16170a61f
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649401"
---
# <a name="what-are-azure-reservations"></a>Azure Ayırmaları nedir?

Azure ayırmalar için bir yıllık ön ödeme yaparak paradan tasarruf etmek veya kapasite, Azure Cosmos DB, aktarım hızı veya diğer Azure kaynaklarını üç yıl sanal makineler, SQL veritabanı, işlem. Önceden ödeme kullandığınız kaynaklar üzerinde bir indirim almak sağlar. Rezervasyonlar, sanal makine, SQL veritabanı işlem, Azure Cosmos DB, önemli ölçüde azaltabilir ya da diğer kaynak en fazla %72 Kullandıkça Öde fiyatlarında maliyetlerini. Rezervasyon faturalandırma indirim sağlar ve kaynaklarınızın çalışma zamanı durumunu etkilemez.

Rezervasyon satın alabileceğiniz [Azure portalında](https://aka.ms/reservations). Daha fazla bilgi için aşağıdaki makalelere bakın:

Hizmet planları:
- [Azure ayrılmış VM örnekleri ile sanal makineler](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure Cosmos DB ile Azure Cosmos DB kaynaklarını ayrılmış kapasite](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Azure SQL veritabanı ile SQL veritabanı işlem kaynakları ayrılmış kapasite](../sql-database/sql-database-reserved-capacity.md)

Yazılım planları:
- [Red Hat yazılımı planlarından Azure ayırmalar](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Azure ayırmalardan SUSE yazılım planları](../virtual-machines/linux/prepay-suse-software-charges.md)

## <a name="why-buy-a-reservation"></a>Neden bir rezervasyon satın?

Sanal makineleri, Azure Cosmos DB veya uzun süreler için çalışan SQL veritabanları varsa, rezervasyon satın alma, en uygun maliyetli bir seçenek sağlar. Örneğin, dört örnek ayırma olmadan bir hizmetin sürekli olarak çalıştırdığınızda, Kullandıkça Öde fiyatları üzerinden ücret ödersiniz. Bu kaynaklar için bir ayırma satın alırsanız, ayırma indirimini hemen erişin. Kaynakları artık Kullandıkça Öde fiyatları üzerinden ücretlendirilir.

## <a name="charges-covered-by-reservation"></a>Ayırma tarafından kapsanan ücretleri

Hizmet planları:

- Ayrılmış sanal makine örneği: Ayırma, yalnızca sanal makine işlem maliyetlerini kapsar. Bu, ek yazılım, ağ ve depolama ücretleri ele alınmamıştır.
- Azure Cosmos DB, kapasite ayrılmıştır: Rezervasyon kaynaklarınız için sağlanan aktarım hızı kapsar. Depolama ve ağ ücretleri ele alınmamıştır.
- SQL veritabanı sanal çekirdek ayrılmıştır: Ayırma ile yalnızca işlem maliyetleri dahildir. Lisans ayrı olarak faturalandırılır.
- Azure Cosmos DB, kapasite ayrılmıştır: Kaynaklarınız için sağlanan aktarım hızı bir ayırma kapsar, depolama ve ağ ücretleri ele alınmamıştır.

Windows sanal makineler ve SQL veritabanı için lisanslama maliyetleri kapsayan [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Rezervasyon satın almak uygun kimdir?

Bir plan satın almak için bir abonelik sahibi rolünde Kurumsal (MS-AZR - 0017 P veya MS-AZR - 0148 P) veya Kullandıkça Öde aboneliğine (MS-AZR - 003 P veya MS-AZR - 0023 P) olmalıdır. Bulut çözüm sağlayıcıları, Azure portalını kullanabilir veya [iş ortağı Merkezi](/partner-center/azure-reservations) Azure rezervasyon satın almak için.

EA müşterileri, satın alma işlemleri EA yöneticilere devre dışı bırakarak sınırlandırabilir **ayrılmış örnekleri ekleme** EA Portal seçeneği. EA yöneticileri rezervasyon satın almak için en az bir kurumsal Anlaşma abonelik için abonelik sahibi olmanız gerekir. Seçeneği, farklı Maliyet merkezleri için ayırma satın almak için merkezi bir ekip istediğiniz kuruluşlar için kullanışlıdır. Satın almanızdan sonra maliyet merkezi sahipleri ayırmaları merkezi takımlar ekleyebilirsiniz. Sahipler, ardından aboneliklerini ayırmaya kapsamını belirleyebilirsiniz. Merkezi takım burada rezervasyon satın alınan abonelik sahibi erişimi olması gerekmez.

Ayırma indirimi, yalnızca Enterprise, Kullandıkça Öde ya da CSP aboneliği türleriyle ilişkili kaynaklar için geçerlidir.

## <a name="how-is-a-reservation-billed"></a>Rezervasyon nasıl faturalandırılır?

Ayırma, aboneliğe bağlı ödeme yöntemi için ücretlendirilir. Bir kurumsal abonelik varsa, rezervasyon maliyeti parasal taahhüt bakiyeniz çıkarılır. Parasal taahhüt bakiyeniz rezervasyon maliyeti yoksa, faturalandırılırsınız kapasite aşımı. Bir Kullandıkça Öde aboneliği varsa, kredi kartı hesabınızda sahip hemen faturalandırılır. Fatura ile faturalandırılırsınız sonraki faturanızı ücretleri görürsünüz.

## <a name="how-reservation-discount-is-applied"></a>Ayırma indirimi nasıl uygulanır

Ayırma indirimi, rezervasyon satın aldığınızda seçtiğiniz öznitelikleri eşleşen kaynak kullanımı için geçerlidir. Öznitelikler, eşleşen VM'ler, SQL veritabanları, Azure Cosmos DB veya diğer kaynaklara çalıştırdığı kapsamı içerir. Örneğin, Batı ABD bölgesindeki dört standart D2 sanal makineler için bir ayırma indirimi istiyorsanız, sanal makinelerin çalıştığı aboneliği seçin. Kayıt/hesabınızda farklı Aboneliklerde bulunan sanal makineleri çalıştırıyorsanız, kapsam olarak paylaşılan seçin. Paylaşılan kapsam ayırma indirimi, abonelikler arasında uygulanmasına olanak tanır. Rezervasyon satın alma sonra kapsam değiştirebilirsiniz. Daha fazla bilgi için [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).

Ayırma indirimi, yalnızca Enterprise, Kullandıkça Öde ya da CSP aboneliği türleriyle ilişkili kaynaklar için geçerlidir. Ayırma indirimi çalıştıran diğer teklif türleri ile bir Abonelikteki kaynakları almaz.

Ayırmalar, faturalandırma nasıl etkilediğini daha iyi anlamak için aşağıdaki makalelere bakın:

Hizmet planları:

- [Azure ayrılmış VM örnekleri indirim anlama](billing-understand-vm-reservation-charges.md)
- [Azure ayırma indirimi anlama](billing-understand-vm-reservation-charges.md)
- [Azure Cosmos DB ayırma indirimi anlama](billing-understand-cosmosdb-reservation-charges.md)

Yazılım planları:

- [Azure ayırma indirimi ve kullanım için Red Hat anlama](billing-understand-rhel-reservation-charges.md)
- [Azure ayırma indirimi ve kullanım için SUSE anlama](billing-understand-suse-reservation-charges.md)

## <a name="when-the-reservation-term-expires"></a>Ayırma dönemi sona erdiğinde

Ayırma dönemi sonunda, fatura indirim süresi dolar ve sanal makine, SQL veritabanı, Azure Cosmos DB veya diğer kaynak ödeme--, go fiyattan faturalandırılır. Azure ayırmaları otomatik olarak yenilenecek yok. Fatura indirim almaya devam etmek için uygun hizmetler ve yazılım için yeni bir rezervasyon satın almanız gerekir.

## <a name="discount-applies-to-different-sizes"></a>İndirimi, farklı boyutlarda için geçerlidir.

Rezervasyon satın aldığınızda, indirim diğer örnekleriyle aynı boyut grubu içinde öznitelikleri uygulayabilirsiniz. Bu özellik, örneği boyutu esnekliği bilinir. İndirim Karşılama esnekliği, ayırma ve rezervasyon satın aldığınızda, çekme özniteliklerinin türüne bağlıdır.

Hizmet planları:

- Ayrılmış VM örnekleri: Ne zaman rezervasyon satın alma ve seçin **için en iyi duruma getirilmiş**: **örnek boyutu esneklik**, indirim kapsamı seçtiğiniz VM boyutuna bağlıdır. Ayırma aynı boyutu seri grubu içindeki sanal makineler (VM) boyutlarına uygulayabilirsiniz. Daha fazla bilgi için [ayrılmış VM örnekleri ile sanal makine boyutu esneklik](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- SQL veritabanı, kapasite ayrılmıştır: İndirim kapsamı seçtiğiniz performans katmanına bağlıdır. Daha fazla bilgi için [bir Azure ayırma indirimi nasıl uygulandığını anlamanız](billing-understand-reservation-charges.md).
- Azure Cosmos DB, kapasite ayrılmıştır: İndirim kapsamı, sağlanan aktarım hızına bağlıdır. Daha fazla bilgi için [bir Azure Cosmos DB ayırma indirimi nasıl uygulandığını anlamanız](billing-understand-cosmosdb-reservation-charges.md).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- Sanal makinelerinizde satın alarak kaydetme başlangıç bir [ayrılmış VM örneği](../virtual-machines/windows/prepay-reserved-vm-instances.md), [SQL veritabanı ayrılan kapasite](../sql-database/sql-database-reserved-capacity.md), veya [Azure Cosmos DB ayrılan kapasite](../cosmos-db/cosmos-db-reserved-capacity.md).
- Azure ayırmaları hakkında daha fazla bilgi içeren aşağıdaki makaleleri öğrenin:
    - [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
    - [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
    - [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
    - [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)
    - [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](https://docs.microsoft.com/partner-center/azure-reservations)
