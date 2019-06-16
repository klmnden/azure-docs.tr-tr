---
title: Azure - Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla Canlı Stream | Microsoft Docs
description: 'Bu konuda, bir şirket içi kodlayıcıdan Çoklu bit hızlı canlı akış alan bir kanalı ayarlama işlemi açıklanmaktadır. Akış, ardından aşağıdaki Uyarlamalı akış protokollerine birini kullanarak bir veya daha fazla akış uç noktaları aracılığıyla, istemci kayıttan yürütme uygulamalara dağıtılabilecek: HLS, kesintisiz akış, DASH.'
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: d9f0912d-39ec-4c9c-817b-e5d9fcf1f7ea
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: cenkd;juliako
ms.openlocfilehash: da20e4601b75bcb22546d21f6ad218ac9ba2728b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61463806"
---
# <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders"></a>Şirket içi kodlayıcılardan Çoklu bit hızlı canlı akış alan kanallar ile çalışma

> [!NOTE]
> Canlı kanallar 12 Mayıs 2018 tarihinden itibaren artık RTP/MPEG-2 aktarım akışı destek alma protokolü. Lütfen RTP/MPEG-2'den RTMP veya parçalanmış MP4'e geçiş (kesintisiz akış) alma protokolleri.

## <a name="overview"></a>Genel Bakış
Azure Media Services, bir *kanal* canlı akış içeriği işlemek için bir işlem hattını temsil eder. Bir kanal, Canlı giriş akışları iki yoldan biriyle alır:

* Bir şirket içi Canlı Kodlayıcı Çoklu bit hızlı RTMP veya kesintisiz akış (parçalanmış MP4) akış Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanala gönderir. Alınan akışların herhangi başka bir işlemeye olmadan kanallar aracılığıyla geçirin. Bu yöntem çağrılır *doğrudan*. Gerçek zamanlı bir kodlayıcı, gerçek zamanlı kodlama için etkinleştirilmemiş bir kanala tek bit hızında akışa da gönderebilir, ancak bunu önermiyoruz. Media Services akış isteyen müşteriler sunar.

  > [!NOTE]
  > Doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur.


* Bir şirket içi Canlı Kodlayıcı, aşağıdaki biçimlerden birinde Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanala tek bit hızlı akış gönderir: RTMP veya kesintisiz akış (parçalanmış MP4). Kanal, ardından gelen tek bit hızlı akışın Çoklu bit hızlı (Uyarlamalı) video akışına gerçek zamanlı kodlama gerçekleştirir. Media Services akış isteyen müşteriler sunar.

Kanal oluşturduğunuzda Media Services 2.10 sürüm ile başlayarak, kanalınızı giriş akışını almak için nasıl istediğinizi belirtebilirsiniz. Akışınız gerçek zamanlı kodlama gerçekleştirmek için kanal isteyip istemediğinizi belirtebilirsiniz. İki seçeneğiniz vardır:

* **Geçişine**: Çıktı olarak bir Çoklu bit hızında akışa (doğrudan akışı) sahip bir şirket içi Canlı Kodlayıcı kullanmayı planlıyorsanız, bu değeri belirtin. Bu durumda, gelen akış çıkışı herhangi bir kodlama içermeyen geçer. Bu kanal 2.10 yayınlanmadan önce davranıştır. Bu makalede, bu tür bir kanallar ile çalışma hakkında ayrıntılar sağlar.
* **Live Encoding**: Çoklu bit hızı akışı, tek bit hızlı canlı akış kodlama için Media Services kullanmayı planlıyorsanız, bu değeri seçin. Canlı kodlama kanalda bırakarak bir **çalıştıran** durumu, fatura ücretleri artmasına neden olur. Ek saatlik ücretlerden kaçınmak için canlı akış etkinliğinizi tamamlandıktan sonra hemen çalışan kanallarınızın durdurmanız önerilir. Media Services akış isteyen müşteriler sunar.

