---
title: "Azure - Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış | Microsoft Docs"
description: "Bu konuda, bir şirket içi kodlayıcıdan Çoklu bit hızlı canlı akış alan bir kanalı açıklar. Akış sonra istemci kayıttan yürütme uygulamaları aşağıdaki Uyarlamalı akış protokollerden birini kullanarak bir veya daha fazla akış uç noktalarını, aracılığıyla alınabilir: HLS, kesintisiz akış, tire."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: d9f0912d-39ec-4c9c-817b-e5d9fcf1f7ea
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 04/12/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 3f6569d32708c42247e0ffec70389f2e0f07389e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="live-streaming-with-on-premises-encoders-that-create-multi-bitrate-streams"></a>Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış
## <a name="overview"></a>Genel Bakış
Azure Media Services, bir *kanal* canlı akış içeriği işlemek için bir işlem hattını temsil eder. Bir kanal Canlı giriş akışları iki yoldan biriyle alır:

* Çoklu bit hızlı RTMP veya kesintisiz akış (parçalanmış MP4) akışı etkin değil. Media Services ile gerçek zamanlı kodlama gerçekleştirmek için kanal için bir şirket içi gerçek zamanlı Kodlayıcı gönderir. Alınan akışların herhangi başka bir işleme olmadan kanallardan geçirin. Bu yöntem çağrılır *doğrudan*. Çoklu bit hızlı kesintisiz akış çıktı olarak sahip şu gerçek zamanlı Kodlayıcıları kullanabilirsiniz: Media Excel, Ateme, düşünün iletişimleri, Envivio, Cisco ve Elemental. Şu gerçek zamanlı kodlayıcılar RTMP çıktı olarak vardır: dinamik Adobe Flash medya Kodlayıcı, Telestream Wirecast, Haivision, Teradek ve TriCaster. Gerçek zamanlı bir kodlayıcı, gerçek zamanlı kodlama için etkinleştirilmemiş bir kanala tek bit hızında akışa da gönderebilir, ancak bunu önermiyoruz. Media Services akışı isteyen müşteriler için sunar.

  > [!NOTE]
  > Bir doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur.


* Bir şirket içi gerçek zamanlı Kodlayıcı aşağıdaki biçimlerden birinde Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanala tek bit hızlı akış gönderir: RTP (MPEG-TS), RTMP veya kesintisiz akış (parçalanmış MP4). Kanal gelen tek bit hızlı akışın Çoklu bit hızlı (Uyarlamalı) video akışına gerçek zamanlı kodlama gerçekleştirir. Media Services akışı isteyen müşteriler için sunar.

Bir kanal oluşturduğunuzda Media Services 2.10 sürümünden başlayarak, nasıl kanalınızı Giriş akışı almak istediğinizi belirtebilirsiniz. Canlı akışınızı kodlama gerçekleştirmek için kanal isteyip istemediğinizi de belirtebilirsiniz. İki seçeneğiniz vardır:

* **Geçir**: çıktı olarak Çoklu bit hızında akışa (geçiş akışı) sahip olan bir şirket içi gerçek zamanlı Kodlayıcı kullanmayı planlıyorsanız, bu değeri belirtin. Bu durumda, gelen akış çıkışı herhangi kodlamadan geçtiği. Bir kanal 2.10 serbest bırakmadan önce davranış budur. Bu konuda bu tür kanallar ile çalışma hakkında ayrıntılar sağlar.
* **Kodlama canlı**: tek bit hızlı Canlı akışınızı Çoklu bit hızlı akış kodlamak için Media Services kullanmayı planlıyorsanız, bu değer seçin. Gerçek zamanlı kodlama bırakarak kanal unutmayın bir **çalıştıran** durumu fatura ücretleri erişimliye. Canlı akış olay fazladan saatlik ücret yazılmaması için tamamlandıktan sonra hemen çalışan kanalların kesmenizi öneririz. Media Services akışı isteyen müşteriler için sunar.

