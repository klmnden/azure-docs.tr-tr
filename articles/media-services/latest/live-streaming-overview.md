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
ms.date: 01/22/2019
ms.author: juliako
ms.openlocfilehash: e7e5ddf06fc98f13b9848bf5ede208eef47efd69
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54886671"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Canlı akış ile Azure Media Services v3

Azure Media Services Canlı etkinlikler müşterilerinizin Azure bulutunda dağıtmanıza olanak sağlar. Media Services ile etkinliklerin canlı akış için aşağıdakiler gerekir:  

- Canlı etkinliği yakalamak için kullanılan bir kamera.
- Media Services gönderilir akış bir katkı sinyalleri kamera (veya başka bir cihaz, bir dizüstü bilgisayar gibi) dönüştürür canlı bir video Kodlayıcısı. Sinyaller SCTE-35 işaretçileri gibi bir reklam ilgili katkı akışı içerebilir.
- Alabilmek için etkinleştirmek, Media Services bileşenleri Önizleme, paket, kayıt, şifrelemek ve müşterilerinize veya başkalarına dağıtım için bir CDN için Canlı etkinlik yayını.

Bu makalede ayrıntılı bir genel kılavuzluk sağlar ve Media Services ile canlı akış ilgili ana bileşen diyagramları içerir.

## <a name="live-streaming-workflow"></a>Canlı akış iş akışı

Canlı akış iş akışı için adımlar şunlardır:

1. Emin **akış uç noktası** çalışıyor. 
2. Oluşturma bir **canlı olay**. <br/>Olay oluşturulurken otomatik başlatma için bunu belirtebilirsiniz. Alternatif olarak, akış başlamaya hazır olduğunuzda olayı başlatın.<br/> Autostart canlı olay true olarak ayarlandığında oluşturulduktan sonra doğru başlatılır. Bu, canlı olay çalışmaya başladıktan hemen sonra Fatura başlatır anlamına gelir. Daha fazla faturalama durdurmak için canlı olay kaynağı durdurma açıkça çağırmanız gerekir. Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).
3. Alma URL'leri alma ve akış katkı göndermek için URL'yi kullanmak için şirket içi Kodlayıcı yapılandırın.<br/>Bkz: [gerçek zamanlı kodlayıcılar önerilen](recommended-on-premises-live-encoders.md).
4. Önizleme URL'sini ve aslında kodlayıcıdan giriş alındığını doğrulamak için kullanın.
5. Yeni bir **varlık** nesne.
6. Oluşturma bir **Canlı çıkış** oluşturduğunuz varlık adını kullanın.<br/>**Canlı çıkış** akışa arşiv **varlık**.
7. Oluşturma bir **akış Bulucu** yerleşik ile **akış ilke** türleri.<br/>İçeriğinizi şifrelemek istiyorsanız, gözden [içerik korumaya genel bakış](content-protection-overview.md).
8. Yolları listesini **akış Bulucu** kullanılacak URL'leri geri almak için (Bu belirleyici).
9. Konak adı için alma **akış uç noktası** alanından akışını yapmak istiyor.
10. Ana bilgisayar adı tam URL'sini almak için 9. adım 8. adımdaki URL'yi birleştirin.
11. Kullanımını durdurmak istiyorsanız, **canlı olay** silme ve olay akışı durdurmak zorunda görüntülenebilir, **akış Bulucu**.<br/>Daha fazla bilgi için [canlı akış öğretici](stream-live-tutorial-with-api.md).

## <a name="overview-of-main-components"></a>Ana bileşenlerini genel bakış

En az bir sahip olması Media Services ile isteğe bağlı veya canlı akışlar sunmanıza olanak [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints). Media Services hesabınız oluşturulduğunda bir **varsayılan** StreamingEndpoint hesabınıza eklenen **durduruldu** durumu. İzleyicilerinize için içerik akışı yapmak istediğiniz StreamingEndpoint başlamak için ihtiyacınız. Varsayılan değer kullandığınız **akış uç noktası**, veya başka bir özelleştirilmiş oluşturma **akış uç noktası** istenen yapılandırma ve CDN ayarları. Birden çok etkinleştirmeye karar verebilir **akış uç noktalarını**, her biri farklı bir CDN hedefleme ve benzersiz bir ana bilgisayar adı için içerik teslimi sağlar. 

