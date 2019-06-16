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
ms.date: 05/28/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: a597ab3519f4ba1696e111622541bcab89488558
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66425420"
---
# <a name="content-key-policies"></a>İçerik Anahtar İlkeleri

Media Services sayesinde, Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. 

Akışınız şifreleme seçeneklerini belirtmek için oluşturmak gereken bir [akış ilke](streaming-policy-concept.md) ilişkilendirin, [akış Bulucu](streaming-locators-concept.md). Oluşturduğunuz [içerik anahtarı ilke](https://docs.microsoft.com/rest/api/media/contentkeypolicies) yapılandırmak için nasıl içerik anahtarı (güvenli erişim sağlayan, [varlıklar](assets-concept.md)) istemcileri sonlandırmak için teslim edilir. Gereksinimleri (kısıtlamaları) ayarlamanız anahtarları için sırayla istemcilere iletilmesi belirtilen yapılandırma ile karşılanması gereken içerik anahtarı ilkeye gerekir. Bu içerik anahtarı ilkeyi Temizle akış veya yükleme için gerekli değildir. 

Genellikle, ilişkilendirme, **içerik anahtarı ilke** ile [akış Bulucu](streaming-locators-concept.md). Alternatif olarak, içerik anahtarı ilke içinde belirtebileceğiniz bir [akış ilke](streaming-policy-concept.md) (ne zaman için özel bir akış ilke oluşturma Gelişmiş senaryoları). 

Media Services için içerik anahtarı otomatik olarak izin vermek için önerilir. Genellikle, uzun süreli bir anahtar kullanmak ve ile ilkeleri varlığını denetle **alma**. Anahtarı almak için parolaları veya kimlik bilgilerini almak için aşağıdaki örneğe bakın ayrı bir eylem yöntemini çağırmak gerekir.

**İçerik anahtarı ilkeleri** güncelleştirilebilir. Bu, güncelleştirme ve güncelleştirilmiş ilke çekme anahtar teslim önbellekleri 15 dakika kadar sürebilir. 

> [!IMPORTANT]
> * Özelliklerini **içerik anahtar ilkeleri** DateTime türü her zaman UTC biçiminde olan.
> * Medya hizmeti hesabınız için sınırlı sayıda ilkeleri tasarım ve aynı seçeneklere gerektiğinde, akış Bulucuyu için yeniden kullanabilirsiniz. Daha fazla bilgi için [kotaları ve sınırlamaları](limits-quotas-constraints.md).

### <a name="example"></a>Örnek

Anahtarı almak için kullanın **GetPolicyPropertiesWithSecretsAsync**gösterildiği [mevcut ilkeden bir imzalama anahtarı alma](get-content-key-policy-dotnet-howto.md#get-contentkeypolicy-with-secrets) örnek.

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Bkz: [filtreleme, sıralama, Media Services varlıklarının sayfalandırma](entities-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* [AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanma](protect-with-aes128.md)
* [DRM dinamik şifreleme ve lisans teslimat hizmeti kullanın](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
