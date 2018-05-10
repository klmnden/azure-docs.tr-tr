---
title: İçerik müşterilere teslim | Microsoft Docs
description: Bu konuda ne içeriğinizi Azure Media Services ile dağıtımına katılan genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 89ede54a-6a9c-4814-9858-dcfbb5f4fed5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: juliako
ms.openlocfilehash: 1d1506e26beec3cc48a904ddeb9bbb4e7656a08e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="deliver-content-to-customers"></a>İçerik müşterilere teslim
Akış veya isteğe bağlı video içeriğinizi müşterilere teslim farklı ağ koşulları altındaki çeşitli cihazlara yüksek kaliteli video iletmek için amacınız olur.

Bu hedefe ulaşmak için aşağıdakileri yapabilirsiniz:

* Akışınızı Çoklu bit hızlı (bit hızı Uyarlamalı) video akışına kodlayın. Bu kalitesini ve ağ koşullarını dikkatli olun.
* Microsoft Azure Media Services'i kullanma [dinamik paketleme](media-services-dynamic-packaging-overview.md) dinamik olarak akışınızı farklı protokollere yeniden paketler. Bu farklı cihazlarda akış ilgilenebilmek. Media Services teknolojileri aşağıdaki bit hızı Uyarlamalı akış teslimini destekler: <br/>
    * **HTTP canlı akış** (HLS) - Ekle "(biçimi = m3u8-aapl)" üzerinde geri HLS içerik tüketimi için döndürülecek akış kaynak sunucu bildirmek için URL "/ bildirimi" bölümünü yoluna **Apple iOS** (Ayrıntılar için yerel cihazlar bkz: [bulucular](#locators) ve [URL'leri](#URLs)),
    * **MPEG-DASH** -Ekle "(biçim mpd zaman csf =)" döndürülecek akış kaynak sunucu bildirmek için URL "/ bildirimi" bölümünü yoluna yedekleyin MPEG-DASH (Ayrıntılar için bkz [bulucular](#locators) ve [URL'leri](#URLs)),
    * **Kesintisiz akış**.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

Bu makalede önemli içerik teslim kavramlara genel bakış sunulmaktadır.

Bilinen sorunlar denetlemek için bkz: [bilinen sorunlar](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dinamik paketleme
Dinamik paketleme ile Media Services, bit hızı Uyarlamalı MP4 veya kesintisiz akış kodlanmış içeriğinizi Media Services (MPEG-DASH, HLS, kesintisiz akış,) tarafından desteklenen akış biçimlerinde sunabilir bu yeniden paketlemeden gerek kalmadan sağlar Akış biçimlerine. Dinamik paketleme ile içeriğinizi teslim etmek öneririz.

Dinamik paketlemeden yararlanmak için Ara (kaynak) dosyanızı bit hızı Uyarlamalı MP4 dosyası ya da Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesine kodlayın gerekir.

Dinamik paketleme ile depolamak ve tek bir depolama biçiminde dosyaları için ödeme yaparsınız. Media Services, isteklerinde göre uygun yanıtı derler ve.

Dinamik paketleme, standart ve premium akış uç noktaları için kullanılabilir. 

Daha fazla bilgi için bkz: [dinamik paketleme](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filtreler ve dinamik bildirimleri
Media Services ile varlıklarınızı için filtreleri tanımlayabilirsiniz. Bu filtreler, bir video belirli bir kısmını çalmak gibi şeyler veya bir alt müşterinizin aygıt (yerine varlıkla ilişkilendirilen tüm yorumlama) işleyebilir ses ve video yorumlama belirtin müşterilerinizin yardımcı sunucu tarafı kurallardır. Bu filtreleme aracılığıyla elde edilen *dinamik bildirimleri* müşteri isteklerini temel alan bir video akışını sağlamak için ya da daha fazla filtreler belirtilen oluşturulur.

Daha fazla bilgi için bkz: [filtreleri ve dinamik bildirimleri](media-services-dynamic-manifest-overview.md).

## <a name="a-idlocatorslocators"></a><a id="locators"/>Belirleyicileri
Kullanıcı akışla aktarmak veya içeriğinizi indirmek için kullanılabilecek bir URL sağlamak için önce bir Bulucu oluşturarak Varlığınızı yayımlamanız gerekir. Bir Bulucu bir varlıkta bulunan dosyalara erişmek için bir giriş noktası sağlar. Media Services iki tür bulucuyu destekler:

* OnDemandOrigin bulucuları. Bu akış medya (örneğin, MPEG-DASH, HLS veya kesintisiz akış) için kullanılan veya aşamalı olarak dosyaları indirme.
* Paylaşılan erişim imzası (SAS) URL bulucular. Bunlar, yerel bilgisayarınıza medya dosyalarını indirmek için kullanılır.

Bir *erişim ilkesi* (örneğin, okuma, yazma ve liste) izinlerini tanımlamak için kullanılır ve bir istemci belirli bir varlık için erişim olduğu süre. Liste izni (AccessPermissions.List) bir OrDemandOrigin Bulucu oluşturmada kullanılmamalıdır unutmayın.

Bulucular sona erme tarihi vardır. Azure portalı kullanarak Bulucu için bir sona erme tarihi 100 yıl sonra ayarlar.

> [!NOTE]
> Mart 2015 öncesinde bulucular oluşturmak için Azure Portalı'nı kullandıysanız, bu konum belirleyicisi iki yıl sonra süresi dolacak şekilde ayarlandı.
> 
> 

Bir bulucunun sona erme tarihini güncelleştirmek için [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) ya da [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API’lerini kullanın. SAS bulucunun sona erme tarihini güncelleştirdiğinizde URL’nin değiştiğini unutmayın.

Bulucular kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Dijital Hak Yönetimi (DRM) çözümleri kullanılarak tek tek kullanıcılar farklı bir erişim hakkı verebilirsiniz. Daha fazla bilgi için bkz: [güvenli hale getirme medya](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Bir Bulucu oluşturduğunuzda, gerekli depolama ve Azure Storage yayma işlemi nedeniyle 30 saniyelik bir gecikme olabilir.

## <a name="adaptive-streaming"></a>Akış Bağdaşık
Bit hızı Uyarlamalı teknolojileri ağ koşulları belirlemek ve birkaç bit seçmek video oynatıcı uygulamaları sağlar. Ağ iletişimi düşürür olduğunda istemci ile alt video kalitesini kayıttan yürütme devam edebilmek için daha düşük bit hızlı seçebilirsiniz. Ağ koşulları geliştirmek gibi istemci geliştirilmiş video kalitesini ile daha yüksek bir bit hızı geçiş yapabilirsiniz. Azure Media Services şu bit hızı Uyarlamalı teknolojilerini destekler: HTTP canlı akışı (HLS), kesintisiz akış ve MPEG-DASH.

Akış URL'lerini kullanıcılara sağlamak için önce bir OnDemandOrigin Bulucu oluşturmanız gerekir. Bulucu oluşturarak akış istediğiniz içerik içerir varlık için temel yolu sağlar. Ancak, bu içerik akışı edebilmek için daha fazla bu yolu değiştirmeniz gerekir. Akış bildirim dosyası için tam bir URL oluşturmak için konum belirleyici'nın yol değeri ve bildirimi (filename.ism) birleştirme dosya adı. Ardından append **/bildirimi** ve (gerekirse) uygun bir biçim Bulucu yolu.

> [!NOTE]
> Ayrıca, bir SSL bağlantısı üzerinden içeriğinizin akışını sağlayabilirsiniz. Bunu yapmak için HTTPS ile akış URL'leri Başlat emin olun. Şu anda AMS SSL ile özel etki alanlarını, desteklemez.  
> 

İçeriğinizi teslim etmek istediğiniz akış uç noktası 10 Eylül 2014 sonra oluşturduysanız yalnızca SSL üzerinden akışını sağlayabilirsiniz. Akış URL'leri 10 Eylül 2014 sonra oluşturulan akış uç noktalarını dayanır, URL "streaming.mediaservices.windows.net." içerir. "Origin.mediaservices.windows.net" (eski biçimde) içeren akış URL'leri SSL desteklemez. URL'niz eski biçimindedir ve SSL üzerinden akışla aktarmak istiyorsanız, yeni bir akış uç noktası oluşturun. SSL üzerinden içeriğinizin akışını sağlamak için yeni akış uç noktasında temel URL kullanın.

## <a name="a-idurlsstreaming-url-formats"></a><a id="URLs"/>Akış URL biçimleri

### <a name="mpeg-dash-format"></a>MPEG-DASH biçimi
{uç nokta adı media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf) akış

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf)

### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP canlı akışı (HLS) V4 biçimi
{uç nokta adı media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl) akış

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP canlı akışı (HLS) V3 biçimi
{uç nokta adı media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3) akış

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP canlı akışı (HLS) biçimi yalnızca ses filtreli
Varsayılan olarak, yalnızca ses parçaları HLS dahil bildirimi. Bu cep telefonu ağları için Apple Store sertifika için gereklidir. Bu durumda, bir istemci yeterli bant genişliği yok ya da 2 G bağlantısı üzerinden bağlanmış, kayıttan yürütme yalnızca ses olarak geçer. Bu içerik akışı arabelleğe alma olmadan kalmasına yardımcı olur, ancak hiçbir video yok. Bazı senaryolarda, arabelleğe alma player yalnızca ses üzerinden tercih edilen olabilir. Yalnızca ses izleme kaldırmak istiyorsanız, ekleme **yalnızca ses = false** URL.

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3Yalnızca ses = false)

Daha fazla bilgi için bkz: [dinamik bildirimi oluşturma desteği ve HLS çıkış ek özellikler](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

### <a name="smooth-streaming-format"></a>Kesintisiz akış biçimi
{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

Örnek:

http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest

### <a id="fmp4_v20"></a>Kesintisiz akış 2.0 bildirimi (eski bildirimi)
Varsayılan olarak, kesintisiz akış bildirim biçimi yineleme etiketi (r-tag) içerir. Ancak, bazı oynatıcıları r-tag desteklemez. İstemciler bu oyuncularla r-tag devre dışı bırakan bir biçimde kullanabilirsiniz:

{uç nokta adı media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20) akış

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

## <a name="progressive-download"></a>Aşamalı indirme
Aşamalı indirme ile dosyanın tamamı indirilmeden önce media çalma başlatabilirsiniz. (.İsm * ismv, ISMA, ismt veya ismc) dosyalarını aşamalı indirmek edilemiyor.

Aşamalı olarak içerik indirmek için Bulucu OnDemandOrigin türünü kullanın. Aşağıdaki örnek, Bulucu OnDemandOrigin türüne göre URL gösterir:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Aşamalı indirme için kaynak hizmetinden akışını sağlamak istediğiniz depolama şifrelenmiş varlıklar şifresini gerekir.

## <a name="download"></a>İndirme
Bir istemci cihaza, içerik indirmek için bir SAS Bulucu oluşturmanız gerekir. SAS Bulucu Azure depolama kapsayıcısının dosyanızı bulunduğu erişmenizi sağlar. İndirme URL'si oluşturmak için SAS imza ve ana bilgisayar arasında dosya adı eklemek zorunda.

Aşağıdaki örnek, SAS Bulucu temel URL gösterir:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Aşağıdaki maddeler geçerlidir:

* Aşamalı indirme için kaynak hizmetinden akışını sağlamak istediğiniz depolama şifrelenmiş varlıklar şifresini gerekir.
* 12 saat içinde tamamlamadı bir yükleme başarısız olur.

## <a name="streaming-endpoints"></a>Akış uç noktaları

Bir akış uç içeriği doğrudan bir istemci oynatıcı uygulaması veya başkalarına dağıtım için bir içerik teslim ağı (CDN) teslim bir akış hizmetini temsil eder. Bir akış uç noktası hizmetinden giden akış canlı akış veya isteğe bağlı video varlık Media Services hesabınızda olabilir. Akış uç noktaları, iki tür vardır **standart** ve **premium**. Daha fazla bilgi için bkz. [Akış uç noktalarına genel bakış](media-services-streaming-endpoints-overview.md).

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

## <a name="known-issues"></a>Bilinen sorunlar
### <a name="changes-to-smooth-streaming-manifest-version"></a>Sürüm kesintisiz akış için değişiklik bildirimi
Temmuz 2016 servis yayımlanmadan önce--varlıklar Medya Kodlayıcısı standart tarafından üretilen zaman Medya Kodlayıcısı Premium iş akışı veya önceki Azure medya Kodlayıcı akışı dinamik paketleme--kesintisiz akış kullanarak döndürülen bildirimi sürümüne uygun 2.0. Sürüm 2.0, parça süreleri sözde Yinele ('r') etiketleri kullanmayın. Örneğin:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

Temmuz 2016 hizmet sürümü, üretilen kesintisiz akış bildirim sürüm 2.2, yineleme etiketleri kullanarak parça süreleri ile uyumludur. Örneğin:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Bazı eski kesintisiz akış istemcilerinin yineleme etiketleri desteklemiyor olabilir ve bildirim yüklenmesi başarısız olur. Bu sorunu azaltmak için eski bildirim biçimi parametresini kullanabilirsiniz **(biçimi fmp4 v20 =)** veya istemciniz yineleme etiketlerini destekler en son sürüme güncelleştirin. Daha fazla bilgi için bkz: [Smooth Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Media Services bulucular depolama anahtarları çalışırken sonra güncelleştirme](media-services-roll-storage-access-keys.md)

