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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
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
| Product | ürün |
| Ölçüm kimliği | meterID |
| Ölçüm kategorisi | meterCategory |
| Ölçüm alt kategorisi | meterSubCategory |
| Ölçüm bölgesi | meterRegion |
| Ölçüm adı | meterName |
| Tüketilen miktar | quantity |
| Kaynak fiyatı | effectivePrice | <!-- this was highlighted -->
| Ayrıntılı maliyet | maliyet |
| Kaynak konumu | resourceLocation |
| Tüketilen hizmet | consumedService |
| Örnek kimliği | instanceId |
| Hizmet bilgisi 1 | serviceInfo1 |
| Hizmet bilgisi 2 | serviceInfo2 |
| Ek bilgi | additionalInfo |
| Etiketler | etiketler |
| Depolama hizmeti tanımlayıcısı | Yok |
| Bölüm adı | invoiceSection | <!-- this was highlighted -->
| Maliyet merkezi | costCenter |
| Ölçü birimi | unitofMeasure |
| ResourceGroup | resourceGroup |
| ChargesBilledSeparately | isAzureCreditEligible | <!-- this was highlighted -->

<!-- TO DO: Marketplace CSV? -->

## <a name="detailed-terms-and-descriptions-in-your-azure-usage-and-charges-file"></a>Ayrıntılı hüküm ve Azure kullanım ve Ücret dosyanızdaki açıklamaları

Aşağıdaki bölümde Azure kullanımı ve ücretleri dosyasında gösterilen önemli koşullar açıklanmaktadır.

Sözleşme Dönemi | Açıklama
--- | ---
invoiceId | PDF faturada listelenen belgenin benzersiz kimliği
previousInvoiceId | Bu satır öğesi bir para iadesi ise özgün Fatura referansı
billingAccountName | Fatura hesabı adı
billingAccountId | Kök hesabı faturalama için benzersiz tanımlayıcı
billingProfileId | Faturalandırılan ücretler tahakkuk eder fatura profilinin adı
billingProfileName | Faturalandırılan ücretler tahakkuk eder faturalandırma profili için benzersiz tanımlayıcı
invoiceSectionId | Fatura bölümü için benzersiz tanımlayıcı
invoiceSectionName | Fatura bölümün adı
costCenter | Abonelik maliyetleri (yalnızca açık faturalandırma dönemlerini kullanılabilir) izlemek için tanımlanan maliyet merkezi
billingPeriodStartDate | Fatura oluşturulduğu fatura dönemi başlangıç tarihi
billingPeriodEndDate | Fatura oluşturulduğu fatura dönemi bitiş tarihi
servicePeriodStartDate | Derecelendirme süresi başlangıç tarihi tanımlanan ve tüketilen veya satın alınan hizmeti için fiyatlandırma kilitli
servicePeriodEndDate | Derecelendirme dönemi bitiş tarihi tanımlanan ve tüketilen veya satın alınan hizmeti için fiyatlandırma kilitli
date | Azure ve Market kullanım tabanlı ücretleri için bu derecelendirme tarihtir. Bu, tek seferlik satın alma işlemleri (ayırmalar, Market) veya sabit tekrar eden ücretler (destek sunar) için satın alma tarihtir.
serviceFamily | Hizmetin ait olduğu hizmet ailesi
productOrderId | Ürün siparişi için benzersiz tanımlayıcı
productOrderName | Ürün siparişi için benzersiz bir ad
consumedService | Tüketim hizmeti adı
meterId | Ölçüm için benzersiz tanımlayıcı
meterName | Ölçüm adı
meterCategory | Ölçüm için sınıflandırma kategorisi adı. Örneğin, *bulut Hizmetleri*, *ağ*vb.
meterSubCategory | Ölçüm alt sınıflandırma kategorisi adı
meterRegion | Ölçüm hizmeti için kullanılabilir olduğu bölge adı. Veri Merkezi konumuna göre ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir.
Teklif | Satın alınan teklif adı
productId | Ücretler tahakkuk ürün için benzersiz tanımlayıcı
ürün | Ücretler tahakkuk ürün adı
Abonelik kimliği | Ücretler tahakkuk abonelik için benzersiz tanımlayıcı
subscriptionName | Ücretler tahakkuk abonelik adı
reservationId | Satın alınan ayırmanın örneği için benzersiz tanımlayıcı
reservationName | Satın alınan ayırmanın örneğinin adı
publisherType | Yayımcı türü (değerleri: firstParty, thirdPartyReseller, thirdPartyAgency)
publisherName | Yayımcının Market Hizmetleri
resourceGroupId | Bu kaynakla ilgili kaynak grubu için benzersiz tanımlayıcı
resourceGroupName | Kaynakla ilişkili bir kaynak grubu adı
resourceId | Kaynak örneği için benzersiz tanımlayıcı
Kaynak türü | Kaynak örnek türü
resourceLocation | Kaynağın çalıştığı veri merkezinin konumunu belirtir.
location | Farklı kaynak konumları aynı bölgeler için yapılandırıldıysa, kaynağın normalleştirilmiş konum
quantity | Satın alınan veya kullanılan birim sayısı
unitOfMeasure | Faturalama hizmeti için ölçü birimidir. Örneğin, işlem Hizmetleri, saat başına faturalandırılır.
chargeType | Ücret türü. Değerler: <ul><li>AsCharged Kullanım: Bir Azure hizmeti kullanıma bağlı doğan ücretleri. Bu, kullanım karşı ayrılmış örnekler nedeniyle ücretlendirilmez Vm'leri içerir.</li><li>AsCharged PurchaseMarketplace: Market satın alımları gelen tek seferlik veya sabit tekrar eden ücretler</li><li>AsCharged UsageMarketplace: Tüketim birimlerine göre ücretlendirilen Market Hizmetleri ücretlerine uygulanabilir</li></ul>
isAzureCreditEligible | Hizmetinde ücretsiz Azure KREDİLERİ kullanımı için ödenecek uygun olup olmadığını gösteren bayrak (değerleri: TRUE, False)
serviceInfo1 | Hizmete özgü meta veriler
serviceInfo2 | İsteğe bağlı hizmete özgü meta veriler yakalanır eski alan
additionalInfo | Ek hizmete özgü meta veriler.
etiketler | Kaynağa atadığınız etiketler

### <a name="how-do-i-make-sure-that-the-charges-in-my-azure-usage-and-charges-file-are-correct"></a>Azure kullanım ve Ücret dosyamı ücretleri doğru olduğunu nasıl emin olabilirim?

Daha fazla ayrıntı istediğiniz ayrıntılı kullanım dosyanızdaki bir ücret yoksa bkz [fatura profil faturasında ücretlerini anlama](billing-mca-understand-your-bill.md)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Görüntüleyin ve Microsoft Azure faturanızı indirin](billing-download-azure-invoice.md)
- [Microsoft Azure kullanım ve Ücret görüntülemenize ve indirmenize](billing-download-azure-daily-usage.md)
