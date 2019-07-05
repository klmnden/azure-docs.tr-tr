---
title: Bir Microsoft Müşteri sözleşmesi - Azure, fiyat koşullarını anlama
description: Okuma ve bir Microsoft Müşteri sözleşmesi kullanımınızı ve faturanızı anlama hakkında bilgi edinin.
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
ms.openlocfilehash: 4d83228fbec395d604e5ce3f988d2a6157f21eed
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490659"
---
# <a name="terms-in-your-microsoft-customer-agreement-price-sheet"></a>Microsoft Müşteri sözleşmesi fiyat bağlamında

Bu makale, Azure faturalandırma hesabınız için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

Fatura Profil sahibi, katkıda bulunan, okuyucu veya fatura Yöneticisi olması durumunda kuruluşunuzun fiyat Azure portalından indirebilirsiniz. Bkz: [görüntüleyin ve indirme, kuruluşunuzun fiyatlandırma](billing-ea-pricing.md).

## <a name="terms-and-descriptions-in-your-price-sheet"></a>Hüküm ve açıklamaları, fiyat listesi

Aşağıdaki bölümde, Microsoft Müşteri sözleşmesi fiyat listesinde gösterilen bir önemli koşullar açıklanmaktadır.

| **Alan adı**   | **Açıklama**   |
| --- | --- |
| billingAccountId  | Fatura hesabı için benzersiz tanımlayıcı.   |
| billingAccountName  | Fatura hesabı adı.  |
| billingProfileId  | Faturalandırma profili için benzersiz tanımlayıcı.   |
| billingProfileName  | Fatura alacak şekilde ayarlanmış fatura profilinin adı. Fiyat listesindeki fiyat fatura bu profille ilişkilendirilmiş. |
| productOrderName  | Satın alınan ürün planı adı. |
| serviceFamily  | Azure hizmet türü. Örn: İşlem, analiz, güvenlik |
| Product  | Ücretler tahakkuk ürün adı. Örn: Temel SQL veritabanı ile standart SQL veritabanı  |
| productId  | Ölçüm, kullanılan ürün için benzersiz tanımlayıcı. |
| UnitOfMeasure  | Faturalama hizmeti için ölçü tanımlar. Örneğin, işlem Hizmetleri, saat başına faturalandırılır. |
| MeterId  | Ölçüm için benzersiz tanımlayıcı. |
| MeterName  | Ölçüm adı. Ölçüm, bir Azure hizmetinin dağıtılabilir kaynağa temsil eder. |
| MeterCategory  | Ölçüm için sınıflandırma kategorisi adı. Örneğin, _bulut Hizmetleri_, _ağ_vb. |
| meterType  |  Ölçer türü adı. |
| MeterSubCategory  | Ölçüm alt sınıflandırma kategorisi adı.  |
| MeterRegion  | Ölçüm hizmeti için kullanılabilir olduğu bölge adı. Veri merkezi konumuna bağlı olarak ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir.    |
| tierId  | Uygun olduğunda fiyatlandırma katmanını tanımlar. Bu, tierMinimumUnits fiyatları farklılık, katmanlı fiyatları tüketilen birim sayısına göre ayarı ile birlikte çalışır.    |
| tierMinimumUnits  | Fiyatlar kendisi için tanımlanan katmanı aralığının alt sınırı tanımlar. Örneğin, 0-100 aralığında ise tierMinimumUnits 0 olacaktır.  |
| effectiveStartDate  | Başlangıç tarihi fiyat ne zaman etkin hale gelir. |
| effectiveEndDate  | Bitiş tarihi geçerli fiyat. |
| UnitPrice  | Fiyat (değil etkin Harmanlanmış Fiyat) fatura zaman birimi başına kadar ayrıntılı bir ölçüm ve ürün siparişi adı.  Not: Kullanım ayrıntılarını etkili fiyatına katmanlarda fark fiyatlara sahip hizmetler durumunda indirilirken birim fiyatı aynı değil.  Katmanlı fiyatlandırma ile Hizmetleri, durumunda etkin fiyat katmanlarda Harmanlanmış oranıdır ve katman özel birim fiyatı göstermez. Tüketilen Miktar (burada her katmanında belirli bir fiyat bulunur) birden çok katmanlarda kapsamasını için net fiyat, harmanlanmış fiyat ya da etkin fiyat olur. |
| basePrice  | Müşteri Pazar fiyat zaman oturum açtığı veya oturum açma işleminden sonra ise zaman Pazar fiyat hizmet ölçer başlatır.   |

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Görüntüleyin ve kuruluşunuzun fiyatlandırma indirin](billing-ea-pricing.md)
