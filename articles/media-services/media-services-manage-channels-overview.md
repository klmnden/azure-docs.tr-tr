---
title: "Azure Media Services'i kullanarak canlı akış bakış | Microsoft Docs"
description: "Bu konu Azure Media Services kullanarak canlı akış genel bir bakış sağlar."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: fb63502e-914d-4c1f-853c-4a7831bb08e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 6f500f25129470a679c75cae6cd1abc9d71b72a7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="overview-of-live-streaming-using-azure-media-services"></a>Azure Media Services'i kullanarak canlı akış genel bakış
## <a name="overview"></a>Genel Bakış
Azure Media Services ile etkinliklerin canlı akış teslim edilirken aşağıdaki bileşenler yaygın olarak kullanılır:

* Etkinliği yayınlamak için kullanılan bir kamera.
* Kameradan gelen sinyalleri bir canlı akış hizmetine gönderilen akışlara dönüştüren gerçek zamanlı bir video kodlayıcısı.

    İsteğe bağlı olarak, birden çok, gerçek zamanlı, zaman eşitlenmiş kodlayıcı. Yüksek kullanılabilirlik ve üstün kaliteli bir deneyim gerektiren bazı önemli etkinliklerde, veri kaybı olmadan sorunsuz yük devretme elde etmek için zaman eşitlemeli aktif-aktif yedekli kodlayıcılar kullanılması önerilir.
* Aşağıdakileri yapmanıza olanak sağlayan bir canlı akış hizmeti:

  * çeşitli canlı akış protokollerini (RTMP veya Kesintisiz Akış gibi) kullanarak canlı içerik alma,
  * (isteğe bağlı olarak) akışınızı, bit hızı uyarlamalı akışa kodlama,
  * canlı akışınızı önizleme,
  * alınan içeriği daha sonra akışla aktarmak üzere kaydedip depolama (İsteğe Bağlı Video),
  * içeriği yaygın akış protokolleri (örneğin MPEG DASH, Kesintisiz, HLS) aracılığıyla doğrudan müşterilerinize veya başkalarına dağıtım için bir İçerik Teslim Ağına (CDN) teslim etme.

**Microsoft Azure Media Services** (AMS) canlı akış içeriğinizi alma, kodlama, önizleme, depolama ve teslim etme olanağı sağlar.

İçeriğinizi müşterilere teslim ederken hedefiniz, farklı ağ koşulları altındaki çeşitli cihazlara yüksek kaliteli bir video sunmaktır. Bunu başarmak için gerçek zamanlı kodlayıcılar kullanarak akışınızı Çoklu bit hızlı (bit hızı Uyarlamalı) video akışına kodlayın kullanın.  Farklı cihazlarda akış yapmayı halletmek için Media Services [dinamik paketlemesini](media-services-dynamic-packaging-overview.md) kullanarak akışınızı dinamik olarak yeniden farklı protokollere paketleyin. Media Services şu bit hızı uyarlamalı akış teknolojilerinin dağıtımını destekler: HTTP Canlı Akışı (HLS), Kesintisiz Akış, MPEG DASH.

Azure Media Services’de **Kanallar**, **Programlar** ve **Akış Uç Noktaları**; alma biçimlendirme, DVR, güvenlik, ölçeklenebilirlik ve yedeklilik dahil olmak üzere tüm canlı akış işlevlerini idare eder.

**Kanal**, canlı akış içeriğinin işleneceği bir işlem hattını temsil eder. Kanal aşağıdaki yollarla bir canlı girdi akışı alabilir:

* Şirket içi bir gerçek zamanlı kodlayıcı, çoklu bit hızına sahip **RTMP** veya **Kesintisiz Akışı** (parçalanmış MP4) **doğrudan geçiş** teslimi için yapılandırılmış Kanala gönderir. **Doğrudan geçiş** teslimi, alınan akışların herhangi başka bir işlemeye uğramadan **Kanallardan** geçmesidir. Çoklu bit hızlı kesintisiz akış çıkışı aşağıdaki gerçek zamanlı Kodlayıcıları kullanabilirsiniz: MediaExcel, Ateme, düşünün iletişimleri, Envivio, Cisco ve Elemental. Şu gerçek zamanlı kodlayıcılar RTMP çıkışı: Adobe Flash medya Canlı Kodlayıcı (FMLE), Telestream Wirecast, Haivision, Teradek ve Tricaster kod dönüştürücüleri.  Gerçek zamanlı bir kodlayıcı, gerçek zamanlı kodlama için etkinleştirilmemiş bir kanala tek bit hızlı bir akış da gönderebilir, ancak bu işlem önerilmez. İstendiğinde, Media Services akışı müşterilere teslim eder.

  > [!NOTE]
  > Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
  > 
  > 
