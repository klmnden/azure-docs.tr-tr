---
title: Azure'da Microsoft Müşteri sözleşmesi faturanızı anlama
description: Okuma ve azure'da Microsoft Müşteri sözleşmesi faturanızı anlama hakkında bilgi edinin
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: fed658d3f672d6116d7c2b0f3e2e9ad989dd67c6
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490649"
---
# <a name="terms-in-your-microsoft-customer-agreement-invoice"></a>Microsoft, müşteri sözleşmesinin koşullarını fatura

Bu makale, Azure faturalandırma hesabınız için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

Faturanızı ücretleri ve ödeme yönergeleri bir özetini sağlar. Taşınabilir Belge Biçimi (.pdf) indirme için kullanılabilir [Azure portalında](https://portal.azure.com/) veya e-posta ile gönderilebilir. Daha fazla bilgi için [görüntüleyin ve indirme, Microsoft Azure fatura](billing-download-azure-invoice.md).

## <a name="billing-period"></a>Fatura dönemi

Aylık olarak faturalandırılır. Hangi denetleyerek faturalar alıyorsunuz ayın gününü bulabilirsiniz *fatura tarihi* profili özelliklerini faturalama altında [Azure portalında](https://portal.azure.com/). Sonraki fatura döneminde olduğundan, fatura döneminin sonuna ve fatura tarihleri arasında oluşan ücretler sonraki ayın faturasında dahil edilir. Fatura dönemi başlangıç ve bitiş tarihlerini her fatura yukarıdaki PDF faturayı listelenen **fatura özeti**.

## <a name="invoice-terms-and-descriptions"></a>Fatura hüküm ve açıklamaları

Aşağıdaki liste önemli terimler olarak bölümlerde, faturada bakın ve her dönem için açıklamalar sağlar.

### <a name="invoice-summary"></a>Fatura özeti

**Fatura özeti** ilk sayfasının üst kısmındaki ve fatura profilinizi ve ödeme yöntemini hakkındaki bilgileri gösterir.

![Fatura Özeti bölümü](./media/billing-understand-your-invoice-mca/invoicesummary.png)

| Terim | Açıklama |
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

| Terim | Açıklama |
| --- | --- |
| Ücretleri|Bu faturalandırma profili son fatura döneminden itibaren Microsoft ücretlendirir toplam sayısı |
| Jenerik |İadelerini aldığınız KREDİLERİ |
| Uygulanan azure kredisine sahip olun | Azure ücretlerine her fatura döneminde otomatik olarak uygulanan azure kredisine sahip olun |
| Ara toplam |Vergi öncesi tutarın |
| Vergi |Profilinizi faturalandırma ülke/bölge bağlı olarak ödeme vergi tutarı ve türü. Ardından Vergi ödenecek yoksa faturası vergisi görmezsiniz. |
| Tahmini Toplam tasarruf |Etkili indirimlere kaydettiğiniz tahmini toplam tutar. Varsa, geçerli İndirim oranları ayrıntılarında satın alma satır öğeleri altında fatura bölümünde listelenir. |

### <a name="invoice-sections"></a>Fatura bölümleri

Her fatura bölümü için fatura profilinizi altında ücretleri, uygulanan Azure kredisi miktarı, vergi ve toplam tutarın görürsünüz.

`Total = Charges - Azure Credit + Tax`

### <a name="details-by-invoice-section"></a>Fatura bölümünde ayrıntıları

Ürün siparişi ayrılmış her fatura bölüm için maliyet ayrıntılarını gösterir. Her ürün siparişi içinde maliyet hizmet türü tarafından ayrılmıştır. Azure portalı ve Azure kullanım ve Ücret CSV, ürün ve hizmetleriniz için günlük bulabilirsiniz. Daha fazla bilgi edinmek için [faturanızla ilgili ücretleri anlamak için bir Microsoft Müşteri sözleşmesi](billing-mca-understand-your-bill.md).

Kalan ödenmemiş toplam tutar her hizmet ailesi çıkarılmasıyla hesaplanır *Azure KREDİLERİ* gelen *KREDİLERİ/ücretleri* ve ekleme *vergi*:


![Fatura bölümünde ayrıntıları](./media/billing-understand-your-invoice-mca/invoicesectiondetails.png)

| Terim |Açıklama |
| --- | --- |
| Birim fiyatı | Geçerli birim fiyatı olan (para birimi fiyatlandırma) hizmeti kullanım oranı için kullanılır. Bu ürün, hizmet ailesi, ölçüm ve teklif için benzersizdir. |
| Miktar | Satın alınan ya da fatura dönemi boyunca Tüketilen Miktar |
| Ücret/KREDİLERİ | Krediler/para iadesi uygulandıktan sonra ücretleri net tutar |
| Azure Kredisi | Ücret/KREDİLERİ uygulanan Azure kredisi miktarı|
| Vergi oranı | Ülke/bölge bağlı olarak Vergi oranı |
| Vergi tutarı | Satın almak için uygulanan vergi tutarı Vergi hızınıza göre |
| Toplam | Kalan ödenmemiş toplam tutar için satın alma |

### <a name="how-to-pay"></a>Ödeme yapma

Fatura sayfanın en faturanızın ödeme yapmak için yönergeler de vardır. Onay, kablo, ödeme yapabilir veya çevrimiçi. Çevrimiçi ödeme yaparsanız varsa kredi/banka kartı veya Azure kredisi kullanabilirsiniz.

### <a name="publisher-information"></a>Yayımcı bilgisi

Üçüncü taraf hizmetleri faturanızda varsa, adını ve her Yayımcı adresini faturanızı alt kısmında listelenir.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Faturadaki fatura profil ücretlerini anlama](billing-mca-understand-your-bill.md)
- [Azure faturanızı ve günlük kullanım verilerinizi edinme](billing-download-azure-invoice-daily-usage-date.md)
- [Kuruluşunuzun Azure fiyatlandırmayı görüntüleyin](billing-ea-pricing.md)
- [Fatura profiliniz için vergi belgeleri görüntüleme](billing-mca-download-tax-document.md)
