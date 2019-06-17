---
title: Azure Media Services bölünmüş MP4 Canlı alma belirtimi | Microsoft Docs
description: Bu belirtim protokolü ve parçalanmış MP4 tabanlı canlı akış alımı için Azure Media Services için biçimini açıklar. Canlı etkinlik akışı ve bulut platformu olarak Azure'u kullanarak gerçek zamanlı olarak içerik yayın için Azure Media Services'ı kullanabilirsiniz. Bu belge, ayrıca en iyi anlatılmaktadır Canlı düzeyde yedekli ve sağlam oluşturmaya yönelik yöntemler alma mekanizmaları.
services: media-services
documentationcenter: ''
author: cenkdin
manager: femila
editor: ''
ms.assetid: 43fac263-a5ea-44af-8dd5-cc88e423b4de
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: cenkd;juliako
ms.openlocfilehash: b3357436d068396c5c3c4fae10ed6857759c5aed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61221355"
---
# <a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services bölünmüş MP4 Canlı içe alma belirtimi 

Bu belirtim protokolü ve parçalanmış MP4 tabanlı canlı akış alımı için Azure Media Services için biçimini açıklar. Media Services müşterileri, etkinliklerin canlı akış ve bulut platformu olarak Azure'u kullanarak gerçek zamanlı olarak içerik yayın için kullanabileceğiniz bir canlı akış hizmeti sağlar. Bu belge, ayrıca en iyi anlatılmaktadır Canlı düzeyde yedekli ve sağlam oluşturmaya yönelik yöntemler alma mekanizmaları.

## <a name="1-conformance-notation"></a>1. Uygunluk gösterimi
","NOT gerekir,"ZORUNDASINIZ" "Gerekli,"SHALL,"" "GELECEKTİR değil," "SHOULD," "Olmamalıdır," anahtar sözcükler "Önerilen,"Mayıs,"" ve "İsteğe bağlı" Bu belgeyi olduğu RFC 2119 açıklandığı yorumlanacağını.

## <a name="2-service-diagram"></a>2. Hizmet diyagramı
Aşağıdaki diyagramda, Media Services canlı akış hizmeti üst düzey mimarisi gösterilmektedir:

1. Gerçek zamanlı bir kodlayıcı Canlı akışlar oluşturulan ve sağlanan kanalları Azure Media Services SDK'sını iter.
1. Kanallar, programlar ve Media Services akış uç noktalarını alma, biçimlendirme dahil olmak üzere tüm canlı akış işlevlerini, bulut DVR, güvenlik, ölçeklenebilirlik ve yedeklilik.
1. İsteğe bağlı olarak, müşteriler, bir akış uç noktası ve istemci uç noktaları arasında Azure Content Delivery Network katmanı dağıtmayı tercih edebilirsiniz.
1. HTTP Uyarlamalı akış protokolleri kullanarak akış uç noktasından istemci uç noktaları akış. Microsoft kesintisiz akış, Dynamic Adaptive Streaming HTTP (DASH veya MPEG-DASH) ve Apple HTTP canlı akış (HLS) üzerinden verilebilir.

![Akış içe alma][image1]

## <a name="3-bitstream-format--iso-14496-12-fragmented-mp4"></a>3. Bitstream biçimi – ISO 14496-12 parçalanmış MP4
Canlı akış içe alma için kablo biçimini makalesinde ele alındığı bu belgeyi [ISO-14496-12] temel alır. Ayrıntılı bir açıklama parçalanmış MP4 biçimi ve hem uzantıları için isteğe bağlı video dosyaları ve canlı akış alımı için bkz: [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).

### <a name="live-ingest-format-definitions"></a>Canlı biçim tanımlarını alma
Aşağıdaki liste, Azure Media Services'e Canlı uygulamak tanımlarını içe alma, özel biçim açıklar:

