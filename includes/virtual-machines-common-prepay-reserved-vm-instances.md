---
author: yashesvi
ms.author: yashar
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 09-05-2018
ms.openlocfilehash: 75e493f6b2e47bf3323df9e52ad3a933a6efbead
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44058342"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Azure ayrılmış VM örnekleri ile sanal makineler için ön ödeme

Sanal makineler için ödeme yaparak Azure ayrılmış sanal makine (VM) örnekleri ile tasarruf edin. Daha fazla bilgi için [Azure ayrılmış VM örnekleri teklifi](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Ayrılmış VM örneği satın alabileceğiniz [Azure portalında](https://portal.azure.com). Bir örnek satın almak için:

- En az bir kuruluş veya Kullandıkça Öde aboneliğine sahip rolünde olması gerekir.
- Kurumsal abonelikler için rezervasyon satın alma işlemleri içinde etkinleştirilmelidir [EA portal](https://ea.azure.com).
- Bulut çözümü sağlayıcısı (CSP) programı için yalnızca yönetim aracıları veya satış aracılarının rezervasyon satın alabilirsiniz.

## <a name="determine-the-right-vm-size-before-purchase"></a>Satın almadan önce doğru VM boyutunu belirler

Kullanım verileri ölçüm alt kategorisi ve ürün alanlarını premium Depolama'yı kullanma premium depolama VM boyutlarını kullanma VM boyutları arasında ayırt etmek değil, VM belirlemek için bu alanı kullanarak rezervasyon satın alma için boyutu için yanlış açabilir Rezervasyon satın alın ve ayırma indirimler sağlar değil. Rezervasyon satın alma için doğru VM boyutunu belirlemek için aşağıdaki yöntemlerden birini kullanın.

- Kullanım dosyanız veya rezervasyon satın alma için doğru VM boyutunu belirlemek için kullanım API'si Additionalınfo alanına bakın. Bu alanlar bir VM S ve S olmayan sürümleri arasında ayrım yapmaz beri ölçüm alt kategorisi veya ürün alanlarındaki değerleri kullanmayın.
- Powershell, Azure Resource Manager kullanarak doğru VM boyutunu bilgilerini de alabilirsiniz veya Azure portalında sanal makineden ayrıntıları.

Ayrılmış VM örnekleri, bazı özel durumlar ile çoğu VM boyutları için kullanılabilir:

- Klasik VM'ler ve bulut Hizmetleri, ayırma indirimini alma yok.
- Gizlenen çekirdek VM ayırma indirimler elde etmezsiniz.
- Aşağıdaki VM serisi ayırma indirimleri seçimlerde: A serisi, Av2 serisi veya G serisi.
- Vm'leri önizlemede: herhangi bir VM serisi veya Önizleme aşamasında olan boyutu rezervasyon satın alma için kullanılabilir değildir.
- Bulut: Rezervasyon satın alma Azure ABD kamu, Almanya ve Çin bölgelerinde kullanılamaz.
- Yetersiz kota: tek bir abonelik için kapsamlı bir ayırma vCPU kotası Abonelikteki yeni RI için kullanılabilir olmalıdır. Örneğin, hedef abonelik, kota sınırı 10 vcpu için D serisi varsa, 11 işler için standart_d1 örnekleri için bir ayırma satın alamazsınız. Ayırmalar için kota denetimi, abonelikte dağıtılmış Vm'leri içerir. Örneğin, abonelik için D serisi 10 Vcpu kotası varsa ve dağıtılan iki işler için standart_d1 örneği ise, bu Abonelikteki 10 işler için standart_d1 örnekleri için bir ayırma satın alabilirsiniz. 
- Kapasite kısıtlamaları: nadir durumlarda, bir bölgede yetersiz kapasite nedeniyle VM boyutlarının alt kümesi için yeni rezervasyon satın Azure sınırlar.

## <a name="buy-a-reserved-vm-instance"></a>Ayrılmış VM örneği satın alın

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Seçin **Ekle** yeni bir rezervasyon satın almak için.
4. Gerekli alanları doldurun. Çalışan VM ayırma indirim almak için öznitelikleri uygun eşleşen örnekleri. Kapsam ve seçilen miktar indirim almak, sanal makine örneği gerçek sayısını bağlıdır.

    | Alan      | Açıklama|
    |:------------|:--------------|
    |Ad        |Bu rezervasyon adı.| 
    |Abonelik|Ayırma için ödeme yapmak üzere kullanılan abonelik. Aboneliğinizin ödeme yöntemini, ön maliyet ayırma için ücretlendirilir. Abonelik, kurumsal anlaşma (teklif numarası: MS-AZR-0017P) veya Kullandıkça Öde (teklif numarası: MS-AZR-0003P) türündedir. Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.|    
    |Kapsam       |Ayırma'nın kapsamı, bir abonelik veya birden çok abonelik (paylaşılan kapsamı) ele. Seçerseniz: <ul><li>Tek bir abonelik - ayırma indirimini bu abonelikte Vm'lere uygulanır. </li><li>Paylaşılan - ayırma indirimi herhangi bir abonelik, fatura bağlamı içinde çalışan Vm'lere uygulanır. Kurumsal müşteriler için Paylaşılan kapsam kayıt ve kayıt (geliştirme ve test abonelikleri) hariç tüm aboneliklere dahildir. Kullandıkça Öde müşterileri için paylaşılan tüm Kullandıkça Öde abonelikleri Hesap Yöneticisi tarafından oluşturulan kapsamdır.</li></ul>|
    |Bölge    |Ayırma tarafından kapsanan Azure bölgesi.|    
    |VM Boyutu     |Sanal makine örneği boyutu.|
    |En iyi duruma getir:     |Sanal makine örneği boyutu esnekliği, aynı diğer VM'ler için ayırma indirimi geçerlidir [VM boyutu grubu](https://aka.ms/RIVMGroups). Kapasite önceliği dağıtımlarınız için veri merkezi kapasitenizi önceliklendirir. Bu, ihtiyaç duyduğunuzda sanal makine örneklerini başlatma yeteneğinizi ek güvence sunar. Kapasite önceliği yalnızca ayırma kapsamı tek bir abonelik olduğunda kullanılabilir. |
    |Sözleşme Dönemi        |Bir yıl veya üç yıl.|
    |Miktar    |İçinde rezervasyon satın örnek sayısı. Çalışan faturalandırma indirim almak sanal makine örneği sayısını miktarıdır. Doğu ABD bölgesinde 10 işler için standart_d2 VM çalıştırıyorsanız, örneğin, ardından, miktar avantajı tüm çalışan makineler için en üst düzeye çıkarmak için 10 olarak belirtmeniz gerekir. |
5. Seçtiğinizde, rezervasyon maliyeti görüntüleyebilirsiniz **maliyeti hesaplamak**.

    ![Rezervasyon satın alma göndermeden önce ekran görüntüsü](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. **Satın al**'ı seçin.
7. Seçin **bu rezervasyonu görüntüle** satın alma işleminizi durumunu görmek için.

    ![Rezervasyon satın alma gönderdikten sonra ekran görüntüsü](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="cancellations-and-exchanges"></a>İptalleri ve değişimler

Rezervasyonunuz iptal etmeniz gerekiyorsa, % 12 erken sonlandırma ücreti olabilir. Para iadesi veya satın alma fiyatına, hem de ayırmanın geçerli fiyatını düşük fiyatı temel alınarak temel alır. Para iadesi yıl başına 50.000 TL sınırlıdır. Para iadesi alırsınız % 12 erken sonlandırma ücreti kalan günlere Bakiye ' dir. Azure portal ve select ayırma Git bir iptal isteğinde bulunmak **para iadesi** bir destek isteği oluşturmak için.

Başka bir bölge, VM boyutu grubu veya terimi, ayrılmış VM örnekleri ayırma değiştirmeniz gerekiyorsa, eşit veya daha fazla değeri için başka bir ayırma gönderip alabilir. Yeni Ayırma dönemi başlangıç tarihi alışverişi olur ayırma aktarılmaz. 1 veya 3 yıllık dönemin yeni ayırma oluştururken başlatır. Bir exchange istemek için Azure portalında ayırma gidin ve seçin **Exchange** bir destek isteği oluşturmak için.

## <a name="next-steps"></a>Sonraki adımlar

Ayırma indirimi, çalışan sanal makineler, öznitelikleri ve ayırma kapsamı eşleşen sayısı için otomatik olarak uygulanır. Rezervasyon kapsamı güncelleştirebilirsiniz [Azure portalında](https://portal.azure.com), PowerShell, CLI, ya da API aracılığıyla.

Rezervasyon yönetme konusunda bilgi almak için bkz: [Azure ayırmalarını yönetme](../articles/billing/billing-manage-reserved-vm-instance.md).

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Azure'da ayırmalarını yönetme](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Ayırma indirimi nasıl uygulanacağını anlama](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [İş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmalar](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
