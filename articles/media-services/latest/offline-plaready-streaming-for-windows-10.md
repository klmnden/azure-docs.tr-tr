---
title: Çevrimdışı PlayReady korumalı içeriği - Azure akış için hesabınızı yapılandırın
description: Bu makalede, Windows 10 için PlayReady çevrimdışı akış için Azure Media Services hesabının nasıl yapılandırılacağı gösterilmektedir.
services: media-services
keywords: DASH, DRM, Widevine çevrimdışı modda ExoPlayer, Android
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/01/2019
ms.author: willzhan
ms.openlocfilehash: ae5fdd51d9bc1a3e7e2521c6ca1ff64d884c96f8
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341773"
---
# <a name="offline-playready-streaming-for-windows-10"></a>Windows 10 için akış çevrimdışı PlayReady

Azure Media Services, çevrimdışı yükleme/oynatma DRM koruması desteği. Bu makale Azure medya Hizmetleri için Windows 10/PlayReady istemcilerin çevrimdışı destek kapsar. İOS/FairPlay ve aşağıdaki makalelerde Android/Widevine cihazlar için çevrimdışı modu desteği hakkında okuyabilirsiniz:

- [iOS için Çevrimdışı FairPlay Akışı](offline-fairplay-for-ios.md)
- [Android için akış çevrimdışı Widevine](offline-widevine-for-android.md)

> [!NOTE]
> Çevrimdışı DRM yalnızca tek bir istek için bir lisans içeriği indirdiklerinde yapmak için faturalandırılır. Hataları faturalandırılmaz.

## <a name="overview"></a>Genel Bakış

Özellikle çevrimdışı modda kayıttan yürütme, bu bölümde bazı arka plan verir neden:

* Bazı ülkeler/bölgeler içinde Internet kullanılabilirliği ve/veya bant genişliği, hala sınırlıdır. Kullanıcılar indirmeniz tatmin edici bir görüntüleme deneyimi için yeterince yüksek çözünürlükte içerik izleyebilirler tercih edebilirsiniz. Bu durumda, daha sık, ağ kullanılabilirliği bir sorun değildir, bunun yerine sınırlı ağ bant genişliği. OTT/OVP sağlayıcıları için çevrimdışı modu desteği seçmenizi istiyoruz.
* Netflix 2016 Q3 hissedarlar konferansında duyurulmuş gibi içerik indirme bir "önemlisi istenen" özelliğidir ve Netflix CEO Reed Hastings tarafından "biz için açık olan" dedi.
* Bazı içerik sağlayıcıları DRM lisans teslimat ötesinde bir ülke/bölgenin kenarlık izin verme. Bir kullanıcının kuruluşunun seyahat gerekir ve içerik izlemek ister, çevrimdışı yükleme gereklidir.
 
Çevrimdışı modda uygulama yüz sınama aşağıda verilmiştir:

* MP4 birçok oynatıcılar, kodlayıcı araçlar tarafından desteklenir, ancak hiçbir bağlama yok MP4 kapsayıcı DRM; arasındaki
* Uzun vadede CFF CENC ile Git yoludur. Bununla birlikte, bugün, Araçlar/player destek ekosistemi henüz yok. Çözüm, bugün ihtiyacımız var.
 
