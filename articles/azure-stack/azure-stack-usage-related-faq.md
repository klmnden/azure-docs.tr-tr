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
ms.date: 02/19/2019
ms.author: mabrigg
ms.reviewer: alfredop
ms.lastreviewed: 11/08/2018
ms.openlocfilehash: 92774592f86a71a8482fd3d44eca404fcf2d4e6e
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429562"
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Sık sorulan sorular, Azure Stack kullanım API'si

Bu makalede, Azure Stack kullanım API'si hakkında sık sorulan bazı sorular yanıtlanmaktadır.

## <a name="what-meter-ids-can-i-see"></a>Hangi ölçüm kimlikleri görebilirim?
Aşağıdaki kaynak sağlayıcıları için kullanımı bildirilir:

### <a name="network"></a>Ağ
  
**Ölçüm kimliği**: F271A8A388C44D93956A063E1D2FA80B  
**Ölçüm adı**: Statik IP adresi kullanımı  
**Birim**: IP adresleri  
**Notları**: Kullanılan sayısı IP adresleri. ' % S'kullanım API'si ile bir günlük ayrıntı düzeyi çağırırsanız, ölçüm saat sayı ile çarpılan IP adresini döndürür.  
  
**Ölçüm kimliği**: 9E2739BA86744796B465F64674B822BA  
**Ölçüm adı**: Dinamik IP adresi kullanımı  
**Birim**: IP adresleri  
**Notları**: Kullanılan sayısı IP adresleri. ' % S'kullanım API'si ile bir günlük ayrıntı düzeyi çağırırsanız, ölçüm saat sayı ile çarpılan IP adresini döndürür.  
  
### <a name="storage"></a>Depolama
  
**Ölçüm kimliği**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Ölçüm adı**: TableCapacity  
**Birim**: GB\*saat  
**Notları**: Tabloları tarafından kullanılan toplam kapasite.  
  
**Ölçüm kimliği**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Ölçüm adı**: PageBlobCapacity  
**Birim**: GB\*saat  
**Notları**: Sayfa blobları tarafından kullanılan toplam kapasite.  
  
**Ölçüm kimliği**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Ölçüm adı**: QueueCapacity  
**Birim**: GB\*saat  
**Notları**: Kuyruk tarafından kullanılan toplam kapasite.  
  
**Ölçüm kimliği**: 09F8879E-87E9-4305-A572-4B7BE209F857  
**Ölçüm adı**: BlockBlobCapacity  
**Birim**: GB\*saat  
**Notları**: Blok BLOB'ları tarafından kullanılan toplam kapasite.  
  
**Ölçüm kimliği**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Ölçüm adı**: TableTransactions  
**Birim**: 10, 000's istek sayısı  
**Notları**: Tablo hizmeti isteklerinde (10,000s).  
  
**Ölçüm kimliği**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Ölçüm adı**: TableDataTransIn  
**Birim**: Giriş verileri GB  
**Notları**: Tablo hizmeti veri girişi GB.  
  
**Ölçüm kimliği**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**Ölçüm adı**: TableDataTransOut  
**Birim**: GB çıkış  
**Notları**: Tablo hizmeti veri çıkışı GB  
  
**Ölçüm kimliği**: 43DAF82B-4618-444A-B994-40C23F7CD438  
**Ölçüm adı**: BlobTransactions  
**Birim**: İstekleri 10'da 000's sayısı.  
**Notları**: BLOB hizmeti isteklerinin (içinde 10,000s).  
  
**Ölçüm kimliği**: 9764F92C-E44A-498E-8DC1-AAD66587A810  
**Ölçüm adı**: BlobDataTransIn  
**Birim**: Giriş verileri GB  
**Notları**: BLOB hizmeti veri girişi GB.  
  
**Ölçüm kimliği**: 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**Ölçüm adı**: BlobDataTransOut  
**Birim**: GB çıkış  
**Notları**: BLOB hizmeti GB veri çıkışı.  
  
**Ölçüm kimliği**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Ölçüm adı**: QueueTransactions  
**Birim**: İstekleri 10'da 000's sayısı.  
**Notları**: Kuyruk hizmet isteklerini (10,000s).  
  