1. **Ftyp**, **Canlı sunucusu bildirim kutusu**, ve **moov** kutuları her bir istekle (HTTP POST) gönderilmelidir. Bu kutular akış başlangıcında gönderilmelidir ve alma dilediğiniz zaman kodlayıcıdan akışı sürdürmek için yeniden bağlanmanız gerekir. Daha fazla bilgi için 6. Bölüm [1] konusuna bakın.
1. [1] 3.3.2 bölümünde tanımlar adlı isteğe bağlı bir kutu **StreamManifestBox** için Canlı alma. Azure load balancer yönlendirme mantığından dolayı bu kutusunu kullanarak kullanım dışı bırakılmıştır. Kutunun Media Services'e alındıktan olduğunda mevcut olmamalıdır. Bu kutuyu varsa, Media Services sessizce yoksayar.
1. **TrackFragmentExtendedHeaderBox** kutusu 3.2.3.2 [1] içinde tanımlanan her parça için mevcut olmalıdır.
1. 2\. sürümünü **TrackFragmentExtendedHeaderBox** kutusu aynı URL birden çok veri merkezlerinde sahip ortam kesimleri oluşturmak için kullanılması gerekir. Parça dizini gibi Apple HLS akış biçimlerinde dizin tabanlı veri merkezli yük devretmesi için gerekli ve MPEG-DASH dizin tabanlı bir alandır. Veri merkezli yük devretmeyi etkinleştirmek için parça dizin arasında birden çok kodlayıcılar eşitlenmesi gerekir ve her art arda gelen medya parça 1 hatta Kodlayıcı yeniden başlatma veya hatalar arasında artırılması.
1. [1] 3.3.6 bölümünde tanımlar olarak adlandırılan bir kutu **MovieFragmentRandomAccessBox** (**mfra**), gönderilebilir Canlı alma sonunda kanala akış sonu (EOS) göstermek için. Media Services'ın alma mantığından dolayı EOS kullanarak kullanım dışıdır ve **mfra** Canlı alma gönderilmez için kutusu. Gönderirse, Media Services sessizce yoksayar. Alma noktası durumunu sıfırlamak için kullanmanızı öneririz [kanalı sıfırlamak](https://docs.microsoft.com/rest/api/media/operations/channel#reset_channels). Ayrıca kullanmanızı öneririz [programı durdurun](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) sunu ve akış sonuna.
1. MP4 parça süresi, istemci bildirimleri boyutunu azaltmak için sabit olmalıdır. Ayırma süresi istemci indirme buluşsal yöntemlerini kullanarak da artırır. sabit bir MP4 etiketleri yineleyin. Süre, tamsayı olmayan kare hızları için dengelemek için seyredebilir.
1. MP4 parça süresi yaklaşık 2-6 saniye arasında olmalıdır.
1. MP4 parçalara zaman damgaları ve dizinleri (**TrackFragmentExtendedHeaderBox** `fragment_ absolute_ time` ve `fragment_index`) artan düzende ulaşacak. Media Services için yinelenen parçaları dayanıklı olsa da, bu parçaları bir medya zaman çizelgesi göre yeniden sıralamak için yeteneği sınırlıdır.

## <a name="4-protocol-format--http"></a>4. HTTP protokolü biçimindeki
ISO parçalanmış MP4 tabanlı Canlı Media Services hizmetine parçalanmış MP4 biçiminde paketlenmiştir kodlanmış medya veri aktarmak için bir standart uzun süren HTTP POST isteği kullanan alma. Her HTTP POST eksiksiz bir parçalanmış MP4 bitstream ("stream"), üstbilgi kutuları ile baştan başlayarak gönderir (**ftyp**, **Canlı sunucusu bildirim kutusu**, ve **moov** kutuları ) ve parçaları bir dizi (**moof** ve **mdat** kutuları). HTTP POST isteği için URL sözdizimi için bölümüne 9.2 in [1] bakın. Gönderme URL'sini örneğidir: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

### <a name="requirements"></a>Gereksinimler
Ayrıntılı gereksinimler şunlardır:

