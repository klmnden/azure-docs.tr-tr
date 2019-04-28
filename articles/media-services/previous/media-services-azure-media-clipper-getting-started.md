---
title: Azure Media Clipper'ı kullanmaya başlama | Microsoft Docs
description: AMS varlıkları video küçük resimleri oluşturmaya yönelik bir araç olan Azure Media Clipper kullanmaya başlama
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 03/14/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 51848b9ba4d18b3ac7d652cfbd97cab6b85f2ee8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61466281"
---
# <a name="create-clips-with-azure-media-clipper"></a>Küçük resimleri ile Azure Media Clipper'ı oluşturma
Bu bölümde, Azure Media Clipper'ı kullanmaya başlama hakkında temel adımları gösterir. İzleyen bölümlerde Azure Media Clipper'ı yapılandırma konusunda ayrıntıları sağlayın.

- İlk olarak, aşağıdaki bağlantıları için belgenizin head için Azure Media Player'ı ve Azure Media Clipper'ı ekleyin. Kırpıcıyı açar ve Azure Media Player URL'lerinde açıkça belirtilmesi önerilir. İsteğe bağlı olarak değişebilir oldukları gibi bu kaynaklar en son sürümü üretim ortamında kullanmayın.

```javascript
<!--Azure Media Player 2.1.4 or later is a prerequisite-->
<link href="//amp.azure.net/libs/amp/2.1.4/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amp/2.1.4/azuremediaplayer.min.js"></script>
<!--Azure Media Clipper script and stylesheet-->
<link href="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.min.js"></script>
```

- Ardından, aşağıdaki sınıflar Clipper örneklemek için istediğiniz div öğesine ekleyin.

```javascript
<div id="root" class="azure-subclipper" />
```

İsteğe bağlı olarak, koyu tema etkinleştirmek için koyu kaplama sınıfı ekleyin:

```javascript
<div id="root" class="azure-subclipper dark-skin" />
```

- Ardından, aşağıdaki API çağrısı ile Clipper'ı örneği:

```javascript
var subclipper = new subclipper({
    selector: '#root',
    logLevel: 'info',
    restVersion: '2.0',
    minimumMarkerGap: 6,
    submitSubclipCallback: onSubmitSubclip,
    singleBitrateMp4Profile: {
            Codecs: [{
                    // Video Codec with single H264Layers
                }, {
                    // Audio Codec
                }]
    },
    multiBitrateMp4Profile: {
            Codecs: [{
                    // Video Codec with multiple H264Layers
                }, {
                    // Audio Codec
                }]
    },
    keymap: {
      // See below for more info
    },
   // Enable the Video Assets panel in the widget to automatically load assets (input contract)
    assetsPanelLoaderCallback: onLoadAssets,
    height: 600,
    subclippingMode: 'all',
    filterAssetsTypes: true,
    speedLevels: [
        { name: '4x', value: 4.0 },
        { name: '3x', value: 3.0 },
        { name: '2x', value: 2.0 },
        { name: 'normal', value: 1.0 },
        { name: '0.75x', value: 0.75 },
        { name: '0.5x', value: 0.5 },
    ],
    resetOnJobDone: false,
    autoplayVideo: true,
    language: 'en'    
});
```

