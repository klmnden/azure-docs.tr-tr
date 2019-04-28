---
title: Azure Media Services hata kodları | Microsoft Docs
description: Konu Azure Media Services hata kodları genel bir bakış sağlar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: d3a62a64-7608-4b17-8667-479b26ba0d6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: f3c362730e7908e88b363659b7fa580b6f2cddf1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61217242"
---
# <a name="azure-media-services-error-codes"></a>Azure Media Services hata kodları
Microsoft Azure Media Services'ı kullanarak, HTTP hata kodları hizmete kimlik doğrulama belirteçlerinizi Media Services'da desteklenmez eylemlerine sona erecek gibi sorunlar bağlı olarak alabilirsiniz. Bir listesi verilmiştir **HTTP hata kodları** , döndürülmesi olası nedenleri ve Media Services ile bunları için.  

## <a name="400-bad-request"></a>400 Hatalı istek
İstek geçersiz bilgiler içerir ve aşağıdaki nedenlerden biri nedeniyle reddedildi:

* Desteklenmeyen API sürümü belirtildi. En güncel sürümü için bkz: [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).
* Media Services API sürümü belirtilmedi. API sürümünü belirtme hakkında daha fazla bilgi için bkz: [medya hizmetleri işlemlerini REST API Başvurusu](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).
  
  > [!NOTE]
  > Media Services'e bağlanmak için .NET veya Java SDK'ları kullanıyorsanız, API sürümü deneyin ve Media Services karşı bazı eylemler gerçekleştirme sizin için belirtilir.
  > 
  > 
