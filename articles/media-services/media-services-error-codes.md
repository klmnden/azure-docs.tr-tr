---
title: "Azure Media Services hata kodları | Microsoft Docs"
description: "Konu Azure Media Services hata kodları genel bir bakış sağlar."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d3a62a64-7608-4b17-8667-479b26ba0d6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 39886a955124429302609dd9d5a7c20ae7f498d9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="azure-media-services-error-codes"></a>Azure Media Services hata kodları
Microsoft Azure Media Services kullanırken, Media Services desteklenmeyen Eylemler süresinin dolmasını kimlik doğrulama belirteçleri gibi sorunları bağlı olarak hizmetinden HTTP hata kodları alabilirsiniz. Bir listesi aşağıda verilmiştir **HTTP hata kodları** , döndürülüp döndürülmediğini Media Services ve olası nedenleri tarafından bunlar için.  

## <a name="400-bad-request"></a>400 Hatalı istek
İstek geçersiz bilgiler içerir ve aşağıdaki nedenlerden biri reddedilemiyor:

* Desteklenmeyen bir API sürümü belirtildi. En güncel sürümü için bkz: [Media Services REST API geliştirme için Kurulum](media-services-rest-how-to-use.md).
* Media Services API sürümü belirtilmedi. API sürümü belirtme hakkında daha fazla bilgi için bkz: [Media Services Operations REST API Başvurusu](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).
  
  > [!NOTE]
  > Media Services'e bağlanmak için .NET veya Java SDK'ları kullanıyorsanız, ne zaman deneyin ve Media Services karşı bazı eylemler gerçekleştirme API sürümü sizin için belirtilir.
  > 
  > 