* Bir şirket içi gerçek zamanlı Kodlayıcı aşağıdaki biçimlerden birinde Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanala tek bit hızlı akış gönderir: RTMP veya kesintisiz akış (parçalanmış MP4). Azure veri merkezine adanmış bir bağlantıya sahip sağlanan RTP (MPEG-TS) de desteklenir. Şu gerçek zamanlı kodlayıcılar RTMP çıkışı ile bu tür kanallar ile çalışma bilinen: Telestream Wirecast, FMLE. Ardından Kanal, gelen tek bit hızlı akışın çoklu bit hızlı (uyarlamalı) bir video akışına gerçek zamanlı kodlanmasını gerçekleştirir. İstendiğinde, Media Services akışı müşterilere teslim eder.

Bir kanal oluşturduğunuzda Media Services 2.10 sürümünden başlayarak, hangi yolla, kanalınızı Giriş akışı almaya ve desteklemediğini kanalın akışınızı gerçek zamanlı kodlama gerçekleştirmek için istiyorsanız belirtebilirsiniz. İki seçeneğiniz vardır:

* **Hiçbiri** (doğrudan geçiş) –, Çoklu bit hızında akışa (geçiş akışı) çıkış bir şirket içi gerçek zamanlı Kodlayıcı kullanmayı planlıyorsanız, bu değeri belirtin. Bu durumda, gelen akış çıkışı herhangi kodlamadan geçirilecek. Bir kanal 2.10 yayın önce davranış budur.  
* **Standart** – tek bit hızlı Canlı akışınızı Çoklu bit hızı akışına kodlamak için Media Services kullanmayı planlıyorsanız, bu değer seçin. Bu yöntem için sık olayları hızla ölçeklendirmeyi için daha ekonomiktir. Gerçek zamanlı kodlama için fatura bir etkisi yoktur ve canlı bir kodlama kanal "Çalışır" durumda bırakır fatura ücret uygulanabilir unutmayın unutmayın.  Fazladan saatlik ücretlerden kaçınmak için canlı akış olayı tamamlandıktan sonra hemen çalışan kanalların durdurmanız önerilir.

## <a name="comparison-of-channel-types"></a>Kanal türleri karşılaştırması
Aşağıdaki tabloda Media Services ile desteklenen iki kanallı türleri karşılaştırma için bir kılavuz sağlar