Fikirdir: kesintisiz akış ([PIFF](https://go.microsoft.com/?linkid=9682897)) H264/AAC ile dosya biçimine sahip bir bağlamayla PlayReady (AES-128 CTRL). (Ses, video karışık olduğunu varsayarak) bireysel kesintisiz akış .ismv dosya kendisi bir fMP4 ve kayıttan yürütme için kullanılabilir. Kesintisiz akış içerik üzerinden PlayReady şifreleme aşması durumunda, her .ismv dosya korumalı bir PlayReady olur parçalanmış MP4. Tercih edilen hızı ile bir .ismv dosyasını seçin ve indirme için .mp4 olarak yeniden adlandırın.

PlayReady barındırma MP4 korumalı aşamalı indirme için iki seçenek vardır:

* Biri bu MP4 aynı kapsayıcı/medya hizmeti varlığı yerleştirme ve Azure Media Services akış uç noktası için aşamalı indirme yararlanın;
* Azure depolama, Azure Media Services atlayarak doğrudan aşamalı indirmek için bir SAS Bulucu kullanabilirsiniz.
 
PlayReady lisans dağıtımı iki tür kullanabilirsiniz:

* Azure Media Services PlayReady lisans teslimat hizmetinin;
* Herhangi bir yerde barındırılan PlayReady lisans sunucuları.

Aşağıda test varlıklar, ilk iki kümesi olan bir Azure sanal makinesinde barındırılan PlayReady lisans sunucumu kullanarak ikinci bir yandan AMS PlayReady lisans dağıtımı kullanarak:

Varlık #1:

* Aşamalı indirme URL'si: [https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4](https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4")
* PlayReady LA_URL (AMS): [https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/](https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/)

Varlık #2:

* Aşamalı indirme URL'si: [https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4](https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500_H264_1644kbps_AAC_und_ch2_256kbps.mp4)
* PlayReady LA_URL (şirket içi): [https://willzhan12.cloudapp.net/playready/rightsmanager.asmx](https://willzhan12.cloudapp.net/playready/rightsmanager.asmx)

Kayıttan yürütme test etmek için bir evrensel Windows uygulaması Windows 10'da kullandık. İçinde [Windows 10 Evrensel örnekleri](https://github.com/Microsoft/Windows-universal-samples), adlı bir temel player örneği [Uyarlamalı akış örnek](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AdaptiveStreaming). Tüm yapılacağını sahibiz gelir bizim yüklenen görüntü çekme ve Uyarlamalı akış kaynağı yerine bir kaynak olarak kullanmak için kodu ekleyin. Değişiklikler düğmesine tıklama olay işleyicisi:

```csharp
private async void LoadUri_Click(object sender, RoutedEventArgs e)
{
    //Uri uri;
    //if (!Uri.TryCreate(UriBox.Text, UriKind.Absolute, out uri))
    //{
    // Log("Malformed Uri in Load text box.");
    // return;
    //}
    //LoadSourceFromUriTask = LoadSourceFromUriAsync(uri);
    //await LoadSourceFromUriTask;

    //willzhan change start
    // Create and open the file picker
    FileOpenPicker openPicker = new FileOpenPicker();
    openPicker.ViewMode = PickerViewMode.Thumbnail;
    openPicker.SuggestedStartLocation = PickerLocationId.ComputerFolder;
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".ismv");
    //openPicker.FileTypeFilter.Add(".mkv");
    //openPicker.FileTypeFilter.Add(".avi");

    StorageFile file = await openPicker.PickSingleFileAsync();

    if (file != null)
    {
       //rootPage.NotifyUser("Picked video: " + file.Name, NotifyType.StatusMessage);
       this.mediaPlayerElement.MediaPlayer.Source = MediaSource.CreateFromStorageFile(file);
       this.mediaPlayerElement.MediaPlayer.Play();
       UriBox.Text = file.Path;
    }
    else
    {
       // rootPage.NotifyUser("Operation cancelled.", NotifyType.ErrorMessage);
    }

    // On small screens, hide the description text to make room for the video.
    DescriptionText.Visibility = (ActualHeight < 500) ? Visibility.Collapsed : Visibility.Visible;
}
```

![FMP4 çevrimdışı moda kayıttan yürütme PlayReady korumalı](./media/offline-playready-for-windows/offline-playready1.jpg)


Ekran görüntüsü, videonun altında PlayReady korumalı olduğundan, videoyu eklemek mümkün olmayacaktır.

Özet olarak, Azure Media Services'da çevrimdışı modda alanımız:

* İçerik kodlama dönüştürme ve PlayReady şifrelemesi, Azure Media Services veya diğer araçları yapılabilir;
* İçeriği aşamalı indirme için Azure Media Services veya Azure depolama alanında barındırılabilir;
* PlayReady lisans dağıtımı, Azure Media Services'dan veya başka bir yerde olabilir;
* Hazırlanan kesintisiz akış içeriği, DASH çevrimiçi akış için hala kullanılabilir veya PlayReady DRM olarak ile kesintisiz.

## <a name="next-steps"></a>Sonraki adımlar

[Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)
