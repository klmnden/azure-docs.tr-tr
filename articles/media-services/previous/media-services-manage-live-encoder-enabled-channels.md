---
title: Çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services aracılığıyla canlı akış | Microsoft Docs
description: 'Bu konuda, bir şirket içi Kodlayıcı tek bit hızlı canlı akış alan ve ardından Media Services ile bit hızı Uyarlamalı akış için gerçek zamanlı kodlama gerçekleştiren bir kanalı ayarlama işlemi açıklanmaktadır. Akış ardından bir veya daha fazla akış uç noktaları aracılığıyla, istemci kayıttan yürütme uygulamalara aşağıdaki Uyarlamalı akış protokollerine biri kullanılarak teslim edilebilir: HLS, kesintisiz Stream, MPEG DASH.'
services: media-services
documentationcenter: ''
author: anilmur
manager: femila
editor: ''
ms.assetid: 30ce6556-b0ff-46d8-a15d-5f10e4c360e2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako;anilmur
ms.openlocfilehash: c168182f0b34329ed3e72e90ce86456dfbe210ca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61217255"
---
# <a name="live-streaming-using-azure-media-services-to-create-multi-bitrate-streams"></a>Azure Media Services aracılığıyla canlı akış gerçekleştirerek çoklu bit hızına sahip akışlar oluşturma

> [!NOTE]
> Canlı kanallar 12 Mayıs 2018 tarihinden itibaren artık RTP/MPEG-2 aktarım akışı destek alma protokolü. Lütfen RTP/MPEG-2'den RTMP veya parçalanmış MP4'e geçiş (kesintisiz akış) alma protokolleri.

## <a name="overview"></a>Genel Bakış
Azure Media Services (AMS) bir **kanal** için canlı akış içeriğinin işleneceği bir işlem hattını temsil eder. A **kanal** iki yoldan biriyle Canlı giriş akışları alır:

* Bir şirket içi Canlı Kodlayıcı, aşağıdaki biçimlerden birinde Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanala tek bit hızlı akış gönderir: RTMP veya kesintisiz akış (parçalanmış MP4). Ardından Kanal, gelen tek bit hızlı akışın çoklu bit hızlı (uyarlamalı) bir video akışına gerçek zamanlı kodlanmasını gerçekleştirir. İstendiğinde, Media Services akışı müşterilere teslim eder.
* Çoklu bit hızına sahip bir şirket içi Canlı Kodlayıcı gönderir **RTMP** veya **kesintisiz akış** (parçalanmış MP4) kanala AMS ile live encoding özelliğinin etkin değil. Alınan akışların **kanal**herhangi başka bir işlemeye olmadan s. Bu yöntem çağrılır **doğrudan**. Çoklu bit hızlı kesintisiz akış çıktısı sağlayan şu gerçek zamanlı Kodlayıcıları kullanabilirsiniz: MediaExcel, Ateme, Imagine Communications, Envivio, Cisco ve Elemental. Şu gerçek zamanlı kodlayıcılar RTMP çıktısı: Adobe Flash Media Live Encoder (FMLE), Telestream Wirecast, Haivision, Teradek ve Tricaster kodlayıcılar.  Gerçek zamanlı bir kodlayıcı, gerçek zamanlı kodlama için etkinleştirilmemiş bir kanala tek bit hızlı bir akış da gönderebilir, ancak bu işlem önerilmez. İstendiğinde, Media Services akışı müşterilere teslim eder.

  > [!NOTE]
  > Doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur.
  > 
  > 

Kanal oluşturduğunuzda Media Services 2.10 sürüm ile başlayarak, kanalın akışınız gerçek zamanlı kodlama gerçekleştirmek için istediğiniz olup olmadığını ve hangi yolla kanalınızı giriş akışını almak istediğiniz belirtebilirsiniz. İki seçeneğiniz vardır:

* **Hiçbiri** –, Çoklu bit hızında akışa (doğrudan akışı) çıkarır bir şirket içi Canlı Kodlayıcı kullanmayı planlıyorsanız, bu değeri belirtin. Bu durumda, gelen akış çıkışı herhangi bir kodlama içermeyen geçtiğini. Bu kanal 2.10 sürüm yayınlanmadan önce davranıştır.  Bu tür bir kanallar ile çalışma hakkında daha ayrıntılı bilgi için bkz: [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md).
* **Standart** – tek bit hızlı Canlı akışınızı Çoklu bit hızı akışına kodlama için Media Services kullanmayı planlıyorsanız, bu değeri seçin. Gerçek zamanlı kodlama için fatura bir etkisi yoktur ve bir canlı kodlama kanal "Çalışıyor" durumda bırakır fatura ücretler ödeyeceğinizi unutmayın unutmayın.  Ek saatlik ücretlerden kaçınmak için Canlı etkinlik akışı tamamlandıktan sonra hemen çalışan kanallarınızın durdurmanız önerilir.

> [!NOTE]
> Bu konu, gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanallar özniteliklerini açıklar (**standart** kodlama türü). Live encoding özelliğinin etkinleştirildiği değil kanallar ile çalışma hakkında daha fazla bilgi için bkz: [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md).
> 
> Gözden geçirdiğinizden emin olun [konuları](media-services-manage-live-encoder-enabled-channels.md#Considerations) bölümü.
> 
> 

## <a name="billing-implications"></a>Faturalandırma etkileri
Bir canlı kodlama kanal API aracılığıyla durumuna geçişler "Çalışır" duruma geldiği faturalama başlar.   Azure portalında veya Azure Media Services Gezgin aracında durumunu da görüntüleyebilirsiniz (https://aka.ms/amse).

Aşağıdaki tabloda, kanal durumlarının faturalandırma durumları API ve Azure Portalı'nda nasıl eşleneceğine gösterilmektedir. UX Portal ve API arasında biraz farklı bir durumlarıdır Faturalama, kanal "Çalışıyor" durumunda API'si aracılığıyla veya Azure portalında "Hazır" veya "Akış" durumunda olduğu sürece etkin olacaktır.
Daha fazla faturalama gelen kanal durdurmak için API aracılığıyla veya Azure portalında kanal'ı durdurmanız gerekir.
Canlı kodlama kanalıyla işiniz bittiğinde kanallarınızın durdurmak için sorumlu olursunuz.  Bir kodlama kanal durdurmak için hata sürekli faturalandırma uygulanmasına neden olur.

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

### <a name="automatic-shut-off-for-unused-channels"></a>Otomatik kapatma için kullanılmayan kanallar alma
25 Ocak 2016 ile başlayan Media Services otomatik olarak bir kanal (Canlı etkin kodlama ile) durduran bir güncelleştirme kullanıma alındı uzun bir süre için kullanılmayan bir durumda çalıştırıldıktan sonra. Bu, sahip olan etkin bir program yok ve akış uzun bir süre için bir giriş katkı almadıysanız kanalları uygular.

Kullanılmayan bir süre için eşik olup 12 saati geçmez, ancak değiştirilebilir.

## <a name="live-encoding-workflow"></a>Canlı kodlama iş akışı
Aşağıdaki diyagramda, burada bir kanala tek bit hızlı akış aşağıdaki protokollerden birini alır bir canlı akış iş akışı temsil eder: RTMP veya kesintisiz akış. Ardından, Çoklu bit hızında akışa akışa kodlar. 

![Canlı iş akışı][live-overview]

## <a id="scenario"></a>Ortak canlı akış senaryosu
Yaygın canlı akış uygulamaları oluşturmak için gerekli olan genel adımlar aşağıdadır.

> [!NOTE]
> Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. Daha uzun bir süre için bir Kanal çalıştırmanız gerekiyorsa lütfen amslived@microsoft.com adresine başvurun. Gerçek zamanlı kodlama için fatura bir etkisi yoktur ve bir canlı kodlama kanal "Çalışıyor" durumda bırakır saatlik faturalandırma ücretler ödeyeceğinizi unutmayın.  Ek saatlik ücretlerden kaçınmak için Canlı etkinlik akışı tamamlandıktan sonra hemen çalışan kanallarınızın durdurmanız önerilir. 

1. Bilgisayara bir video kamera bağlayın. Başlatma ve çıkışı bir şirket içi Canlı Kodlayıcı yapılandırma bir **tek** aşağıdaki protokollerden birini bit hızı akışı: RTMP veya kesintisiz akış. 

    Bu adım, Kanalınızı oluşturduktan sonra da gerçekleştirilebilir.
2. Bir Kanal oluşturup başlatın. 
3. Kanal alma URL’sini alın. 

    Alma URL’si gerçek zamanlı kodlayıcı tarafından akışı Kanala göndermek için kullanılır.
4. Kanal önizleme URL’sini alın. 

    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.
5. Bir program oluşturun. 

    Azure portalını kullanarak, bir program oluşturma bir varlık da oluşturur. 

    .NET SDK veya REST kullanırken bir varlık oluşturun ve bir Program oluştururken bu varlığın kullanılacağını belirtmeniz gerekir. 
6. Programla ilişkili varlığı yayımlayın.   

    >[!NOTE]
    >AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumunda olması gerekir. 

7. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın.
8. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.
9. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.
10. Programı silin (ve isteğe bağlı olarak varlığı da silin).   

> [!NOTE]
> Canlı kodlama kanal durdurmayı unutmayın çok önemlidir. Faturalama etkisi gerçek zamanlı kodlama için saatlik bir yoktur ve bir canlı kodlama kanal "Çalışıyor" durumda bırakır fatura ücretler ödeyeceğinizi unutmayın unutmayın.  Ek saatlik ücretlerden kaçınmak için Canlı etkinlik akışı tamamlandıktan sonra hemen çalışan kanallarınızın durdurmanız önerilir. 
> 
> 

## <a id="channel"></a>Kanal giriş (alma) yapılandırmalar
### <a id="Ingest_Protocols"></a>Akış Protokolü alma
Varsa **Kodlayıcı türü** ayarlanır **standart**, geçerli seçenekler şunlardır:

* Tekli bit hızı **RTMP**
* Tekli bit hızı **parçalanmış MP4** (kesintisiz akış)

#### <a id="single_bitrate_RTMP"></a>Tek bit hızlı RTMP
Dikkat edilmesi gerekenler:

* Gelen akışın Çoklu bit hızına sahip video içeremez
* Video akışı aşağıda 15 MB/sn ortalama bir hızına sahip olmalıdır
* Ses akışı aşağıda 1 MB/sn ortalama bir hızına sahip olmalıdır
* Desteklenen codec bileşenleri şunlardır:
* MPEG-4 AVC / H.264 Video
* Taban çizgisi, ana yüksek profili (8-bit 4:2:0)
* Yüksek 10 profili (10-bit 4:2:0)
* Yüksek 422 profili (10-bit 4:2:2)
* MPEG-2 AAC-LC ses
* Mono, Stereo, çevreleyen (5.1, 7.1)
* 44,1 kHz örnekleme hızı
* MPEG-2 stil ADTS paketleme
* Önerilen Kodlayıcı şunlardır:
* Telestream Wirecast
* Flash medyanın gerçek zamanlı kodlayıcı

#### <a name="single-bitrate-fragmented-mp4-smooth-streaming"></a>Tek bit hızlı Parçalanmış MP4 (Kesintisiz Akış)
Tipik kullanım örneği:

Kullanım şirket içi Canlı Kodlayıcıları satıcılardan Elemental teknolojileri, Ericsson, Ateme, Envivio Giriş akışı açık internet üzerinden yakında Azure'a göndermek için veri merkezi.

Dikkat edilmesi gerekenler:

Olarak aynı [tek bit hızlı RTMP](media-services-manage-live-encoder-enabled-channels.md#single_bitrate_RTMP).

#### <a name="other-considerations"></a>Dikkat edilecek diğer noktalar
* Kanal veya ilişkili programları çalışıyorken giriş protokolünü değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Gelen video akışı için en yüksek çözünürlük 1920 x 1080 olduğunu ve en fazla 60 alanları aralıklı, saniyede veya 30 kare/saniye aşamalı durumunda.

### <a name="ingest-urls-endpoints"></a>Alma URL'leri (Bitiş)
Bir kanalın giriş uç noktası sağlar (alma URL'si) Kodlayıcı gönderebilirsiniz, böylece, gerçek zamanlı Kodlayıcı akışları Kanallarınıza belirttiğiniz.

Bir kanal oluşturduktan sonra içe alma URL'lerini alabilirsiniz. Bu URL'leri almak için kanal olması gerekmez **çalıştıran** durumu. Kanal veri göndermeye başlamak için hazır olduğunuzda olmalıdır **çalıştıran** durumu. Veri alma kanal başladıktan sonra akışınızı Önizleme URL yoluyla önizleyebilirsiniz.

Parçalanmış MP4 almak bir seçeneğiniz vardır (kesintisiz akış) canlı akış bir SSL bağlantısı üzerinden. SSL üzerinden alımı için alma URL'si için HTTPS güncelleştirdiğinizden emin olun. Şu anda AMS SSL ile özel etki alanlarını desteklemiyor.  

### <a name="allowed-ip-addresses"></a>İzin verilen IP adresleri
Bu kanala video yayımlamasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri tek IP bir IP adresi kullanarak bir IP aralığı (örneğin, '10.0.0.1'), adresi ve CIDR alt ağ maskesi (örneğin, ' 10.0.0.1/22') veya bir IP aralığı (örneğin bir IP adresi ve noktalı ondalık alt ağ maskesi kullanılarak belirtilebilir. , ' 10.0.0.1(255.255.252.0)').

Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.

## <a name="channel-preview"></a>Kanal önizlemesi
### <a name="preview-urls"></a>Önizleme URL'leri
Kanalları daha fazla işleme edip teslime geçmeden önce akışınızı onaylama için kullandığınız bir önizleme uç noktası (Önizleme URL'si) sağlayın.

Kanıl oluşturduğunuzda Önizleme URL'sini alabilirsiniz. URL almak için kanal olması gerekmez **çalıştıran** durumu.

Veri alma kanal başladıktan sonra akışınızın önizlemesini.

> [!NOTE]
> Şu anda Önizleme akışı yalnızca parçalanmış MP4 dağıtılabilecek (kesintisiz akış) biçimi belirtilen giriş türü ne olursa olsun.  Azure Portalı'nda barındırılan bir oynatıcı, akışınız görüntülemek için kullanabilirsiniz.
> 
> 

### <a name="allowed-ip-addresses"></a>İzin verilen IP adresleri
Önizleme uç noktasına bağlanmasına izin verilen IP adreslerini tanımlayabilirsiniz. IP adresi yok belirtilirse herhangi bir IP adresine izin verilir. İzin verilen IP adresleri tek IP bir IP adresi kullanarak bir IP aralığı (örneğin, '10.0.0.1'), adresi ve CIDR alt ağ maskesi (örneğin, ' 10.0.0.1/22') veya bir IP aralığı (örneğin bir IP adresi ve noktalı ondalık alt ağ maskesi kullanılarak belirtilebilir. , ' 10.0.0.1(255.255.252.0)').

## <a name="live-encoding-settings"></a>Canlı kodlama ayarları
Bu bölümde, nasıl kanal içindeki gerçek zamanlı Kodlayıcı ayarları, ne zaman ayarlanabilir açıklanmaktadır **kodlama türü** bir kanalı kümesine **standart**.

> [!NOTE]
> Katkı akışınızı yalnızca tek bir ses kaydı – içerebilir birden çok ses parçaları başlayan kümeniz şu anda desteklenmiyor. Yaparken live encoding [şirket içi Canlı kodlar](media-services-live-streaming-with-onprem-encoders.md), kesintisiz Akış Protokolü birden çok ses parçalarını içeren akış bir katkı gönderebilirsiniz.
> 
> 

### <a name="ad-marker-source"></a>Reklam işaretçisi kaynak
Reklam işaretçi sinyallerinin kaynağını belirtebilirsiniz. Varsayılan değer **API**, kanal içindeki gerçek zamanlı Kodlayıcı için bir zaman uyumsuz dinlemesi gerektiğini gösteren **reklam işaretçisi API'yi**.

### <a name="cea-708-closed-captions"></a>CEA 708 kapalı açıklamalı Altyazılar
CEA-708 açıklamalı alt yazılar verileri yoksayacak şekilde gerçek zamanlı Kodlayıcı belirten isteğe bağlı bir bayrak gelen videoda katıştırılmış. Bayrak (varsayılan) false olarak ayarlandığında, kodlayıcı algılamak ve CEA-708 veri çıkış video akışları yeniden ekleyin.

#### <a name="index"></a>Dizin oluşturma
Bir tek program aktarım akışı (SPTS) göndermek için önerilir. Kanal içindeki gerçek zamanlı Kodlayıcı Giriş akışı birden çok programlar içeriyorsa, Program harita tablo (devresel_ödeme) girişte ayrıştırır, akış türü adı MPEG-2 AAC ADTS veya AC-3 bir sistem veya Sistem AC 3-B veya MPEG-2 özel PES veya MPEG-1 olan girişleri tanımlar Ses veya MPEG-2 ses ve bunları ödeme içinde belirtilen sırada yerleştirir Sıfır tabanlı dizini, sonra Bu düzende n. girişini çekmek için kullanılır.

#### <a name="language"></a>Dil
Dil tanımlayıcısını Müh gibi ISO 639-2 uyumludur ses akışı Yoksa, Geri Al (tanımsız) varsayılandır.

### <a id="preset"></a>Sistem Önayarı
Bu kanal içindeki gerçek zamanlı Kodlayıcı tarafından kullanılmak üzere hazır belirtir. Şu anda yalnızca izin verilen değer olduğu **Default720p** (varsayılan).

Özel önayarların kullanılmasına gereksinim duyarsanız, başvurmalısınız Not amslived@microsoft.com.

**Default720p** video aşağıdaki 6 katmanlara kodlar.

#### <a name="output-video-stream"></a>Çıkış Video Stream

| Bit hızı | Genişlik | Yükseklik | MaxFPS | Profil | Çıkış Stream adı |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Yüksek |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Yüksek |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Yüksek |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Yüksek |Video_512x288_850kbps |
| 550 |384 |216 |30 |Yüksek |Video_384x216_550kbps |
| 200 |340 |192 |30 |Yüksek |Video_340x192_200kbps |

#### <a name="output-audio-stream"></a>Ses çıkış Stream

128 Kb/sn, 48 kHz örnekleme hızı anda stereo AAC-LC ses kodlanır.

## <a name="signaling-advertisements"></a>Sinyal reklamları
Kanalınızın Canlı kodlama etkin olduğunda, video işleme, işlem hattı, bir bileşene sahip ve onu işleyebilirsiniz. Giden bit hızı Uyarlamalı akışa maskeleme görüntülerini ve/veya reklamlar kanalın için sinyal verebilirsiniz. Maskeleme görüntülerini yine de bazı özel durumlarda Canlı giriş akışını'kurmak (örneğin bir ticari sonu sırasında) karşılamak için kullanabileceğiniz görüntüleridir. Sinyaller reklam, saatin eşitlenmiş sinyalleri uygun zamanda duyuru geçmek gibi özel bir eylem – yararlanmak için video oynatıcı bildirmek için giden akış içine ekleme var. Bkz. Bu [blog](https://codesequoia.wordpress.com/2014/02/24/understanding-scte-35/) SCTE-35 sinyal mekanizması bu amaç için kullanılan genel bir bakış için. Canlı etkinliğiniz uygulayabileceğine tipik bir senaryo aşağıdadır.

1. Olay başlamadan önce bir ön olay görüntüsünü Al, görüntüleyicilerinizin vardır.
2. Etkinlik bittikten sonra bir etkinlik sonrası görüntüsü alın, görüntüleyicilerinizin vardır.
3. ' % S'olayı (örneğin, stadyum, güç kesintisi) sırasında bir sorun varsa bir hata OLAYI görüntüsünü Al, görüntüleyicilerinizin vardır.
4. Canlı etkinlik akışı sırasında bir ticari sonu gizlemek için bir AD-BREAK görüntü gönderin.

Reklamlar sinyali zaman ayarlayabilirsiniz özellikleri aşağıda verilmiştir. 

### <a name="duration"></a>Süre
Ticari sonu saniye cinsinden süre. Bu, ticari sonu başlatmak için sıfır olmayan pozitif bir değer olması gerekir. Ne zaman bir ticari sonu devam ediyor ve sonra bu kesme iptal süresi CueId ile sıfır olarak devam eden ticari sonu eşleşen ayarlanır.

### <a name="cueid"></a>CueId
İçin benzersiz bir aşağı akış uygulaması tarafından uygun eylemleri almak için kullanılacak kimlik ticari sonu. Pozitif bir tamsayı olması gerekir. İşaret kimliklerini izlemek için bir Yukarı Akış sistemini kullanın ya da herhangi bir rastgele pozitif tamsayı bu değeri ayarlayın. API aracılığıyla göndermeden önce tüm kimlikleri pozitif tam sayılar için Normalleştirilecek emin olun.

### <a name="show-slate"></a>Kurşun Göster
İsteğe bağlı. Geçiş için gerçek zamanlı Kodlayıcı sinyalleri [varsayılan maskeleme görüntüsü](media-services-manage-live-encoder-enabled-channels.md#default_slate) görüntü sırasında bir ticari sonu ve gelen video akışına gizle. Ses sırasında maskeleme görüntüsü de kapalı. Varsayılan değer **false**. 

Varsayılan maskeleme görüntüsü varlık kimliği özelliği kanal oluşturma sırasında belirtilen bir kullanılan görüntüsü olacaktır. Maskeleme görüntüsü görünen görüntü boyuta sığması için esnetilip. 

## <a name="insert-slate--images"></a>Maskeleme görüntüsü görüntüleri ekleme
Kanal içindeki gerçek zamanlı Kodlayıcı, bir maskeleme görüntüsü için geçiş yapmak için sinyal. Bu ayrıca süregelen Kurşun sonlandırmak için sinyal. 

Gerçek zamanlı Kodlayıcı, bir maskeleme görüntüsü için geçiş ve bazı durumlarda – gelen video sinyali gibi bir ad sonu sırasında gizlemek için yapılandırılabilir. Böyle bir maskeleme görüntüsü yapılandırılmamışsa, giriş video sırasında bu ad sonu maskelenir değil.

### <a name="duration"></a>Süre
Maskeleme görüntüsü süresini saniye cinsinden. Maskeleme görüntüsü başlatmak için sıfır olmayan pozitif bir değer olması gerekir. Devam eden Kurşun yoktur ve sıfır süresi belirtilirse, devam eden Kurşun sonlandırılacak.

### <a name="insert-slate-on-ad-marker"></a>Reklam işaretçisine maskeleme görüntüsü Ekle
Ne zaman true olarak bu ayar bir ad sonu sırasında maskeleme görüntüsü eklemek için gerçek zamanlı Kodlayıcı yapılandırır. Varsayılan değer true olur. 

### <a id="default_slate"></a>Varsayılan maskeleme görüntüsü varlık kimliği

İsteğe bağlı. Medya Hizmetleri varlık maskeleme görüntüsünü içeren varlık kimliğini belirtir. Varsayılan olarak NULL'dur. 


> [!NOTE] 
> Kanal oluşturmadan önce aşağıdaki kısıtlamalar geçerlidir maskeleme görüntüsü (başka hiçbir dosya bu varlığı olmalıdır) özel bir varlık olarak yüklenmelidir. Bu görüntü, yalnızca gerçek zamanlı Kodlayıcı ad sonu nedeniyle bir maskeleme görüntüsü ekleme veya açıkça bir maskeleme görüntüsü eklemek için sinyal kullanılır. Şu anda Canlı Kodlayıcı bu tür bir 'giriş sinyalini kayıp' durumuna girdiğinde, özel görüntü kullanma seçeneği yoktur. Bu özellik için oy verebilirsiniz [burada](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/10190457-define-custom-slate-image-on-a-live-encoder-channel).


* En fazla 1920 x 1080 çözünürlükte.
* En fazla 3 MB boyutunda.
* Dosya adı, *.jpg uzantısı olmalıdır.
* Varlık ve bu AssetFile birincil dosya işaretlenmesi gerekir, görüntüyü bir varlığa yalnızca AssetFile yüklenmelidir. Varlık şifrelenmiş depolama olamaz.

Varsa **varsayılan maskeleme görüntüsü varlık kimliği** belirtilmezse, ve **reklam işaretçisine maskeleme görüntüsü Ekle** ayarlanır **true**, bir varsayılan Azure Media Services görüntü giriş videosunun gizlemek için kullanılır Akış. Ses sırasında maskeleme görüntüsü de kapalı. 

## <a name="channels-programs"></a>Kanalın programlar
Kanal, bir canlı akıştaki segmentlerin yayımlanması ve depolanmasını denetlemenizi sağlayan programlarla ilişkilidir. Kanallar, Programları yönetir. Kanal ve Program ilişki, burada bir kanal sabit bir içerik akışının vardır ve bir programın bu kanalda zamanlanmış bir olayı kapsamlı geleneksel Medyadaki çok benzer.

Program için kaydedilen içeriği kaç saat tutmak istediğinizi **Arşiv Penceresi** uzunluğunu ayarlayarak belirleyebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. Arşiv penceresi uzunluğu ayrıca en fazla saat istemciyi geçmişe geçerli Canlı konumdan gidebilecekleri. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her program akış içeriği depolayan bir varlıkla ilişkilidir. Bir varlık, bir Azure depolama hesabındaki blok blob kapsayıcısını eşlenir ve varlık içindeki dosyalara, kapsayıcıdaki blobları olarak depolanır. Müşterilerinizin akış görebilecek şekilde programı yayımlamak için ilişkili varlığa yönelik bir OnDemand Bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal en fazla eşzamanlı çalışan Üçe destekler, böylece aynı gelen akışın birden fazla arşiv oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

Yeni olaylar için mevcut programları yeniden kullanmamalısınız. Bunun yerine, oluşturun ve canlı akış uygulamaları programlama bölümünde açıklandığı gibi her olay için yeni bir programı başlatın.

Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun. 

Arşivlenen içeriği silmek için, programı durdurup silin ve ardından ilişkili varlığı silin. Bir program tarafından kullanılıyorsa varlık silinemez; önce programın silinmesi gerekir. 

Programı durdurup sildikten sonra bile, varlığı silmediğiniz sürece kullanıcılar, arşivlenen içeriğinizin isteğe bağlı video olarak akışını gerçekleştirebilir.

Arşivlenen içeriği tutmak istiyor ancak bu içeriğin akış için kullanılmasını istemiyorsanız, akış bulucuyu silin.

## <a name="getting-a-thumbnail-preview-of-a-live-feed"></a>Küçük resim önizlemesi bir canlı akışı alma
Live Encoding etkinleştirildiğinde, kanal ulaştığında gibi canlı akışın önizlemesini artık alabilirsiniz. Bu kanal, canlı akış gerçekten ulaşmasını olup olmadığını denetlemek için değerli bir aracı olabilir. 

## <a id="states"></a>Kanal durumları ve nasıl faturalandırma modu durumları eşleme
Kanalın geçerli durumu. Olası değerler şunlardır:

* **Durduruldu**. Bu, Kanalın oluşturulduktan sonraki ilk durumudur. Bu durumda, Kanal özellikleri güncelleştirilebilir ama akışa izin verilmez.
* **Başlangıç**. Kanal başlatılıyor. Bu durum süresince güncelleştirmelere veya akışa izin verilmez. Bir sorun oluşursa Kanal, Durduruldu durumuna döndürülür.
* **Çalışan**. Kanal canlı akışları işleyebilir.
* **Durdurma**. Kanal durduruluyor. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.
* **Silme**. Kanal siliniyor. Bu durum süresince güncelleştirmelere veya akışa izin verilmez.

Aşağıdaki tabloda, Kanal durumlarının faturalandırma modu ile nasıl eşleştiği gösterilir. 

| Kanal durumu | Portal Arabirimi Göstergeleri | Faturalandırılmış mı? |
| --- | --- | --- |
| Başlatılıyor |Başlatılıyor |Hayır (geçici durum) |
| Çalışıyor |Hazır (çalışan program yok)<br/>or<br/>Akış (en az bir program çalışıyor) |Evet |
| Durduruluyor |Durduruluyor |Hayır (geçici durum) |
| Durduruldu |Durduruldu |Hayır |

> [!NOTE]
> Şu anda, kanal başlangıç ortalama yaklaşık 2 dakika, ancak bazen en fazla 20'den fazla dakika sürebilir. Kanal sıfırlar, 5 dakika kadar sürebilir.
> 
> 

## <a id="Considerations"></a>Dikkat edilmesi gerekenler
* Bir kanalı durumlarda **standart** kodlama türü giriş kaynağı/katkı akışı kaybı karşılaştığında, bunun için bir hata Kurşun ve sessizlik ile kaynak görüntü/ses değiştirerek dengeler. Kanal sürdürür giriş/katkı akışı kadar bir maskeleme görüntüsü yayma devam eder. Canlı kanal 2 saatten uzun bir böyle bir durumda bırakılması değil, öneririz. Bu noktanın ötesinde giriş yeniden bağlanma kanalda davranışını garanti edilmez, diğerinden sıfırlama komutuna yanıt olarak, davranıştır. Kanalı durdurun, silin ve yeni bir tane oluşturmanız gerekir.
* Kanal veya ilişkili programları çalışıyorken giriş protokolünü değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Gerçek zamanlı Kodlayıcı yeniden her seferinde, çağrı **sıfırlama** kanalındaki yöntemi. Kanal sıfırlamadan önce programı durdurmak zorunda. Kanal sıfırladıktan sonra programın yeniden başlatın.
* Bir kanal yalnızca çalışıyor durumda olduğunda ve kanalındaki tüm programlar durduruldu olduğunda durdurulabilir.
* Varsayılan olarak yalnızca Media Services hesabınız için 5 kanallar ekleyebilirsiniz. Tüm yeni hesaplar üzerinde Esnek kota budur. Daha fazla bilgi için [kotaları ve sınırlamaları](media-services-quotas-and-limitations.md).
* Kanal veya ilişkili programları çalışıyorken giriş protokolünü değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Kanalınızı olduğunda yalnızca faturalandırılırsınız **çalıştıran** durumu. Daha fazla bilgi için [bu](media-services-manage-live-encoder-enabled-channels.md#states) bölümü.
* Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. Daha uzun bir süre için bir Kanal çalıştırmanız gerekiyorsa lütfen amslived@microsoft.com adresine başvurun.
* Stream içeriği için istediğiniz akış uç noktasını bulunduğundan emin olun **çalıştıran** durumu.
* Kodlama Önayarı "max kare hızı" 30 fps kavramını kullanır. Giriş 60 fps ise bunu / 59.94i, giriş çerçeveleri bırakılan/sona erdirme olanağı-için 30/29.97 fps interlaced. Giriş 50 fps/50i giriş çerçeveleri bırakılan/sona erdirme olanağı-25 fps için interlaced tutulur. 25 fps giriş ise çıktı 25 fps kalır.
* DURDURMA YOUR işiniz bittiğinde kanala unutmayın. Yapmadığınız takdirde, faturalandırma devam eder.

## <a name="known-issues"></a>Bilinen Sorunlar
* Kanal başlatma süresi 2 dakika ortalama için iyileştirilmiştir, ancak bazen artan talebi en fazla 20'den fazla dakika yine de gerçekleştirebilir.
* Maskeleme görüntüleri açıklanan kısıtlamaları için uyması [burada](media-services-manage-live-encoder-enabled-channels.md#default_slate). 1920 x 1080 ' büyük bir varsayılan maskeleme görüntüsü ile bir kanal oluşturmaya çalışırsanız, istek olacak sonuçta ortaya çıkan hata giderildi.
* Bir kez daha... İşiniz bittiğinde durdurma YOUR KANALLARA unutmayın akış. Yapmadığınız takdirde, faturalandırma devam eder.

## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Azure Media Services ile etkinliklerin canlı akış sunma](media-services-overview.md)

[Gerçek zamanlı kodlama singe hızından portalı ile bit hızı Uyarlamalı akış gerçekleştiren kanallar oluşturun](media-services-portal-creating-live-encoder-enabled-channel.md)

[Gerçek zamanlı kodlama singe hızından .NET SDK'sı ile bit hızı Uyarlamalı akış gerçekleştiren kanallar oluşturun](media-services-dotnet-creating-live-encoder-enabled-channel.md)

[REST API ile kanalları Yönet](https://docs.microsoft.com/rest/api/media/operations/channel)

[Medya Hizmetleri kavramları](media-services-concepts.md)

[Azure Media Services bölünmüş MP4 Canlı içe alma belirtimi](media-services-fmp4-live-ingest-overview.md)

[live-overview]: ./media/media-services-manage-live-encoder-enabled-channels/media-services-live-streaming-new.png