> [!NOTE]
> Bu konu, gerçek zamanlı kodlama gerçekleştirmek için etkin değil kanalları özniteliklerini açıklar. Gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanallar ile çalışma hakkında daha fazla bilgi için bkz: [Çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services kullanarak canlı akış](media-services-manage-live-encoder-enabled-channels.md).
>
>

Aşağıdaki diyagramda, Çoklu bit hızlı RTMP ya da parçalanmış MP4 sağlamak için bir şirket içi gerçek zamanlı Kodlayıcı kullanan bir canlı akış bir iş akışı temsil eder (kesintisiz akış) akışları çıktı olarak.

![Canlı iş akışı][live-overview]

## <a id="scenario"></a>Ortak canlı akış senaryosu
Aşağıdaki adımlar, ortak canlı akış uygulamaları oluşturmaya dahil olan görevleri açıklamaktadır.

1. Bilgisayara bir video kamera bağlayın. Başlangıç ve Çoklu bit hızlı RTMP veya parçalanmış MP4 sahip bir şirket içi gerçek zamanlı Kodlayıcı yapılandırın (kesintisiz akış) akış çıktı olarak. Daha fazla bilgi için bkz. [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](http://go.microsoft.com/fwlink/?LinkId=532824).

    Bu adım, kanalınızı oluşturduktan sonra de gerçekleştirebilirsiniz.
2. Bir kanal oluşturup başlatın.

3. Kanal alma URL'sini.

    Gerçek zamanlı Kodlayıcı alma URL'si akış kanala göndermek için kullanır.
4. Kanal Önizleme URL'sini alın.

    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.
5. Bir program oluşturun.

    Azure Portalı'nı kullandığınızda, bir program oluşturma bir varlık da oluşturur.

    .NET SDK veya REST kullandığınızda, bir varlık oluşturun ve bir programı oluştururken bu varlığın kullanılacağını belirtmeniz gerekir.
6. Programla ilişkili varlığı yayımlayın.   

    >[!NOTE]
    >Azure Media Services hesabınızı oluştururken bir **varsayılan** akış uç noktası ekleniyor hesabınızda **durduruldu** durumu. İçerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumunda olması gerekir.

7. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın.

8. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.

9. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.

10. Programı silin (ve isteğe bağlı olarak varlığı da silin).     

## <a id="channel"></a>Bir kanal ve ilgili bileşenlerini açıklaması
### <a id="channel_input"></a>Kanal giriş (alma) yapılandırmaları
#### <a id="ingest_protocols"></a>Alma Akış Protokolü
Media Services, Çoklu bit hızlı parçalanmış MP4 ve Çoklu bit hızlı RTMP olarak akış protokolleri kullanarak canlı akışlar alanını destekler. RTMP alma Protokolü akış seçildiğinde, iki (giriş) uç noktalarını alma kanalı oluşturulur:

* **Birincil URL**: kanalın birincil RTMP tam URL'sini alma uç nokta belirtir.
* **İkincil URL** (isteğe bağlı): kanalın ikincil RTMP tam URL'sini alma uç nokta belirtir.

Alma akış (yanı Kodlayıcı yük devretme ve hataya dayanıklılık), dayanıklılık ve hataya dayanıklılık artırmak istiyorsanız ikincil URL özellikle aşağıdaki senaryolarda kullanın:

- Birincil ve ikincil URL'lere çift Ftp'den tek Kodlayıcı:

    Bu senaryo ana amacı, ağ dalgalanmaları ve sinyal daha fazla esneklik sağlamaktır. Bazı RTMP kodlayıcılar işlemek olmayan ağ iyi bağlantısını keser. Bir ağ bağlantısının gerçekleştiğinde bir kodlayıcı kodlamayı durdurmak ve bir reconnect gerçekleştiğinde sonra arabelleğe alınan verileri göndermek. Bu discontinuities ve veri kaybına neden olabilir. Ağ bağlantısını keser Azure tarafında hatalı ağ veya bakım nedeniyle oluşabilir. Birincil/ikincil URL'leri ağ sorunlarını azaltmak ve denetlenen bir yükseltme işlemi sağlar. Her zaman bir zamanlanmış ağ bağlantısının olduğunda, Media Services yöneten birincil ve ikincil bağlantısını keser ve bir Gecikmeli sağlar ikisi arasında bağlantısını kesin. Kodlayıcılar veri göndermeye devam ve yeniden bağlanmanız zamanı sonra olmalıdır. Sırasını keser rastgele olabilir, ancak her zaman olacaktır bir gecikme birincil/ikincil ya da ikincil/birincil URL'ler arasında. Bu senaryoda, kodlayıcı hala tek hata noktası işliyor.

- Ayrılmış bir noktasına dağıtmaya her Kodlayıcı ile birden çok kodlayıcılar:

    Bu senaryoda, her iki Kodlayıcı sunar ve artıklık alma. Bu senaryoda, birincil URL'sine encoder1 iter ve ikincil URL'sine encoder2 iter. Bir kodlayıcı başarısız olduğunda, diğer Kodlayıcı veri göndermeye devam. Media Services, aynı zamanda birincil ve ikincil URL'leri kesmez olduğundan veri artıklığı korunabilir. Bu senaryo Kodlayıcıları eşitlenen zamanı ve tam olarak aynı veri sağlayan varsayar.  

- Birincil ve ikincil URL'lere çift Ftp'den birden çok kodlayıcılar:

    Bu senaryoda, her iki kodlayıcılar veri birincil ve ikincil URL'lere iletin. Bu, en iyi güvenilirlik ve hataya dayanıklılık, yanı sıra veri artıklık sağlar. Bu senaryo iki Kodlayıcı hatalarına dayanabilir ve bir kodlayıcı çalışmayı durdurursa bile, bağlantısını keser. Kodlayıcılar eşitlenen zamanı ve tam olarak aynı veri sağlayan varsayar.  

Gerçek zamanlı kodlayıcılar RTMP hakkında daha fazla bilgi için bkz: [Azure Media Services RTMP desteği ve gerçek zamanlı kodlayıcılar](http://go.microsoft.com/fwlink/?LinkId=532824).

#### <a name="ingest-urls-endpoints"></a>Alma URL'leri (Bitiş)
Bir giriş uç noktası bir kanal sağlar (URL alma) Kodlayıcı gönderebilir şekilde, gerçek zamanlı Kodlayıcı akışlar, kanala belirttiğiniz.   

Kanal oluşturduğunuzda alma URL'lerini alabilirsiniz. Bu URL'leri almak için kanal olması gerekmez **çalıştıran** durumu. Veri kanalı dağıtmaya başlamak için hazır olduğunuzda, kanal olmalıdır **çalıştıran** durumu. Veri alma kanal başladıktan sonra akışınızı Önizleme URL yoluyla önizleyebilirsiniz.

Parçalanmış MP4 alanını bir seçeneğiniz bir SSL bağlantısı üzerinden canlı akış (kesintisiz akış). SSL üzerinden alma için alma URL'si için HTTPS güncelleştirdiğinizden emin olun. Şu anda, SSL üzerinden RTMP alma olamaz.

#### <a id="keyframe_interval"></a>Ana kare aralığı
Çoklu bit hızında akışa oluşturmak için bir şirket içi gerçek zamanlı Kodlayıcı kullanırken, ana kare aralığı olarak bu dış Kodlayıcı tarafından kullanılan resimleri (GOP) grubunun süreyi belirtir. Kanal gelen bu akış aldıktan sonra canlı akışınızı istemci kayıttan yürütme uygulamaları aşağıdaki biçimlerden birinde ulaştırabilirsiniz: kesintisiz akış, dinamik Uyarlamalı akış HTTP (DASH) ve HTTP canlı akışı (HLS) üzerinden. Canlı akış yaparken HLS her zaman dinamik olarak paketlenir. Media Services, varsayılan olarak, gerçek zamanlı Kodlayıcı alınan ana kare aralığı göre HLS segment paketleme oranı (kesim başına parça) otomatik olarak hesaplar.

Aşağıdaki tabloda, kesim süresinin nasıl hesaplandığını gösterilmektedir:

| Ana kare aralığı | HLS segment paketleme oranı (FragmentsPerSegment) | Örnek |
| --- | --- | --- |
| 3 saniye küçük veya eşit |3:1 |KeyFrameInterval (veya GOP) 2 saniye varsayılan HLS segment paketleme oranı 3'e 1 ise. Bu, 6-ikinci HLS segment oluşturur. |
| 3 ila 5 saniye |2:1 |KeyFrameInterval (veya GOP) 4 saniye ise, varsayılan HLS segment paketleme 2'ye 1 orandır. Bu 8 saniyelik HLS segment oluşturur. |
| 5 saniyeden büyük |1:1 |KeyFrameInterval (veya GOP) 6 saniye ise, varsayılan HLS segment paketleme 1-1 orandır. Bu, 6-ikinci HLS segment oluşturur. |

Kanalın çıkış yapılandırarak ve FragmentsPerSegment üzerinde ChannelOutputHls ayarlama kesim başına parçaları oranı değiştirebilirsiniz.

Ayrıca, üzerinde ChanneInput KeyFrameInterval özelliği ayarlanarak ana kare aralık değeri değiştirebilirsiniz. Açıkça KeyFrameInterval ayarlarsanız, HLS FragmentsPerSegment daha önce açıklanan kuralları hesaplanır paketleme oranı kesiminde.  

Açıkça KeyFrameInterval ve FragmentsPerSegment ayarlarsanız, Media Services ayarladığınız değerleri kullanır.

#### <a name="allowed-ip-addresses"></a>İzin verilen IP adresi
Bu kanala video yayımlamasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresi aşağıdakilerden biri belirtilebilir:

* Tek bir IP adresi (örneğin, 10.0.0.1)
* Bir IP adresi ve CIDR alt ağ maskesi (örneğin, 10.0.0.1/22) kullanan bir IP aralığı
* Bir IP adresi ve noktalı ondalık alt ağ maskesine (örneğin, 10.0.0.1(255.255.252.0)) kullanan bir IP aralığı

Herhangi bir IP adresi belirtilir ve hiçbir kural tanımı yoksa hiçbir IP adresi izin verilir. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.

### <a name="channel-preview"></a>Kanal Önizleme
#### <a name="preview-urls"></a>Önizleme URL'leri
Kanal Önizleme ve başka bir işleme ve teslim önce akışınızı doğrulamak için kullanacağınız bir önizleme uç noktası (Önizleme URL) sağlar.

Kanal oluşturduğunuzda Önizleme URL'sini alabilirsiniz. URL almak için kanal olması gerekmez **çalıştıran** durumu. Veri alma kanal başladıktan sonra akışınızın önizlemesini.

Şu anda Önizleme akış yalnızca parçalanmış MP4 içinde teslim edilebilir belirtilen giriş türü ne olursa olsun (kesintisiz akış) biçimi. Kullanabileceğiniz [kesintisiz akış sistem durumu İzleyicisi](http://smf.cloudapp.net/healthmonitor) kesintisiz akış test etmek için player. Akışınızı görüntülemek için Azure Portalı'nda barındırılan bir oynatıcı de kullanabilirsiniz.

#### <a name="allowed-ip-addresses"></a>İzin verilen IP adresi
Önizleme uç noktasına bağlanmasına izin verilen IP adreslerini tanımlayabilirsiniz. Herhangi bir IP adresi belirtilmezse, herhangi bir IP adresine izin verilir. İzin verilen IP adresi aşağıdakilerden biri belirtilebilir:

* Tek bir IP adresi (örneğin, 10.0.0.1)
* Bir IP adresi ve CIDR alt ağ maskesi (örneğin, 10.0.0.1/22) kullanan bir IP aralığı
* Bir IP adresi ve noktalı ondalık alt ağ maskesine (örneğin, 10.0.0.1(255.255.252.0)) kullanan bir IP aralığı

### <a name="channel-output"></a>Kanal çıktı
Kanal çıktı hakkında daha fazla bilgi için bkz: [ana kare aralığı](#keyframe_interval) bölümü.

### <a name="channel-managed-programs"></a>Kanal yönetilen programlar
Bir kanal yayımlanması ve depolanmasını Canlı akıştaki kesimleri denetlemek için kullanabileceğiniz programlarla ilişkilidir. Kanallar, programları yönetir. Kanal ve program arasındaki ilişki, burada bir kanal sabit bir içerik akışının bulunduğu ve bir programın bu kanalda zamanlanmış bir olayı kapsamlıdır geleneksel Medyadaki çok benzer.

Program için kaydedilen içeriği kaç saat tutmak istediğinizi **Arşiv Penceresi** uzunluğunu ayarlayarak belirleyebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. Arşiv penceresi uzunluğu, istemcilerin geçerli canlı konumdan zamanda geri gidebilecekleri en uzun süre miktarını da belirler. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her bir program akışlı içerik depolayan bir varlıkla ilişkilidir. Bir varlık, bir Azure depolama hesabı blok blob kapsayıcısında eşlenmiş ve varlık içindeki dosyalara bu kapsayıcıdaki blobları olarak depolanır. Müşterilerinize akış görüntüleyebilmeniz programı yayımlamak için ilişkili varlığa yönelik bir OnDemand Bulucu oluşturmanız gerekir. Bu konum belirleyicisi istemcilerinize sağlayabilen bir akış URL'si oluşturmak için kullanabilirsiniz.

Bir kanal en fazla eşzamanlı çalışan Üçe, destekler, böylece aynı gelen akışın birden çok arşivini oluşturabilirsiniz. Yayımlama ve gerektiğinde bir olayın farklı kısımlarını arşivleyin. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak yalnızca son 10 dakikasını yayınlamak için olduğunu düşünün. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Olay 6 saatini arşivlemek için bir programın ayarlandığından ancak program yayımlanamaz. Diğer program 10 dakika arşivlenecek şekilde ayarlanır ve bu program yayımlanır.

Yeni olaylar için mevcut programları yeniden kullanmamalısınız. Bunun yerine, her olay için yeni bir program oluşturun. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.

Arşivlenen içeriği silmek için durdurmak ve program silin ve ardından ilişkili varlığı silin. Bir program kullanıyorsa, bir varlık silinemiyor. Programın silinmesi gerekir.

Bile durdurma ve program silme sonra varlık silene kadar kullanıcılar, arşivlenen içeriğinizin, isteğe bağlı video olarak akışını sağlayabilirsiniz. Arşivlenen içeriği tutmak ancak sahip değil kullanılabilir akış için isterseniz, akış Bulucuyu silin.

## <a id="states"></a>Kanal durumları ve faturalama
Bir kanal geçerli durumu için olası değerler şunlardır:

* **Durdurulmuş**: Bu ilk kanal oluşturulduktan sonra bir durumda. Bu durumda, kanal özellikleri güncelleştirildi ancak Akış verilmez.
* **Başlangıç**: kanal başlatıldı. Bu durum süresince güncelleştirmelere veya akışa izin verilmez. Bir hata oluşursa, kanal döndürür **durduruldu** durumu.
* **Çalışan**: kanal Canlı akışlar işleyebilir.
* **Durdurma**: kanal durduruldu. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.
* **Silme**: kanal silindi. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.

Aşağıdaki tabloda nasıl kanal Haritası fatura moduna durumlarını gösterir.

| Kanal durumu | Portal UI göstergeleri | Faturalandırılmış mı? |
| --- | --- | --- | --- |
| **Başlatma** |**Başlatma** |Hayır (geçici durum) |
| **Çalışan** |**Hazır** (çalışan program yok)<p><p>or<p>**Akış** (en az bir çalışan program) |Evet |
| **Durdurma** |**Durdurma** |Hayır (geçici durum) |
| **Durduruldu** |**Durduruldu** |Hayır |

## <a id="cc_and_ads"></a>Kapalı açıklamalı alt yazı ve ad ekleme
Desteklenen standartlar kapalı açıklamalı alt yazı ve ad eklemek için aşağıdaki tabloda gösterilmiştir.

| Standart | Notlar |
| --- | --- |
| CEA 708 ve EIA 608 (708/608) |CEA 708 ve EIA 608 kapalı-açıklamalı alt yazı ABD ve Kanada için standartları.<p><p>Şu anda, açıklamalı alt yazı kodlanmış Giriş akışı yalnızca taşınan durumunda desteklenir. 608 veya 708 resim yazıları için Media Services gönderilen kodlanmış akış ekleyebilirsiniz bir canlı Medya Kodlayıcısı kullanmanız gerekir. Media Services, izleyicilere eklenen resim yazıları içerikle sunar. |
| TTML .ismt (metin parçaları kesintisiz akış) içinde |Media Services dinamik paketleme sağlar istemcilerinize akış içerikte aşağıdaki biçimler: DASH, HLS veya kesintisiz akış. Alma, ancak parçalanmış MP4 (kesintisiz akış) .ismt (metin parçaları kesintisiz akış) içinde resim yazıları ile yalnızca kesintisiz akış istemcilere akış sunabilir. |
| SCTE-35 |SCTE-35 reklam ekleme işaret için kullanılan dijital bir sinyal sistemidir. Aşağı Akış alıcıları sinyal reklam akışa ayrılan süre için splice için kullanın. Giriş akışı formatta seyrek bir parçası olarak SCTE-35 gönderilmesi gerekir.<p><p>Şu anda, yalnızca desteklenen Giriş akışı biçimlendirme ad sinyalleri parçalanmış bu taşıyan MP4 (kesintisiz akış). Yalnızca desteklenen çıktı biçimidir da kesintisiz akış. |

## <a id="considerations"></a>Dikkat edilecek noktalar
Çoklu bit hızında akışa bir kanala göndermek için bir şirket içi gerçek zamanlı Kodlayıcı kullanırken aşağıdaki kısıtlamalar geçerlidir:

* Veri alma noktasına göndermek için yeterli boş Internet bağlantısına sahip olduğundan emin olun.
* Alma URL'si ikincil kullanarak ek bant genişliği gerektirir.
* Gelen Çoklu bit hızında akışa en fazla 10 video kalitesini düzeyleri (katman) ve en fazla 5 ses izleri olabilir.
* Herhangi bir video kalitesini düzeyleri için en yüksek ortalama bit hızı 10 MB/sn olmalıdır.
* Tüm video ve ses akışları için ortalama bit toplama 25 MB/sn olmalıdır.
* Kanal giriş protokolünü değiştiremezsiniz veya ilişkili programları çalışmıyor. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Tek bit hızlı, kanalda işleyebilen. Ancak istemci uygulamaları kanal akış işlemez olduğundan, ayrıca tek bit hızlı akış alırsınız. (Bu seçenek öneririz yoktur.)

Çalışma kanalları ve ilgili bileşenlerin ile ilgili diğer konular şunlardır:

* Gerçek zamanlı Kodlayıcı yeniden her zaman, çağrı **sıfırlama** kanalda yöntemi. Kanal sıfırlamadan program durdurmanız gerekir. Kanal sıfırladıktan sonra program yeniden başlatın.
* Bir kanal yalnızca olduğunda durdurulabilir **çalıştıran** durumu ve kanalındaki tüm programlar durduruldu.
* Varsayılan olarak, Media Services hesabınıza yalnızca 5 kanalları ekleyebilirsiniz. Daha fazla bilgi için bkz: [kotaları ve kısıtlamaları](media-services-quotas-and-limitations.md).
* Yalnızca, kanal olduğunda faturalandırılır **çalıştıran** durumu. Daha fazla bilgi için bkz [kanal durumları ve faturalama](media-services-live-streaming-with-onprem-encoders.md#states) bölümü.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="feedback"></a>Geri Bildirim
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Azure Media Services parçalanmış MP4 Canlı belirtimi alma](media-services-fmp4-live-ingest-overview.md)

[Azure Media Services genel bakış ve yaygın senaryolar](media-services-overview.md)

[Media Services kavramları](media-services-concepts.md)

[live-overview]: ./media/media-services-manage-channels-overview/media-services-live-streaming-current.png
