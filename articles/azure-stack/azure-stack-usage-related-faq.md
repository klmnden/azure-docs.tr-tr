---
title: Kullanım API'si ilgili sık sorulan sorular | Microsoft Docs
description: Azure Stack ölçümlerini, Azure kullanım API'si, kullanım süresi ve hata kodları bildirilen saati karşılaştırma listesi.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: ffe3dbf975984eb8df341728075e628d14080ada
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44380242"
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Sık sorulan sorular, Azure Stack kullanım API'si

Bu makalede, Azure Stack kullanım API'si hakkında sık sorulan bazı sorular yanıtlanmaktadır.

## <a name="what-meter-ids-can-i-see"></a>Hangi ölçüm kimlikleri görebilirim?
Aşağıdaki kaynak sağlayıcıları için kullanımı bildirilir:

### <a name="network"></a>Ağ
  
**Ölçüm kimliği**: F271A8A388C44D93956A063E1D2FA80B  
**Ölçüm adı**: statik IP adresi kullanımı  
**Birim**: IP adresleri  
**Notları**: sayısı IP adresleri kullanılır. ' % S'kullanım API'si ile bir günlük ayrıntı düzeyi çağırırsanız, ölçüm saat sayı ile çarpılan IP adresini döndürür.  
  
**Ölçüm kimliği**: 9E2739BA86744796B465F64674B822BA  
**Ölçüm adı**: dinamik IP adresi kullanımı  
**Birim**: IP adresleri  
**Notları**: sayısı IP adresleri kullanılır. ' % S'kullanım API'si ile bir günlük ayrıntı düzeyi çağırırsanız, ölçüm saat sayı ile çarpılan IP adresini döndürür.  
  
### <a name="storage"></a>Depolama
  
**Ölçüm kimliği**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Ölçüm adı**: TableCapacity  
**Birim**: GB\*saat  
**Notları**: Toplam Kapasite tabloları tarafından tüketilen.  
  
**Ölçüm kimliği**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Ölçüm adı**: PageBlobCapacity  
**Birim**: GB\*saat  
**Notları**: toplam sayfa blobları tarafından tüketilen kapasite.  
  
**Ölçüm kimliği**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Ölçüm adı**: QueueCapacity  
**Birim**: GB\*saat  
**Notları**: Toplam Kapasite kuyruk tarafından tüketilen.  
  
**Ölçüm kimliği**: 09F8879E-87E9-4305-A572-4B7BE209F857  
**Ölçüm adı**: BlockBlobCapacity  
**Birim**: GB\*saat  
**Notları**: toplam blok BLOB'ları tarafından kullanılan kapasite.  
  
**Ölçüm kimliği**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Ölçüm adı**: TableTransactions  
**Birim**: istek sayısı 10'da, 000's  
**Notları**: Tablo hizmeti istekleri (10,000s içinde).  
  
**Ölçüm kimliği**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Ölçüm adı**: TableDataTransIn  
**Birim**: giriş verileri GB  
**Notları**: Tablo hizmeti veri girişi GB.  
  
**Ölçüm kimliği**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**Ölçüm adı**: TableDataTransOut  
**Birim**: GB çıkış  
**Notları**: Tablo hizmeti veri çıkışı GB  
  
**Ölçüm kimliği**: 43DAF82B-4618-444A-B994-40C23F7CD438  
**Ölçüm adı**: BlobTransactions  
**Birim**: istek sayısı 10'da 000's  
**Notları**: Blob hizmeti isteklerinin (10,000s içinde).  
  
**Ölçüm kimliği**: 9764F92C-E44A-498E-8DC1-AAD66587A810  
**Ölçüm adı**: BlobDataTransIn  
**Birim**: giriş verileri GB  
**Notları**: Blob hizmeti veri girişi GB.  
  
**Ölçüm kimliği**: 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**Ölçüm adı**: BlobDataTransOut  
**Birim**: GB çıkış  
**Notları**: Blob hizmeti GB veri çıkışı.  
  
