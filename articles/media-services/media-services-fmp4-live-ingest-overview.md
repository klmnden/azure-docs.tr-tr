---
title: "Azure Media Services parçalanmış MP4 Canlı alma belirtimi | Microsoft Docs"
description: "Bu belirtimi protokolü ve parçalanmış MP4 tabanlı canlı akış alımı için Azure Media Services biçimini açıklar. Azure Media Services, canlı olay akışını ve bulut platformu olarak Azure kullanarak gerçek zamanlı içeriği yayını için kullanabilirsiniz. Bu belge, ayrıca en iyi anlatılmaktadır Canlı yüksek oranda yedekli ve sağlam oluşturmaya yönelik yöntemler mekanizmaları alma."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 43fac263-a5ea-44af-8dd5-cc88e423b4de
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 6250b73504bec765b8299060a29e84e771791cc9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services parçalanmış MP4 Canlı belirtimi alma
Bu belirtimi protokolü ve parçalanmış MP4 tabanlı canlı akış alımı için Azure Media Services biçimini açıklar. Media Services müşteriler canlı olay akışını ve bulut platformu olarak Azure kullanarak gerçek zamanlı içeriği yayını için kullanabileceğiniz bir canlı akış hizmeti sağlar. Bu belge, ayrıca en iyi anlatılmaktadır Canlı yüksek oranda yedekli ve sağlam oluşturmaya yönelik yöntemler mekanizmaları alma.

## <a name="1-conformance-notation"></a>1. Uygunluk gösterimi
Anahtar sözcükler ","Değil gerekir,"gerekir" "Gerekli", "SHALL," "GELECEKTİR değil," "SHOULD," "Olmamalıdır," "Önerilen", "Olabilir," ve "İsteğe bağlı" Bu belgeyi olduğu RFC 2119 açıklanan yorumlanacağını.

## <a name="2-service-diagram"></a>2. Hizmet diyagramı
Aşağıdaki diyagramda, Media Services ile canlı akış hizmeti üst düzey mimarisi gösterilmektedir:

1. Gerçek zamanlı Kodlayıcı Canlı akışlar oluşturulan ve sağlanan kanallar Azure Media Services SDK'sı iter.
2. Kanallar, programları ve Media Services akış uç noktalarını alma biçimlendirme dahil olmak üzere tüm canlı akış işlevlerini, işleme, DVR, güvenlik, ölçeklenebilirlik ve artıklık bulut.
3. İsteğe bağlı olarak, müşteriler, Azure içerik teslim ağı katman akış uç noktası ve istemci uç noktaları arasında dağıtmayı seçebilirsiniz.
4. HTTP Uyarlamalı akış protokolleri kullanarak akış uç noktasından istemci uç noktaları akış. Microsoft kesintisiz akış, dinamik Uyarlamalı akış HTTP (tire veya MPEG-DASH) ve Apple HTTP canlı akışı (HLS) üzerinden örnekler.

![Akış alma][image1]

## <a name="3-bitstream-format--iso-14496-12-fragmented-mp4"></a>3. Bitstream biçimi – ISO 14496-12 parçalanmış MP4
Canlı akış alma için gönderim biçimi de ele alınan bu belgeyi [ISO-14496-12] dayanır. Ayrıntılı açıklaması parçalanmış MP4 biçimindeki ve hem uzantıları için isteğe bağlı video dosyaları ve canlı akış alımı için bkz: [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx).

### <a name="live-ingest-format-definitions"></a>Canlı biçim tanımlarını alma
Aşağıdaki listede, Azure Media Services'e Canlı uygulamak tanımları alma özel biçim açıklanmaktadır:

1. **Ftyp**, **Canlı sunucusu bildirim kutusu**, ve **moov** her istek (HTTP POST) ile kutuları gönderilir. Bu kutuları akış başında gönderilmelidir ve alma dilediğiniz zaman Kodlayıcı akışı sürdürmeyi yeniden bağlanmanız gerekir. Daha fazla bilgi için bkz: Bölüm 6 [1].
2. [1] 3.3.2 bölümünde tanımlar adlı isteğe bağlı bir kutu **StreamManifestBox** için Canlı alma. Azure yük dengeleyici yönlendirme mantığından dolayı bu kutusunu kullanarak kullanım dışıdır. Kutunun olmamalıdır Media Services'e alındıktan olduğunda mevcut. Bu kutu mevcut değilse, Media Services sessizce onu yok sayar.
3. **TrackFragmentExtendedHeaderBox** 3.2.3.2 [1] içinde tanımlı kutusunu her parçası için mevcut olmalıdır.
4. Sürüm 2 **TrackFragmentExtendedHeaderBox** kutusunu, aynı URL'lere sahip birden çok veri merkezlerinde ortam kesimleri oluşturmak için kullanılmalıdır. Parça dizini dizin tabanlı akış biçimlerine Apple HLS gibi çapraz veri merkezi yük devretme için gerekli ve dizin tabanlı MPEG-DASH alanıdır. Çapraz veri merkezi yük devretme etkinleştirmek için parça dizin arasında birden çok kodlayıcılar eşitlenen gerekir ve her art arda medya parçası için 1 tarafından bile Kodlayıcı yeniden başlatmalar veya hatalar arasında artırılması.
5. [1] 3.3.6 bölümünde tanımlar adlı bir kutusu **MovieFragmentRandomAccessBox** (**mfra**), gönderilebilir Canlı alım sonunda kanala akış uç (EOS) belirtmek için. Media Services alma mantığından dolayı EOS kullanarak kullanım dışı bırakılmıştır ve **mfra** Canlı alım gönderilmez için kutusu. Gönderilen, Media Services sessizce onu yok sayar. Alma noktası durumunu sıfırlamak için kullanmanız önerilir [kanal sıfırlama](https://docs.microsoft.com/rest/api/media/operations/channel#reset_channels). Ayrıca, kullanmanızı öneririz [programı durdurun](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) sunu ve akış sona erdirmek için.
6. MP4 parça süresi istemci bildirimleri boyutunu azaltmak için sabit olmalıdır. İstemci Yükleme buluşsal yöntemler kullanılarak parça süresi de geliştirir sabit bir MP4 etiketleri yineleyin. Süre için tamsayı olmayan çerçeve hızlarını dengelemek için dalgalanma.
7. MP4 parça süresi yaklaşık 2-6 saniye arasında olmalıdır.
8. MP4 parçalara zaman damgaları ve dizinleri (**TrackFragmentExtendedHeaderBox** `fragment_ absolute_ time` ve `fragment_index`) artan düzende ulaşması. Media Services yinelenen parçaları esnek olsa da, parçaları medya zaman çizelgesi göre yeniden sıralamak için yeteneği sınırlıdır.

## <a name="4-protocol-format--http"></a>4. Protokol biçimi – HTTP
ISO parçalanmış MP4 tabanlı Canlı Media Services hizmetine parçalanmış MP4 biçimindeki paketlenmiş kodlanmış medya verilerini iletmek için bir standart uzun süre çalışan HTTP POST isteği kullanan alma. Her HTTP POST tamamı parçalanmış MP4 bitstream ("akış"), üstbilgi kutuları ile en başından başlayarak gönderir (**ftyp**, **Canlı sunucusu bildirim kutusu**, ve **moov** kutuları ) ve parçaları bir dizi devam (**moof** ve **mdat** kutuları). HTTP POST isteği için URL sözdizimi için 9.2 in [1] bölümüne bakın. Gönderme URL'sini örneğidir: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

### <a name="requirements"></a>Gereksinimler
Ayrıntılı gereksinimler şunlardır:

1. Kodlayıcı yayın aynı alım URL'yi kullanarak bir boş "body" (içerik uzunluğu sıfır) ile bir HTTP POST isteği göndererek başlamanız gerekir. Bu, hızlı bir şekilde tespit Canlı alım uç noktanın geçerli olup olmadığını ve herhangi bir kimlik doğrulaması ya da gereken diğer koşulları varsa Kodlayıcı yardımcı olabilir. HTTP protokolü sunucu POST gövde dahil olmak üzere tüm istek alınana kadar bir HTTP yanıtının geri gönderemez. Bu adım olmadan canlı bir olay uzun süre çalışan yapısını verilen Kodlayıcı tüm verileri gönderme sonlanana kadar tüm hata algılayabilir olmayabilir.
2. Kodlayıcı herhangi bir hata veya kimlik doğrulama sınaması (1) nedeniyle işlemesi gerekir. Varsa (1) başarılı bir 200 yanıt ile devam edin.
3. Kodlayıcı yeni bir HTTP POST isteği parçalanmış MP4 akışı ile başlamalıdır. Yükü parçaları tarafından izlenen üstbilgi kutuları ile başlamalıdır. Unutmayın **ftyp**, **Canlı sunucusu bildirim kutusu**, ve **moov** (bu sırayla) kutuları gönderileceği her bir istekle Kodlayıcı, çünkü yeniden bağlanmanız gerekir olsa bile önceki İstek akışın sonuna önce sonlandırıldı. 
4. Kodlayıcı öbekli aktarım yüklemek için canlı olay tüm içerik uzunluğu tahmin etmek mümkün olduğundan kodlaması kullanmanız gerekir.
5. Olay üzerinden, son parça gönderdikten sonra olduğunda Kodlayıcı düzgün biçimde öbekli aktarım (çoğu HTTP istemci yığınları, otomatik olarak işlemek) ileti sırası kodlaması bitmelidir. Kodlayıcı hizmetinin son yanıt kodu dönmek için bekleyin ve ardından bağlantıyı sonlandırmasına. 
6. Kodlayıcı kullanmamasını gerekir `Events()` 9.2 in [1] Media Services içine Canlı alımı için açıklanan isim.
7. HTTP POST isteği sonlandırır veya TCP hata akışın sonuna önce zaman aşımına uğradı, kodlayıcı gereken yeni bir bağlantı kullanarak yeni bir POST isteği gönderin ve yukarıdaki gereksinimleri izleyin. Ayrıca, kodlayıcı gerekir akıştaki her izlemek için önceki iki MP4 parçasının yeniden ve medya zaman çizelgesinde süreksizlik oluşturmaksızın Sürdür. Son iki MP4 parçaları her izlemek için yeniden göndermeyi veri kaybı olmasını sağlar. Diğer bir deyişle, ses ve video izleme bir akış içerir ve geçerli POST isteği başarısız olursa, kodlayıcı gerekir yeniden ve ses izlemek için daha önce başarıyla gönderildi, son iki parça ve video için son iki parçalarını yeniden gönderin , hangi daha önce başarıyla, veri kaybı olduğundan emin olmak için gönderilen izler. Kodlayıcı bağlandığında, onu yeniden gönderir medya parçasının "İleri" bir arabellek bulundurmanız gerekir.

## <a name="5-timescale"></a>5. Zaman Çizelgesi
[[MS-SSTR] ](https://msdn.microsoft.com/library/ff469518.aspx) ilişkin zaman ölçeğini kullanımını açıklar **SmoothStreamingMedia** (Bölüm 2.2.2.1) **StreamElement** (Bölüm 2.2.2.3) **StreamFragmentElement**(2.2.2.6 bölüm) ve **LiveSMIL** (Bölüm 2.2.7.3.1). Ölçeği değer yoksa, kullanılan varsayılan değeri 10,000,000 (10 MHz) kullanılır. Kesintisiz akış biçim belirtimi diğer ölçeği değerleri kullanımını engellemez rağmen çoğu Kodlayıcı uygulamaları bu varsayılan kullanmak değeri (MHz 10 kesintisiz akış oluşturmak için) veri alın. Nedeniyle [Azure medya dinamik paketleme](media-services-dynamic-packaging-overview.md) özelliği, öneririz, 90 KHz ölçeği video akışları ve 44,1 KHz veya 48.1 KHz ses akışları için kullanmanızı. Farklı bir zaman ölçeğine göre değerleri için farklı akışları kullandıysanız, akış düzeyi ölçeği gönderilmelidir. Daha fazla bilgi için bkz: [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

## <a name="6-definition-of-stream"></a>6. "Akış" tanımı
Akış işleme akış yük devretme ve artıklık senaryoları temel Canlı sunuları oluşturma için Canlı alım işlemde birimidir. Akış, tek bir parça içerebilecek bir benzersiz, parçalanmış MP4 bitstream veya birden çok parçaları tanımlanır. Tam canlı sunu gerçek zamanlı kodlayıcılar yapılandırmasına bağlı olarak bir veya daha fazla akışlar içerebilir. Aşağıdaki örneklerde tam canlı sunu oluşturmak için akışlar kullanmanın çeşitli seçenekler gösterilmektedir.

**Örnek:** 

Bir müşteri aşağıdaki ses/video bit içeren bir canlı akış sunumu oluşturmak ister:

Video – 3000 KB/sn, 1500 KB/sn, 750 KB/sn

Ses – 128 Kb/sn

### <a name="option-1-all-tracks-in-one-stream"></a>Seçenek 1: Tüm parçaları bir akış
Bu seçenek, tek bir kodlayıcı tüm ses/video parçalar oluşturur ve bunları bir parçalanmış MP4 bitstream sunmaktadır. Parçalanmış MP4 bitstream tek bir HTTP POST bağlantısı üzerinden gönderilir. Bu örnekte, bu canlı sunu için yalnızca bir akış yoktur.

![Akışlar bir izleme][image2]

### <a name="option-2-each-track-in-a-separate-stream"></a>Seçenek 2: Ayrı akıştaki her izleme
Bu seçenek Kodlayıcı bir izleme her parça MP4 bitstream yerleştirir ve tüm akışlar ayrı HTTP bağlantıları üzerinden gönderir. Bu, birden çok kodlayıcılar veya bir kodlayıcı ile yapılabilir. Canlı alım bu canlı sunu dört akışları olarak görür.

![Akışlar ayrı izler][image3]

### <a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Seçenek 3: Paket ses izleme bir akışa düşük bit hızlı video İzle
Bu seçenek, bir parça MP4 bitstream düşük bit hızlı video kanalı ile ses izleme paketini ve diğer iki video parçaları olarak ayrı akışları bırakmak müşteri seçer. 

![Akışlar ses ve video izler][image4]

### <a name="summary"></a>Özet
Bu örneğin tüm olası alım seçenekleri kapsamlı bir listesi değil. Gibi bir matter, hatta herhangi bir gruplama akışları içine parçalarının Canlı alım tarafından desteklenir. Müşteriler ve Kodlayıcı satıcılar mühendislik karmaşıklık, kodlayıcı kapasite ve artıklık ve yük devretme konuları göre kendi uygulamaları seçebilirsiniz. Ancak, çoğu durumda, tüm canlı sunu için yalnızca bir ses izleme yoktur. Bu nedenle, ses parçası içeren alma akış healthiness emin olmak önemlidir. Bu faktör genellikle kendi akış (seçenek 2) olduğu gibi ses izleme almadan veya en düşük bit hızlı video İzle (olduğu gibi seçeneği 3) ile paketleme sonuçlanır. Ayrıca, daha iyi yedeklik ve hataya dayanıklılık için aynı ses izleme iki farklı akışlar (yedekli ses izleri seçeneği 2) içinde gönderirken ya da ses izleme en az iki düşük bit hızlı video izler (seçenek 3 en az iki paketlenmiş sesle paketleme video akışları) için Canlı önerilir medya Hizmetleri içine alın.

## <a name="7-service-failover"></a>7. Hizmet yük devretmesi
Canlı akış yapısı, iyi yük devretme desteği hizmetin kullanılabilirliğini sağlamak için önemlidir. Media Services, ağ hataları, sunucu hataları ve depolama sorunlarını hataları gibi çeşitli türlerde işlemek için tasarlanmıştır. Müşteriler, uygun yük devretme mantığı gerçek zamanlı Kodlayıcı taraftaki ile birlikte kullanıldığında, yüksek oranda güvenilir bir canlı akış hizmetine bulutta elde edebilirsiniz.

Bu bölümde, hizmet bir yük devretme senaryosu açıklanmaktadır. Bu durumda hizmet içinde herhangi bir yerde başarısız olur ve bir ağ hatası çıkmaktadır. Hizmet yük devretmesi işlemek için Kodlayıcı uygulaması için bazı öneriler şunlardır:

1. 10 saniye zaman aşımını TCP bağlantısı kurmak için kullanın. Bağlantı kurma girişimi 10 saniyeden uzun sürerse, işlemi durdurmak ve yeniden deneyin. 
2. Kısa bir zaman aşımı iletisi öbekleri HTTP isteği göndermek için kullanın. Hedef MP4 parça süresi N saniye ise, N ve 2 N saniye arasında gönderilen zaman aşımı kullanın; Örneğin, MP4 parça süresi 6 saniye ise, 6 ila 12 saniyelik zaman aşımı kullanın. Zaman aşımı meydana gelirse, bağlantıyı sıfırlar, yeni bir bağlantı açmak ve akış sürdürme yeni bağlantıda alma. 
3. Başarılı bir şekilde ve tamamen hizmete gönderilen son iki parça her parçasının sahip çalışırken bir arabellek korur.  Bir akış için HTTP POST isteği sonlandırılır veya öncesinde zaman aşımına akışın sonuna yeni bir bağlantı açmak ve başka bir HTTP POST isteği başlatmak, akış üstbilgileri yeniden, her izleme için son iki parça yeniden ve d oluşturmaksızın akış Sürdür medya zaman çizelgesinde iscontinuity. Bu, veri kaybı olasılığını azaltır.
4. Kodlayıcı bağlantı kurun veya TCP hata oluştuktan sonra akış sürdürmek için yeniden deneme sayısını sınırlamaz öneririz.
5. TCP hatasından sonra:
  
    a. Geçerli bağlantının kapatılması gerekir ve yeni bir bağlantı için yeni bir HTTP POST isteği oluşturulması gerekir.

    b. Yeni HTTP POST URL ilk gönderme URL'SİYLE aynı olması gerekir.
  
    c. Yeni HTTP POST içermelidir akış üstbilgileri (**ftyp**, **Canlı sunucusu bildirim kutusu**, ve **moov** kutuları), ilk POST akış üstbilgilerinde aynı.
  
    d. Her parça için gönderilen son iki parça gönderilmesi gerekir ve akış medya zaman çizelgesinde süreksizlik oluşturmaksızın çıkmalıdır. MP4 parça zaman damgaları sürekli olarak bile HTTP POST istekleri artırmanız gerekir.
6. MP4 parça süresiyle orantılı bir hızda veri gönderilmiyor varsa Kodlayıcı HTTP POST isteği sonlanmalıdır.  Veri göndermediğini bir HTTP POST isteği Media Services hızlı bir şekilde kodlayıcıdan bir hizmet güncelleştirmesi durumunda bağlantıyı kesme gelen engelleyebilir. Bu nedenle, HTTP POST için seyrek (ad sinyali) parçaları olmalıdır kısa süreli seyrek parça gönderilen hemen sonlandırılıyor.

## <a name="8-encoder-failover"></a>8. Kodlayıcı yük devretme
Kodlayıcı yük devretme için uçtan uca canlı akış halinde teslim ele alınması gereken yük devretme senaryosu ikinci türüdür. Bu senaryoda, hata durumu Kodlayıcı tarafında gerçekleşir. 

![Kodlayıcı yük devretme][image5]

Kodlayıcı yük devretme gerçekleştiğinde aşağıdaki beklentilerini Canlı alım uç noktasından Uygula:

1. Yeni bir kodlayıcı örneği (3000 k kesikli çizgi ile video için akış) diyagramda gösterildiği gibi akış, devam etmek için oluşturulmalıdır.
2. Yeni Kodlayıcı aynı URL için HTTP POST istekleri başarısız örneği olarak kullanmanız gerekir.
3. Yeni Kodlayıcı'nın POST isteği başarısız örnekle aynı parçalanmış MP4 üstbilgi kutuları eklemeniz gerekir.
4. Yeni Kodlayıcı hizalanmış parça sınırlarla eşitlenen ses/video örnekleri oluşturmak için tüm diğer çalışan kodlayıcılar aynı Canlı sunum ile düzgün olarak eşitlenmedi gerekir.
5. Yeni akış önceki akışıyla anlam olarak eşdeğer olmalıdır ve üstbilgi ve parça düzeylerinde değiştirilebilir.
6. Yeni Kodlayıcı, veri kaybını en aza indirmek denemelisiniz. `fragment_absolute_time` Ve `fragment_index` ortam parçaları Kodlayıcı son durduğu noktasından artırmanız gerekir. `fragment_absolute_time` Ve `fragment_index` sürekli bir biçimde artırmanız gerekir, ancak, gerekirse, süreksizlik tanıtmak için izin verilir. Medya zaman çizelgesinde discontinuities tanıtmak üzere daha parçaları yeniden göndermeyi yan tarafında hata daha iyidir şekilde Media Services, daha önce alındı ve işlenen, parça yok sayar. 

## <a name="9-encoder-redundancy"></a>9. Kodlayıcı artıklık
Bazı önemli olayları isteğe bağlı daha yüksek kullanılabilirlik ve kaliteli bir deneyim, veri kaybı olmadan sorunsuz yük devretme elde etmek için aktif-aktif yedekli kodlayıcılar kullanmak önerilir, dinamik.

![Kodlayıcı artıklık][image6]

Bu diyagramda gösterildiği gibi kodlayıcılar iki grupları her akış iki kopyasını Canlı hizmetinde aynı anda iletin. Media Services akışı kimliği ve parçası zaman damgasını göre yinelenen parçaları filtrelemenize çünkü bu kurulumu desteklenir. Sonuçta elde edilen canlı akış ve Arşiv iki kaynaklardan gelen en iyi olası toplama olan tek bir kopyasını tüm akışlar olur. Örneğin, durumda bir kuramsal aşırı yoktur (adla aynı olması gerekmez) bir kodlayıcı sürece belirli bir anda her akış için zaman içinde sonuçta ortaya çıkan bir canlı akışı hizmetinden veri kaybı olmadan sürekli çalışıyor. 

Bu senaryo için gereksinimler neredeyse gereksinimleri "Kodlayıcı yük devretme" durumda, ikinci kodlayıcılar kümesi çalıştıran birincil kodlayıcılar aynı zamanda özel durumu ile aynıdır.

## <a name="10-service-redundancy"></a>10. Hizmet artıklık
Yüksek oranda yedekli genel dağıtım için bazen bölgesel afetler işlemek için çapraz bölge yedekleme olması gerekir. "Kodlayıcı artıklık" topolojisine genişleterek, müşterilerin yedekli hizmet dağıtımı kodlayıcılar ikinci kümesiyle bağlı farklı bir bölgede sahip olmayı seçebilirsiniz. Müşteriler, aynı zamanda genel trafik sorunsuz bir şekilde istemci trafiğini yönlendirmek için Yöneticisi önünde iki hizmet dağıtımları dağıtmak için içerik teslim ağı sağlayıcı ile çalışabilirsiniz. Kodlayıcılar gereksinimleri "Kodlayıcı artıklık" servis talebi ile aynıdır. Farklı bir canlı yönlendirilmesi için kodlayıcılar gereksinimlerini ikinci kümesi uç noktasını alın tek özel durumdur. Aşağıdaki diyagramda, bu kurulum gösterilmektedir:

![Hizmet artıklık][image7]

## <a name="11-special-types-of-ingestion-formats"></a>11. Özel türde alım biçimleri
Bu bölümde belirli senaryolar işlemek üzere tasarlanmış Canlı alım biçimlerinin özel türleri açıklanmaktadır.

### <a name="sparse-track"></a>Seyrek İzle
Bir zengin istemci deneyimi ile canlı akış sunu genellikle, zaman eşitlenmiş olayları iletmek gerekli olan veya bant dışı ana medya verilerle işaret eder. Bu dinamik Canlı reklam ekleme örneğidir. Bu tür olay sinyal seyrek doğası nedeniyle akış normal ses/video farklıdır. Diğer bir deyişle, sinyal verileri genellikle sürekli olarak gerçekleşmez ve aralığı tahmin etmek zor olabilir. Seyrek izleme kavramı, alma ve bant dışı sinyal veri yayını için tasarlanmıştır.

Aşağıdaki adımlar seyrek izleme alma için önerilen uygulama şunlardır:

1. Ses/video parçaları olmadan yalnızca seyrek parçaları içeren ayrı bir parçalanmış MP4 bitstream oluşturun.
2. İçinde **Canlı sunucusu bildirim kutusu** [1] Bölüm 6'tanımlandığı gibi kullanmak *parentTrackName* üst izleme adını belirtmek için parametre. [1] 4.2.1.2.1.2 bölümünde daha fazla bilgi için bkz.
3. İçinde **Canlı sunucusu bildirim kutusu**, **manifestOutput** ayarlanmalıdır **doğru**.
4. Sinyal Olayı seyrek yapısını verildiğinde, aşağıdakiler önerilir:
   
    a. Canlı olay başında Kodlayıcı ilk üstbilgi kutuları istemci bildiriminde seyrek izleme kaydedilecek hizmet veren hizmetine gönderir.
   
    b. Veri gönderilmiyor zaman Kodlayıcı HTTP POST isteği sonlanmalıdır. Veri göndermediğini uzun süre çalışan HTTP POST Media Services hızlı bir şekilde kodlayıcıdan hizmet güncelleştirmesi veya sunucu yeniden başlatma durumunda bağlantıyı kesme gelen engelleyebilir. Bu durumlarda, ortam sunucusu yuvada alma işleminde geçici olarak engellendi.
   
    c. Sinyal veri kullanılabilir olmadığında olduğu süre boyunca SHOULD Kodlayıcı Kapat HTTP POST isteği. POST isteğini etkinken Kodlayıcı veri göndermesi gerekir.

    d. Kullanılabilir durumdaysa Kodlayıcı seyrek parçaları gönderirken, açık bir content-length üstbilgisi ayarlayabilirsiniz.

    e. Yeni bir bağlantıyla seyrek parçaları gönderirken Kodlayıcı tarafından yeni parçaları ardından üstbilgi kutularından gönderme başlamanız gerekir. Bu, yük devretme ortası olur ve seyrek izleme önce görmediği yeni bir sunucu için yeni seyrek bağlantı kuruldu durumlarda içindir.

    f. Eşit veya daha büyük bir zaman damgası değeri karşılık gelen üst parça parça istemciye kullanılabilir olduğunda seyrek parça parça istemciye kullanılabilir hale gelir. Bir zaman damgası t seyrek parça varsa, örneğin, = 1000, beklenen, "video" (üst izleme adı "video" olduğunu varsayarak) parçalara zaman damgası 1000 veya sonrasını, seyrek parça t indirebilirsiniz istemci görür sonra = 1000. Gerçek sinyal sunu zaman çizelgesi farklı bir konuma için belirlenen amacı için kullanılabilir olduğunu unutmayın. Bu örnekte, mümkündür, seyrek parça t = 1000 olan birkaç saniye olan bir konuma bir ad eklemek için bir XML yükü daha sonra.

    g. Seyrek parça parça yükü senaryoya bağlı olarak (örneğin, XML, metin veya ikili), farklı biçimlerde olabilir.

### <a name="redundant-audio-track"></a>Yedek ses İzle
Tipik bir HTTP Uyarlamalı akış senaryoda (örneğin, kesintisiz akış veya tire), genellikle var. tüm sunuya yalnızca bir ses İzle Ses parçası içeren akış alımı bozuk ise hata koşulları seçmek istemcinin birden çok kalitesi düzeylerini olan video parçaları tek hata noktası ses parçası olabilir. 

Bu sorunu çözmek için yedek ses izleri, Canlı alım Media Services destekler. Birden çok kez farklı akış gönderilebilir aynı ses parçası olur. Hizmet yalnızca ses izleme kez istemci bildirimini kaydeder rağmen birincil ses izleme sorunlar varsa, ses parçaları almak için bunu yedekli ses izleri yedek olarak kullanabilirsiniz. Yedek ses izleri alma için Kodlayıcı gerekir:

1. Birden çok parçadaki MP4 bitstreams aynı ses kaydı oluşturun. Yedek ses izleri ile aynı parça zaman damgaları anlam olarak eşdeğer ve üstbilgi ve parça düzeylerinde değiştirilebilir.
2. Canlı sunucu bildirimi (Bölüm 6 [1].) "ses" girdisinde tüm gereksiz ses izleri için aynı olduğundan emin olun.

Aşağıdaki uygulama için yedek ses izleri önerilir:

1. Her benzersiz ses izleme tek başına bir akışa gönderin. Ayrıca, her burada ikinci akış farklı birinciden bu ses izleme akışlar için yedek bir akış Gönder yalnızca HTTP POST URL'de tanımlayıcısı tarafından: {protokol} :// {sunucu adresi} / {noktası path}/Streams({identifier}) yayımlama.
2. İki düşük video bit göndermek için ayrı akışları kullanın. Her bu akışlar benzersiz her ses izleme bir kopyasını da içermelidir. Örneğin, birden fazla dili desteklendiğinde, bu akışları her dil için ses izleri içermelidir.
3. Kodlamak ve belirtilen yedek akışlar (1) ve (2) göndermek için ayrı bir sunucu (kodlayıcı) örneği kullanın. 

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
