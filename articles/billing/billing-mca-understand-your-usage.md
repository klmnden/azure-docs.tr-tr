---
title: Azure kullanımı ve ücretleri dosya için bir Microsoft Müşteri sözleşmesinin koşulları
description: Okuma ve Azure kullanım ve faturalandırma profiliniz için CSV ücretleri bölümlerini anlama hakkında bilgi edinin.
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
ms.openlocfilehash: d11e31366ea5aa15cf7a790eaee800fa2ea6dabe
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490617"
---
# <a name="terms-in-the-azure-usage-and-charges-file-for-a-microsoft-customer-agreement"></a>Azure kullanımı ve ücretleri dosya için bir Microsoft Müşteri sözleşmesinin koşulları

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

Azure kullanım ve Ücret CSV dosyası, geçerli fatura dönemi için günlük ve ölçüm düzey kullanım ücretlerini içerir.

Azure kullanımı ve ücretleri dosyasını almak için bkz. [ve görüntüleyin ve indirme Azure kullanım ücretleri için Microsoft Müşteri sözleşmenizi](billing-download-azure-daily-usage.md). Elektronik tablo uygulamasında açabileceğiniz bir virgülle ayrılmış değerler (.csv) dosya biçiminde kullanılabilir.

Kullanım ücretleri, toplam **aylık** ücretleri bir Abonelikteki. Kullanım ücretleri, herhangi bir kredi veya indirim dikkate almaz.

## <a name="changes-from-azure-ea-usage-and-charges"></a>Azure EA kullanımı ve ücretleri değişiklikleri

Bir EA müşterisinin olsaydı, Azure faturalandırma profili kullanım CSV dosyası koşullarını Azure EA kullanım CSV dosyasında terimlerden farklı olduğunu fark edeceksiniz. Bir eşleme profili kullanım koşulları faturalandırma için EA Kullanım Koşulları'nın şu şekildedir:

| Azure EA kullanım CSV | Microsoft Müşteri sözleşmesi Azure kullanım ve Ücret CSV |
| --- | --- |
| Tarih | date |
| Ay| date |
| Gün | date |
| Yıl | date |
| Product | product |
| MeterId | meterID |
| MeterCategory | MeterCategory |
| MeterSubCategory | MeterSubCategory |
| MeterRegion | MeterRegion |
| MeterName | MeterName |
| ConsumedQuantity | Miktar |
| ResourceRate | effectivePrice |
| ExtendedCost | maliyet |
| resourceLocation | resourceLocation |
| ConsumedService | ConsumedService |
| InstanceId | InstanceId |
| ServiceInfo1 | ServiceInfo1 |
| ServiceInfo2 | ServiceInfo2 |
| Additionalınfo | Additionalınfo |
| Tags | tags |
| StoreServiceIdentifier | Yok |
| Bölüm adı | invoiceSection |
| CostCenter | CostCenter |
| UnitOfMeasure | unitofMeasure |
| ResourceGroup | resourceGroup |
| ChargesBilledSeparately | isAzureCreditEligible |

## <a name="detailed-terms-and-descriptions"></a>Ayrıntılı hüküm ve açıklamaları

Aşağıdaki terimler Azure kullanım ve Ücret dosyasında gösterilmektedir.

