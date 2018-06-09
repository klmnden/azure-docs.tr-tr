---
title: Azure Media Services'i kullanarak canlı akış bakış | Microsoft Docs
description: Bu konu genel bir bakış Canlı Azure Media Services v3 kullanarak akış olanağı sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/06/2018
ms.author: juliako
ms.openlocfilehash: b8c9375d8ad915200cbc8b2e1a62979fd1b7d179
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238303"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Canlı akış ile Azure Media Services v3

Azure Media Services ile etkinliklerin canlı akış teslim edilirken aşağıdaki bileşenler yaygın olarak kullanılır:

* Etkinliği yayınlamak için kullanılan bir kamera.
* Media Services gönderilen akışlara kamera (veya dizüstü bilgisayar gibi başka bir aygıt) sinyalleri dönüştürür canlı bir video Kodlayıcısı canlı akış hizmeti. Sinyaller SCTE-35 ve Ad yardımlar reklam de içerebilir. 
* Media Services canlı akış hizmeti, alma, Önizleme, paketi, kayıt, şifrelemek ve müşterilerinize veya daha fazla dağıtım için bir CDN içeriği yayını olanak sağlar.

Bu makalede, ayrıntılı bir genel bakış sağlar ve Media Services ile canlı akış söz konusu ana bileşen diyagramları içerir.

## <a name="overview-of-main-components"></a>Ana bileşenler genel bakış

