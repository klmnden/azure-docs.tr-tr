---
title: Ayrıntılı kullanım ve Ücret anlama | Microsoft Docs
description: Okuma ve ayrıntılı kullanımı ve ücretleri anlamak hakkında bilgi edinin
author: bandersmsft
manager: micflan
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/24/2019
ms.author: banders
ms.openlocfilehash: 9ff9b6b5313026d2102b98659183fa97c6a5ef84
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64683997"
---
# <a name="understand-the-terms-in-your-azure-usage-and-charges-file"></a>Azure kullanım ve Ücret dosyanızda koşulları anlama

Ayrıntılı kullanım ve Ücret dosya belirtilen dönem için üzerinde anlaşılan oranları, satın alma işlemleri (örneğin, rezervasyonları, Market ücretleri) ve para iadesi göre günlük derecelendirilmiş kullanım içerir.
Ücretler, KREDİLERİ, vergileri veya diğer ücretleri veya indirimleri içermez.
Aşağıdaki tablo, her hesap türü için hangi ücretleri dahildir kapsar.

Hesap türü | Azure kullanımı | Market kullanım | Satın alma işlemleri | Mahsup işlemleri
--- | --- | --- | --- | ---
Kurumsal Anlaşma (EA) | Evet | Evet | Evet | Hayır
Microsoft Müşteri Sözleşmesi (MCA) | Evet | Evet | Evet | Evet
Kullandıkça Öde (PAYG) | Evet | Hayır | Hayır | Hayır

Market siparişlerini (Dış hizmetler olarak da bilinir) hakkında daha fazla bilgi için bkz: [Azure, dış hizmet ücretlerini anlama](billing-understand-your-azure-marketplace-charges.md).

Bkz: [Azure faturanızı ve günlük kullanım verilerinizi edinme](billing-download-azure-invoice-daily-usage-date.md) yükleme yönergeleri.
Kullanımı ve ücretleri dosyası, bir elektronik tablo uygulamasında açabileceğiniz bir virgülle ayrılmış değerler (.csv) dosya biçiminde kullanılabilir.

## <a name="list-of-terms-and-descriptions"></a>Hüküm ve açıklamalarının listesi

Aşağıdaki tabloda, Azure kullanımı ve ücretleri dosyanın en son sürümünde kullanılan önemli koşullar açıklanmaktadır.
Liste, Kullandıkça Öde (PAYG), Kurumsal Anlaşma (EA) ve Microsoft Müşteri Sözleşmesi (MCA) hesapları kapsar.

