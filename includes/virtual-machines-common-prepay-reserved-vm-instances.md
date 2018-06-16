---
ms.topic: include
ms.openlocfilehash: 99eaa667e4c6a9d63b4cc43ada8c6e36f7365610
ms.sourcegitcommit: 39f4911b5933f7062dcf5d57af94eab8a0740b2b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2018
ms.locfileid: "35683058"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Azure ayrılmış VM örnekleri ile sanal makineler için ön ödeme

Sanal makineler için ön ödeme ve Azure ayrılmış sanal makine (VM) örnekleriyle paradan tasarruf. Daha fazla bilgi için bkz: [Azure ayrılmış örnekler teklifi](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Azure ayrılmış örnekler satın alabileceğiniz [Azure portal](https://portal.azure.com). Ayrılmış örnek satın almak için:
-   En az bir kuruluş veya Kullandıkça Öde aboneliğine sahip rolünde olması gerekir.
-   Ayrılmış örnek satın alma işlemleri Kurumsal abonelikler için etkinleştirilmesi gerekir [EA portal](https://ea.azure.com).
-   Bulut çözümü sağlayıcısı (CSP) programı için ayrılmış örnekler yalnızca yönetim aracıları veya satış aracılarının satın alabilirsiniz.

[!IMPORTANT]
Tanımlamak için aşağıda açıklanan yöntemlerden birini kullanmalıdır doğru ayırma satın alma için VM boyutu.

## <a name="determine-the-right-vm-size-before-purchase"></a>Satın alma önce sağ VM boyutu belirleme
1. Kullanım dosyasını veya rezervasyon satın alma için doğru VM boyutunu belirlemek için kullanım API'si additionalınfo alanına alanına bakın. Bu alan bir VM S ve Non-S sürümleri arasında ayrım yapmaz beri değerlerini ölçer alt kategori veya ürün alanlardan kullanmayın.
2. Ayrıca Azure Resource Manager Powershell kullanarak doğru VM boyutu bilgi elde edebilirsiniz veya Azure portalında sanal makineden ayrıntıları.

## <a name="buy-a-reserved-virtual-machine-instance"></a>Ayrılmış sanal makine örneğini satın alın
1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Seçin **Ekle** yeni bir ayrılmış örnek satın alın.
4. Gerekli alanları doldurun. Ayrılmış örnek indirim almak için seçtiğiniz özniteliklerine nitelemek eşleşen çalışan VM örnekleri. İndirim almak üzerindeki VM örneklerinize gerçek sayısını kapsam ve seçili miktar bağlıdır.

    | Alan      | Açıklama|
    |:------------|:--------------|
    |Ad        |Bu ayrılmış örnek adı.| 
    |Abonelik|Ayrılmış örnek için ödeme yapmak üzere kullanılan aboneliği. Abonelik için ödeme yöntemini ön maliyetleri ayrılmış örnek için ücret kesilir. Abonelik türü bir Kurumsal Anlaşma olmalıdır (teklif numarası: MS-AZR - 0017P) veya Kullandıkça Öde (teklif numarası: MS-AZR - 0003P). Bir kurumsal aboneliği için ücretleri kayıt ait parasal taahhüt bakiyenin dışında kesinti veya fazla kullanım ücretlendirilir. Kullandıkça Öde abonelik için aboneliğe kredi kartı veya fatura ödeme yöntemine ücret faturalandırılır.|    
    |Kapsam       |Ayrılmış örneğinin kapsamı, bir abonelik ya da birden çok abonelik (Paylaşılan kapsam) ele. Seçerseniz: <ul><li>Tek bir abonelik - ayrılmış örnek indirim VM'ler için bu abonelikte uygulanır. </li><li>Paylaşılan - hiç abonelik faturalama içeriğiniz içinde çalışan sanal makineler için ayrılmış örnek indirim uygulanır. Kurumsal müşteriler için Paylaşılan kapsam kayıt ve kayıt içindeki tüm abonelikleri (dışında geliştirme ve test abonelikleri) içerir. Kullandıkça Öde müşteriler için Paylaşılan kapsam Hesap Yöneticisi tarafından oluşturulan tüm Kullandıkça Öde abonelikleri içindir.</li></ul>|
    |Konum    |Ayrılmış örnek tarafından kapsanan Azure bölgesi.|    
    |VM Boyutu     |VM örnekleri boyutu.|
    |Sözleşme Dönemi        |Bir yıl veya üç yıl.|
    |Miktar    |Ayrılmış örnek içinde satın örneği sayısı. Fatura indirim almak VM örnekleri çalışan sayısı miktarıdır. Doğu ABD'de 10 Standard_D2 sanal makineleri çalıştırıyorsanız, örneğin, miktar avantajı tüm çalışan makineler için en üst düzeye çıkarmak için 10 olarak belirtirsiniz. |
5. Seçtiğinizde, ayrılmış örnek maliyetini görüntüleyebilirsiniz **hesapla maliyet**.

    ![Ayrılmış örnek satın alma göndermeden önce ekran görüntüsü](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Seçin **satın alma**.
7. Seçin **bu ayırma görüntülemek** satın alma işleminiz durumunu görmek için.

    ![Ayrılmış örnek satın alma gönderdikten sonra ekran görüntüsü](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Sonraki adımlar 
Ayrılmış örnek indirim öznitelikleri ve ayrılmış örnek kapsamını eşleşen sanal makinelerde çalışan sayısı için otomatik olarak uygulanır. Ayrılmış örnek kapsamını güncelleştirebilirsiniz [Azure portal](https://portal.azure.com), PowerShell'i, CLI veya API'si aracılığıyla. 

Ayrılmış örnek yönetme konusunda bilgi almak için bkz: [Azure ayrılmış örnekler yönetmek](../articles/billing/billing-manage-reserved-vm-instance.md).

Azure ayrılmış örnekler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Ayrılmış örnekler sahip sanal makinelerde paradan tasarruf](../articles/billing/billing-save-compute-costs-reservations.md)
- [Azure ayrılmış örnekler yönetme](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Ayrılmış örnek indirim nasıl uygulandığını anlama](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak](../articles/billing/billing-understand-reserved-instance-usage.md)
- [İşletme kaydı için ayrılmış örnek kullanım anlama](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayrılmış örnekler dahil değil](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Ayrılmış örnekler iş ortağı merkezi bulut çözümü sağlayıcısı (CSP) programı](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.