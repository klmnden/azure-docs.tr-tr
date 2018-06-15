---
title: HLS çevrimdışı Apple FairPlay - Azure ile içerik koruma | Microsoft Docs
description: Bu konu genel bir bakış sağlar ve dinamik olarak Apple FairPlay çevrimdışı modda, HTTP canlı akışı (HLS) içeriğinizle şifrelemek için Azure Media Services kullanmayı gösterir.
services: media-services
keywords: HLS, DRM, FairPlay akış (FPS), çevrimdışı iOS 10
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
ms.date: 12/01/2017
ms.author: willzhan, dwgeo
ms.openlocfilehash: dc38772097dddb7c7135d55598373d7ab544f9ea
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790394"
---
# <a name="offline-fairplay-streaming-for-ios"></a>FairPlay çevrimdışı akış iOS için 
 Azure Media Services, iyi tasarlanmış bir dizi sağlar [içerik koruma hizmetleri](https://azure.microsoft.com/services/media-services/content-protection/) bu kapsar:

- Microsoft PlayReady
- Google Widevine
- Apple FairPlay
- AES-128 şifrelemesi

Dijital Hak Yönetimi (DRM) / Gelişmiş Şifreleme Standardı (AES) şifreleme içerik gerçekleştirilir dinamik olarak çeşitli akış protokolleri için istek. DRM lisans/AES şifre çözme anahtarı teslimat hizmetlerini de Media Services tarafından sağlanır.

Çevrimiçi çeşitli akış protokolleri akış içeriği koruma yanı sıra, korumalı içerik için Çevrimdışı mod da bir sık istenen özelliğidir. Çevrimdışı modda desteği aşağıdaki senaryolar için gereklidir:

* Internet bağlantısı sırasında seyahat gibi kullanılabilir olmadığı durumlarda kayıttan yürütme.
* Bazı içerik sağlayıcıları DRM lisans teslimat bir ülkenin kenarlık ötesinde izin vermeyebilir. Çevrimdışı yükleme, kullanıcıların ülke dışında seyahat ederken içeriği izlemek istiyorsanız gereklidir.
* Bazı ülkelerde Internet kullanılabilirliği ve/veya bant genişliği olduğundan hala sınırlı. Kullanıcılar, tatmin edici izleme deneyimi için yeterince yüksek çözünürlüğü içeriği izlemek için indirmek üzere seçebilirsiniz. Bu durumda, sorunu genellikle ağ kullanılabilirliğini ancak sınırlı ağ bant genişliği değil. Üzerinden--top (OTT) / çevrimiçi video platform (OVP) sağlayıcıları çevrimdışı modda destek isteği.

Bu makalede iOS 10 veya üstünü çalıştıran cihazları hedefler FairPlay akış (FPS) çevrimdışı modda destek yer almaktadır. Bu özellik, diğer Apple platformlarda, watchOS, tvOS veya Safari gibi macOS için desteklenmiyor.

## <a name="preliminary-steps"></a>Başlangıç adımları
FairPlay için 10 + iOS cihazında çevrimdışı DRM uygulamadan önce:

* FairPlay için çevrimiçi içerik koruma öğrenmeniz. Daha fazla bilgi için aşağıdaki makaleler ve örnekleri bakın:

    - [Apple FairPlay akış Azure Media Services için genel olarak kullanılabilir](https://azure.microsoft.com/blog/apple-FairPlay-streaming-for-azure-media-services-generally-available/)
    - [Apple FairPlay veya Microsoft PlayReady ile içerik, HLS koruma](https://docs.microsoft.com/azure/media-services/media-services-protect-hls-with-FairPlay)
    - [Çevrimiçi FPS akış için bir örnek](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-FairPlay/)

* Apple Geliştirici ağdan FPS SDK'sını alın. FPS SDK'sı iki bileşenleri içerir:

    - FPS sunucu anahtarı Güvenlik Modülü (KSM), istemci örnekler, bir belirtim ve test vektörlerinin kümesi içeren SDK.
    - FPS dağıtım D işlevi belirtimi birlikte FPS sertifika, müşteriye özgü özel anahtarı ve uygulama gizli anahtarı oluşturma hakkında yönergeler içeren paketi. Apple FPS dağıtım paketi, yalnızca lisanslı içerik sağlayıcılarına verir.

## <a name="configuration-in-media-services"></a>Media Services yapılandırma
FPS çevrimdışı modda yapılandırması için [Media Services .NET SDK'sı](https://www.nuget.org/packages/windowsazure.mediaservices), Media Services .NET SDK sürüm 4.0.0.4 kullanın veya FPS çevrimdışı modunu yapılandırmak için gereken API sağlayan daha sonra.
Ayrıca çevrimiçi moda FPS içerik korumayı yapılandırmak için çalışma kod gerekir. Çevrimiçi modda içerik koruma FPS için yapılandırmak için kodu edindikten sonra yalnızca aşağıdaki iki değişiklikleri gerekir.

## <a name="code-change-in-the-fairplay-configuration"></a>FairPlay yapılandırma kod değişikliği
"Etkinleştirme çevrimdışı modu" tanımlamak için ilk değişikliktir Boolean, çevrimdışı DRM senaryo etkinleştirdiğinde, true değerine objDRMSettings.EnableOfflineMode çağrılır. Bu gösterge bağlı olarak, aşağıdaki FairPlay yapılandırma değişiklik yapılabilir:

```csharp
if (objDRMSettings.EnableOfflineMode)
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv, 
            RentalAndLeaseKeyType.PersistentUnlimited,
            0x9999);
    }
    else
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv);
    }
```

## <a name="code-change-in-the-asset-delivery-policy-configuration"></a>Kodunu değiştirmek varlık teslim ilkesi yapılandırması
İkinci değişikliği üçüncü anahtar sözlükteki < AssetDeliveryPolicyConfigurationKey, dize > eklemektir.
AssetDeliveryPolicyConfigurationKey aşağıda gösterildiği gibi ekleyin:
 
```csharp
// FPS offline mode
    if (drmSettings.EnableOfflineMode)
    {
        objDictionary_AssetDeliveryPolicyConfigurationKey.Add(AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense, "true");
        Console.WriteLine("FPS OFFLINE MODE: AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense added into asset delivery policy configuration.");
    }

    // for IAssetDelivery for FPS
    IAssetDeliveryPolicy objIAssetDeliveryPolicy = objCloudMediaContext.AssetDeliveryPolicies.Create(
            drmSettings.AssetDeliveryPolicyName,
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            objDictionary_AssetDeliveryPolicyConfigurationKey);
```

Bu adımdan sonra aşağıdaki üç girdileri FPS varlık teslim ilkesini < Dictionary_AssetDeliveryPolicyConfigurationKey > dizesinde içerir:

* AssetDeliveryPolicyConfigurationKey.FairPlayBaseLicenseAcquisitionUrl ya da kullanılan FPS KSM/anahtar sunucusu gibi etkenlere ve aynı varlık teslim yeniden olup bağlı olarak AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl birden çok varlık arasında İlkesi
* AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs
* AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense

Media Services hesabınızı çevrimdışı FairPlay lisansları teslim etmek için yapılandırılmıştır.

## <a name="sample-ios-player"></a>Örnek iOS Player
FPS çevrimdışı modda desteği ve sonrasında yalnızca ios'ta 10 kullanılabilir değil. Belge ve örnek FPS çevrimdışı modu için FPS sunucu SDK'sı (sürüm 3.0 veya üstü) içerir. Özellikle, FPS Server SDK (sürüm 3.0 veya üstü) çevrimdışı moda ilgili aşağıdaki iki öğeyi içerir:

* Belge: "FairPlay akış ile çevrimdışı kayıttan yürütme ve HTTP canlı akış." Apple, 14 Eylül 2016'i tıklatın. FPS Server SDK sürüm 4.0, bu belgenin ana FPS belgeye birleştirilir.
* Örnek kod: HLSCatalog örnek \FairPlay akış sunucusu SDK sürüm 3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\ çevrimdışı modda FPS için. HLSCatalog örnek uygulaması, aşağıdaki kod dosyaları çevrimdışı modda özellikleri uygulamak için kullanılır:

    - AssetPersistenceManager.swift kod dosyası: AssetPersistenceManager olduğunu gösteren bu örnekteki ana sınıfı nasıl yapılır:

        - API'ları başlatmak ve yüklemeleri iptal etmek için ve cihazları devre dışı varolan varlıkları silmek için kullanılan gibi indirme HLS akış yönetin.
        - Yükleme ilerleme durumunu izleyin.
    - Kod dosyaları AssetListTableViewController.swift ve AssetListTableViewCell.swift: AssetListTableViewController olduğundan bu örnek ana arabirimi. Örnek yürütmek, indirin, silmek veya bir yüklemeyi iptal etmek için kullanabileceğiniz varlıklar listesini sağlar. 

Bu adımlar çalışan bir iOS oynatıcı ayarlamak nasıl gösterir. FPS Server SDK 4.0.1 sürümünü HLSCatalog örnekten Başlat varsayıldığında, aşağıdaki kod değişiklikleri yapın:

HLSCatalog\Shared\Managers\ContentKeyDelegate.swift içinde yöntemi uygulaması `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` aşağıdaki kodu kullanarak. "DrmUr" HLS URL'sine atanmış bir değişken olmasına izin verin.

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

HLSCatalog\Shared\Managers\ContentKeyDelegate.swift içinde yöntemi uygulaması `requestApplicationCertificate()`. Bu uygulama sertifikası (yalnızca ortak anahtar) aygıtıyla katıştırmak mı web sertifikadaki ana bilgisayar bağlıdır. Aşağıdaki uygulama test örneklerinde kullanılan barındırılan uygulama sertifikasını kullanır. "CertUrl" uygulama sertifika URL'sini içeren bir değişkeni olmasına izin verin.

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

Son tümleşik test için video URL'si ve uygulama sertifika URL'si bölümündeki "Tümleşik Test." sağlanır

HLSCatalog\Shared\Resources\Streams.plist içinde test video URL'nizi ekleyin. İçerik için anahtar kimliği, FairPlay lisans edinme URL'si skd protokolüyle benzersiz değer olarak kullanın.

![Çevrimdışı FairPlay iOS App akışlar](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-ios-app-streams.png)

Bunları ayarlamak varsa, kendi test video URL'si, FairPlay lisans edinme URL'si ve uygulama sertifika URL'sini kullanın. Veya test örnekleri içeren bir sonraki bölümüne geçebilirsiniz.

## <a name="integrated-test"></a>Tümleşik test
Media Services üç test örnekleri aşağıdaki üç senaryodan kapsar:

* , Video, ses ve diğer ses parça ile korunan FPS
* Video ve ses, ancak başka hiçbir ses izleme ile korunan FPS
* Yalnızca video ve ses yok ile korunan FPS

Bu örnekler konumunda bulabilirsiniz [Bu gösteri sitesi](http://aka.ms/poc#22), karşılık gelen uygulama sertifikayla barındırılan bir Azure web uygulaması.
Ana çalma alternatif ses içeriyorsa sürüm 3 veya FPS Server SDK sürüm 4 örneği ile sırasında çevrimdışı modda, yalnızca ses yürütür. Bu nedenle, diğer ses çıkarabilmesi gerekir. Diğer bir deyişle, listelenen ikinci ve üçüncü örnekleri, daha önce çevrimiçi ve çevrimdışı modda çalışmaz. Listelenen örnek ilk çevrimiçi works düzgün akışı sırasında yalnızca çevrimdışı modda sırasında ses yürütür.

## <a name="faq"></a>SSS
Aşağıdaki sık sorulan sorular sorun giderme ile ilgili Yardım sağlar:

- **Neden yalnızca ses video sırasında çevrimdışı modda oynar?** Bu davranış, örnek uygulamayı tasarım gereği görünüyor. Alternatif bir ses İzle (HLS için söz konusu olduğu) yoksa çevrimdışı modda, iOS 10 ve 11 iOS varsayılan alternatif ses izlenecek sırasında olduğunda. Bu davranış FPS çevrimdışı modu için dengelemek için alternatif ses izleme akıştan kaldırın. Media Services'de bunun için dinamik bildirim Filtresi Ekle "yalnızca ses = false." Diğer bir deyişle, HLS URL .ism/manifest(format=m3u8-aapl,audio-only=false) ile sona erer. 
- **Neden hala oynatmak yalnızca video ses çevrimdışı modu sırasında yalnızca ses ekledikten sonra = false?** İçerik teslim ağı (CDN) önbelleği temel tasarım bağlı olarak, içerik önbelleğe. Önbelleği Temizle.
- **FPS Çevrimdışı mod da iOS 10 yanı sıra iOS 11 destekleniyor mu?** Evet. Çevrimdışı modda FPS iOS 10 ve 11 iOS için desteklenir.
- **"Çevrimdışı kayıttan yürütme ile FairPlay akış ve HTTP canlı akış" Belge FPS sunucu SDK'da neden bulamıyorum?** FPS Server SDK sürüm 4 itibaren bu belgede "FairPlay akış programlama kılavuzu." birleştirildiği
- **Ne son parametre FPS çevrimdışı modu aşağıdaki API'sindeki göze?**
`Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(objX509Certificate2, pfxPassword, pfxPasswordId, askId, iv, RentalAndLeaseKeyType.PersistentUnlimited, 0x9999);`

    Bu API için belgelere bakın [FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.mediaservices.client.FairPlay.FairPlayconfiguration.createserializedFairPlayoptionconfiguration?view=azure-dotnet). Parametre birimi olarak saat ile çevrimdışı kiralama süresini temsil eder.
- **İOS cihazlarda yüklenen/çevrimdışı dosya yapısı nedir?** Aşağıdaki ekran görüntüsüne benzeyen bir iOS cihazında indirilen dosya yapısı arar. `_keys` Klasör depoları her lisans hizmeti ana bilgisayarı için bir depolama dosyayla FPS lisansları indirilir. `.movpkg` Klasör, ses ve video içeriğine depolar. Sayısal tarafından izlenen bir tire ile biten bir adla ilk klasörü video içeriği içerir. Video yorumlama PeakBandwidth sayısal değerdir. 0 ve ardından bir tire ile biten bir ada sahip ikinci klasör ses içeriğini içerir. "Data" adlı üçüncü klasörü FPS içerik ana çalma listesi içerir. Son olarak, boot.xml eksiksiz bir açıklaması verilmiştir `.movpkg` klasör içeriği. 

![Çevrimdışı FairPlay iOS örnek uygulama dosya yapısı](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-file-structure.png)

Örnek boot.xml dosyası:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<HLSMoviePackage xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://apple.com/IMG/Schemas/HLSMoviePackage" xsi:schemaLocation="http://apple.com/IMG/Schemas/HLSMoviePackage /System/Library/Schemas/HLSMoviePackage.xsd">
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

## <a name="summary"></a>Özet
Bu belgede aşağıdaki adımları ve FPS çevrimdışı modda uygulamak için kullanabileceğiniz bilgileri içerir:

* Medya Hizmetleri içerik koruma yapılandırması Media Services .NET API aracılığıyla dinamik FairPlay şifreleme ve FairPlay lisans teslimat Media Services yapılandırır.
* FPS Server SDK örnekten dayalı bir iOS player FPS yürütebilen bir iOS player akış modu çevrimiçi veya çevrimdışı modda içerik ayarlar.
* Örnek FPS videolar çevrimdışı modda ve çevrimiçi akış test etmek için kullanılır.
* Bir SSS FPS çevrimdışı modu hakkında sorular yanıtlanmaktadır.
