---
title: .NET kullanarak Azure Media Services içerik yayımlama | Microsoft Docs
description: Akış URL'si oluşturmak için kullanılan bir Bulucu oluşturmayı öğrenin. İçinde yazılan kod örneklerini C# ve .NET için Media Services SDK'sını kullanın.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: c53b1f83-4cb1-4b09-840f-9c145b7d6f8d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: b1d0c070a9196eaa9a2706a607baa9a2926e2db4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051753"
---
# <a name="publish-media-services-content-using-net"></a>.NET ile Media Services içerik yayımlama  
> [!div class="op_single_selector"]
> * [REST](media-services-rest-deliver-streaming-content.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [Portal](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Bir OnDemand akış Bulucusu oluşturma ve bir akış URL'si oluşturma bir hızı Uyarlamalı MP4 kümesine akışını yapabilirsiniz. [Bir varlığı kodlama](media-services-encode-asset.md) konu başlığı altında Uyarlamalı bit hızı MP4 kümesi kodlama gösterilir. 

> [!NOTE]
> İçeriğinizi şifrelenmişse, varlık teslim ilkesini yapılandırın (açıklandığı [bu](media-services-dotnet-configure-asset-delivery-policy.md) konu) önce bir Bulucu oluşturma. 
> 
> 

Bir OnDemand akış Bulucusu, aşamalı olarak indirilebilir MP4 dosyasına işaret eden URL'leri oluşturmak için de kullanabilirsiniz.  

Bu konu, bir OnDemand akış Bulucusu, varlığı yayımlayın ve kesintisiz, MPEG DASH ve HLS akış URL'lerini oluşturmak için nasıl oluşturulacağını gösterir. Aşamalı indirme URL'lerini oluşturmak için sık erişimli gösterir. 

## <a name="create-an-ondemand-streaming-locator"></a>Bir OnDemand akış Bulucusu oluşturma
OnDemand akış Bulucusu oluşturmak ve URL'leri almak için şunları yapmanız gerekir:

1. İçeriğin şifreli değilse, bir erişim ilkesi tanımlayın.
2. Bir OnDemand akış Bulucusu oluşturma.
3. Akış planlıyorsanız, varlık, akış bildirim dosyası (.ism) alın. 
   
   Aşamalı indirmeyi planlıyorsanız, varlık MP4 dosyalarının adlarını alın.  
4. Bildirim dosyası veya MP4 dosyaları için URL'leri oluşturun. 


>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Aynı günleri / erişim izinlerini her zaman aynı ilke Kimliğini kullanın. Örneğin, ilkeleri kalmasına yerinde uzun bir süredir (karşıya yükleme olmayan ilkeler) yöneliktir bulucular için. Daha fazla bilgi için [bu](media-services-dotnet-manage-entities.md#limit-access-policies) konu başlığına bakın.

### <a name="use-media-services-net-sdk"></a>Media Services .NET SDK'sını kullanın
Akış URL'leri oluşturun 

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
        var manifestFile = asset.AssetFiles.ToList().Where(f => f.Name.ToLower().
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

Çıkışlar:

    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> Ayrıca, bir SSL bağlantısı üzerinden içeriğinizin akışını yapabilirsiniz. Bu yaklaşım yapmak için HTTPS ile akış URL'leri başlatma emin olun. Şu anda AMS SSL ile özel etki alanlarını desteklemiyor.
> 
> 

Aşamalı indirme URL'leri oluşturun 

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
Çıkışlar:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a>Media Services .NET SDK uzantıları kullanma
Aşağıdaki kod, bir Bulucu oluşturmanız ve Uyarlamalı akış için kesintisiz akış, HLS ve MPEG-DASH URL'leri oluşturacak .NET SDK uzantıları yöntemleri çağırır.
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
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Varlıklar indirin](media-services-deliver-asset-download.md)
* [Varlık teslim ilkesini yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md)

