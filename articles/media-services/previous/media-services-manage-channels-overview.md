---
title: Azure Media Services'i kullanarak canlı akış genel bakış | Microsoft Docs
description: Bu konu Azure Media Services'i kullanarak canlı akış genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: fb63502e-914d-4c1f-853c-4a7831bb08e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: a9d0daaacb046df7943202775adc77bc912cce11
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61217583"
---
# <a name="overview-of-live-streaming-using-media-services"></a>Media Services'i kullanarak canlı akış genel bakış

> [!NOTE]
> Canlı kanallar 12 Mayıs 2018 tarihinden itibaren artık RTP/MPEG-2 aktarım akışı destek alma protokolü. Lütfen RTP/MPEG-2'den RTMP veya parçalanmış MP4'e geçiş (kesintisiz akış) alma protokolleri.

## <a name="overview"></a>Genel Bakış

Azure Media Services ile etkinliklerin canlı akış sunarken aşağıdaki bileşenler yaygın olarak kullanılır:

* Etkinliği yayınlamak için kullanılan bir kamera.
* Kameradan gelen sinyalleri bir canlı akış hizmetine gönderilen akışlara dönüştüren gerçek zamanlı bir video kodlayıcısı.

    İsteğe bağlı olarak, birden çok, gerçek zamanlı, zaman eşitlenmiş kodlayıcı. Yüksek kullanılabilirlik ve üstün kaliteli bir deneyim gerektiren bazı önemli etkinliklerde, veri kaybı olmadan sorunsuz yük devretme elde etmek için zaman eşitlemeli aktif-aktif yedekli kodlayıcılar kullanılması önerilir.
* Aşağıdakileri yapmanıza olanak sağlayan bir canlı akış hizmeti:

  * çeşitli canlı akış protokollerini (RTMP veya Kesintisiz Akış gibi) kullanarak canlı içerik alma,
  * (isteğe bağlı olarak) akışınızı, bit hızı uyarlamalı akışa kodlama,
  * canlı akışınızı önizleme,
  * alınan içeriği daha sonra akışla aktarmak üzere kaydedip depolama (İsteğe Bağlı Video),
  * içeriği yaygın akış protokolleri (örneğin MPEG DASH, Kesintisiz, HLS) aracılığıyla doğrudan müşterilerinize veya başkalarına dağıtım için bir Content Delivery Network’e (CDN) teslim etme.

**Microsoft Azure Media Services** (AMS) canlı akış içeriğinizi alma, kodlama, önizleme, depolama ve teslim etme olanağı sağlar.

İçeriğinizi müşterilere teslim ederken hedefiniz, farklı ağ koşulları altındaki çeşitli cihazlara yüksek kaliteli bir video sunmaktır. Bunu başarmak için gerçek zamanlı kodlayıcılar kullanarak akışınızı Çoklu bit hızlı (bit hızı Uyarlamalı) video akışına kodlayın kullanın.  Farklı cihazlarda akış yapmayı halletmek için Media Services [dinamik paketlemesini](media-services-dynamic-packaging-overview.md) kullanarak akışınızı dinamik olarak yeniden farklı protokollere paketleyin. Media Services teslim aşağıdaki hızı Uyarlamalı akış teknolojilerini destekler: HTTP canlı akış (HLS), kesintisiz akış, MPEG DASH.

Azure Media Services’de **Kanallar**, **Programlar** ve **Akış Uç Noktaları**; alma biçimlendirme, DVR, güvenlik, ölçeklenebilirlik ve yedeklilik dahil olmak üzere tüm canlı akış işlevlerini idare eder.

**Kanal**, canlı akış içeriğinin işleneceği bir işlem hattını temsil eder. Kanal aşağıdaki yollarla bir canlı girdi akışı alabilir:

* Şirket içi bir gerçek zamanlı kodlayıcı, çoklu bit hızına sahip **RTMP** veya **Kesintisiz Akışı** (parçalanmış MP4) **doğrudan geçiş** teslimi için yapılandırılmış Kanala gönderir. **Doğrudan geçiş** teslimi, alınan akışların herhangi başka bir işlemeye uğramadan **Kanallardan** geçmesidir. Çoklu bit hızlı kesintisiz akış çıktısı sağlayan şu gerçek zamanlı Kodlayıcıları kullanabilirsiniz: MediaExcel, Ateme, Imagine Communications, Envivio, Cisco ve Elemental. Şu gerçek zamanlı kodlayıcılar RTMP çıktısı: Adobe Flash Media Live Encoder (FMLE), Telestream Wirecast, Haivision, Teradek ve Tricaster kod dönüştürücüleri.  Gerçek zamanlı bir kodlayıcı, gerçek zamanlı kodlama için etkinleştirilmemiş bir kanala tek bit hızlı bir akış da gönderebilir, ancak bu işlem önerilmez. İstendiğinde, Media Services akışı müşterilere teslim eder.

  > [!NOTE]
  > Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
  > 
  > 
* Bir şirket içi Canlı Kodlayıcı, aşağıdaki biçimlerden birinde Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanala tek bit hızlı akış gönderir: RTMP veya kesintisiz akış (parçalanmış MP4). Şu gerçek zamanlı kodlayıcılar RTMP çıktısı ile bu tür kanallarla çalışmayı bilinmektedir: Telestream Wirecast, FMLE. Ardından Kanal, gelen tek bit hızlı akışın çoklu bit hızlı (uyarlamalı) bir video akışına gerçek zamanlı kodlanmasını gerçekleştirir. İstendiğinde, Media Services akışı müşterilere teslim eder.

Kanal oluşturduğunuzda Media Services 2.10 sürüm ile başlayarak, kanalın akışınız gerçek zamanlı kodlama gerçekleştirmek için istediğiniz olup olmadığını ve hangi yolla kanalınızı giriş akışını almak istediğiniz belirtebilirsiniz. İki seçeneğiniz vardır:

* **Hiçbiri** (doğrudan geçiş) – (doğrudan akışı) Çoklu bit hızında akışa çıkarır, bir şirket içi Canlı Kodlayıcı kullanmayı planlıyorsanız, bu değeri belirtin. Bu durumda, gelen akış çıkışı herhangi bir kodlama içermeyen geçtiğini. Bu kanal 2.10 sürüm yayınlanmadan önce davranıştır.  
* **Standart** – tek bit hızlı Canlı akışınızı Çoklu bit hızı akışına kodlama için Media Services kullanmayı planlıyorsanız, bu değeri seçin. Bu yöntem, sık erişilmeyen olayları hızla ölçek artırmaya yönelik daha ekonomiktir. Gerçek zamanlı kodlama için fatura bir etkisi yoktur ve bir canlı kodlama kanal "Çalışıyor" durumda bırakır fatura ücretler ödeyeceğinizi unutmayın unutmayın.  Ek saatlik ücretlerden kaçınmak için Canlı etkinlik akışı tamamlandıktan sonra hemen çalışan kanallarınızın durdurmanız önerilir.

## <a name="comparison-of-channel-types"></a>Kanal türlerinden karşılaştırması

Aşağıdaki tabloda Media Services'da desteklenen iki kanallı türlerini karşılaştırma için bir kılavuz sağlar

| Özellik | Doğrudan geçiş kanalı | Standart kanal |
| --- | --- | --- |
| Tekli bit hızı girişi Çoklu bit hızlarında buluta halinde kodlanır |Hayır |Evet |
| En yüksek çözünürlük, Katmanlar sayısı |1080p, 8 katmanları 60 + fps |720p, 6 katmanları 30 fps |
| Giriş protokolleri |RTMP, kesintisiz akış |RTMP, kesintisiz akış |
| Fiyat |Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesine tıklayın |Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) |
| Maksimum Çalıştırma süresi |7/24 |8 saat |
| Maskeleme görüntülerini ekleme desteği |Hayır |Evet |
| Sinyal ad desteği |Hayır |Evet |
| Doğrudan CEA 608/708 açıklamalı alt yazılar |Evet |Evet |
| Tekdüzen olmayan giriş GOPs desteği |Evet |Yok – giriş 2 sn GOPs giderilmiş olmalıdır |
| Değişken kare hızı girişi için destek |Evet |Yok – giriş kare hızı düzeltilmesi gerekir.<br/>Küçük farklılıklar, örneğin, yüksek bir hareket sahneler sırasında izin verilir. Ancak, kodlayıcı 10 Çerçeve/sn için bırakılamıyor. |
| Otomatik akışı kapatmaya kanal zaman giriş kayboluyor |Hayır |12 çalışan bir Program yok ise saat sonra |