> [!NOTE]
> Bu makalede, gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş değil kanallar öznitelikleri açıklanmaktadır. Gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanallar ile çalışma hakkında daha fazla bilgi için bkz: [Çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services'i kullanarak canlı akış](media-services-manage-live-encoder-enabled-channels.md).
>
>Şirket içi Kodlayıcıları, önerilen hakkında bilgi için bkz [şirket içi Kodlayıcıları önerilen](media-services-recommended-encoders.md).

Aşağıdaki diyagramda bir şirket içi Canlı Kodlayıcı, Çoklu bit hızlı RTMP veya parçalanmış MP4 kullanan bir canlı akış bir iş akışını temsil eden çıkış akışlarına (kesintisiz akış).

![Canlı iş akışı][live-overview]

## <a id="scenario"></a>Ortak canlı akış senaryosu
Aşağıdaki adımlar, ortak canlı akış uygulamaları oluşturmak için gerekli olan görevleri açıklamaktadır.

1. Bilgisayara bir video kamera bağlayın. Başlatın ve Çoklu bit hızlı RTMP veya parçalanmış MP4 sahip bir şirket içi Canlı Kodlayıcı yapılandırın (kesintisiz akış) akış çıkış olarak. Daha fazla bilgi için bkz. [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](https://go.microsoft.com/fwlink/?LinkId=532824).

    Bu adım, kanalınızı oluşturduktan sonra da gerçekleştirebilirsiniz.
2. Bir kanal oluşturup başlatın.

3. Kanal alma URL'sini.

    Gerçek zamanlı Kodlayıcı alma URL'si akışı kanala göndermek için kullanır.
4. Kanal Önizleme URL'sini alın.

    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.
5. Bir program oluşturun.

    Azure portalı kullandığınızda, bir program oluşturma bir varlık da oluşturur.

    .NET SDK veya REST kullandığınızda, bir varlık oluşturun ve bir program oluştururken bu varlığın kullanılacağını belirtmeniz gerekir.
6. Programla ilişkili varlığı yayımlayın.   

    >[!NOTE]
    >Azure Media Services hesabınız oluşturulduğunda bir **varsayılan** akış uç noktası eklenir, hesabınızdaki **durduruldu** durumu. İçerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumunda olması gerekir.

7. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın.

8. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.

9. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.

10. Programı silin (ve isteğe bağlı olarak varlığı silin).     

## <a id="channel"></a>Bir kanal ve ilgili bileşenlerini açıklaması
### <a id="channel_input"></a>Kanal giriş (alma) yapılandırmalar
#### <a id="ingest_protocols"></a>Akış Protokolü alma
Media Services, Çoklu bit hızlı parçalanmış MP4 ve Çoklu bit hızlı RTMP olarak akış protokolleri kullanarak canlı akışlar başlayan kümeniz destekler. RTMP içe alma, Akış Protokolü seçildiğinde, iki alma (giriş) uç noktaları için kanal oluşturulur:

* **Birincil URL**: Tam URL'sini kanal birincil RTMP içe alma, uç nokta belirtir.
* **İkincil URL** (isteğe bağlı): Tam URL'sini kanal ikincil RTMP içe alma, uç nokta belirtir.

Alma stream (aynı zamanda Kodlayıcı yük devretme ve hataya dayanıklılık), dayanıklılık ve hataya dayanıklılığını artırmak istiyorsanız, özellikle aşağıdaki senaryolar için ikincil URL'yi kullanın:

- Tek Kodlayıcı: birincil ve ikincil URL'lere çift gönderme

    Ana amacı, bu senaryo ağ dalgalanmaları ve sinyal daha dayanıklı sağlamaktır. Bazı RTMP kodlayıcılar işlemez ağ iyi bağlantısını keser. Bir ağ bağlantı gerçekleştiğinde bir kodlayıcı kodlamayı durdurmak ve bir yeniden gerçekleştiğinde ardından arabelleğe alınan verileri göndermek. Bu discontinuities ve veri kaybına neden olur. Ağ bağlantısı kesilmelerine hatalı ağ veya bakım nedeniyle Azure tarafında gerçekleşebilir. Birincil/ikincil URL'leri ağ sorunları azaltmak ve denetimli bir yükseltme işlemi sağlar. Zamanlanmış ağ bağlantısını kes olduğunda, Media Services yöneten birincil ve ikincil bağlantısını keser ve bir Gecikmeli sağlayan her zaman iki tür arasında bağlantıyı kes. Kodlayıcıları daha sonra yeniden bağlanmanız ve veri göndermeye devam zaman sağlayın. Sırasını keser rastgele olabilir, ancak her zaman olacaktır bir gecikme birincil/ikincil veya ikincil/birincil URL. Bu senaryoda, kodlayıcı hala tek hata noktasıdır.

- Adanmış bir noktaya gönderme her Kodlayıcı ile birden çok kodlayıcılar:

    Bu senaryoda, her iki Kodlayıcı sunar ve yedeklilik alır. Bu senaryoda encoder1 birincil URL'sine gönderir ve encoder2 ikincil URL'sine gönderir. Bir kodlayıcı başarısız olduğunda, diğer Kodlayıcı veri göndermeye devam. Media Services, aynı zamanda birincil ve ikincil URL'leri kesmez çünkü veri yedekliği tutulabilir. Bu senaryo, kodlayıcılar zaman eşitlenmiş olan ve tam olarak aynı verileri sağlayan varsayar.  

- Birden çok kodlayıcılar: birincil ve ikincil URL'lere çift gönderme

    Bu senaryoda, hem Kodlayıcı hem birincil hem de ikincil URL'lere veri gönderme. Bu, en iyi güvenilirlik ve hata toleransı, hem de veri yedekliği sağlar. Bu senaryo, hem Kodlayıcı hatalarına dayanabileceğinden ve bir kodlayıcı çalışmayı durdurursa bile keser. Kodlayıcıları zaman eşitlenmiş olan ve tam olarak aynı verileri sağlayan olduğunu varsayar.  

Gerçek zamanlı kodlayıcılar RTMP hakkında daha fazla bilgi için bkz. [Azure Media Services RTMP desteği ve gerçek zamanlı kodlayıcılar](https://go.microsoft.com/fwlink/?LinkId=532824).

#### <a name="ingest-urls-endpoints"></a>Alma URL'leri (Bitiş)
Bir kanalın giriş uç noktası sağlar (alma URL'si) Kodlayıcı gönderebilirsiniz, böylece, gerçek zamanlı Kodlayıcı akışları kanallarınıza belirttiğiniz.   

Kanıl oluşturduğunuzda alma URL'lerini alabilirsiniz. Bu URL'leri almak için kanal olması gerekmez **çalıştıran** durumu. Kanala veri göndermeye başlamak için hazır olduğunuzda, kanal olmalıdır **çalıştıran** durumu. Veri alma kanal başlatıldıktan sonra akışınızı Önizleme URL yoluyla önizleyebilirsiniz.

Parçalanmış MP4 almak bir seçeneğiniz vardır (kesintisiz akış) canlı akış bir SSL bağlantısı üzerinden. SSL üzerinden alımı için alma URL'si için HTTPS güncelleştirdiğinizden emin olun. Şu anda, SSL üzerinden RTMP alma olamaz.

#### <a id="keyframe_interval"></a>Ana kare aralığı
Çoklu bit hızı akışı oluşturmak için bir şirket içi Canlı Kodlayıcı kullanırken, dış, kodlayıcı tarafından kullanılan ana kare aralığı grubu (GOP) resimlerin süresini belirtir. Kanal gelen bu akış aldıktan sonra aşağıdaki biçimlerden birinde istemci kayıttan yürütme uygulamalara Canlı akışınızı iletebilirsiniz: Akış (HLS), kesintisiz akış, dinamik Uyarlamalı HTTP (DASH) ve HTTP üzerinden akış Canlı. Canlı akış yaparken HLS her zaman dinamik olarak paketlenir. Varsayılan olarak, Media Services Canlı kodlayıcıdan alındığı ana kare aralığı temel HLS segment paketleme oranı (kesim başına parça) otomatik olarak hesaplar.

Aşağıdaki tabloda, kesim süresinin nasıl hesaplandığını gösterilmektedir:

| Ana kare aralığı | HLS segment paketleme oranı (FragmentsPerSegment) | Örnek |
| --- | --- | --- |
| 3 saniye küçüktür veya eşittir |3:1 |2 saniye KeyFrameInterval (veya GOP) ise, varsayılan HLS segment paketleme 3'e 1 oranıdır. Bu, 6-ikinci HLS kesimi oluşturur. |
| 3 ila 5 saniye |2:1 |4 saniyenin KeyFrameInterval (veya GOP) ise, varsayılan HLS segment paketleme 2'den 1 oranıdır. Bu, bir 8 saniyelik HLS segment oluşturur. |
| 5 saniyeden büyük |1:1 |6 saniye KeyFrameInterval (veya GOP) ise, varsayılan HLS segment paketleme 1'e 1 oranıdır. Bu, 6-ikinci HLS kesimi oluşturur. |

Kanalın çıkış yapılandırarak ve üzerinde ChannelOutputHls FragmentsPerSegment ayarı parça başına kesim oranı değiştirebilirsiniz.

Ana kare aralık değeri üzerinde ChannelInput KeyFrameInterval özelliğini ayarlayarak da değiştirebilirsiniz. Açıkça KeyFrameInterval ayarlarsanız, HLS FragmentsPerSegment daha önce açıklanan kuralları hesaplanır paketleme oranı segmentlere ayırın.  

Media Services, açıkça KeyFrameInterval hem FragmentsPerSegment ayarlarsanız, ayarladığınız değerleri kullanır.

#### <a name="allowed-ip-addresses"></a>İzin verilen IP adresleri
Bu kanala video yayımlamasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresi aşağıdakilerden biri belirtilebilir:

* Tek bir IP adresi (örneğin, 10.0.0.1)
* Bir IP adresi ve CIDR alt ağ maskesi (örneğin, 10.0.0.1/22) kullanan bir IP aralığı
* Bir IP adresi ve noktalı ondalık alt ağ maskesine (örneğin, 10.0.0.1(255.255.252.0)) kullanan bir IP aralığı

Herhangi bir IP adresi belirtilmezse ve kural tanımı yoksa hiçbir IP adresine izin verilir. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.

### <a name="channel-preview"></a>Kanal önizlemesi
#### <a name="preview-urls"></a>Önizleme URL'leri
Kanalları daha fazla işleme edip teslime geçmeden önce akışınızı onaylama için kullandığınız bir önizleme uç noktası (Önizleme URL'si) sağlayın.

Kanıl oluşturduğunuzda Önizleme URL'sini alabilirsiniz. URL almak için kanal olması gerekmez **çalıştıran** durumu. Veri alma kanal başladıktan sonra akışınızın önizlemesini.

Şu anda Önizleme akışı yalnızca parçalanmış MP4 içinde teslim edilebilir belirtilen giriş türü ne olursa olsun (kesintisiz akış) biçimi. Kullanabileceğiniz [kesintisiz akış sistem durumu İzleyicisi](https://playready.directtaps.net/smoothstreaming/) kesintisiz akışı test etmek için player. Akışınız görüntülemek için Azure Portalı'nda barındırılan bir oynatıcı de kullanabilirsiniz.

#### <a name="allowed-ip-addresses"></a>İzin verilen IP adresleri
Önizleme uç noktasına bağlanmasına izin verilen IP adreslerini tanımlayabilirsiniz. IP adresi belirtilmezse, herhangi bir IP adresine izin verilir. İzin verilen IP adresi aşağıdakilerden biri belirtilebilir:

* Tek bir IP adresi (örneğin, 10.0.0.1)
* Bir IP adresi ve CIDR alt ağ maskesi (örneğin, 10.0.0.1/22) kullanan bir IP aralığı
* Bir IP adresi ve noktalı ondalık alt ağ maskesine (örneğin, 10.0.0.1(255.255.252.0)) kullanan bir IP aralığı

### <a name="channel-output"></a>Kanal çıkış
Kanal çıkış hakkında daha fazla bilgi için bkz. [ana kare aralığı](#keyframe_interval) bölümü.

### <a name="channel-managed-programs"></a>Kanal yönetilen programlar
Bir kanal yayımlanması ve depolanmasını Canlı akıştaki segmentlerin denetlemek için kullanabileceğiniz programlarla ilişkilidir. Kanallar, programları yönetir. Kanal ve program ilişki, burada bir kanal sabit bir içerik akışının vardır ve bir programın bu kanalda zamanlanmış bir olayı kapsamlı geleneksel Medyadaki benzerdir.

Program için kaydedilen içeriği kaç saat tutmak istediğinizi **Arşiv Penceresi** uzunluğunu ayarlayarak belirleyebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. Arşiv penceresi uzunluğu ayrıca en fazla saat istemciyi geçmişe geçerli Canlı konumdan gidebilecekleri. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her bir program, akış içeriği depolar bir varlıkla ilişkilidir. Bir varlık, bir Azure depolama hesabındaki blok blob kapsayıcısını eşlenir ve varlık içindeki dosyalara, kapsayıcıdaki blobları olarak depolanır. Müşterilerinizin akış görebilecek şekilde programı yayımlamak için ilişkili varlığa yönelik bir OnDemand Bulucu oluşturmanız gerekir. Bu bulucuya istemcilerinize sağlayabilen bir akış URL'si oluşturmak için kullanabilirsiniz.

Bir kanal sağlayan üç adede kadar eşzamanlı çalışan programlar destekler, böylece aynı gelen akışın birden fazla arşiv oluşturabilirsiniz. Yayımlama ve gerektiğinde bir olayın farklı kısımlarını arşivleyin. Örneğin, iş gereksiniminiz bir programın 6 saatini Arşivlerken yalnızca son 10 dakikasını yayınlamak için olduğunu düşünün. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program altı saatlik olayı arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ayarlanır ve bu program yayımlanır.

Yeni olaylar için mevcut programları yeniden kullanmamalısınız. Bunun yerine, her olay için yeni bir program oluşturun. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.

Arşivlenen içeriği silmek için durdurun ve programı silin ve ardından ilişkili varlığı silin. Bir program kullanılıyorsa varlık silinemez. Programın silinmesi gerekir.

Bile durdurduktan ve programı silmenin sonra varlık silene kadar kullanıcılar arşivlenen içeriğinizin isteğe bağlı video olarak akışını yapabilirsiniz. Arşivlenen içeriği tutmak, ancak sahip değil kullanılabilir akış için isterseniz akış Bulucuyu silin.

## <a id="states"></a>Kanal durumları ve faturalandırma
Kanalın geçerli durumu için olası değerler şunlardır:

* **Durduruldu**: Bu, kanalın oluşturulduktan sonraki ilk durumudur. Bu durumda, kanal özellikleri güncelleştirilebilir ama akışa izin verilmez.
* **Başlangıç**: Kanal başlatılıyor. Bu durum süresince güncelleştirmelere veya akışa izin verilmez. Bir hata oluşursa kanal döndürür **durduruldu** durumu.
* **Çalışan**: Kanal Canlı akışları işleyebilir.
* **Durdurma**: Kanal durduruluyor. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.
* **Silme**: Kanal siliniyor. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.

Aşağıdaki tabloda, kanal durumlarının faturalandırma modu ile nasıl eşleştiği gösterilir.

| Kanal durumu | Portal kullanıcı arabirimi göstergeleri | Faturalandırılmış mı? |
| --- | --- | --- |
| **Başlatma** |**Başlatma** |Hayır (geçici durum) |
| **Çalıştıran** |**Hazır** (çalışan program yok)<p><p>or<p>**Akış** (en az bir program çalışıyor) |Evet |
| **Durduruluyor** |**Durduruluyor** |Hayır (geçici durum) |
| **Durduruldu** |**Durduruldu** |Hayır |

## <a id="cc_and_ads"></a>Kapalı Açıklamalı Altyazı ve reklam ekleme
Aşağıdaki tablo, Kapalı Açıklamalı Altyazı ve ad ekleme için desteklenen standartları göstermektedir.

| Standart | Notlar |
| --- | --- |
| CEA-708 kapalı ve EIA 608 (708/608) |CEA-708 kapalı ve EIA 608 Kapalı Açıklamalı Altyazı Amerika Birleşik Devletleri ve Kanada'da standartları.<p><p>Şu anda, açıklamalı alt yazı yalnızca kodlanmış giriş akışında taşınan durumunda desteklenir. Media Services'e gönderilen kodlanmış stream'de 608 veya 708 açıklamalı alt yazı ekleyebileceğiniz bir canlı Medya Kodlayıcısı kullanmanız gerekir. Media Services, görüntüleyenler için eklenen açıklamalı alt yazılı içerik sunar. |
| TTML içinde .ismt (metin parçaları kesintisiz akış) |Media Services dinamik paketleme, istemcilerinize içerik akışı şu biçimlerden birinde sağlar: DASH, HLS veya kesintisiz akış. İçe alma, ancak parçalanmış MP4 (kesintisiz akış) .ismt (metin parçaları kesintisiz akış) içinde açıklamalı alt yazılar ile akış yalnızca kesintisiz akış istemcilere sunabilirsiniz. |
| SCTE-35 |SCTE-35 işaret reklam ekleme için kullanılan dijital bir sinyal sistemidir. Aşağı Akış alıcılar sinyal akışının içine bir reklam için ayrılan süre splice için kullanın. Giriş akışındaki seyrek bir parçası olarak SCTE-35 gönderilmesi gerekir.<p><p>Şu anda yalnızca desteklenen Giriş akışı biçimlendirme ad sinyalleri parçalanmış bu taşıyan MP4 (kesintisiz akış). Desteklenen tek çıktı biçimi de kesintisiz akış. |

## <a id="considerations"></a>Dikkat edilmesi gerekenler
Çoklu bit hızlı akış kanala göndermek için bir şirket içi Canlı Kodlayıcı kullanırken, aşağıdaki kısıtlamalar geçerlidir:

* Veri alma noktalarına göndermek için yeterli boş Internet bağlantısı olduğundan emin olun.
* Alma URL'si ikincil kullanarak ek bant genişliği gerektirir.
* Gelen Çoklu bit hızı akışı, en fazla 10 video kalite düzeylerine (katman) ve en fazla 5 ses parçaları olabilir.
* Video kalitesi düzeylerini hiçbiri için en yüksek ortalama hızı 10 MB/sn olmalıdır.
* Tüm video ve ses akışları için ortalama bit hızlarında toplamını 25 MB/sn olmalıdır.
* Kanal giriş protokolünü değiştiremezsiniz veya ilişkili programları çalışıyorken. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Kanalınızı tek bit hızlı içe alabilir. Ancak stream kanal işlemez olduğundan, istemci uygulamalar da tek bit hızlı akış alırsınız. (Bu seçenek önerilmemektedir.)

Kanallar ve ilgili bileşenler ile ilgili diğer konular şunlardır:

* Gerçek zamanlı Kodlayıcı yeniden her seferinde, çağrı **sıfırlama** kanalındaki yöntemi. Kanal sıfırlamadan önce programı durdurmak zorunda. Kanal sıfırladıktan sonra programın yeniden başlatın.

  > [!NOTE]
  > Program yeniden başlattığınızda, yeni bir varlık ile ilişkilendirmek ve yeni bir Bulucu oluşturmanız gerekir. 
  
* Bir kanal yalnızca olduğunda durdurulabilir **çalıştıran** durumu ve tüm programlar durduruldu.
* Varsayılan olarak, Media Services hesabınız için yalnızca beş kanallar ekleyebilirsiniz. Daha fazla bilgi için [kotaları ve sınırlamaları](media-services-quotas-and-limitations.md).
* Yalnızca, kanal içinde olduğunda faturalandırılırsınız **çalıştıran** durumu. Daha fazla bilgi için [kanal durumları ve faturalandırma](media-services-live-streaming-with-onprem-encoders.md#states) bölümü.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="feedback"></a>Geri Bildirim
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Önerilen şirket içi kodlayıcılar](media-services-recommended-encoders.md)

[Azure Media Services parçalanmış MP4 hayatını içe alma belirtimi](media-services-fmp4-live-ingest-overview.md)

[Azure Media Services'a Genel Bakış ve sıklıkla karşılaşılan senaryolar](media-services-overview.md)

[Media Services kavramları](media-services-concepts.md)

[live-overview]: ./media/media-services-manage-channels-overview/media-services-live-streaming-current.png