**Ölçüm kimliği**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Ölçüm adı**: QueueTransactions  
**Birim**: istek sayısı 10'da 000's  
**Notları**: sıra hizmet istekleri (10,000s içinde).  
  
**Ölçüm kimliği**: E518E809-E369-4A45-9274-2017B29FFF25  
**Ölçüm adı**: QueueDataTransIn  
**Birim**: giriş verileri GB  
**Notları**: kuyruk hizmeti veri girişi GB.  
  
**Ölçüm kimliği**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Ölçüm adı**: QueueDataTransOut  
**Birim**: GB çıkış  
**Notları**: kuyruk hizmeti GB veri çıkışı  

### <a name="compute"></a>İşlem 
  
**Ölçüm kimliği**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Ölçüm adı**: temel VM boyutu saatleri  
**Birim**: sanal çekirdek saatleri  
**Notları**: sanal çekirdek sayısı çarpılmış olarak VM çalıştıran saat.  
  
**Ölçüm kimliği**: 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**Ölçüm adı**: Windows VM boyutu saatleri  
**Birim**: sanal çekirdek saatleri  
**Notları**: sanal çekirdek sayısı çarpılmış olarak VM çalıştıran saat.  
  
**Ölçüm kimliği**: 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**Ölçüm adı**: VM boyutu saatleri  
**Birim**: VM saatleri  
**Notları**: hem temel hem de Windows VM yakalar. Çekirdekler için ayarlanmaz.  
  
### <a name="managed-disks"></a>Yönetilen Diskler

**Ölçüm kimliği**: 5d76e09f-4567-452a-94cc-7d1f097761f0   
**Ölçüm adı**: S4   
**Birim**: disk sayısı   
**Notları**: standart yönetilen Disk-32 GB 

**Ölçüm kimliği**: dc9fc6a9-0782-432a-b8dc-978130457494   
**Ölçüm adı**: S6   
**Birim**: disk sayısı   
**Notları**: standart yönetilen Disk – 64 GB 

**Ölçüm kimliği**: e5572fce-9f58-49d7-840c-b168c0f01fff   
**Ölçüm adı**: S10   
**Birim**: disk sayısı   
**Notları**: standart yönetilen Disk – 128 GB 

**Ölçüm kimliği**: 9a8caedd-1195-4cd5-80b4-a4c22f9302b8   
**Ölçüm adı**: S15   
**Birim**: disk sayısı   
**Notları**: standart yönetilen Disk – 256 GB 

**Ölçüm kimliği**: 5938f8da-0ecd-4c48-8d5a-c7c6c23546be   
**Ölçüm adı**: S20   
**Birim**: disk sayısı      
**Notları**: standart yönetilen Disk-512 GB 

**Ölçüm kimliği**: 7705a158-bd8b-4b2b-b4c2-0782343b81e6   
**Ölçüm adı**: S30   
**Birim**: disk sayısı   
**Notları**: standart yönetilen Disk – 1024 GB 

**Ölçüm kimliği**: d9aac1eb-a5d1-42f2-b617-9e3ea94fed88   
**Ölçüm adı**: S40   
**Birim**: disk sayısı   
**Notları**: standart yönetilen Disk – 2048 GB 

**Ölçüm kimliği**: a54899dd-458e-4a40-9abd-f57cafd936a7   
**Ölçüm adı**: S50   
**Birim**: disk sayısı   
**Notları**: standart yönetilen Disk – 4096 GB 

**Ölçüm kimliği**: 5c105f5f-cbdf-435c-b49b-3c7174856dcc   
**Ölçüm adı**: P4   
**Birim**: disk sayısı   
**Notları**: Premium yönetilen Disk-32 GB 

**Ölçüm kimliği**: 518b412b-1927-4f25-985f-4aea24e55c4f   
**Ölçüm adı**: P6   
**Birim**: disk sayısı   
**Notları**: Premium yönetilen Disk – 64 GB 

