---
title: "Azure Media Services Widevine lisansları teslim için castLabs kullanma | Microsoft Docs"
description: "Bu makalede, PlayReady ve Widevine DRMs AMS tarafından dinamik olarak şifrelenmiş bir akış sağlamak üzere Azure Media Services (AMS) nasıl kullanabileceğinizi açıklar. Medya Hizmetleri PlayReady lisans sunucusundan PlayReady lisans gelir ve castLabs lisans sunucusu tarafından Widevine lisans teslim edilir."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: Mingfeiy;willzhan;Juliako
ms.openlocfilehash: 5b69e804809f834e81221fb2787a997a52dbe286
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>Azure Media Services’ta Widevine lisansları vermek için castLabs kullanma
> [!div class="op_single_selector"]
> * [Axinom](media-services-axinom-integration.md)
> * [castLabs](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Bu makalede, PlayReady ve Widevine DRMs AMS tarafından dinamik olarak şifrelenmiş bir akış sağlamak üzere Azure Media Services (AMS) nasıl kullanabileceğinizi açıklar. Medya Hizmetleri PlayReady lisans sunucusundan PlayReady lisans gelir ve Widevine lisans teslim tarafından **castLabs** lisans sunucusu.

Kayıttan yürütme CENC (PlayReady ve/veya Widevine) tarafından korunan içeriğin akış için kullandığınız [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Bkz: [AMP belge](http://amp.azure.net/libs/amp/latest/docs/) Ayrıntılar için.

Aşağıdaki diyagramda bir üst düzey Azure Media Services ve castLabs tümleştirme mimarisi gösterilir.

![Tümleştirme](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a>Tipik sistem ayarlama
* Medya içeriği AMS içinde depolanır.
* İçerik anahtarı anahtar kimliklerini castLabs ve AMS içinde depolanır.
* castLabs ve AMS belirteci kimlik doğrulaması yerleşik olarak sahiptir. Aşağıdaki bölümlerde, kimlik doğrulama belirteçleri açıklanmaktadır. 
* Video akışını sağlamak için bir istemci istediğinde, içerik ile dinamik olarak şifrelenir **ortak şifreleme** (CENC) ve kesintisiz akış ve tire AMS tarafından dinamik olarak paket. Biz, PlayReady M2TS başlangıç akışı şifreleme HLS Akış Protokolü için de sunar.
* PlayReady lisans AMS lisans sunucusundan alınır ve Widevine lisans castLabs lisans sunucusundan alınır. 
* Media Player, getirmek için hangi lisans istemci platformu özelliği temelinde otomatik olarak karar verir. 

## <a name="authentication-token-generation-for-getting-a-license"></a>Bir lisans almak için kimlik doğrulama belirteci oluşturma
Hem castLabs hem de AMS lisans yetkilendirmek için kullanılan (JSON Web belirteci) JWT belirteci biçimi destekler. 

### <a name="jwt-token-in-ams"></a>AMS JWT belirteci
Aşağıdaki tabloda AMS JWT belirteci açıklanmaktadır. 

| Veren | Seçilen veren dizeden güvenli belirteç hizmeti (STS) |
| --- | --- |
| Hedef kitle |Kullanılan STS dizeden hedef kitle |
| Talepler |Talepler kümesi |
| NotBefore |Belirtecin geçerliliğini Başlat |
| Süre Sonu: |Son belirtecin geçerliliğini |
| SigningCredentials |Lisans sunucusu ve STS, castLabs olan PlayReady lisans sunucusu arasında paylaşılan anahtarı simetrik ya da asimetrik olabilir anahtarı. |

### <a name="jwt-token-in-castlabs"></a>CastLabs JWT belirteci
Aşağıdaki tabloda castLabs JWT belirteci açıklanmaktadır. 

| Ad | Açıklama |
| --- | --- |
| optData |Sizinle ilgili bilgileri içeren bir JSON dizesi. |
| CRT |Varlık hakkında bilgi içeren bir JSON dizesi lisans bilgileri ve kayıttan yürütme hakları. |
| IAT |Geçerli dönem datetime. |
| jti |(Her belirteci yalnızca bir kez castLabs sisteminde kullanılabilir) bu belirteç hakkında benzersiz bir tanımlayıcı. |

## <a name="sample-solution-set-up"></a>Örnek çözümü ayarlayın
[Örnek çözümü](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) iki projeden oluşan:

* PlayReady ve Widevine DRM kısıtlamaları zaten alınan bir varlık üzerinde ayarlamak için kullanılan bir konsol uygulaması.
* Bir çok Basitleştirilmiş sürümü bir STS olarak görülebilir belirteçleri dışarı aktarır bir Web uygulamasıdır.

Konsol uygulaması kullanmak için:

1. AMS kimlik bilgileri, castLabs kimlik bilgileri, STS yapılandırma ve paylaşılan anahtar kurulumu için app.config değiştirin.
2. Bir varlık AMS yükleyin.
3. Karşıya yüklenen varlığından UUID almak ve Program.cs dosyasındaki satır 32 değiştirin:
   
      var objIAsset _bağlamı =. Assets.Where (x = > x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf"). FirstOrDefault();
4. Bir AssetID castLabs sisteminde (Program.cs dosyasındaki satır 44) varlık adlandırmak için kullanın.
   
   İçin AssetID ayarlamalısınız **castLabs**; benzersiz bir alfasayısal dize olması gerekir.
5. Programını çalıştırın.

Web uygulaması'nı (STS) kullanmak için:

1. Kurulum castlabs Satıcı web.config değiştirmek kimliği, STS yapılandırma ve paylaşılan anahtar.
2. Azure Web sitelerine dağıtma.
3. Web sitesine gidin.

## <a name="playing-back-a-video"></a>Bir video kayıttan yürütme
Ortak şifreleme ile (PlayReady ve/veya Widevine) şifreli video kayıttan yürütme için kullandığınız [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Konsol uygulaması çalıştırırken, içerik anahtarı kimliği ve bildirim URL echoed.

1. Yeni bir sekme açın ve, STS başlatın: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2. Git [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3. Akış URL'sini yapıştırın.
4. Tıklatın **Gelişmiş Seçenekler** onay kutusu.
5. İçinde **koruma** açılan listesinde, select PlayReady ve/veya Widevine.
6. Belirteç metin kutusuna, STS dan aldığınız belirteci yapıştırın. 
   
   CastLab lisans sunucusu ihtiyaç duymadığı "taşıyıcı =" belirteç önünde öneki. Bu nedenle, belirteç göndermeden önce lütfen kaldırın.
7. Player güncelleştirin.
8. Video oynatma.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

