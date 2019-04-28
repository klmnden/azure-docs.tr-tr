---
title: Müşterilere içerik teslim | Microsoft Docs
description: Bu konu ne içeriğinizi Azure Media Services ile ilgili genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 89ede54a-6a9c-4814-9858-dcfbb5f4fed5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 5db2cb983c0c3cd0e2194f7686964d9ec3828d6f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61232284"
---
# <a name="deliver-content-to-customers"></a>Müşterilere içerik teslim edin
Müşterilere, akış ve isteğe bağlı video içerik teslim, amacınız farklı ağ koşulları altındaki çeşitli cihazlara yüksek kaliteli video teslim andır.

Bu hedefe ulaşmak için aşağıdakileri yapabilirsiniz:

* Akışınızı Çoklu bit hızlı (bit hızı Uyarlamalı) video akışına kodlayın. Bu kalite ve ağ koşullarını ilgileniriz.
* Microsoft Azure Media Services'i kullanma [dinamik paketleme](media-services-dynamic-packaging-overview.md) dinamik olarak akışınız farklı protokollere yeniden paketler. Bu farklı cihazlarda akış ilgileniriz. Media Services teslim aşağıdaki hızı Uyarlamalı akış teknolojilerini destekler: <br/>
    * **HTTP canlı akış** (HLS) - ekleme "(biçim = m3u8-aapl)" "/ MANIFEST" bölümüne geri HLS içerik tüketimi için döndürülecek akış kaynağı sunucusu bildirmek için URL'nin yol **Apple iOS** (Ayrıntılar için yerel cihazlar bkz: [bulucular](#locators) ve [URL'leri](#URLs)),
    * **MPEG-DASH** -ekleme "(biçim mpd-time-CSF)" yolu için döndürülecek akış kaynağı sunucusu bildirmek için URL "/ bildirimi" bölümünü geri MPEG-DASH (Ayrıntılar için bkz [bulucular](#locators) ve [URL'leri](#URLs)),
    * **Kesintisiz akış**.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

Bu makalede, önemli bir içerik teslim kavramlarına genel bakış sağlar.

Bilinen sorunlar denetlemek için bkz: [bilinen sorunlar](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dinamik paketleme
Dinamik paketleme ile Media Services, bit hızı Uyarlamalı MP4 veya kesintisiz akış kodlanmış içeriğinizi Media Services (MPEG-DASH, HLS, kesintisiz akış) tarafından desteklenen akış biçimlerinde sunabilirsiniz yeniden paketlemenize gerek kalmadan sağlar Akış biçimlerinde. Dinamik paketleme ile içeriğinizi teslim etmek öneririz.

Dinamik paketlemeden yararlanmak için Ara (kaynak) dosyanızı bit hızı Uyarlamalı MP4 dosyası ya da Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesine kodlamanız gerekir.

Dinamik paketleme ile depolayın ve tek bir depolama biçimindeki dosyaları için ödeme yaparsınız. Media Services isteklerinizi üzerinde göre uygun yanıtı derler ve.

Dinamik paketleme, standart ve premium akış uç noktaları için kullanılabilir. 

Daha fazla bilgi için [dinamik paketleme](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filtreler ve dinamik bildirimlere
Media Services ile varlıklarınız için filtreleri tanımlayabilirsiniz. Bu filtreler, müşterilerinizin müşterinizin cihaz (yerine, varlıkla ilişkili olan tüm önayarda) işleyebilir ses ve video yorumlama kümesini belirtin ya da belirli bir bölümünde bir video yürütme gibi şeyler yardımcı sunucu tarafı kurallardır. Bu filtreleme yoluyla elde edilir *olan dinamik bildirimler* müşteri isteklerini temel alan bir video akışını sağlamak veya daha fazla filtre belirtilen oluşturulur.

Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](media-services-dynamic-manifest-overview.md).

## <a name="a-idlocatorslocators"></a><a id="locators"/>Bulucular
Kullanıcı akışı veya içerik indirmek için kullanılabilecek bir URL sağlamak için önce Bulucu oluşturarak Varlığınızı yayımlamanız gerekir. Bir Bulucu bir varlıkta bulunan dosyalara erişmek için bir giriş noktası sağlar. Media Services iki tür bulucuyu destekler:

* OnDemandOrigin bulucuları. Bu akış medya (örneğin, MPEG-DASH, HLS veya kesintisiz akış) için kullanılan veya dosyalarını aşamalı indirmek.
* Paylaşılan erişim imzası (SAS) URL'si bulucular. Bunlar, yerel bilgisayarınıza medya dosyalarını indirmek için kullanılır.

Bir *erişim ilkesi* izinleri (örneğin, okuma, yazma ve liste) tanımlamak için kullanılır ve süresi bir istemci belirli bir varlık için erişimine sahiptir. İzin (AccessPermissions.List) OnDemandOrigin bir Bulucu oluşturma kullanılmamalıdır unutmayın.

Bulucuların sona erme tarihi vardır. Azure portalında bir sona erme tarihi 100 yıl sonra bulucular için ayarlar.

> [!NOTE]
> Azure portalında Mart 2015 öncesinde bulucular oluşturmak için kullanılan, bu bulucuların iki yıl sonra süresi dolacak şekilde ayarlandı.
> 
> 

Bir bulucunun sona erme tarihini güncelleştirmek için [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) ya da [.NET](https://go.microsoft.com/fwlink/?LinkID=533259) API’lerini kullanın. SAS bulucunun sona erme tarihini güncelleştirdiğinizde URL’nin değiştiğini unutmayın.

Bulucular kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Digital Rights Management (DRM) çözümleri kullanarak, ayrı kullanıcılara farklı erişim hakkı verebilirsiniz. Daha fazla bilgi için [güvenli hale getirme medya](https://msdn.microsoft.com/library/azure/dn282272.aspx).

Bir Bulucu oluşturduğunuzda, gerekli depolama ve Azure depolama yayma işlemi nedeniyle 30 saniyelik gecikme olabilir.

## <a name="adaptive-streaming"></a>Akış Bağdaşık
Bit hızı Uyarlamalı teknolojileri birkaç bit hızlarına dönüştürme ağ koşulları belirlemek ve video oynatıcı uygulamaları sağlar. Ağ iletişimi değerlerinde düşme olduğunda düşük video kalitesi ile kayıttan yürütme devam edebilmesi istemci daha düşük bir bit hızı seçebilirsiniz. Ağ koşulları arttıkça, istemci bir daha yüksek hızı ile geliştirilmiş video kalitesi geçiş yapabilirsiniz. Azure Media Services şu bit hızı Uyarlamalı teknolojilerini destekler: HTTP canlı akış (HLS), kesintisiz akış ve MPEG-DASH.

Akış URL'lerini kullanıcılara sağlamak için önce bir OnDemandOrigin Bulucu oluşturmanız gerekir. Bulucu oluşturarak, içerik akışı yapmak istediğiniz içeren bir varlık için temel yol sunar. Ancak, bu içerik akışı yapmak için bu yolu daha da değiştirmeniz gerekir. Akış bildirim dosyasına tam bir URL oluşturmak için konum yol değeri ve ' % s'bildirimi (filename.ism) birleştirme dosya adı. Ardından Ekle **/MANIFEST** ve (gerekirse) uygun bir biçimde Bulucu yolu.

> [!NOTE]
> Ayrıca, bir SSL bağlantısı üzerinden içeriğinizin akışını yapabilirsiniz. Bunu yapmak için akış URL'leri HTTPS ile Başlat emin olun. Şu anda AMS SSL ile özel etki alanları, desteklemez.  
> 

İçeriğinizi teslim etmek istediğiniz akış uç noktası 10 Eylül 2014'ten sonra oluşturduysanız yalnızca SSL üzerinden akışını yapabilirsiniz. 10 Eylül 2014'ten sonra oluşturulan akış uç noktaları, akış URL'leri dayalı URL "streaming.mediaservices.windows.net." içerir "Origin.mediaservices.windows.net" (eski biçimde) içeren akış URL'leri SSL'yi desteklemez. URL'NİZİN biçimi eski olduğundan ve SSL üzerinden akışını yapmak istiyorsanız, yeni bir akış uç noktası oluşturun. Yeni akış uç noktasına göre URL'ler, SSL üzerinden içeriğinizin akışını yapmak amacıyla kullanın.

## <a name="a-idurlsstreaming-url-formats"></a><a id="URLs"/>Akış URL biçimleri

### <a name="mpeg-dash-format"></a>MPEG-DASH biçimi
{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf)

### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP canlı akış (HLS) V4 biçimi
{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP canlı akış (HLS) V3 biçimi
{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP canlı akış (HLS) biçimi yalnızca ses filtresi
Varsayılan olarak, HLS yalnızca ses parçaları dahil bildirimi. Bu, cep telefonu şebekeleri için Apple Store sertifika için gereklidir. Bu durumda, bir istemci yeterli bant genişliğine sahip değil ya da 2 G bağlantısı üzerinden bağlandığından, kayıttan yürütme yalnızca ses olarak geçer. Bu içerik akışı arabelleğe alma olmadan tutmaya yardımcı olur, ancak video yoktur. Bazı senaryolarda, arabelleğe alma player yalnızca ses üzerinden tercih edilen olabilir. Yalnızca ses izleme kaldırmak istiyorsanız, ekleme **yalnızca ses = false** URL'si.

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,audio-only=false)

Daha fazla bilgi için [dinamik bildirim oluşturma desteği ve HLS çıktı ek özellikler](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

### <a name="smooth-streaming-format"></a>Kesintisiz akış biçimi
{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

Örnek:

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest

### <a id="fmp4_v20"></a>Kesintisiz akış 2.0 bildirimi (eski bildirimi)
Varsayılan olarak, kesintisiz akış bildirim biçimi yineleme etiketi (r-etiketi) içerir. Ancak, bazı oyuncuların r-tag desteklemez. İstemciler bu yürütücüler ile r etiketi devre dışı bırakan bir biçim kullanabilirsiniz:

{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

## <a name="progressive-download"></a>Aşamalı indirme
Aşamalı indirme ile tüm dosya indirilmeden önce ortam yürütmeyi başlatabilirsiniz. Aşamalı olarak (.ism * ismv, ISMA, ismt veya ismc) dosyalarını karşıdan yükleyemiyor.

Aşamalı olarak içerik indirmek için Bulucu OnDemandOrigin türünü kullanın. Aşağıdaki örnek, Bulucu OnDemandOrigin türüne göre URL'yi gösterir:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Aşamalı indirme için kaynak hizmetinden akışı gerçekleştirmek istediğiniz depolama şifrelenmiş varlıklar dosyanın şifresini çözmelisiniz.

## <a name="download"></a>İndirme
Bir istemci cihaza içerik indirmek için bir SAS Bulucu oluşturmanız gerekir. SAS Bulucu, dosyanızın bulunduğu Azure depolama kapsayıcısına erişmenizi sağlar. İndirme URL'sini oluşturmak için SAS imzası ve ana bilgisayar arasında dosya adı eklemek zorunda.

Aşağıdaki örnek, SAS Bulucu üzerinde temel URL'yi gösterir:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Aşağıdaki maddeler geçerlidir:

* Aşamalı indirme için kaynak hizmetinden akışı gerçekleştirmek istediğiniz depolama şifrelenmiş varlıklar dosyanın şifresini çözmelisiniz.
* 12 saat içinde tamamlamadı bir yükleme başarısız olur.

## <a name="streaming-endpoints"></a>Akış uç noktaları

Bir akış uç noktası, içeriği doğrudan bir istemci Yürütücü uygulamasına veya başkalarına dağıtım için bir içerik teslim ağı (CDN) teslim eden bir akış hizmetini temsil eder. Bir akış uç noktası hizmetinize giden akıştan, canlı akış veya Media Services hesabınızı bir isteğe bağlı video varlığı olabilir. Akış uç noktaları, iki tür vardır **standart** ve **premium**. Daha fazla bilgi için bkz. [Akış uç noktalarına genel bakış](media-services-streaming-endpoints-overview.md).

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

## <a name="known-issues"></a>Bilinen sorunlar
### <a name="changes-to-smooth-streaming-manifest-version"></a>Kesintisiz akış değişiklikleri bildirim sürümü
Temmuz 2016 hizmet sürümünden önce--varlıklar Media Encoder Standard tarafından üretilen, Media Encoder Premium iş akışı ya da eski Azure Medya Kodlayıcısı'dan akış dinamik paketleme--kesintisiz akış kullanarak iade bildirimde sürümüne uygun 2.0. Sürüm 2.0, parça süreleri sözde yineleme ('r') etiketler kullanmayın. Örneğin:


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

Temmuz 2016 hizmet sürümü, kesintisiz akış oluşturulan bildirim sürüm 2.2, yineleme etiketleri kullanarak parça süreleri ile uyumludur. Örneğin:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Bazı eski kesintisiz akış istemcilerini yineleme etiketleri desteklemeyebilir ve bildirim yüklenmesi başarısız olur. Bu sorunu gidermek için eski bildirim biçimi parametresini kullanabilirsiniz **(biçim fmp4 v20 =)** veya istemci yineleme etiketlerini destekleyen en son sürüme güncelleştirin. Daha fazla bilgi için [Smooth Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Depolama anahtarlarını değiştirdikten sonra Media Services bulucular güncelleştir](media-services-roll-storage-access-keys.md)

