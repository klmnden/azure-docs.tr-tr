---
title: Bir Microsoft Müşteri sözleşmesi - Azure, fiyat koşullarını anlama | Microsoft Docs
description: Okuma ve Azure aboneliğiniz için fatura ve kullanım anlama hakkında bilgi edinin
services: ''
documentationcenter: ''
author: jureid
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
ms.openlocfilehash: eb6184e10d38cdcfad7070663e36f6610d009cdb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60371368"
---
# <a name="understand-the-terms-in-your-price-sheet-for-a-microsoft-customer-agreement"></a>Bir Microsoft Müşteri sözleşmesi, fiyat koşullarını anlama

Bu makale, bir faturalama hesabı için bir Microsoft Müşteri sözleşmesi için geçerlidir. [Microsoft Müşteri sözleşmesi erişimi olup olmadığını denetlemek](#check-access-to-a-microsoft-customer-agreement).

Fatura Profil sahibi, katkıda bulunan, okuyucu veya fatura Yöneticisi olması durumunda kuruluşunuzun fiyat Azure portalından indirebilirsiniz. Bkz: [görüntüleyin ve indirme, kuruluşunuzun fiyatlandırma](billing-ea-pricing.md).

## <a name="detailed-terms-and-descriptions-in-your-microsoft-customer-agreement-price-sheet"></a>Ayrıntılı hüküm ve Microsoft Müşteri sözleşmesi fiyat açıklamaları

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
| unitOfMeasure  | Faturalama hizmeti için ölçü tanımlar. Örneğin, işlem Hizmetleri, saat başına faturalandırılır. |
| meterId  | Ölçüm için benzersiz tanımlayıcı. |
| meterName  | Ölçüm adı. Ölçüm, bir Azure hizmetinin dağıtılabilir kaynağa temsil eder. |
| meterCategory  | Ölçüm için sınıflandırma kategorisi adı. Örneğin, _bulut Hizmetleri_, _ağ_vb. |
| meterType  |  Ölçer türü adı. |
| meterSubCategory  | Ölçüm alt sınıflandırma kategorisi adı.  |
| meterRegion  | Ölçüm hizmeti için kullanılabilir olduğu bölge adı. Veri merkezi konumuna bağlı olarak ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir.    |
| tierId  | Uygun olduğunda fiyatlandırma katmanını tanımlar. Bu, tierMinimumUnits fiyatları farklılık, katmanlı fiyatları tüketilen birim sayısına göre ayarı ile birlikte çalışır.    |
| tierMinimumUnits  | Fiyatlar kendisi için tanımlanan katmanı aralığının alt sınırı tanımlar. Örneğin, 0-100 aralığında ise tierMinimumUnits 0 olacaktır.  |
| effectiveStartDate  | Başlangıç tarihi fiyat ne zaman etkin hale gelir. |
| effectiveEndDate  | Bitiş tarihi geçerli fiyat. |
| UnitPrice  | Fiyat (değil etkin Harmanlanmış Fiyat) fatura zaman birimi başına kadar ayrıntılı bir ölçüm ve ürün siparişi adı.  Not: Kullanım ayrıntılarını etkili fiyatına katmanlarda fark fiyatlara sahip hizmetler durumunda indirilirken birim fiyatı aynı değil.  Katmanlı fiyatlandırma ile Hizmetleri, durumunda etkin fiyat katmanlarda Harmanlanmış oranıdır ve katman özel birim fiyatı göstermez. Tüketilen Miktar (burada her katmanında belirli bir fiyat bulunur) birden çok katmanlarda kapsamasını için net fiyat, harmanlanmış fiyat ya da etkin fiyat olur. |
| basePrice  | Müşteri Pazar fiyat zaman oturum açtığı veya oturum açma işleminden sonra ise zaman Pazar fiyat hizmet ölçer başlatır.   |

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Microsoft Müşteri sözleşmesi için erişim denetimi
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

- [Görüntüleyin ve kuruluşunuzun fiyatlandırma indirin](billing-ea-pricing.md)
