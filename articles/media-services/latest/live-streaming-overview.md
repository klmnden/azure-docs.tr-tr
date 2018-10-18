---
title: Azure Media Services'i kullanarak canlı akış genel bakış | Microsoft Docs
description: Bu konuda genel bir bakış Canlı Azure Media Services v3 kullanarak akış olanağı sağlar.
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
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: 533aa505c38d3cbfb46d70acecd43cc66614b13d
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378145"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Canlı akış ile Azure Media Services v3

Azure Media Services ile etkinliklerin canlı akış sunarken aşağıdaki bileşenler yaygın olarak kullanılır:

* Etkinliği yayınlamak için kullanılan bir kamera.
* Sinyaller kamera (veya başka bir cihaz, dizüstü bilgisayar gibi) Lve akış hizmetine gönderilen akışlara dönüştürür canlı bir video Kodlayıcısı. Sinyaller SCTE-35 ve Ad ipuçları reklam de içerebilir. 
* Media Services canlı akış hizmeti, alma, Önizleme, paketleme, kaydetme, şifrelemek ve müşterilerinize veya başkalarına dağıtım için bir CDN içeriği yayını sağlar.

Bu makalede ayrıntılı bir genel bakış sağlar ve Media Services ile canlı akış ilgili ana bileşenleri diyagramları içerir.

## <a name="overview-of-main-components"></a>Ana bileşenlerini genel bakış

Media Services'de canlı akış içeriğini işlemekten [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) sorumludur. Bir Livestream giriş uç noktası sağlar (alma URL'si) bir şirket içi Canlı Kodlayıcı ardından sağlayın. Livestream RTMP veya kesintisiz akış biçiminde gerçek zamanlı Kodlayıcı giriş Canlı akışlarınızdan aldığında ve bir veya daha fazla akış için kullanılabilir hale getirir [akış](https://docs.microsoft.com/rest/api/media/streamingendpoints). A [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) yayımlama, kayıt ve canlı akış'ın DVR penceresi ayarlarını denetlemenize olanak verir. Livestream ayrıca daha fazla işleme edip teslime geçmeden önce akışınızı onaylama için kullandığınız bir önizleme uç noktası (Önizleme URL'si) sağlar. 

Medya hizmetleri sağlayan **dinamik paketleme**, Önizleme ve yayın, MPEG DASH, HLS, kesintisiz akış içeriğinizi olanak tanıyan akış biçimlerinde gerek kalmadan bu akış biçimlerinin el ile yeniden paketleyin. Tüm HLS, DASH veya kesintisiz uyumlu oyuncuların oynatabilirsiniz. Ayrıca [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) akışınızı test etmek için.

Media Services dinamik olarak şifrelenmiş içerik teslim etmenizi sağlar (**dinamik şifreleme**) Gelişmiş Şifreleme Standardı (AES-128) veya herhangi bir üç ana dijital hak yönetimi (DRM) sistemi: Microsoft PlayReady Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere.

İsterseniz, ayrıca uygulayabilirsiniz **dinamik filtreleme**, parçalar, biçimleri, oyuncular gönderilen bit hızlarına dönüştürme sayısını kontrol etmek için kullanılabilir. Media Services, reklam yerleştirme de destekler.

### <a name="new-live-encoding-improvements"></a>Yeni canlı kodlama geliştirmeleri

Aşağıdaki yeni geliştirmeler yeni sürümde yapıldığını.

- Yeni düşük gecikme süresi modu için Canlı (10 saniye için uçtan uca).
- Geliştirilmiş RTMP desteği (daha fazla kararlılık ve daha fazla kaynak Kodlayıcı desteği).
- Güvenli RTMPS alın.

    Artık size bir Livestream oluşturduğunuzda 4 URL'lerini alabilirsiniz. 4 alma URL'leri neredeyse aynıdır, yalnızca bir bağlantı noktası numarası bölümü farklı aynı akış belirteci (AppID) sahip. URL'lerin ikisinin, birincil ve yedek RTMPS için.   
- 24 saatlik biçim dönüştürme desteği. 
- RTMP SCTE35 aracılığıyla ad sinyal desteği geliştirildi.

## <a name="liveevent-types"></a>Livestream türleri

A [Livestream](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: live encoding ve doğrudan. 

### <a name="live-encoding-with-media-services"></a>Gerçek zamanlı ile Media Services encoding

![live Encoding](./media/live-streaming/live-encoding.png)

Aşağıdaki protokollerden birini Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş Livestream tek bit hızında akışa bir şirket içi Canlı Kodlayıcı gönderir: RTMP veya kesintisiz akış (parçalanmış MP4). Livestream ardından gelen gerçek zamanlı kodlama gerçekleştiren tek bit hızlı akışın Çoklu bit hızlı (Uyarlamalı) video akışı. İstendiğinde, Media Services akışı müşterilere teslim eder.

Bu tür bir Livestream oluştururken belirtmeniz **temel** (LiveEventEncodingType.Basic).

### <a name="pass-through"></a>Geçiş

![geçiş](./media/live-streaming/pass-through.png)

Geçiş, uzun süre çalışan Canlı akışlar veya 24 x 7 doğrusal canlı bir şirket içi Canlı Kodlayıcı kullanarak kodlama için optimize edilmiştir. Şirket içi Kodlayıcı, Çoklu bit hızına sahip gönderir **RTMP** veya **kesintisiz akış** (parçalanmış MP4) için yapılandırılmış Livestream **doğrudan** teslim. **Doğrudan** teslimi, alınan akışların gerektiğinde **Livestream**herhangi başka bir işlemeye olmadan s. 

Doğrudan LiveEvents destekleyebilir yukarı 4 K çözünürlüğe ve HEVC geçirmek aracılığıyla kesintisiz akış ile kullanıldığında alma protokolü. 

Bu tür bir Livestream oluştururken belirtmeniz **hiçbiri** (LiveEventEncodingType.None).

> [!NOTE]
> Uzun bir dönem içerisinde birden çok etkinlik gerçekleştirecekseniz ve zaten şirket içi kodlayıcılara yatırım yaptıysanız, doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/media-services/) detaylarına bakın.
> 

## <a name="liveevent-types-comparison"></a>Livestream türlerini karşılaştırma 

Aşağıdaki tabloda, iki Livestream tür özellikleri karşılaştırılır.

| Özellik | Doğrudan Livestream | Temel Livestream |
| --- | --- | --- |
| Tekli bit hızı girişi Çoklu bit hızlarında buluta halinde kodlanır |Hayır |Evet |
| En yüksek çözünürlük, Katmanlar sayısı |4Kp30  |720p, 6 katmanları 30 fps |
| Giriş protokolleri |RTMP, kesintisiz akış |RTMP, kesintisiz akış |
| Fiyat |Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesine tıklayın |Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) |
| Maksimum Çalıştırma süresi |7/24 |7/24 |
| Maskeleme görüntülerini ekleme desteği |Hayır |Evet |
| API aracılığıyla sinyal ad desteği|Hayır |Evet |
| Sinyal SCTE35 ınband ad desteği|Evet |Evet |
| Doğrudan CEA 608/708 açıklamalı alt yazılar |Evet |Evet |
| Akış katkısı kısa takılması kurtarma olanağı |Evet |Hayır (Livestream slating giriş verileri olmadan 6 + saniye sonra başlayacak)|
| Tekdüzen olmayan giriş GOPs desteği |Evet |Yok – giriş 2 sn GOPs giderilmiş olmalıdır |
| Değişken kare hızı girişi için destek |Evet |Yok – giriş kare hızı düzeltilmesi gerekir.<br/>Küçük farklılıklar, örneğin, yüksek bir hareket sahneler sırasında izin verilir. Ancak, kodlayıcı 10 Çerçeve/sn için bırakılamıyor. |
| Otomatik akışı kapatmaya Livestream anahtar kullanıldığında, kaybolur |Hayır |12 çalışan hiçbir LiveOutput ise saat sonra |

