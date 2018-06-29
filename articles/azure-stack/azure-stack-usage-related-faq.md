---
title: Kullanım API'si ile ilgili sık sorulan sorular | Microsoft Docs
description: Azure yığın ölçümler, Azure kullanım API, kullanım süresi ve hata kodları bildirilen saati karşılaştırma listesi.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: ac22ba34bff1d5321c05bc0a0a1b14ca742079a7
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051517"
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Sık sorulan sorular Azure yığın kullanımı API

Bu makalede Azure yığın kullanım API'si hakkında sık sorulan bazı sorular yanıtlanmaktadır.

## <a name="what-meter-ids-can-i-see"></a>Hangi ölçer kimlikleri görebilirim?
Kullanım aşağıdaki kaynak sağlayıcıları için bildirdi:

**Ağ**  
  
**Ölçüm kimliği**: F271A8A388C44D93956A063E1D2FA80B  
**Ölçüm adı**: statik IP adresi kullanımı  
**Birim**: IP adresleri  
**Notlar**: sayısı IP adresi kullanılır. Günlük bir ayrıntı düzeyi içeren kullanım API'si çağırırsanız, ölçüm saat sayısıyla çarpılmasıyla IP adresini döndürür.  
  
**Ölçüm kimliği**: 9E2739BA86744796B465F64674B822BA  
**Ölçüm adı**: dinamik IP adresi kullanımı  
**Birim**: IP adresleri  
**Notlar**: sayısı IP adresi kullanılır. Günlük bir ayrıntı düzeyi içeren kullanım API'si çağırırsanız, ölçüm saat sayısıyla çarpılmasıyla IP adresini döndürür.  
  
**Depolama**  
  
**Ölçüm kimliği**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Ölçüm adı**: TableCapacity  
**Birim**: GB\*saatleri  
**Notlar**: Toplam Kapasite tabloları tarafından tüketilen.  
  
**Ölçüm kimliği**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Ölçüm adı**: PageBlobCapacity  
**Birim**: GB\*saatleri  
**Notlar**: toplam sayfa blobları tarafından tüketilen kapasitesi.  
  
**Ölçüm kimliği**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Ölçüm adı**: QueueCapacity  
**Birim**: GB\*saatleri  
**Notlar**: Toplam sıra tarafından tüketilen kapasitesi.  
  
**Ölçüm kimliği**: 09F8879E-87E9-4305-A572-4B7BE209F857  
**Ölçüm adı**: BlockBlobCapacity  
**Birim**: GB\*saatleri  
**Notlar**: Toplam Kapasite blok blobları tarafından tüketilen.  
  
**Ölçüm kimliği**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Ölçüm adı**: TableTransactions  
**Birim**: 10, sayıma 000's isteği  
**Notlar**: Tablo hizmet istekleri (10,000s içinde).  
  
**Ölçüm kimliği**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Ölçüm adı**: TableDataTransIn  
**Birim**: giriş verileri GB  
**Notlar**: Tablo hizmeti veri giriş GB.  
  
**Ölçüm kimliği**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**Ölçüm adı**: TableDataTransOut  
**Birim**: çıkış GB  
**Notlar**: Tablo hizmeti veri çıkışı GB  
  
**Ölçüm kimliği**: 43DAF82B-4618-444A-B994-40C23F7CD438  
**Ölçüm adı**: BlobTransactions  
**Birim**: isteklerinin sayısı 10'da 000's  
**Notlar**: Blob hizmet istekleri (10,000s içinde).  
  
**Ölçüm kimliği**: 9764F92C-E44A-498E-8DC1-AAD66587A810  
**Ölçüm adı**: BlobDataTransIn  
**Birim**: giriş verileri GB  
**Notlar**: Blob hizmeti veri giriş GB.  
  
**Ölçüm kimliği**: 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**Ölçüm adı**: BlobDataTransOut  
**Birim**: çıkış GB  
**Notlar**: Blob hizmeti veri çıkışı GB.  
  
