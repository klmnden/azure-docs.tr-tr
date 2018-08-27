---
title: Azure için faturanızı anlayın | Microsoft Docs
description: Okuma ve Azure aboneliğiniz için fatura ve kullanım anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: tonguyen10
manager: tonguyen
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2018
ms.author: tonguyen
ms.openlocfilehash: c782cadadb0250e6c3ca4912dbf8f81e19cb88c5
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42919063"
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Microsoft Azure faturanızı anlama
Azure faturanızı anlama faturanızı ayrıntılı günlük kullanım dosyası ve Azure portalında maliyet Yönetimi raporlarını ile karşılaştırın.

>[!NOTE]
>Bu makalede, Kurumsal Anlaşma (EA) müşterileri için geçerli değildir. Bir EA müşterisinin kullanıyorsanız [fatura belgeleri Enterprise Portal'da bulabilirsiniz.](https://ea.azure.com/helpdocs/understandingYourInvoice)  

PDF, fatura ve ayrıntılı günlük kullanım dosyası CSV indirme işleminizi bir kopyasını edinmek için bkz. [Azure faturanızı ve günlük kullanım verilerinizi alın](billing-download-azure-invoice-daily-usage-date.md). 

Ayrıntılı hüküm ve fatura ve günlük kullanım dosyası ayrıntılı açıklamaları için bkz. [Microsoft Azure faturanızla ilgili koşulları anlama](billing-understand-your-invoice.md) ve [anlayın koşulları, Microsoft Azure üzerinde ayrıntılı kullanım](billing-understand-your-usage.md). 

Maliyet Yönetimi raporları hakkında daha fazla bilgi için bkz: [Azure portalında maliyet Yönetimi](https://docs.microsoft.com/azure/billing/billing-getting-started).

> [!div class="nextstepaction"]
> [Azure faturalama belgeleri geliştirilmesine yardımcı olun](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="charges"></a>My fatura ücretleri doğru olduğunu nasıl emin olabilirim?

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Daha fazla ayrıntı almak istediğiniz faturanızla ilgili bir ücret ise, birkaç seçenek vardır.

### <a name="option-1-review-your-invoice-and-compare-the-usage-and-costs-with-the-detailed-usage-csv-file"></a>1. seçenek: faturanızı gözden geçirin ve kullanımı ve maliyetleri ayrıntılı kullanım CSV dosyası ile Karşılaştır

Ayrıntılı kullanım CSV dosyası, fatura dönemindeki günlük kullanım ile bir ücret tahminini gösterir. Ayrıntılı kullanım CSV dosyanız almak için bkz. [Azure faturanızı ve günlük kullanım verilerinizi alın](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date).

Kullanım ücretlerinizi ölçüm düzeyinde görüntülenir. Aşağıdaki terimler fatura hem de ayrıntılı kullanım dosyası aynı şeyi anlamına gelir. Örneğin, faturadaki fatura dönemi için ayrıntılı kullanım dosyasında gösterilen fatura döneminde eşdeğerdir.

 | Fatura (PDF) | Ayrıntılı kullanım (CSV)|
 | --- | --- |
|Fatura döngüsü | Fatura Dönemi |
 |Ad |Ölçüm Kategorisi |
 |Tür |Ölçüm alt kategorisi |
 |Kaynak |Ölçüm Adı |
 |Bölge |Ölçüm Bölgesi |
 |Kullanılan |Kullanılan Miktar |
 |Dahil |Dahil Edilen Miktar |
 |Faturalanabilir |Kapasite Aşım Miktarı |

**Kullanım ücretleri** faturanızı bölümünü, fatura dönemi boyunca tüketilen her bir ölçüm için toplam değerine sahiptir. Örneğin, aşağıdaki ekran görüntüsünde Azure Zamanlayıcı hizmeti için bir kullanım ücreti gösterir.

![Fatura kullanım ücretleri](./media/billing-understand-your-bill/1.png)

**Deyimi** ayrıntılı kullanım CSV bölümü aynı ücretlendirme gösterir. Her iki *tüketilen* tutar ve *değer* fatura eşleşmesi.

![CSV kullanım ücretleri](./media/billing-understand-your-bill/2.png)

Günlük olarak bu ücret dökümünü görmek için Git **günlük kullanım** CSV bölümü. Filtrelemek için "Zamanlayıcı" altında *ölçüm kategorisi* ve hangi günlerinde gördüğünüz ölçüm kullanıldı ve ne kadar tüketildiğinin. *Kaynak* ve *kaynak grubu* bilgi karşılaştırma için ayrıca listelenir. *Tüketilen* değerleri ekleme fatura üzerinde gösterilen için.

![Günlük kullanım bölümünde CSV](./media/billing-understand-your-bill/3.png)

Gün başına maliyet almak için çarpma *tüketilen* ile tutarları *oranı* değerini **deyimi** bölümü.

Fatura hakkında daha fazla bilgi için bkz: [Azure faturanızı anlama](billing-understand-your-invoice.md).

Her CSV sütunların hakkında bilgi edinmek için [Azure ayrıntılı kullanımınızı anlayın](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-the-usage-and-costs-in-the-azure-portal"></a>2. seçenek: faturanızı gözden geçirin ve kullanımı ve maliyetleri Azure portalında ile karşılaştırın

Azure portalı Ayrıca, yapılacak bir ücret doğrulamanıza yardımcı olabilir. Azure portalı, kullanım ve ücret, faturada hızlı bir genel bakış için maliyet Yönetimi grafikler sağlar.

Yukarıdaki örnekte devam etmek için ziyaret [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), aboneliğinizi seçin ve ardından **maliyet analizi**. Burada, zaman aralığı belirtin ve Azure Zamanlayıcı hizmeti için kullanım ücreti bakın.

![Azure portalında maliyet analizi görüntüle](./media/billing-understand-your-bill/4.png)

Günlük maliyet dökümdeki görmek için **geçmişi maliyet**, satıra tıklayın.

![Azure portalında maliyet geçmişini görüntüle](./media/billing-understand-your-bill/5.png)

Daha fazla bilgi için bkz. [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md#costs).

## <a name="external"></a>Dış hizmet ücretlerini hakkında neler diyeceksiniz?
Dış hizmetlere (olarak da bilinen Azure Market siparişlerini) bağımsız hizmeti satıcısı tarafından sağlanan ve ayrı olarak faturalandırılır. Ücretler Azure faturanızı gösterme. Daha fazla bilgi için bkz. [Azure, dış hizmet ücretlerini anlama](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Ödeme nasıl yapılsın mı?

Bir kredi kartı veya banka kartı ödeme yönteminiz olarak ayarlarsanız, ödeme faturalandırma dönemi sona erdikten sonra otomatik olarak 10 gün içinde ücretlendirilir. Kredi kartı Ekstrenizi satır öğesi söyleyin **MSFT Azure**.

Varsa, [fatura ile ödeme](billing-how-to-pay-by-invoice.md), ödemenin konumuna faturanızı alt kısmında listelenen Gönder. Daha fazla yardım için [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>Kredi kartı ile yapılan bir ödeme durumunu nasıl kontrol edebilirim?

[Bir destek bileti oluşturma](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ödemenizin durumu için isteyebilir. 

## <a name="are-there-different-azure-customer-types-how-do-i-know-what-customer-type-i-am"></a>Farklı Azure müşteri türleri var mı? Ben hangi müşteri türü olduğunu nasıl öğrenebilirim?
Azure müşterileri farklı türde vardır. Fiyatlandırma ve fatura daha iyi anlamak için aşağıdaki müşteri türü açıklamaları bakın.

- **Kurumsal**: Kurumsal müşteriler, Azure üzerinde anlaşılan parasal taahhütler olun ve Azure kaynakları için özel fiyatlandırma için erişim ile Kurumsal Sözleşme açtığınızdan.
- **Web Direct**: doğrudan Web müşterileri Azure ile özel bir sözleşme işaretli değil. Bu müşteriler, azure.com Azure'a kaydolup ve tüm Azure kaynakları için genel kullanıma yönelik fiyatlar alabilirsiniz.
- **Bulut hizmeti sağlayıcısı**: Bulut hizmeti sağlayıcıları şirketlerdir genellikle son müşteri tarafından Azure üzerine çözümler oluşturmak için işe.

## <a name="why-dont-i-see-the-cost-the-resource-i-have-created-in-my-bill"></a>Faturamı içinde oluşturulmuş kaynak maliyeti neden göremiyorum?
Olmayan bill maliyet kaynağını doğrudan temel azure yapar. Faturalandırma, bir kaynağın kullanım ömrü boyunca izlemek için kullanılan bir veya daha fazla ölçümleri devre dışı tabanlı gerçekleştirilir. Bu ölçümleri daha sonra faturanıza yansıyan tutarı hesaplamak için kullanılır. Aşağıda, Azure ölçümü hakkında daha fazla bilgi bkz.

## <a name="how-does-azure-charge-metering-work"></a>Nasıl Azure ölçüm iş Ücretli midir?
Bir sanal makine gibi tek bir Azure kaynak hızlanması, de oluşturulan bir veya birden çok ölçüm örnekleri olur. Bu ölçüm, zaman içinde kaynak kullanımını izlemek için kullanılır. Her bir ölçüm daha sonra Azure tarafından sistem ölçümünü bizim maliyetine faturanıza yansıyan tutarı hesaplamak için kullanılan kullanım kayıtlarını yayar. 

Örneğin, Azure'da oluşturulan tek bir sanal makine kullanımını izlemek için oluşturulan aşağıdaki ölçümler sahip olabilir:

- İşlem Saatleri
- IP adresi saatleri
- Veri Aktarımı Girişi
- Giden Veri Aktarımı
- Standart yönetilen Disk
- Standart yönetilen Disk işlemleri
- Standart GÇ-Disk
- Standart GÇ-blok blobu okuma
- Standart GÇ-blok blobu yazma
- Standart GÇ-blok blobu silme

VM oluşturulduktan sonra her biri yukarıdaki ölçütlerden kullanım kayıtlarını yayma başlar. Bu kullanım sonra Azure'nın Ölçüm sistemi ölçümün fiyat yanı sıra bir müşterinin ne kadar ücret belirlemek için kullanılır.

> [!Note]
> Yukarıdaki örnek ölçümleri yalnızca oluşturulan oluşturulan bir VM'yi ölçümleri bir alt kümesi olabilir.

## <a name="what-is-the-difference-between-azure-1st-party-charges-and-azure-marketplace-charges"></a>Azure arasındaki fark nedir 1. taraf ücretleri ve Azure Market kullanım ücretlerinin?
Azure 1. taraf ücretleri, doğrudan geliştirilen ve Azure tarafından sunulan kaynakları. Azure Marketi'nde Azure Marketi aracılığıyla kullanılabilir olan üçüncü taraf yazılım satıcıları tarafından oluşturulan kaynakları ücretlerdir. Örneğin, bir Barracuda Güvenlik Duvarı bir üçüncü taraf tarafından sunulan bir Azure Market kaynağıdır. Tüm ücretler için güvenlik duvarı ve kendi ilgili ölçümleri Market ücretlerini görünür. 

## <a name="tips-for-cost-management"></a>Maliyet yönetimi için ipuçları
- Kullanarak maliyetleri tahmin etme [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) ve [toplam sahiplik hesaplayıcı maliyeti](https://aka.ms/azure-tco-calculator)ve [ayrıntılı fiyatlandırma bilgileri her hizmet için](https://azure.microsoft.com/pricing/).
- [Fatura uyarılarını ayarlama](billing-set-up-alerts.md).
- [Kullanımı ve maliyetleri Azure portalında düzenli olarak gözden geçirme](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