**Ölçüm kimliği**: E518E809-E369-4A45-9274-2017B29FFF25  
**Ölçüm adı**: QueueDataTransIn  
**Birim**: Giriş verileri GB  
**Notları**: Kuyruk hizmeti veri girişi GB.  
  
**Ölçüm kimliği**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Ölçüm adı**: QueueDataTransOut  
**Birim**: GB çıkış  
**Notları**: Kuyruk hizmeti veri çıkışı GB  

### <a name="compute"></a>İşlem 
  
**Ölçüm kimliği**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Ölçüm adı**: Temel VM boyutu saatleri  
**Birim**: Sanal çekirdek saatleri  
**Notları**: Sanal çekirdek sayısı VM çalıştıran saat ile çarpılır.  
  
**Ölçüm kimliği**: 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**Ölçüm adı**: Windows VM boyutu saatleri  
**Birim**: Sanal çekirdek saatleri  
**Notları**: Sanal çekirdek sayısı olarak VM çalıştıran saat çarpılır.  
  
**Ölçüm kimliği**: 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**Ölçüm adı**: VM boyutu saatleri  
**Birim**: VM saatleri  
**Notları**: Hem temel hem de Windows VM yakalar. Çekirdekler için ayarlanmaz.  
  
### <a name="managed-disks"></a>Yönetilen Diskler

**Ölçüm kimliği**: 380874f9-300c-48e0-95a0-d2d9a21ade8f   
**Ölçüm adı**: S4   
**Birim**: Count of Disks\*ay   
**Notları**: Standart yönetilen Disk-32 GB 

**Ölçüm kimliği**: 1b77d90f-427b-4435-b4f1-d78adec53222   
**Ölçüm adı**: S6   
**Birim**: Count of Disks\*ay   
**Notları**: Standart yönetilen Disk – 64 GB 

**Ölçüm kimliği**: d5f7731b-f639-404a-89d0-e46186e22c8d   
**Ölçüm adı**: S10   
**Birim**: Count of Disks\*ay   
**Notları**: Standart yönetilen Disk – 128 GB 

**Ölçüm kimliği**: ff85ef31-da5b-4eac-95dd-a69d6f97b18a   
**Ölçüm adı**: S15   
**Birim**: Count of Disks\*ay   
**Notları**: Standart yönetilen Disk – 256 GB 

**Ölçüm kimliği**: 88ea9228-457a-4091-adc9-ad5194f30b6e   
**Ölçüm adı**: S20   
**Birim**: Count of Disks\*ay      
**Notları**: Standart yönetilen Disk-512 GB 

**Ölçüm kimliği**: 5b1db88a-8596-4002-8052-347947c26940   
**Ölçüm adı**: S30   
**Birim**: Count of Disks\*ay   
**Notları**: Standart yönetilen Disk – 1024 GB 

**Ölçüm kimliği**: 7660b45b-b29d-49cb-b816-59f30fbab011   
**Ölçüm adı**: P4   
**Birim**: Count of Disks\*ay   
**Notları**: Premium yönetilen Disk-32 GB 

**Ölçüm kimliği**: 817007fd-a077-477f-bc01-b876f27205fd   
**Ölçüm adı**: P6   
**Birim**: Count of Disks\*ay   
**Notları**: Premium yönetilen Disk – 64 GB 

**Ölçüm kimliği**: e554b6bc-96cd-4938-a5b5-0da990278519   
**Ölçüm adı**: P10   
**Birim**: Count of Disks\*ay   
**Notları**: Premium yönetilen Disk – 128 GB  

**Ölçüm kimliği**: cdc0f53a-62a9-4472-a06c-e99a23b02907   
**Ölçüm adı**: P15  
**Birim**: Count of Disks\*ay   
**Notları**: Premium yönetilen Disk – 256 GB 

**Ölçüm kimliği**: b9cb2d1a-84c2-4275-aa8b-70d2145d59aa   
**Ölçüm adı**: P20   
**Birim**: Count of Disks\*ay   
**Notları**: Premium yönetilen Disk-512 GB 

**Ölçüm kimliği**: 06bde724-9f94-43c0-84c3-d0fc54538369   
**Ölçüm adı**: P30   
**Birim**: Count of Disks\*ay   
**Notları**: Premium yönetilen Disk – 1024 GB 

