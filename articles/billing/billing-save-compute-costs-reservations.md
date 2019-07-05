---
title: Azure Ayırmaları nedir?
description: Sanal makinelerinizde, SQL veritabanları, Azure Cosmos DB ve diğer kaynak maliyetleri kaydetmek için Azure ayırmaları ve fiyatlandırma hakkında bilgi edinin.
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: banders
ms.openlocfilehash: cd0a70aa0fb5096c5b0157ae078c961da03109bc
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565344"
---
# <a name="what-are-azure-reservations"></a>Azure Ayırmaları nedir?

Azure ayırmalar için bir yıllık ön ödeme yaparak paradan tasarruf etmek veya kapasite, Azure Cosmos DB, aktarım hızı veya diğer Azure kaynaklarını üç yıl sanal makineler, SQL veritabanı, işlem. Önceden ödeme kullandığınız kaynaklar üzerinde bir indirim almak sağlar. Rezervasyonlar, sanal makine, SQL veritabanı işlem, Azure Cosmos DB, önemli ölçüde azaltabilir ya da diğer kaynak en fazla %72 Kullandıkça Öde fiyatlarında maliyetlerini. Rezervasyon faturalandırma indirim sağlar ve kaynaklarınızın çalışma zamanı durumunu etkilemez.

Rezervasyon satın alabileceğiniz [Azure portalında](https://ms.portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

## <a name="why-buy-a-reservation"></a>Neden bir rezervasyon satın?

Sanal makineleri, Azure Cosmos DB veya uzun süreler için çalışan SQL veritabanları varsa, rezervasyon satın alma, en uygun maliyetli bir seçenek sağlar. Örneğin, dört örnek ayırma olmadan bir hizmetin sürekli olarak çalıştırdığınızda, Kullandıkça Öde fiyatları üzerinden ücret ödersiniz. Bu kaynaklar için bir ayırma satın alırsanız, ayırma indirimini hemen erişin. Kaynakları artık Kullandıkça Öde fiyatları üzerinden ücretlendirilir.

## <a name="charges-covered-by-reservation"></a>Ayırma tarafından kapsanan ücretleri

Hizmet planları:

- **Ayrılmış sanal makine örneği** -ayırma yalnızca sanal makine işlem maliyetlerini kapsar. Bu, ek yazılım, ağ ve depolama ücretleri ele alınmamıştır.
- **Azure Cosmos DB ayrılan kapasite** -kaynaklarınız için sağlanan aktarım hızı bir ayırma kapsar. Depolama ve ağ ücretleri ele alınmamıştır.
- **SQL veritabanı sanal çekirdek ayrılmış** - yalnızca bir ayırma ile işlem maliyetleri dahildir. Lisans ayrı olarak faturalandırılır.

Windows sanal makineler ve SQL veritabanı için lisanslama maliyetleri kapsayan [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Rezervasyon satın almak uygun kimdir?

Bir plan satın almak için bir abonelik sahibi rolünde Kurumsal (MS-AZR - 0017 P veya MS-AZR - 0148 P) veya Kullandıkça Öde aboneliğine (MS-AZR - 003 P veya MS-AZR - 0023 P) olmalıdır. Bulut çözüm sağlayıcıları, Azure portalını kullanabilir veya [iş ortağı Merkezi](/partner-center/azure-reservations) Azure rezervasyon satın almak için.

EA müşterileri, satın alma işlemleri EA yöneticilere devre dışı bırakarak sınırlandırabilir **ayrılmış örnekleri ekleme** EA Portal seçeneği. EA yöneticileri rezervasyon satın almak için en az bir kurumsal Anlaşma abonelik için abonelik sahibi olmanız gerekir. Seçeneği, farklı Maliyet merkezleri için ayırma satın almak için merkezi bir ekip istediğiniz kuruluşlar için kullanışlıdır. Satın almanızdan sonra maliyet merkezi sahipleri ayırmaları merkezi takımlar ekleyebilirsiniz. Sahipler, ardından aboneliklerini ayırmaya kapsamını belirleyebilirsiniz. Merkezi takım burada rezervasyon satın alınan abonelik sahibi erişimi olması gerekmez.

Ayırma indirimi, yalnızca Enterprise, CSP ve Kullandıkça Öde tarifesine göre tek tek planlarıyla aracılığıyla satın alınan aboneliklere ilişkili kaynaklar için geçerlidir.

## <a name="scope-reservations"></a>Kapsam ayırmalar

Bir abonelik veya kaynak grupları için bir ayırma kapsamını belirleyebilirsiniz. Rezervasyon için kapsam ayırma tasarruf burada geçerli seçer ayarlanıyor. Bir kaynak grubu ayırmaya kapsamını, yalnızca kaynak grubu için ayırma indirimleri uygulanır; aboneliğin tümü değil.

### <a name="reservation-scoping-options"></a>Rezervasyon için kapsam belirleme seçenekleri

Kaynak grubu, kapsam ihtiyaçlarınıza bağlı olarak bir ayırma kapsamı için üç seçeneğiniz vardır:

- **Tek bir kaynak grup kapsamı** — ayırma indirimi, eşleşen kaynakları yalnızca seçilen kaynak grubunda uygular.
- **Tek abonelik kapsamında** — ayırma indirimi, eşleşen kaynaklara seçili Abonelikteki geçerlidir.
- **Paylaşılan kapsam** — fatura bağlamında uygun aboneliklerin kaynaklarında eşleşen ayırma indirimi geçerlidir. Kurumsal Anlaşma müşterileri için fatura bağlamı kaydı değil. Kullandıkça Öde tarifesine göre ile tek tek abonelikleri için faturalama Hesap Yöneticisi tarafından oluşturulan tüm uygun abonelikleri kapsamıdır.

Ayırma indirimler, kullanımınıza uygulanırken, Azure aşağıdaki sırayla ayırma işlemleri:

1. Bir kaynak grubu için kapsamlı ayırmalar
2. Tek bir kapsam ayırmalar
3. Paylaşılan kapsam ayırmalar

Tek bir kaynak grubu nasıl ayırmalarınızın kapsamını bağlı olarak birden çok ayırmalardan ayırma indirimler elde edebilirsiniz.

### <a name="scope-a-reservation-to-a-resource-group"></a>Bir kaynak grubu için bir ayırma kapsamı

Rezervasyon satın alma veya satın almanızdan sonra kapsamı ayarlayın, bir kaynak grubu ayırmaya kapsamını sınırlandırabilirsiniz. Bir kaynak grubu ayırmaya kapsam için bir abonelik sahibi olmanız gerekir.

Kapsamını ayarlamak için şuraya gidin: [rezervasyon satın](https://ms.portal.azure.com/#blade/Microsoft\_Azure\_Reservations/CreateBlade/referrer/Browse\_AddCommand) Azure portalında sayfası. Ardından satın almak istediğiniz ayırma türü seçin. Üzerinde **satın almak istediğiniz ürünü seçin** seçimi form, değişiklik **kapsam** değerini **tek bir kaynak grubu** ve bir kaynak grubu seçin.

![VM rezervasyon satın alma seçimi gösteren örnek](./media/billing-save-compute-costs-reservations/select-product-to-purchase.png)

Kaynak grubunda sanal makine rezervasyonu için satın alma önerileri gösterilir. Öneriler son 30 gün içindeki kullanımınızı analiz ederek hesaplanır. Ayrılmış örnekleri ile kaynakları çalıştırmanın maliyeti ucuz kaynaklar ile Kullandıkça Öde tarifesine göre çalıştırmanın maliyeti ise bir satın alma önerisi yapılır. Rezervasyon satın alma önerileri hakkında daha fazla bilgi için bkz. [alma ayrılmış örnek satın alma önerileri kullanım deseni temel alınarak](https://azure.microsoft.com/blog/get-usage-based-reserved-instance-recommendations) blog gönderisi.

Rezervasyon satın alma sonra kapsam her zaman güncelleştirebilirsiniz. Bunu yapmak için ayırmaya gidin, **yapılandırma** ve ayırma rescope. Rezervasyon rescoping ticari bir işlem değil. Ayırma döneminizin değiştirilmez. Kapsamı güncelleştirme hakkında daha fazla bilgi için bkz. [rezervasyon satın aldıktan sonra kapsam güncelleştirme](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).

![Ayırma kapsamı değişiklik gösteren örnek](./media/billing-save-compute-costs-reservations/rescope-reservation-resource-group.png)

### <a name="monitor-and-optimize-reservation-usage"></a>İzleme ve ayırma kullanımı en iyi duruma getirme

Ayırma kullanımınızı – Azure portalı üzerinden, API'ler aracılığıyla veya kullanım verilerini çeşitli şekillerde izleyebilirsiniz. Tüm olan rezervasyonlar erişim görmek için Git **ayırmaları** Azure portalında. Ayırmaları ızgara ayırma için son kaydedilen kullanım yüzdesini gösterir. Ayırma, ayırma uzun vadeli kullanımını görmek için tıklayın.

Ayırma kullanımı kullanarak da alabilirsiniz [API'leri](billing-reservation-apis.md#see-reservation-usage) ve, [kullanım verilerini](billing-understand-reserved-instance-usage-ea.md#common-cost-and-usage-tasks) bir Kurumsal Sözleşme müşterisi iseniz.

Kaynak grubunuzun kullanımı ayırma kapsamı fark ederseniz, tek abonelik veya fatura bağlamı arasında paylaşmak için ayırma kapsamı güncelleştirebilirsiniz sonra düşüktür. Ayrıca, ayırma bölme ve elde edilen ayırmaları farklı kaynak gruplarına uygulayabilirsiniz.

### <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Eşleşen bir kaynak grubundaki kaynaklar yoksa, daha sonra Rezervasyon potansiyelinden az kullanılmasına neden. Ayırma farklı kaynak grubuna veya aboneliğe otomatik olarak uygulanmaz düşük kullanımı olduğu.

Kaynak grubu bir abonelikten diğerine taşıdığınızda ayırma kapsamı otomatik olarak güncelleştirmez. Rezervasyon rescope gerekecektir. Aksi takdirde, rezervasyon gereğinden az.

## <a name="discounted-subscription-and-offer-types"></a>İndirimli abonelik ve teklif türleri

Ayırma indirim uygulamak için aşağıdaki uygun abonelikleri ve türleri sunulur.

- Kurumsal Anlaşma (sayılar sunar: MS-AZR-0017P veya MS-AZR - 0148 P)
- Kullandıkça Öde tarifesine göre tek tek planlarıyla (sayılar sunar: MS-AZR-0003P veya MS-AZR - 0023 P)
- CSP abonelikleri

Ayırma indirimi çalıştıran diğer teklif türleri ile bir Abonelikteki kaynakları almaz.

## <a name="how-is-a-reservation-billed"></a>Rezervasyon nasıl faturalandırılır?

Ayırma, aboneliğe bağlı ödeme yöntemi için ücretlendirilir. Bir kurumsal abonelik varsa, rezervasyon maliyeti parasal taahhüt bakiyeniz çıkarılır. Parasal taahhüt bakiyeniz rezervasyon maliyeti yoksa, faturalandırılırsınız kapasite aşımı. Tek bir plan ile Kullandıkça Öde tarifesine göre bir abonelik varsa, kredi kartı hesabınızda sahip hemen faturalandırılır. Fatura ile faturalandırılırsınız sonraki faturanızı ücretleri görürsünüz.

## <a name="how-reservation-discount-is-applied"></a>Ayırma indirimi nasıl uygulanır

Ayırma indirimi, rezervasyon satın aldığınızda seçtiğiniz öznitelikleri eşleşen kaynak kullanımı için geçerlidir. Öznitelikler, eşleşen VM'ler, SQL veritabanları, Azure Cosmos DB veya diğer kaynaklara çalıştırdığı kapsamı içerir. Örneğin, Batı ABD bölgesindeki dört standart D2 sanal makineler için bir ayırma indirimi istiyorsanız, ardından sanal makinelerin çalıştığı aboneliği seçin.

Ayırma indirimi olan "*kullanın-BT-veya-kaybetmek-BT*". Ardından kaynakları için saat eşleştirme yoksa, bu saat için bir ayırma miktarını kaybedersiniz. Yerine getirilemiyor kullanılmayan ayrılmış saat iletin.

Bir kaynak kapattığınızda, ayırma indirimini Belirtilen kapsam içinde başka bir eşleşen kaynak otomatik olarak uygular. Belirtilen kapsamda bulunan eşleşen kaynak yok sonra ayrılmış saatleri *kayıp*.

Örneğin, olabileceğiniz daha sonra bir kaynak oluşturmak ve potansiyelinden az kullanılmasına neden eşleşen bir ayırma vardır. Ayırma indirimi, bu örnekte, yeni eşleşen kaynağı otomatik olarak uygular.

Kayıt/hesabınızda farklı Aboneliklerde bulunan sanal makineleri çalıştırıyorsanız, kapsam olarak paylaşılan seçin. Paylaşılan kapsam ayırma indirimi, abonelikler arasında uygulanmasına olanak tanır. Rezervasyon satın alma sonra kapsam değiştirebilirsiniz. Daha fazla bilgi için [Azure ayırmalarını yönetme](billing-manage-reserved-vm-instance.md).

Ayırma indirimi yalnızca Enterprise, CSP ile ilişkili kaynaklar için geçerlidir veya ödeme-olarak-, aboneliklerle oranları gidin. Ayırma indirimi çalıştıran diğer teklif türleri ile bir Abonelikteki kaynakları almaz.

## <a name="when-the-reservation-term-expires"></a>Ayırma dönemi sona erdiğinde

Ayırma dönemi sonunda, fatura indirim süresi dolar ve sanal makine, SQL veritabanı, Azure Cosmos DB veya diğer kaynak ödeme--, go fiyattan faturalandırılır. Azure ayırmaları otomatik olarak yenilenecek yok. Fatura indirim almaya devam etmek için uygun hizmetler ve yazılım için yeni bir rezervasyon satın almanız gerekir.

## <a name="discount-applies-to-different-sizes"></a>İndirimi, farklı boyutlarda için geçerlidir.

Rezervasyon satın aldığınızda, indirim diğer örnekleriyle aynı boyut grubu içinde öznitelikleri uygulayabilirsiniz. Bu özellik, örneği boyutu esnekliği bilinir. İndirim Karşılama esnekliği, ayırma ve rezervasyon satın aldığınızda, çekme özniteliklerinin türüne bağlıdır.

Hizmet planları:

- Ayrılmış VM örnekleri: Ne zaman rezervasyon satın alma ve seçin **için en iyi duruma getirilmiş**: **örnek boyutu esneklik**, indirim kapsamı seçtiğiniz VM boyutuna bağlıdır. Ayırma aynı boyutu seri grubu içindeki sanal makineler (VM) boyutlarına uygulayabilirsiniz. Daha fazla bilgi için [ayrılmış VM örnekleri ile sanal makine boyutu esneklik](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- SQL veritabanı, kapasite ayrılmıştır: İndirim kapsamı seçtiğiniz performans katmanına bağlıdır. Daha fazla bilgi için [bir Azure ayırma indirimi nasıl uygulandığını anlamanız](billing-understand-reservation-charges.md).
- Azure Cosmos DB, kapasite ayrılmıştır: İndirim kapsamı, sağlanan aktarım hızına bağlıdır. Daha fazla bilgi için [bir Azure Cosmos DB ayırma indirimi nasıl uygulandığını anlamanız](billing-understand-cosmosdb-reservation-charges.md).

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- Azure ayırmaları hakkında daha fazla bilgi içeren aşağıdaki makaleleri öğrenin:
    - [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
    - [Aboneliğinizin Kullandıkça Öde tarifesine göre ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
    - [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
    - [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)
    - [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](/partner-center/azure-reservations)

- Hizmet planları için ayırma hakkında daha fazla bilgi edinin:
    - [Azure ayrılmış VM örnekleri ile sanal makineler](../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Azure Cosmos DB ile Azure Cosmos DB kaynaklarını ayrılmış kapasite](../cosmos-db/cosmos-db-reserved-capacity.md)
    - [Azure SQL veritabanı ile SQL veritabanı işlem kaynakları ayrılan kapasite](../sql-database/sql-database-reserved-capacity.md) ayırmaları yazılım planları hakkında daha fazla bilgi edinin:
    - [Red Hat yazılımı planlarından Azure ayırmalar](../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Azure ayırmalardan SUSE yazılım planları](../virtual-machines/linux/prepay-suse-software-charges.md)
