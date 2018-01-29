---
title: "Kullanım API'si ile ilgili sık sorulan sorular | Microsoft Docs"
description: "Azure yığın ölçümler, Azure kullanım API, kullanım süresi ve hata kodları bildirilen saati karşılaştırma listesi."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: alfredop
ms.openlocfilehash: 65b9ff0881e46836d9f19a04cf470835679e7b2f
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Sık sorulan sorular Azure yığın kullanımı API
Bu makalede Azure yığın kullanım API'si hakkında sık sorulan bazı sorular yanıtlanmaktadır.

## <a name="what-meter-ids-can-i-see"></a>Hangi ölçer kimlikleri görebilirim?
Kullanım aşağıdaki kaynak sağlayıcıları için bildirdi:

| **Kaynak sağlayıcısı** | **Ölçer kimliği** | **Ölçüm adı** | **Birim** | **Ek bilgi** |
| --- | --- | --- | --- | --- |
| **Ağ** |F271A8A388C44D93956A063E1D2FA80B |Statik IP adresi kullanımı |IP adresleri| Kullanılan sayısı, IP adresleri |
| |9E2739BA86744796B465F64674B822BA |Dinamik IP adresi kullanımı |IP adresleri| Kullanılan sayısı, IP adresleri |
| **Depolama** |B4438D5D-453B-4EE1-B42A-DC72E377F1E4 |TableCapacity |GB\*saatleri |Tabloları tarafından kullanılan toplam kapasite |
| |B5C15376-6C94-4FDD-B655-1A69D138ACA3 |PageBlobCapacity |GB\*saatleri |Sayfa blobları tarafından kullanılan toplam kapasite |
| |B03C6AE7-B080-4BFA-84A3-22C800F315C6 |QueueCapacity |GB\*saatleri |Sıra tarafından kullanılan toplam kapasite |
| |09F8879E-87E9-4305-A572-4B7BE209F857 |BlockBlobCapacity |GB\*saatleri |Blok blobları tarafından kullanılan toplam kapasite |
| |B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 |TableTransactions |10, 000's isteği sayısı |Tablo hizmeti isteklerinde (10, 000's) |
| |50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D |TableDataTransIn |Giriş verileri GB |Tablo hizmeti veri giriş GB |
| |1B8C1DEC-EE42-414B-AA36-6229CF199370 |TableDataTransOut |Çıkış GB |Tablo hizmeti veri çıkışı GB |
| |43DAF82B-4618-444A-B994-40C23F7CD438 |BlobTransactions |000's 10'da isteklerinin sayısı |BLOB hizmeti isteklerinde (10, 000's) |
| |9764F92C-E44A-498E-8DC1-AAD66587A810 |BlobDataTransIn |Giriş verileri GB |BLOB hizmeti veri giriş GB |
| |3023FEF4-ECA5-4D7B-87B3-CFBC061931E8 |BlobDataTransOut |Çıkış GB |BLOB hizmeti veri çıkışı GB |
| |EB43DD12-1AA6-4C4B-872C-FAF15A6785EA |QueueTransactions |000's 10'da isteklerinin sayısı |Kuyruk hizmet isteklerini (10, 000's) |
| |E518E809-E369-4A45-9274-2017B29FFF25 |QueueDataTransIn |Giriş verileri GB |Kuyruk hizmeti veri giriş GB |
| |DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2 |QueueDataTransOut |Çıkış GB |Kuyruk hizmeti veri çıkışı GB |
| **Sql RP**            | CBCFEF9A-B91F-4597-A4D3-01FE334BED82 | DatabaseSizeHourSqlMeter   | MB\*saatleri   | Toplam DB kapasitede oluşturma, her saat bildirdi.  |
| **MySql RP**          | E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3 | DatabaseSizeHourMySqlMeter | MB\*saatleri    | Toplam DB kapasitede oluşturma, her saat bildirdi. |
| **İşlem** |FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5 |Temel VM boyutu saatleri |Sanal çekirdek dakika | Sanal çekirdek sayısı VM çalıştıran dakika zaman |
| |9CD92D4C-BAFD-4492-B278-BEDC2DE8232A |Windows VM boyutu saatleri |Sanal çekirdek dakika | Sanal çekirdek sayısı VM çalıştıran dakika zaman |
| |6DAB500F-A4FD-49C4-956D-229BB9C8C793 |VM boyutu saatleri |VM saatleri |Hem temel hem de Windows VM yakalar. Çekirdeği ayarlanmaz |
| **Anahtar Kasası** |EBF13B9F-B3EA-46FE-BF54-396E93D48AB4 |Anahtar kasası işlemleri | 10, 000's isteği sayısı| Anahtar kasası veri düzlemi tarafından alınan REST API isteklerinin sayısı |
| **Uygulama hizmeti** |190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  | App Service   | Sanal çekirdek saatleri  | Uygulama hizmeti çalıştırmak için kullanılan sanal çekirdek sayısı |
|             | 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE | İşlevler - işlem istekleri      | 10 istekleri              | İşlevlere uygulanır  |
|             | 957E9F36-2C14-45A1-B6A1-1723EF71A01D | Paylaşılan uygulama hizmeti saatleri          | 1 saat                   |                       |
|             | 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9 | Ücretsiz uygulama hizmeti saatleri            | 1 saat                   |                       |
|             | 88039D51-A206-3A89-E9DE-C5117E2D10A6 | Standart kısa uygulama hizmeti saatleri  | 1 saat                   |                       |
|             | 83A2A13E-4788-78DD-5D55-2831B68ED825 | Standart Orta uygulama hizmeti saatleri | 1 saat                   |                       |
|             | 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6 | Standart uzun uygulama hizmeti saatleri  | 1 saat                   |                       |
|             | 264ACB47-AD38-47F8-ADD3-47F01DC4F473 | SNI SSL                           | Per SNI SSL Binding      | Uygulama hizmeti için geçerlidir |
|             | 60B42D72-DC1C-472C-9895-6C516277EDB4 | IP SSL                            | IP temelli SSL bağlaması | Uygulama hizmeti için geçerlidir |

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>API karşılaştırmak için Azure yığın kullanımı nasıl yapılacağı [Azure kullanım API'si](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (şu anda önizlemede ortak)?
* Bir özel durum ile Azure API ile Kiracı kullanım API tutarlıdır: *showDetails* bayrağı şu anda desteklenmiyor Azure yığını.
* Sağlayıcı kullanım API'si yalnızca Azure yığını için geçerlidir.
* Şu anda [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) diğer bir deyişle Azure'nın Azure yığınında kullanılabilir değil.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Kullanım süresi ve bildirilen süresi arasındaki fark nedir?
Kullanım verileri raporları iki ana saat değerleri vardır:

* **Bildirilen süresi**. Ne zaman kullanım olayı kullanım sistem süresi
* **Kullanım süresi**. Azure yığın kaynak zaman tüketilen saat

Belirli kullanım olayı için kullanım süresi ve bildirilen süresi için değerlerin tutarsızlık görebilirsiniz. Gecikme olduğu sürece herhangi bir ortamın birden çok saat olabilir.

Şu anda yalnızca Sorgulayabileceğiniz *bildirilen zaman*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Bu kullanım API hata kodları ne anlama geliyor?
| **HTTP durum kodu** | **Hata kodu** | **Açıklama** |
| --- | --- | --- |
| 400/Hatalı istek |*NoApiVersion* |*API sürümü* sorgu parametresi eksik. |
| 400/Hatalı istek |*InvalidProperty* |Bir özellik eksik veya geçersiz bir değere sahip. Yanıt gövdesi hata kodunu iletisinde eksik özelliği tanımlar. |
| 400/Hatalı istek |*RequestEndTimeIsInFuture* |Değeri *ReportedEndTime* gelecekte. Değerleri gelecekte bu bağımsız değişken için izin verilmez. |
| 400/Hatalı istek |*SubscriberIdIsNotDirectTenant* |Sağlayıcısı API çağrısı arayanın geçerli bir kiracı değil bir abonelik kimliği kullandı. |
| 400/Hatalı istek |*SubscriptionIdMissingInRequest* |Arayanın abonelik kimliği eksik. |
| 400/Hatalı istek |*InvalidAggregationGranularity* |Geçersiz toplama ayrıntı düzeyi istendi. Geçerli değerler, günlük ve saatlik. |
| 503 |*ServiceUnavailable* |Hizmet meşgul veya çağrı kısıtlanan nedeniyle yeniden denenebilir bir hata oluştu. |

## <a name="next-steps"></a>Sonraki Adımlar
[Faturalama müşteri ve Azure yığınında geri ödeme](azure-stack-billing-and-chargeback.md)

[Sağlayıcı kaynak kullanım API'si](azure-stack-provider-resource-api.md)

[Kiracı kaynak kullanım API'si](azure-stack-tenant-resource-usage-api.md)
