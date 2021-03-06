---
author: yashesvi
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 07/03/2019
ms.openlocfilehash: 31c6521ca77d9d85fc8388d7ebc5d25defc69bd0
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67568388"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances-ri"></a>Azure ayrılmış VM örnekleri (RI) ile sanal makineler için ön ödeme

Sanal makineler için ödeme yaparak Azure ayrılmış sanal makine (VM) örnekleri ile tasarruf edin. Daha fazla bilgi için [Azure ayrılmış VM örnekleri teklifi](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Ayrılmış VM örneği satın alabileceğiniz [Azure portalında](https://portal.azure.com). Bir örnek satın almak için:

- En az bir kurumsal abonelik veya Kullandıkça Öde fiyatı olan bir abonelik sahibi rolünde olması gerekir.
- Kurumsal abonelikler için **ayrılmış örnekleri ekleme** içinde etkinleştirilmelidir [EA portal](https://ea.azure.com). Veya bu ayarı devre dışıysa, aboneliğini bir EA yönetici olması gerekir.
- Bulut çözümü sağlayıcısı (CSP) programı için yalnızca yönetim aracıları veya satış aracılarının rezervasyon satın alabilirsiniz.

Ayırma indirimi, çalışan sanal makineler, öznitelikleri ve ayırma kapsamı eşleşen sayısı için otomatik olarak uygulanır. Rezervasyon kapsamı güncelleştirebilirsiniz [Azure portalında](https://portal.azure.com), PowerShell, CLI, ya da API aracılığıyla.

## <a name="determine-the-right-vm-size-before-you-buy"></a>Satın almadan önce doğru VM boyutunu belirler.

Rezervasyon satın almadan önce gereken VM boyutunu belirlemeniz gerekir. Aşağıdaki bölümlerde, doğru VM boyutunu belirlemenize yardımcı olur.

### <a name="use-reservation-recommendations"></a>Ayırma önerileri kullanın

Rezervasyon satın almalıyım belirlemeye yardımcı olması için ayırma önerileri kullanabilirsiniz.

- Satın alma önerileri ve önerilen miktar olan Göster Azure portalında bir VM ayrılmış örnek satın aldığınızda.
- Azure Danışmanı için bireysel abonelikler satın alma önerileri sağlar.  
- API'leri, hem Paylaşılan kapsam hem de tek abonelik kapsamında satın alma önerileri almak için kullanabilirsiniz. Daha fazla bilgi için [ayrılmış örnek satın alma öneri API'leri, Kurumsal müşteriler için](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation).
- EA müşterileri, paylaşılan satın alma önerileri ve tek bir abonelik kapsamları ile kullanılabilir [Azure tüketim öngörüleri Power BI içerik paketi](/power-bi/service-connect-to-azure-consumption-insights).

### <a name="classic-vms-and-cloud-services"></a>Klasik VM'ler ve bulut Hizmetleri

Ayrılmış sanal makine örnekleri otomatik olarak her iki Klasik Vm'leri için geçerli ve bulut Hizmetleri örneği boyutu esnekliği etkinleştirildiğinde. Klasik sanal makineleri veya Bulut Hizmetleri için özel bir SKU değildir. Aynı VM SKU'ları için geçerlidir.

Örneğin, Azure Resource Manager tabanlı VM'ler için Klasik sanal makineleri veya cloud services dönüştürebilir. Bu örnekte, ayırma indirimi, eşleşen Vm'leri otomatik olarak uygular. Gerekli olmadığı için *exchange* mevcut bir ayrılmış örnek - otomatik olarak uygular.

### <a name="analyze-your-usage-information"></a>Kullanım bilgilerinizi analiz edin
Hangi rezervasyon satın almanız gerekir belirlenebilmesine yardımcı olmak üzere kullanım bilgilerinizi çözümlemeniz gerekir.

Kullanım verileri, kullanım dosyası ve API'ler kullanılabilir. Bunları birlikte satın almak için hangi ayırma belirlemek için kullanın. Yüksek kullanım satın almak amacıyla ayırma miktarını belirlemek üzere günlük olarak sahip VM örnekleri için denetlemeniz gerekir.

Önlemek `Meter` subcategory ve `Product` kullanım verilerini alanları. Bunlar, premium depolama kullanan sanal makine boyutları arasında ayrım yoktur. Rezervasyon satın alma için VM boyutunu belirlemek için bu alanları kullanmak, boyutu hatalı satın alabilir. Ayırma indirimi beklediğiniz elde etmezsiniz. Bunun yerine, başvurmak `AdditionalInfo` alanındaki kullanım dosyanız veya doğru VM boyutunu belirlemek için kullanım API'si.

### <a name="purchase-restriction-considerations"></a>Satın alma kısıtlama konuları

Ayrılmış VM örnekleri, bazı özel durumlar ile çoğu VM boyutları için kullanılabilir. Ayırma indirimleri aşağıdaki sanal makineleri için geçerli değildir:

- **VM serisi** -A serisi, Av2 serisi veya G serisi.

- **Vm'leri önizlemede** -herhangi bir VM serisi veya Önizleme aşamasında olan boyutu.

- **Bulut** -rezervasyon satın Almanya veya Çin bölgelerinde kullanıma sunulmaz.

- **Yetersiz kota** -tek bir abonelik için kapsamlı bir ayırma vCPU kotası Abonelikteki yeni RI için kullanılabilir olmalıdır. Örneğin, hedef abonelik, kota sınırı 10 vcpu için D serisi varsa, 11 işler için standart_d1 örnekleri için bir ayırma satın alamazsınız. Ayırmalar için kota denetimi, abonelikte dağıtılmış Vm'leri içerir. Örneğin, abonelik için D serisi 10 Vcpu kotası varsa ve dağıtılan iki işler için standart_d1 örneği ise, bu Abonelikteki 10 işler için standart_d1 örnekleri için bir ayırma satın alabilirsiniz. Yapabilecekleriniz [teklif artırma isteği oluşturma](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu sorunu çözmek için.

- **Kapasite kısıtlamaları** - nadir durumlarda, bir bölgede yetersiz kapasite nedeniyle VM alt kümesi için yeni rezervasyon satın boyutları Azure sınırları.

## <a name="buy-a-reserved-vm-instance"></a>Ayrılmış VM örneği satın alın

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Seçin **tüm hizmetleri** > **ayırmaları**.
1. Seçin **Ekle** yeni bir rezervasyon satın alın ve ardından **sanal makine**.
1. Gerekli alanları girin. Çalışan VM ayırma indirim almak için öznitelikleri uygun eşleşen örnekleri. Kapsam ve seçilen miktar indirim almak, sanal makine örneği gerçek sayısını bağlıdır.

| Alan      | Açıklama|
|------------|--------------|
|Abonelik|Ayırma için ödeme yapmak üzere kullanılan abonelik. Aboneliğinizin ödeme yöntemini, ön maliyet ayırma için ücretlendirilir. Kurumsal Anlaşma abonelik türü olmalıdır (sayılar sunar: MS-AZR-0017P veya MS-AZR - 0148 P) ya da Kullandıkça Öde tarifesine göre tek tek bir abonelikle (sayılar sunar: MS-AZR-0003P veya MS-AZR-0023P). Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Kullandıkça Öde tarifesine göre olan bir abonelik için ücretleri, aboneliğinizin kredi kartı veya fatura ödeme yöntemi için faturalandırılır.|    
|`Scope`       |Ayırma'nın kapsamı, bir abonelik veya birden çok abonelik (paylaşılan kapsamı) ele. Seçerseniz: <ul><li>**Tek bir kaynak grup kapsamı** — ayırma indirimi, eşleşen kaynakları yalnızca seçilen kaynak grubunda uygular.</li><li>**Tek abonelik kapsamında** — ayırma indirimi, eşleşen kaynaklara seçili Abonelikteki geçerlidir.</li><li>**Paylaşılan kapsam** — fatura bağlamında uygun aboneliklerin kaynaklarında eşleşen ayırma indirimi geçerlidir. Kurumsal Anlaşma müşterileri için fatura bağlamı kaydı değil. Kullandıkça Öde tarifesine göre ile tek tek abonelikleri için faturalama Hesap Yöneticisi tarafından oluşturulan tüm uygun abonelikleri kapsamıdır.</li></ul>|
|Bölge    |Ayırma tarafından kapsanan Azure bölgesi.|    
|VM Boyutu     |Sanal makine örneği boyutu.|
|İçin en iyi duruma getirme     |Sanal makine örneği boyutu esnekliği, varsayılan olarak seçilidir. Aynı diğer VM'ler için ayırma indirimini uygulamak için örnek boyutunu esneklik değeri değiştirmek için Gelişmiş ayarları [VM boyutu grubu](../articles/virtual-machines/windows/reserved-vm-instance-size-flexibility.md). Kapasite önceliği dağıtımlarınız için veri merkezi kapasitenizi önceliklendirir. Bu, ihtiyaç duyduğunuzda sanal makine örneklerini başlatma yeteneğinizi ek güvence sunar. Kapasite önceliği yalnızca ayırma kapsamı tek bir abonelik olduğunda kullanılabilir. |
|Terim        |Bir yıl veya üç yıl.|
|Miktar    |İçinde rezervasyon satın örnek sayısı. Çalışan faturalandırma indirim almak sanal makine örneği sayısını miktarıdır. Doğu ABD bölgesinde 10 işler için standart_d2 VM çalıştırıyorsanız, örneğin, ardından, miktar avantajı tüm çalışan makineler için en üst düzeye çıkarmak için 10 olarak belirtmeniz gerekir. |

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2PjmT]

## <a name="change-a-reservation-after-purchase"></a>Rezervasyon satın alma sonra Değiştir

Rezervasyon satın alma sonra aşağıdaki türde değişiklikler yapabilirsiniz:

- Ayırma kapsamı güncelleştirme
- Örnek boyutu esneklik (varsa)
- Sahipliği

Ayrıca, daha küçük öbekler halinde ve birleştirme zaten ayırmaları bölme halinde bir ayırma bölebilirsiniz. Değişikliklerin hiçbiri, yeni bir ticari işlem neden veya rezervasyon bitiş tarihini değiştirin.

Satın almanızdan sonra doğrudan aşağıdaki türde değişiklikler yapamaz:

- Mevcut bir ayırma'nın bölgesi
- SKU
- Miktar
- Duration

Ancak, *exchange* değişiklik yapmak istiyorsanız bir ayırma.

## <a name="cancellations-and-exchanges"></a>İptalleri ve değişimler

Ayırma işlemini iptal etmeniz gerekiyorsa %12 erken sonlandırma ücreti tahsil edilebilir. Para iadeleri satın aldığınız fiyattan veya geçerli rezervasyon fiyatından düşük olana göre hesaplanır. Para iadesi yıllık 50.000 ABD doları ile sınırlıdır. Yapılacak para iadesi, %12 erken sonlandırma ücreti kesildikten sonra kalan kullanım süresine göre hesaplanır. Azure portal ve select ayırma Git bir iptal isteğinde bulunmak **para iadesi** bir destek isteği oluşturmak için.

Ayrılmış VM Örnekleri rezervasyonunuzun bölgesini, VM boyutu grubunu veya süresini değiştirmeniz gerekiyorsa aynı veya daha yüksek maliyete sahip olan başka bir rezervasyonla değiştirebilirsiniz. Yeni ayırma işleminin başlangıç tarihi değiştirilen ayırma işleminin başlangıç tarihiyle aynı olmaz. 1 veya 3 yıllık terimi, yeni ayırma oluştururken başlatır. Bir exchange istemek için Azure portalında ayırma gidin ve seçin **Exchange** bir destek isteği oluşturmak için.

Exchange ya da para iadesinin ayırmaları hakkında daha fazla bilgi için bkz. [ayırma değişimleri ve para iadesi](../articles/billing/billing-azure-reservations-self-service-exchange-and-refund.md).

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyon yönetme konusunda bilgi almak için bkz: [Azure ayırmalarını yönetme](../articles/billing/billing-manage-reserved-vm-instance.md).
- Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
    - [Azure ayırmaları nelerdir?](../articles/billing/billing-save-compute-costs-reservations.md)
    - [Azure'da ayırmalarını yönetme](../articles/billing/billing-manage-reserved-vm-instance.md)
    - [Ayırma indirimi nasıl uygulanacağını anlama](../articles/billing/billing-understand-vm-reservation-charges.md)
    - [Kullandıkça Öde tarifesine göre olan bir abonelik için ayırma kullanımını anlama](../articles/billing/billing-understand-reserved-instance-usage.md)
    - [Kurumsal kayıt için ayırma kullanımını anlama](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
    - [Windows yazılım maliyetleri ile ayırmaları dahil değil](../articles/billing/billing-reserved-instance-windows-software-costs.md)
    - [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](https://docs.microsoft.com/partner-center/azure-reservations)
