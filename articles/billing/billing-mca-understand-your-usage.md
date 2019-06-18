---
title: Bir Microsoft Müşteri sözleşmesi Azure kullanım ve Ücret CSV ilgili koşulları anlama | Microsoft Docs
description: Okuma ve Azure kullanım ve faturalandırma profiliniz için CSV ücretleri bölümlerini anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: bandersmsft
manager: alherz
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: banders
ms.openlocfilehash: 8f71f42386ce49d4d7178cb03d28d74edacd7e39
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60371317"
---
# <a name="understand-terms-on-your-azure-usage-and-charges-csv-for-a-microsoft-customer-agreement"></a>Bir Microsoft Müşteri sözleşmesi Azure kullanım ve Ücret CSV ilgili koşulları anlama

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

Azure kullanım ve Ücret CSV dosyası, geçerli fatura dönemi için günlük ve ölçüm düzey kullanım ücretlerini içerir.

Azure kullanımı ve ücretleri dosyasını almak için bkz. [ve görüntüleyin ve indirme Azure kullanım ücretleri için Microsoft Müşteri sözleşmenizi](billing-download-azure-daily-usage.md).
Elektronik tablo uygulamasında açabileceğiniz bir virgülle ayrılmış değerler (.csv) dosya biçiminde kullanılabilir.

Kullanım ücretleri, toplam **aylık** ücretleri bir Abonelikteki. Kullanım ücretleri, herhangi bir kredi veya indirim dikkate almaz.

## <a name="changes-in-the-enterprise-agreement-azure-usage-and-charges-csv"></a>Kurumsal Anlaşma Azure kullanım ve Ücret CSV değişiklikleri

Bir EA müşterisinin olsaydı, faturalandırma profili CSV dosyası Azure kullanım koşullarını EA Azure kullanım CSV dosyasında koşulları farklı olan fark edeceksiniz. Bir eşleme profili kullanım koşulları faturalandırma için EA Kullanım Koşulları'nın şu şekildedir:

| Kurumsal Anlaşma Azure kullanım CSV | Microsoft Müşteri sözleşmesi Azure kullanım ve Ücret CSV |
| --- | --- |
| Tarih | date |
| Ay| date |
| Gün | date |
| Yıl | date |
| Product | Ürün |
| MeterId | meterID |
| MeterCategory | MeterCategory |
| MeterSubCategory | MeterSubCategory |
| MeterRegion | MeterRegion |
| MeterName | MeterName |
| ConsumedQuantity | Miktar |
| ResourceRate | effectivePrice | <!-- this was highlighted -->
| ExtendedCost | maliyet |
| resourceLocation | resourceLocation |
| ConsumedService | ConsumedService |
| InstanceId | InstanceId |
| ServiceInfo1 | ServiceInfo1 |
| ServiceInfo2 | ServiceInfo2 |
| Additionalınfo | Additionalınfo |
| Tags | tags |
| StoreServiceIdentifier | Yok |
| Bölüm adı | invoiceSection | <!-- this was highlighted -->
| CostCenter | CostCenter |
| UnitOfMeasure | unitofMeasure |
| ResourceGroup | Kaynak grubu |
| ChargesBilledSeparately | isAzureCreditEligible | <!-- this was highlighted -->

<!-- TO DO: Marketplace CSV? -->

## <a name="detailed-terms-and-descriptions-in-your-azure-usage-and-charges-file"></a>Ayrıntılı hüküm ve Azure kullanım ve Ücret dosyanızdaki açıklamaları

Aşağıdaki bölümde Azure kullanımı ve ücretleri dosyasında gösterilen önemli koşullar açıklanmaktadır.

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
Ürün | Ücretler tahakkuk ürün adı
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

### <a name="how-do-i-make-sure-that-the-charges-in-my-azure-usage-and-charges-file-are-correct"></a>Azure kullanım ve Ücret dosyamı ücretleri doğru olduğunu nasıl emin olabilirim?

Daha fazla ayrıntı istediğiniz ayrıntılı kullanım dosyanızdaki bir ücret yoksa bkz [fatura profil faturasında ücretlerini anlama](billing-mca-understand-your-bill.md)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Görüntüleyin ve Microsoft Azure faturanızı indirin](billing-download-azure-invoice.md)
- [Microsoft Azure kullanım ve Ücret görüntülemenize ve indirmenize](billing-download-azure-daily-usage.md)