**Ölçüm kimliği**: 5cfb1fed-0902-49e3-8217-9add946fd624   
**Ölçüm adı**: P10   
**Birim**: disk sayısı   
**Notları**: Premium yönetilen Disk – 128 GB  

**Ölçüm kimliği**: 8de91c94-f740-4d9a-b665-bd5974fa08d4   
**Ölçüm adı**: P15  
**Birim**: disk sayısı   
**Notları**: Premium yönetilen Disk – 256 GB 

**Ölçüm kimliği**: c7e7839c-293b-4761-ae4c-848eda91130b   
**Ölçüm adı**: P20   
**Birim**: disk sayısı   
**Notları**: Premium yönetilen Disk-512 GB 

**Ölçüm kimliği**: 9f502103-adf4-4488-b494-456c95d23a9f   
**Ölçüm adı**: P30   
**Birim**: disk sayısı   
**Notları**: Premium yönetilen Disk – 1024 GB 

**Ölçüm kimliği**: 043757fc-049f-4e8b-8379-45bb203c36b1   
**Ölçüm adı**: P40   
**Birim**: disk sayısı    
**Notları**: Premium yönetilen Disk – 2048 GB 

**Ölçüm kimliği**: c0342c6f-810b-4942-85d3-6eaa561b6570   
**Ölçüm adı**: P50   
**Birim**: disk sayısı   
**Notları**: Premium yönetilen Disk – 4096 GB 

**Ölçüm kimliği**: 8a409390-1913-40ae-917b-08d0f16f3c38   
**Ölçüm adı**: ActualStandardDiskSize   
**Birim**: bayt      
**Notları**: standart yönetilen disk diskte gerçek boyut  

**Ölçüm kimliği**: 1273b16f-8458-4c34-8ce2-a515de551ef6  
**Ölçüm adı**: ActualPremiumDiskSize   
**Birim**: bayt      
**Notları**: Premium disk gerçek boyutuna yönetilen disk 

**Ölçüm kimliği**: 89009682-df7f-44fe-aeb1-63fba3ddbf4c  
**Ölçüm adı**: ActualStandardSnapshotSize   
**Birim**: bayt   
**Notları**: standart yönetilen anlık görüntü diskte gerçek boyut.  

**Ölçüm kimliği**: 95b0c03f-8a82-4524-8961-ccfbf575f536   
**Ölçüm adı**: ActualPremiumSnapshotSize   
**Birim**: bayt   
**Notları**: Premium yönetilen disk üzerinde gerçek boyut.   

### <a name="sql-rp"></a>SQL RP
  
**Ölçüm kimliği**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**Ölçüm adı**: DatabaseSizeHourSqlMeter  
**Birim**: MB\*saat  
**Notları**: toplam DB kapasitede oluşturma. ' % S'kullanım API'si ile bir günlük ayrıntı düzeyi çağırırsanız, ölçüm saat sayı ile çarpılan MB döndürür.  
  
### <a name="mysql-rp"></a>MySql RP   
  
**Ölçüm kimliği**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Ölçüm adı**: DatabaseSizeHourMySqlMeter  
**Birim**: MB\*saat  
**Notları**: toplam DB kapasitede oluşturma. ' % S'kullanım API'si ile bir günlük ayrıntı düzeyi çağırırsanız, ölçüm saat sayı ile çarpılan MB döndürür.    
### <a name="key-vault"></a>Key Vault   
  
**Ölçüm kimliği**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Ölçüm adı**: anahtar kasası işlemleri  
**Birim**: istek sayısı 10'da, 000's  
**Notları**: Key Vault veri düzlemi tarafından alınan numarası, REST API istekleri.  
  
**Ölçüm kimliği**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Ölçüm adı**: Gelişmiş anahtarlar işlemleri  
**Birim**: 10 bin işlemleri  
**Notları**: RSA 3 K/4 K, ECC anahtar işlemleri. (Önizleme).  
  
