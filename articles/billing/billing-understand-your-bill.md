---
title: Azure faturanızı anlama | Microsoft Docs
description: Okuma ve Azure aboneliğiniz için fatura ve kullanım anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: tonguyen10
manager: jureid
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 4303b79a7ee69d029504bf6ca2359f6e6070e5b8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370189"
---
# <a name="understand-your-microsoft-azure-bill"></a>Microsoft Azure faturanızı anlama
Azure faturanızı anlama faturanızı ayrıntılı günlük kullanım dosyası ve Azure portalında maliyet Yönetimi raporlarını ile karşılaştırın.

Bu makalede, Azure müşterilerine bir Kurumsal Anlaşma (EA müşterileri) için geçerli değildir. Bir EA müşterisinin kullanıyorsanız bkz [bir kurumsal anlaşma kapsamında olan Azure müşterileri için faturanızı anlayın bölümü](billing-understand-your-bill-ea.md).

Bu makalede, Microsoft Müşteri anlaşmasına sahip Azure müşterileri için geçerli değildir. Bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi varsa, bkz: [Microsoft Müşteri sözleşmesi faturanızla ilgili Azure ücretlerini anlama](billing-mca-understand-your-bill.md).

Fatura döngüsü, fiyatlandırma ve kullanım, bkz: faturalandırma Azure bulut çözümü sağlayıcısı (Azure CSP) programında nasıl çalıştığına ilişkin bir açıklama için dahil olmak üzere [Azure CSP fatura genel bakış](/azure/cloud-solution-provider/billing/azure-csp-billing-overview/).

## <a name="charges"></a>Yapılacak bir ücret tahminini gözden geçirin

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Hakkında daha fazla bilgi almak istiyorsanız faturanızla ilgili bir ücret ise, kullanım dosyanın veya Azure portalı ile kullanımı ve maliyetleri karşılaştırabilirsiniz.

### <a name="option-1-compare-usage-and-costs-with-usage-file"></a>1\. seçenek: Kullanımı ve maliyetleri kullanım dosyayla Karşılaştır

Ayrıntılı kullanım CSV dosyası, fatura dönemindeki günlük kullanım ile bir ücret tahminini gösterir. Dosyasını almak için bkz. [Azure faturanızı ve günlük kullanım verilerinizi alın](billing-download-azure-invoice-daily-usage-date.md).

Kullanım ücretlerinizi ölçüm düzeyinde görüntülenir. Aşağıdaki terimler fatura hem de ayrıntılı kullanım dosyası aynı şeyi anlamına gelir. Örneğin, faturadaki fatura döngüsü ayrıntılı kullanım dosyasında gösterilen fatura döneminde aynıdır.

 | Fatura (PDF) | Ayrıntılı kullanım (CSV)|
 | --- | --- |
|Fatura döngüsü | Fatura Dönemi |
 |Ad |Ölçüm Kategorisi |
 |Tür |Ölçüm alt kategorisi |
 |Resource |Ölçüm Adı |
 |Bölge |Ölçüm Bölgesi |
 |Kullanılan |Kullanılan Miktar |
 |Dahil |Dahil Edilen Miktar |
 |Faturalanabilir |Kapasite Aşım Miktarı |

**Kullanım ücretleri** faturanızı bölümünü, fatura dönemi boyunca tüketilen her bir ölçüm için toplam değerine sahiptir. Örneğin, aşağıdaki ekran görüntüsünde Azure Zamanlayıcı hizmeti için bir kullanım ücreti gösterir.

![Fatura kullanım ücretleri](./media/billing-understand-your-bill/1.png)

**Deyimi** ayrıntılı kullanım CSV bölümü aynı ücretlendirme gösterir. Her iki *tüketilen* tutar ve *değer* fatura eşleşmesi.

![CSV kullanım ücretleri](./media/billing-understand-your-bill/2.png)

Bu Ücret günlük dökümünü görmek için Git **günlük kullanım** CSV bölümü. Filtre *Zamanlayıcı* altında *ölçüm kategorisi*. Hangi günlerin gördüğünüz ölçüm kullanıldı ve ne kadar tüketildiğinin. *Kaynak* ve *kaynak grubu* bilgi karşılaştırma için ayrıca listelenir. *Tüketilen* değerleri ekleme fatura üzerinde gösterilen için.

![Günlük kullanım bölümünde CSV](./media/billing-understand-your-bill/3.png)

Gün başına maliyet almak için çarpma *tüketilen* ile tutarları *oranı* değerini **deyimi** bölümü.

Daha fazla bilgi için bkz:

- [Azure faturanızı anlama](billing-understand-your-invoice.md)
- [Azure ayrıntılı kullanımınızı anlayın](billing-understand-your-invoice.md)

### <a name="option-2-compare-the-usage-and-costs-in-the-azure-portal"></a>2\. seçenek: Kullanımı ve maliyetleri Azure portalında karşılaştırın

Azure portalı Ayrıca, yapılacak bir ücret doğrulamanıza yardımcı olabilir. Faturalanan kullanımı ve ücretleri hızlı bir genel bakış edinmek için maliyet Yönetimi grafiklerini görüntüleyin.

