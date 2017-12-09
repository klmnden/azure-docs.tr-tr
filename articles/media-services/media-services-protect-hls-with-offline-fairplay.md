---
title: "HLS çevrimdışı Apple FairPlay - Azure ile içerik koruma | Microsoft Docs"
description: "Bu konu genel bir bakış sağlar ve dinamik olarak Apple FairPlay çevrimdışı modda, HTTP canlı akışı (HLS) içeriğinizle şifrelemek için Azure Media Services kullanmayı gösterir."
services: media-services
keywords: "HLS, DRM, FairPlay akış (FPS), çevrimdışı iOS 10"
documentationcenter: 
author: willzhan
manager: steveng
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: willzhan, dwgeo
ms.openlocfilehash: bf5828ecd6b6bd2e862c4d7709014ecac47c6be0
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="offline-fairplay-streaming"></a>Çevrimdışı FairPlay akış
Microsoft Azure Media Services, iyi tasarlanmış bir dizi sağlar [içerik koruma hizmetleri](https://azure.microsoft.com/services/media-services/content-protection/), kapsayan:
- Microsoft PlayReady
- Google Widevine
- Apple FairPlay
- AES-128 şifrelemesi

DRM/AES şifreleme içerik çeşitli akış protokolleri için istek üzerine dinamik olarak gerçekleştirilir. DRM lisans/AES şifre çözme anahtarı teslimat hizmetlerini de Azure Media Services tarafından sağlanır.

Çevrimiçi çeşitli akış protokolleri akış içeriği koruma yanı sıra, korumalı içerik için Çevrimdışı mod da bir sık istenen özelliğidir. Çevrimdışı modda desteği aşağıdaki senaryolar için gereklidir:
1. Internet bağlantısı sırasında seyahat gibi kullanılamadığında kayıttan yürütme;
2. Bazı içerik sağlayıcıları DRM lisans teslimat bir ülkenin kenarlık ötesinde izin vermeyecek. Bir kullanıcı içeriği yurtdışına seyahat ederken izlemek isterse, çevrimdışı yükleme gereklidir.
3. Bazı ülkelerde Internet kullanılabilirliği ve/veya bant genişliği olduğundan hala sınırlı. Kullanıcılar, tatmin edici görüntüleme deneyimi için yeterince yüksek çözünürlüklü içeriği izlemek için indirmek üzere seçebilirsiniz. Bu durumda, daha sık sorun ağ kullanılabilirliğini değil, yerine sınırlı ağ bant genişliği. Çevrimdışı modda desteği OTT/OVP sağlayıcıları istiyoruz.

Bu makalede iOS 10 veya üstünü çalıştıran cihazları hedefleme FairPlay akış (FPS) çevrimdışı modu desteği yer almaktadır. Bu özellik, diğer Apple platformlarda watchOS, tvOS veya Safari gibi macOS için desteklenmiyor.

## <a name="preliminary-steps"></a>Başlangıç adımları
Çevrimdışı DRM FairPlay için 10 + iOS cihazında uygulamadan önce ilk gerekir:
1. FairPlay için çevrimiçi içerik koruma öğrenmeniz. Bu, aşağıdaki makaleler/samples bölümlerinde ayrıntılı ele alınmıştır:
- [Apple FairPlay akış için Azure Media Services Genel olarak kullanılabilir](https://azure.microsoft.com/blog/apple-FairPlay-streaming-for-azure-media-services-generally-available/)
- [Apple FairPlay veya Microsoft PlayReady ile içerik, HLS koruma](https://docs.microsoft.com/azure/media-services/media-services-protect-hls-with-FairPlay)
- [Çevrimiçi FPS akış için bir örnek](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-FairPlay/)
2. Apple Geliştirici ağdan FPS SDK'sını alın. FPS SDK iki bileşenleri içerir:
- FPS sunucu KSM (anahtar güvenlik modülü), istemci örnekleri, bir belirtim ve test vektörlerinin kümesi içeren SDK;
- FPS dağıtım hakkında yönergeler FPS sertifika, müşteriye özgü özel anahtarı ve uygulama gizli anahtarı (İSTEYİN) oluşturmak birlikte belirtimi D işlevi içeren paketi. Apple FPS dağıtım paketi, yalnızca lisanslı içerik sağlayıcılarına verir.

## <a name="configuration-in-azure-media-services"></a>Azure Media Services yapılandırma
FPS çevrimdışı modda yapılandırması için [Azure Media Services .NET SDK'sı](https://www.nuget.org/packages/windowsazure.mediaservices), Azure Media Services .NET SDK'sı v kullanmanız gerekir. 4.0.0.4 ya da daha sonra hangi FPS çevrimdışı modunu yapılandırmak için gereken API sağlanan olabilir.
Varsayımlar yukarıda gösterildiği gibi çevrimiçi moda FPS içerik koruma yapılandırma çalışma kodu olduğunu varsayın. Çevrimiçi modda içerik koruma FPS için yapılandırma kodunu olduktan sonra aşağıdaki iki değişiklik yeterlidir.

## <a name="code-change-in-fairplay-configuration"></a>FairPlay yapılandırma kod değişikliği
Şimdi çevrimdışı DRM senaryo etkinleştirirken true ise bir "çevrimdışı modu etkinleştir" boolean, çağrılan objDRMSettings.EnableOfflineMode tanımlayın. Bu gösterge bağlı olarak, biz FairPlay yapılandırma aşağıdaki değişikliği yapın:

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

## <a name="code-change-in-asset-delivery-policy-configuration"></a>Kod değişikliği varlık teslim ilkesini yapılandırma
İkinci değişikliği üçüncü anahtar sözlükteki sözlük < AssetDeliveryPolicyConfigurationKey, dize > eklemektir.
Eklenecek üçüncü AssetDeliveryPolicyConfigurationKey ihtiyaçları olacaktır olarak aşağıda: 
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

Bu adımdan sonra aşağıdaki üç girdileri FPS varlık teslim ilkesini < AssetDeliveryPolicyConfigurationKey, dize > sözlükte içerir:
1. AssetDeliveryPolicyConfigurationKey.FairPlayBaseLicenseAcquisitionUrl veya AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl kullanılan FPS KSM/anahtar sunucusu ve aynı varlık teslim yeniden istiyoruz gibi etkenlere bağlı olarak Birden çok varlık arasında İlkesi
2. AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs
3. AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense

Medya Hizmetleri hesabınızı çevrimdışı FairPlay lisansları teslim için yapılandırılmıştır.

## <a name="sample-ios-player"></a>Örnek iOS Player
İlk olarak, FPS çevrimdışı modu desteği yalnızca ios'ta 10 kullanılabilir ve sonraki olduğunu unutmamalısınız. FPS Server SDK almanız gerekir (v3.0 veya sonrası) belge ve örnek FPS çevrimdışı modu için içerir. Özellikle, FPS Server SDK (v3.0 veya sonrası) çevrimdışı moda ilgili aşağıdaki iki öğeleri içerir:
1. Belge: FairPlay akış ile çevrimdışı kayıttan yürütme ve HTTP canlı akış. Apple, 9/14/2016. FPS Server SDK v 4.0'da, bu belge ana FPS akış belgeye birleştirilmedi.
2. Örnek kod: HLSCatalog örnek \FairPlay akış sunucusu SDK v3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\ çevrimdışı modda FPS için. HLSCatalog örnek uygulaması, özellikle çevrimdışı modda özellikleri uygulamak için aşağıdaki kod dosyaları şunlardır:
- AssetPersistenceManager.swift kod dosyası: AssetPersistenceManager olduğunu gösteren bu örnekteki ana sınıfı
    - Kullanıcının cihazını devre dışı varolan varlıkları silme başlatma ve indirme, iptal API'leri gibi indirme HLS akış yönetmek nasıl;
    - Yükleme ilerleme durumunu izlemek nasıl.
- Kod dosyaları AssetListTableViewController.swift ve AssetListTableViewCell.swift: AssetListTableViewController olduğundan bu örnek ana arabirimi. Örnek yürütmek, indirin, silmek veya bir yüklemeyi iptal varlıklar listesini sağlar. 

Çalışan bir iOS oynatıcı için ayrıntılı adımlar aşağıda verilmiştir. 4.0.1 FPS Server SDK V'de HLSCatalog örnekten Başlat varsayalım.  Aşağıdaki kod değişikliklerini yapmanız gerekir:

HLSCatalog\Shared\Managers\ContentKeyDelegate.swift içinde yöntemi uygulaması `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` aşağıdaki kodu kullanarak: izin drmUr akış URL'si HLS atanmış bir değişken olmalıdır.

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

HLSCatalog\Shared\Managers\ContentKeyDelegate.swift içinde yöntemi uygulaması `requestApplicationCertificate()`. Bu uygulama sertifikası (yalnızca ortak anahtar) aygıtıyla katıştırmak mı web sertifikadaki ana bilgisayar bağlıdır. Aşağıda test örneklerimizi kullanılan barındırılan uygulama sertifikasını kullanarak bir uygulamasıdır. Uygulama sertifikasını URL'sini içeren bir değişken certUrl olanak tanır.

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

Son tümleşik test için video URL'si ve uygulama sertifika URL'si "Tümleşik Test" bölümünde sağlanır.

HLSCatalog\Shared\Resources\Streams.plist içinde test video URL'nizi ekleyin ve içerik anahtarı kimliği, yalnızca kullanabileceğiniz FairPlay lisans edinme URL'si skd protokolü ile benzersiz değer olarak.

![Çevrimdışı FairPlay iOS App akışlar](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-ios-app-streams.png)

Test video URL'si, FairPlay lisans edinme URL'si ve uygulama sertifika URL'si için kendi kullanabilirsiniz, varsa bunları ayarlamanız veya test örnekleri içeren bir sonraki bölüme devam edebilirsiniz.

## <a name="integrated-test"></a>Tümleşik Test
Azure Media Services aşağıdaki üç senaryolarını kapsamak üç test örnekleri ayarlanan:
1.  , Video, ses ve diğer ses parça ile korunan FPS;
2.  Video, ses ancak alternatif ses parçası korunan FPS;
3.  FPS, yalnızca, video ile korunan ses yok.

Şu anda bu örnekler bulunabilir [gösteri sitesi](http://aka.ms/poc#22), karşılık gelen uygulama sertifikayla barındırılan bir Azure web uygulaması.
Ana çalma çevrimdışı modda sırasında alternatif ses içeriyorsa ile v3 veya v4 örnek FPS Server SDK'ın yalnızca ses çalınır, fark etmiş. Bu nedenle biz alternatif ses çıkarabilmesi gerekir. Diğer bir deyişle, (2) yukarıdaki üç örnek ve (3) arasında hem çevrimiçi hem de çevrimdışı modda çalışmaz. Ancak yalnızca çevrimdışı modda çalışırken çevrimiçi akış works sırasında düzgün olarak ses kaydını oynatın (1) olacaktır.

## <a name="faq"></a>SSS
Sorun giderme için sık sorulan bazı sorular:
- **Neden çevrimdışı modu sırasında yalnızca ses çalma ancak hiçbir video mu?** Bu davranış, örnek uygulamayı tasarım gereği görünüyor. Alternatif ses parçası olduğunda (HLS olduğu durum), çevrimdışı modda sırasında hem iOS 10 sunmak ve iOS 11 alternatif ses kaydı varsayılan olarak kabul eder. Bu davranış FPS çevrimdışı modu için dengelemek için biz alternatif ses izleme akıştan kaldırmanız gerekir. Azure Media Services tarafında bunun için yalnızca dinamik bildirim filtresi ekleyebiliriz "yalnızca ses = false." Diğer bir deyişle, HLS URL .ism/manifest(format=m3u8-aapl,audio-only=false) ile biter olacaktır. 
- **Neden hala oynatmak yalnızca video ses çevrimdışı modu sırasında yalnızca ses eklendikten sonra = false?** CDN önbellek temel tasarım bağlı olarak, içerik önbelleğe. Önbelleği temizlemek gerekir.
- **FPS Çevrimdışı mod da iOS 10 yanı sıra iOS 11 destekleniyor mu?** Evet, FPS çevrimdışı modda iOS 10 ve 11 iOS için desteklenir.
- **Çevrimdışı kayıttan yürütme FairPlay akış ve HTTP canlı akış ile belge FPS sunucu SDK'da neden bulamıyorum?** FPS Server SDK v 4 itibaren bu belgede FairPlay akış programlama kılavuzu belgeye birleştirilmedi.
- **Ne son parametre FPS çevrimdışı modu aşağıdaki API'sindeki göze?**
`Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(objX509Certificate2, pfxPassword, pfxPasswordId, askId, iv, RentalAndLeaseKeyType.PersistentUnlimited, 0x9999);`

Bu API belgelerine bulunabilir [burada](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.mediaservices.client.FairPlay.FairPlayconfiguration.createserializedFairPlayoptionconfiguration?view=azure-dotnet). Parametre birimi olarak saat ile çevrimdışı kiralama süresini temsil eder.
- **İOS cihazlarda yüklenen/çevrimdışı dosya yapısı nedir?** Bir iOS cihazında indirilen dosya yapısı (ekran görüntüsü) gibi görünüyor. `_keys`klasör depoları FPS lisansları, her lisans hizmeti ana bilgisayarı için bir deposu dosyası indirilir. `.movpkg`klasör, ses ve video içeriğine depolar. Sayısal tarafından izlenen bir tire ile biten adda ilk klasör video içeriği içerir. Sayısal değer video yorumlama "PeakBandwidth" dir. 0 ve ardından bir tire ile biten ada sahip ikinci klasör ses içeriğini içerir. "Data" adlı üçüncü klasörü FPS içerik ana çalma listesi içerir. Boot.XML ilişkin kapsamlı bir açıklama sağlar `.movpkg` klasör içeriği (örnek boot.xml dosyası için aşağıya bakın).

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
Bu belgede ayrıntılı adımlar ve bilgi FPS çevrimdışı modda, uygulama için sağladık dahil olmak üzere:
1. AMS .NET API aracılığıyla Azure medya Hizmetleri içerik koruma yapılandırması. Bu seçenek, dinamik FairPlay şifreleme ve FairPlay lisans teslimat AMS yapılandırır.
2. iOS player Apple FPS Server SDK örnekten göre. Bu FPS yürütebilen bir iOS player akış modu çevrimiçi veya çevrimdışı modda içerik ayarlamanız gerekir.
3. Çevrimdışı modda ve çevrimiçi akış test etmek için örnek FPS videolar.
4. FPS çevrimdışı modu hakkında SSS.