### <a name="app-service"></a>App Service   
  
**Ölçüm kimliği**: 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**Ölçüm adı**: App Service  
**Birim**: sanal çekirdek saatleri  
**Notları**: app service'ı çalıştırmak için kullanılan sanal çekirdek sayısı. Not: Microsoft Azure Stack üzerinde App Service'te kaydedilecek Bu ölçüm kullanır. Bulut hizmeti sağlayıcıları kiracıları için kullanım hesaplamak için bir App Service, ölçümleri (aşağıda) kullanabilirsiniz.  
  
**Ölçüm kimliği**: 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE  
**Ölçüm adı**: işlev istekleri  
**Birim**: 10 istekleri  
**Notları**: (başına 10 yürütme) istenen yürütmelerin toplam sayısı. Yürütme sayısı, bir olaya yanıt olarak çalışır veya bağlamalar tarafından tetiklenen bir işlev her zaman sayılır.  
  
**Ölçüm kimliği**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Ölçüm adı**: İşlevler - işlem  
**Birim**: GB-s  
**Notları**: kaynak tüketimi, gigabayt saniye (GB/s) cinsinden ölçülür. **Gözlemlenen kaynak tüketimini** alması işlevi yürütme için milisaniye olarak süre GB cinsinden ortalama bellek boyutu çarpılmasıyla hesaplanır. İşlev tarafından kullanılan bellek, yürütme süresi hesaplanır 1.536 MB maksimum bellek boyutu kadar yakın 128 MB kadar yuvarlama tarafından ölçülür kadar yakın 1 ms. İlişkin minimum yürütme süresi ve bellek tek bir işlev yürütmeye olan 100 ms ve 128 mb sırasıyla.  
  
**Ölçüm kimliği**: 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**Ölçüm adı**: paylaşılan uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notları**: saat kullanımını parça App Service planı başına. Planları uygulama başına temelinde ölçülür.  
  
**Ölçüm kimliği**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**Ölçüm adı**: ücretsiz App Service saatleri  
**Birim**: 1 saat  
**Notları**: saat kullanımı ücretsiz App Service planı başına. Planları uygulama başına temelinde ölçülür.  
  
**Ölçüm kimliği**: 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**Ölçüm adı**: küçük standart uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notları**: göre boyutu ve örnek sayısı hesaplanır.  
  
**Ölçüm kimliği**: 83A2A13E-4788-78DD-5D55-2831B68ED825  
**Ölçüm adı**: Orta Standart uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notları**: göre boyutu ve örnek sayısı hesaplanır.  
  
**Ölçüm kimliği**: 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**Ölçüm adı**: standart büyük uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notları**: göre boyutu ve örnek sayısı hesaplanır.  
  
### <a name="custom-worker-tiers"></a>Özel çalışan katmanları   
  
**Ölçüm kimliği**: *özel çalışan katmanları*  
**Ölçüm adı**: özel çalışan katmanları  
**Birim**: saat  
**Notları**: belirleyici ölçüm kimliği, SKU ve özel çalışan katmanı adı göre oluşturulur. Bu ölçüm kimliği, her özel çalışan katmanı için benzersizdir.  
  
**Ölçüm kimliği**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**Ölçüm adı**: SNI SSL  
**Birim**: SNI SSL bağlaması başına  
**Notları**: App Service iki tür SSL bağlantısını destekler: sunucu adı belirtme (SNI) SSL bağlantıları ve IP adresi SSL bağlantıları. SNI tabanlı SSL modern tarayıcılarda çalışırken IP tabanlı SSL tüm tarayıcılarda çalışır.  
  