* Tanımlanmamış bir özellik belirtilmedi. Hata iletisinde özelliği adıdır. Belirli bir varlık üyesi olan özellikler belirtilebilir. Bkz: [Azure Media Services REST API Başvurusu](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) varlıkları ve özelliklerinin listesi.
* Geçersiz özellik değeri belirtildi. Hata iletisinde özelliği adıdır. Geçerli özellik türleri ve değerleri için önceki bağlantısına bakın.
* Bir özellik değeri eksik ve gereklidir.
* Belirtilen URL parçası, hatalı bir değer içeriyor.
* WriteOnce özelliğini güncelleştirmek için girişimde bulunuldu.
* Bir giriş varlığı belirtilmedi veya belirlenemedi birincil bir AssetFile ile sahip bir iş oluşturmak için girişimde bulunuldu.
* SAS Bulucunun güncelleştirmek için girişimde bulunuldu. SAS bulucuları yalnızca oluşturulan veya silinebilir. Akış bulucuları güncelleştirilebilir. Daha fazla bilgi için [Bulucular](https://docs.microsoft.com/rest/api/media/operations/locator).
* Desteklenmeyen bir işlem veya sorgu gönderildi.

## <a name="401-unauthorized"></a>401 Yetkisiz
(Bunu yetkilendirilebilir önce) istek aşağıdakilerden biri nedeniyle doğrulanamadı:

* Kimlik doğrulama üst bilgisi eksik.
* Hatalı kimlik doğrulaması üstbilgi değeri.
  * Belirtecin süresi doldu. 
  * Belirteç geçersiz bir imza içeriyor.

## <a name="403-forbidden"></a>403 Yasak
İstek aşağıdaki nedenlerden biri nedeniyle izin verilmez:

* Media Services hesabı bulunamadı veya silinmiş olabilir.
* Media Services hesabı devre dışı bırakılır ve HTTP GET isteği türü değil. Hizmet işlemleri de 403 bir yanıt döndürür.
* Kimlik doğrulaması belirteci, kullanıcının kimlik bilgilerini içermiyor: AccountName ve/veya Subscriptionıd. Bu bilgiler, Azure Yönetim Portalı'nda Media Services hesabınız için medya Hizmetleri kullanıcı Arabirimi uzantısı'nda bulabilirsiniz.
* Kaynağa erişilemiyor.
  
  * Media Services hesabınız için kullanılabilir olmayan bir MediaProcessor kullanmak için girişimde bulunuldu.
  * Media Services tarafından tanımlanan bir JobTemplate güncelleştirmek için girişimde bulunuldu.
  * Bazı diğer Media Services hesabı Bulucu üzerine yazmak için girişimde bulunuldu.
  * Bazı diğer Media Services hesabının ContentKey üzerine yazmak için girişimde bulunuldu.
* Kaynak için Media Services hesabı ulaşıldığında hizmet kotası nedeniyle oluşturulamadı. Hizmet kotaları hakkında daha fazla bilgi için bkz. [kotaları ve sınırlamaları](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 Bulunamadı
İstek, aşağıdaki nedenlerden biri nedeniyle bir kaynakta izin verilmiyor:

* Mevcut bir varlığı güncelleştirmek için girişimde bulunuldu.
* Mevcut bir varlığı silmek için girişimde bulunuldu.
* Var olmayan bir varlığa bağlanan bir varlık oluşturmak için girişimde bulunuldu.
* Varolmayan bir varlık almak için girişimde bulunuldu.
* Media Services hesabıyla ilişkili olmayan bir depolama hesabı belirtmek için girişimde bulunuldu.  

## <a name="409-conflict"></a>409 çakışma
İstek aşağıdaki nedenlerden biri nedeniyle izin verilmez:

* Birden fazla AssetFile varlık içinde belirtilen ada sahip.
* Varlık içinde ikinci bir birincil AssetFile oluşturmak için girişimde bulunuldu.
* Zaten kullanılan belirtilen kimliğe sahip bir ContentKey oluşturmak için girişimde bulunuldu.
* Zaten kullanılan belirtilen kimliğe sahip bir Bulucu oluşturmanız için girişimde bulunuldu.
* Birden fazla IngestManifestFile IngestManifest içinde belirtilen ada sahip.
* İkinci depolama şifrelemesi ContentKey depolama şifrelenmiş varlığına bağlamak için girişimde bulunuldu.
* Varlık için aynı ContentKey bağlamak için girişimde bulunuldu.
* Depolama kapsayıcısını eksik veya artık varlıkla ilişkili olmayan bir varlığa bir Bulucu oluşturmanız için girişimde bulunuldu.
* Zaten 5 bulucular kullanımda olan bir varlığa bir Bulucu oluşturmanız için girişimde bulunuldu. (Azure depolama sınırını bir depolama kapsayıcısı üzerinde beş paylaşılan erişim ilkeleri zorunlu kılar.)
* Bir varlığın depolama hesabı için bir IngestManifestAsset bağlama IngestManifest üst öğe tarafından kullanılan depolama hesabı ile aynı değil.  

## <a name="500-internal-server-error"></a>500 İç Sunucu Hatası
İsteğin işlenmesi sırasında Media Services işleme devam etmesini engelleyen bazı hatayla karşılaşır. Bunun nedeni, aşağıdakilerden biri olabilir:

* Media Services hesap hizmet kota bilgileri geçici olarak kullanılamadığı için bir varlık veya işi oluşturma başarısız olur.
* Hesabın depolama hesabı bilgileri geçici olarak kullanılamadığı için bir varlık veya IngestManifest blob depolama kapsayıcısı oluşturma başarısız olur.
* Diğer beklenmeyen hata oluştu.

## <a name="503-service-unavailable"></a>503 Hizmet Kullanılamıyor
Sunucu isteklerini almak şu anda belirlenemiyor. Bu hatanın nedeni aşırı isteklerine hizmet. Media Services: azaltma mekanizması, aşırı uzun istek hizmetinize uygulamalar için kaynak kullanımını kısıtlıyor.

> [!NOTE]
> Hata iletisi ve hata kodu dizesi nedeni hakkında daha ayrıntılı bilgi 503 hatası aldığınız almak için denetleyin. Bu hata, azaltma her zaman gelmez.
> 
> 

Olası durum açıklamalar şunlardır:

* "Sunucu meşgul. Önceki çalıştırmaları, bu tür bir istek aldı birden fazla {0} saniye. "
* "Sunucu meşgul. Birden fazla {0} saniye başına istek kısıtlanmış. "
* "Sunucu meşgul. Birden fazla {0} içinde istekleri {1} saniye kısıtlanmış. "

Bu hatayı işlemek için üstel geri alma yeniden deneme mantığı kullanmanızı öneririz. Art arda hata yanıtları için yeniden denemeler arasındaki aşamalı olarak uzun bekler kullanarak anlamına gelir.  Daha fazla bilgi için [geçici hata işleme uygulama bloğu](https://msdn.microsoft.com/library/hh680905.aspx).

> [!NOTE]
> Kullanıyorsanız [.Net için Azure Media Services SDK](https://github.com/Azure/azure-sdk-for-media-services/tree/master), SDK tarafından 503 hatası için yeniden deneme mantığı uygulanmıştır.  
> 
> 

## <a name="see-also"></a>Ayrıca Bkz.
[Media Services'ı Yönetim hata kodları](https://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

