---
title: Azure için faturanızı anlayın | Microsoft Docs
description: Okuma ve Azure aboneliğiniz için fatura ve kullanım anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: tonguyen10
manager: alherz
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: cwatson
ms.openlocfilehash: b9b1496c71e61fce815d225310c8beb57e8f9b19
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584579"
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Microsoft Azure faturanızı anlama
Azure faturanızı anlama faturanızı ayrıntılı günlük kullanım dosyası ve Azure portalında maliyet Yönetimi raporlarını ile karşılaştırın.

Bu makalede, Azure müşterilerine bir Kurumsal Anlaşma (EA müşterileri) için geçerli değildir. Bir EA müşterisinin kullanıyorsanız bkz [bir kurumsal anlaşma kapsamında olan Azure müşterileri için faturanızı anlayın bölümü](billing-understand-your-bill-ea.md).  

## <a name="charges"></a>Yapılacak bir ücret tahminini gözden geçirin

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Hakkında daha fazla bilgi almak istiyorsanız faturanızla ilgili bir ücret ise, kullanım dosyanın veya Azure portalı ile kullanımı ve maliyetleri karşılaştırabilirsiniz.

### <a name="option-1-compare-usage-and-costs-with-usage-file"></a>1. seçenek: kullanımı ve maliyetleri kullanım dosyayla Karşılaştır

Ayrıntılı kullanım CSV dosyası, fatura dönemindeki günlük kullanım ile bir ücret tahminini gösterir. Dosyasını almak için bkz. [Azure faturanızı ve günlük kullanım verilerinizi alın](billing-download-azure-invoice-daily-usage-date.md).

Kullanım ücretlerinizi ölçüm düzeyinde görüntülenir. Aşağıdaki terimler fatura hem de ayrıntılı kullanım dosyası aynı şeyi anlamına gelir. Örneğin, faturadaki fatura döngüsü ayrıntılı kullanım dosyasında gösterilen fatura döneminde aynıdır.

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

Bu Ücret günlük dökümünü görmek için Git **günlük kullanım** CSV bölümü. "Zamanlayıcı" için filtre altında *ölçüm kategorisi*. Hangi günlerin gördüğünüz ölçüm kullanıldı ve ne kadar tüketildiğinin. *Kaynak* ve *kaynak grubu* bilgi karşılaştırma için ayrıca listelenir. *Tüketilen* değerleri ekleme fatura üzerinde gösterilen için.

![Günlük kullanım bölümünde CSV](./media/billing-understand-your-bill/3.png)

Gün başına maliyet almak için çarpma *tüketilen* ile tutarları *oranı* değerini **deyimi** bölümü.

Daha fazla bilgi için bkz:

- [Azure faturanızı anlama](billing-understand-your-invoice.md)
- [Azure ayrıntılı kullanımınızı anlayın](billing-understand-your-invoice.md)

### <a name="option-2-compare-the-usage-and-costs-with-the-azure-portal"></a>Seçenek 2: kullanımı ve maliyetleri Azure portalı ile Karşılaştır

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

Kaynak maliyeti fatura tabanlı doğrudan azure değil. Bir kaynak için bir veya daha fazla ölçümleri kullanılarak hesaplanır. Bu ölçüm, bir kaynağın kullanım ömrü boyunca izlemek için kullanılır. Bu ölçümleri daha sonra faturanıza yansıyan tutarı hesaplamak için kullanılır.

Örneğin, bir sanal makine gibi tek bir Azure kaynak oluşturduğunuzda oluşturulan bir veya daha fazla ölçüm örneği yok. Bu ölçüm, zaman içinde kaynak kullanımını izlemek için kullanılır. Her bir ölçüm, faturanıza yansıyan tutarı hesaplamak için kullanılan Azure tarafından kullanım kayıtlarını yayar.

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

VM oluşturulduğunda, her biri, bu ölçümleri kullanım kayıtlarını yayma başlar. Bu kullanım ve ölçümün fiyat sistem ölçümünü Azure'da izlenir.

## <a name="payment"></a>Faturanızın ödeme

Bir kredi kartı veya banka kartı ödeme yönteminiz olarak ayarlarsanız, ödeme faturalandırma dönemi sona erdikten sonra otomatik olarak 10 gün içinde ücretlendirilir. Kredi kartı Ekstrenizi satır öğesi söyleyin **MSFT Azure**.

Ücretlendirilir kredi veya banka kartı değiştirmek için bkz [ekleme, güncelleştirme veya Azure için bir kredi kartı veya banka kartı kaldırma](billing-how-to-change-credit-card.md).

Varsa, [faturayla ödeme](billing-how-to-pay-by-invoice.md), ödemenin konumuna faturanızı alt kısmında listelenen Gönder.

Üzerinde ödemenin durumunu denetlemek için [bir destek bileti oluşturma](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


## <a name="tips-for-cost-management"></a>Maliyet yönetimi için ipuçları

- Kullanarak maliyetleri tahmin etme:
  - [Azure fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)
  - [Sahipliği hesaplayıcı toplam maliyeti](https://aka.ms/azure-tco-calculator)
  - [Ayrıntılı fiyatlandırma bilgileri her hizmet için](https://azure.microsoft.com/pricing/)
- [Kullanımı ve maliyetleri Azure portalında düzenli olarak gözden geçirme](billing-getting-started.md#costs).

## <a name="learn-more"></a>Daha fazla bilgi edinin

- [Azure fatura ve günlük kullanım verilerini al](billing-download-azure-invoice-daily-usage-date.md)
- [Microsoft Azure faturanızla ilgili koşulları anlama](billing-understand-your-invoice.md)
- [Şirket koşulları anlama, Microsoft Azure ayrıntılı kullanım](billing-understand-your-usage.md)
- [Azure portal maliyet Yönetimi](https://docs.microsoft.com/azure/billing/billing-getting-started)
- [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing-getting-started.md#costs)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