**Ölçüm kimliği**: 7ba084ec-ef9c-4d64-a179-7732c6cb5e28   
**Ölçüm adı**: ActualStandardDiskSize   
**Birim**: GB\*ay      
**Notları**: Standart yönetilen disk diskte gerçek boyut  

**Ölçüm kimliği**: daef389a-06e5-4684-a7f7-8813d9f792d5  
**Ölçüm adı**: ActualPremiumDiskSize   
**Birim**: GB\*ay      
**Notları**: Yönetilen disk gerçek Premium disk boyutu 

**Ölçüm kimliği**: 75d4b707-1027-4403-9986-6ec7c05579c8  
**Ölçüm adı**: ActualStandardSnapshotSize   
**Birim**: GB\*ay   
**Notları**: Standart yönetilen anlık görüntü diskte gerçek boyutu.  

**Ölçüm kimliği**: 5ca1cbb9-6f14-4e76-8be8-1ca91547965e   
**Ölçüm adı**: ActualPremiumSnapshotSize   
**Birim**: GB\*ay   
**Notları**: Premium yönetilen disk üzerinde gerçek boyutu.   

**Ölçüm kimliği**: 5d76e09f-4567-452a-94cc-7d1f097761f0   
**Ölçüm adı**: S4   
**Birim**: Count of Disks\*saat   
**Notları**: Standart yönetilen Disk-32 GB (kullanım dışı) 

**Ölçüm kimliği**: dc9fc6a9-0782-432a-b8dc-978130457494   
**Ölçüm adı**: S6   
**Birim**: Count of Disks\*saat   
**Notları**: Standart yönetilen Disk – 64 GB (kullanım dışı) 

**Ölçüm kimliği**: e5572fce-9f58-49d7-840c-b168c0f01fff   
**Ölçüm adı**: S10   
**Birim**: Count of Disks\*saat   
**Notları**: Standart yönetilen Disk – 128 GB (kullanım dışı) 

**Ölçüm kimliği**: 9a8caedd-1195-4cd5-80b4-a4c22f9302b8   
**Ölçüm adı**: S15   
**Birim**: Count of Disks\*saat   
**Notları**: Standart yönetilen Disk – 256 GB (kullanım dışı) 

**Ölçüm kimliği**: 5938f8da-0ecd-4c48-8d5a-c7c6c23546be   
**Ölçüm adı**: S20   
**Birim**: Count of Disks\*saat      
**Notları**: Standart yönetilen Disk-512 GB (kullanım dışı) 

**Ölçüm kimliği**: 7705a158-bd8b-4b2b-b4c2-0782343b81e6   
**Ölçüm adı**: S30   
**Birim**: Count of Disks\*saat   
**Notları**: Standart yönetilen Disk – 1024 GB (kullanım dışı) 

**Ölçüm kimliği**: 5c105f5f-cbdf-435c-b49b-3c7174856dcc   
**Ölçüm adı**: P4   
**Birim**: Count of Disks\*saat   
**Notları**: Premium yönetilen Disk-32 GB (kullanım dışı) 

**Ölçüm kimliği**: 518b412b-1927-4f25-985f-4aea24e55c4f   
**Ölçüm adı**: P6   
**Birim**: Count of Disks\*saat   
**Notları**: Premium yönetilen Disk – 64 GB (kullanım dışı) 

**Ölçüm kimliği**: 5cfb1fed-0902-49e3-8217-9add946fd624   
**Ölçüm adı**: P10   
**Birim**: Count of Disks\*saat   
**Notları**: Premium yönetilen Disk – 128 GB (kullanım dışı)  

**Ölçüm kimliği**: 8de91c94-f740-4d9a-b665-bd5974fa08d4   
**Ölçüm adı**: P15  
**Birim**: Count of Disks\*saat   
**Notları**: Premium yönetilen Disk – 256 GB (kullanım dışı) 

**Ölçüm kimliği**: c7e7839c-293b-4761-ae4c-848eda91130b   
**Ölçüm adı**: P20   
**Birim**: Count of Disks\*saat   
**Notları**: Premium yönetilen Disk-512 GB (kullanım dışı) 