Media Services [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) canlı akış içeriğinin işlemekten sorumludur. Bir giriş uç noktası bir LiveEvent sağlar (URL alma) sonra bir şirket içi gerçek zamanlı Kodlayıcı sağlayın. LiveEvent Canlı giriş akışları gerçek zamanlı Kodlayıcı, RTMP veya kesintisiz akış biçiminde alır ve bir veya daha fazla akış kullanımına [akış](https://docs.microsoft.com/rest/api/media/streamingendpoints). A [LiveOutput](https://docs.microsoft.com/en-us/rest/api/media/liveoutputs) yayımlama, kaydetme ve canlı akış DVR penceresi ayarlarını denetlemenize olanak verir. LiveEvent Önizleme ve başka bir işleme ve teslim önce akışınızı doğrulamak için kullanacağınız bir önizleme uç noktası (Önizleme URL) de sağlar. 

Medya hizmetleri sağlayan **dinamik paketleme**, olanak sağlayan Önizleme ve MPEG DASH, HLS, kesintisiz akış içeriğinizi yayınlamak, bu akış biçimlerine el ile yeniden paketleyin zorunda kalmadan akış biçimlerine. Herhangi HLS, DASH veya kesintisiz uyumlu oynatıcıları oynatabilirsiniz. Aynı zamanda [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) akışınızı test etmek için.

Media Services, dinamik olarak şifrelenmiş içeriğinizi teslim etmek sağlar (**dinamik şifreleme**) Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden herhangi birini: Microsoft PlayReady Google Widevine ve Apple FairPlay. Media Services de AES anahtarları ve DRM teslim etmek için bir hizmet sunar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere.

İsterseniz, ayrıca uygulayabilirsiniz **dinamik filtreleme**, izler, biçimleri, oyunculara gönderilen bit sayısını kontrol etmek için kullanılabilir. Media Services, reklam eklemeyi de destekler.


## <a name="liveevent-types"></a>LiveEvent türleri

A [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: Gerçek zamanlı kodlama ve doğrudan. 

### <a name="live-encoding-with-media-services"></a>Media Services ile kodlama Canlı

![Kodlama Canlı](./media/live-streaming/live-encoding.png)

Bir şirket içi gerçek zamanlı Kodlayıcı aşağıdaki protokollerden birini Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş LiveEvent tek bit hızlı akış gönderir: RTMP veya kesintisiz akış (parçalanmış MP4). LiveEvent ardından gelen gerçek zamanlı kodlama gerçekleştiren tek bit hızlı akışın Çoklu bit hızlı (Uyarlamalı) video akışına. İstendiğinde, Media Services akışı müşterilere teslim eder.

Bu tür LiveEvent oluştururken belirtin **temel** (LiveEventEncodingType.Basic).

### <a name="pass-through"></a>Geçiş

![Doğrudan geçiş](./media/live-streaming/pass-through.png)

Doğrudan uzun süre çalışan Canlı akışlar veya 7 x 24 doğrusal gerçek zamanlı bir şirket içi gerçek zamanlı Kodlayıcı kullanarak kodlama için optimize edilmiştir. Şirket içi Kodlayıcı Çoklu bit hızlı gönderir **RTMP** veya **kesintisiz akış** (parçalanmış MP4) için yapılandırılmış LiveEvent için **doğrudan** teslim. **Doğrudan** teslim alınan akışların geçirir olduğunda **LiveEvent**herhangi başka bir işleme olmadan s. 

Doğrudan LiveEvents destekleyebilmesi yukarı 4 K bir çözüm ve HEVC geçirin aracılığıyla kesintisiz akış ile kullanıldığında alma protokolü. 

Bu tür LiveEvent oluştururken belirtin **hiçbiri** (LiveEventEncodingType.None).

> [!NOTE]
> Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
> 

## <a name="liveevent-types-comparison"></a>LiveEvent türleri karşılaştırma 

Aşağıdaki tabloda iki LiveEvent türlerinin özellikleri karşılaştırılır.

| Özellik | Doğrudan LiveEvent | Temel LiveEvent |
| --- | --- | --- |
| Tek bit hızlı giriş bulutta Çoklu bit içine kodlanmış |Hayır |Evet |
| Maksimum çözünürlük, Katmanlar sayısı |4Kp30  |720p, 6 Katmanlar 30 fps |
| Giriş protokolleri |RTMP, kesintisiz akış |RTMP, kesintisiz akış |
| Fiyat |Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesini tıklatın |Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) |
| En fazla çalışma süresi |7/24 |7/24 |
| Maskeleme görüntülerini ekleme desteği |Hayır |Evet |
| API üzerinden sinyal ad desteği|Hayır |Evet |
| Ad SCTE35 ınband sinyal desteği|Evet |Evet |
| Doğrudan CEA 608/708 resim yazıları |Evet |Evet |
| Akış katkı içinde kısa takılması kurtarma olanağı |Evet |Hayır (LiveEvent slating giriş verisi olmadan 6 + saniye sonra başlayacak)|
| Tekdüzen olmayan giriş GOPs desteği |Evet |Giriş 2 sn GOPs Hayır – düzeltilmesi gerekir |
| Değişken kare hızı giriş desteği |Evet |Hayır – giriş kare hızı düzeltilmesi gerekir.<br/>İkincil Çeşitlemeler, örneğin, yüksek hareket planda sırasında izin verilir. Ancak Kodlayıcı 10 Çerçeve/sn için bırakılamıyor. |
| Otomatik-akış kesici LiveEvent kullanıldığında, kaybolur |Hayır |12 çalışan hiçbir LiveOutput ise saat sonra |

## <a name="liveevent-states"></a>LiveEvent durumları 

Bir LiveEvent geçerli durumu. Olası değerler şunlardır:

|Durum|Açıklama|
|---|---|
|**Durduruldu**| Bu ilk LiveEvent oluşturulduktan sonra bir durumda (autostart seçmediyseniz belirtilen.) Bu durumda hiçbir faturalama oluşur. Bu durumda, LiveEvent özellikleri güncelleştirildi ancak Akış verilmez.|
|**Başlatma**| LiveEvent başlatılıyor. Bu durumda hiçbir faturalama oluşur. Bu durum süresince güncelleştirmelere veya akışa izin verilmez. Bir hata oluşursa LiveEvent durduruldu durumuna döndürür.|
|**Çalışan**| LiveEvent Canlı akışlar işleyebilen ' dir. Şimdi kullanım faturalama. Daha fazla faturalama önlemek için LiveEvent durdurmanız gerekir.|
|**Durdurma**| LiveEvent durduruluyor. Hiçbir faturalama bu geçici durumunda oluşur. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.|
|**Silme**| LiveEvent siliniyor. Hiçbir faturalama bu geçici durumunda oluşur. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.|

## <a name="liveoutput"></a>LiveOutput

A [LiveOutput](https://docs.microsoft.com/en-us/rest/api/media/liveoutputs) yayımlama, kaydetme ve canlı akış DVR penceresi ayarlarını denetlemenize olanak verir. Burada bir kanal (LiveEvent) sabit bir içerik akışının bulunduğu ve programın (LiveOutput) bu LiveEvent üzerinde zamanlanmış bir olayı kapsamlıdır geleneksel bir ortama LiveEvent ve LiveOutput ilişki benzer.
Ayarlayarak LiveOutput için kaydedilen içeriği korumak istediğiniz saat sayısını belirtebilirsiniz **ArchiveWindowLength** özelliği. **ArchiveWindowLength** arşiv penceresi uzunluğu (Dijital Video Kaydedici veya DVR) bir ISO 8601 timespan süresi. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. 

**ArchiveWindowLength** maksimum süre istemci sayısı geçmişe arama geçerli Canlı konumdan de belirler. Belirtilen süre miktarından uzun LiveOutputs çalıştırabilirsiniz, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin değeri, istemci bildiriminin ne kadar süreyle büyüyebilir de belirler.

Her LiveOutput ile ilişkili bir [varlık](https://docs.microsoft.com/rest/api/media/assets)ve Media Services hesabına bağlı Azure depolama alanında bir kapsayıcı halinde verilerini kaydeder. LiveOutput yayımlamak için oluşturmalısınız bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) ilişkili varlığa yönelik. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir LiveEvent eşzamanlı çalışan Üçe LiveOutputs destekler, böylece aynı gelen akışın birden çok arşivini oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir 7 x 24 Canlı doğrusal akışı yayınlamak için ancak Programlar "kayıtları" yakalama görüntülemek için isteğe bağlı içerik olarak müşterilere sunmak için gün boyunca oluşturmak istediğiniz.  Bu senaryo için önce birincil LiveOutput 1 saat ile bir kısa arşiv penceresi oluşturmanız veya müşterilerin içine birincil canlı akış olarak ayarlamak daha az. Bu LiveOutput için bir StreamingLocator oluşturun ve uygulama veya web sitesi "Canlı" akışı olarak yayımlamak.  Akış çalışıyor gibi bir Göster (veya bazı tanıtıcıları sağlamak için 5 dakika erken. daha sonra kırpmaya) başında ikinci eşzamanlı LiveOutput program aracılığıyla oluşturabilirsiniz Bu ikinci LiveOutput 5 dakika ve program veya olay sona erdikten sonra durdurulabilir ve daha sonra uygulamanızın katalog bir isteğe bağlı varlığı olarak bu programı yayımlamak için yeni bir StreamingLocator oluşturabilirsiniz.  Bu işlem için diğer program sınırları birden çok kez yineleyin veya vurgular, isteğe bağlı hemen paylaşmak istediğiniz, tüm ilk LiveOutput "Canlı" akışı sırasında doğrusal akış yayın denemeye devam eder.  Ayrıca, head ve Arşiv programlar arasında tail "çakışma güvenliği" sunulan LiveOutput gelen kırpma ve daha doğru bir başlangıç ve bitiş içeriğin elde etmek için dinamik filtre destek yararlanabilir. Arşivlenen içeriği de gönderilebilir için bir [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms) kodlama veya diğer hizmetler için dağıtım olarak kullanılmak üzere birden çok çıktı biçimi için doğru subclipping çerçeve.

## <a name="streamingendpoint"></a>StreamingEndpoint

LiveEvent akan akış olduktan sonra bir varlık, LiveOutput ve StreamingLocator oluşturarak akış olayını başlayabilirsiniz. Bu akış arşivlemek ve aracılığıyla izleyiciler için kullanılabilir hale getirmek [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Media Services hesabınızı oluştururken bir varsayılan akış uç noktası durduruldu durumunda hesabınıza eklenir. İçeriğinizi akış başlatmak ve dinamik paketleme ve dinamik şifreleme yararlanmak için içerik akışını gerçekleştirmek istediğiniz akış uç çalışıyor durumda olması gerekir.

## <a name="billing"></a>Faturalandırma

Bir LiveEvent durumu "Çalışıyor" geçişleri hemen faturalama başlar. Faturalama gelen LiveEvent durdurmak için LiveEvent durdurmak zorunda.

> [!NOTE]
> Zaman **LiveEventEncodingType** üzerinde [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) ayarlanmış temel olarak 12 saat Giriş akışı kaybedilir ve vardır sonra hala "Çalışır" durumda olan tüm LiveEvent kesici Media Services otomatik yok LiveOutputs çalışıyor. Ancak, yine "Çalışır" durumda LiveEvent saat için faturalandırılır.
>

Aşağıdaki tabloda nasıl fatura moduna LiveEvent durumları eşleme gösterir.

| LiveEvent durumu | Faturalama mi? |
| --- | --- |
| Başlatılıyor |Hayır (geçici durum) |
| Çalışıyor |EVET |
| Durduruluyor |Hayır (geçici durum) |
| Durduruldu |Hayır |

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)