Başlatma yöntemi çağrısı için Parametreler şunlardır:
- `selector` {GEREKLİ dize}: CSS Seçici pencere burada işleneceğini eşleşen HTML öğesi.
- `restVersion` {GEREKLİ dize}: Hedef Azure Media Services REST API sürümü. REST sürümü pencere öğesi tarafından oluşturulan çıktı biçimini tanımlar. Şu anda yalnızca 2.0 desteklenmektedir.
- `submitSubclipCallback` {GEREKLİ promise} Pencere öğesinin "Gönder" düğmesine tıklandığında çağrılan geri çağırma işlevi. Geri çağırma işlevi (bir işleme işi yapılandırması veya filtre tanımını) pencere tarafından oluşturulan çıktıyı beklemelisiniz. Daha fazla bilgi için alt klip geri gönderme bakın.
- `logLevel` {İsteğe bağlı, {'info', 'Uyar', 'error'}}: Tarayıcının konsolunda görüntülenecek günlük kaydı düzeyi. Varsayılan değer: hata
- `minimumMarkerGap` {İsteğe bağlı, int}: Bir alt klip (saniye cinsinden) en küçük boyutu. Not: değer aynı zamanda varsayılan değer olan 6, eşit veya daha büyük olmalıdır.
- `singleBitrateMp4Profile` {İsteğe bağlı, JSON nesnesi} Pencere öğesi tarafından oluşturulan işleme işi yapılandırması için kullanmak üzere tek bit hızlı mp4 profili. Sağlanmazsa, kullandığı [varsayılan tek bit hızlı MP4 profil](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-single-bitrate-1080p).
- `multiBitrateMp4Profile` {İsteğe bağlı, JSON nesnesi} Kullanılmak üzere Çoklu bit hızı mp4 profili pencere öğesi tarafından oluşturulan iş yapılandırması işleyin. Sağlanmazsa, kullandığı [varsayılan Çoklu bit hızına sahip MP4 profil](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-multiple-bitrate-1080p).
- `keymap` {İsteğe bağlı, json nesnesi} Pencere öğesinin klavye kısayolları özelleştirilmesine olanak tanır. Daha fazla bilgi için [özelleştirilebilir klavye kısayolları](media-services-azure-media-clipper-keyboard-shortcuts.md).
- `assetsPanelLoaderCallback` {İsteğe bağlı, promise} Kullanıcı bölmenin altındaki aşağı kaydırma her seferinde yeni bir sayfa varlıkların varlıklar bölmesine (zaman uyumsuz olarak) yüklemek için çağrılan geri çağırma işlevi. Varlık bölmesinde yükleyici geri çağırma daha fazla bilgi için bkz.
- `height` {İsteğe bağlı, number} Pencere öğesinin toplam yüksekliği (minimum yükseklik olan 600 piksel varlıklar bölmesinde ve 850 olmadan varlıklar bölmesinde piksel).
- `subclippingMode` (İsteğe bağlı {'all', 'işleme', 'filtre'}): Klip modları izin verilir. Varsayılan değer tümüdür.
- `filterAssetsTypes` (İsteğe bağlı, Boole): filterAssetsTypes varlıklar bölmesinde filtreler açılan Göster/Gizle olanak sağlar. Varsayılan değer true olur.
- `speedLevels` (İsteğe bağlı, dizi): speedLevels, farklı hızı düzeyi için video oynatıcı ayarlamaya olanak tanır, bkz: [Azure Media Player belgeleri](https://amp.azure.net/libs/amp/latest/docs/#amp.player.playbackspeedoptions) daha fazla bilgi için.
- `resetOnJobDone` (İsteğe bağlı, Boole): resetOnJobDone Clipper'ı, bir işi başarıyla gönderildiğinde, alt klip oluşturucu bir ilk durumuna sıfırlayın sağlar.
- `autoplayVideo` (İsteğe bağlı, Boole): autoplayVideo yük video için otomatik Clipper sağlar. Varsayılan değer true olur.
- `language` {İsteğe bağlı, dize}: dil pencere öğesinin dili ayarlar. Belirtilmezse, pencere öğesi tarayıcı diline dayalı iletilerini yerelleştirmeniz dener. Pencere öğesi hiçbir dil tarayıcıda algılanırsa, İngilizce için varsayılan olarak. Daha fazla bilgi için [yerelleştirmeyi yapılandırma](media-services-azure-media-clipper-localization.md) bölümü.
- `languages` {İsteğe bağlı, JSON}: kullanıcı tarafından tanımlanan özel bir sözlük dilleri varsayılan sözlüğü dilleri parametreyi değiştirir. Daha fazla bilgi için [yerelleştirmeyi yapılandırma](media-services-azure-media-clipper-localization.md) bölümü.
- `extraLanguages` (İsteğe bağlı, JSON): extraLanguages parametre yeni diller varsayılan sözlüğe ekler. Daha fazla bilgi için [yerelleştirmeyi yapılandırma](media-services-azure-media-clipper-localization.md) bölümü.

## <a name="typescript-definition"></a>TypeScript tanımı
A [TypeScript](https://www.typescriptlang.org/) Clipper'ı için tanım dosyasını bulunabilir [burada](https://amp.azure.net/libs/amc/latest/azuremediaclipper.d.ts).

## <a name="azure-media-clipper-api"></a>Azure Media Clipper'ı API
Bu bölümde Clipper tarafından sağlanan bir API yüzeyi belgeler.

- `ready(handler)`: Clipper'ı tam olarak yüklenir ve kullanılmaya hazır duruma geldiği JavaScript çalıştırmanın bir yolunu sunar.
- `load(assets)`: varlıklar listesi (kullanılmamalıdır assetsPanelLoaderCallback birlikte) pencere öğesi zaman çizelgesi yükler. Bkz. Bu [makale](media-services-azure-media-clipper-load-assets.md) varlıklar Clipper'a varlık yükleme hakkında ayrıntılar için.
- `setLogLevel(level)`: tarayıcının konsolunda görüntülenecek günlüğe kaydetme düzeyini ayarlar. Olası değerler şunlardır: `info`, `warn`, `error`.
- `setHeight(height)`: toplam pencere öğesinin yüksekliğini piksel cinsinden ayarlar (minimum yükseklik olan 600 piksel varlıklar bölmesinde olmadan ve 850 varlıklar bölmesinde piksel).
- `version`: pencere öğesi sürümünü alır.

## <a name="next-steps"></a>Sonraki adımlar
Azure Media Clipper'ı yapılandırmak için sonraki adımlara bakın:
- [Azure Media Clipper'a varlık yükleme](media-services-azure-media-clipper-load-assets.md)
- [Özel klavye kısayollarını yapılandırma](media-services-azure-media-clipper-keyboard-shortcuts.md)
- [Clipper kırpma işlerini gönderme](media-services-azure-media-clipper-submit-job.md)
- [Yerelleştirme yapılandırma](media-services-azure-media-clipper-localization.md)