**Ölçüm kimliği**: 9f502103-adf4-4488-b494-456c95d23a9f   
**Ölçüm adı**: P30   
**Birim**: Count of Disks\*saat   
**Notları**: Premium yönetilen Disk – 1024 GB (kullanım dışı) 

**Ölçüm kimliği**: 8a409390-1913-40ae-917b-08d0f16f3c38   
**Ölçüm adı**: ActualStandardDiskSize   
**Birim**: Bayt\*saat      
**Notları**: Standart yönetilen disk (kullanım dışı) diskte gerçek boyut  

**Ölçüm kimliği**: 1273b16f-8458-4c34-8ce2-a515de551ef6  
**Ölçüm adı**: ActualPremiumDiskSize   
**Birim**: Bayt\*saat      
**Notları**: Gerçek Boyut Premium disk üzerindeki yönetilen disk (kullanım dışı) 

**Ölçüm kimliği**: 89009682-df7f-44fe-aeb1-63fba3ddbf4c  
**Ölçüm adı**: ActualStandardSnapshotSize   
**Birim**: Bayt\*saat   
**Notları**: Standart yönetilen anlık görüntü (kullanım dışı) diskte gerçek boyut 

**Ölçüm kimliği**: 95b0c03f-8a82-4524-8961-ccfbf575f536   
**Ölçüm adı**: ActualPremiumSnapshotSize   
**Birim**: Bayt\*saat   
**Notları**: Gerçek Boyut diskte yönetilen premium (kullanım dışı) 

### <a name="sql-rp"></a>Sql RP
  
**Ölçüm kimliği**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**Ölçüm adı**: DatabaseSizeHourSqlMeter  
**Birim**: MB\*saat  
**Notları**: Oluşturma sırasında toplam DB kapasitesi. ' % S'kullanım API'si ile bir günlük ayrıntı düzeyi çağırırsanız, ölçüm saat sayı ile çarpılan MB döndürür.  
  
### <a name="mysql-rp"></a>MySql RP   
  
**Ölçüm kimliği**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Ölçüm adı**: DatabaseSizeHourMySqlMeter  
**Birim**: MB\*saat  
**Notları**: Oluşturma sırasında toplam DB kapasitesi. ' % S'kullanım API'si ile bir günlük ayrıntı düzeyi çağırırsanız, ölçüm saat sayı ile çarpılan MB döndürür.    
### <a name="key-vault"></a>Key Vault   
  
**Ölçüm kimliği**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Ölçüm adı**: Anahtar kasası işlemleri  
**Birim**: 10, 000's istek sayısı  
**Notları**: Key Vault veri düzlemi tarafından alınan REST API isteklerinin sayısı.  
  
**Ölçüm kimliği**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Ölçüm adı**: Gelişmiş anahtarlar işlemleri  
**Birim**:  10 bin işlemleri  
**Notları**: RSA 3K / 4K, ECC anahtar işlemleri. (Önizleme).  
  
### <a name="app-service"></a>App Service   
  
**Ölçüm kimliği**: 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**Ölçüm adı**: App Service  
**Birim**: Sanal çekirdek saatleri  
**Notları**: App service'ı çalıştırmak için kullanılan sanal çekirdek sayısı. Not: Microsoft Azure Stack üzerinde App Service'te kaydedilecek Bu ölçüm kullanır. Bulut hizmeti sağlayıcıları kiracıları için kullanım hesaplamak için bir App Service, ölçümleri (aşağıda) kullanabilirsiniz.  
  
**Ölçüm kimliği**: 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE  
**Ölçüm adı**: İşlev istekleri  
**Birim**: 10 istekleri  
**Notları**: (Başına 10 yürütme) istenen yürütmelerin toplam sayısı. Yürütme sayısı, bir olaya yanıt olarak çalışır veya bağlamalar tarafından tetiklenen bir işlev her zaman sayılır.  
  
**Ölçüm kimliği**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Ölçüm adı**: İşlevler - işlem  
**Birim**:  GB-s  
**Notları**:  Kaynak tüketimi, gigabayt saniye (GB/s) cinsinden ölçülür. **Gözlemlenen kaynak tüketimini** alması işlevi yürütme için milisaniye olarak süre GB cinsinden ortalama bellek boyutu çarpılmasıyla hesaplanır. İşlev tarafından kullanılan bellek, yürütme süresi hesaplanır 1.536 MB maksimum bellek boyutu kadar yakın 128 MB kadar yuvarlama tarafından ölçülür kadar yakın 1 ms. İlişkin minimum yürütme süresi ve bellek tek bir işlev yürütmeye olan 100 ms ve 128 mb sırasıyla.  
  
