---
title: "Azure için faturanızı anlamak | Microsoft Docs"
description: "Okuma ve kullanım ve fatura Azure aboneliğiniz için anlama hakkında bilgi edinin"
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: tonguyen
ms.openlocfilehash: 668b32e99ba9a3bdf8e8f16ac51c35c609444cd9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Microsoft Azure faturanızı anlama
Azure faturasını anlama faturanızı ayrıntılı günlük kullanım dosyasını ve Azure portalında maliyet yönetim raporları ile karşılaştırın.

>[!NOTE]
>Bu makalede, Kurumsal Anlaşma (Kurumsal Sözleşme) müşteriler için geçerli değildir. EA müşteri iseniz [fatura Enterprise Portal'da bulabilirsiniz.](https://ea.azure.com/helpdocs/understandingYourInvoice)  

Bir PDF, fatura ve ayrıntılı günlük kullanım dosya CSV karşıdan bir kopyasını edinmek için bkz: [faturayı ve günlük kullanım verileri faturalama Azure alma](billing-download-azure-invoice-daily-usage-date.md). 

Ayrıntılı hüküm ve fatura ve ayrıntılı günlük kullanım dosya açıklamaları için bkz: [anlamak, Microsoft Azure fatura koşullarınızda](billing-understand-your-invoice.md) ve [anlayın koşulları, Microsoft Azure ile ilgili ayrıntılı kullanım](billing-understand-your-usage.md). 

Maliyet Yönetimi raporları hakkında daha fazla bilgi için bkz: [Azure portal maliyet Yönetim](https://docs.microsoft.com/en-us/azure/billing/billing-getting-started).

## <a name="charges"></a>My fatura ücretlere doğru olup olmadığını nasıl emin?
<div style="padding-top: 56.25%; position: relative; width: 100%;">
<iframe style="position: absolute;top: 0;left: 0;right: 0;bottom: 0;" width="100%" height="100%" src="https://www.youtube.com/embed/3YegFD769Pk" frameborder="0" allowfullscreen></iframe>
</div>

Daha fazla ayrıntı almak istediğiniz faturanızı üzerinde bir ücret ise, birkaç seçeneğiniz vardır.

### <a name="option-1-review-your-invoice-and-compare-the-usage-and-costs-with-the-detailed-usage-csv-file"></a>Seçenek 1: faturanızı gözden geçir ve kullanım ve maliyetleri ayrıntılı kullanımı CSV dosyası ile Karşılaştır

Ayrıntılı kullanım CSV dosyası, fatura döneminin ve günlük kullanımı tarafından ücretlerinizi gösterir. Ayrıntılı kullanım CSV dosyasını almak için bkz: [faturayı ve günlük kullanım verileri faturalama Azure alma](https://docs.microsoft.com/en-us/azure/billing/billing-download-azure-invoice-daily-usage-date).

Kullanım ücretleri ölçer düzeyinde görüntülenir. Aşağıdaki terimler, fatura ve ayrıntılı kullanım dosyasını kullanarak aynı şeyi anlamına gelir. Örneğin, fatura döngüsü fatura üzerinde ayrıntılı kullanımı dosyasında gösterilen fatura döneminde eşdeğerdir.

 | Fatura (PDF) | Ayrıntılı kullanımı (CSV)|
 | --- | --- |
|Fatura döngüsü | Fatura Dönemi |
 |Ad |Ölçüm Kategorisi |
 |Tür |Ölçer alt kategorisi |
 |Kaynak |Ölçüm Adı |
 |Bölge |Ölçüm Bölgesi |
 |Kullanılan |Kullanılan Miktar |
 |Dahil |Dahil Edilen Miktar |
 |Faturalanabilir |Kapasite Aşım Miktarı |

**Kullanım ücretleri** faturanızı bölümünü fatura döneminde tüketilen her ölçüm için toplam değer içeriyor. Örneğin, aşağıdaki ekran görüntüsünde Azure Zamanlayıcı hizmeti için bir kullanım ücret gösterir.

![Fatura kullanım ücretleri](./media/billing-understand-your-bill/1.png)

**Deyimi** ayrıntılı kullanımınızı CSV bölümü aynı ücret gösterir. Her iki *tüketilen* tutar ve *değeri* fatura eşleşmesi.

![CSV kullanım ücretleri](./media/billing-understand-your-bill/2.png)

Bu ücret dökümünü günlük olarak görmek için Git **günlük kullanımı** CSV bölümü. Altında "Zamanlayıcısı" filtre *ölçer kategori* ve hangi günlerde görebilirsiniz ölçer kullanıldı ve ne kadar tüketilen. *Kaynak* ve *kaynak grubu* bilgi karşılaştırma için de listelenir. *Tüketilen* değerleri toplamasını fatura üzerinde gösterilen için.

![CSV günlük kullanım bölümünde](./media/billing-understand-your-bill/3.png)

Gün başına maliyet almak için çarpma *tüketilen* ile tutar *oranı* değeri **deyimi** bölümü.

Fatura hakkında daha fazla bilgi için bkz: [Azure faturanızı anlamak](billing-understand-your-invoice.md).

CSV'ye sütunların her biri hakkında bilgi edinmek için [Azure ayrıntılı kullanımınızı anlamak](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-the-usage-and-costs-in-the-azure-portal"></a>Seçenek 2: faturanızı gözden geçir ve kullanım ve maliyetler Azure portalında ile Karşılaştır

Azure portal Ayrıca, ücretlerinizi doğrulamanıza yardımcı olabilir. Azure portalı maliyet yönetim grafikler, kullanımı ve ücretleri faturanızı üzerinde hızlı bir genel bakış sağlar.

Yukarıda örnek devam etmek için ziyaret [abonelikler sayfası](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), aboneliğinizi seçin ve ardından **analiz maliyetiyle**. Buradan, zaman aralığı belirtin ve kullanım ücret Azure Zamanlayıcı hizmeti için bkz.

![Azure portalında maliyet analiz görünümü](./media/billing-understand-your-bill/4.png)

Günlük maliyet dökümünü görmek için **geçmiş maliyet**, satıra tıklayın.

![Azure portalında maliyet geçmişini görüntüle](./media/billing-understand-your-bill/5.png)

Daha fazla bilgi için bkz: [Azure faturalama ve maliyet yönetimi ile beklenmeyen maliyetleri önlemek](billing-getting-started.md#costs).

## <a name="external"></a>Dış servis ücretleri nasıldır?
Dış hizmetler (olarak da bilinen Azure Marketi siparişler) bağımsız hizmet satıcıları tarafından sağlanan ve ayrı olarak faturalandırılır. Ücretler Azure faturanızı gösterme. Daha fazla bilgi için bkz: [, Azure dış hizmet ücretlerini anlama](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Ödeme nasıl sağlarım?

Ödeme yönteminiz olarak bir kredi kartı veya ATM kartı ayarlamak, ödeme fatura dönemi sona erdikten sonra otomatik olarak 10 gün içinde ücretlendirilir. Kredi kartı deyiminizde satır öğesi diyor **MSFT Azure**.

Varsa, [faturalama tarafından ödeme](billing-how-to-pay-by-invoice.md), ödemenizi konumuna faturanızı alt kısmında listelenen Gönder. Daha fazla yardım için [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>Kredi kartı ile yapılan bir ödeme durumunu nasıl kontrol?

[Destek bileti oluşturma](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ödemenizi durumunun isteyebilir. 

## <a name="tips-for-cost-management"></a>Maliyet yönetimi için ipuçları
- Kullanarak maliyetleri tahmin [fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) ve [toplam sahip olma hesaplayıcı maliyeti](https://aka.ms/azure-tco-calculator)ve alma [ayrıntılı fiyatlandırma bilgileri her hizmet için](https://azure.microsoft.com/en-us/pricing/).
- [Uyarıları faturalama yukarı ayarlamak](billing-set-up-alerts.md).
- [Kullanım ve maliyetler Azure portalında düzenli olarak gözden](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
