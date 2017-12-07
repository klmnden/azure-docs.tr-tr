---
title: "Azure Media Kırpıcıyı ile çalışmaya başlama | Microsoft Docs"
description: "Azure Media Kırpıcıyı, video klip AMS varlıklarından oluşturmak için bir aracı ile çalışmaya başlama"
services: media-services
keywords: "küçük; subclip; kodlama; ortam"
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: ac64d97aeeef6147aa62658c9ee440bf058f4db1
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="create-clips-with-azure-media-clipper"></a>Azure Media Kırpıcıyı ile Klip Oluştur
Bu bölümde Azure medya Kırpıcıyı ile çalışmaya başlama temel adımları gösterir. İzleyen bölümlerde Azure medya Kırpıcıyı yapılandırma konusunda özellikleri sağlar.

- İlk olarak, aşağıdaki bağlantılardan Azure Media Player ve Azure Media Kırpıcıyı belgenizin head ekleyin. URL'lerinde Kırpıcıyı ve Azure Media Player sürümünü açıkça belirtilmesi önerilir. İsteğe bağlı değiştirilebilir oldukları gibi bu kaynakları en son sürümünü üretimde kullanmayın.

```javascript
<!--Azure Media Player 2.1.4 or later is a prerequisite-->
<link href="//amp.azure.net/libs/amp/2.1.4/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amp/2.1.4/azuremediaplayer.min.js"></script>
<!--Azure Media Clipper script and stylesheet-->
<link href="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.min.js"></script>
```

- Ardından, aşağıdaki sınıflar nereye Kırpıcıyı örneği istersiniz div öğesinin ekleyin.

```javascript
<div id="root" class="azure-subclipper" />
```

İsteğe bağlı olarak koyu tema etkinleştirmek için koyu kaplama sınıfı ekleyin:

```javascript
<div id="root" class="azure-subclipper dark-skin" />
```

