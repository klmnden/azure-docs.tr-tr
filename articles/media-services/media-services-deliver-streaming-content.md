---
title: ".NET kullanarak Azure Media Services içerik yayımlama | Microsoft Docs"
description: "Bir akış URL'si oluşturmak için kullanılan bir Bulucu oluşturmayı öğrenin. Kod örnekleri, C# dilinde yazılmıştır ve .NET için Media Services SDK'sını kullanın."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: c53b1f83-4cb1-4b09-840f-9c145b7d6f8d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 979c88b72aba6e054bc507e22f48cae1441957cb
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="publish-azure-media-services-content-using-net"></a>.NET kullanarak Azure Media Services içerik yayımlama
> [!div class="op_single_selector"]
> * [REST](media-services-rest-deliver-streaming-content.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [Portal](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Bir OnDemand akış Bulucusu oluşturma ve akış URL'si oluşturma MP4 kümesine bir bit hızı Uyarlamalı akışını sağlayabilirsiniz. [Bir varlık kodlama](media-services-encode-asset.md) konu nasıl kodlanacağını Uyarlamalı bit hızlı MP4 kümesi gösterir. 

> [!NOTE]
> İçeriğinizi şifrelenmişse, varlık teslim ilkesini yapılandırın (açıklandığı gibi [bu](media-services-dotnet-configure-asset-delivery-policy.md) konu) bir Bulucu oluşturmadan önce. 
> 
> 

Bir OnDemand Bulucu akış, aşamalı olarak indirilebilir MP4 dosyaları işaret URL'ler oluşturmak için de kullanabilirsiniz.  

Bu konuda bir OnDemand Bulucu, varlığı yayımlayın ve kesintisiz, MPEG DASH ve HLS akış URL'lerini oluşturmak için akış oluşturulacağını gösterir. Aşamalı indirme URL'leri oluşturmak için etkin gösterir. 

## <a name="create-an-ondemand-streaming-locator"></a>Bir OnDemand akış Bulucusu oluşturma
OnDemand akış Bulucusu oluşturmak ve URL'leri almak için şunları yapmanız gerekir:

1. İçerik şifrelenmişse, bir erişim ilkesi tanımlayın.
2. Bir OnDemand Bulucu akış oluşturun.
3. Akış yapmayı planlıyorsanız, varlık içindeki akış bildirim dosyası (.ism) alın. 
   
   Aşamalı indirmeyi planlıyorsanız, varlık MP4 dosyaları adlarını alır.  
4. URL'ler bildirim dosyası veya MP4 dosyaları oluşturun. 


>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Aynı gün / erişim izinleri her zaman aynı ilke kimliği kullanın. Örneğin, ilkeleri kalmasına yerinde uzun bir süre (karşıya yükleme olmayan ilkeleri) yöneliktir bulucular için. Daha fazla bilgi için [bu](media-services-dotnet-manage-entities.md#limit-access-policies) konu başlığına bakın.

### <a name="use-media-services-net-sdk"></a>Media Services .NET SDK'yı kullanın
Akış URL'leri derleme 

```csharp
    private static void BuildStreamingURLs(IAsset asset)
    {

        // Create a 30-day readonly access policy. 
          // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();

        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }
```

Çıktı:

    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> Ayrıca, bir SSL bağlantısı üzerinden içeriğinizin akışını sağlayabilirsiniz. Bu yaklaşım yapmak için HTTPS ile akış URL'leri başlatma emin olun. Şu anda AMS SSL ile özel etki alanlarını desteklemiyor.
> 
> 

Aşamalı indirme URL'leri derleme 

```csharp
    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }
```
Çıktı:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a>Media Services .NET SDK uzantıları kullanma
Aşağıdaki kod bir Bulucu oluşturmanız ve Uyarlamalı akış için kesintisiz akış, HLS ve MPEG-DASH URL'ler oluşturmak .NET SDK uzantıları yöntemleri çağırır.
```csharp
    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));

    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();

    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);
```

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Varlıklar indirin](media-services-deliver-asset-download.md)
* [Varlık teslim ilkesini yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md)