Medya Hizmetleri'nde [Canlı olayları](https://docs.microsoft.com/rest/api/media/liveevents) almak ve canlı video akışları işlenmesinden sorumludur. Canlı bir olay oluşturduğunuzda, bir uzak kodlayıcıdan canlı bir sinyal göndermek için kullanabileceğiniz bir giriş uç noktası oluşturulur. Uzaktan gerçek zamanlı Kodlayıcı giriş uç noktası kullanarak katkı için akış gönderir [RTMP](https://www.adobe.com/devnet/rtmp.html) veya [kesintisiz akış](https://msdn.microsoft.com/library/ff469518.aspx) (parçalanmış MP4) protokolü. Kesintisiz akış alma protokolü, desteklenen URL şemalarını `http://` veya `https://`. Desteklenen URL şemalarını için RTMP alma protokolü, `rtmp://` veya `rtmps://`. Daha fazla bilgi için [önerilen Canlı Kodlayıcıları akış](recommended-on-premises-live-encoders.md).<br/>
Oluştururken bir **canlı olay**, izin verilen IP adresleri aşağıdaki biçimlerden birinde belirtebilirsiniz: IPv4 adresi 4 sayılarla CIDR adres aralığı.

Bir kez **canlı olay** başlatır, akış katkı alma, Önizleme uç noktası (Önizleme ve daha fazla yayımlamadan önce Canlı akışı aldığını doğrulamak için Önizleme URL'si. kullanabilirsiniz. Önizleme akışı iyi olduğuna iade ettikten sonra Livestream canlı akış bir veya daha fazla (önceden oluşturulmuş) aracılığıyla teslimi için kullanılabilir hale getirmek için kullanabileceğiniz **akış uç noktalarını**. Bunu yapmak için yeni oluşturduğunuz [Canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs) üzerinde **canlı olay**. 

**Canlı çıkış** catch ve Media Services hesabınızda bir varlığa canlı akış kayıt bant Kaydedici gibi nesnedir. Varlık kaynağı tarafından tanımlı kapsayıcı içine hesabınıza bağlı Azure depolama hesabına kaydedilen içeriği kalıcı.  **Canlı çıkış** Ayrıca, bazı giden canlı akış, arşiv kaydı (örneğin, bulut DVR Kapasite) akışın ne kadar tutulacağını gibi özellikleri denetlemenize olanak tanır. Döngüsel bir arşiv "penceresinin" arşividir diskte belirtilen içerik miktarını yalnızca tutan **archiveWindowLength** özelliği **Canlı çıkış**. Bu pencere dışında kalan içerik depolama kapsayıcısından otomatik olarak atılır ve kurtarılabilir durumda değil. Birden çok oluşturabilirsiniz **Canlı çıkışları** (yukarı üç en) üzerinde bir **canlı olay** farklı arşiv uzunlukları ve ayarları.  

Media Services ile avantajlarından yararlanabilirsiniz **dinamik paketleme**, Önizleme ve yayın Canlı akışlarınız olanak tanıyan [MPEG DASH, HLS ve kesintisiz akış biçimlerinde](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) gelen akış katkı hizmete gönderin. İzleyicilerinize herhangi HLS, DASH veya kesintisiz akış uyumlu yürütücüler ile canlı akış oynatabilirsiniz. Kullanabileceğiniz [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) web veya mobil uygulamalar, akışınız şu protokollerin birinde sunmak için.

Media Services dinamik olarak şifrelenmiş içerik teslim etmenizi sağlar (**dinamik şifreleme**) Gelişmiş Şifreleme Standardı (AES-128) veya herhangi bir üç ana dijital hak yönetimi (DRM) sistemi: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services, yetkili istemcilere AES anahtarları ve DRM lisanslarını teslim etmek üzere bir hizmet de sağlar. İçeriğinizi Media Services ile şifreleme hakkında daha fazla bilgi için bkz: [içerik genel koruma](content-protection-overview.md)

İsterseniz, parçalar, biçimleri, bit hızlarına dönüştürme ve oyunculara gönderilen sunu zaman pencereleri sayısını denetlemek için kullanılabilen dinamik filtreleme, de uygulayabilirsiniz. Daha fazla bilgi için [filtreleri ve dinamik bildirimlere](filters-dynamic-manifest-overview.md).

V3 sürümünde canlı akış için yeni özellikler hakkında daha fazla bilgi için bkz. [Media Services v2'den v3 taşımak için geçiş kılavuzunu](migrate-from-v2-to-v3.md).

## <a name="live-event-types"></a>Canlı olay türleri

A [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: doğrudan ve canlı kodlama. 

### <a name="pass-through"></a>Geçiş

![geçiş](./media/live-streaming/pass-through.png)

Doğrudan kullanırken **canlı olay**, Çoklu bit hızı video akışı oluşturmak ve katkı Canlı (RTMP ya da parçalı MP4 protokolü kullanılarak) olay için akışı göndermek için şirket içi Canlı Kodlayıcı dayanır. Canlı olay ardından gelen video akışları herhangi başka bir işlemeye olmadan taşır. 24 x 365 doğrusal canlı akış ya da doğrudan bir Livestream uzun süre çalışan Canlı etkinlikler için optimize edilmiştir. Bu canlı olay türünü oluştururken yok (LiveEventEncodingType.None) belirtin.

Katkı çözünürlüklerde 4 K akışı ve bir karede 60 oranını çerçeveler, H.264/AVC ya da H.265/HEVC video codec bileşenleri ve AAC saniyede gönderebilirsiniz (AAC-LC, HE-AACv1 veya HE-AACv2) ses kodek bileşeni.  Bkz: [karşılaştırma ve sınırlamalar canlı olay türleri](live-event-types-comparison.md) makale daha fazla ayrıntı için.

> [!NOTE]
> Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
> 

Canlı bir örneğe bakın [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live encoding  

![live Encoding](./media/live-streaming/live-encoding.png)

Media Services ile Live encoding kullanıldığında, tek bit hızlı Canlı (RTMP veya parçalanmış Mp4 protokolünü kullanarak) olay için akışı katkı olarak video göndermek için şirket içi Canlı Kodlayıcı yapılandırırsınız. Bu gelen tek bit hızlı canlı olay kodlar için akış bir [birden çok hızlı video akışına](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), MPEG-DASH, HLS ve kesintisiz akış gibi protokolleri aracılığıyla cihazları kayıttan yürütmek teslim için kullanılabilir hale getirir. Bu canlı olay türünü oluştururken, kodlama türü olarak belirtin **standart** (LiveEventEncodingType.Standard).

En 30 kare/saniye, H.264/AVC video codec ve AAC kare hızı 1080 p çözünürlükte en fazla akış katkı gönderebilirsiniz (AAC-LC, HE-AACv1 veya HE-AACv2) ses kodek bileşeni. Bkz: [karşılaştırma ve sınırlamalar canlı olay türleri](live-event-types-comparison.md) makale daha fazla ayrıntı için.

## <a name="live-event-types-comparison"></a>Canlı olay türlerini karşılaştırma

İki tür canlı olay özelliklerinin karşılaştıran bir tablo makalesinin içerir: [Karşılaştırma](live-event-types-comparison.md).

## <a name="live-output"></a>Canlı çıkış

A [Canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs) giden canlı akış akışın ne kadar (örneğin, bulut DVR kapasitesini) kaydedilir ve canlı akış izleme görüntüleyiciler olup olmadığını başlayabilirsiniz gibi özellikleri denetlemenize olanak verir. Arasındaki ilişkiyi bir **canlı olay** ve kendi **Canlı çıkış**s ilişki benzer geleneksel televizyon yayına verebileceğiniz bir kanal (**canlı olay**) video ve bir kayıt sabit akışını temsil eder (**Canlı çıkış**) belirli bir zaman kesimine (örneğin, akşam haber 18:30:00 19:00:00 için) kapsamlıdır. Televizyon bir Dijital Video Kaydedici DVR kullanarak kaydedebilirsiniz: LiveEvents eşdeğer özelliği ArchiveWindowLength özelliği aracılığıyla yönetilir. Bu, DVR kapasitesini belirtir ve en az 3 dakika, en çok 25 saat için ayarlanabilir bir ISO 8601 zaman aralığı süresi (örneğin, PTHH:MM:SS) olur.

> [!NOTE]
> **Çıkış canlı**s oluşturma başlatıp silindiğinde durdurabilir. Sildiğinizde **Canlı çıkış**, arka plandaki silmezsiniz **varlık** ve varlık içeriği. 
>
> Yayımladıysanız **Canlı çıkış** varlık kullanan bir **akış Bulucu**, **canlı olay** (DVR pencere uzunluğunun en fazla) kadar görüntülenebilirolmayadevamedecek**Akış Bulucu**'s süre sonu veya silme işlemi, hangisi gelir önce.

Daha fazla bilgi için [kullanarak bulut DVR](live-event-cloud-dvr.md).

## <a name="streaming-endpoint"></a>Akış Uç Noktası

İçine giden akış oluşturduktan sonra **canlı olay**, oluşturarak akış olayını başlayabilirsiniz bir **varlık**, **Canlı çıkış**, ve **akış Bulucu** . Bu akışı arşivler ve aracılığıyla izleyiciler kullanabilmesi [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Media Services hesabınız oluşturulduğunda hesabınıza durdurulmuş durumda bir varsayılan akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının çalışıyor durumda olması gerekir.

## <a name="a-idbilling-live-event-states-and-billing"></a><a id="billing" />Canlı olay durumları ve faturalandırma

A **canlı olay** durumuna geçer hemen sonra Fatura başlar **çalıştıran**. Faturalandırma canlı olay durdurmak için Canlı etkinliği durdurmak zorunda.

Ayrıntılı bilgi için bkz. [durumları ve faturalandırma](live-event-states-billing.md).

## <a name="latency"></a>Gecikme süresi

LiveEvents gecikme süresi hakkında ayrıntılı bilgi için bkz: [gecikme](live-event-latency.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı olay türlerini karşılaştırma](live-event-types-comparison.md)
- [Durumları ve faturalandırma](live-event-states-billing.md)
- [Gecikme süresi](live-event-latency.md)