| Özellik | Doğrudan geçiş kanalı | Standart kanal |
| --- | --- | --- |
| Tek bit hızlı giriş bulutta Çoklu bit içine kodlanmış |Hayır |Evet |
| Maksimum çözünürlük, Katmanlar sayısı |1080p, 8 Katmanlar 60 + fps |720p, 6 Katmanlar 30 fps |
| Giriş protokolleri |RTMP, kesintisiz akış |RTMP, kesintisiz akış ve RTP |
| Fiyat |Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesini tıklatın |Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) |
| En fazla çalışma süresi |7 x 24 |8 saat |
| Maskeleme görüntülerini ekleme desteği |Hayır |Evet |
| Ad sinyal desteği |Hayır |Evet |
| Doğrudan CEA 608/708 resim yazıları |Evet |Evet |
| Akış katkı içinde kısa takılması kurtarma olanağı |Evet |Hayır (kanal slating giriş verisi olmadan 6 + saniye sonra başlayacak) |
| Tekdüzen olmayan giriş GOPs desteği |Evet |Giriş 2 sn GOPs Hayır – düzeltilmesi gerekir |
| Değişken kare hızı giriş desteği |Evet |Hayır – giriş kare hızı düzeltilmesi gerekir.<br/>İkincil Çeşitlemeler, örneğin, yüksek hareket planda sırasında izin verilir. Ancak Kodlayıcı 10 Çerçeve/sn için bırakılamıyor. |
| Otomatik-akış kesici zaman Giriş kanalı kaybolur |Hayır |12 çalışan bir Program yok ise saat sonra |

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
Media Services [kanal](https://docs.microsoft.com/rest/api/media/operations/channel)s canlı akış içeriğinin işlemekten sorumlu. Bir giriş uç noktası bir kanal sağlar (URL alma) için dinamik bir dönüştürücü ardından sağlayın. Kanal, Canlı giriş akışları Canlı dönüştürücü alır ve bir veya daha fazla Akış akış için kullanılabilir hale getirir. Kanal Önizleme ve başka bir işleme ve teslim önce akışınızı doğrulamak için kullanacağınız bir önizleme uç noktası (Önizleme URL) de sağlar.

Kanal oluşturduğunuzda alma URL'si ve önizleme URL'sini alabilirsiniz. Bu URL'leri almak için kanal başlatılmış durumda olması gerekmez. Veri canlı bir dönüştürücü kanal dağıtmaya başlamak için hazır olduğunuzda, kanal başlatılması gerekir. Veri alma Canlı dönüştürücü başladıktan sonra akışınızın önizlemesini.

Her Media Services hesabı birden fazla kanal, birden çok program ve birden çok akış içerebilir. Bant genişliği ve güvenlik gereksinimlerine bağlı olarak, bir veya daha fazla kanala StreamingEndpoint Hizmetleri ayrılabilir. Tüm StreamingEndpoint herhangi bir kanaldan çeker.

### <a name="program"></a>Program
A [Program](https://docs.microsoft.com/rest/api/media/operations/program) yayımlama ve canlı akıştaki kesimleri depolanmasını denetlemenizi sağlar. Kanallar, Programları yönetir. Kanal ve Program arasındaki ilişki, kanalın sürekli bir içerik akışının bulunduğu ve programın bu kanalda zamanlanmış bir olayı kapsadığı geleneksel medyadaki ilişkiye benzer.
Program için kaydedilen içeriği ayarlayarak korumak istediğiniz saat sayısını belirtebilirsiniz **ArchiveWindowLength** özelliği. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir.

ArchiveWindowLength de maksimum zaman istemcilerine geçmişe geçerli Canlı konumdan gidebilecekleri. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her program bir Varlık ile ilişkilidir. Programı yayımlamak için ilişkili varlığa yönelik bir Bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal eşzamanlı çalışan üçe kadar olayı destekler, böylece aynı gelen akışın birden fazla arşivini oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

## <a name="billing-implications"></a>Faturalama etkileri
Kanal durumu geçişleri "Çalışır" için API üzerinden olduğunda hemen faturalama başlar.  

Aşağıdaki tabloda, API ve Azure portalındaki Faturalama durumları nasıl kanal durumları Eşle gösterir. Durumları API ve Portal UX arasında biraz farklı olduğuna dikkat edin API aracılığıyla "Çalışır" durumda ya da Azure Portalı'ndaki "Hazır" veya "Akış" durumdaki bir kanaldır hemen faturalama etkin olacaktır.

Daha fazla faturalama gelen kanal durdurmak için Azure portalında veya API aracılığıyla kanalı durdurun sahip.
Kanal ile işiniz bittiğinde, kanalları durdurmak için sorumlu. Kanalı durdurun hatası içinde sürekli faturalama neden olur.

> [!NOTE]
> Standart kanallar ile çalışırken, AMS kesici hala "Çalışır" durumda 12 saat Giriş akışı kaybedilir ve çalışan program yok sonra herhangi bir kanal otomatik. Ancak, siz hala kanal "Çalışır" durumda ne zaman için Fatura edilecek.
>
>

### <a id="states"></a>Kanal durumları ve bunların fatura moduna nasıl eşleneceğine
Kanalın geçerli durumu. Olası değerler şunlardır:

* **Durdurulmuş**. (Autostart portalda seçilmedi. sürece) bu ilk kanal oluşturulduktan sonra bir durumda Bu durumda hiçbir faturalama oluşur. Bu durumda, Kanal özellikleri güncelleştirilebilir ama akışa izin verilmez.
* **Başlangıç**. Kanal başlatılıyor. Bu durumda hiçbir faturalama oluşur. Bu durum süresince güncelleştirmelere veya akışa izin verilmez. Bir sorun oluşursa Kanal, Durduruldu durumuna döndürülür.
* **Çalışan**. Kanal canlı akışları işleyebilir. Şimdi kullanım faturalama. Daha fazla faturalama önlemek için kanal durdurmanız gerekir.
* **Durdurma**. Kanal durduruluyor. Hiçbir faturalama bu geçici durumunda oluşur. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.
* **Silme**. Kanal siliniyor. Hiçbir faturalama bu geçici durumunda oluşur. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.

Aşağıdaki tabloda, Kanal durumlarının faturalandırma modu ile nasıl eşleştiği gösterilir.

| Kanal durumu | Portal Arabirimi Göstergeleri | Faturalama mi? |
| --- | --- | --- |
| Başlangıç |Başlangıç |Hayır (geçici durum) |
| Çalışıyor |Hazır (çalışan program yok)<br/>veya<br/>Akış (en az bir program çalışıyor) |EVET |
| Durduruluyor |Durduruluyor |Hayır (geçici durum) |
| Durduruldu |Durduruldu |Hayır |

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Azure Media Services parçalanmış MP4 Canlı belirtimi alma](media-services-fmp4-live-ingest-overview.md)

[Gerçek zamanlı kodlama gerçekleştirmek Azure Media Services ile için etkinleştirilmiş kanallar ile çalışma](media-services-manage-live-encoder-enabled-channels.md)

[Şirket İçi Kodlayıcılardan Çoklu Bit Hızlı Canlı Akış Alan Kanallar ile Çalışma](media-services-live-streaming-with-onprem-encoders.md)

[Kotalar ve kısıtlamaları](media-services-quotas-and-limitations.md).  

[Media Services kavramları](media-services-concepts.md)
