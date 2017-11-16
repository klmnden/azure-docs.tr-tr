---
title: "Azure Media Kırpıcıyı ile çalışmaya başlama | Microsoft Docs"
description: "Azure Media Kırpıcıyı Başlarken, medya oluşturmak için bir aracı varlıklarından küçük"
services: media-services
keywords: "küçük; subclip; kodlama; ortam"
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 8a4f2c79131664ca0d078fa58c6a75b54243e705
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
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
- `language`{İsteğe bağlı, dize}: dili pencere öğesi dili ayarlar. Belirtilmezse, pencere öğesi tarayıcı diline dayalı iletileri yerelleştirme dener. Hiçbir dil tarayıcıda algılanırsa, İngilizce'ye pencere öğesi varsayılan olarak ayarlanır. Daha fazla bilgi için desteklenen diller bölümüne bakın.
- `languages`{İsteğe bağlı, JSON}: kullanıcı tarafından tanımlanan özel bir sözlük ile dilleri varsayılan sözlüğü dilleri parametre değiştirir. Daha fazla bilgi için desteklenen diller bölümüne bakın.
- `extraLanguages`(İsteğe bağlı, JSON): extraLanaguages parametresi yeni dilleri varsayılan sözlüğe ekler. Daha fazla bilgi için desteklenen diller bölümüne bakın.

