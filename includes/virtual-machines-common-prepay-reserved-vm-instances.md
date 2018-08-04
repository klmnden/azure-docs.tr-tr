---
ms.topic: include
ms.openlocfilehash: c30dbe25252a18e1edaed5ec81d86cc1dd976250
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39513812"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Azure ayrılmış VM örnekleri ile sanal makineler için ön ödeme

Sanal makineler için ödeme yaparak Azure ayrılmış sanal makine (VM) örnekleri ile tasarruf edin. Daha fazla bilgi için [Azure ayrılmış VM örnekleri teklifi](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Azure ayrılmış örnekler satın alabileceğiniz [Azure portalında](https://portal.azure.com). Ayrılmış örnek satın almak için:
-   En az bir kuruluş veya Kullandıkça Öde aboneliğine sahip rolünde olması gerekir.
-   Kurumsal abonelikler için ayrılmış örnek satın alma işlemleri içinde etkinleştirilmelidir [EA portal](https://ea.azure.com).
-   Bulut çözümü sağlayıcısı (CSP) programı için yalnızca yönetim aracıları veya satış aracılarının ayrılmış örnekler satın alabilirsiniz.

## <a name="determine-the-right-vm-size-before-purchase"></a>Satın almadan önce doğru VM boyutunu belirler
Kullanım verileri ölçüm alt kategorisi ve ürün alanlarını premium Depolama'yı kullanma premium depolama VM boyutlarını kullanma VM boyutları arasında ayırt etmek değil, VM belirlemek için bu alanı kullanarak rezervasyon satın alma için boyutu için yanlış açabilir Rezervasyon satın alın ve ayırma indirimler sağlar değil. Rezervasyon satın alma için doğru VM boyutunu belirlemek için aşağıdaki yöntemlerden birini kullanın.
- Başvurmak için Additionalınfo > kullanım dosyanız veya kullanım API'si veri ServiceType alanı. Bu alan doğru VM boyutu premium depolama kullanan VM'ler dağıtırken sağlar.
- Powershell, Azure Resource Manager kullanarak doğru VM boyutunu bilgilerini de alabilirsiniz veya Azure portalında sanal makineden ayrıntıları.

## <a name="buy-a-reserved-virtual-machine-instance"></a>Ayrılmış sanal makine örneği satın alın
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Seçin **Ekle** yeni bir ayrılmış örnek satın almak için.
4. Gerekli alanları doldurun. Çalışan VM öznitelikleri ayrılmış örnek indirim almak için uygun eşleşen örnekleri. Kapsam ve seçilen miktar indirim almak, sanal makine örneği gerçek sayısını bağlıdır.

    | Alan      | Açıklama|
    |:------------|:--------------|
    |Ad        |Bu ayrılmış örnek adı.| 
    |Abonelik|Ayrılmış örnek için ödeme yapmak üzere kullanılan abonelik. Abonelikteki ödeme yöntemi, Ayrılmış Örnek için ön maliyetleri ücretlendirir. Abonelik, kurumsal anlaşma (teklif numarası: MS-AZR-0017P) veya Kullandıkça Öde (teklif numarası: MS-AZR-0003P) türündedir. Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir. Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.|    
    |Kapsam       |Ayrılmış örneğin kapsam bir abonelik veya birden çok abonelik (paylaşılan kapsamı) ele. Seçerseniz: <ul><li>Tek bir abonelik - ayrılmış örnek indirimi bu abonelikte Vm'lere uygulanır. </li><li>Paylaşılan - ayrılmış örnek indirimi herhangi bir abonelik, fatura bağlamı içinde çalışan Vm'lere uygulanır. Kurumsal müşteriler için Paylaşılan kapsam kayıt ve kayıt (geliştirme ve test abonelikleri) hariç tüm aboneliklere dahildir. Kullandıkça Öde müşterileri için paylaşılan tüm Kullandıkça Öde abonelikleri Hesap Yöneticisi tarafından oluşturulan kapsamdır.</li></ul>|
    |Konum    |Ayrılmış örnek tarafından kapsanan Azure bölgesi.|    
    |VM Boyutu     |Sanal makine örneği boyutu.|
    |En iyi duruma getir:     |Sanal makine örneği boyutu esnekliği, aynı diğer VM'ler için ayırma indirimi geçerlidir [VM boyutu grubu](https://aka.ms/RIVMGroups). Kapasite önceliği dağıtımlarınız için veri merkezi kapasite ayırır. Bu, ihtiyaç duyduğunuzda sanal makine örneklerini başlatma yeteneğinizi ek güvence sunar. Kapasite önceliği yalnızca ayırma kapsamı tek bir abonelik olduğunda kullanılabilir. |
    |Sözleşme Dönemi        |Bir yıl veya üç yıl.|
    |Miktar    |İçinde ayrılmış örnek satın örnek sayısı. Çalışan faturalandırma indirim almak sanal makine örneği sayısını miktarıdır. Doğu ABD bölgesinde 10 işler için standart_d2 VM çalıştırıyorsanız, örneğin, ardından, miktar avantajı tüm çalışan makineler için en üst düzeye çıkarmak için 10 olarak belirtmeniz gerekir. |
5. Seçtiğinizde, ayrılmış örnek maliyeti görüntüleyebilirsiniz **maliyeti hesaplamak**.

    ![Ayrılmış örnek satın göndermeden önce ekran görüntüsü](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. **Satın al**'ı seçin.
7. Seçin **bu rezervasyonu görüntüle** satın alma işleminizi durumunu görmek için.

    ![Ayrılmış örnek satın gönderdikten sonra ekran görüntüsü](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Sonraki adımlar 
Ayrılmış örnek indirimi, öznitelikleri ve ayrılmış örnek kapsamını eşleşen sanal çalışan sayısı için otomatik olarak uygulanır. Ayrılmış örnek kapsamını güncelleştirebilirsiniz [Azure portalında](https://portal.azure.com), PowerShell, CLI veya API üzerinden. 

Ayrılmış örnek yönetme konusunda bilgi almak için bkz: [yönetme Azure ayrılmış örnekler](../articles/billing/billing-manage-reserved-vm-instance.md).

Azure ayrılmış örnekleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure ayrılmış VM örnekleri nelerdir?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Azure ayrılmış örnekler yönetme](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Ayrılmış örnek indiriminin nasıl uygulandığını anlama](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlama](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için ayrılmış örnek kullanımını anlama](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Ayrılmış örneklere dahil olmayan Windows yazılım maliyetleri](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [İş Ortağı Bulut Çözümü Sağlayıcısı (CSP) programında ayrılmış örnekler](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