**Ölçüm kimliği**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Ölçüm adı**: QueueTransactions  
**Birim**: isteklerinin sayısı 10'da 000's  
**Notlar**: sıraya hizmet istekleri (10,000s içinde).  
  
**Ölçüm kimliği**: E518E809-E369-4A45-9274-2017B29FFF25  
**Ölçüm adı**: QueueDataTransIn  
**Birim**: giriş verileri GB  
**Notlar**: kuyruk hizmet verileri giriş GB.  
  
**Ölçüm kimliği**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Ölçüm adı**: QueueDataTransOut  
**Birim**: çıkış GB  
**Notlar**: kuyruk hizmeti veri çıkışı GB  
  
**SQL RP**  
  
**Ölçüm kimliği**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**Ölçüm adı**: DatabaseSizeHourSqlMeter  
**Birim**: MB\*saatleri  
**Notlar**: toplam DB kapasitede oluşturma. Günlük bir ayrıntı düzeyi içeren kullanım API'si çağırırsanız, ölçüm saat sayısıyla çarpılmasıyla MB döndürür.  
  
**MySql RP**  
  
**Ölçüm kimliği**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Ölçüm adı**: DatabaseSizeHourMySqlMeter  
**Birim**: MB\*saatleri  
**Notlar**: toplam DB kapasitede oluşturma. Günlük bir ayrıntı düzeyi içeren kullanım API'si çağırırsanız, ölçüm saat sayısıyla çarpılmasıyla MB döndürür.  
  
**İşlem**  
  
**Ölçüm kimliği**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Ölçüm adı**: Base VM boyutu saatleri  
**Birim**: sanal çekirdek saatleri  
**Notlar**: sanal çekirdek sayısı VM çalıştıran saatleri tarafından çarpılmasıyla hesaplanır.  
  
**Ölçüm kimliği**: 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**Ölçüm adı**: Windows VM boyutu saatleri  
**Birim**: sanal çekirdek saatleri  
**Notlar**: sanal çekirdek sayısı olarak VM çalıştıran saatte çarpılmasıyla hesaplanır.  
  
**Ölçüm kimliği**: 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**Ölçüm adı**: VM boyutu saatleri  
**Birim**: VM saatleri  
**Notlar**: hem temel hem de Windows VM yakalar. Çekirdeği ayarlanmaz.  
  
**Anahtar Kasası**  
  
**Ölçüm kimliği**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Ölçüm adı**: anahtar kasası işlemleri  
**Birim**: 10, sayıma 000's isteği  
**Notlar**: anahtar kasası veri düzlemi tarafından alınan REST API numarası istekleri.  
  
**Ölçüm kimliği**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Ölçüm adı**: anahtarları işlemleri Gelişmiş  
**Birim**: 10 K işlemleri  
**Notlar**: RSA 4 3 K veya K, ECC anahtar işlemleri. (Önizleme).  
  
*Uygulama hizmeti**  
  
**Ölçüm kimliği**: 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**Ölçüm adı**: uygulama hizmeti  
**Birim**: sanal çekirdek saatleri  
**Notlar**: uygulama hizmeti çalıştırmak için kullanılan sanal çekirdek sayısı. Not: Microsoft Azure yığın uygulama hizmeti kaydedilecek Bu ölçer kullanır. Bulut hizmeti sağlayıcıları kiracıları için kullanım hesaplamak için ölçümler (aşağıda) bir uygulama hizmeti kullanabilirsiniz.  
  
**Ölçüm kimliği**: 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE  
**Ölçüm adı**: işlevleri istekleri  
**Birim**: 10 istekleri  
**Notlar**: İstenen yürütmeleri (her 10 yürütmeleri) toplam sayısı. Yürütmeleri işlevi bir olaya yanıt olarak çalışan veya bir bağlama tarafından tetiklenen her zaman sayılır.  
  