1. Kodlayıcı, yayın aynı alımı URL'yi kullanarak boş "gövdesi" (içerik uzunluğu sıfır) ile bir HTTP POST isteği göndererek başlamanız gerekir. Bu, Canlı alma uç noktanın geçerli olup olmadığını ve herhangi bir kimlik doğrulaması veya gereken başka koşullar varsa hızlı bir şekilde algılayın Kodlayıcı yardımcı olabilir. Her HTTP protokolü, sunucunun POST gövdesini dahil olmak üzere tüm istek alınana kadar bir HTTP yanıtının geri gönderemez. Uzun süre çalışan yapısı gereği Bu adım olmadan canlı bir etkinlik Kodlayıcı tüm verileri gönderen bitene kadar herhangi bir hata algılamak mümkün olmayabilir.
1. Kodlayıcı (1) nedeniyle herhangi bir hata veya kimlik doğrulama sınaması işlemesi gerekir. Varsa (1) başarılı bir 200 yanıt ile devam edin.
1. Kodlayıcı, yeni bir HTTP POST isteği parçalanmış MP4 akışı ile başlamalıdır. Yükü parçalar tarafından izlenen üst bilgisi kutuları ile başlamalıdır. Unutmayın **ftyp**, **Canlı sunucusu bildirim kutusu**, ve **moov** kutuları (bu sırayla) Kodlayıcı, çünkü yeniden bağlanmanız gerekir olsa bile her bir istekle gönderilmelidir önceki Akışın sonuna önce istek sona erdirildi. 
1. Kodlayıcı Canlı etkinlik tüm içerik uzunluğu tahmin etmek mümkün olduğu için karşıya yükleme için öbekli aktarım kodlamasını kullanmanız gerekir.
1. Olay, son parça gönderdikten sonra bittiğinde Kodlayıcı (çoğu HTTP istemci yığınları, otomatik olarak işler) ileti sırası öbekli aktarım kodlamasını düzgün bir şekilde bitmelidir. Kodlayıcı hizmetinin son yanıt kodu iade için bekleyin ve ardından bağlantıyı sonlandırın. 
1. Kodlayıcı gerekir kullanım `Events()` 9.2 in [1] için Media Services içine Canlı alma açıklandığı isim.
1. HTTP POST isteği sonlandırır veya bir TCP hatası akış sonundan önce zaman aşımına Kodlayıcı gereken yeni bir bağlantı kullanarak yeni bir POST isteği yayımlamanız ve önceki gereksinimleri izleyin. Ayrıca, kodlayıcı gerekir akıştaki her parça için önceki iki MP4 parçaları yeniden ve medya zaman çizelgesi süreksizlik oluşturmaksızın Sürdür. Her parça için son iki MP4 parçaları yeniden göndermeyi veri kaybı olmadan olmasını sağlar. Diğer bir deyişle, bir ses ve video izleme hem bir akış içeriyor, ve geçerli bir POST isteği başarısız olursa, kodlayıcı yeniden ve gerekir, daha önce başarıyla gönderilen son iki parçaları için ses kaydının ve video için son iki parçalarını yeniden gönder , izleme, daha önce başarıyla, veri kaybı olduğundan emin olmak için gönderildi. Kodlayıcı bağlandığında, bu beşe medya parçasının "İleri" bir arabellek sürdürmeniz gerekir.

