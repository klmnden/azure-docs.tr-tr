---
title: Microsoft Müşteri sözleşmesi faturanızı anlama | Microsoft Docs
description: Okuma ve MCA faturanızı anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: jureid
manager: jureid
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: ee6317f61f95b19effd64308b88f53c027582b63
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58883018"
---
# <a name="understand-terms-on-your-microsoft-customer-agreement-invoice"></a>Microsoft Müşteri sözleşmesi faturanızla ilgili koşulları anlama

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

Faturanızı ücretleri ve ödeme yönergeleri bir özetini sağlar. Taşınabilir Belge Biçimi (.pdf) indirme için kullanılabilir [Azure portalında](https://portal.azure.com/) veya e-posta ile gönderilebilir. Daha fazla bilgi için [görüntüleyin ve indirme, Microsoft Azure fatura](billing-download-azure-invoice.md).

<!-- ## When am I billed?

You are invoiced on a monthly basis. You can find out which day of the month you receive invoices by checking *invoice date* under billing profile properties in the [Azure portal](https://portal.azure.com/). Charges that occur between the end of the billing period and the invoice date are included in the next month's invoice, since they are in the next billing period. The billing period start and end dates for each invoice are listed in the invoice PDF above **Billing Summary**. -->

## <a name="invoice-terms-and-descriptions"></a>Fatura hüküm ve açıklamaları

Aşağıdaki bölümlerde her dönem için fatura ve açıklamaları gördüğünüz önemli terimleri listelenmektedir.

### <a name="invoice-summary"></a>Fatura özeti

**Fatura özeti** üst kısmındaki ilk sayfa verilmiştir ve fatura profilinizi ve ödeme yöntemini hakkındaki bilgileri gösterir.

![Fatura Özeti bölümü](./media/billing-understand-your-invoice-mca/invoicesummary.png)

| Sözleşme Dönemi | Açıklama |
| --- | --- |
| Kime Satıldı |Faturalama hesabı özellikleri bulunan varlığınız yasal adresi|
| Fatura adresi |Faturayı almadan fatura profilinin adresi faturalandırma, profil özelliklerini faturalama bulunamadı|
| Faturalama Profili |Faturayı almadan fatura profilinin adı |
| P.O. number |İzleme için size atanan bir isteğe bağlı satınalma siparişi numarası |
| Fatura numarası |İzleme amacıyla kullanılan benzersiz, Microsoft tarafından oluşturulan fatura sayı |
| Fatura tarihi |Fatura, genellikle 5-12 gün faturalama döngüsü sonuna sonra oluşturulan tarih. Fatura profil özelliklerini, fatura tarihi kontrol edebilirsiniz.|
| Ödeme koşulları |Nasıl Microsoft faturanız için ödeme yaparsınız. *30 gün içinde net* Fatura tarihinden itibaren 30 gün içinde ödeme yapacağınız anlamına gelir. |

### <a name="billing-summary"></a>Fatura özeti

**Fatura özeti** önceki fatura döneminde, uygulanan tüm krediler, vergi ve toplam tutarı beri faturalandırma profili karşı ücretleri son gösterir.

![Fatura Özeti bölümü](./media/billing-understand-your-invoice-mca/billingsummary.png)

| Sözleşme Dönemi | Açıklama |
| --- | --- |
| Ücretler|Bu faturalandırma profili son fatura döneminden itibaren Microsoft ücretlendirir toplam sayısı |
| Jenerik |İadelerini aldığınız KREDİLERİ |
| Uygulanan azure kredisine sahip olun | Azure ücretlerine her fatura döneminde otomatik olarak uygulanan azure kredisine sahip olun |
| Ara toplam |Vergi öncesi tutarın |
| Vergi |Fatura profilinizin ülke bağlı olarak ödeme vergi tutarı ve türü. Ardından Vergi ödenecek yoksa faturası vergisi görmezsiniz. |
| Tahmini Toplam tasarruf |Etkili indirimlere kaydettiğiniz tahmini toplam tutar. Varsa, geçerli İndirim oranları ayrıntılarında satın alma satır öğeleri altında fatura bölümünde listelenir. |

### <a name="invoice-sections"></a>Fatura bölümleri

Her fatura bölümü için fatura profilinizi altında ücretleri, uygulanan Azure kredisi miktarı, vergi ve toplam tutarın görürsünüz.

`Total = Charges - Azure Credit + Tax`

### <a name="details-by-invoice-section"></a>Fatura bölümünde ayrıntıları

Ürün siparişi ayrılmış her fatura bölüm için maliyet ayrıntılarını gösterir. Her ürün siparişi içinde maliyet hizmet türü tarafından ayrılmıştır. Azure portalı ve Azure kullanım ve Ücret CSV, ürün ve hizmetleriniz için günlük bulabilirsiniz. Daha fazla bilgi edinmek için [faturanızla ilgili ücretleri anlamak için bir Microsoft Müşteri sözleşmesi](billing-mca-understand-your-bill.md).

Kalan ödenmemiş toplam tutar her hizmet ailesi çıkarılmasıyla hesaplanır *Azure KREDİLERİ* gelen *KREDİLERİ/ücretleri* ve ekleme *vergi*:

<!-- `Total = Charges/Credits - Azure Credit + Tax` -->

![Fatura bölümünde ayrıntıları](./media/billing-understand-your-invoice-mca/invoicesectiondetails.png)

| Sözleşme Dönemi |Açıklama |
| --- | --- |
| Birim fiyatı | Geçerli birim fiyatı olan (para birimi fiyatlandırma) hizmeti kullanım oranı için kullanılır. Bu ürün, hizmet ailesi, ölçüm ve teklif için benzersizdir. |
| Miktar | Satın alınan ya da fatura dönemi boyunca Tüketilen Miktar |
| Ücret/KREDİLERİ | Krediler/para iadesi uygulandıktan sonra ücretleri net tutar |
| Azure Kredisi | Ücret/KREDİLERİ uygulanan Azure kredisi miktarı|
| Vergi oranı | Ülkeye göre Vergi oranı |
| Vergi tutarı | Satın almak için uygulanan vergi tutarı Vergi hızınıza göre |
| Toplam | Kalan ödenmemiş toplam tutar için satın alma |

### <a name="how-to-pay"></a>Ödeme yapma

Fatura sayfanın en faturanızın ödeme yapmak için yönergeler de vardır. Onay, kablo, ödeme yapabilir veya çevrimiçi. Çevrimiçi ödeme yaparsanız varsa kredi/banka kartı veya Azure kredisi kullanabilirsiniz.

### <a name="publisher-information"></a>Yayımcı bilgisi

Üçüncü taraf hizmetleri faturanızda varsa, adını ve her Yayımcı adresini faturanızı alt kısmında listelenir.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Faturadaki fatura profil ücretlerini anlama](billing-mca-understand-your-bill.md)
- [Azure faturanızı ve günlük kullanım verilerinizi edinme](billing-download-azure-invoice-daily-usage-date.md)
- [Kuruluşunuzun Azure fiyatlandırmayı görüntüleyin](billing-ea-pricing.md)
- [Fatura profiliniz için vergi belgeleri görüntüleme](billing-mca-download-tax-document.md)
