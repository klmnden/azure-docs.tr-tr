---
title: "Azure Media Services’a genel bakış ve sık karşılaşılan senaryolar | Microsoft Belgeleri"
description: "Bu konu Azure Media Services’e genel bir bakış sağlar."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/12/2016
ms.author: juliako;anilmur
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 1a680f20bc9ceee8efd943cd71312372fe56a878


---
# <a name="azure-media-services-overview-and-common-scenarios"></a>Azure Media Services’e genel bakış ve sıklıkla karşılaşılan senaryolar
Microsoft Azure Media Services, geliştiricilerin ölçeklenebilir medya yönetimi ve teslimi uygulamaları oluşturmalarına olanak tanıyan genişletilebilir bir bulut tabanlı platformdur. Media Services, çeşitli istemcilere (TV, PC ve mobil cihazlar gibi) isteğe bağlı olarak veya canlı akış halinde teslim amacıyla video ve ses içeriklerini güvenli bir şekilde yüklemenizi, depolamanızı, kodlamanızı ve paketlemenizi sağlayan REST API’lerini temel alır.

Yalnızca Media Services’i kullanarak uçtan uca iş akışları oluşturabilirsiniz. Ayrıca, iş akışınızın bazı bölümleri için üçüncü taraf bileşenleri kullanmayı da tercih edebilirsiniz. Örneğin, bir üçüncü taraf kodlayıcısı kullanarak kodlayın. Daha sonra Media Services’i kullanarak yükleyin, koruyun, paketleyin ve teslim edin.

