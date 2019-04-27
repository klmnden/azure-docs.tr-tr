---
title: İçerik anahtarı ilkelerinde medya Hizmetleri - Azure | Microsoft Docs
description: Bu makalede, içerik anahtarı ilkeleri nelerdir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/03/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: d9e86c45d535862e0c3d02b3f331bc40ebb7f6c7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733054"
---
# <a name="content-key-policies"></a>İçerik Anahtar İlkeleri

Media Services sayesinde, Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere.

Akışınız şifreleme seçeneklerini belirtmek için oluşturmanız gerekir [içerik anahtarı ilke](https://docs.microsoft.com/rest/api/media/contentkeypolicies) ilişkilendirin, **akış Bulucu**. **İçerik anahtarı ilke** aracılığıyla anahtar teslim bileşen Media Services'ın istemcileri sonlandırmak için içerik anahtarını nasıl teslim edildiğini yapılandırır. Media Services, içerik anahtarı otomatik sağlayabilirsiniz. Genellikle, uzun süreli bir anahtar ve Get ile ilkeleri bulunup bulunmadığını denetleyin. Anahtarı almak için parolaları veya kimlik bilgilerini almak için aşağıdaki örneğe bakın ayrı bir eylem yöntemini çağırmak gerekir.

**İçerik anahtarı ilkeleri** güncelleştirilebilir. Örneğin, bir anahtar döndürme yapmanız gerekiyorsa ilkesini güncelleştirme isteyebilirsiniz. Birincil doğrulama anahtarı ve diğer doğrulama anahtarları mevcut ilkedeki listesini güncelleştirebilirsiniz. Bu, güncelleştirme ve güncelleştirilmiş ilke çekme anahtar teslim önbellekleri 15 dakika kadar sürebilir. 

> [!IMPORTANT]
> * Özelliklerini **içerik anahtar ilkeleri** DateTime türü her zaman UTC biçiminde olan.
> * Medya hizmeti hesabınız için sınırlı sayıda ilkeleri tasarım ve aynı seçeneklere gerektiğinde, akış Bulucuyu için yeniden kullanabilirsiniz. 

## <a name="example"></a>Örnek

Anahtarı almak için kullanın **GetPolicyPropertiesWithSecretsAsync**aşağıdaki örnekte gösterildiği gibi.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Bkz: [filtreleme, sıralama, Media Services varlıklarının sayfalandırma](entities-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* [AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanma](protect-with-aes128.md)
* [DRM dinamik şifreleme ve lisans teslim hizmetini kullanma](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