## <a name="typescript-definition"></a>TypeScript tanım
A [TypeScript](https://www.typescriptlang.org/) Kırpıcıyı tanım dosyasını bulunabilir [burada](http://amp.azure.net/libs/amc/latest/azuremediaclipper.d.ts).

## <a name="azure-media-clipper-api"></a>Azure Media Kırpıcıyı API
Bu bölümde Kırpıcıyı tarafından sağlanan API yüzeyi belgeler.

- `load(assets)`: varlıklar bölmesine varlıklarını listesini yükler (ile birlikte kullanılmamalıdır `assetsPanelLoaderCallback`). Bu bkz [makale](media-services-azure-media-clipper-load-assets.md) Kırpıcıyı varlıklar yük hakkında ayrıntılar için.
- `setLogLevel(level)`: tarayıcının konsolunda görüntülenmesi için günlüğe kaydetme düzeyi ayarlar. Olası değerler şunlardır: `info`, `warn`, `error`.
- `setHeight(height)`: pencere öğesi toplam yüksekliğini piksel cinsinden ayarlar (en küçük yükseklik olduğu 600 piksel varlıklar bölmesi olmadan ve 850 varlıklar bölmesiyle piksel).
- `version`: pencere öğesi sürümünü alır.

## <a name="configuring-azure-media-clipper"></a>Azure Media Kırpıcıyı yapılandırma
Azure Media Kırpıcıyı yapılandırma sonraki adımlara bakın:
- [Azure Media Kırpıcıyı varlıklar yükleniyor](media-services-azure-media-clipper-load-assets.md)
- [Yapılandırma özel klavye kısayolları](media-services-azure-media-clipper-keyboard-shortcuts.md)
- [Kırpıcıyı kırpma işlerini gönderme](media-services-azure-media-clipper-submit-job.md)

## <a name="supported-languages"></a>Desteklenen diller
Kırpıcıyı pencere öğesi 18 dillerde mevcuttur. Pencere öğesi dilini ayarlamak amacıyla, tanımlamalısınız `language` başlatma sırasında parametre. Aşağıdaki listeden istenen dil kodu dizesinde geçirin:
- Çince (Basitleştirilmiş): zh-atanır
- Çince (Geleneksel): zh-hant
- Çekçe: cs
- Felemenkçe, Flemish: nl
- İngilizce: tr
- Fransızca: fr
- Almanca: Gizle
- Macarca: hu
- İtalyanca:,
- Japonca: ja
- Kore dili: ko
- Lehçe: pl
- Portekizce (Brezilya): pt-br
- Portekizce (Portekiz): pt-pt
- Rusça: ru
- İspanyolca: es
- İsveççe: sv
- Türkçe: tr

Bir özel dil sözlük ayarlamak veya varsayılan dil sözlüğünü genişletmek için tanımlamalısınız `languages` veya `extraLanguages` parametresi, sırasıyla. Şu JSON biçimini kullanarak özel bir sözlükte geçirin:

```javascript
{
      "{language-code}":
          "{message-id}": "{message}"
          ...
      }
      ...
}
```

Örneğin, aşağıdaki örnek yerelleştirilmiş İngilizce dizeleri tanımlar:

```javascript
export default {
  'VideoPlayer.noPreview': 'No video preview',
  'VideoPlayer.loadAsset': 'You must provide a valid asset',
  'AssetsPanel.name': 'Name',
  'AssetsPanel.type': 'Asset type',
  'AssetsPanel.actions': 'Actions',
  'AssetsPanel.loading': 'Loading...',
  'AssetsPanel.duration': 'Duration',
  'AssetsPanel.resolution': 'Resolution',
  'AssetsPanel.pluralFiles': '{0} assets',
  'AssetsPanel.searchFiles': 'Search assets',
  'AssetsPanel.showTypes': 'Show:',
  'AssetsPanel.typesInfo': 'Rendered assets are actual MP4 files. Dynamic manifest filters are filters applied to a parent asset\'s video segment playlist.',
  'AssetsPanel.filterTypes': 'Filters',
  'AssetsPanel.assetTypes': 'Assets',
  'AssetsPanel.assetsAll': 'All',
  'AssetsPanel.addAsset': 'Add asset to the end',
  'AssetsPanel.addFilter': 'Add filter to the timeline',
  'AssetsPanel.invalidAsset': 'The metadata of this asset is not compatible with the other assets in the timeline',
  'AssetsPanel.addAssetWarning': 'Subclipping on assets with different resolutions may cause resolution autoscaling.',
  'AssetsPanel.live': 'LIVE',
  'AssetsPanel.unknown': 'UNKNOWN',
  'AssetsPanel.minimGapNotMet': 'The asset duration must be greater than the minimum clip duration ({0} seconds)',
  'VideoPlayer.openAdvancedSettings': 'Advanced settings',
  'VideoPlayer.singleBitrate': 'Single-bitrate MP4 (rendered)',
  'VideoPlayer.multiBitrate': 'Multi-bitrate MP4 (rendered)',
  'VideoPlayer.dynamicManifest': 'Dynamic manifest filter',
  'VideoPlayer.ErrorWithMessage': 'There was an error in the video player, code {0}, message: {1}',
  'Common.cancel': 'Cancel',
  'Common.OK': 'OK',
  'AdvancedSettings.framerate': 'Frame rate',
  'Dropdown.select': 'Select an option...',
  'InputAsset.RemoveInput': 'Remove source',
  'Zoom.startTime': 'Start time',
  'Zoom.endTime': 'End time',
  'VideoPlayer.subclips': 'Subclips:',
  'VideoPlayer.length': 'Clip length:',
  'Accordion.scrollLeft': 'Scroll to the left',
  'Accordion.scrollRight': 'Scroll to the right',
  'AdvancedSettings.title': 'Advanced settings',
  'AdvancedSettings.subclipName': 'Subclip name',
  'AdvancedSettings.subclipType': 'Subclipping mode',
  'AdvancedSettings.includeAudioTracks': 'Include audio tracks',
  'AdvancedSettings.subclipTypeInfo': 'Single-bitrate and multi-bitrate MP4s are frame accurate rendered assets. Dynamic manifest filters are group-of-pictures (GOP) accurate filters applied to a parent asset. Creating filters does not create a new asset and does not require encoding. Subclipping jobs on live assets are valid as long as their mark times are within the archive window of the parent asset. Filters are valid as long as the parent asset exists and mark times are within its archive window.',
  'AdvancedSettings.frameRateInfo': 'We autodetect frame rate under most scenarios. however, If we cannot autodetect, choose a frame rate from the dropdown for the selected asset(s).',
  'AdvancedSettings.frameRateError': 'Unable to determine frame rate',
  'AdvancedSettings.subclipNameInfo': 'Choose a name for your subclip.',
  'AdvancedSettings.singleAudioTrack': '1 audio track selected',
  'AdvancedSettings.allAudioTracks': 'All audio tracks selected',
  'AdvancedSettings.someAudioTracks': '{0} audio tracks selected',
  'AdvancedSettings.includeAllAudioTracks': 'Include all audio tracks',
  'AssetsPanel.loadingError': 'Failed to retreive assets from server.',
  'AssetsPanel.retry': 'Retry?',
  'CommandBar.prevFrameTitle': 'Back up one frame',
  'CommandBar.prevKeyFrameTitle': 'Back up one GOP',
  'CommandBar.cleanJob': 'Remove all assets',
  'CommandBar.cleanJobTitle': 'Remove all assets from timeline',
  'CommandBar.cleanJobMessage': 'This will empty all video clips from your timeline.',
  'CommandBar.update': 'Update filter',
  'CommandBar.createFilter': 'Create filter',
  'CommandBar.submit': 'Submit subclipper job',
  'CommandBar.jobErrorTitle': 'Operation failed',
  'CommandBar.jobErrorMessage': 'Your subclip failed to submit. Please try again.',
  'CommandBar.markInTitle': 'Set in at playhead',
  'CommandBar.markInPosition': 'Mark in timecode',
  'CommandBar.markOutTitle': 'Set out at playhead',
  'CommandBar.markOutPosition': 'Mark out timecode',
  'CommandBar.nextFrameTitle': 'Advance one frame',
  'CommandBar.nextKeyFrameTitle': 'Advance one GOP',
  'CommandBar.play': 'Play video',
  'CommandBar.pause': 'Pause video',
  'CommandBar.playPreviewTitle': 'Play subclip preview',
  'CommandBar.pausePreviewTitle': 'Pause subclip preview',
  'CommandBar.redoTitle': 'Redo last action',
  'CommandBar.removeAsset': 'Remove current asset',
  'CommandBar.undoTitle': 'Undo last action',
  'VideoPlayer.errorTitle': 'Error',
  'VideoPlayer.errorMessage': 'There was an error loading the selected asset.',
  'Timeline.markIn': 'Mark in bracket',
  'Timeline.markOut': 'Mark out bracket',
  'Timeline.playHead': 'Play head',
};
```