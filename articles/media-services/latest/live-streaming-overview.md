---
title: Azure Media Services'i kullanarak canlı akış genel bakış | Microsoft Docs
description: Bu makalede, genel bir bakış Canlı Azure Media Services v3 kullanarak akış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/15/2019
ms.author: juliako
ms.openlocfilehash: 6f7c6c2265fe13eb50aa900e9a51e11edfd90201
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382092"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Canlı akış ile Azure Media Services v3

Azure Media Services Canlı etkinlikler müşterilerinizin Azure bulutunda dağıtmanıza olanak sağlar. Media Services ile etkinliklerin canlı akış için aşağıdakiler gerekir:  

- Canlı etkinliği yakalamak için kullanılan bir kamera.
- Media Services gönderilir akış bir katkı sinyalleri kamera (veya başka bir cihaz, bir dizüstü bilgisayar gibi) dönüştürür canlı bir video Kodlayıcısı. Sinyaller SCTE-35 işaretçileri gibi bir reklam ilgili katkı akışı içerebilir.
- Alabilmek için etkinleştirmek, Media Services bileşenleri Önizleme, paket, kayıt, şifrelemek ve müşterilerinize veya başkalarına dağıtım için bir CDN için Canlı etkinlik yayını.

Bu makalede ayrıntılı bir genel kılavuzluk sağlar ve Media Services ile canlı akış ilgili ana bileşen diyagramları içerir.

## <a name="live-streaming-workflow"></a>Canlı akış iş akışı

Canlı akış iş akışı için adımlar şunlardır:

1. Oluşturma bir **canlı olay**.
2. Yeni bir **varlık** nesne.
3. Oluşturma bir **Canlı çıkış** oluşturduğunuz varlık adını kullanın.
4. Oluşturma bir **akış ilke** ve **içerik anahtarı** DRM ile içeriğinizi şifrelemek istiyorsanız.
5. DRM kullanılmıyorsa, oluşturun bir **akış Bulucu** yerleşik ile **akış ilke** türleri.
6. Yolları listesini **akış ilke** kullanılacak URL'leri geri almak için (Bu belirleyici).
7. Konak adı için alma **akış uç noktası** gelen akışla aktarmak istediğiniz (akış uç noktası çalıştığından emin olun). 
8. Ana bilgisayar adı, tam bir URL almak için 7. adım adım 6 URL'den birleştirin.
9. Kullanımını durdurmak istiyorsanız, **canlı olay** silerek olay akışı durdurmak zorunda görüntülenebilir, **akış Bulucu**.

Daha fazla bilgi için bir [canlı akış öğretici](stream-live-tutorial-with-api.md) temel [Canlı .NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live) örnek.

## <a name="overview-of-main-components"></a>Ana bileşenlerini genel bakış

En az bir sahip olması Media Services ile isteğe bağlı veya canlı akışlar sunmanıza olanak [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints). Media Services hesabınız oluşturulduğunda bir **varsayılan** StreamingEndpoint hesabınıza eklenen **durduruldu** durumu. İzleyicilerinize için içerik akışı yapmak istediğiniz StreamingEndpoint başlamak için ihtiyacınız. Varsayılan değer kullandığınız **StreamingEndpoint**, veya başka bir özelleştirilmiş oluşturma **StreamingEndpoint** istenen yapılandırma ve CDN ayarları. Her biri farklı bir CDN hedefleme ve içerik teslimi için benzersiz bir ana bilgisayar adı sağlayarak birden çok akış etkinleştirmek isteyebilirsiniz. 

Medya Hizmetleri'nde [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) almak ve canlı video akışları işlenmesinden sorumludur. Bir Livestream oluşturduğunuzda, bir uzak kodlayıcıdan canlı bir sinyal göndermek için kullanabileceğiniz bir giriş uç noktası oluşturulur. Uzaktan gerçek zamanlı Kodlayıcı giriş uç noktası kullanarak katkı için akış gönderir [RTMP](https://www.adobe.com/devnet/rtmp.html) veya [kesintisiz akış](https://msdn.microsoft.com/library/ff469518.aspx) (parçalanmış MP4) protokolü. Kesintisiz akış alma protokolü, desteklenen URL şemalarını `http://` veya `https://`. Desteklenen URL şemalarını için RTMP alma protokolü, `rtmp://` veya `rtmps://`. Daha fazla bilgi için [önerilen Canlı Kodlayıcıları akış](recommended-on-premises-live-encoders.md).<br/>
Oluştururken bir **Livestream**, izin verilen IP adresleri aşağıdaki biçimlerden birinde belirtebilirsiniz: IPv4 adresi 4 sayılarla CIDR adres aralığı.

