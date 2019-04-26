---
title: Canlı akış Azure medya Hizmetleri - Canlı olayları ve canlı çıkışları kavramları | Microsoft Docs
description: Bu makalede, Azure Media Services v3 sürümünde canlı akış kavramlarına genel bir bakış sağlar.
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
ms.date: 03/30/2019
ms.author: juliako
ms.openlocfilehash: 00dab8381c26a6331dd325eacd4a550892bd3411
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60325842"
---
# <a name="live-events-and-live-outputs"></a>Canlı Etkinlikler ve Canlı Çıkışlar

Azure Media Services Canlı etkinlikler müşterilerinizin Azure bulutunda dağıtmanıza olanak sağlar. Media Services v3 sürümünde, canlı akış olayları yapılandırmak için bu makalede ele alınan kavramları anlamanız gerekir. <br/>Bölüm listesi, sayfanın sağ tarafındaki listelenir.

## <a name="live-events"></a>Canlı Etkinlikler

[Canlı Etkinlikler](https://docs.microsoft.com/rest/api/media/liveevents) sırasında canlı video akışları alınır ve işlenir. Canlı Etkinlik oluşturduğunuzda uzaktaki bir kodlayıcıdan canlı sinyal göndermek için kullanabileceğiniz bir giriş uç noktası oluşturulur. Uzaktaki gerçek zamanlı kodlayıcı, katılım akışını [RTMP](https://www.adobe.com/devnet/rtmp.html) veya [Kesintisiz Akış](https://msdn.microsoft.com/library/ff469518.aspx) (bölünmüş MP4) protokolünü kullanarak bu giriş uç noktasına gönderir. Kesintisiz akış alma protokolü, desteklenen URL şemalarını `http://` veya `https://`. Desteklenen URL şemalarını için RTMP alma protokolü, `rtmp://` veya `rtmps://`. 

## <a name="live-event-types"></a>Canlı olay türleri

A [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: doğrudan ve canlı kodlama. 

### <a name="pass-through"></a>Geçiş

![doğrudan geçiş](./media/live-streaming/pass-through.svg)

Geçişli **Canlı Etkinlik** seçeneğini kullandığınızda şirket içi gerçek zamanlı kodlayıcı ile çoklu bit hızına sahip video akışı oluşturup katılım akışı olarak Canlı Etkinliğe (RTMP veya bölünmüş MP4 protokolünü kullanarak) gönderirsiniz. Daha sonra Canlı Etkinlik, gelen video akışlarını üzerinde herhangi bir işlem yapmadan iletir. Bu geçişli Canlı Etkinlik, uzun süren canlı etkinlikler veya 24x365 doğrusal canlı akış için iyileştirilmiştir. Bu türde bir Canlı Etkinlik oluştururken None (LiveEventEncodingType.None) seçeneğini kullanın.

H.264/AVC veya H.265/HEVC video codec'leri ve AAC (AAC-LC, HE-AACv1 veya HE-AACv2) ses codec'i ile katılım akışını en fazla 4K çözünürlük ve 60 kare/saniye kare hızıyla gönderebilirsiniz.  Ayrıntılı bilgi için [Canlı Etkinlik türlerinin karşılaştırması](live-event-types-comparison.md) makalesine bakın.

> [!NOTE]
> Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
> 

Bir .NET kod örneğinde bkz [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live encoding  

![gerçek zamanlı kodlama](./media/live-streaming/live-encoding.svg)

Media Services ile gerçek zamanlı kodlama özelliğini kullandığınızda şirket içi gerçek zamanlı kodlayıcınızı Canlı Etkinliğe katılım akışı olarak tek bit hızına sahip video gönderecek şekilde (RTMP veya Bölünmüş Mp4 protokolünü kullanarak) yapılandırmanız gerekir. Canlı Etkinlik, gelen tek bit hızına sahip video akışını [birden çok bit hızına sahip video akışı](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) olarak kodlayarak MPEG-DASH, HLS ve Kesintisiz Akış gibi protokoller aracılığıyla cihazlarda kayıttan yürütmek üzere hazır hale getirir. Bu türde Canlı Etkinlik oluştururken kodlama türünü **Standart** (LiveEventEncodingType.Standard) olarak belirtmeniz gerekir.

H.264/AVC video codec'i ve AAC (AAC-LC, HE-AACv1 veya HE-AACv2) ses codec'i ile katılım akışını en fazla 1080p çözünürlük ve 30 kare/saniye kare hızıyla gönderebilirsiniz. Ayrıntılı bilgi için [Canlı Etkinlik türlerinin karşılaştırması](live-event-types-comparison.md) makalesine bakın.

Gerçek zamanlı kodlama (**Standart** Canlı Etkinlik ile) kullanıldığında gelen akışın birden fazla bit hızı veya katman halinde kodlanma biçimi, kodlama ön ayarı tarafından belirlenir. Bilgi edinmek için bkz. [Sistem ön ayarları](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Şu anda Standart Canlı Etkinlik türü için izin verilen tek ön ayar, *Default720p* ayarıdır. Özel gerçek zamanlı kodlama ön ayarı kullanmanız gerekiyorsa lütfen amshelp@microsoft.com üzerinden bizimle iletişime geçin. İstediğiniz çözünürlüklerin ve bit hızlarının yer aldığı tabloyu paylaşmanız gerekir. 720p çözünürlükte yalnızca bir katman ve toplamda en fazla 6 katman bulunduğundan emin olun.

## <a name="live-event-creation-options"></a>Canlı olay oluşturma seçenekleri

Canlı bir olay oluştururken, aşağıdaki seçenekleri belirtebilirsiniz:

* Canlı olay için Akış Protokolü (şu anda RTMP ve kesintisiz akış protokollerini desteklenmektedir).<br/>Canlı olay veya kendi ilişkili Canlı çıkış çalışıyor durumdayken protokol seçeneğini değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa, her bir akış protokolü için ayrı canlı olay oluşturmanız gerekir.  
* Alma ve önizleme için IP kısıtlamaları. Bu canlı olay video almasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.<br/>Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.<br/>IP adresleri aşağıdaki biçimlerden birinde olması gerekir: IPv4 adresi 4 sayılarla CIDR adres aralığı.
* Etkinlik oluştururken, etkinliğin otomatik başlatılmasını belirtebilirsiniz. <br/>Autostart canlı olay true olarak ayarlandığında, oluşturulduktan sonra başlatılacak. Faturalandırma, canlı olay çalışmaya başladıktan hemen sonra başlar. Daha fazla faturalama durdurmak için canlı olay kaynağı durdurma açıkça çağırmanız gerekir. Alternatif olarak, akış başlamaya hazır olduğunuzda olayı başlatın. 

    Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).

## <a name="live-event-ingest-urls"></a>Canlı etkinlik URL'lerini alma

Canlı Etkinlik oluşturulduktan sonra, şirket içi gerçek zamanlı kodlayıcıya sağlayacağınız alma URL’lerini alabilirsiniz. Gerçek zamanlı kodlayıcı bu URL'leri canlı akış girişi için kullanır. Daha fazla bilgi için [şirket içi Canlı Kodlayıcıları önerilen](recommended-on-premises-live-encoders.md). 

Gösterim amaçlı olmayan URL'leri veya gösterim URL'lerini kullanabilirsiniz. 

* Gösterim olmayan URL'si

    AMS v3'te varsayılan modda gösterim amaçlı olmayan URL'ler kullanılır. Potansiyel olarak Canlı Etkinliği daha hızlı bir şekilde alabilirsiniz ancak alma URL'si yalnızca canlı etkinlik başlatıldığında bildirilir. Canlı Etkinliği durdurup yeniden başlattığınızda URL değişir. <br/>Gösterim amaçlı olmayan URL'ler, son kullanıcının canlı etkinliği mümkün olan en hızlı şekilde almak isteyen ve dinamik alma URL'leriyle uyumlu olan bir uygulama kullanarak akış yapmak istediğinde kullanışlıdır.
* Gösterim URL'si

    Yayın kodlayıcı donanımları kullanan ve Canlı Etkinliği başlattıktan sonra kodlayıcılarını yeniden yapılandırmak istemeyen büyük medya yayımcıları gösterim modunu tercih eder. Bu yayımcılar zaman içinde değişmeyen, tahmine dayalı bir alma URL'sini kullanmayı tercih eder.

> [!NOTE] 
> Alma URL'si Tahmine dayalı olarak bir "gösterim" mod kullanmak ve (URL'sindeki rastgele bir belirteç) kendi erişim belirtecinizi geçmesi gerekir.

### <a name="live-ingest-url-naming-rules"></a>Canlı URL adlandırma kuralları alma

Aşağıdaki *rastgele* dize, 128 bit bir onaltılık sayıdır (0-9 a-f arası 32 karakterden oluşur).<br/>
Sabit URL için aşağıdaki *erişim belirtecini* belirtmeniz gerekir. Bu da 128 bit onaltılık sayıdır.

#### <a name="non-vanity-url"></a>Gösterim olmayan URL'si

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/<access token>`
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/<access token>`
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/<access token>`
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/<access token>`

##### <a name="smooth-streaming"></a>Kesintisiz Akış

`http://<random 128bit hex string>.channel.media.azure.net/<access token>/ingest.isml`
`https://<random 128bit hex string>.channel.media.azure.net/<access token>/ingest.isml`

#### <a name="vanity-url"></a>Gösterim URL'si

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/<access token>`
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/<access token>`
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/<access token>`
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/<access token>`

##### <a name="smooth-streaming"></a>Kesintisiz Akış

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<access token>/ingest.isml`
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<access token>/ingest.isml`

## <a name="live-event-preview-url"></a>Canlı olay Önizleme URL'si

Bir kez **canlı olay** başlatır, akış katkı alma, bir önizleme uç noktası Önizleme ve daha fazla yayımlamadan önce Canlı akışı aldığını doğrulamak için kullanabilirsiniz. Önizleme akışı iyi olduğuna iade ettikten sonra Livestream canlı akış bir veya daha fazla (önceden oluşturulmuş) aracılığıyla teslimi için kullanılabilir hale getirmek için kullanabileceğiniz **akış uç noktalarını**. Bunu yapmak için yeni oluşturduğunuz [Canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs) üzerinde **canlı olay**. 

> [!IMPORTANT]
> Video önizleme URL'sine devam etmeden önce akan emin olun!

## <a name="live-event-long-running-operations"></a>Canlı olay uzun süre çalışan işlemleri

Ayrıntılar için bkz [uzun süre çalışan işlemler](media-services-apis-overview.md#long-running-operations)

## <a name="live-outputs"></a>Canlı Çıkışlar

Canlı olay giden akış oluşturduktan sonra oluşturarak akış olayını başlayabilirsiniz bir [varlık](https://docs.microsoft.com/rest/api/media/assets), [Canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs), ve [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators). Canlı bir çıkış akışı arşivler ve aracılığıyla izleyiciler kullanabilmesi [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

> [!NOTE]
> Oluşturma çıkışları başlangıç canlı ve silindiğinde sona erer. Canlı çıkış sildiğinizde, temel alınan varlık veya varlık içeriği silmekte olduğunuz değil. 

Arasındaki ilişkiyi bir **canlı olay** ve kendi **Canlı çıkışları** benzer geleneksel televizyon yayın verebileceğiniz bir kanaldır (**canlı olay**) bir sabiti temsil eder Stream, video ve bir kayıt (**Canlı çıkış**) belirli bir zaman kesimine (örneğin, akşam haber 18:30:00 19:00:00 için) kapsamlıdır. Televizyonu bir Dijital Video Kaydedici (DVR) kullanarak kaydedebileceğiniz gibi Canlı Etkinliklerde de bu durumu **ArchiveWindowLength** özelliğiyle yönetebilirsiniz. Bu, DVR kapasitesini belirtir ve en az 3 dakika, en çok 25 saat için ayarlanabilir bir ISO 8601 zaman aralığı süresi (örneğin, PTHH:MM:SS) olur.

**Canlı çıkış** catch ve Media Services hesabınızda bir varlığa canlı akış kayıt bant Kaydedici gibi nesnedir. Varlık kaynağı tarafından tanımlı kapsayıcı içine hesabınıza bağlı Azure depolama hesabına kaydedilen içeriği kalıcı. **Canlı çıkış** Ayrıca, bazı akışın ne kadar arşiv kaydı (örneğin, bulut DVR Kapasite) tutulur ve görüntüleyiciler olup olmadığını başlayabilirsiniz gibi giden canlı akış özellikleri denetlemenize olanak tanır Canlı akış izleme. Döngüsel bir arşiv "penceresinin" arşividir diskte belirtilen içerik miktarını yalnızca tutan **archiveWindowLength** özelliği **Canlı çıkış**. Bu pencere dışında kalan içerik depolama kapsayıcısından otomatik olarak atılır ve kurtarılabilir durumda değil. Birden çok oluşturabilirsiniz **Canlı çıkışları** (yukarı üç en) üzerinde bir **canlı olay** farklı arşiv uzunlukları ve ayarları.  

Yayımladıysanız **Canlı çıkış**'s **varlık** kullanarak bir **akış Bulucu**, **canlı olay** olur (DVR pencere uzunluğunun en fazla) Akış Bulucu'nın süre sonu veya silme kadar görüntülenebilir devam, hangisinin önce geldiğine.

Daha fazla bilgi için [kullanılarak bir bulut DVR](live-event-cloud-dvr.md).

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)
