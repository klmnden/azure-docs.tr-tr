---
title: Azure Media Services varlıkları genel bakış - Azure | Microsoft Docs
description: Bu makalede, Azure Media Services varlıkları genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/24/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 3f3322245983508e374d081e5d7905f67344ad7a
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54912655"
---
# <a name="azure-media-services-entities-overview"></a>Azure Media Services varlıkları genel bakış

Bu makalede, her varlık, Media Services iş akışlarında nasıl kullanıldığı hakkında daha fazla bilgi için makaleler için Azure Media Services varlıkları ve noktaları kısa bir genel bakış verilmektedir. 

| Konu | Açıklama |
|---|---|
| [Hesap filtreleri ve varlık filtreleri](filters-dynamic-manifest-overview.md)|İçeriğinizi müşterilere (Canlı etkinlik veya isteğe bağlı Video akışı) sunarken istemcinizi varsayılan varlığın bildirim dosyasında tanımlanan değerinden daha fazla esneklik gerekebilir. Azure Media Services, tanımlamanıza imkan tanır [hesap filtreleri](https://docs.microsoft.com/rest/api/media/accountfilters) ve [varlık filtreleri](https://docs.microsoft.com/rest/api/media/assetfilters). Ardından, **dinamik bildirimlerini** önceden tanımlanmış filtrelere göre. |
| [Varlıklar](assets-concept.md)|Bir [varlık](https://docs.microsoft.com/rest/api/media/assets) varlık, dijital dosyalar (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil) ve bu dosyalar hakkındaki meta veriler içerir. Dijital dosyalar bir varlığa karşıya yüklendikten sonra medya kodlama, akış, içerik iş akışları çözümleme Hizmetleri'nde kullanılabilir.|
| [İçerik anahtar ilkeleri](content-key-policy-concept.md)|Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdaki güvenliğini sağlamak için kullanabilirsiniz. Media Services sayesinde, Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere.|
| [Canlı etkinlikler ve canlı çıkışları](live-events-outputs-concept.md)|Media Services Canlı etkinlikler müşterilerinizin Azure bulutunda dağıtmanıza olanak sağlar. Media Services v3 sürümünde, canlı akış olayları yapılandırmak için hakkında bilgi edinmek gereken [Canlı olayları](https://docs.microsoft.com/rest/api/media/liveevents) ve [Canlı çıkışları](https://docs.microsoft.com/rest/api/media/liveoutputs).|
| [Akış uç noktaları](streaming-endpoint-concept.md)|A [akış uç noktalarını](https://docs.microsoft.com/rest/api/media/streamingendpoints) varlık içeriği doğrudan bir istemci Yürütücü uygulamasına veya daha fazla dağıtım bir içerik teslim ağı'için (CDN) teslim eden bir akış hizmetini temsil eder. Bir akış uç noktası hizmetinden giden akış canlı akış ve isteğe bağlı varlığı Media Services hesabınızda olabilir. Bir Media Services hesabı oluşturduğunuzda bir **varsayılan** akış uç noktası, durdurulmuş durumda sizin için oluşturulur. Nelze odstranit **varsayılan** akış uç noktası. Ek akış uç noktaları hesap altında oluşturulabilir. Video akışını başlatmak için videonuzun akışını yapmak istediğiniz akış uç başlatmanız gerekir. |
| [Akış bulucuları](streaming-locators-concept.md)|Kayıttan yürütme için kullanabileceğiniz bir URL ile istemcilerinize kodlanmış video veya ses dosyası sağlamanız gerekir, oluşturmanız gerekir. bir [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) ve akış URL'leri oluşturun.|
| [Akış ilkeleri](streaming-policy-concept.md)| [İlkeleri akış](https://docs.microsoft.com/rest/api/media/streamingpolicies) , akış protokolleri ve şifreleme seçeneklerini, StreamingLocators tanımlamanıza olanak sağlar. Özel akış oluşturduğunuz ilke adını belirtebilir veya önceden tanımlanmış akış Media Services tarafından sunulan ilkelerden birini kullanın. <br/><br/>Özel bir akış ilke kullanırken, medya hizmeti hesabınız için sınırlı sayıda tür ilkeleri tasarlayın ve protokolleri ve aynı şifreleme seçenekleri gerektiğinde bunları yeniden için akış Bulucular gerekir. Yeni bir akış ilke her akış Bulucu için oluşturduğunuz değil.|
| [Dönüşümler ve işler](transforms-jobs-concept.md)|Kullanım [dönüştüren](https://docs.microsoft.com/rest/api/media/transforms) kodlama veya videoları analiz için ortak görevler yapılandırmak için. Her **dönüştürme** bir tarif veya bir iş akışı, video veya ses dosyalarını işlemek için görevler açıklanmaktadır.<br/><br/>A [iş](https://docs.microsoft.com/rest/api/media/jobs) uygulamak için Azure Media Services için fiili istek **dönüştürme** belirli bir giriş video veya ses içeriği için. **İş** konumun video giriş ve çıkış konumunu gibi bilgileri belirtir. Giriş video kullanarak konumu belirtebilirsiniz: HTTPS URL'leri, SAS URL'lerini veya bir varlık.|

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
