---
ms.topic: include
ms.openlocfilehash: b41b93db63038645257bc348d5a027fbc4f7fdc9
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="prepay-for-virtual-machines-with-reserved-vm-instances"></a>Ayrılmış VM örnekleri ile sanal makineler için ön ödeme

Sanal makineler için ön ödeme ve ayrılmış sanal makine örnekleriyle paradan tasarruf. Daha fazla bilgi için bkz: [ayrılmış sanal makine örnekleri teklifi](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Ayrılmış sanal makine örnekleri satın alabileceğiniz [Azure portal](https://portal.azure.com). Ayrılmış bir sanal makine örnek satın almak için:
-   En az bir kuruluş veya Kullandıkça Öde aboneliğine sahip rolünde olması gerekir.
-   Ayırma satın alma işlemleri Kurumsal abonelikler için etkinleştirilmesi gerekir [EA portal](https://ea.azure.com).

## <a name="buy-a-reserved-virtual-machine-instance"></a>Ayrılmış sanal makine örneğini satın alın
1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Seçin **Ekle** yeni bir ayırma satın alın.
4. Gerekli alanları doldurun. Çalışan VM ayırma indirim almak için seçtiğiniz özniteliklerine nitelemek eşleşen örnekler. İndirim almak üzerindeki VM örneklerinize gerçek sayısını kapsam ve seçili miktar bağlıdır.

    | Alan      | Açıklama|
    |:------------|:--------------|
    |Ad        |Bu ayırma adı.| 
    |Abonelik|Ayırma için ödeme yapmak üzere kullanılan aboneliği. Abonelik için ödeme yöntemini ayırma ön maliyetlerini ücretlendirilir. Abonelik türü bir Kurumsal Anlaşma olmalıdır (teklif numarası: MS-AZR - 0017P) veya Kullandıkça Öde (teklif numarası: MS-AZR - 0003P). Bir kurumsal aboneliği için ücretleri kayıt ait parasal taahhüt bakiyenin dışında kesinti veya fazla kullanım ücretlendirilir. Kullandıkça Öde abonelik için aboneliğe kredi kartı veya fatura ödeme yöntemine ücret faturalandırılır.|    
    |Kapsam       |Ayırma'nın kapsamı, bir abonelik ya da birden çok abonelik (Paylaşılan kapsam) ele. Seçerseniz: <ul><li>Tek bir abonelik - ayırma indirim VM'ler için bu abonelikte uygulanır. </li><li>Paylaşılan - ayırma indirim hiç abonelik faturalama içeriğiniz içinde çalışan sanal makineleri uygulanır. Kurumsal müşteriler için Paylaşılan kapsam kayıt ve kayıt içindeki tüm abonelikleri (dışında geliştirme ve test abonelikleri) içerir. Kullandıkça Öde müşteriler için Paylaşılan kapsam Hesap Yöneticisi tarafından oluşturulan tüm Kullandıkça Öde abonelikleri içindir.</li></ul>|
    |Konum    |Ayırma tarafından kapsanan Azure bölgesi.|    
    |VM Boyutu     |VM örnekleri boyutu.|
    |Sözleşme Dönemi        |Bir yıl veya üç yıl.|
    |Miktar    |İçinde ayırma satın örneği sayısı. Fatura indirim almak VM örnekleri çalışan sayısı miktarıdır. Doğu ABD 10 Standard_D2 sanal makineleri çalıştırıyorsanız, örneğin, miktar avantajı tüm çalışan makineler için en üst düzeye çıkarmak için 10 olarak belirtirsiniz. |
5. Seçtiğinizde ayırma maliyetini görüntüleyebilirsiniz **hesapla maliyet**.

    ![Ayırma satın alma göndermeden önce ekran görüntüsü](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Seçin **satın alma**.
7. Seçin **bu ayırma görüntülemek** satın alma işleminiz durumunu görmek için.

    ![Ayırma satın alma göndermeden önce ekran görüntüsü](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Sonraki adımlar 
Ayırma indirim ayırma kapsamı ve öznitelikleri eşleşen sanal makinelerde çalışan sayısı için otomatik olarak uygulanır. Kapsam ayırma güncelleştirebilirsiniz [Azure portal](https://portal.azure.com), PowerShell'i, CLI veya API'si aracılığıyla. 

Bir ayırma yönetme konusunda bilgi almak için bkz: [yönetmek Azure ayrılmış sanal makine örnekleri](../articles/billing/billing-manage-reserved-vm-instance.md).

Ayrılmış sanal makine örnekleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın.

- [Ayrılmış sanal makine örnekleri sahip sanal makinelerde paradan tasarruf](../articles/billing/billing-save-compute-costs-reservations.md)
- [Ayrılmış sanal makine örnek indirim nasıl uygulandığını anlama](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak](../articles/billing/billing-understand-reserved-instance-usage.md)
- [İşletme kaydı için ayrılmış örnek kullanım anlama](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayrılmış örnekler dahil değil](../articles/billing/billing-reserved-instance-windows-software-costs.md)