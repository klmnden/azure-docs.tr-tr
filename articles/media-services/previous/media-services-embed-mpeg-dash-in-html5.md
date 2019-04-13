---
title: MPEG-DASH Uyarlamalı Akış videosunu bir HTML5 uygulamasına DASH.js ile ekleme | Microsoft Docs
description: Bu konuda, MPEG-DASH Uyarlamalı Akış videosunu bir HTML5 uygulamasına DASH.js ile katıştırmak gösterilmiştir.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 5aa0e7b6-f5c3-4cc1-aa33-ed16ea4780c2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: f521fd11a2053cf8cf1ea0f9f91667fe475f0eee
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59522444"
---
# <a name="embedding-an-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>MPEG-DASH Uyarlamalı Akış videosunu bir HTML5 uygulamasına DASH.js ile ekleme  

## <a name="overview"></a>Genel Bakış
MPEG-DASH Uyarlamalı akış yüksek kaliteli, Uyarlamalı video çıkışının akışını teslim isteyen geliştiriciler için önemli avantajlar sunan video içeriğinin bir ISO standardıdır. Ağ Tıkanıklığı olur, MPEG-DASH ile video akışı için daha düşük bir tanımı otomatik olarak ayarlar. Bu, "duraklatılmış" video oynatıcı (arabelleğe alma olarak da bilinir) yürütmek için sonraki birkaç saniye indirirken görmeye Görüntüleyicisi olasılığını azaltır. Ağ Tıkanıklığı azaltır gibi video oynatıcı sırayla daha yüksek kaliteli akışına döndürür. Gereken bant genişliği uyarlama olanağı da video için daha hızlı bir başlangıç saati sonuçlanır. Bu, ilk birkaç saniyede bir hızlı indirme daha düşük kaliteli kesimde yürütülebilecek ve sonra adım daha yüksek kaliteli bir kez yeterli içerik kadar arabelleğe alındı anlamına gelir.

Dash.js JavaScript'te yazılmış bir açık kaynak MPEG-DASH video bir oyuncu bulunur. Hedefi, ücretsiz video kayıttan yürütme gerektiren uygulamalarda yeniden kullanılabilen sağlam, platformlar arası bir oynatıcı sağlamaktır. Bunu, Chrome, Microsoft Edge ve ıe11 (diğer tarayıcılar MSE desteklemek için bunların amacı belirttiyseniz) Bugün W3C medya kaynağı Uzantıları (MSE) destekleyen herhangi bir tarayıcıda MPEG-DASH oynatma sağlar. DASH.js hakkında daha fazla bilgi için js dash.js GitHub deposuna bakın.

## <a name="creating-a-browser-based-streaming-video-player"></a>Bir tarayıcı tabanlı akış video oynatıcı oluşturma
Bir video oynatıcı ile beklenen görüntüleyen basit bir web sayfası oluşturmak için bu tür bir play, duraklatma, geri sarma vb. denetler, şunları yapmanız gerekir:

1. Bir HTML sayfası oluşturma
2. Görüntü Etiketi Ekle
3. Dash.js player Ekle
4. Oyuncu Başlat
5. Bazı CSS stil ekleme
6. Sonuçları MSE uygulayan bir tarayıcıda görüntüle

Oyuncu başlatma, yalnızca birkaç satır JavaScript kod içinde tamamlanabilir. Dash.js kullanarak bu gerçekten tarayıcı tabanlı uygulamalarınızı MPEG-DASH video eklemek bu basit bir işlemdir.

## <a name="creating-the-html-page"></a>HTML sayfası oluşturma
Standart bir HTML sayfası içeren oluşturmak için ilk adımıdır **video** öğesi, bu dosyayı basicPlayer.html, aşağıdaki örnekteki gibi farklı kaydet gösterilmektedir:

```html
    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>
```

## <a name="adding-the-dashjs-player"></a>DASH.js Player ekleme
Dash.js başvuru uygulaması için uygulama eklemek için en son sürümünü dash.js proje dash.all.js dosyasından almak gerekir. Bu uygulamanızın JavaScript klasöründe kaydedilmelidir. Bu dosya birlikte tek bir dosya halinde tüm gerekli dash.js kodu çeker kolaylık dosyasıdır. Dash.js depo bir görünüm varsa, tek tek dosyaları, kod ve daha fazlasını test, ancak yapmak istiyorsanız, tüm olan dash.js, ihtiyacınız olanları dash.all.js dosya ise.

Dash.js player uygulamalarınıza eklemek için bir komut dosyası etiketini basicPlayer.html baş bölümüne ekleyin:

```html
    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>
```

Ardından, oyuncunun sayfa yüklendiğinde başlatmak için bir işlev oluşturun. Dash.all.js yüklediğiniz satırın sonunda aşağıdaki betiği ekleyin:

```html
    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>
```

Bu işlev, ilk bir DashContext oluşturur. Bu uygulama için belirli bir çalışma zamanı ortamı yapılandırmak için kullanılır. Bir teknik bakış açısıyla, bağımlılık ekleme framework uygulama oluştururken kullanması gereken sınıfları tanımlar. Çoğu durumda, Dash.di.DashContext kullanın.

Ardından, MediaPlayer dash.js framework'ün birincil sınıfı örneği. Bu sınıf, gerektiği gibi yöntemleri yürütmek ve duraklatmak, video öğeyle ilişkiyi yönetir ve yorumu yürütülecek video açıklar medya sunu açıklaması (MPD) dosyasının da yönetir çekirdek içerir.

MediaPlayer sınıfı başlangıç() işlev, oyuncunun video oynatma hazır olduğundan emin olmak için çağrılır. Diğerlerinin yanı sıra, gerekli tüm sınıflar (bağlam tarafından tanımlandığı şekilde) olarak yüklenmiş olan işlev sağlar. Oyuncu hazır hale geldikten sonra video öğesini attachView() işlevi kullanarak ekleyebilirsiniz. MediaPlayer video akışı öğesine ekleme ve ayrıca gerekli olarak kayıttan yürütmeyi denetlemek başlangıç işlevi sağlar.

Bu videoyu oynatmak için beklenen bilebilmesi için MediaPlayer MPD dosyasının URL'sini geçirin. Yeni oluşturduğunuz setupVideo() işlevi, sayfa tam olarak yüklendikten sonra yürütülecek gerekir. Gövde öğesinin yüklendiğinde olayı kullanarak bunu. Değişiklik, `<body>` öğesi:

```html
    <body onload="setupVideo()">
```

Son olarak, CSS kullanarak video öğesini boyutunu ayarlayın. Bir Uyarlamalı akış ortamda boyutu oynatılan videonun kayıttan yürütme, değişen ağ koşullarını uyum sağlayarak değişebilir olduğundan bu özellikle önemlidir. Basit bu tanıtımda, yalnızca % 80'kullanılabilir tarayıcı penceresini aşağıdaki CSS sayfasının baş bölümüne ekleyerek video öğenin zorlayın:

```html
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>
```

## <a name="playing-a-video"></a>Video oynatma
Bir videoyu oynatmak için tarayıcınızı basicPlayback.html dosyasına işaret ve görüntülenen video oynatıcı play'den tıklayın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Video oynatıcı uygulamaları geliştirme](media-services-develop-video-players.md)

[GitHub dash.js deposu](https://github.com/Dash-Industry-Forum/dash.js) 