## <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Şirket içi kodlayıcılardan çoklu bit hızlı canlı akış alan Kanallar ile çalışma (doğrudan geçiş)

Aşağıdaki diyagramda, AMS platformunun **doğrudan geçiş** iş akışında rol oynayan başlıca parçaları gösterilmektedir.

![Canlı iş akışı](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Daha fazla bilgi için bkz. [Şirket İçi Kodlayıcılardan Çoklu Bit Hızlı Canlı Akış Alan Kanallar ile Çalışma](media-services-live-streaming-with-onprem-encoders.md).

## <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Azure Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş Kanallar ile çalışma

Aşağıdaki diyagramda, AMS platformunun bir Kanalın, Media Services ile kodlama gerçekleştirmek için etkinleştirildiği Canlı Akış iş akışında rol oynayan başlıca parçaları gösterilmektedir.

![Canlı iş akışı](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

## <a name="description-of-a-channel-and-its-related-components"></a>Bir kanal ve ilgili bileşenlerini açıklaması

### <a name="channel"></a>Kanal

Medya Hizmetleri'nde [kanal](https://docs.microsoft.com/rest/api/media/operations/channel)s canlı akış içeriğinin işlemekten sorumlu. Bir kanalın giriş uç noktası sağlar (alma URL'si) için Canlı bir işlenmesinde ardından sağlayın. Kanal, Canlı giriş akışları Canlı işlenmesinde alır ve bir veya daha fazla Akış akış için kullanılabilir hale getirir. Kanalları da daha fazla işleme edip teslime geçmeden önce akışınızı onaylama için kullandığınız bir önizleme uç noktası (Önizleme URL'si) sağlar.

Kanıl oluşturduğunuzda alma URL'si ve önizleme URL'sini alabilirsiniz. Bu URL'ler almak için kanal başlatılmış durumda olması gerekmez. Kanal canlı bir işlenmesinde veri göndermeye başlamak hazır olduğunuzda, kanal başlatılması gerekir. Veri alma, Canlı işlenmesinde başladıktan sonra akışınızın önizlemesini.

Her bir Media Services hesabı, birden çok kanalda, birden çok programları ve birden çok akış içerebilir. Bant genişliği ve güvenlik gereksinimlerine bağlı olarak, bir veya daha fazla kanala StreamingEndpoint Hizmetleri ayrılabilir. Herhangi bir kanaldan herhangi StreamingEndpoint çekmeden.

Bir kanal oluşturulması, izin verilen IP adresleri aşağıdaki biçimlerden birinde belirtebilirsiniz: IPv4 adresi 4 sayılarla CIDR adres aralığı.

### <a name="program"></a>Program
A [Program](https://docs.microsoft.com/rest/api/media/operations/program) yayımlanması ve depolanmasını Canlı akıştaki segmentlerin denetlemenizi sağlar. Kanallar, Programları yönetir. Kanal ve Program arasındaki ilişki, kanalın sürekli bir içerik akışının bulunduğu ve programın bu kanalda zamanlanmış bir olayı kapsadığı geleneksel medyadaki ilişkiye benzer.
Saat ayarlayarak program için kaydedilen içeriği tutmak istediğinizi belirtebilirsiniz **ArchiveWindowLength** özelliği. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir.

ArchiveWindowLength ayrıca en fazla saat istemcileri geçmişe geçerli Canlı konumdan gidebilecekleri. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her program bir Varlık ile ilişkilidir. Programı yayımlamak için ilişkili varlığa yönelik bir Bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal eşzamanlı çalışan üçe kadar olayı destekler, böylece aynı gelen akışın birden fazla arşivini oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

## <a name="billing-implications"></a>Faturalandırma etkileri
Bir kanal API aracılığıyla durumuna geçişler "Çalışır" duruma geldiği faturalama başlar.  

Aşağıdaki tabloda, kanal durumlarının faturalandırma durumları API ve Azure Portalı'nda nasıl eşleneceğine gösterilmektedir. Durumları UX'i Portal ve API arasında biraz daha farklı olduğuna dikkat edin Faturalama, kanal "Çalışıyor" durumunda API'si aracılığıyla veya Azure portalında "Hazır" veya "Akış" durumunda olduğu sürece etkin olacaktır.

Daha fazla faturalama gelen kanal durdurmak için API aracılığıyla veya Azure portalında kanal'ı durdurmanız gerekir.
Kanallarınızı kanalıyla işiniz bittiğinde durdurmak için sorumlu olursunuz. Kanalı durdurun hatası sürekli faturalandırma uygulanmasına neden olur.

> [!NOTE]
> Standart kanallar ile çalışırken, AMS 12 saat sonra giriş akışı kaybolur ve çalışan program yok hala "Çalışıyor" durumunda olduğu herhangi bir kanal kesici otomatik olarak tamamlar. Ancak, yine de kanal "Çalışıyor" konumunda olduğu süre için faturalandırılırsınız.
>
>

### <a id="states"></a>Durumları ve bunların faturalandırma modu nasıl eşleneceğine kanal
Kanalın geçerli durumu. Olası değerler şunlardır:

* **Durduruldu**. (Autostart portalda seçildi. sürece) bu ilk kanalın oluşturulduktan sonraki durumudur Bu durumda hiçbir faturalandırma gerçekleşir. Bu durumda, Kanal özellikleri güncelleştirilebilir ama akışa izin verilmez.
* **Başlangıç**. Kanal başlatılıyor. Bu durumda hiçbir faturalandırma gerçekleşir. Bu durum süresince güncelleştirmelere veya akışa izin verilmez. Bir sorun oluşursa Kanal, Durduruldu durumuna döndürülür.
* **Çalışan**. Kanal canlı akışları işleyebilir. Artık kullanım faturalandırma. Daha fazla faturalama önlemek için kanal durdurmanız gerekir.
* **Durdurma**. Kanal durduruluyor. Bu geçici bir durumda hiçbir faturalandırma gerçekleşir. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.
* **Silme**. Kanal siliniyor. Bu geçici bir durumda hiçbir faturalandırma gerçekleşir. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.

Aşağıdaki tabloda, Kanal durumlarının faturalandırma modu ile nasıl eşleştiği gösterilir.

| Kanal durumu | Portal Arabirimi Göstergeleri | Faturalandırma nedir? |
| --- | --- | --- |
| Başlatılıyor |Başlatılıyor |Hayır (geçici durum) |
| Çalışıyor |Hazır (çalışan program yok)<br/>or<br/>Akış (en az bir program çalışıyor) |EVET |
| Durduruluyor |Durduruluyor |Hayır (geçici durum) |
| Durduruldu |Durduruldu |Hayır |

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Azure Media Services bölünmüş MP4 Canlı içe alma belirtimi](media-services-fmp4-live-ingest-overview.md)

[Gerçek zamanlı kodlama gerçekleştirmek ile Azure Media Services için etkinleştirilmiş kanallar ile çalışma](media-services-manage-live-encoder-enabled-channels.md)

[Şirket İçi Kodlayıcılardan Çoklu Bit Hızlı Canlı Akış Alan Kanallar ile Çalışma](media-services-live-streaming-with-onprem-encoders.md)

[Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md).  

[Medya Hizmetleri kavramları](media-services-concepts.md)