İçeriğinizi canlı akışla aktarmayı veya isteğe bağlı teslim etmeyi tercih edebilirsiniz. Bu konuda, içeriğinizin [canlı](media-services-overview.md#live_scenarios) veya [isteğe bağlı](media-services-overview.md#vod_scenarios) teslimiyle ilgili yaygın senaryolar gösterilmektedir. Konu, aynı zamanda ilgili diğer konulara bağlantılar içerir.

## <a name="sdks-and-tools"></a>SDK’lar ve araçlar
Media Services çözümleri oluşturmak için şunları kullanabilirsiniz:

* [Media Services REST API'si](https://msdn.microsoft.com/library/azure/hh973617.aspx)
* Kullanılabilir istemci SDK'larından biri:
* [.NET için Azure Media Services SDK](https://github.com/Azure/azure-sdk-for-media-services),
* [Java için Azure SDK](https://github.com/Azure/azure-sdk-for-java),
* [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
* [Node.js için Azure Media Services](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Bu, Microsoft dışı bir Node.js SDK sürümüdür. Bir topluluğun gözetimi altındadır ve şu anda AMS API'lerinin %100’ünü kapsamamaktadır).
* Mevcut araçlar:
* [Azure portal](https://portal.azure.com/)
* [Azure-Media-Services-Gezgini](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Gezgini (AMSE), Windows için bir Winforms/C# uygulamasıdır)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
AMS öğrenme yollarını burada görebilirsiniz:

* [AMS Canlı Akış İş Akışı](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [AMS İsteğe Bağlı Akış İş Akışı](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Önkoşullar
Azure Media Services’i kullanmaya başlamak için aşağıdakilerin bulunması gerekir:

1. Bir Azure hesabı. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com).
2. Bir Azure Media Services hesabı. Azure Media Services hesabı oluşturmak için Azure portal, .NET veya REST API’yi kullanın. Daha fazla bilgi için bkz. [Hesap Oluşturma](media-services-portal-create-account.md).
3. (İsteğe bağlı) Geliştirme ortamı ayarlayın. Geliştirme ortamınız için .NET veya REST API’yi seçin. Daha fazla bilgi için bkz. [Ortam Ayarlama](media-services-dotnet-how-to-use.md).

Ayrıca, [Bağlanma](media-services-dotnet-connect-programmatically.md) ile programlı olarak nasıl bağlanılacağını öğrenin.

1. (Önerilen) Bir veya daha fazla ölçek birimi ayırın. Üretim ortamındaki uygulamalar için bir veya daha fazla ölçek birimi ayırmak önerilir.   Daha fazla bilgi için bkz. [Akış uç noktalarını yönetme](media-services-portal-manage-streaming-endpoints.md).

## <a name="concepts-and-overview"></a>Kavramlar ve genel bakış
Azure Media Services kavramları hakkında bilgi edinmek için bkz. [Kavramlar](media-services-concepts.md).

Azure Media Services ana bileşenlerinin tümünü tanıtan bir dizi nasıl yapılır makalesi için bkz. [Azure Media Services Adım Adım öğreticileri](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Bu makale dizisi, kavramlara çok iyi bir genel bakış sunar ve AMSE aracını kullanarak AMS görevlerini gösterir. AMSE aracının bir Windows aracı olduğunu unutmayın. Bu araç, [.NET için AMS SDK](https://github.com/Azure/azure-sdk-for-media-services), [Java için Azure SDK](https://github.com/Azure/azure-sdk-for-java) veya [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php) ile programlama aracılığıyla elde edebileceğiniz görevlerin çoğunu destekler.

## <a name="a-idvodscenariosadelivering-media-on-demand-with-azure-media-services-common-scenarios-and-tasks"></a><a id="vod_scenarios"></a>Azure Media Services ile İsteğe Bağlı Medya Teslimi: yaygın senaryolar ve görevler
Bu bölümde, yaygın senaryolar açıklanmakta ve ilgili konulara bağlantılar sağlanmaktadır. Aşağıdaki diyagramda, Media Services platformunun isteğe bağlı içerik tesliminde rol oynayan başlıca parçaları gösterilmektedir. 

![VoD iş akışı](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

### <a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Depolama alanında içeriği koruma ve akan medyayı temiz olarak (şifrelenmemiş) teslim etme
1. Yüksek kaliteli bir ara dosyayı bir varlığa yükleyin.
   
    İçeriğinizi yükleme sırasında ve depolama alanında beklerken korumak için depolama şifrelemesi seçeneğini uygulamanız önerilir.
2. Uyarlamalı bit hızlı bir MP4 dosyaları grubuna kodlayın. 
   
    İçeriğinizi beklerken korumak için çıktı varlığına depolama şifrelemesi seçeneğini uygulamanız önerilir.
3. Varlık teslim ilkesini (dinamik paketleme tarafından kullanılır) yapılandırın. 
   
    Varlığınıza depolama şifrelemesi uygulanmışsa varlık teslim ilkesini yapılandırmanız **gerekir**. 
4. Bir OnDemand bulucu oluşturarak varlığı yayımlayın.
   
    İçerik akışını gerçekleştirmek istediğiniz akış uç noktasında akışa ayrılan en az bir birim olduğundan emin olun.
5. Yayımlanan içeriği akışla aktarın.

### <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Depolama alanında içeriği koruma, dinamik olarak şifrelenmiş akan medya teslim etme
Dinamik şifreleme kullanabilmek için, ilk olarak kendisinden şifrelenmiş içerik akışı gerçekleştirmek istediğiniz akış uç noktasında akışa ayrılan en az bir birim almanız gerekir.

1. Yüksek kaliteli bir ara dosyayı bir varlığa yükleyin. Varlığa depolama şifrelemesi seçeneğini uygulayın.
2. Uyarlamalı bit hızlı bir MP4 dosyaları grubuna kodlayın. Çıktı varlığına depolama şifrelemesi seçeneğini uygulayın.
3. Kayıttan yürütme sırasında dinamik olarak şifrelenmesini istediğiniz varlık için şifreleme içerik anahtarı oluşturun.
4. İçerik anahtarı yetkilendirme ilkesini yapılandırın.
5. Varlık teslim ilkesini (dinamik paketleme ve dinamik şifreleme tarafından kullanılır) yapılandırın.
6. Bir OnDemand bulucu oluşturarak varlığı yayımlayın.
7. Yayımlanan içeriği akışla aktarın. 

### <a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Medya Analizi kullanarak videolarınızdan eyleme dönüştürülebilir öngörüler türetme
Medya Analizi, kuruluş ve işletmelerin video dosyalarından eyleme dönüştürülebilir öngörüler türetmesini kolaylaştıran bir grup konuşma ve görme bileşenidir. Daha fazla bilgi için bkz. [Azure Media Services Analizi’ne Genel Bakış](media-services-analytics-overview.md).

1. Yüksek kaliteli bir ara dosyayı bir varlığa yükleyin.
2. Aşağıdaki Medya Analizi hizmetlerinden birini kullanarak videolarınızı işleyin:
   
   * **Dizin Oluşturucu** – [Azure Media Indexer 2 ile video işleme](media-services-process-content-with-indexer2.md)
   * **Hyperlapse** – [Azure Medya Hyperlapse ile Medya Dosyalarını Aşırı Zaman Atlamalı Hale Getirme](media-services-hyperlapse-content.md)
   * **Hareket algılama** – [Azure Medya Analizi için Hareket Algılama](media-services-motion-detection.md).
   * **Yüz algılama ve Yüzdeki duygular** – [Azure Medya Analizi için Yüz ve Duygu Algılama](media-services-face-and-emotion-detection.md).
   * **Video özetleme** – [Azure Medya Video Küçük Resimleri’ni Kullanarak Bir Video Özeti Oluşturma](media-services-video-summarization.md)
3. Medya Analizi medya işlemcileri MP4 veya JSON dosyaları üretir. Medya işlemcisi bir MP4 dosyası oluşturduysa dosyayı aşamalı olarak indirebilirsiniz. Medya işlemcisi bir JSON dosyası oluşturduysa dosyayı Azure blob depolamadan indirebilirsiniz. 

### <a name="deliver-progressive-download"></a>Aşamalı indirme teslimi
1. Yüksek kaliteli bir ara dosyayı bir varlığa yükleyin.
2. Tek bir MP4 dosyasına kodlayın.
3. Bir OnDemand veya SAS bulucu oluşturarak varlığı yayımlayın.
   
    OnDemand bulucu kullanıyorsanız, kendisinden aşamalı olarak içerik indirmeyi planladığınız akış uç noktasında akışa ayrılan en az bir birim bulunduğundan emin olun.
   
    SAS Bulucu kullanıyorsanız içerik, Azure blob depolama alanından indirilir. Bu durumda, akışa ayrılan birime gerekli değildir.
4. Aşamalı olarak içerik indirin.

## <a name="a-idlivescenariosadelivering-live-streaming-events-with-azure-media-services"></a><a id="live_scenarios"></a>Azure Media Services ile Etkinliklerin Canlı Akış Halinde Teslimi
Canlı Akış ile çalışırken aşağıdaki bileşenler yaygın olarak kullanılır:

* Etkinliği yayınlamak için kullanılan bir kamera.
* Kameradan gelen sinyalleri bir canlı akış hizmetine gönderilen akışlara dönüştüren gerçek zamanlı bir video kodlayıcısı.

İsteğe bağlı olarak, birden çok, gerçek zamanlı, zaman eşitlenmiş kodlayıcı. Yüksek kullanılabilirlik ve üstün kaliteli bir deneyim gerektiren bazı önemli etkinliklerde, veri kaybı olmadan sorunsuz yük devretme elde etmek için zaman eşitlemeli aktif-aktif yedekli kodlayıcılar kullanılması önerilir.

* Aşağıdakileri yapmanıza olanak sağlayan bir canlı akış hizmeti:
* çeşitli canlı akış protokollerini (RTMP veya Kesintisiz Akış gibi) kullanarak canlı içerik alma,
* (isteğe bağlı olarak) akışınızı, bit hızı uyarlamalı akışa kodlama,
* canlı akışınızı önizleme,
* alınan içeriği daha sonra akışla aktarmak üzere kaydedip depolama (İsteğe Bağlı Video),
* içeriği yaygın akış protokolleri (örneğin MPEG DASH, Kesintisiz, HLS, HDS) aracılığıyla doğrudan müşterilerinize veya başkalarına dağıtım için bir İçerik Teslim Ağına (CDN) teslim etme.

**Microsoft Azure Media Services** (AMS) canlı akış içeriğinizi alma, kodlama, önizleme, depolama ve teslim etme olanağı sağlar.

İçeriğinizi müşterilere teslim ederken hedefiniz, farklı ağ koşulları altındaki çeşitli cihazlara yüksek kaliteli bir video sunmaktır. Kaliteyi ve ağ koşullarını halletmek için gerçek zamanlı kodlayıcılar kullanarak akışınızı çoklu bit hızlı (bit hızı uyarlamalı) video akışına kodlayın.  Farklı cihazlarda akış yapmayı halletmek için Media Services [dinamik paketlemesini](media-services-dynamic-packaging-overview.md) kullanarak akışınızı dinamik olarak yeniden farklı protokollere paketleyin. Media Services şu bit hızı uyarlamalı akış teknolojilerini destekler: HTTP Canlı Akışı (HLS), Kesintisiz Akış, MPEG DASH ve HDS (yalnızca Adobe PrimeTime/Erişim lisans sahipleri için).

Azure Media Services’de **Kanallar**, **Programlar** ve **Akış Uç Noktaları**; alma biçimlendirme, DVR, güvenlik, ölçeklenebilirlik ve yedeklilik dahil olmak üzere tüm canlı akış işlevlerini idare eder.

**Kanal**, canlı akış içeriğinin işleneceği bir işlem hattını temsil eder. Kanal aşağıdaki yollarla bir canlı girdi akışı alabilir:

* Şirket içi bir gerçek zamanlı kodlayıcı, çoklu bit hızına sahip **RTMP** veya **Kesintisiz Akışı** (parçalanmış MP4) **doğrudan geçiş** teslimi için yapılandırılmış Kanala gönderir. **Doğrudan geçiş** teslimi, alınan akışların herhangi başka bir işlemeye uğramadan **Kanallardan** geçmesidir. Çoklu bit hızlı Kesintisiz Akış çıktısı sağlayan şu gerçek zamanlı kodlayıcıları kullanabilirsiniz: Elemental, Envivio, Cisco.  Şu gerçek zamanlı kodlayıcılar RTMP çıktısı sağlar: Adobe Flash Live, Telestream Wirecast ve Tricaster kod dönüştürücüleri.  Gerçek zamanlı bir kodlayıcı, gerçek zamanlı kodlama için etkinleştirilmemiş bir kanala tek bit hızlı bir akış da gönderebilir, ancak bu işlem önerilmez. İstendiğinde, Media Services akışı müşterilere teslim eder.

> [!NOTE]
> Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
> 
> 

* Şirket içi gerçek zamanlı bir kodlayıcı, Media Services ile şu biçimlerden birinde gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş Kanala tek bit hızlı bir akış gönderir: RTP (MPEG-TS), RTMP veya Kesintisiz Akış (Parçalanmış MP4). Ardından Kanal, gelen tek bit hızlı akışın çoklu bit hızlı (uyarlamalı) bir video akışına gerçek zamanlı kodlanmasını gerçekleştirir. İstendiğinde, Media Services akışı müşterilere teslim eder.

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Şirket içi kodlayıcılardan çoklu bit hızlı canlı akış alan Kanallar ile çalışma (doğrudan geçiş)
Aşağıdaki diyagramda, AMS platformunun **doğrudan geçiş** iş akışında rol oynayan başlıca parçaları gösterilmektedir.

![Canlı iş akışı][live-overview2]

Daha fazla bilgi için bkz. [Şirket İçi Kodlayıcılardan Çoklu Bit Hızlı Canlı Akış Alan Kanallar ile Çalışma](media-services-live-streaming-with-onprem-encoders.md).

### <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Azure Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş Kanallar ile çalışma
Aşağıdaki diyagramda, AMS platformunun bir Kanalın, Media Services ile kodlama gerçekleştirmek için etkinleştirildiği Canlı Akış iş akışında rol oynayan başlıca parçaları gösterilmektedir.

![Canlı iş akışı][live-overview1]

Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

## <a name="consuming-content"></a>İçerik kullanma
Azure Media Services, şunlar dahil olmak üzere çoğu platform için zengin ve dinamik istemci oynatıcı uygulamaları oluştururken ihtiyacınız olan araçları sağlar: iOS Cihazları, Android Cihazları, Windows, Windows Phone, Xbox ve Alıcı kutuları. Aşağıdaki konu, Media Services’den akan medyayı kullanabilecek kendi istemci uygulamalarınızı geliştirmek için yararlanabileceğiniz SDK'lar ve Oynatıcı Çerçevelerine bağlantılar sağlar.

[Video Oynatıcı Uygulamaları Geliştirme](media-services-develop-video-players.md)

## <a name="enabling-azure-cdn"></a>Azure CDN'yi etkinleştirme
Media Services, Azure CDN ile tümleştirmeyi destekler. Azure CDN'yi etkinleştirme hakkında daha fazla bilgi için bkz. [Media Services Hesabında Akış Uç Noktalarını Yönetme](media-services-portal-manage-streaming-endpoints.md).

## <a name="scaling-a-media-services-account"></a>Media Services hesabını ölçeklendirme
Hesabınıza sağlanmasını istediğiniz **Akışa Ayrılan Birim** ve **Kodlamaya Ayrılan Birim** sayısını belirterek **Media Services**’i ölçeklendirebilirsiniz.

Media Services hesabınızı, depolama hesapları ekleyerek de ölçeklendirebilirsiniz. Her depolama hesabı 500 TB ile sınırlıdır. Depolama alanınızı varsayılan sınırlamaların ötesine genişletmek için, tek bir Media Services hesabına birden çok depolama hesabı eklemeyi seçebilirsiniz.

[Bu](media-services-portal-scale-streaming-endpoints.md) konuda, ilgili konulara bağlantılar sağlanmaktadır.

## <a name="support"></a>Destek
[Azure Desteği](https://azure.microsoft.com/support/options/), Media Services de dahil olmak üzere Azure için destek seçenekleri sağlar.

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="service-level-agreement-sla"></a>Hizmet Düzeyi Sözleşmesi (SLA)
* Media Services Kodlama için, REST API işlemlerinin %99,9 kullanılabilirliğini garanti ediyoruz.
* Akış için, en az bir Akış Birimi satın alındığında mevcut medya içeriğinin %99,9 kullanılabilirliği garantisi ile isteklere başarıyla hizmet vereceğiz.
* Canlı Kanallar için, çalışan Kanalların en az %99,9 oranda dış bağlantıya sahip olacağını garanti ediyoruz.
* Content Protection için, önemli istekleri en az %99,9 oranda başarıyla karşılayacağımızı garanti ediyoruz.
* Dizin Oluşturucu için, bir Kodlamaya Ayrılan Birim ile işlenen Dizin Oluşturucu Görevi isteklerine %99,9 oranda başarıyla hizmet vereceğiz.

Daha fazla bilgi için bkz. [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png




<!--HONumber=Dec16_HO1-->