## <a name="liveevent-states"></a>Livestream durumları 

Bir Livestream geçerli durumu. Olası değerler şunlardır:

|Durum|Açıklama|
|---|---|
|**Durduruldu**| Bu ilk Livestream oluşturulduktan sonra durumudur (autostart seçmediyseniz belirtilir.) Bu durumda hiçbir faturalandırma gerçekleşir. Bu durumda Livestream özellikleri güncelleştirilebilir ama akışa izin verilmiyor.|
|**Başlatma**| Livestream başlatılıyor. Bu durumda hiçbir faturalandırma gerçekleşir. Bu durum süresince güncelleştirmelere veya akışa izin verilmez. Bir hata oluşursa Livestream durduruldu durumuna döndürülür.|
|**Çalıştıran**| Livestream Canlı akışları işleyebilir. Artık kullanım faturalandırma. Daha fazla faturalama önlemek için Livestream durdurmanız gerekir.|
|**Durduruluyor**| Livestream durduruluyor. Bu geçici bir durumda hiçbir faturalandırma gerçekleşir. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.|
|**Silme**| Livestream siliniyor. Bu geçici bir durumda hiçbir faturalandırma gerçekleşir. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.|

## <a name="liveoutput"></a>LiveOutput

A [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) yayımlama, kayıt ve canlı akış'ın DVR penceresi ayarlarını denetlemenize olanak verir. Livestream ve LiveOutput ilişki burada sabit bir içerik akışının bir kanal (Livestream) sahip ve bir program (LiveOutput) bu Livestream üzerinde zamanlanmış bir olayı kapsamlı geleneksel Medyadaki benzer.
Saat ayarlayarak LiveOutput için kaydedilen içeriği tutmak istediğinizi belirtebilirsiniz **ArchiveWindowLength** özelliği. **ArchiveWindowLength** bir ISO 8601 zaman aralığı süresince arşiv penceresi uzunluğu (Dijital Video Kaydedici veya DVR). Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. 