**Ölçüm kimliği**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Ölçüm adı**: işlevleri - işlem  
**Birim**: GB-s  
**Notlar**: kaynak tüketimini gigabayt (GB/s) saniye cinsinden ölçülür. **Kaynak tüketimini gözlenen** ortalama bellek boyutu GB alan işlevi çalıştırmak için milisaniye olarak süre çarparak hesaplanır. Bir işlev tarafından kullanılan bellek, yürütme süresi yuvarlama tarafından hesaplanan 1.536 MB maksimum bellek boyutu kadar yakın 128 MB kadar yuvarlama tarafından ölçülür yakın 1 kadar ms. En düşük yürütme süresi ve bellek tek işlevi yürütme için olan 100 ms ve 128 mb sırasıyla.  
  
**Ölçüm kimliği**: 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**Ölçüm adı**: paylaşılan uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notlar**: parça App Service planı kullanımını saat başına. Planları, uygulama başına temelinde ölçülen.  
  
**Ölçüm kimliği**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**Ölçüm adı**: ücretsiz uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notlar**: ücretsiz uygulama hizmeti planı saat kullanımını başına. Planları, uygulama başına temelinde ölçülen.  
  
**Ölçüm kimliği**: 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**Ölçüm adı**: standart kısa uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notlar**: temel alınarak boyutu ve örnek sayısı hesaplanır.  
  
**Ölçüm kimliği**: 83A2A13E-4788-78DD-5D55-2831B68ED825  
**Ölçüm adı**: Orta Standart uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notlar**: temel alınarak boyutu ve örnek sayısı hesaplanır.  
  
**Ölçüm kimliği**: 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**Ölçüm adı**: büyük standart uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notlar**: temel alınarak boyutu ve örnek sayısı hesaplanır.  
  
**Özel çalışan katmanı**  
  
**Ölçüm kimliği**: *özel çalışan katmanı*  
**Ölçüm adı**: özel çalışan katmanı  
**Birim**: saat  
**Notlar**: belirleyici ölçer kimliği SKU ve özel çalışan katmanı adı göre oluşturulur. Bu ölçüm kimliği her özel çalışan katmanı için benzersizdir.  
  
**Ölçüm kimliği**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**Ölçüm adı**: SNI SSL  
**Birim**: SNI SSL bağlaması başına  
**Notlar**: uygulama hizmeti, iki tür SSL bağlantılarını destekler: sunucu adı göstergesi (SNI) SSL bağlantılarını ve IP adresi SSL bağlantısı. SNI tabanlı SSL modern tarayıcılarda çalışırken IP tabanlı SSL tüm tarayıcılarda çalışır.  
  
**Ölçüm kimliği**: 60B42D72-DC1C-472C-9895-6C516277EDB4  
**Ölçüm adı**: IP SSL  
**Birim**: IP temelli SSL bağlaması  
**Notlar**: uygulama hizmeti, iki tür SSL bağlantılarını destekler: sunucu adı göstergesi (SNI) SSL bağlantılarını ve IP adresi SSL bağlantısı. SNI tabanlı SSL modern tarayıcılarda çalışırken IP tabanlı SSL tüm tarayıcılarda çalışır.  
  
**Ölçüm kimliği**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Ölçüm adı**: Web işlemi  
**Birim**:  
**Notlar**: her saat başına etkin site hesaplanır.  
  
**Ölçüm kimliği**: 5887D39B-0253-4E12-83C7-03E1A93DFFD9  
**Ölçüm adı**: dış Çıkış bant genişliği  
**Birim**: GB  
**Notlar**: toplam gelen istek yanıtı bayt sayısı + bayt + toplam gelen FTP istek yanıtı bayt sayısı + toplam gelen web toplam giden istek dağıtmak istek yanıtı bayt sayısı.  
  

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>API karşılaştırmak için Azure yığın kullanımı nasıl yapılacağı [Azure kullanım API'si](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (şu anda önizlemede ortak)?
* Bir özel durum ile Azure API ile Kiracı kullanım API tutarlıdır: *showDetails* bayrağı şu anda desteklenmiyor Azure yığını.
* Sağlayıcı kullanım API'si yalnızca Azure yığını için geçerlidir.
* Şu anda [RateCard API](https://msdn.microsoft.com/library/azure/mt219004.aspx) diğer bir deyişle Azure'nın Azure yığınında kullanılabilir değil.

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