* Tanımlanmamış özellik belirtilmedi. Hata iletisinde özelliği adıdır. Belirli bir varlık üyeleri olan özellikler belirtilebilir. Bkz: [Azure Media Services REST API Başvurusu](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) varlıkları ve özelliklerinin listesi.
* Geçersiz bir özellik değeri belirtildi. Hata iletisinde özelliği adıdır. Geçerli bir özellik türlerini ve değerlerini önceki bağlantısına bakın.
* Bir özellik değeri eksik ve gereklidir.
* Belirtilen URL parçası hatalı bir değer içeriyor.
* WriteOnce özelliği güncelleştirmek için girişimde bulunuldu.
* Bir giriş varlığı belirtilmedi veya belirlenemedi birincil bir AssetFile sahip bir iş oluşturmak için girişimde bulunuldu.
* Bir SAS Bulucu güncelleştirmek için girişimde bulunuldu. SAS bulucular yalnızca oluşturulan veya silinebilir. Bulucular akış güncelleştirilebilir. Daha fazla bilgi için bkz: [Bulucular](https://docs.microsoft.com/rest/api/media/operations/locator).
* Desteklenmeyen bir işlem veya sorgu gönderildi.

## <a name="401-unauthorized"></a>401 Yetkisiz
(Bunu yetkilendirilebilir önce) istek aşağıdaki nedenlerden biri dolayısıyla doğrulanamadı:

* Kimlik doğrulama üstbilgisi eksik.
* Hatalı kimlik doğrulaması üstbilgi değeri.
  * Belirtecin süresi sona erdi. 
  * Belirteç geçersiz bir imza içeriyor.

## <a name="403-forbidden"></a>403 Yasak
İstek aşağıdaki nedenlerden biri dolayısıyla izin verilmiyor:

* Media Services hesabı bulunamadı veya silinmiş olabilir.
* Media Services hesabı devre dışı bırakılır ve HTTP GET isteği türü değil. Hizmet işlemleri de 403 bir yanıt döndürür.
* Kimlik doğrulama belirteci kullanıcının kimlik bilgileri içermiyor: AccountName ve/veya Subscriptionıd. Azure Yönetim Portalı'nda Media Services hesabınız için medya Hizmetleri UI uzantısı'nda bu bilgileri bulabilirsiniz.
* Kaynak erişilemez.
  
  * Media Services hesabınız için kullanılabilir olmayan bir MediaProcessor kullanmak için girişimde bulunuldu.
  * Media Services tarafından tanımlanan bir JobTemplate güncelleştirmek için girişimde bulunuldu.
  * Bazı diğer Media Services hesabı Bulucu üzerine yazmak için girişimde bulunuldu.
  * Bazı diğer Media Services hesabı ContentKey üzerine yazmak için girişimde bulunuldu.
* Kaynak için Media Services hesabı ulaşıldı hizmet kotasını nedeniyle oluşturulamadı. Hizmeti kotaları hakkında daha fazla bilgi için bkz: [kotaları ve kısıtlamaları](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 Bulunamadı
İstek, aşağıdaki nedenlerin birinden dolayı bir kaynakta izin verilmiyor:

* Var olmayan bir varlığı güncelleştirmek için girişimde bulunuldu.
* Var olmayan bir varlığı silmek için girişimde bulunuldu.
* Var olmayan bir varlığa bağlanan bir varlık oluşturmak için girişimde bulunuldu.
* Var olmayan bir varlık almak için girişimde bulunuldu.
* Media Services hesabıyla ilişkilendirilmemiş bir depolama hesabı belirtmek için girişimde bulunuldu.  

## <a name="409-conflict"></a>409 çakışma
İstek aşağıdaki nedenlerden biri dolayısıyla izin verilmiyor:

* Birden fazla AssetFile varlık içinde belirtilen ada sahip.
* Varlık içindeki ikinci bir birincil AssetFile oluşturmak için girişimde bulunuldu.
* Zaten kullanılan belirtilen kimliğe bir ContentKey oluşturmak için girişimde bulunuldu.
* Zaten kullanılan belirtilen kimliğe bir Bulucu oluşturmanız için girişimde bulunuldu.
* Birden fazla IngestManifestFile IngestManifest içinde belirtilen ada sahip.
* Depolama şifrelenmiş varlık için ikinci bir depolama şifreleme ContentKey bağlamak için girişimde bulunuldu.
* Varlık için aynı ContentKey bağlamak için girişimde bulunuldu.
* Depolama kapsayıcısı eksik ya da artık varlıkla ilişkilendirilen bir varlık için bir Bulucu oluşturmanız için girişimde bulunuldu.
* 5 bulucular zaten kullanımda olan bir varlık için bir Bulucu oluşturmanız için girişimde bulunuldu. (Azure depolama sınırını bir depolama kapsayıcısı üzerinde beş paylaşılan erişim ilkeleri zorunlu tutar.)
* Bir malın depolama hesabı için bir IngestManifestAsset bağlama IngestManifest üst tarafından kullanılan depolama hesabı ile aynı değil.  

## <a name="500-internal-server-error"></a>500 İç sunucu hatası
İsteğin işlenmesi sırasında Media Services işleme devam etmesini engelleyen bazı hatayla karşılaşır. Bunun nedeni, aşağıdakilerden biri olabilir:

* Media Services hesabın hizmet kota bilgileri geçici olarak kullanılamadığından bir varlık veya iş oluşturma başarısız olur.
* Hesabın depolama hesabı bilgilerini geçici olarak kullanılamadığından bir varlık veya IngestManifest blob depolama kapsayıcısını oluşturma başarısız olur.
* Diğer beklenmeyen hata oluştu.

## <a name="503-service-unavailable"></a>503 Hizmet kullanılamıyor
Sunucu isteklerini almak şu anda alamıyor. Bu hatanın nedeni aşırı isteklerine hizmet. Media Services mekanizması azaltma hizmete aşırı istekte uygulamalar için kaynak kullanımını kısıtlar.

> [!NOTE]
> Hata iletisi ve hata kodu dizesi 503 hatası aldığınız nedeni hakkında daha ayrıntılı bilgi almak için denetleyin. Bu hata, azaltma her zaman gelmez.
> 
> 

Olası durum açıklamaları şunlardır:

* "Sunucu meşgul. Bu istek türü önceki çalışmalarını birden çok {0} saniye sürdü."
* "Sunucu meşgul. Birden çok saniyede {0} istekler kısıtlanan."
* "Sunucu meşgul. Fazlasını {0} istekleri {1} saniye içinde kısıtlanan."

Bu hatayı işlemeye üstel geri alma yeniden deneme mantığı kullanmanızı öneririz. Ardışık hata yanıtları denemeler arasındaki aşamalı olarak uzun bekler kullanarak anlamına gelir.  Daha fazla bilgi için bkz: [geçici hata işleme uygulama blok](https://msdn.microsoft.com/library/hh680905.aspx).

> [!NOTE]
> Kullanıyorsanız [.Net için Azure Media Services SDK](https://github.com/Azure/azure-sdk-for-media-services/tree/master), 503 hatası için yeniden deneme mantığı SDK tarafından uygulanır.  
> 
> 

## <a name="see-also"></a>Ayrıca Bkz.
[Yönetim hata kodları medya Hizmetleri](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