**ArchiveWindowLength** zaman istemcilerin sayısı geçerli Canlı konumdan geçmişe arama de belirler. Belirtilen süre LiveOutputs çalıştırabilirsiniz, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin değeri, ne kadar istemci uzayabileceğini de belirler.

Her LiveOutput ile ilişkili bir [varlık](https://docs.microsoft.com/rest/api/media/assets)ve Media Services hesabına bağlı Azure depolamadaki bir kapsayıcıda içine verileri kaydeder. LiveOutput yayımlamak için oluşturmanız gereken bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) ilişkili varlığa yönelik. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir Livestream eşzamanlı çalışan Üçe kadar LiveOutputs destekler, böylece aynı gelen akışın birden fazla arşiv oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir 24 x 7 Canlı doğrusal akış yayını için olan, ancak oldukça görüntülemek için isteğe bağlı içerik olarak müşterilere sunmak için gün boyunca programlar "kayıt" oluşturmak istediğiniz.  Bu senaryo için öncelikle bir birincil LiveOutput bir kısa arşiv penceresi 1 saat ile oluşturmanız veya müşterilerin içine birincil canlı akış olarak ayarlamak daha az. Bir StreamingLocator oluşturmak için bu LiveOutput ve uygulama veya web sitesi "Live" akış olarak yayımlayın.  Akış çalışırken bir Göster (veya bazı işler sağlamak için 5 dakika erken. daha sonra kırpılacak) başında ikinci bir eş zamanlı LiveOutput programlı olarak oluşturabilirsiniz Bu ikinci LiveOutput programı veya olay sona erdikten sonra 5 dakika durdurulabilir ve daha sonra uygulama Kataloğu'nda bir isteğe bağlı varlık olarak bu programı yayımlamak için yeni bir StreamingLocator oluşturabilirsiniz.  Bu işlem, diğer program sınırları için birden çok kez yineleyebilirsiniz ya da isteğe bağlı hemen paylaşmak istiyorsanız, tüm ilk LiveOutput "Live" akışı sırasında devam doğrusal akış yayını vurgular.  Ayrıca, baş ve Arşiv programlar arasında kuyruk "çakışma güvenliği" sunulan LiveOutput gelen trim ve bir daha doğru başlangıç ve bitiş içeriğin elde etmek için dinamik filtre desteği yararlanabilir. Arşivlenen içeriği da gönderilebilir için bir [dönüştürme](https://docs.microsoft.com/rest/api/media/transforms) kodlama veya diğer hizmetler için dağıtım olarak kullanılmak üzere birden çok çıkış biçimleri için doğru klip çerçeve.

## <a name="streamingendpoint"></a>StreamingEndpoint

Akışın LiveEvent'e akmasını sağladıktan sonra bir Asset, LiveOutput ve StreamingLocator oluşturarak etkinliği akışla göndermeye başlayabilirsiniz. Bu akışı arşivler ve aracılığıyla izleyiciler kullanabilmesi [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Media Services hesabınız oluşturulduğunda hesabınıza durdurulmuş durumda bir varsayılan akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının çalışıyor durumda olması gerekir.

## <a name="billing"></a>Faturalandırma

Bir Livestream "Çalışıyor" durumuna geçiş hemen sonra Fatura başlar. Faturalandırma gelen Livestream durdurmak için Livestream durdurmak zorunda.

> [!NOTE]
> Zaman **LiveEventEncodingType** üzerinde [Livestream](https://docs.microsoft.com/rest/api/media/liveevents) ayarlanmış temel olarak 12 saat sonra giriş akışı kaybolur ve başka hala "Çalışıyor" durumda olan tüm Livestream kesici Media Services otomatik yok LiveOutputs çalışıyor. Ancak, yine de "Livestream çalışıyor" konumunda olduğu süre için faturalandırılırsınız.
>

Aşağıdaki tabloda, Livestream durumları faturalandırma modu nasıl eşleştiği gösterilmektedir.

| Livestream durumu | Faturalandırma nedir? |
| --- | --- |
| Başlatılıyor |Hayır (geçici durum) |
| Çalışıyor |EVET |
| Durduruluyor |Hayır (geçici durum) |
| Durduruldu |Hayır |

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış Öğreticisi](stream-live-tutorial-with-api.md)
