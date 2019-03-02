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
ms.date: 03/01/2019
ms.author: juliako
ms.openlocfilehash: c4be56b3ee32a5177c66353ba45c6b3647c732f2
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57240091"
---
# <a name="live-events-and-live-outputs"></a>Canlı Etkinlikler ve Canlı Çıkışlar

Azure Media Services Canlı etkinlikler müşterilerinizin Azure bulutunda dağıtmanıza olanak sağlar. Media Services v3 sürümünde, canlı akış olayları yapılandırmak için bu makalede ele alınan kavramları anlamanız gerekir:

* [Canlı etkinlikler](#live-events)
* Canlı olay türleri
* Canlı olay türlerini karşılaştırma
* [Canlı olay oluşturma seçenekleri](#live-event-creation-options)
* [Canlı etkinlik URL'lerini alma](#live-event-ingest-urls)
* [Canlı olay Önizleme URL'si](#live-event-preview-url)
* [Çıkışlar canlı](#live-outputs).

## <a name="live-events"></a>Canlı Etkinlikler

[Canlı etkinlikler](https://docs.microsoft.com/rest/api/media/liveevents) almak ve canlı video akışları işlenmesinden sorumludur. Canlı bir olay oluşturduğunuzda, bir uzak kodlayıcıdan canlı bir sinyal göndermek için kullanabileceğiniz bir giriş uç noktası oluşturulur. Uzaktan gerçek zamanlı Kodlayıcı giriş uç noktası kullanarak katkı için akış gönderir [RTMP](https://www.adobe.com/devnet/rtmp.html) veya [kesintisiz akış](https://msdn.microsoft.com/library/ff469518.aspx) (parçalanmış MP4) protokolü. Kesintisiz akış alma protokolü, desteklenen URL şemalarını `http://` veya `https://`. Desteklenen URL şemalarını için RTMP alma protokolü, `rtmp://` veya `rtmps://`. 

## <a name="live-event-types"></a>Canlı olay türleri

A [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: doğrudan ve canlı kodlama. 

### <a name="pass-through"></a>Geçiş

![geçiş](./media/live-streaming/pass-through.svg)

Doğrudan kullanırken **canlı olay**, Çoklu bit hızı video akışı oluşturmak ve katkı Canlı (RTMP ya da parçalı MP4 protokolü kullanılarak) olay için akışı göndermek için şirket içi Canlı Kodlayıcı dayanır. Canlı olay ardından gelen video akışları herhangi başka bir işlemeye olmadan taşır. 24 x 365 doğrusal canlı akış ya da doğrudan bir Livestream uzun süre çalışan Canlı etkinlikler için optimize edilmiştir. Bu canlı olay türünü oluştururken yok (LiveEventEncodingType.None) belirtin.

Katkı çözünürlüklerde 4 K akışı ve bir karede 60 oranını çerçeveler, H.264/AVC ya da H.265/HEVC video codec bileşenleri ve AAC saniyede gönderebilirsiniz (AAC-LC, HE-AACv1 veya HE-AACv2) ses kodek bileşeni.  Bkz: [canlı olay türlerini karşılaştırma](live-event-types-comparison.md) makale daha fazla ayrıntı için.

> [!NOTE]
> Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
> 

Bir .NET kod örneğinde bkz [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live encoding  

![live Encoding](./media/live-streaming/live-encoding.svg)

Media Services ile Live encoding kullanıldığında, tek bit hızlı Canlı (RTMP veya parçalanmış Mp4 protokolünü kullanarak) olay için akışı katkı olarak video göndermek için şirket içi Canlı Kodlayıcı yapılandırırsınız. Bu gelen tek bit hızlı canlı olay kodlar için akış bir [birden çok hızlı video akışına](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), MPEG-DASH, HLS ve kesintisiz akış gibi protokolleri aracılığıyla cihazları kayıttan yürütmek teslim için kullanılabilir hale getirir. Bu canlı olay türünü oluştururken, kodlama türü olarak belirtin **standart** (LiveEventEncodingType.Standard).

En 30 kare/saniye, H.264/AVC video codec ve AAC kare hızı 1080 p çözünürlükte en fazla akış katkı gönderebilirsiniz (AAC-LC, HE-AACv1 veya HE-AACv2) ses kodek bileşeni. Bkz: [canlı olay türlerini karşılaştırma](live-event-types-comparison.md) makale daha fazla ayrıntı için.

Live encoding kullanıldığında (canlı olay kümesine **standart**), Çoklu bit hızlarında veya katmanları gelen nasıl kodlandığını kodlama Önayarı tanımlar. Bilgi için [sistem önayarlarını](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Standart canlı olay türü için şu anda yalnızca hazır değer izin verilen maksimum *Default720p*. Özel bir canlı kodlama Önayarı kullanmanız gerekiyorsa, lütfen başvurun amshelp@microsoft.com. İstediğiniz tabloyu çözünürlük ve bit hızlarına dönüştürme belirtmeniz gerekir. 720 p yalnızca bir katmanında ve en fazla 6 katmanları olduğundan emin olun.

## <a name="live-event-creation-options"></a>Canlı olay oluşturma seçenekleri

Canlı bir olay oluştururken, aşağıdaki seçenekleri belirtebilirsiniz:

* Canlı olay için Akış Protokolü (şu anda RTMP ve kesintisiz akış protokollerini desteklenmektedir).<br/>Canlı olay veya kendi ilişkili Canlı çıkış çalışıyor durumdayken protokol seçeneğini değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa, her bir akış protokolü için ayrı canlı olay oluşturmanız gerekir.  
* Alma ve önizleme için IP kısıtlamaları. Bu canlı olay video almasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.<br/>Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.<br/>IP adresleri aşağıdaki biçimlerden birinde olması gerekir: IPv4 adresi 4 sayılarla CIDR adres aralığı.
* Etkinlik oluştururken, etkinliğin otomatik başlatılmasını belirtebilirsiniz. <br/>Autostart canlı olay true olarak ayarlandığında, oluşturulduktan sonra başlatılacak. Faturalandırma, canlı olay çalışmaya başladıktan hemen sonra başlar. Daha fazla faturalama durdurmak için canlı olay kaynağı durdurma açıkça çağırmanız gerekir. Alternatif olarak, akış başlamaya hazır olduğunuzda olayı başlatın. 

    Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).

## <a name="live-event-ingest-urls"></a>Canlı etkinlik URL'lerini alma

Canlı olay oluşturulduktan sonra alabilirsiniz şirket Canlı kodlayıcıya sağlayacağınız URL'lerini alabilirsiniz. Gerçek zamanlı Kodlayıcı bu URL'leri canlı akış girişi için kullanır. Daha fazla bilgi için [şirket içi Canlı Kodlayıcıları önerilen](recommended-on-premises-live-encoders.md). 

Gösterim olmayan URL'ler veya gösterim URL'leri ya da kullanabilirsiniz. 

* Gösterim olmayan URL'si

    AMS v3 varsayılan modunda olmayan gösterim URL'dir. Potansiyel olarak canlı olay hızlıca ancak alma URL'si yalnızca canlı etkinliği başlatıldığında bilinir. Durdurmak/başlatmak canlı olay varsa, URL'sini değiştirir. <br/>Son kullanıcı burada Canlı etkinlik EE almak uygulamanın istediği ve alma URL'si dinamik olan bir sorun değil bir uygulama kullanarak akış istediğinde gösterim olmayan senaryolarda yararlıdır.
* Gösterim URL'si

    Gösterim modu donanım kullanan yayıncıları kodlayıcılar yayın ve kendi kodlayıcılarda canlı olay başlatmaları ne zaman yeniden yapılandırmak istemiyorsanız büyük medya tarafından tercih edilir. Tahmine dayalı bir alma zaman içinde değiştirmez URL'si, isterler.

> [!NOTE] 
> Alma URL'si Tahmine dayalı olarak bir "gösterim" mod kullanmak ve (URL'sindeki rastgele bir belirteç) kendi erişim belirtecinizi geçmesi gerekir.

### <a name="live-ingest-url-naming-rules"></a>Canlı URL adlandırma kuralları alma

*Rastgele* aşağıdaki dizedir 128-bit onaltılık bir sayı (0-9 32 karakterlerinden oluşan a-f).<br/>
*Erişim belirteci* sabit URL'sini belirtmek için ihtiyacınız olanlar aşağıda verilmiştir. Ayrıca, 128 bit onaltılık bir sayı değildir.

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

## <a name="live-outputs"></a>Canlı Çıkışlar

Canlı olay giden akış oluşturduktan sonra oluşturarak akış olayını başlayabilirsiniz bir [varlık](https://docs.microsoft.com/rest/api/media/assets), [Canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs), ve [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators). Canlı bir çıkış akışı arşivler ve aracılığıyla izleyiciler kullanabilmesi [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

> [!NOTE]
> Oluşturma çıkışları başlangıç canlı ve silindiğinde sona erer. Canlı çıkış sildiğinizde, temel alınan varlık veya varlık içeriği silmekte olduğunuz değil. 

Arasındaki ilişkiyi bir **canlı olay** ve kendi **Canlı çıkışları** benzer geleneksel televizyon yayın verebileceğiniz bir kanaldır (**canlı olay**) bir sabiti temsil eder Stream, video ve bir kayıt (**Canlı çıkış**) belirli bir zaman kesimine (örneğin, akşam haber 18:30:00 19:00:00 için) kapsamlıdır. Televizyon bir Dijital Video Kaydedici DVR kullanarak kaydedebilirsiniz: Canlı olayları eşdeğer özelliği aracılığıyla yönetilen **ArchiveWindowLength** özelliği. Bu, DVR kapasitesini belirtir ve en az 3 dakika, en çok 25 saat için ayarlanabilir bir ISO 8601 zaman aralığı süresi (örneğin, PTHH:MM:SS) olur.

**Canlı çıkış** catch ve Media Services hesabınızda bir varlığa canlı akış kayıt bant Kaydedici gibi nesnedir. Varlık kaynağı tarafından tanımlı kapsayıcı içine hesabınıza bağlı Azure depolama hesabına kaydedilen içeriği kalıcı. **Canlı çıkış** Ayrıca, bazı akışın ne kadar arşiv kaydı (örneğin, bulut DVR Kapasite) tutulur ve görüntüleyiciler olup olmadığını başlayabilirsiniz gibi giden canlı akış özellikleri denetlemenize olanak tanır Canlı akış izleme. Döngüsel bir arşiv "penceresinin" arşividir diskte belirtilen içerik miktarını yalnızca tutan **archiveWindowLength** özelliği **Canlı çıkış**. Bu pencere dışında kalan içerik depolama kapsayıcısından otomatik olarak atılır ve kurtarılabilir durumda değil. Birden çok oluşturabilirsiniz **Canlı çıkışları** (yukarı üç en) üzerinde bir **canlı olay** farklı arşiv uzunlukları ve ayarları.  

Yayımladıysanız **Canlı çıkış**'s **varlık** kullanarak bir **akış Bulucu**, **canlı olay** olur (DVR pencere uzunluğunun en fazla) Akış Bulucu'nın süre sonu veya silme kadar görüntülenebilir devam, hangisinin önce geldiğine.

Daha fazla bilgi için [kullanılarak bir bulut DVR](live-event-cloud-dvr.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı etkinlik akışı](live-streaming-overview.md)
- [Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)