Terim | Hesap türü | Açıklama
--- | --- | ---
accountName | EA | Kayıt hesabı görünen adı.
AccountOwnerId | EA | Kayıt hesabı için benzersiz tanımlayıcı.
Additionalınfo | Tümü | Hizmete özgü meta veriler. Örneğin, bir sanal makine için görüntü türü.
BillingAccountId | EA, MCA | Kök hesabı faturalama için benzersiz tanımlayıcı.
billingAccountName | EA, MCA | Fatura hesabı adı.
billingCurrency | EA, MCA | Fatura hesabı ile ilişkili bir para birimi.
BillingPeriod | EA | Fatura dönemi gider.
billingPeriodEndDate | EA, MCA | Fatura dönemi bitiş tarihi.
billingPeriodStartDate | EA, MCA | Fatura dönemi başlangıç tarihi.
BillingProfileId | EA, MCA | MCA profili faturalama ve EA kayıt benzersiz tanımlayıcısı.
BillingProfileName | EA, MCA | MCA profili faturalama ve EA kayıt adı.
chargeType | EA, MCA | Ücretlendirme, kullanım temsil edip etmediğini belirtir (**kullanım**), bir satın alma (**satın**), ya da para iadesi (**para iadesi**).
ConsumedQuantity | PAYG | Miktar bakın.
ConsumedService | Tümü | Ücretsiz hizmetin adını ilişkilidir.
Maliyet | EA | See CostInBillingCurrency.
CostCenter | EA, MCA | Maliyet merkezi maliyetlerini (yalnızca açık faturalandırma dönemleri MCA hesapları için de kullanılabilir) izlemek için abonelik için tanımlanmış.
CostInBillingCurrency | MCA | Fatura para kredisi veya vergi önce ücret maliyeti.
CostInPricingCurrency | MCA | Ücretsiz fiyatlandırma para kredisi veya vergi önce maliyeti.
Para birimi | PAYG | BillingCurrency bakın.
Tarih | EA, MCA | Kullanım veya satın alma tarihi gider.
ExchangeRateDate | MCA | Tarih döviz kuru kuruldu.
ExchangeRatePricingToBilling | MCA | Döviz kuru maliyeti fiyatlandırma para birimi için fatura para birimi dönüştürmek için kullanılır.
Sıklık | EA, MCA | Bir ücret yinelemek için beklenen durum olup olmadığını gösterir. Ya da gerçekleşebilir kez ücretleri (**OneTime**), aylık veya yıllık olarak yineleme (**yinelenen**), veya kullanımıyla ilgili temel (**UsageBased**).
includedQuantity | PAYG | Geçerli fatura döneminize ücretsiz dahil olan ölçüm miktarı.
InstanceId | PAGY | ResourceId bakın.
Fatura kodu | EA, MCA | PDF faturada listelenen belgenin benzersiz kimliği.
invoiceSection | MCA | InvoiceSectionName bakın.
invoiceSectionId | EA, MCA | EA departman veya MCA fatura bölümü için benzersiz tanımlayıcı.
invoiceSectionName | EA, MCA | EA departman veya MCA fatura bölümün adı.
isAzureCreditEligible | EA, MCA | Ücretsiz Azure KREDİLERİ kullanımı için ödenecek uygun olup olmadığını gösterir (değerleri: TRUE, False).
Location | EA, MCA | Kaynağın çalıştığı veri merkezi konumu.
MeterCategory | Tümü | Ölçüm için sınıflandırma kategorisi adı. Örneğin, *bulut Hizmetleri* ve *ağ*.
MeterId | Tümü | Ölçüm için benzersiz tanımlayıcı.
MeterName | Tümü | Ölçüm adı.
MeterRegion | Tümü | Konumuna göre ücretlendirilen hizmetler için veri merkezi bölgesi adı. Konum bakın.
MeterSubCategory | Tümü | Ölçüm subclassification kategorisi adı.
OfferId | EA, MCA | Satın alınan teklif adı.
PartNumber | EA | Özel ölçüm fiyatlandırma almak için kullanılan tanımlayıcı.
PlanName | EA | Market planı adı.
previousInvoiceId | MCA | Bu satır öğesi bir para iadesi ise özgün Fatura başvuru.
pricingCurrency | MCA | Üzerinde anlaşılan fiyatlarına göre derecelendirme temel kullanılan para birimi.
Product | MCA | ProductName bakın.
ProductID | EA, MCA | Ürün için benzersiz tanımlayıcı.
ProductName | EA | Ürün adı.
ProductOrderId | EA, MCA | Ürün siparişi için benzersiz tanımlayıcı.
productOrderName | EA, MCA | Ürün siparişi için benzersiz bir ad.
PublisherName | EA, MCA | Market Hizmetleri yayımcısı.
publisherType | EA, MCA | Yayımcı türü (değerleri: firstParty, thirdPartyReseller, thirdPartyAgency).
Miktar | EA, MCA | Satın alınan veya kullanılan birim sayısı.
Fiyat | PAYG | UnitPrice bakın.
Reservationıd | EA, MCA | Satın alınan ayırmanın örneği için benzersiz tanımlayıcı.
reservationName | EA, MCA | Satın alınan ayırmanın örneğinin adı.
ResourceGroupId | EA, MCA | İçin benzersiz tanımlayıcı [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) içinde kaynaktır.
ResourceGroupName | EA, MCA | Adını [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) içinde kaynaktır.
ResourceId | EA, MCA | Benzersiz tanımlayıcısı [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/resources) kaynak.
resourceLocation | EA, MCA | Kaynağın çalıştığı veri merkezi konumu. Konum bakın.
ResourceName | EA | Kaynağın adı.
ResourceType | MCA | Kaynak örneği türü.
serviceFamily | EA, MCA | Hizmetin ait olduğu hizmet ailesi.
ServiceInfo1 | Tümü | Hizmete özgü meta veriler.
ServiceInfo2 | Tümü | Eski alanda hizmete özgü isteğe bağlı meta verilerle.
servicePeriodEndDate | MCA | Tanımlanan ve tüketilen veya satın alınan hizmeti için fiyatlandırma kilitli derecelendirme dönemi bitiş tarihi.
servicePeriodStartDate | MCA | Tanımlanan ve tüketilen veya satın alınan hizmeti için fiyatlandırma kilitli derecelendirme süresi başlangıç tarihi.
SubscriptionId | Tümü | Abonelik için benzersiz tanımlayıcı.
subscriptionName | Tümü | Abonelik adı.
Tags | Tümü | Kaynağa atanmış etiketler. Kaynak grubu etiketleri içermez. Grup veya dahili geri ödeme maliyetleri dağıtmak için kullanılabilir. Daha fazla bilgi için [etiketlerle Azure kaynaklarınızı düzenleme](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/).
Birim | PAYG | UnitOfMeasure bakın.
UnitOfMeasure | Tümü | Faturalama hizmeti için ölçü birimidir. Örneğin, işlem Hizmetleri, saat başına faturalandırılır.
UnitPrice | EA | Ücretlendirme için birim fiyatı.
UsageDate | PAYG | Tarih bakın.

Bazı alanlar büyük/küçük harfleri ve hesap türleri aralık farklı olabileceğini unutmayın.
Kullandıkça Öde kullanım dosyaların eski sürümlerini deyimi ve günlük kullanım için ayrı bölümlere sahip.

## <a name="ensure-that-your-charges-are-correct"></a>Ücretleriniz doğru olduğundan emin olun

Ayrıntılı kullanım ve Ücret hakkında daha fazla bilgi edinmek için anlama hakkında bilgi için [Kullandıkça Öde](./billing-understand-your-bill.md) veya [Microsoft Müşteri sözleşmesi](billing-mca-understand-your-bill.md) fatura.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Görüntüleyin ve Microsoft Azure faturanızı indirin](billing-download-azure-invoice.md)
- [Microsoft Azure kullanım ve Ücret görüntülemenize ve indirmenize](billing-download-azure-daily-usage.md)