Terim | Açıklama
--- | ---
Fatura kodu | PDF faturada listelenen belgenin benzersiz kimliği
previousInvoiceId | Bu satır öğesi bir para iadesi ise özgün Fatura referansı
billingAccountName | Fatura hesabı adı
billingAccountId | Kök hesabı faturalama için benzersiz tanımlayıcı
billingProfileId | Faturalandırılan ücretler tahakkuk eder fatura profilinin adı
billingProfileName | Faturalandırılan ücretler tahakkuk eder faturalandırma profili için benzersiz tanımlayıcı
invoiceSectionId | Fatura bölümü için benzersiz tanımlayıcı
invoiceSectionName | Fatura bölümün adı
CostCenter | Abonelik maliyetleri (yalnızca açık faturalandırma dönemlerini kullanılabilir) izlemek için tanımlanan maliyet merkezi
billingPeriodStartDate | Fatura oluşturulduğu fatura dönemi başlangıç tarihi
billingPeriodEndDate | Fatura oluşturulduğu fatura dönemi bitiş tarihi
servicePeriodStartDate | Derecelendirme süresi başlangıç tarihi tanımlanan ve tüketilen veya satın alınan hizmeti için fiyatlandırma kilitli
servicePeriodEndDate | Derecelendirme dönemi bitiş tarihi tanımlanan ve tüketilen veya satın alınan hizmeti için fiyatlandırma kilitli
date | Azure ve Market kullanım tabanlı ücretleri için bu derecelendirme tarihtir. Bu, tek seferlik satın alma işlemleri (ayırmalar, Market) veya sabit tekrar eden ücretler (destek sunar) için satın alma tarihtir.
serviceFamily | Hizmetin ait olduğu hizmet ailesi
productOrderId | Ürün siparişi için benzersiz tanımlayıcı
productOrderName | Ürün siparişi için benzersiz bir ad
ConsumedService | Tüketim hizmeti adı
MeterId | Ölçüm için benzersiz tanımlayıcı
MeterName | Ölçüm adı
MeterCategory | Ölçüm için sınıflandırma kategorisi adı. Örneğin, *bulut Hizmetleri*, *ağ*vb.
MeterSubCategory | Ölçüm alt sınıflandırma kategorisi adı
MeterRegion | Ölçüm hizmeti için kullanılabilir olduğu bölge adı. Veri Merkezi konumuna göre ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir.
Teklif | Satın alınan teklif adı
productId | Ücretler tahakkuk ürün için benzersiz tanımlayıcı
product | Ücretler tahakkuk ürün adı
Abonelik kimliği | Ücretler tahakkuk abonelik için benzersiz tanımlayıcı
subscriptionName | Ücretler tahakkuk abonelik adı
Reservationıd | Satın alınan ayırmanın örneği için benzersiz tanımlayıcı
reservationName | Satın alınan ayırmanın örneğinin adı
publisherType | Yayımcı türü (değerleri: firstParty, thirdPartyReseller, thirdPartyAgency)
PublisherName | Yayımcının Market Hizmetleri
resourceGroupId | Bu kaynakla ilgili kaynak grubu için benzersiz tanımlayıcı
resourceGroupName | Kaynakla ilişkili bir kaynak grubu adı
resourceId | Kaynak örneği için benzersiz tanımlayıcı
Kaynak türü | Kaynak örnek türü
resourceLocation | Kaynağın çalıştığı veri merkezinin konumunu belirtir.
location | Farklı kaynak konumları aynı bölgeler için yapılandırıldıysa, kaynağın normalleştirilmiş konum
Miktar | Satın alınan veya kullanılan birim sayısı
UnitOfMeasure | Faturalama hizmeti için ölçü birimidir. Örneğin, işlem Hizmetleri, saat başına faturalandırılır.
chargeType | Ücret türü. Değerler: <ul><li>AsCharged Kullanım: Bir Azure hizmeti kullanıma bağlı doğan ücretleri. Bu, kullanım karşı ayrılmış örnekler nedeniyle ücretlendirilmez Vm'leri içerir.</li><li>AsCharged PurchaseMarketplace: Market satın alımları gelen tek seferlik veya sabit tekrar eden ücretler</li><li>AsCharged UsageMarketplace: Tüketim birimlerine göre ücretlendirilen Market Hizmetleri ücretlerine uygulanabilir</li></ul>
isAzureCreditEligible | Hizmetinde ücretsiz Azure KREDİLERİ kullanımı için ödenecek uygun olup olmadığını gösteren bayrak (değerleri: TRUE, False)
ServiceInfo1 | Hizmete özgü meta veriler
ServiceInfo2 | İsteğe bağlı hizmete özgü meta veriler yakalanır eski alan
Additionalınfo | Ek hizmete özgü meta veriler.
tags | Kaynağa atadığınız etiketler

### <a name="make-sure-that-charges-are-correct"></a>Ücretleri doğru olduğundan emin olun

Ayrıntılı kullanım dosyanızdaki ücretleri doğru olduğundan emin olmak istiyorsanız, bunları doğrulayabilirsiniz. Bkz: [fatura profil faturasında ücretlerini anlama](billing-mca-understand-your-bill.md)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Görüntüleyin ve Microsoft Azure faturanızı indirin](billing-download-azure-invoice.md)
- [Microsoft Azure kullanım ve Ücret görüntülemenize ve indirmenize](billing-download-azure-daily-usage.md)