**Ölçüm kimliği**: 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**Ölçüm adı**: Paylaşılan uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notları**: Parça App Service planı kullanımı saat. Planları uygulama başına temelinde ölçülür.  
  
**Ölçüm kimliği**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**Ölçüm adı**: Ücretsiz uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notları**: Saatlik kullanım ücretsiz App Service planı. Planları uygulama başına temelinde ölçülür.  
  
**Ölçüm kimliği**: 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**Ölçüm adı**: Standart kısa uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notları**: Boyutu ve örnek sayısını göre hesaplanır.  
  
**Ölçüm kimliği**: 83A2A13E-4788-78DD-5D55-2831B68ED825  
**Ölçüm adı**: Standart Orta uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notları**: Boyutu ve örnek sayısını göre hesaplanır.  
  
**Ölçüm kimliği**: 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**Ölçüm adı**: Standart uzun uygulama hizmeti saatleri  
**Birim**: 1 saat  
**Notları**: Boyutu ve örnek sayısını göre hesaplanır.  
  
### <a name="custom-worker-tiers"></a>Özel çalışan katmanları   
  
**Ölçüm kimliği**: *Özel çalışan katmanları*  
**Ölçüm adı**: Özel çalışan katmanları  
**Birim**: Saat  
**Notları**: Belirlenimci ölçüm kimliği, SKU ve özel çalışan katmanı adı göre oluşturulur. Bu ölçüm kimliği, her özel çalışan katmanı için benzersizdir.  
  
**Ölçüm kimliği**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**Ölçüm adı**: SNI SSL  
**Birim**: SNI SSL bağlaması başına  
**Notları**: App Service iki tür SSL bağlantısını destekler: Sunucu Adı Belirtme (SNI) SSL Bağlantıları ve IP Adresi SSL Bağlantıları. SNI tabanlı SSL modern tarayıcılarda çalışırken IP tabanlı SSL tüm tarayıcılarda çalışır.  
  
**Ölçüm kimliği**: 60B42D72-DC1C-472C-9895-6C516277EDB4  
**Ölçüm adı**: IP SSL  
**Birim**: IP temelli SSL bağlaması  
**Notları**: App Service iki tür SSL bağlantısını destekler: Sunucu Adı Belirtme (SNI) SSL Bağlantıları ve IP Adresi SSL Bağlantıları. SNI tabanlı SSL modern tarayıcılarda çalışırken IP tabanlı SSL tüm tarayıcılarda çalışır.  
  
**Ölçüm kimliği**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Ölçüm adı**:  Web İşlemi  
**Birim**:  
**Notları**: Etkin site başına saatlik hesaplanır.  
  
**Ölçüm kimliği**: 5887D39B-0253-4E12-83C7-03E1A93DFFD9  
**Ölçüm adı**: Dış Çıkış bant genişliği  
**Birim**: GB  
**Notları**: İstek yanıtı bayt sayısı, toplam gelen istek yanıtı bayt sayısı + istek bayt + toplam gelen FTP yanıt bayt + toplam gelen web toplam giden istek dağıtın.  
  

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsdocsmicrosoftcomazurebillingbilling-usage-rate-card-overviewazure-resource-usage-api-preview-currently-in-public-preview"></a>Bunu nasıl API'leri karşılaştırmak için Azure Stack kullanım [Azure kullanım API'si](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-usage-api-preview) (şu anda genel önizlemede)?
* Kiracı kullanım API'si, bir özel durum ile bir Azure API tutarlıdır: *showDetails* bayrağı şu anda desteklenmeyen Azure Stack'te.
* Sağlayıcı kullanım API'si, yalnızca Azure Stack için geçerlidir.
* Şu anda [RateCard API'si](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-ratecard-api-preview) diğer bir deyişle azure'da kullanılabilir Azure Stack'te kullanılabilir değil.

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

[Kiracı Kaynak Kullanım API’si](azure-stack-tenant-resource-usage-api.md)
