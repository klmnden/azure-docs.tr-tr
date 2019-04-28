---
title: Azure Media Services Widevine lisanslarını teslim etmek için castLabs kullanma | Microsoft Docs
description: Bu makale, PlayReady ve Widevine benzeri DRM ile AMS tarafından dinamik olarak şifrelenmiş bir akış sunmak için Azure Media Services (AMS) nasıl kullanabileceğinizi açıklar. Media Services PlayReady lisans sunucusundan PlayReady lisans gelir ve Widevine lisans castLabs lisans sunucusu tarafından sağlanır.
services: media-services
documentationcenter: ''
author: Mingfeiy
manager: femila
editor: ''
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: Mingfeiy;willzhan;Juliako
ms.openlocfilehash: dfb82e91b0f65b85d34b7e20d57ed9929469321f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61232582"
---
# <a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>Azure Media Services’ta Widevine lisansları vermek için castLabs kullanma 
> [!div class="op_single_selector"]
> * [Axinom](media-services-axinom-integration.md)
> * [castLabs](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a>Genel Bakış

Bu makale, PlayReady ve Widevine benzeri DRM ile AMS tarafından dinamik olarak şifrelenmiş bir akış sunmak için Azure Media Services (AMS) nasıl kullanabileceğinizi açıklar. Media Services PlayReady lisans sunucusundan PlayReady lisans gelir ve Widevine lisans teslim edilen **castLabs** lisans sunucusu.

Geri CENC (PlayReady ve/veya Widevine) tarafından korunan içeriği akış yürütmek için kullanabileceğiniz [Azure Media Player](https://amsplayer.azurewebsites.net/azuremediaplayer.html). Bkz: [AMP belge](https://amp.azure.net/libs/amp/latest/docs/) Ayrıntılar için.

Aşağıdaki diyagramda bir üst düzey Azure Media Services ve castLabs tümleştirme mimarisi gösterilmektedir.

![Tümleştirme](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a>Tipik sistem ayarlama

* Medya içeriği AMS içinde depolanır.
* İçerik anahtarı anahtar kimliklerini castLabs ve AMS depolanır.
* castLabs hem de AMS belirteci kimlik doğrulaması yerleşik olarak sahiptir. Aşağıdaki bölümlerde, kimlik doğrulama belirteçlerinizi açıklanmaktadır. 
* Bir istemci, videonuzun akışını yapmak istediğinde, içerik ile dinamik olarak şifrelenir **ortak şifreleme** (CENC) ve kesintisiz akış ve tire AMS tarafından dinamik olarak paket. Biz de PlayReady M2TS HLS Akış Protokolü için temel akış şifreleme sunun.
* PlayReady lisans AMS lisans sunucusundan alınır ve Widevine lisans castLabs lisans sunucusundan alınır. 
* Media Player, hangi lisans almak için İstemci Platformu özelliği temelinde otomatik olarak karar verir. 

## <a name="authentication-token-generation-for-getting-a-license"></a>Bir lisans almak için kimlik doğrulama belirteci oluşturma

CastLabs hem AMS lisans izin vermek için kullanılan belirteç biçimi JWT (JSON Web belirteci) destekler. 

### <a name="jwt-token-in-ams"></a>AMS JWT belirteci

Aşağıdaki tabloda, JWT belirteci AMS açıklanmaktadır. 

| Veren | Seçilen veren dizeden güvenli belirteç hizmeti (STS) |
| --- | --- |
| Hedef kitle |Kullanılan STS İzleyici dizeden |
| Talepler |Talepler kümesi |
| notBefore |Belirtecin geçerlilik Başlat |
| Bitiş Tarihi |Belirtecin geçerlilik bitiş |
| Samlassertion'da |PlayReady lisans sunucusu, sunucu lisansı ve STS, castLabs arasında paylaşılan anahtarı simetrik ya da asimetrik olabilir anahtarı. |

### <a name="jwt-token-in-castlabs"></a>JWT belirteci castLabs

Aşağıdaki tabloda, JWT belirteci castLabs açıklanmaktadır. 

| Ad | Açıklama |
| --- | --- |
| optData |İlgili bilgiler içeren bir JSON dizesi. |
| CRT |Varlık hakkında bilgi içeren bir JSON dizesi lisans bilgileri ve kayıttan yürütme hakları. |
| IAT |Geçerli dönem datetime. |
| jti |Bu belirteci (her belirteci yalnızca bir kez castLabs sisteminde kullanılabilir) hakkında benzersiz bir tanımlayıcı. |

## <a name="sample-solution-setup"></a>Örnek çözüm Kurulumu

[Örnek çözüm](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) iki projeden oluşan:

* PlayReady ve Widevine DRM kısıtlamalarını zaten alınan bir varlık üzerinde ayarlamak için kullanılan bir konsol uygulaması.
* Bir çok Basitleştirilmiş sürümü STS olarak görülebilir belirteçleri, out uygulamalı bir Web uygulaması.

Konsol uygulamasını kullanmak için:

1. AMS kimlik bilgileri, castLabs kimlik bilgileri, STS yapılandırma ve paylaşılan anahtarı ayarlama app.config değiştirin.
2. Bir varlık AMS karşıya yükleyin.
3. Karşıya yüklenen varlığından UUID alın ve Program.cs dosyasındaki satır 32 değiştirin:
   
      var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();
4. Bir AssetID (Program.cs dosyasındaki satır 44) castLabs sistemdeki varlık adlandırmak için kullanın.
   
   İçin AssetID ayarlamalısınız **castLabs**; benzersiz bir alfasayısal dize olması gerekir.
5. Programı çalıştırın.

Web uygulaması'nı (STS) kullanmak için:

1. Web.config değiştirmek için Kurulum castlabs satıcı kimliği, STS yapılandırma ve paylaşılan anahtarı.
2. Azure Web Siteleri'ne dağıtın.
3. Web sitesine gidin.

## <a name="playing-back-a-video"></a>Video kayıttan yürütme

Ortak şifreleme ile (PlayReady ve/veya Widevine) şifrelenmiş bir video kayıttan yürütme için kullanabilirsiniz [Azure Media Player](https://amsplayer.azurewebsites.net/azuremediaplayer.html). Konsol uygulaması çalıştırırken, içerik anahtarı kimliği ve bildirim URL'sini okunmaz.

1. Yeni bir sekme açın ve, STS başlatın: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2. Git [Azure Media Player](https://amsplayer.azurewebsites.net/azuremediaplayer.html).
3. Akış URL'si yapıştırın.
4. Tıklayın **Gelişmiş Seçenekler** onay kutusu.
5. İçinde **koruma** açılır menüsünde, select PlayReady ve/veya Widevine.
6. Belirteç metin kutusuna, STS adresinden aldığınız belirteç yapıştırın. 
   
   CastLab lisans sunucusuna ihtiyaç duymadığı "taşıyıcı =" belirteci önünde öneki. Bu nedenle, belirteç göndermeden önce lütfen kaldırın.
7. Oyuncu güncelleştirin.
8. Videoyu oynatmaya.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