**Ölçüm kimliği**: 60B42D72-DC1C-472C-9895-6C516277EDB4  
**Ölçüm adı**: IP SSL  
**Birim**: başına IP temelli SSL bağlaması  
**Notları**: App Service iki tür SSL bağlantısını destekler: sunucu adı belirtme (SNI) SSL bağlantıları ve IP adresi SSL bağlantıları. SNI tabanlı SSL modern tarayıcılarda çalışırken IP tabanlı SSL tüm tarayıcılarda çalışır.  
  
**Ölçüm kimliği**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Ölçüm adı**: Web işlemi  
**Birim**:  
**Notları**: etkin site başına saatlik olarak hesaplanır.  
  
**Ölçüm kimliği**: 5887D39B-0253-4E12-83C7-03E1A93DFFD9  
**Ölçüm adı**: dış Çıkış bant genişliği  
**Birim**: GB  
**Notları**: toplam gelen istek yanıtı bayt sayısı ve toplam giden istek bayt + toplam gelen FTP istek yanıtı bayt sayısı ve toplam gelen web dağıtma istek yanıtı bayt sayısı.  
  

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Bunu nasıl API'leri karşılaştırmak için Azure Stack kullanım [Azure kullanım API'si](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (şu anda genel önizlemede)?
* Kiracı kullanım API'si, bir özel durum ile bir Azure API tutarlıdır: *showDetails* bayrağı şu anda desteklenmeyen Azure Stack'te.
* Sağlayıcı kullanım API'si, yalnızca Azure Stack için geçerlidir.
* Şu anda [RateCard API'si](https://msdn.microsoft.com/library/azure/mt219004.aspx) diğer bir deyişle azure'da kullanılabilir Azure Stack'te kullanılabilir değil.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Kullanım ve bildirilen saat arasındaki fark nedir?
Kullanım verileri raporlar, iki ana saat değerleri vardır:

* **Bildirilen süresi**. Ne zaman kullanım olayı kullanım sistemiyle girilen saati
* **Kullanım süresi**. Azure Stack kaynak tüketildiğinde içinde zaman

Belirli kullanım olayı için kullanım süresi ve bildirilen süresi için değerlerin bir tutarsızlık görebilirsiniz. Gecikme, her türlü ortamda birden fazla saat uzunluğunda olabilir.

Şu anda yalnızca sorgulayabilirsiniz *bildirilen zaman*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Bu kullanım API'si hata kodları ne anlama gelir?
| **HTTP durum kodu** | **Hata kodu** | **Açıklama** |
| --- | --- | --- |
| 400/Hatalı istek |*NoApiVersion* |*Api sürümü* sorgu parametresi eksik. |
| 400/Hatalı istek |*InvalidProperty* |Bir özellik eksik veya geçersiz bir değere sahip. İleti hata kodu ile yanıt gövdesi içinde eksik özellik tanımlar. |
| 400/Hatalı istek |*RequestEndTimeIsInFuture* |Değeri *ReportedEndTime* gelecekte. Değerleri gelecekte bu bağımsız değişken için izin verilmez. |
| 400/Hatalı istek |*SubscriberIdIsNotDirectTenant* |Sağlayıcısı API çağrısı, arayanın geçerli bir kiracı değil bir abonelik kimliği kullandı. |
| 400/Hatalı istek |*SubscriptionIdMissingInRequest* |Arayanın abonelik kimliği eksik. |
| 400/Hatalı istek |*InvalidAggregationGranularity* |Bir geçersiz toplama ayrıntı düzeyi istendi. Günlük ve saatlik değerler geçerlidir. |
| 503 |*ServiceUnavailable* |Çağrı kısıtlanan veya Hizmet meşgul olduğundan yeniden denenebilir bir hata oluştu. |

## <a name="next-steps"></a>Sonraki Adımlar
[Müşteri faturalandırma ve Azure stack'teki geri ödeme](azure-stack-billing-and-chargeback.md)

[Sağlayıcı kaynak kullanım API'si](azure-stack-provider-resource-api.md)

[Kiracı kaynak kullanım API'si](azure-stack-tenant-resource-usage-api.md)
