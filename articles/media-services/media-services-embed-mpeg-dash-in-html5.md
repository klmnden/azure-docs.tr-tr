---
title: "DASH.js ile HTML5 uygulamada MPEG-DASH Uyarlamalı Akış Video katıştırma | Microsoft Docs"
description: "Bu konu, HTML5 uygulamayla DASH.js MPEG-DASH Uyarlamalı Akış Video ekleme gösterilmiştir."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 5aa0e7b6-f5c3-4cc1-aa33-ed16ea4780c2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 27ce6325773ba1f9fd9cd9ab9e07ea9f5e2488ac
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>DASH.js ile HTML5 uygulamada MPEG-DASH Uyarlamalı Akış Video katıştırma
## <a name="overview"></a>Genel Bakış
MPEG-DASH, çıkış akış yüksek kaliteli, Uyarlamalı video teslim etmek istediğiniz olanlar için önemli avantajlar sunar video içeriği Uyarlamalı akış için bir ISO standardıdır. Ağ yoğun hale geldiğinde MPEG-DASH ile video akışına otomatik olarak bir alt tanımına bırakılmasına neden olacak. Bu, "Duraklatıldı" bir video oynatıcı (diğer adıyla arabelleğe alma) yürütmek için sonraki birkaç saniye indirirken görmesini Görüntüleyicisi olasılığını azaltır. Ağ Tıkanıklığı azaltır gibi video oynatıcı sırayla daha yüksek bir kalite akışına döndürür. İstenen bant genişliği uyarlama olanağı da video için daha hızlı bir başlangıç saati sonuçlanır. İlk birkaç saniye içinde hızlı yükleme alt kalite kesimi çalınabilir ve daha yüksek kaliteli bir kez yeterli içerik kadar adım bir sonra arabelleğe anlamına gelir.

Dash.js JavaScript'te yazılmış bir açık kaynak MPEG-DASH video bir oyuncu bulunur. Amacı serbestçe video oynatmayı gerektiren uygulamalar içinde yeniden kullanılabilir bir güçlü, platformlar arası oynatıcı sağlamaktır. MPEG-DASH kayıttan yürütme, Chrome, Microsoft Edge ve (diğer tarayıcılarda MSE desteklemek için kendi hedefi belirttiyseniz) IE11 W3C medya kaynağı Uzantıları (MSE) Bugün destekleyen herhangi bir tarayıcıda sağlar. DASH.js hakkında daha fazla bilgi için GitHub dash.js depo js bakın.

## <a name="creating-a-browser-based-streaming-video-player"></a>Bir tarayıcı tabanlı akış video oynatıcı oluşturma
Bir video oynatıcı beklenen ile görüntüleyen basit bir web sayfası oluşturmak için bu tür bir yürütme, duraklatma, Geri Sar vb. denetimleri, yapmanız gerekir:

1. Bir HTML sayfası oluşturun
2. Video etiket ekleme
3. Dash.js player ekleme
4. Player başlatma
5. Bazı CSS stil ekleme
6. MSE uygulayan bir tarayıcıda sonuçları görüntüleme

Player başlatma yalnızca JavaScript kod satırlarını sayıda içinde tamamlanabilir. Dash.js kullanarak gerçekten tarayıcı tabanlı uygulamalarınızda MPEG-DASH video ekleme, basit değildir.

## <a name="creating-the-html-page"></a>HTML sayfası oluşturma
Standart bir HTML sayfası içeren oluşturmak için ilk adımdır **video** bu dosyayı aşağıdaki örnekteki gibi basicPlayer.html Farklı Kaydet öğesini gösterir:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

## <a name="adding-the-dashjs-player"></a>DASH.js Player ekleme
Uygulama dash.js başvuru uygulaması eklemek için dash.js proje 1.0 sürümü dash.all.js dosyasından alın gerekir. Bu, uygulamanızın JavaScript klasöründe kaydedilmelidir. Bu dosya birlikte tek bir dosyaya tüm gerekli dash.js kodu çeken bir kolaylık dosyasıdır. Dash.js depo bir görünüm varsa, tek tek dosyaları bulmak, kod ve çok daha fazlasını test ancak tüm yapmak istiyorsanız kullanım dash.js olan sonra gerekenler dash.all.js dosyasıdır.

Dash.js player uygulamalarınıza eklemek için bir komut dosyası etiketinin basicPlayer.html baş bölümüne ekleyin:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Ardından, sayfa yüklendiğinde player başlatmak için bir işlev oluşturun. Aşağıdaki komut dosyası dash.all.js yük satırından sonra ekleyin:

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

Bu işlev bir DashContext ilk oluşturur. Bu uygulama için belirli çalışma zamanı ortamı yapılandırmak için kullanılır. Bir teknik açısından bakıldığında, bağımlılık ekleme framework uygulama oluştururken kullanması gereken sınıfları tanımlar. Çoğu durumda, Dash.di.DashContext kullanır.

Ardından, MediaPlayer dash.js framework'ün birincil sınıfının örneği. Bu sınıf gibi gerekli yöntemleri Yürüt ve duraklatmak, video öğesi olan yönetir ve ayrıca çalınacak video tanımlayan medya sunu açıklaması (MPD) dosya yorumu yönetir çekirdek içerir.

MediaPlayer sınıfının startup() işlev player video yürütmek hazır olduğundan emin olmak için çağrılır. Diğer işlemlerin arasında gerekli tüm sınıflar (bağlam tarafından tanımlandığı gibi) olarak yüklenmiş olan bu işlev sağlar. Player hazır olduktan sonra attachView() işlevi ile video öğesi ekleyebilirsiniz. Bu öğeye video akışına ekleme ve ayrıca gerektiği gibi kayıttan yürütme denetlemek MediaPlayer sağlar.

Böylece yürütmek için beklenen ilgili video bilir MPD dosyanın URL'sini MediaPlayer geçirin. Yeni oluşturduğunuz setupVideo() işlevi, sayfa tam olarak yüklendikten sonra yürütülecek gerekir. Gövde öğesinin yüklendiğinde olayı kullanarak bunu. Değişiklik, <body> öğesine:

    <body onload="setupVideo()">

Son olarak, CSS kullanarak video öğesi boyutunu ayarlayın. Uyarlamalı akış ortamında, değişen ağ koşullarını kayıttan yürütme uyum olarak yürütülen video boyutunu değiştirme çünkü bu özellikle önemlidir. Bu basit tanıtım sayfası merkez bölümüne aşağıdaki CSS ekleyerek kullanılabilir tarayıcı penceresinin % 80 olarak video öğesi yalnızca zorla:

    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

## <a name="playing-a-video"></a>Bir Video oynatma
Bir videoyu yürütmek için tarayıcınızı basicPlayback.html dosyasına işaret ve görüntülenen video oynatıcı çalmayı'ı tıklatın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Video oynatıcı uygulamaları geliştirme](media-services-develop-video-players.md)

[GitHub dash.js deposu](https://github.com/Dash-Industry-Forum/dash.js) 