## <a name="5-timescale"></a>5. Timescale
[[MS-SSTR] ](https://msdn.microsoft.com/library/ff469518.aspx) için ölçeği kullanımını açıklar **SmoothStreamingMedia** (Bölüm 2.2.2.1) **StreamElement** (Bölüm 2.2.2.3) **StreamFragmentElement** () Bölümü 2.2.2.6) ve **LiveSMIL** (Bölüm 2.2.7.3.1). Ölçeği değer yoksa, kullanılan varsayılan değer 10,000,000 (10 MHz) ' dir. Kesintisiz akış biçim belirtimi diğer zaman ölçütü değerleri kullanımını engellemez, ancak bu varsayılan kodlayıcı uygulamalarının çoğu kullanın (MHz 10 kesintisiz akış oluşturmak için) değeri, veri alma. Şu nedenle [Azure medya dinamik paketleme](media-services-dynamic-packaging-overview.md) özelliği, 90 KHz ölçeği video akışları ve 44,1 KHz veya 48.1 KHz ses akışları için kullanmanızı öneririz. Farklı bir zaman ölçeğine göre değerler farklı akış için kullanılır, akış düzeyinde ölçeği gönderilmelidir. Daha fazla bilgi için [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

## <a name="6-definition-of-stream"></a>6. "Stream" tanımı
Stream akış yük devretme ve yedeklilik senaryoları işleme temel canlı sunumları oluşturma için Canlı alma işleminde birimidir. Stream, tek bir parça içerebilecek bir benzersiz, parçalanmış MP4 bitstream veya birden çok parça tanımlanır. Tam canlı bir sunumu, gerçek zamanlı kodlayıcılar yapılandırmasına bağlı olarak bir veya daha fazla akış içerebilir. Aşağıdaki örnekler, tam canlı bir sunumu oluşturmak üzere akışları kullanmanın çeşitli seçenekleri gösterir.

**Örnek:** 

Bir müşteri, aşağıdaki ses/video bit hızlarına dönüştürme içeren bir canlı akış sunumu oluşturmak istiyor:

Video: 3000 KB/sn, 1500 KB/sn, 750 KB/sn

Ses – 128 Kb/sn

### <a name="option-1-all-tracks-in-one-stream"></a>1\. seçenek: Tüm parçaları bir akış
Bu seçenek, tek bir kodlayıcı tüm ses/video parçalar oluşturur ve ardından bunları bir parçalanmış MP4 bitstream oluşturur. Parçalanmış MP4 bitstream tek bir HTTP POST bağlantısı aracılığıyla gönderilir. Bu örnekte, bu Canlı sunumu için yalnızca bir akış yok.

![Akışları bir izleme][image2]

### <a name="option-2-each-track-in-a-separate-stream"></a>2\. seçenek: Ayrı bir iş akışındaki her izleme
Bu seçenek, kodlayıcı adet her parça MP4 bitstream yerleştirir ve ardından tüm akışları ayrı HTTP bağlantıları üzerinden gönderir. Bu, bir kodlayıcı veya birden çok kodlayıcılar ile yapılabilir. Canlı alma bu Canlı sunumu dört akışları olarak görür.

![Akışları ayrı parçaları][image3]

### <a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Seçenek 3: Ses kaydı en düşük hızı video izleme ile bir akışa paket
Bu seçenekte, müşterinin bir parçanın MP4 bitstream ses kaydı ile bit hızı düşük video izleme paketini ve diğer iki video parçaları olarak ayrı akışları bırakın seçer. 

![Akışları ses ve video izler][image4]

### <a name="summary"></a>Özet
Bu, tüm olası alma seçeneklerinin Bu örnek için kapsamlı bir liste değildir. Gibi bir matter, hatta, herhangi bir gruplama akışları halinde parçalarının Canlı alma tarafından desteklenir. Müşteriler ve Kodlayıcı satıcılar mühendislik karmaşıklığı, kodlayıcı kapasite ve yedeklilik ve yük devretme konuları göre kendi uygulamaları seçebilirsiniz. Ancak, çoğu durumda, tüm Canlı sunumu için yalnızca bir ses kaydı yoktur. Bu nedenle, ses kaydının içeren alma akışın healthiness emin olmak önemlidir. Bu durum genellikle kendi akış (seçenek 2) olduğu gibi ses kaydının almadan veya en düşük bit hızlı video İzle (olduğu gibi seçeneği 3) ile paketleme sonuçlanır. Ayrıca, daha iyi yedekliliği ve hataya dayanıklılık için aynı ses kaydının farklı iki akışlarında (seçenek 2 ile yedekli ses parçaları) gönderme veya ses kaydının en az iki bit hızı düşük video izler (seçeneği 3 en az iki paket ses ile paketleme video akışları) için Canlı önerilir Media Services'e alma.

## <a name="7-service-failover"></a>7. Hizmet Yük Devretmesini
Canlı akış yapısı, iyi bir yük devretme desteği hizmetinin kullanılabilirliğini sağlamak için önemlidir. Media Services, ağ hataları, sunucu hataları ve depolama sorunlarını hataları gibi çeşitli türlerde işlemek için tasarlanmıştır. Gerçek zamanlı Kodlayıcı taraftan uygun yük devretme mantığı ile birlikte kullanıldığında, müşterilerin yüksek oranda güvenilir bir canlı akış hizmeti buluttan elde edebilirsiniz.

Bu bölümde, hizmeti yük devretme senaryolarını ele alır. Bu durumda, hatanın Hizmeti'nde yere olur ve bir ağ hatası çıkmaktadır. Hizmet Yük Devretmesini işlemek için Kodlayıcı uygulaması için bazı öneriler şunlardır:

1. 10 saniyelik zaman aşımı, TCP bağlantısı kurmak için kullanın. Bağlantı kurma denemesi 10 saniyeden uzun sürerse, işlemi durdurun ve yeniden deneyin. 
1. Kısa bir zaman aşımı iletisi öbekleri HTTP isteği göndermek için kullanın. Hedef MP4 parça süresi N saniye ise, bir gönderme işlemi zamanı aşımını N ve N 2 saniye arasında kullanın. Örneğin, 6 saniye MP4 parça süresi ise 6 ila 12 saniyelik bir zaman aşımı kullanın. Bir zaman aşımı meydana gelirse, bağlantı sıfırlama, yeni bir bağlantı açmak ve akışı sürdürme yeni bağlantısında alma. 
1. Her parça için başarıyla ve tamamen hizmete gönderilen son iki parçası olmayan bir sıralı arabellek korur.  Bir akış için HTTP POST isteği sonlandırılır veya öncesinde zaman aşımına akışın sonuna yeni bir bağlantı açmak ve başka bir HTTP POST isteği başlatmak, akışı üst bilgilerini yeniden göndermek, her parça için son iki parçaları yeniden ve d oluşturmaksızın akışı sürdürme Medya Zaman Çizelgesi'nde iscontinuity. Bu, veri kaybı olasılığı azaltır.
1. Kodlayıcı bağlantısı kurması veya TCP hata oluştuktan sonra akış sürdürmek için yeniden deneme sayısını sınırlamaz öneririz.
1. Bir TCP hatası sonra:
  
    a. Geçerli bağlantının kapalı ve yeni bir HTTP POST isteği için yeni bir bağlantı oluşturmanız gerekir.

    b. Yeni HTTP POST URL'si ilk POST URL'si ile aynı olması gerekir.
  
    c. Yeni HTTP POST içermelidir akışı üst bilgilerini (**ftyp**, **Canlı sunucusu bildirim kutusu**, ve **moov** kutuları) ilk posta akışı üstbilgilerinde aynı olan.
  
    d. Her parça için gönderilen son iki parça gönderilmesi gerekir ve akış medya zaman çizelgesi süreksizlik oluşturmaksızın çıkmalıdır. MP4 parça zaman damgaları bile HTTP POST istekleri arasında sürekli olarak artırmanız gerekir.
1. Veri MP4 parça süresiyle verilmesidir bir hızda değil gönderiliyorsa Kodlayıcı HTTP POST isteği sonlandırması gerekir.  Veri göndermediğini bir HTTP POST isteği, Media Services kodlayıcıdan bir hizmet güncelleştirmesi durumunda hızlı bir şekilde kesme öğesinden engelleyebilirsiniz. Bu nedenle, HTTP POST için seyrek izler (ad sinyali) olmalıdır kısa süreli seyrek parça gönderilen olarak sonlandırılıyor.

## <a name="8-encoder-failover"></a>8. Kodlayıcı yük devretme
Kodlayıcı yük devretme için uçtan uca canlı akış teslimi ilgilenilmesi gereken yük devretme senaryosu ikinci türüdür. Bu senaryoda, hata durumu Kodlayıcı tarafında gerçekleşir. 

![Kodlayıcı yük devretme][image5]

Kodlayıcı yük devretme gerçekleştiğinde aşağıdaki beklentileri Canlı alma uç noktasından geçerlidir:

1. Yeni bir kodlayıcı örneği (Stream için video ile kesikli çizgiye 3000 k) diyagramda gösterildiği gibi akış, devam etmek için oluşturulmalıdır.
1. Yeni Kodlayıcı, başarısız örnek olarak HTTP POST istekleri için aynı URL'yi kullanmanız gerekir.
1. Yeni Kodlayıcı'nın POST isteği aynı parçalanmış MP4 üstbilgi kutular olarak başarısız örneğini içermesi gerekir.
1. Yeni Kodlayıcı kodlayıcılarda hizalanmış parça sınırları ile eşitlenmiş ses/video örnekleri oluşturmak için tüm diğer çalışan aynı Canlı sunumu için düzgün şekilde eşitlenmesi gerekir.
1. Yeni akış önceki stream'le anlamsal olarak eşdeğer olmalıdır ve üst bilgi ve parça düzeylerinde değiştirilebilir.
1. Yeni Kodlayıcı, veri kaybını en aza indirmeyi denemelisiniz. `fragment_absolute_time` Ve `fragment_index` medya Kodlayıcı son durduğu noktasından parçaları artırmalısınız. `fragment_absolute_time` Ve `fragment_index` sürekli bir şekilde yükseltmeniz gerekir, ancak gerekirse bir süreksizlik tanıtmak için izin verilen değildir. Media Services medya Zaman Çizelgesi'nde discontinuities tanıtmak daha parçaları yeniden göndermeyi kenarındaki hata daha iyi, bu nedenle, zaten aldı ve işlenen, parça yok sayar. 

## <a name="9-encoder-redundancy"></a>9. Kodlayıcı yedeklilik
Bazı önemli Canlı isteğe daha yüksek kullanılabilirlik ve kaliteli bir deneyim, veri kaybı olmadan sorunsuz yük devretme elde etmek için aktif-aktif yedekli kodlayıcılar kullanmak önerilir, etkinlikler.

![Kodlayıcı yedeklilik][image6]

Bu diyagramda gösterildiği gibi kodlayıcılar iki grupları her akış iki kopyasını Canlı hizmette aynı anda gönderin. Bu ayarı, Media Services akış kimliği ve parça zaman damgası üzerinde göre yinelenen parçaları filtrelemek çünkü desteklenir. Arşiv ve sonuçta elde edilen canlı akış, iki farklı kaynaktan gelen en iyi olası toplama olan tek bir kopyasını tüm akışları olduğu. Örneğin, durumda bir kuramsal aşırı (aynı olması gerekmez) bir kodlayıcı olarak uzun zaman her akış için belirli bir anda çalışan, elde edilen Canlı akışı hizmetinden veri kaybı olmadan sürekli. 

Bu senaryo için gereksinimleri neredeyse gereksinimleri "Kodlayıcı yük devretme" durumda, ikinci kodlayıcılar kümesi aynı zamanda birincil kodlayıcılarda çalışıyor özel durum ile aynıdır.

## <a name="10-service-redundancy"></a>10. Hizmet artıklığı
Yüksek düzeyde yedekli genel dağıtım, bazen bölgesel felaketler işlemek için bölgeler arası yedekleme olması gerekir. "Kodlayıcı yedeklilik" topolojisine ek olarak, müşteriler kodlayıcılar ikinci kümesiyle bağlı farklı bir bölgede yedekli hizmet dağıtımı tercih edebilir. Müşteriler ayrıca bir genel istemci trafiğini sorunsuz bir şekilde yönlendirmek için Traffic Manager önünde iki hizmet dağıtımları dağıtmak için bir Content Delivery Network sağlayıcısı ile çalışabilir. Kodlayıcıları gereksinimleri "Kodlayıcı yedeklilik" durumu ile aynıdır. Kodlayıcıları farklı bir canlı yönlendirilmesi gerekiyor ikinci kümesi uç noktasını alın yalnızca istisnadır. Bu Kurulumu Aşağıdaki diyagramda gösterilmiştir:

![Hizmet artıklığı][image7]

## <a name="11-special-types-of-ingestion-formats"></a>11. Özel tür alımı biçim
Bu bölümde, Canlı alma biçimlerinin belirli senaryoları işlemek için tasarlanmış özel türleri açıklanmaktadır.

### <a name="sparse-track"></a>Seyrek İzle
Bir zengin istemci deneyimi ile canlı akış sunu, genellikle zaman eşitlenmiş olayları iletmeye gereklidir veya bant dışı ana medya verilerle bildirir. Bu Canlı dinamik reklam ekleme örneğidir. Bu türdeki olay sinyal, normal ses/video akışı seyrek doğası nedeniyle farklıdır. Diğer bir deyişle, sinyal veriler genellikle sürekli olarak gerçekleşmez ve aralığı tahmin etmek zor olabilir. Seyrek izleme kavramı, alma ve bant dışı sinyal verilerini yayınlamak için tasarlanmıştır.

Seyrek izleme almak için önerilen bir uygulaması aşağıdaki adımlar şunlardır:

1. Ses/video parçaları olmadan yalnızca seyrek parçalar içeren ayrı bir parçalanmış MP4 bitstream oluşturun.
1. İçinde **Canlı sunucusu bildirim kutusu** [1] Bölüm 6'da tanımlandığı gibi kullanın *parentTrackName* üst izleme adını belirtmek için parametre. [1] 4.2.1.2.1.2 bölümünde daha fazla bilgi için bkz.
1. İçinde **Canlı sunucusu bildirim kutusu**, **manifestOutput** ayarlanmalıdır **true**.
1. Sinyal olay seyrek yapısı gereği, aşağıdakiler önerilir:
   
    a. Canlı etkinliği başlangıcında, kodlayıcı başlangıç başlığı kutuları istemci bildiriminde seyrek izleme kaydedilecek hizmet veren hizmet gönderir.
   
    b. Veri yok gönderiliyorken Kodlayıcı HTTP POST isteği sonlandırması gerekir. Veri göndermediğini uzun süren HTTP POST, Media Services kodlayıcıdan hizmet güncelleştirmesi veya sunucu yeniden başlatma durumunda hızlı bir şekilde kesme öğesinden engelleyebilirsiniz. Bu durumlarda, ortam sunucusu yuvada alma işleminde geçici olarak engellendi.
   
    c. Ne zaman sinyal veriler kullanılamaz olduğu süre boyunca SHOULD Kodlayıcı Kapat HTTP POST isteği. POST isteği etkin durumdayken Kodlayıcı veri göndermesi gerekir.

    d. Varsa Kodlayıcı seyrek parçaları gönderirken, açık bir content-length üst bilgisi ayarlayabilirsiniz.

    e. Yeni bir bağlantıyla seyrek parçaları gönderirken Kodlayıcı yeni parçalar tarafından izlenen üst bilgi kutularından gönderme başlamanız gerekir. Bu, yük devretme raflarının olur ve önce seyrek izleme görmediği yeni bir sunucuya yeni seyrek bağlantı kuruldu çalışmalarını içindir.

    f. Seyrek parça parça eşit veya daha büyük bir zaman damgası değeri olan ilgili üst parça parça istemciye kullanılabilir olduğunda istemci için kullanılabilir hale gelir. Bir zaman damgası t seyrek parça varsa, örneğin, 1000 =, beklenen, 1000 istemci "video" ("Görüntü" üst izleme adı olduğunu varsayarak) parçalara zaman damgası 1000 veya seyrek parça t güncelleştirmesinden indirebilirsiniz görür sonra =. Gerçek sinyal sunu zaman çizelgesi farklı bir konum için belirlenen amacı için kullanılabilir olduğunu unutmayın. Bu örnekte, olası, seyrek parça t = 1000 sahip birkaç saniye olduğu bir konumda bir ad eklemek için bir XML yükünü daha sonra.

    g. Seyrek parça parça yükü senaryoya bağlı olarak (örneğin, XML, metin veya ikili), farklı biçimlerde olabilir.

### <a name="redundant-audio-track"></a>Yedekli ses kaydı
Tipik bir HTTP Uyarlamalı akış senaryoda (örneğin, kesintisiz akış veya çizgi), genellikle var. sunu içinde yalnızca bir ses kaydı Ses kaydının içeren akış alımı koparsa hata koşulları seçin istemcinin birden fazla kalite düzeylerine sahip, video parçaları ses kaydının bir tek hata noktası olabilir. 

Bu sorunu çözmek için Canlı alma yedekli ses parçaları, Media Services'ı destekler. Aynı ses kaydının farklı akış birden çok kez gönderilebilir olur. Hizmet yalnızca ses kaydının kez istemci bildiriminde kaydeder, ancak birincil ses kaydının sorunlar varsa, ses parçaları almak için yedekli ses parçaları yedeklemeler gibi kullanabilirsiniz. Yedekli ses izleri almak için Kodlayıcı gerekir:

1. Birden çok parça MP4 bitstreams aynı ses kaydı oluşturun. Yedekli ses izleri ile aynı parça zaman damgaları anlamsal olarak eşdeğer ve üst bilgi ve parça düzeylerinde değiştirilebilir.
1. "Ses" Live sunucu bildirim ([1], Bölüm 6) girişi yedekli tüm ses parçaları için aynı olduğundan emin olun.

Aşağıdaki uygulama yedekli ses parçaları için önerilir:

1. Her benzersiz bir ses kaydı tek başına bir akışa gönderin. Ayrıca, her biri burada ikinci akış farklı ilk bu ses akışları için yedek bir akışı göndermek yalnızca HTTP POST URL'si tanımlayıcıda tarafından: {protocol} :// {sunucu adresi} / {noktası path}/Streams({identifier}) yayımlanıyor.
1. İki en düşük video bit hızlarında göndermek için ayrı akışları kullanın. Ayrıca, her biri bu akışları her benzersiz bir ses kaydı bir kopyasını içermelidir. Örneğin, birden fazla dili desteklendiğinde, ses parçalarını her dil için bu akışları içermelidir.
1. Kodlama ve belirtilen yedekli akışları (1) ve (2) göndermek için ayrı bir sunucu (kodlayıcı) örneklerini kullanın. 

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
