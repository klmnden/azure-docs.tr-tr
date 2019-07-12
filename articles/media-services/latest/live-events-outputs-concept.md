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
ms.date: 06/19/2019
ms.author: juliako
ms.openlocfilehash: a951ebd46335ad4639b8499283ddd30f13edd64e
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605645"
---
# <a name="live-events-and-live-outputs"></a>Canlı Etkinlikler ve Canlı Çıkışlar

Azure Media Services Canlı etkinlikler müşterilerinizin Azure bulutunda dağıtmanıza olanak sağlar. Media Services v3 sürümünde, canlı akış olayları yapılandırmak için bu makalede ele alınan kavramları anlamanız gerekir.

> [!TIP]
> Media Services v2 API'lerinden geçişini gerçekleştiren müşteriler için **canlı olay** varlık değiştirir **kanal** v2'de ve **Canlı çıkış** değiştirir **Program**.

## <a name="live-events"></a>Canlı Etkinlikler

[Canlı Etkinlikler](https://docs.microsoft.com/rest/api/media/liveevents) sırasında canlı video akışları alınır ve işlenir. Canlı bir olay oluşturduğunuzda, bir uzak kodlayıcıdan canlı bir sinyal göndermek için kullanabileceğiniz birincil ve ikincil giriş uç noktası oluşturulur. Uzaktan gerçek zamanlı Kodlayıcı giriş uç noktası kullanarak katkı için akış gönderir [RTMP](https://www.adobe.com/devnet/rtmp.html) veya [kesintisiz akış](https://msdn.microsoft.com/library/ff469518.aspx) giriş Protokolü (parçalanmış MP4). RTMP alma protokolü için açık bir şekilde içerik gönderilebilir (`rtmp://`) ya da kablo üzerinde güvenli bir şekilde şifrelenir (`rtmps://`). Kesintisiz akış alma protokolü, desteklenen URL şemalarını `http://` veya `https://`.  

## <a name="live-event-types"></a>Canlı olay türleri

A [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: doğrudan ve canlı kodlama. Oluşturma işlemi sırasında kullanarak türleri Ayarla [LiveEventEncodingType](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype):

* **LiveEventEncodingType.None** -şirket içi Canlı Kodlayıcı, bir Çoklu bit hızlı akış gönderir. Alınan akışların herhangi başka bir işlemeye gerek kalmadan canlı olay geçirir. 
* **LiveEventEncodingType.Standard** - bir şirket içinde gerçek zamanlı Kodlayıcı, Media Services ve canlı olay bir tek bit hızlı akışın Çoklu bit hızı akışları oluşturur gönderir. Katkı akış 720 p veya daha yüksek çözünürlükte ise **Default720p** hazır 6 çözümleme/bit hızlarına dönüştürme çiftleri kümesini kodlama.
* **LiveEventEncodingType.Premium1080p** - bir şirket içinde gerçek zamanlı Kodlayıcı, Media Services ve canlı olay bir tek bit hızlı akışın Çoklu bit hızı akışları oluşturur gönderir. Default1080p hazır çözüm/bit hızlarına dönüştürme çiftleri çıkış kümesini belirtir. 

### <a name="pass-through"></a>Geçiş

![doğrudan geçiş](./media/live-streaming/pass-through.svg)

Geçişli **Canlı Etkinlik** seçeneğini kullandığınızda şirket içi gerçek zamanlı kodlayıcı ile çoklu bit hızına sahip video akışı oluşturup katılım akışı olarak Canlı Etkinliğe (RTMP veya bölünmüş MP4 protokolünü kullanarak) gönderirsiniz. Daha sonra Canlı Etkinlik, gelen video akışlarını üzerinde herhangi bir işlem yapmadan iletir. Bu tür bir doğrudan canlı olay uzun süre çalışan Canlı etkinlikler için optimize edilmiştir veya 24 x 365 doğrusal canlı akış. Bu türde bir Canlı Etkinlik oluştururken None (LiveEventEncodingType.None) seçeneğini kullanın.

H.264/AVC veya H.265/HEVC video codec'leri ve AAC (AAC-LC, HE-AACv1 veya HE-AACv2) ses codec'i ile katılım akışını en fazla 4K çözünürlük ve 60 kare/saniye kare hızıyla gönderebilirsiniz.  Ayrıntılı bilgi için [Canlı Etkinlik türlerinin karşılaştırması](live-event-types-comparison.md) makalesine bakın.

> [!NOTE]
> Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
> 

Bir .NET kod örneğinde bkz [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live encoding  

![gerçek zamanlı kodlama](./media/live-streaming/live-encoding.svg)

Media Services ile gerçek zamanlı kodlama özelliğini kullandığınızda şirket içi gerçek zamanlı kodlayıcınızı Canlı Etkinliğe katılım akışı olarak tek bit hızına sahip video gönderecek şekilde (RTMP veya Bölünmüş Mp4 protokolünü kullanarak) yapılandırmanız gerekir. Ardından, gelen tek bit hızlı kodlar, canlı bir olay ayarlamalı için akış bir [birden çok hızlı video akışına](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)ve çıkış MPEG-DASH, HLS, gibi protokolleri aracılığıyla cihazları kayıttan yürütme ve kesintisiz teslim için kullanılabilir hale getirir Akış.

Gerçek zamanlı kodlama kullandığınızda, yalnızca çözünürlüklerde 30 kare/saniye, H.264/AVC video codec ve AAC kare hızı 1080 p çözünürlükte akış katkı gönderebilirsiniz (AAC-LC, HE-AACv1 veya HE-AACv2) ses kodek bileşeni. Canlı geçiş olayları çözümleri avc'de 4 K en 60 kare/saniye sürümlerin desteklendiğini unutmayın. Ayrıntılı bilgi için [Canlı Etkinlik türlerinin karşılaştırması](live-event-types-comparison.md) makalesine bakın.

Çözünürlüklerine ve bit hızlarına dönüştürme gerçek zamanlı Kodlayıcı çıkışı yer alan ön ayarı tarafından belirlenir. Kullanılıyorsa bir **standart** Canlı Kodlayıcı (LiveEventEncodingType.Standard) sonra *Default720p* hazır 3.5Mbps aşağı 200 Kbps 192 p konumunda 720 p gitmek 6 çözümleme/bit oranı çiftleri kümesi belirtir. Aksi takdirde kullanıyorsanız bir **Premium1080p** Canlı Kodlayıcı (LiveEventEncodingType.Premium1080p) sonra *Default1080p* hazır 1080 p konumunda 3.5Mbps gitmek 6 çözümleme/bit oranı çiftleri kümesi belirtir 200 Kbps 180 p gösteriyor. Bilgi edinmek için bkz. [Sistem ön ayarları](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Canlı kodlama Önayarı özelleştirmek istiyorsanız lütfen Azure portalı üzerinden bir destek bileti açın. İstediğiniz çözünürlüklerin ve bit hızlarının yer aldığı tabloyu paylaşmanız gerekir. 720 p (standart bir gerçek zamanlı Kodlayıcı için bir ön ayarı isteyen olursa) veya 1080 p (önceden ayarlanmış bir Premium1080p gerçek zamanlı Kodlayıcı için isteyen olursa) yalnızca bir katmanı ve en fazla 6 katmanları olduğundan emin olun.

## <a name="live-event-creation-options"></a>Canlı olay oluşturma seçenekleri

Canlı bir olay oluştururken, aşağıdaki seçenekleri belirtebilirsiniz:

* Canlı olay için Akış Protokolü (şu anda RTMP ve kesintisiz akış protokollerini desteklenmektedir).<br/>Canlı olay veya kendi ilişkili Canlı çıkış çalışıyor durumdayken protokol seçeneğini değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa, her bir akış protokolü için ayrı canlı olay oluşturmanız gerekir.  
* Alma ve önizleme için IP kısıtlamaları. Bu canlı olay video almasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek bir IP adresi (örneğin '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1/22') veya bir IP adresi ve bir noktalı ondalık alt ağ maskesi kullanan bir IP aralığı (örneğin '10.0.0.1(255.255.252.0)') olabilir.<br/>Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.<br/>IP adresleri aşağıdaki biçimlerden birinde olması gerekir: IPv4 adresi 4 sayılarla CIDR adres aralığı.
* Etkinlik oluştururken, etkinliğin otomatik başlatılmasını belirtebilirsiniz. <br/>Autostart canlı olay true olarak ayarlandığında, oluşturulduktan sonra başlatılacak. Faturalandırma, canlı olay çalışmaya başladıktan hemen sonra başlar. Daha fazla faturalama durdurmak için canlı olay kaynağı durdurma açıkça çağırmanız gerekir. Alternatif olarak, akış başlamaya hazır olduğunuzda olayı başlatın. 

    Daha fazla bilgi için [canlı olay durumları ve faturalandırma](live-event-states-billing.md).

## <a name="live-event-ingest-urls"></a>Canlı etkinlik URL'lerini alma

Canlı Etkinlik oluşturulduktan sonra, şirket içi gerçek zamanlı kodlayıcıya sağlayacağınız alma URL’lerini alabilirsiniz. Gerçek zamanlı kodlayıcı bu URL'leri canlı akış girişi için kullanır. Daha fazla bilgi için [şirket içi Canlı Kodlayıcıları önerilen](recommended-on-premises-live-encoders.md). 

Gösterim amaçlı olmayan URL'leri veya gösterim URL'lerini kullanabilirsiniz. 

> [!NOTE] 
> Alma URL'si Tahmine dayalı olarak bir "gösterim" modunu ayarlayın.

* Gösterim olmayan URL'si

    Media Services v3 varsayılan modunda olmayan gösterim URL'dir. Potansiyel olarak Canlı Etkinliği daha hızlı bir şekilde alabilirsiniz ancak alma URL'si yalnızca canlı etkinlik başlatıldığında bildirilir. Canlı Etkinliği durdurup yeniden başlattığınızda URL değişir. <br/>Gösterim amaçlı olmayan URL'ler, son kullanıcının canlı etkinliği mümkün olan en hızlı şekilde almak isteyen ve dinamik alma URL'leriyle uyumlu olan bir uygulama kullanarak akış yapmak istediğinde kullanışlıdır.
    
    Bir istemci uygulaması gereksinimi yok canlı olay önce bir alma URL'si önceden oluşturmak için oluşturulan, yalnızca otomatik olarak Canlı etkinlik için erişim belirteci oluşturmak için medya hizmetleri sağlar.
* Gösterim URL'si

    Yayın kodlayıcı donanımları kullanan ve Canlı Etkinliği başlattıktan sonra kodlayıcılarını yeniden yapılandırmak istemeyen büyük medya yayımcıları gösterim modunu tercih eder. Bu yayımcılar zaman içinde değişmeyen, tahmine dayalı bir alma URL'sini kullanmayı tercih eder.
    
    Bu mod belirtmek için ayarladığınız `vanityUrl` için `true` olacak şekilde oluşturulma zamanında (varsayılan değer `false`). Ayrıca kendi erişim belirteci geçmesi gerekir (`LiveEventInput.accessToken`) olacak şekilde oluşturulma zamanında. URL'deki rastgele bir belirteç önlemek için belirteci değeri belirtin. Erişim belirteci (ile veya kısa çizgi olmadan) geçerli bir GUID dizesi olması gerekir. Modu ayarlandıktan sonra güncelleştirilemiyor.

    Erişim belirteci, veri merkezinizde benzersiz olması gerekir. Gösterim URL'sini kullanmak üzere uygulamanız gerekiyorsa, her zaman erişim belirteciniz (yerine, herhangi bir mevcut GUID yeniden) için yeni bir GUID örneği oluşturmak için önerilir. 

    Gösterim URL etkinleştirme ve erişim belirteci için geçerli bir GUID ayarlamak için aşağıdaki API'leri kullanın (örneğin `"accessToken": "1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`).  
    
    |Dil|Gösterim URL'sini etkinleştir|Erişim belirteci ayarlama|
    |---|---|---|
    |REST|[properties.vanityUrl](https://docs.microsoft.com/rest/api/media/liveevents/create#liveevent)|[LiveEventInput.accessToken](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventinput)|
    |CLI|[--gösterim URL'si](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#az-ams-live-event-create)|[--erişim belirteci](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#optional-parameters)|
    |.NET|[LiveEvent.VanityUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent.vanityurl?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEvent_VanityUrl)|[LiveEventInput.AccessToken](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveeventinput.accesstoken?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEventInput_AccessToken)|
    
### <a name="live-ingest-url-naming-rules"></a>Canlı URL adlandırma kuralları alma

* Aşağıdaki *rastgele* dize, 128 bit bir onaltılık sayıdır (0-9 a-f arası 32 karakterden oluşur).
* *erişim belirtecinizi* -gösterim modunu kullanırken ayarladığınız geçerli GUID dize. Örneğin: `"1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`.
* *Akış adı* -belirli bir bağlantı için akış adını belirtir. Akış adı değeri, genellikle kullandığınız gerçek zamanlı Kodlayıcı tarafından eklenir. Bağlantı, örneğin açıklamak için herhangi bir ad kullanmak için gerçek zamanlı Kodlayıcı yapılandırabilirsiniz: "video1_audio1", "video2_audio1", "stream".

#### <a name="non-vanity-url"></a>Gösterim olmayan URL'si

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/live/<auto-generated access token>/<stream name>`<br/>
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/live/<auto-generated access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Kesintisiz Akış

`http://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>

#### <a name="vanity-url"></a>Gösterim URL'si

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/live/<your access token>/<stream name>`<br/>
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/live/<your access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Kesintisiz Akış

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>

## <a name="live-event-preview-url"></a>Canlı olay Önizleme URL'si

Canlı olay akışı katkı almaya başladıktan sonra Önizleme uç noktası, Önizleme ve daha fazla yayımlamadan önce Canlı akışı aldığını doğrulamak için kullanabilirsiniz. Önizleme akışı iyi olduğuna iade ettikten sonra canlı akış bir veya daha fazla (önceden oluşturulmuş) akış uç noktaları aracılığıyla teslim için kullanılabilir hale getirmek için canlı olay kullanabilirsiniz. Bunu yapmak için yeni oluşturduğunuz [Canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs) canlı olay. 

> [!IMPORTANT]
> Video önizleme URL'sine devam etmeden önce akan emin olun!

## <a name="live-event-long-running-operations"></a>Canlı olay uzun süre çalışan işlemleri

Ayrıntılar için bkz [uzun süre çalışan işlemler](media-services-apis-overview.md#long-running-operations)

## <a name="live-outputs"></a>Canlı Çıkışlar

Canlı olay giden akış oluşturduktan sonra oluşturarak akış olayını başlayabilirsiniz bir [varlık](https://docs.microsoft.com/rest/api/media/assets), [Canlı çıkış](https://docs.microsoft.com/rest/api/media/liveoutputs), ve [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators). Canlı bir çıkış akışı arşivler ve aracılığıyla izleyiciler kullanabilmesi [akış uç noktası](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

> [!NOTE]
> Oluşturma çıkışları başlangıç canlı ve silindiğinde sona erer. Canlı çıkış sildiğinizde, temel alınan varlık veya varlık içeriği silmekte olduğunuz değil. 

Arasındaki ilişkiyi bir **canlı olay** ve kendi **Canlı çıkışları** için geleneksel televizyon yayın, bir kanal (canlı olay) sabit video ve bir kayıt (Canlı akışını temsil eden gerçekleştirilmesine benzer Çıkış) için belirli bir zaman segment (örneğin, akşam haber 18:30:00 19:00:00 için) kapsama alınır. Televizyon bir Dijital Video Kaydedici DVR kullanarak kaydedebilirsiniz: Canlı olayları eşdeğer özelliği aracılığıyla yönetilen **archiveWindowLength** özelliği. Bu, DVR kapasitesini belirtir ve en az 3 dakika, en çok 25 saat için ayarlanabilir bir ISO 8601 zaman aralığı süresi (örneğin, PTHH:MM:SS) olur.

Bir bant Kaydedici, yakalar ve kayıt Media Services hesabınızda bir varlığa canlı akış gibi canlı çıkış nesnedir. Varlık kaynağı tarafından tanımlı kapsayıcı içine hesabınıza bağlı Azure depolama hesabına kaydedilen içeriği kalıcı. Canlı çıkış bazı giden canlı akış akışın ne kadar arşiv kaydı (örneğin, bulut DVR Kapasite) tutulur ve canlı akış izleme görüntüleyiciler olup olmadığını başlayabilirsiniz gibi özellikleri denetlemenize olanak verir. Döngüsel bir arşiv "penceresinin" arşividir diskte dinamik çıkışın archiveWindowLength özelliğinde belirtilen içerik miktarını yalnızca tutar. Bu pencere dışında kalan içerik depolama kapsayıcısından otomatik olarak atılır ve kurtarılabilir durumda değil. Birden çok çıktı (en fazla üç maksimum) canlı bir canlı olay farklı arşiv uzunlukları ve ayarlarla oluşturabilirsiniz.  

Dinamik çıkışın yayımladıysanız **varlık** kullanarak bir **akış Bulucu**, canlı olay (en fazla DVR pencere uzunluğunun) akış Bulucu'nın süre sonu veya silme kadar görüntülenebilir olmaya devam edecek hangisi önce gelirse.

Daha fazla bilgi için [kullanılarak bir bulut DVR](live-event-cloud-dvr.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)