Bir kez **Livestream** başlatır, akış katkı alma, Önizleme uç noktası (Önizleme ve daha fazla yayımlamadan önce Canlı akışı aldığını doğrulamak için Önizleme URL'si. kullanabilirsiniz. Önizleme akışı iyi olduğuna iade ettikten sonra Livestream canlı akış bir veya daha fazla (önceden oluşturulmuş) aracılığıyla teslimi için kullanılabilir hale getirmek için kullanabileceğiniz **akış**. Bunu yapmak için yeni oluşturduğunuz [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) üzerinde **Livestream**. 

**LiveOutput** catch ve Media Services hesabınızda bir varlığa canlı akış kayıt bant Kaydedici gibi nesnedir. Varlık kaynağı tarafından tanımlı kapsayıcı içine hesabınıza bağlı Azure depolama hesabına kaydedilen içeriği kalıcı.  **LiveOuput** Ayrıca, bazı giden canlı akış, arşiv kaydı (örneğin, bulut DVR Kapasite) akışın ne kadar tutulacağını gibi özellikleri denetlemenize olanak tanır. Döngüsel bir arşiv "penceresinin" arşividir diskte belirtilen içerik miktarını yalnızca tutan **archiveWindowLength** özelliği **LiveOutput**. Bu pencere dışında kalan içerik depolama kapsayıcısından otomatik olarak atılır ve kurtarılabilir durumda değil. Birden çok LiveOutputs (en fazla üç maksimum) bir Livestream farklı arşiv uzunlukları ve ayarlarla oluşturabilirsiniz.  

Media Services ile avantajlarından yararlanabilirsiniz **dinamik paketleme**, Önizleme ve yayın Canlı akışlarınız olanak tanıyan [MPEG DASH, HLS ve kesintisiz akış biçimlerinde](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) gelen akış katkı hizmete gönderin. İzleyicilerinize herhangi HLS, DASH veya kesintisiz akış uyumlu yürütücüler ile canlı akış oynatabilirsiniz. Kullanabileceğiniz [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) web veya mobil uygulamalar, akışınız şu protokollerin birinde sunmak için.

Media Services dinamik olarak şifrelenmiş içerik teslim etmenizi sağlar (**dinamik şifreleme**) Gelişmiş Şifreleme Standardı (AES-128) veya herhangi bir üç ana dijital hak yönetimi (DRM) sistemi: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services, yetkili istemcilere AES anahtarları ve DRM lisanslarını teslim etmek üzere bir hizmet de sağlar. İçeriğinizi Media Services ile şifreleme hakkında daha fazla bilgi için bkz: [içerik genel koruma](content-protection-overview.md)

İsterseniz, parçalar, biçimleri, bit hızlarına dönüştürme ve oyunculara gönderilen sunu zaman pencereleri sayısını denetlemek için kullanılabilen dinamik filtreleme, de uygulayabilirsiniz. Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

### <a name="new-capabilities-for-live-streaming-in-v3"></a>V3 sürümünde canlı akış için yeni özellikler

V3 API'ler, Media Services ile aşağıdaki yeni özelliklerden yararlanır:

- Yeni düşük gecikme süresi modu. Daha fazla bilgi için [gecikme](live-event-latency.md).
- Geliştirilmiş RTMP desteği (daha fazla kararlılık ve daha fazla kaynak Kodlayıcı desteği).
- Güvenli RTMPS alın.<br/>Bir Livestream oluşturduğunuzda, 4 alma URL'lerini alabilirsiniz. 4 alma URL'leri neredeyse aynıdır, yalnızca bir bağlantı noktası numarası bölümü farklı aynı akış belirteci (AppID) sahip. URL'lerin ikisinin, birincil ve yedek RTMPS için.   
- 24 tekli bit hızı katkı için medya hizmetlerine kullanarak Çoklu bit hızlarında bir çıkış akışına zaman akışı saate kadar uzun olan Canlı etkinliklerin akışını yapabilirsiniz. 

## <a name="liveevent-types"></a>Livestream türleri

A [Livestream](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: doğrudan ve canlı kodlama. 

### <a name="pass-through"></a>Geçiş

![geçiş](./media/live-streaming/pass-through.png)

Doğrudan Livestream kullanırken, Çoklu bit hızı video akışı oluşturmak ve katkı (RTMP ya da parçalı MP4 protokolü kullanılarak) Livestream akışına göndermek için şirket içi Canlı Kodlayıcı dayanır. Livestream ardından gelen video akışları herhangi başka bir işlemeye olmadan taşır. 24 x 365 doğrusal canlı akış ya da doğrudan bir Livestream uzun süre çalışan Canlı etkinlikler için optimize edilmiştir. Bu tür bir Livestream oluştururken yok (LiveEventEncodingType.None) belirtin.

Katkı çözünürlüklerde 4 K akışı ve bir karede 60 oranını çerçeveler, H.264/AVC ya da H.265/HEVC video codec bileşenleri ve AAC saniyede gönderebilirsiniz (AAC-LC, HE-AACv1 veya HE-AACv2) ses kodek bileşeni.  Bkz: [Livestream türleri, karşılaştırma ve sınırlamalar](live-event-types-comparison.md) makale daha fazla ayrıntı için.

> [!NOTE]
> Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
> 

Canlı bir örneğe bakın [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live encoding  

![live Encoding](./media/live-streaming/live-encoding.png)

Media Services ile Live encoding kullanıldığında, tek bit hızlı (RTMP veya parçalanmış Mp4 protokolünü kullanarak) Livestream akışı katkı olarak video göndermek için şirket içi Canlı Kodlayıcı yapılandırırsınız. Livestream kodlar, gelen tek bit hızlı akış için bir [birden çok hızlı video akışına](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), MPEG-DASH, HLS ve kesintisiz akış gibi protokolleri aracılığıyla cihazları kayıttan yürütmek teslim için kullanılabilir hale getirir. Bu tür bir Livestream oluştururken, kodlama türü olarak belirtin **standart** (LiveEventEncodingType.Standard).

En 30 kare/saniye, H.264/AVC video codec ve AAC kare hızı 1080 p çözünürlükte en fazla akış katkı gönderebilirsiniz (AAC-LC, HE-AACv1 veya HE-AACv2) ses kodek bileşeni. Bkz: [Livestream türleri, karşılaştırma ve sınırlamalar](live-event-types-comparison.md) makale daha fazla ayrıntı için.

## <a name="liveevent-types-comparison"></a>Livestream türlerini karşılaştırma

Livestream iki özelliklerinin karşılaştıran bir tablo makalesinin içerir: [Karşılaştırma](live-event-types-comparison.md).

## <a name="liveoutput"></a>LiveOutput

A [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) giden canlı akış akışın ne kadar (örneğin, bulut DVR kapasitesini) kaydedilir ve canlı akış izleme görüntüleyiciler olup olmadığını başlayabilirsiniz gibi özellikleri denetlemenize olanak verir. Arasındaki ilişkiyi bir **Livestream** ve kendi **LiveOutput**s ilişki benzer geleneksel televizyon yayına verebileceğiniz bir kanal (**Livestream**) video ve bir kayıt sabit akışını temsil eder (**LiveOutput**) belirli bir zaman kesimine (örneğin, akşam haber 18:30:00 19:00:00 için) kapsamlıdır. Televizyon bir Dijital Video Kaydedici DVR kullanarak kaydedebilirsiniz: LiveEvents eşdeğer özelliği ArchiveWindowLength özelliği aracılığıyla yönetilir. Bu, DVR kapasitesini belirtir ve en az 3 dakika, en çok 25 saat için ayarlanabilir bir ISO 8601 zaman aralığı süresi (örneğin, PTHH:MM:SS) olur.

> [!NOTE]
> **LiveOutput**s oluşturma başlatıp silindiğinde durdurabilir. Sildiğinizde **LiveOutput**, arka plandaki silmezsiniz **varlık** ve varlık içeriği. 
>
> Yayımladıysanız **akış Bulucu**s için varlık üzerinde **LiveOutput**, olay (en fazla DVR pencere uzunluğunun) kadar bitiş saati görüntülenebilir olmaya devam edecek **akış Bulucu**  veya Bulucu sildiğinizde kasa hangisinin önce geldiğine bağlı.   

Daha fazla bilgi için [kullanarak bulut DVR](live-event-cloud-dvr.md).

## <a name="streamingendpoint"></a>StreamingEndpoint

İçine giden akış oluşturduktan sonra **Livestream**, oluşturarak akış olayını başlayabilirsiniz bir **varlık**, **LiveOutput**, ve **StreamingLocator** . Bu akışı arşivler ve aracılığıyla izleyiciler kullanabilmesi [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Media Services hesabınız oluşturulduğunda hesabınıza durdurulmuş durumda bir varsayılan akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının çalışıyor durumda olması gerekir.

## <a name="a-idbilling-liveevent-states-and-billing"></a><a id="billing" />Livestream durumları ve faturalandırma

Bir Livestream durumuna geçer hemen sonra Fatura başlar **çalıştıran**. Faturalandırma gelen Livestream durdurmak için Livestream durdurmak zorunda.

Ayrıntılı bilgi için bkz. [durumları ve faturalandırma](live-event-states-billing.md).

## <a name="latency"></a>Gecikme süresi

LiveEvents gecikme süresi hakkında ayrıntılı bilgi için bkz: [gecikme](live-event-latency.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Livestream türlerini karşılaştırma](live-event-types-comparison.md)
- [Durumları ve faturalandırma](live-event-states-billing.md)
- [Gecikme süresi](live-event-latency.md)