- Ardından, aşağıdaki API çağrısı ile Kırpıcıyı örneği:

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
- `selector`{GEREKLİ, dize}: CSS Seçici burada pencere öğesi çizilir eşleşen HTML öğesi.
- `restVersion`{GEREKLİ, dize}: hedef için Azure Media Services REST API sürümü. REST sürüm pencere öğesi tarafından oluşturulan çıktı biçimi tanımlar. Şu anda yalnızca 2.0 desteklenir.
- `submitSubclipCallback`{GEREKLİ promise} Pencere öğesinin "gönderme" düğmesine tıklandığında çağrılan geri çağırma işlevi. Geri çağırma işlevi (bir işleme iş yapılandırma veya bir filtre tanımını) pencere tarafından oluşturulan çıktı beklemelisiniz. Daha fazla bilgi için bkz: gönderme subclip geri çağırma.
- `logLevel`{İsteğe bağlı, {'bilgi', 'Uyar', 'error'}}: tarayıcının konsolunda görüntülenmesi için günlüğe kaydetme düzeyi. Varsayılan değer: hata
- `minimumMarkerGap`{İsteğe bağlı, int}: (saniye cinsinden) subclip minimum boyutu. Not: değer Ayrıca varsayılan ayar olan 6, eşit veya daha büyük olmalıdır.
- `singleBitrateMp4Profile`{İsteğe bağlı, JSON nesnesi} Pencere öğesi tarafından oluşturulan işleme iş yapılandırması için kullanılacak tek bit hızlı mp4 profili. Sağlanmazsa, kullanan [varsayılan tek bit hızlı MP4 profil](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-single-bitrate-1080p).
- `multiBitrateMp4Profile`{İsteğe bağlı, JSON nesnesi} Kullanılmak üzere Çoklu bit hızlı mp4 profili pencere öğesi tarafından oluşturulan iş yapılandırma işlenemiyor. Sağlanmazsa, kullanan [varsayılan Çoklu bit hızlı MP4 profil](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-multiple-bitrate-1080p).
- `keymap`{İsteğe bağlı, json nesnesi} Pencere öğesinin klavye kısayollarını özelleştirme sağlar. Daha fazla bilgi için bkz: [özelleştirilebilir klavye kısayolları](media-services-azure-media-clipper-keyboard-shortcuts.md).
- `assetsPanelLoaderCallback`{İsteğe bağlı, promise} Kullanıcı bölmesinde en alta kadar kaydırın kayar her zaman yeni bir sayfa varlıklarını varlıklar bölmesine (zaman uyumsuz olarak) yüklemek için çağrılan geri çağırma işlevi. Varlık bölmesinde yükleyicisi geri çağırma daha fazla bilgi için bkz.
- `height`{İsteğe bağlı, number} Pencere öğesi toplam yüksekliği (en küçük yükseklik olduğu 600 piksel varlıklar bölmesinde ve 850 olmadan varlıklar bölmesiyle piksel).
- `subclippingMode`(İsteğe bağlı, {'all', 'Oluştur', 'Filtrele'}): izin verilen subclipping modları. Tüm varsayılan değerdir.
- `filterAssetsTypes`(İsteğe bağlı, bool): filterAssetsTypes varlıklar bölmesinden filtreleri açılır Göster/Gizle olanak sağlar. Varsayılan değer true olur.
- `speedLevels`(İsteğe bağlı, dizi): speedLevels sağlayan video oynatıcı için farklı hızı düzeylerini ayarlama, bkz: [Azure Media Player belgelerine](http://amp.azure.net/libs/amp/latest/docs/#amp.player.playbackspeedoptions) daha fazla bilgi için.
- `resetOnJobDone`(İsteğe bağlı, bool): resetOnJobDone bir işi başarıyla gönderildiğinde subclipper ilk durumuna sıfırlamak Kırpıcıyı sağlar.
- `autoplayVideo`(İsteğe bağlı, bool): autoplayVideo video yükleme için Otomatik Yürüt'ü Kırpıcıyı sağlar. Varsayılan değer true olur.
- `language`{İsteğe bağlı, dize}: dili pencere öğesi dili ayarlar. Belirtilmezse, pencere öğesi tarayıcı diline dayalı iletileri yerelleştirme dener. Hiçbir dil tarayıcıda algılanırsa, İngilizce'ye pencere öğesi varsayılan olarak ayarlanır. Daha fazla bilgi için bkz: [yerelleştirme yapılandırma](media-services-azure-media-clipper-localization.md) bölümü.
- `languages`{İsteğe bağlı, JSON}: kullanıcı tarafından tanımlanan özel bir sözlük ile dilleri varsayılan sözlüğü dilleri parametre değiştirir. Daha fazla bilgi için bkz: [yerelleştirme yapılandırma](media-services-azure-media-clipper-localization.md) bölümü.
- `extraLanguages`(İsteğe bağlı, JSON): extraLanaguages parametresi yeni dilleri varsayılan sözlüğe ekler. Daha fazla bilgi için bkz: [yerelleştirme yapılandırma](media-services-azure-media-clipper-localization.md) bölümü.

## <a name="typescript-definition"></a>TypeScript tanım
A [TypeScript](https://www.typescriptlang.org/) Kırpıcıyı tanım dosyasını bulunabilir [burada](http://amp.azure.net/libs/amc/latest/azuremediaclipper.d.ts).

## <a name="azure-media-clipper-api"></a>Azure Media Kırpıcıyı API
Bu bölümde Kırpıcıyı tarafından sağlanan API yüzeyi belgeler.

- `ready(handler)`: tam olarak yüklenen ve kullanılacak hazır Kırpıcıyı hemen JavaScript çalıştırmak için bir yol sunar.
- `load(assets)`: varlıklar listesi (kullanılmamalıdır assetsPanelLoaderCallback birlikte) pencere öğesi zaman çizelgesi yükler. Bu bkz [makale](media-services-azure-media-clipper-load-assets.md) Kırpıcıyı varlıklar yük hakkında ayrıntılar için.
- `setLogLevel(level)`: tarayıcının konsolunda görüntülenmesi için günlüğe kaydetme düzeyi ayarlar. Olası değerler şunlardır: `info`, `warn`, `error`.
- `setHeight(height)`: pencere öğesi toplam yüksekliğini piksel cinsinden ayarlar (en küçük yükseklik olduğu 600 piksel varlıklar bölmesi olmadan ve 850 varlıklar bölmesiyle piksel).
- `version`: pencere öğesi sürümünü alır.

## <a name="next-steps"></a>Sonraki adımlar
Azure Media Kırpıcıyı yapılandırma sonraki adımlara bakın:
- [Azure Media Kırpıcıyı varlıklar yükleniyor](media-services-azure-media-clipper-load-assets.md)
- [Yapılandırma özel klavye kısayolları](media-services-azure-media-clipper-keyboard-shortcuts.md)
- [Kırpıcıyı kırpma işlerini gönderme](media-services-azure-media-clipper-submit-job.md)
- [Yerelleştirme yapılandırma](media-services-azure-media-clipper-localization.md)