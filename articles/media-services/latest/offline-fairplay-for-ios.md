---
title: HLS çevrimdışı Apple FairPlay - Azure ile içerik koruma | Microsoft Docs
description: Bu konu, genel bir bakış sağlar ve HTTP canlı akışı (HLS) içeriğinizi Apple FairPlay ile çevrimdışı modda dinamik olarak şifrelemek için Azure Media Services'ı kullanma işlemini gösterir.
services: media-services
keywords: HLS, DRM, FairPlay Streaming (FPS), çevrimdışı iOS 10
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2019
ms.author: willzhan
ms.openlocfilehash: f2514fff2a3bb292a86c9f4c0e92c37ed2709097
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341037"
---
# <a name="offline-fairplay-streaming-for-ios"></a>İOS için çevrimdışı FairPlay Streaming 

 Azure Media Services sağlar, iyi tasarlanmış bir dizi [içerik koruma hizmetleri](https://azure.microsoft.com/services/media-services/content-protection/) kapsayan:

- Microsoft PlayReady
- Google Widevine
- Apple FairPlay
- AES-128 şifrelemesi

Dijital Hak Yönetimi (DRM) / Gelişmiş Şifreleme Standardı (AES) şifreleme içeriğinin gerçekleştirilir dinamik olarak çeşitli akış protokolü için istek. DRM lisans/AES şifre çözme anahtar teslim hizmetleri de Media Services tarafından sağlanır.

Çeşitli akış protokolleri üzerinden çevrimiçi akış içeriği koruma yanı sıra korumalı içerik Çevrimdışı mod da bir sıklıkla istediği özelliğidir. Çevrimdışı modu desteği aşağıdaki senaryolar için gereklidir:

* İnternet bağlantısı yolculuğu sırasında gibi kullanılabilir olmadığı durumlarda kayıttan yürütme.
* Bazı içerik sağlayıcıları DRM lisans teslimat ötesinde bir ülke/bölgenin kenarlık izin vermeyebilir. Çevrimdışı yükleme, kullanıcıların ülke/bölge dışında seyahat ederken içerik izlemek istiyorsanız gereklidir.
* Bazı ülkeler/bölgeler içinde Internet kullanılabilirliği ve/veya bant genişliği, hala sınırlıdır. Kullanıcılar, içeriği tatmin edici bir görüntüleme deneyimi için yeterince yüksek bir çözünürlük izleme yapabilmek için önce indirmek seçebilirsiniz. Bu durumda, sorun genellikle ağ kullanılabilirliğini ancak sınırlı ağ bant genişliği değildir. Üzerinden-üst düzey (OTT) / çevrimiçi video Platformu (OVP) sağlayıcıları, çevrimdışı moda destek isteyin.

Bu makalede, iOS 10 veya üstünü çalıştıran cihazları hedefleyen FairPlay Streaming (FPS) çevrimdışı modda destek kapsar. Bu özellik için Apple gibi diğer platformlarda, watchOS, tvOS ve Safari macOS üzerinde desteklenmiyor.

> [!NOTE]
> Çevrimdışı DRM yalnızca tek bir istek için bir lisans içeriği indirdiklerinde yapmak için faturalandırılır. Hataları faturalandırılmaz.

## <a name="prerequisites"></a>Önkoşullar

İOS 10 + cihazında HLS için FairPlay DRM çevrimdışı uygulamadan önce:

* FairPlay için çevrimiçi içerik koruma gözden geçirin: 

    - [Apple FairPlay lisansı gereksinimleri ve yapılandırması](fairplay-license-overview.md)
    - [DRM dinamik şifreleme ve lisans teslimat hizmeti kullanın](protect-with-drm.md)
    - Çevrimiçi akış FPS yapılandırmasını içeren bir .NET örneği: [ConfigureFairPlayPolicyOptions](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L505)
* Apple Geliştirici ağdan FPS SDK'sını alın. FPS SDK'sı iki bileşenleri içerir:

    - FPS sunucu anahtarı Güvenlik Modülü (KSM), istemci örnekleri, bir belirtimi ve test vektörler bir dizi içeren SDK.
    - FPS dağıtım D işlev belirtimi birlikte FPS sertifika, müşteriye özgü özel anahtarı ve uygulama gizli anahtarı oluşturma hakkında yönergeler içeren paketi. Apple, yalnızca lisanslı içerik sağlayıcıları için FPS dağıtım paketi yayımlar.
* Kopya https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git. 

    Kodda değiştirmeniz gerekecektir [şifrele .NET kullanarak DRM ile](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) FairPlay yapılandırmaları eklemek için.  

## <a name="configure-content-protection-in-azure-media-services"></a>Azure Media Services'da içerik korumayı yapılandırma

İçinde [GetOrCreateContentKeyPolicyAsync](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L189) yöntemi, aşağıdakileri yapın:

FairPlay ilke seçeneği yapılandırır kodun açıklamasını kaldırın:

```csharp
ContentKeyPolicyFairPlayConfiguration fairplayConfig = ConfigureFairPlayPolicyOptions();
```

Ayrıca, CBCS ContentKeyPolicyOption ContentKeyPolicyOptions listesine ekleyen bir kod açıklamasını kaldırın

```csharp
options.Add(
    new ContentKeyPolicyOption()
    {
        Configuration = fairplayConfig,
        Restriction = restriction,
        Name = "ContentKeyPolicyOption_CBCS"
    });
```

## <a name="enable-offline-mode"></a>Çevrimdışı modunu etkinleştir

Çevrimdışı modunu etkinleştirmek, bir özel StreamingPolicy oluşturup adı içinde bir StreamingLocator oluştururken kullanmak için [CreateStreamingLocatorAsync](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L563).
 
```csharp
CommonEncryptionCbcs objStreamingPolicyInput= new CommonEncryptionCbcs()
{
    Drm = new CbcsDrmConfiguration()
    {
        FairPlay = new StreamingPolicyFairPlayConfiguration()
        {
            AllowPersistentLicense = true  //this enables offline mode
        }
    },
    EnabledProtocols = new EnabledProtocols()
    {
        Hls = true,
        Dash = true //Even though DASH under CBCS is not supported for either CSF or CMAF, HLS-CMAF-CBCS uses DASH-CBCS fragments in its HLS playlist
    },

    ContentKeys = new StreamingPolicyContentKeys()
    {
        //Default key must be specified if keyToTrackMappings is present
        DefaultKey = new DefaultKey()
        {
            Label = "CBCS_DefaultKeyLabel"
        }
    }

```

Media Services hesabınız çevrimdışı FairPlay lisansları vermek için artık yapılandırılmıştır.

## <a name="sample-ios-player"></a>Örnek iOS Player

FPS çevrimdışı modu desteği yalnızca bulunan iOS 10 ve üzeri. Belge ve FPS çevrimdışı modu için örnek FPS sunucu SDK'sını (sürüm 3.0 veya üstü) içerir. Özellikle, FPS sunucu SDK'sı (sürüm 3.0 veya üstü) çevrimdışı moda ilgili aşağıdaki iki öğeyi içerir:

* Belge: "FairPlay akışı ile çevrimdışı kayıttan yürütme ve HTTP canlı akış." Apple, 14 Eylül 2016'i tıklatın. FPS sunucu SDK sürüm 4.0, bu belgenin ana FPS belgeye birleştirilir.
* Örnek kod: Çevrimdışı modda \FairPlay akış sunucu SDK'sı sürüm 3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\ FPS için HLSCatalog örneği (FPS sunucu SDK'sı Apple'nın bir parçası). HLSCatalog örnek uygulamada, aşağıdaki kod dosyalarını çevrimdışı modda özellikleri uygulamak için kullanılır:

    - AssetPersistenceManager.swift kod dosyası: AssetPersistenceManager gösteren Bu örnekte ana sınıftır nasıl yapılır:

        - İndirme HLS akış API'leri başlatıp yüklemeler iptal ve cihazları devre dışı varolan varlıkları silmek için kullanılan gibi yönetin.
        - İndirme ilerleme durumunu izleyin.
    - AssetListTableViewController.swift ve AssetListTableViewCell.swift kod dosyaları: AssetListTableViewController bu örneğe ilişkin ana arabirimidir. Bu yürütmek, indirin, silin veya karşıdan yükleme iptal için örnek kullanabilirsiniz varlıklar listesini sağlar. 

Bu adımları, çalışan bir iOS player ayarlama işlemini göstermektedir. FPS sunucu SDK'sı sürüm 4.0.1'in HLSCatalog örnekte başlangıç varsayarsak, aşağıdaki kod değişiklikleri yapın:

HLSCatalog\Shared\Managers\ContentKeyDelegate.swift içinde yöntemi uygulamak `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` aşağıdaki kodu kullanarak. HLS URL'sine atanmış bir değişken "drmUr" sağlar.

```swift
    var ckcData: Data? = nil
    
    let semaphore = DispatchSemaphore(value: 0)
    let postString = "spc=\(spcData.base64EncodedString())&assetId=\(assetIDString)"
    
    if let postData = postString.data(using: .ascii, allowLossyConversion: true), let drmServerUrl = URL(string: self.drmUrl) {
        var request = URLRequest(url: drmServerUrl)
        request.httpMethod = "POST"
        request.setValue(String(postData.count), forHTTPHeaderField: "Content-Length")
        request.setValue("application/x-www-form-urlencoded", forHTTPHeaderField: "Content-Type")
        request.httpBody = postData
        
        URLSession.shared.dataTask(with: request) { (data, _, error) in
            if let data = data, var responseString = String(data: data, encoding: .utf8) {
                responseString = responseString.replacingOccurrences(of: "<ckc>", with: "").replacingOccurrences(of: "</ckc>", with: "")
                ckcData = Data(base64Encoded: responseString)
            } else {
                print("Error encountered while fetching FairPlay license for URL: \(self.drmUrl), \(error?.localizedDescription ?? "Unknown error")")
            }
            
            semaphore.signal()
            }.resume()
    } else {
        fatalError("Invalid post data")
    }
    
    semaphore.wait()
    return ckcData
```

HLSCatalog\Shared\Managers\ContentKeyDelegate.swift içinde yöntemi uygulamak `requestApplicationCertificate()`. Bu uygulama, cihaz (yalnızca ortak anahtar) sertifika ekleme veya web sertifikadaki ana bilgisayar bağlıdır. Aşağıdaki uygulama test örneklerde kullanılan barındırılan uygulama sertifikasını kullanır. Uygulama sertifikasını URL'sini içeren bir değişken "certUrl" sağlar.

```swift
func requestApplicationCertificate() throws -> Data {

        var applicationCertificate: Data? = nil
        do {
            applicationCertificate = try Data(contentsOf: URL(string: certUrl)!)
        } catch {
            print("Error loading FairPlay application certificate: \(error)")
        }
        
        return applicationCertificate
    }
```

Son tümleşik test için hem video URL'sini ve uygulama sertifika URL'si bölümünde "Tümleşik Test." sağlanır

HLSCatalog\Shared\Resources\Streams.plist içinde test video URL'nizi ekleyin. İçerik için anahtar kimliği, FairPlay lisans edinme URL'si skd protokolü ile benzersiz değeri olarak kullanın.

![Çevrimdışı FairPlay iOS uygulaması akışları](media/offline-fairplay-for-ios/offline-fairplay-ios-app-streams.png)

Bunları ayarlamak varsa kendi test video URL'si, FairPlay lisans edinme URL'si ve uygulama sertifika URL'sini kullanın. Veya test örnekleri içeren bir sonraki bölümüne geçebilirsiniz.

## <a name="integrated-test"></a>Tümleşik test

Media Services üç test örnekleri aşağıdaki üç senaryo kapsar:

* , Video, ses ve diğer ses kaydı ile korunan FPS
* Video ve ses, ancak başka hiçbir ses kaydı ile korunan FPS
* Yalnızca video ve ses yok ile korunan FPS

Bu örneği bulabilirsiniz [bu demo sitesi](https://aka.ms/poc#22), karşılık gelen uygulama sertifikası ile bir Azure web uygulamasında barındırılan.
Alternatif ses ana çalma listesi içeriyorsa, sürüm 3 veya FPS sunucu SDK'sı sürüm 4 örneği ile çevrimdışı modunda, yalnızca ses çalar. Bu nedenle, diğer ses çıkarmanız gerekir. Diğer bir deyişle, listede ikinci ve üçüncü örnek, daha önce çevrimiçi ve çevrimdışı modda çalışmaz. Listelenen örnek ilk çevrimiçi works düzgün bir şekilde akış sırasında çevrimdışı modu sırasında yalnızca ses çalar.

## <a name="faq"></a>SSS

Aşağıdaki sık sorulan sorular sorun giderme Yardımı sağlarız:

- **Neden yalnızca ses video sırasında çevrimdışı modda oynar?** Bu davranış tasarım örnek uygulama tarafından gibi görünüyor. Alternatif bir ses kaydı (HLS için söz konusu olduğu) yoksa sırasında hem iOS 10 hem de alternatif bir ses kaydı için iOS 11 varsayılan çevrimdışı modda olduğunda. Bu davranış için çevrimdışı modu FPS dengelemek için akıştan alternatif ses kaydının kaldırın. Medya Hizmetleri bunun için dinamik bildirim Filtresi Ekle "yalnızca ses = false." Diğer bir deyişle, HLS URL .ism/manifest(format=m3u8-aapl,audio-only=false) ile sona erer. 
- **Neden hala oynatmak video yalnızca ses sırasında çevrimdışı modu yalnızca ses ekleyebilirim sonra = false?** İçerik teslim ağı (CDN) önbellek temel tasarım bağlı olarak, içeriği önbelleğe alınabilir. Önbellek temizleme.
- **İOS 10 yanı sıra iOS 11 FPS Çevrimdışı mod da destekleniyor mu?** Evet. FPS çevrimdışı modu iOS 10 ve 11 iOS için desteklenir.
- **Belgenin "Çevrimdışı kayıttan yürütme ile FairPlay akış ve HTTP canlı akış" FPS sunucu SDK'ın neden bulamıyorum?** FPS sunucu SDK sürüm 4 itibaren bu belgede "FairPlay akış programlama kılavuzu." birleştirildiği
- **İOS cihazlarında indirilen/çevrimdışı dosya yapısı nedir?** İndirilen dosya yapısı bir iOS cihazında aşağıdaki ekran görüntüsüne benzer görünür. `_keys` Klasör depoları indirilen FPS lisansları, her lisans hizmeti konağı için bir depolama dosya. `.movpkg` Klasör ses ve video içeriği depolar. Video içeriği sayısal tarafından izlenen bir tire ile biten bir ada sahip ilk klasör içerir. Video yorumlama, PeakBandwidth sayısal değerdir. Ses içeriğini 0 olmayanla izlenir bir tire ile biten bir ada sahip ikinci klasör içerir. "Veri" adlı üçüncü klasörü FPS içeriğinin ana çalma listesini içerir. Son olarak, eksiksiz bir açıklaması boot.xml sağlar `.movpkg` klasör içeriği. 

![Çevrimdışı FairPlay iOS uygulaması dosya yapısı örneği](media/offline-fairplay-for-ios/offline-fairplay-file-structure.png)

Bir örnek boot.xml dosyası:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<HLSMoviePackage xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://apple.com/IMG/Schemas/HLSMoviePackage" xsi:schemaLocation="http://apple.com/IMG/Schemas/HLSMoviePackage /System/Library/Schemas/HLSMoviePackage.xsd">
  <Version>1.0</Version>
  <HLSMoviePackageType>PersistedStore</HLSMoviePackageType>
  <Streams>
    <Stream ID="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" Path="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(127000)/Manifest(aac_eng_2_127,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
    <Stream ID="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" Path="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(161000)/Manifest(video,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
  </Streams>
  <MasterPlaylist>
    <NetworkURL>https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/manifest(format=m3u8-aapl,audio-only=false)</NetworkURL>
  </MasterPlaylist>
  <DataItems Directory="Data">
    <DataItem>
      <ID>CB50F631-8227-477A-BCEC-365BBF12BCC0</ID>
      <Category>Playlist</Category>
      <Name>master.m3u8</Name>
      <DataPath>Playlist-master.m3u8-CB50F631-8227-477A-BCEC-365BBF12BCC0.data</DataPath>
      <Role>Master</Role>
    </DataItem>
  </DataItems>
</HLSMoviePackage>
```

## <a name="next-steps"></a>Sonraki adımlar

[AES-128 ile koruma](protect-with-aes128.md) hakkında bilgi edinin