1. Azure portalında Git [abonelikleri](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Aboneliğinizi seçin > **maliyet analizi**.
1. Filtre ölçütü **Timespan**.
1. Önceki örneği devam etmek için Azure Zamanlayıcı hizmeti için bir kullanım ücreti görürsünüz.

   ![Azure portalında maliyet analizi görüntüle](./media/billing-understand-your-bill/4.png)

1. Günlük maliyet dökümünü görmek için satır seçin.

   ![Azure portalında maliyet geçmişini görüntüle](./media/billing-understand-your-bill/5.png)

Daha fazla bilgi için bkz. [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md#costs).

## <a name="external"></a>Dış hizmetler ayrı olarak faturalandırılır

Dış hizmetlere ya da Market ücretlerini, üçüncü taraf yazılım satıcıları tarafından oluşturulan kaynakları içindir. Bu kaynakları Azure Market'ten kullanılabilir. Örneğin, bir Barracuda güvenlik duvarı üçüncü taraf tarafından sunulan bir Azure Market kaynağıdır. Tüm ücretler için güvenlik duvarı ve kendi ilgili ölçümleri dış hizmet ücretlerini görülür.

Dış hizmet ücretlerini ayrı olarak faturalandırılır. Ücretler Azure faturanızı gösterme. Daha fazla bilgi için bkz. [Azure, dış hizmet ücretlerini anlama](billing-understand-your-azure-marketplace-charges.md).

## <a name="resources-billed-by-usage-meters"></a>Kullanım ölçümleri ile faturalandırılır kaynakları

Kaynak maliyeti fatura tabanlı doğrudan azure değil. Bir kaynak için bir veya daha fazla ölçümleri kullanılarak hesaplanır. Ölçümleri ömrü boyunca kaynak kullanımını izlemek için kullanılır. Bu ölçümleri daha sonra faturanıza yansıyan tutarı hesaplamak için kullanılır.

Örneğin, bir sanal makine gibi tek bir Azure kaynak oluşturduğunuzda oluşturulan bir veya daha fazla ölçüm örneği yok. Ölçümleri, zaman içinde kaynak kullanımını izlemek için kullanılır. Her bir ölçüm, faturanıza yansıyan tutarı hesaplamak için kullanılan Azure tarafından kullanım kayıtlarını yayar.

Örneğin, Azure üzerinde oluşturulan bir tek sanal makine (VM) kullanımını izlemek için oluşturulan aşağıdaki ölçümler sahip olabilir:

- İşlem Saatleri
- IP adresi saatleri
- Gelen Veri Aktarımı
- Giden Veri Aktarımı
- Standart yönetilen Disk
- Standart yönetilen Disk işlemleri
- Standart GÇ-Disk
- Standart GÇ-blok blobu okuma
- Standart GÇ-blok blobu yazma
- Standart GÇ-blok blobu silme

VM oluşturulduğunda, her ölçer kullanım kayıtlarının yayma başlar. Bu kullanım ve ölçümün fiyat sistem ölçümünü Azure'da izlenir.

## <a name="payment"></a>Faturanızın ödeme

Bir kredi kartı veya banka kartı ödeme yönteminiz olarak ayarlarsanız, ödeme faturalandırma dönemi sona erdikten sonra otomatik olarak 10 gün içinde ücretlendirilir. Kredi kartı Ekstrenizi satır öğesi söyleyin **MSFT Azure**.

Ücretlendirilir kredi veya banka kartı değiştirmek için bkz [ekleme, güncelleştirme veya Azure için bir kredi kartı veya banka kartı kaldırma](billing-how-to-change-credit-card.md).

Varsa, [faturayla ödeme](billing-how-to-pay-by-invoice.md), ödemenin konumuna faturanızı alt kısmında listelenen Gönder.

Ödemenizin, durumunu denetlemek için [bir destek bileti oluşturma](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


## <a name="tips-for-cost-management"></a>Maliyet yönetimi için ipuçları

- Kullanarak maliyetleri tahmin etme:
  - [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)
  - [Sahipliği hesaplayıcı toplam maliyeti](https://aka.ms/azure-tco-calculator)
  - [Ayrıntılı fiyatlandırma bilgileri her hizmet için](https://azure.microsoft.com/pricing/)
- [Kullanımı ve maliyetleri Azure portalında düzenli olarak gözden geçirme](billing-getting-started.md#costs).

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="learn-more"></a>Daha fazla bilgi edinin

- [Azure fatura ve günlük kullanım verilerini al](billing-download-azure-invoice-daily-usage-date.md)
- [Microsoft Azure faturanızla ilgili koşulları anlama](billing-understand-your-invoice.md)
- [Şirket koşulları anlama, Microsoft Azure ayrıntılı kullanım](billing-understand-your-usage.md)
- [Azure portal maliyet Yönetimi](https://docs.microsoft.com/azure/billing/billing-getting-started)
- [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md#costs)
