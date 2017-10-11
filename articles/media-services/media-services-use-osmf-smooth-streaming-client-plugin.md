---
title: "Açık kaynak Media Framework için kesintisiz akış eklentisi"
description: "Adobe açık kaynak Media Framework için Azure Media Services kesintisiz akış eklentisi kullanmayı öğrenin."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6068151f-b6b0-4507-9346-f03416d3d572
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 9c764f176ae75085320882de3fb26d8e7d52daaf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Microsoft Adobe açık kaynak Media Framework eklentisi akış kesintisiz kullanma
## <a name="overview"></a>Genel Bakış
Açık kaynak Media Framework 2.0 (SS OSMF için) için Microsoft kesintisiz akış eklentisi OSMF varsayılan yeteneklerini genişletir ve yeni ve mevcut OSMF oynatıcıları Microsoft kesintisiz akış içeriği oynatmayı ekler. Eklenti kesintisiz Akış kayıttan yürütme özellikleri flaş medya kayıttan yürütme (SMP) da ekler.

OSMF SS eklentisi iki sürümünü içerir:

* OSMF (.swc) için statik kesintisiz akış eklentisi
* OSMF (.swf) için dinamik kesintisiz akış eklentisi

Bu belge okuyucunun OSMF ve OSMF genel bilgilere sahip olduğunu varsayar eklentileri. OSMF hakkında daha fazla bilgi için lütfen belgelere bakın [resmi OSMF sitesi](http://osmf.org/).

### <a name="smooth-streaming-plugin-for-osmf-20"></a>OSMF 2.0 için kesintisiz akış eklentisi
Eklenti yükleme ve isteğe bağlı kesintisiz akış içeriği aşağıdaki özelliklerle kayıttan yürütmeyi destekler:

* İsteğe bağlı kesintisiz Akış kayıttan yürütme (yürütme, duraklatma, arama, Dur)
* Canlı kesintisiz Akış kayıttan yürütme (kullan)
* Canlı DVR işlevleri (Duraklat, arama, DVR kayıttan yürütme, Canlı Git)
* Görüntü codec bileşenleri - H.264 desteği
* Ses codec bileşenleri - AAC desteği
* Birden çok ses dil OSMF yerleşik API'leri ile değiştirme
* En fazla kayıttan yürütme kalitesi seçimi OSMF yerleşik API'leri ile
* Resim yazıları OSMF resim yazıları eklentisi ile sepet kapalı
* Adobe&reg; Flash&reg; Player 11.4 ya da daha yüksek.
* Bu sürümü yalnızca OSMF 2.0 destekler.

## <a name="supported-features-and-known-issues"></a>Desteklenen özellikler ve bilinen sorunlar
Desteklenen özellikler, desteklenmeyen özellikler ve bilinen sorunlar tam bir listesi için bkz [bu belgeyi](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).

## <a name="loading-the-plugin"></a>Eklentisi yükleniyor
Statik olarak (derleme zamanında) OSMF eklentileri yüklenebilir veya dinamik olarak (çalışma zamanında). Kesintisiz akış eklentisi OSMF indirmek için dinamik ve statik sürümlerini içerir.

* Statik yükleme: statik olarak yüklemek için bir statik kitaplık (SWC) dosyası gereklidir. Statik eklentileri varsayılan olarak, derleme zamanında son çıktı dosyası içinde birleştirme ve projeler başvuru olarak eklenir.
* Dinamik yükleme: dinamik olarak yüklemek için önceden derlenmiş (SWF) dosyası gereklidir. Dinamik eklenti çalışma zamanı'nda yüklü ve proje çıktısına dahil değildir. (Derlenmiş çıktı) Dinamik eklenti, HTTP ve dosya protokolleri kullanılarak yüklenebilir.

Resmi statik ve dinamik yükleme hakkında daha fazla bilgi için bkz: [OSMF eklenti sayfası](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

### <a name="ss-for-osmf-static-loading"></a>SS OSMF statik yükleme için
Aşağıdaki kod parçacığında, SS eklentisi OSMF için statik olarak yüklemek ve OSMF MediaFactory sınıfını kullanarak bir temel çalmasına gösterilmektedir. OSMF kod SS eklemeden önce lütfen proje başvurusu "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc" statik eklentisi içerdiğinden emin olun.

```
package 
{

    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;



    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }

        private function initMediaPlayer():void
        {

            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;

            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")

        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started to load.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;

            // Add the media element
            _mediaPlayerSprite.media = element;
        }     

    }
}
```


### <a name="ss-for-osmf-dynamic-loading"></a>SS OSMF dinamik yükleme için
Aşağıdaki kod parçacığında SS eklentisi OSMF için dinamik olarak yükleme ve temel bir Yürüt OSMF MediaFactory sınıfını kullanarak video gösterilmektedir. OSMF kod SS dahil olmak üzere önce dosya protokolü kullanarak yüklemek istiyorsanız, "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf" dinamik eklenti proje klasörüne kopyalayın veya bir web sunucusu HTTP yük altında kopyalayın. Proje başvurularını "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc" eklemenize gerek yoktur.

Paket {

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;


    //Sets the size of the SWF

    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }

        private function initMediaPlayer():void
        {

            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started to load.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     

    }
}

## <a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>SS ODMF dinamik eklenti ile flaş ortam çalma
Kesintisiz akış OSMF dinamik eklenti için uyumlu [flaş medya kayıttan yürütme (SMP)](http://osmf.org/strobe_mediaplayback.html). OSMF eklentisi SS SMP için kesintisiz akış içeriği oynatmayı eklemek için kullanabilirsiniz. Bunu yapmak için bir web sunucusu için aşağıdaki adımları kullanarak HTTP yük altında "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf" kopyalayın:

1. Gözat [flaş Media Çalma Kurulum sayfasında](http://osmf.org/dev/2.0gm/setup.html). 
2. Src kesintisiz akış için kaynak (örneğin http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) ayarlayın 
3. İstenen yapılandırma değişiklikleri yapın ve Önizleme ve güncelleştirme'yi tıklatın.
   
   **Not** içerik web sunucunuzun geçerli crossdomain.xml gerekiyor. 
4. Kodu kopyalayıp aşağıdaki örnekte olduğu gibi sık kullandığınız metin düzenleyiciyi kullanarak basit bir HTML sayfasına yapıştırın:

        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



1. Kesintisiz akış OSMF eklenti ekleme kodu ekleme ve kaydedin.
   
        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>
2. HTML sayfası kaydedin ve bir web sunucusunda yayımlayın. Sık kullanılan Flash kullanarak yayımlanan web sayfasına göz atın&reg; Player etkin Internet tarayıcısı (Internet Explorer, Chrome, Firefox, vb.).
3. Kesintisiz akış içerikten Adobe içinde&reg; Flash&reg; Player.

Resmi genel OSMF geliştirme hakkında daha fazla bilgi için lütfen bkz [OSMF geliştirme sayfa](http://osmf.org/resources.html).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Microsoft eklentisi OSMF güncelleştirmesi Uyarlamalı](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

