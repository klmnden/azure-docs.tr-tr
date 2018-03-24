---
title: Canlı çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services'ı kullanarak akış | Microsoft Docs
description: 'Bu konuda, bir şirket içi kodlayıcıdan tek bit hızlı bir canlı akışı alıp ardından bit hızı Uyarlamalı akışa Media Services ile gerçek zamanlı kodlama gerçekleştiren bir kanalı açıklar. Akış sonra istemci kayıttan yürütme uygulamaları bir veya daha fazla akış uç noktaları, aracılığıyla için aşağıdaki Uyarlamalı akış protokollerden birini kullanılarak alınabilir: HLS, kesintisiz akış, MPEG DASH.'
services: media-services
documentationcenter: ''
author: anilmur
manager: cfowler
editor: ''
ms.assetid: 30ce6556-b0ff-46d8-a15d-5f10e4c360e2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 9d89849bb982804515b21de8c251859591dbf6ce
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="live-streaming-using-azure-media-services-to-create-multi-bitrate-streams"></a>Azure Media Services aracılığıyla canlı akış gerçekleştirerek çoklu bit hızına sahip akışlar oluşturma

> [!NOTE]
> 12 May 2018 Canlı kanallar başlayarak artık destek RTP/MPEG-2 aktarım akışı alma protokolü. Lütfen RTP/MPEG-2'den RTMP veya parçalanmış MP4'e geçiş (kesintisiz akış) protokolleri alma.

## <a name="overview"></a>Genel Bakış
Azure Media Services (AMS) bir **kanal** canlı akış içeriğinin işlemek için bir işlem hattını temsil eder. A **kanal** Canlı giriş akışları iki yoldan biriyle alır:

* Şirket içi gerçek zamanlı bir kodlayıcı, Media Services ile şu biçimlerden birinde gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş Kanala tek bit hızlı bir akış gönderir: RTP (MPEG-TS), RTMP veya Kesintisiz Akış (Parçalanmış MP4). Ardından Kanal, gelen tek bit hızlı akışın çoklu bit hızlı (uyarlamalı) bir video akışına gerçek zamanlı kodlanmasını gerçekleştirir. İstendiğinde, Media Services akışı müşterilere teslim eder.
* Çoklu bit hızlı bir şirket içi gerçek zamanlı Kodlayıcı gönderir **RTMP** veya **kesintisiz akış** (parçalanmış MP4) kanala AMS ile gerçek zamanlı kodlama gerçekleştirmek için etkin değil. Alınan akışların geçirir **kanal**herhangi başka bir işleme olmadan s. Bu yöntem çağrılır **doğrudan**. Çoklu bit hızlı kesintisiz akış çıkışı aşağıdaki gerçek zamanlı Kodlayıcıları kullanabilirsiniz: MediaExcel, Ateme, düşünün iletişimleri, Envivio, Cisco ve Elemental. Şu gerçek zamanlı kodlayıcılar RTMP çıkışı: Adobe Flash medya Canlı Kodlayıcı (FMLE), Telestream Wirecast, Haivision, Teradek ve Tricaster kodlayıcılar.  Gerçek zamanlı bir kodlayıcı, gerçek zamanlı kodlama için etkinleştirilmemiş bir kanala tek bit hızlı bir akış da gönderebilir, ancak bu işlem önerilmez. İstendiğinde, Media Services akışı müşterilere teslim eder.
  
  > [!NOTE]
  > Bir doğrudan geçiş yöntemini kullanmak canlı akış yapmanın en ekonomik yoludur.
  > 
  > 

Bir kanal oluşturduğunuzda Media Services 2.10 sürümünden başlayarak, hangi yolla, kanalınızı Giriş akışı almaya ve desteklemediğini kanalın akışınızı gerçek zamanlı kodlama gerçekleştirmek için istiyorsanız belirtebilirsiniz. İki seçeneğiniz vardır:

* **Hiçbiri** –, Çoklu bit hızında akışa (geçiş akışı) çıkış bir şirket içi gerçek zamanlı Kodlayıcı kullanmayı planlıyorsanız, bu değeri belirtin. Bu durumda, gelen akış çıkışı herhangi kodlamadan geçirilecek. Bir kanal 2.10 yayın önce davranış budur.  Bu tür kanallar ile çalışma hakkında daha ayrıntılı bilgi için bkz: [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarda canlı akış](media-services-live-streaming-with-onprem-encoders.md).
* **Standart** – tek bit hızlı Canlı akışınızı Çoklu bit hızı akışına kodlamak için Media Services kullanmayı planlıyorsanız, bu değer seçin. Gerçek zamanlı kodlama için fatura bir etkisi yoktur ve canlı bir kodlama kanal "Çalışır" durumda bırakır fatura ücret uygulanabilir unutmayın unutmayın.  Fazladan saatlik ücretlerden kaçınmak için canlı akış olayı tamamlandıktan sonra hemen çalışan kanalların durdurmanız önerilir.

> [!NOTE]
> Bu konu gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanallar özniteliklerini açıklar (**standart** kodlama türü). Gerçek zamanlı kodlama gerçekleştirmek için etkin değil kanallar ile çalışma hakkında daha fazla bilgi için bkz: [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarda canlı akış](media-services-live-streaming-with-onprem-encoders.md).
> 
> Gözden geçirdiğinizden emin olun [konuları](media-services-manage-live-encoder-enabled-channels.md#Considerations) bölümü.
> 
> 

## <a name="billing-implications"></a>Faturalama etkileri
Canlı bir kodlama kanal durumu geçişleri "Çalışır" için API üzerinden olduğunda hemen faturalama başlar.   Azure portalında veya Azure Media Services Gezgini aracı durumunu da görüntüleyebilirsiniz (http://aka.ms/amse).

Aşağıdaki tabloda, API ve Azure portalındaki Faturalama durumları nasıl kanal durumları Eşle gösterir. Durumları API ve Portal UX arasında biraz farklı olduğuna dikkat edin API aracılığıyla "Çalışır" durumda ya da Azure Portalı'ndaki "Hazır" veya "Akış" durumdaki bir kanaldır hemen faturalama etkin olacaktır.
Daha fazla faturalama gelen kanal durdurmak için Azure portalında veya API aracılığıyla kanalı durdurun sahip.
Canlı kodlama kanalıyla bittiğinde, kanalları durdurmak için sorumlu.  Bir kodlama kanalını durduramadı hatası içinde sürekli faturalama neden olur.

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
| Başlatılıyor |Başlatılıyor |Hayır (geçici durum) |
| Çalışıyor |Hazır (çalışan program yok)<br/>or<br/>Akış (en az bir program çalışıyor) |EVET |
| Durduruluyor |Durduruluyor |Hayır (geçici durum) |
| Durduruldu |Durduruldu |Hayır |

### <a name="automatic-shut-off-for-unused-channels"></a>Otomatik kapatma kullanılmayan kanallar için kapalı
25 Ocak 2016 ile başlayan Media Services bir kanal (Canlı etkin kodlama ile) otomatik olarak durdurur bir güncelleştirme kullanıma alındı uzun bir süre boyunca kullanılmayan bir durumda yürütüldükten sonra. Bu, etkin program yok sahip olan ve uzun süre akışı bir giriş katkı yapmadığınız kanallar uygular.

Kullanılmayan bir süre için eşik ismen 12 saat olsa da değiştirilebilir.

## <a name="live-encoding-workflow"></a>Canlı kodlama iş akışı
Aşağıdaki diyagramda, burada bir kanal alır tek bit hızlı akış aşağıdaki protokollerden birini bir canlı akış iş akışı temsil eder: RTMP, kesintisiz akış veya RTP (MPEG-TS); Ardından, Çoklu bit hızında akışa akışa kodlar. 

![Canlı iş akışı][live-overview]

## <a id="scenario"></a>Ortak canlı akış senaryosu
Yaygın canlı akış uygulamaları oluşturmak için gerekli olan genel adımlar aşağıdadır.

> [!NOTE]
> Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. Temasa amslived@microsoft.com uzun süreler için bir kanal çalıştırmanız gerekiyorsa. Gerçek zamanlı kodlama için fatura bir etkisi yoktur ve canlı bir kodlama kanal "Çalışır" durumda bırakır saatlik faturalandırma ücret uygulanabilir unutmayın unutmayın.  Fazladan saatlik ücretlerden kaçınmak için canlı akış olayı tamamlandıktan sonra hemen çalışan kanalların durdurmanız önerilir. 
> 
> 

1. Bilgisayara bir video kamera bağlayın. Başlatma ve çıkışı sağlayabilecek bir şirket içi gerçek zamanlı Kodlayıcı yapılandırma bir **tek** hızında akışa aşağıdaki protokollerden birini: RTMP, kesintisiz akış veya RTP (MPEG-TS). 
   
    Bu adım, Kanalınızı oluşturduktan sonra da gerçekleştirilebilir.
2. Bir Kanal oluşturup başlatın. 
3. Kanal alma URL’sini alın. 
   
    Alma URL’si gerçek zamanlı kodlayıcı tarafından akışı Kanala göndermek için kullanılır.
4. Kanal önizleme URL’sini alın. 
   
    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.
5. Bir program oluşturun. 
   
    Azure Portalı'nı kullanırken, bir program oluşturma bir varlık da oluşturur. 
   
    .NET SDK veya REST kullanırken bir varlık oluşturun ve bir programı oluştururken bu varlığın kullanılacağını belirtmeniz gerekir. 
6. Programla ilişkili varlığı yayımlayın.   
   
    >[!NOTE]
    >AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumunda olması gerekir. 
    
7. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın.
8. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.
9. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.
10. Programı silin (ve isteğe bağlı olarak varlığı da silin).   

> [!NOTE]
> Bir canlı kodlama kanalını durduramadı unuttunuz değil çok önemlidir. Gerçek zamanlı kodlama için etkisi faturalama bir saatlik yoktur ve canlı bir kodlama kanal "Çalışır" durumda bırakır fatura ücret uygulanabilir unutmayın unutmayın.  Fazladan saatlik ücretlerden kaçınmak için canlı akış olayı tamamlandıktan sonra hemen çalışan kanalların durdurmanız önerilir. 
> 
> 

## <a id="channel"></a>Kanal girdisini (alma) yapılandırmaları
### <a id="Ingest_Protocols"></a>Alma Akış Protokolü
Varsa **Kodlayıcı türü** ayarlanır **standart**, geçerli seçenekler şunlardır:

* **RTP** (MPEG-TS): RTP üzerinden MPEG-2 aktarım akışı.  
* Tek bit hızlı **RTMP**
* Tek bit hızlı **parçalanmış MP4** (kesintisiz akış)

#### <a name="rtp-mpeg-ts---mpeg-2-transport-stream-over-rtp"></a>RTP (MPEG-TS) - RTP üzerinden MPEG-2 aktarım akışı.
Tipik kullanım örneği: 

Profesyonel yayıncıları genellikle yüksek kaliteli şirket içi gerçek zamanlı kodlayıcılar Elemental teknolojileri, Ericsson, Ateme, Imagine veya Envivio bir akış göndermeye gibi satıcılardan çalışırlar. Genellikle bir BT departmanının ve özel ağlar ile birlikte kullanılır.

Dikkate alınacak noktalar:

* Giriş tek bir program aktarım akışı (SPTS) kullanılması önerilir. 
* 8 adete kadar ses akışları RTP MPEG-2 TS kullanarak girebilirsiniz. 
* Video akışına ortalama bir bit hızı 15 Mbps aşağıda olmalıdır
* Ses akışları, toplam ortalama bit hızı 1 MB/sn olmalıdır
* Desteklenen codec bileşenleri şunlardır:
  
  * MPEG-2 / H.262 Video 
    
    * Ana profili (4:2:0)
    * Yüksek profil (4:2:0, 4:2:2)
    * 422 profil (4:2:0, 4:2:2)
  * MPEG-4 AVC / H.264 Video  
    
    * Taban çizgisi, ana, yüksek profil (8 bit 4:2:0)
    * Yüksek 10 profili (10 bit 4:2:0)
    * Yüksek 422 profili (10 bit 4:2:2)
  * MPEG-2 AAC-LC Audio 
    
    * Mono, Stereo, Surround (5.1, 7.1)
    * MPEG-2 stili ADTS paketleme
  * Dolby Digital (AC-3) Audio 
    
    * Mono, Stereo, Surround (5.1, 7.1)
  * MPEG Ses (Katman II ve III) 
    
    * Mono, Stereo
* Önerilen yayın kodlayıcılar içerir:
  
  * Imagine Communications Selenio ENC 1
  * Imagine Communications Selenio ENC 2
  * Elemental dinamik

#### <a id="single_bitrate_RTMP"></a>Tek bit hızlı RTMP
Dikkate alınacak noktalar:

* Gelen akışın Çoklu bit hızlı video içeremez.
* Video akışına ortalama bir bit hızı 15 Mbps aşağıda olmalıdır
* Ses akışı aşağıda 1 MB/sn ortalama bir bit hızı olmalıdır
* Desteklenen codec bileşenleri şunlardır:
* MPEG-4 AVC / H.264 Video
* Taban çizgisi, ana, yüksek profil (8 bit 4:2:0)
* Yüksek 10 profili (10 bit 4:2:0)
* Yüksek 422 profili (10 bit 4:2:2)
* MPEG-2 AAC-LC Audio
* Mono, Stereo, Surround (5.1, 7.1)
* 44,1 kHz örnekleme hızı
* MPEG-2 stili ADTS paketleme
* Önerilen kodlayıcılar şunları içerir:
* Telestream Wirecast
* Flash medya gerçek zamanlı kodlayıcı

#### <a name="single-bitrate-fragmented-mp4-smooth-streaming"></a>Tek bit hızlı Parçalanmış MP4 (Kesintisiz Akış)
Tipik kullanım örneği:

Şirket içi gerçek zamanlı kodlayıcılar Elemental teknolojileri, Ericsson, Ateme, Envivio Giriş akışı yakındaki bir Azure açık internet üzerinden göndermek için gibi satıcılardan veri merkezi.

Dikkate alınacak noktalar:

Aynı olarak [tek bit hızlı RTMP](media-services-manage-live-encoder-enabled-channels.md#single_bitrate_RTMP).

#### <a name="other-considerations"></a>Diğer konular
* Kanal veya ilişkili programları çalışıyorken giriş protokolünü değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Gelen video akış için en yüksek çözünürlük 1920 x 1080 olan ve en fazla 60 alanları aralıklı, saniye başına veya 30 Çerçeve/saniye aşamalı durumunda.

### <a name="ingest-urls-endpoints"></a>Alma URL'leri (Bitiş)
Bir giriş uç noktası bir kanal sağlar (URL alma) Kodlayıcı gönderebilir şekilde, gerçek zamanlı Kodlayıcı akışlar, kanala belirttiğiniz.

Kanalı oluşturduktan sonra alma URL'lerini alabilirsiniz. Bu URL'leri almak için kanal olması gerekmez **çalıştıran** durumu. Veri kanal dağıtmaya başlamak için hazır olduğunuzda, olmalıdır **çalıştıran** durumu. Veri alma kanal başladıktan sonra akışınızı Önizleme URL yoluyla önizleyebilirsiniz.

Parçalanmış MP4 alanını bir seçeneğiniz bir SSL bağlantısı üzerinden canlı akış (kesintisiz akış). SSL üzerinden alma için alma URL'si için HTTPS güncelleştirdiğinizden emin olun. Şu anda AMS SSL ile özel etki alanlarını, desteklemez.  

### <a name="allowed-ip-addresses"></a>İzin verilen IP adresi
Bu kanala video yayımlamasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri, tek bir IP adresi (ör. '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanarak bir IP aralığı (ör. ‘10.0.0.1/22’) veya bir IP adresi ve noktalı ondalık alt ağ maskesi kullanarak bir IP aralığı (ör. '10.0.0.1(255.255.252.0)').

Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.

## <a name="channel-preview"></a>Kanal önizlemesi
### <a name="preview-urls"></a>Önizleme URL'leri
Kanal Önizleme ve başka bir işleme ve teslim önce akışınızı doğrulamak için kullanacağınız bir önizleme uç noktası (Önizleme URL) sağlar.

Kanal oluşturduğunuzda Önizleme URL'sini alabilirsiniz. URL almak için kanal olması gerekmez **çalıştıran** durumu.

Veri alma kanal başladıktan sonra akışınızın önizlemesini.

> [!NOTE]
> Şu anda Önizleme akış yalnızca parçalanmış MP4 alınabilir (kesintisiz akış) biçimi belirtilen giriş türü ne olursa olsun. Kullanabileceğiniz [ http://smf.cloudapp.net/healthmonitor ](http://smf.cloudapp.net/healthmonitor) kesintisiz akış test etmek için player. Azure Portalı'nda barındırılan bir oynatıcı, akışınızı görüntülemek için de kullanabilirsiniz.
> 
> 

### <a name="allowed-ip-addresses"></a>İzin verilen IP adreslerini
Önizleme uç noktasına bağlanmasına izin verilen IP adreslerini tanımlayabilirsiniz. Herhangi bir IP adresine izin herhangi bir IP adresi belirtilmezse. İzin verilen IP adresleri, tek bir IP adresi (ör. '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanarak bir IP aralığı (ör. ‘10.0.0.1/22’) veya bir IP adresi ve noktalı ondalık alt ağ maskesi kullanarak bir IP aralığı (ör. ‘10.0.0.1(255.255.252.0)’) şeklinde belirtilebilir.

## <a name="live-encoding-settings"></a>Canlı kodlama ayarları
Bu bölümde nasıl kanal içindeki gerçek zamanlı Kodlayıcı ayarları, ne zaman ayarlanabilir açıklanmaktadır **kodlama türü** bir kanalı kümesine **standart**.

> [!NOTE]
> Zaman birden çok dil parçaları giriş yapma ve Azure ile gerçek zamanlı kodlama yapılması, yalnızca RTP çok dilli giriş için desteklenir. 8 adete kadar ses akışları RTP MPEG-2 TS kullanarak tanımlayabilirsiniz. RTMP veya kesintisiz akış ile birden çok ses izleri alma şu anda desteklenmiyor. Yaparken ile gerçek zamanlı kodlama [şirket içi Canlı kodlar](media-services-live-streaming-with-onprem-encoders.md), ne olursa olsun AMS için gönderilen bir kanal herhangi başka bir işleme olmadan geçirdiği için böyle bir kısıtlama değil.
> 
> 

### <a name="ad-marker-source"></a>Ad işaret kaynağı
Reklam işaretçi sinyallerinin kaynağını belirtebilirsiniz. Varsayılan değer **API**, gösterir kanal içindeki gerçek zamanlı Kodlayıcı için bir zaman uyumsuz olarak dinleyecek **reklam işaretçisi API'yi**.

Geçerli bir seçenek **Scte35** (yalnızca alma akış protokolüne RTP (MPEG-TS) ayarlanmış olup olmadığını izin verilir. Scte35 belirtildiğinde, gerçek zamanlı Kodlayıcı giriş RTP (MPEG-TS) akışından SCTE-35 sinyalleri ayrıştırılamıyor.

### <a name="cea-708-closed-captions"></a>CEA 708 kapalı açıklamalı alt yazıları
CEA 708 resim yazıları verileri yoksayacak şekilde gerçek zamanlı Kodlayıcı belirten isteğe bağlı bir bayrak gelen videoda katıştırılmış. Bayrak (varsayılan) false olarak ayarlandığında, kodlayıcı algılamak ve CEA 708 veri çıkış video akışları yeniden ekleyin.

### <a name="video-stream"></a>Video akışı
İsteğe bağlı. Giriş video akışı açıklanır. Bu alan belirtilmezse, varsayılan değer kullanılır. Bu ayar yalnızca RTP (MPEG-TS) protokolü akışı giriş ayarlarsanız izin verilir.

#### <a name="index"></a>Dizin
Hangi giriş video akışı kanal içindeki gerçek zamanlı Kodlayıcı tarafından işleneceğini belirtir sıfır tabanlı dizini. Bu ayar yalnızca geçerli alma Protokolü akış RTP (MPEG-TS) kullanılır.

Varsayılan değer sıfır olur. Bir tek bir program aktarım akışı (SPTS) göndermek için önerilir. Giriş akışı birden çok program içeriyorsa, gerçek zamanlı Kodlayıcı giriş Program eşleme tablosu (devresel_ödeme) ayrıştırır, akış türü adı MPEG-2 Video veya H.264 sahip girişleri tanımlar ve bunları ödeme belirtilen sırada düzenler Sıfır tabanlı dizin sonra Bu düzende n. girişini seçmek için kullanılır.

### <a name="audio-stream"></a>Ses akışı
İsteğe bağlı. Giriş ses akışları açıklar. Bu alan belirtilmezse, belirtilen varsayılan değerleri uygulayın. Bu ayar yalnızca RTP (MPEG-TS) protokolü akışı giriş ayarlarsanız izin verilir.

#### <a name="index"></a>Dizin
Bir tek bir program aktarım akışı (SPTS) göndermek için önerilir. Giriş akışı birden çok program içeriyorsa kanal içindeki gerçek zamanlı Kodlayıcı giriş Program eşleme tablosu (devresel_ödeme) ayrıştırır, MPEG-2 AAC ADTS veya AC-3 sistem A veya Sistem AC-3-B veya MPEG-2 özel PES veya MPEG-1 bir akış türü ada sahip girişleri tanımlar Ses veya MPEG-2 ses ve bunları ödeme belirtilen sırada düzenler Sıfır tabanlı dizin sonra Bu düzende n. girişini seçmek için kullanılır.

#### <a name="language"></a>Dil
ISO 639-2, Müh gibi uyumludur ses akışı dil tanıtıcısı Yoksa, varsayılan değer (tanımsız) Geri Al ' dir.

Olabilir en fazla 8 ses akışı kümeleri belirtilen giriş kanal RTP MPEG-2 TS olup olmadığını. Ancak, hiçbir iki giriş dizini aynı değere sahip olabilir.

### <a id="preset"></a>Sistem hazır
Bu kanal içindeki gerçek zamanlı Kodlayıcı tarafından kullanılmak üzere hazır belirtir. Şu anda yalnızca izin verilen değer **Default720p** (varsayılan).

Özel hazır gerekiyorsa, başvurmalıdır Not amslived@microsoft.com.

**Default720p** video aşağıdaki 7 katmanlara kodlar.

#### <a name="output-video-stream"></a>Çıkış Video akışı
| BitRate | Genişlik | Yükseklik | MaxFPS | Profil | Çıkış akış adı |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Yüksek |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Ana |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Ana |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Ana |Video_512x288_850kbps |
| 550 |384 |216 |30 |Ana |Video_384x216_550kbps |
| 350 |340 |192 |30 |Taban çizgisi |Video_340x192_350kbps |
| 200 |340 |192 |30 |Taban çizgisi |Video_340x192_200kbps |

#### <a name="output-audio-stream"></a>Çıkış ses akışı
64 KB/sn, 44,1 kHz örnekleme oranını adresindeki stereo AAC-LC ses kodlanır.

## <a name="signaling-advertisements"></a>Sinyal reklamları
Kanalınızı gerçek zamanlı kodlama etkin olduğunda, işleme video, ardışık düzeninde bir bileşen varsa ve bunu değiştirebilirsiniz. Maskeleme görüntülerini ve/veya reklam giden bit hızı Uyarlamalı akışa eklemek kanalı sinyal. Bazı durumlarda giriş canlı akış yukarı (örneğin bir ticari sonu sırasında) karşılamak üzere kullanabileceğiniz görüntüleri maskeleme görüntülerini hala var. Sinyalleri reklam, eşitlenen zaman sinyalleri video oynatıcı özel – gibi bir reklam uygun zamanda geçmek için önlem bildirmek için giden akışa katıştırmak var. Bu bkz [blog](https://codesequoia.wordpress.com/2014/02/24/understanding-scte-35/) bu amaç için kullanılan SCTE-35 sinyal mekanizması genel bakış. Aşağıda, Canlı olayınızın uygulamadan tipik bir senaryo yer almaktadır.

1. Olay başlamadan önce öncesi olay görüntüsünü al sahip.
2. Olayın sona erdikten sonra sonrası olay görüntüsünü Al, görüntüleyiciler vardır.
3. (Örneğin, stadyum içinde güç arızası) olay sırasında bir sorun varsa bir hata OLAYI görüntüsünü Al, görüntüleyiciler vardır.
4. Bir ticari sonu sırasında akışı canlı olay gizlemek için bir AD-BREAK görüntü gönderin.

Reklamları sinyal zaman ayarlayabileceğiniz özellikleri şunlardır: 

### <a name="duration"></a>Süre
Ticari sonuna saniye cinsinden süre. Bu, ticari sonu başlatabilmek için sıfır olmayan pozitif bir değer olması gerekir. Ne zaman bir ticari sonu ediyor ve bu sonu iptal süresi ile CueId sıfıra devam eden ticari sonu eşleşen ayarlanmışsa.

### <a name="cueid"></a>CueId
İçin benzersiz bir uygun eylemleri almak için aşağı akış uygulaması tarafından kullanılacak kimlik ticari sonu. Pozitif bir tamsayı olması gerekir. Herhangi bir rastgele pozitif tamsayı bu değeri ayarlayın ya da bir Yukarı Akış sistemi işaret kimlikleri izlemek için kullanın. Tüm kimlikleri pozitif tamsayılar için API aracılığıyla göndermeden önce normalleştirmek emin olun.

### <a name="show-slate"></a>Göster Kurşun
İsteğe bağlı. Geçiş yapmak için gerçek zamanlı Kodlayıcı sinyalleri [varsayılan Kurşun](media-services-manage-live-encoder-enabled-channels.md#default_slate) görüntü sırasında ticari sonu ve gelen video akışına gizle. Ses sırasında Kurşun de kapalı. Varsayılan değer **false**. 

Kullanılan görüntü slate varsayılan varlık ID özelliği kanal oluşturma sırasında belirtilen bir olacaktır. Ekran görüntüsü boyuta sığması için Kurşun uzatılmış. 

## <a name="insert-slate--images"></a>Kurşun görüntüleri ekleme
Kanal içindeki gerçek zamanlı Kodlayıcı slate görüntüye geçiş yapmak için bildirilebilir. Ayrıca, devam eden Kurşun sonlandırmak için de bildirilebilir. 

Gerçek zamanlı Kodlayıcı slate görüntüye geçin ve bazı durumlarda – gelen video sinyali Örneğin, bir ad sonu sırasında gizlemek için yapılandırılabilir. Bu tür bir Kurşun yapılandırılmamışsa, bu ad sonu sırasında giriş video maskelenir değil.

### <a name="duration"></a>Süre
Kurşun süresini saniye cinsinden. Bu, Kurşun başlatabilmek için sıfır olmayan pozitif bir değer olması gerekir. Devam eden Kurşun yoktur ve bir süre sıfır belirtilirse, devam eden Kurşun sonlandırılacak.

### <a name="insert-slate-on-ad-marker"></a>Reklam işaretçisine maskeleme görüntüsü ekle
Ne zaman true olarak bu ayar bir ad sonu sırasında slate resim eklemek için gerçek zamanlı Kodlayıcı yapılandırır. Varsayılan değer true olur. 

### <a id="default_slate"></a>Varsayılan slate varlık kimliği

İsteğe bağlı. Medya Hizmetleri varlık slate görüntü içeren varlık kimliğini belirtir. Varsayılan değer null şeklindedir. 


>[!NOTE] 
>Kanal oluşturmadan önce aşağıdaki kısıtlamalar slate görüntüsüyle (diğer bir dosyaları bu varlığı olmalıdır) adanmış bir varlık olarak yüklenmelidir. Bu görüntü, yalnızca gerçek zamanlı Kodlayıcı Kurşun bir ad sonu nedeniyle ekleme veya açıkça bir Kurşun eklemek için sinyal kullanılır. Gerçek zamanlı Kodlayıcı de slate moduna bazı hata koşullarını sırasında – Örneğin giriş sinyali kayıp olup olmadığını gidebilirsiniz. Şu anda gerçek zamanlı Kodlayıcı bu tür bir 'giriş sinyali kayıp' durumuna girdiğinde, özel bir görüntü kullanmak için bir seçenek yoktur. Bu özellik için oy [burada](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/10190457-define-custom-slate-image-on-a-live-encoder-channel).


* En çok 1920 x 1080 çözünürlük.
* En fazla 3 MB boyutunda.
* Dosya adı *.jpg uzantısı olmalıdır.
* Varlık ve bu AssetFile birincil dosya olarak işaretlenmelidir, görüntünün bir varlığa yalnızca AssetFile yüklenmelidir. Varlık şifrelenmiş depolama olamaz.

Varsa **varsayılan Kurşun varlık kimliği** belirtilmezse, ve **ad işaret üzerinde Kurşun Ekle** ayarlanır **doğru**, varsayılan bir Azure Media Services görüntüsü giriş video gizlemek için kullanılan Akış. Ses sırasında Kurşun de kapalı. 

## <a name="channels-programs"></a>Kanalın programlar
Kanal, bir canlı akıştaki segmentlerin yayımlanması ve depolanmasını denetlemenizi sağlayan programlarla ilişkilidir. Kanallar, Programları yönetir. Kanal ve Program arasındaki ilişki, burada bir kanal sabit bir içerik akışının bulunduğu ve bir programın bu kanalda zamanlanmış bir olayı kapsamlıdır geleneksel bir ortama çok benzer.

Program için kaydedilen içeriği kaç saat tutmak istediğinizi **Arşiv Penceresi** uzunluğunu ayarlayarak belirleyebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. Arşiv penceresi uzunluğu, istemcilerin geçerli canlı konumdan zamanda geri gidebilecekleri en uzun süre miktarını da belirler. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her bir program akışlı içerik depolayan bir varlıkla ilişkilidir. Bir varlık, bir Azure depolama hesabı blok blob kapsayıcısında eşlenmiş ve varlık içindeki dosyalara bu kapsayıcıdaki blobları olarak depolanır. Müşterilerinize akış görüntüleyebilmeniz programı yayımlamak için ilişkili varlığa yönelik bir OnDemand Bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal en fazla eşzamanlı çalışan Üçe destekler, böylece aynı gelen akışın birden çok arşivini oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

Yeni olaylar için mevcut programları yeniden kullanmamalısınız. Bunun yerine, oluşturun ve canlı akış uygulamaları programlama bölümünde açıklandığı gibi her olay için yeni bir programı başlatın.

Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun. 

Arşivlenen içeriği silmek için, programı durdurup silin ve ardından ilişkili varlığı silin. Bir program tarafından kullanılıyorsa varlık silinemez; önce programın silinmesi gerekir. 

Programı durdurup sildikten sonra bile, varlığı silmediğiniz sürece kullanıcılar, arşivlenen içeriğinizin isteğe bağlı video olarak akışını gerçekleştirebilir.

Arşivlenen içeriği tutmak istiyor ancak bu içeriğin akış için kullanılmasını istemiyorsanız, akış bulucuyu silin.

## <a name="getting-a-thumbnail-preview-of-a-live-feed"></a>Bir canlı akış küçük resim önizlemesini alma
Gerçek zamanlı kodlama etkinleştirildiğinde, kanal ulaştığında gibi artık canlı akış önizlemesini alabilirsiniz. Bu kanal, canlı akış gerçekte ulaşıyor olup olmadığını denetlemek için değerli bir araç olabilir. 

## <a id="states"></a>Kanal durumları ve nasıl fatura moduna durumları eşleme
Kanalın geçerli durumu. Olası değerler şunlardır:

* **Durdurulmuş**. Bu, Kanalın oluşturulduktan sonraki ilk durumudur. Bu durumda, Kanal özellikleri güncelleştirilebilir ama akışa izin verilmez.
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
> Şu anda, kanal başlangıç ortalama yaklaşık 2 dakika, ancak bazen en fazla 20 + dakika sürebilir. Kanal sıfırlar 5 dakika kadar sürebilir.
> 
> 

## <a id="Considerations"></a>Dikkat edilecek noktalar
* Bir kanalı zaman **standart** kodlama türü giriş kaynağı/katkı akış kaybı karşılaştığında, bunun için bir hata Kurşun ve sessizlik kaynak video/ses değiştirerek dengeler. Kanal giriş/katkı sürdürür akış kadar bir Kurşun yayma devam eder. Canlı kanal böyle bir durumda 2 saatten uzun bırakılması değil, öneririz. Bu noktanın ötesine giriş yeniden bağlanmadan kanalda davranışını kesin değildir, tipleri sıfırlama komutuna yanıt olarak kendi davranıştır. Kanalı durdurun, silin ve yeni bir tane oluşturmanız gerekir.
* Kanal veya ilişkili programları çalışıyorken giriş protokolünü değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Gerçek zamanlı Kodlayıcı yeniden her zaman, çağrı **sıfırlama** kanalda yöntemi. Kanal sıfırlamadan program durdurmanız gerekir. Kanal sıfırladıktan sonra program yeniden başlatın.
* Bir kanal yalnızca çalışıyor durumda olduğunda ve kanalındaki tüm programlar durduruldu durdurulabilir.
* Varsayılan olarak 5 kanalları Media Services hesabınıza yalnızca ekleyebilirsiniz. Tüm yeni hesaplarda Esnek kota budur. Daha fazla bilgi için bkz: [kotaları ve kısıtlamaları](media-services-quotas-and-limitations.md).
* Kanal veya ilişkili programları çalışıyorken giriş protokolünü değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.
* Kanalınızı olduğunda, yalnızca faturalandırılır **çalıştıran** durumu. Daha fazla bilgi için bkz [bu](media-services-manage-live-encoder-enabled-channels.md#states) bölümü.
* Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. Daha uzun bir süre için bir Kanal çalıştırmanız gerekiyorsa lütfen amslived@microsoft.com adresine başvurun.
* İçinde içerik akışı sağlamak istediğiniz akış uç bulunduğundan emin olun **çalıştıran** durumu.
* Zaman birden çok dil parçaları giriş yapma ve Azure ile gerçek zamanlı kodlama yapılması, yalnızca RTP çok dilli giriş için desteklenir. 8 adete kadar ses akışları RTP MPEG-2 TS kullanarak tanımlayabilirsiniz. RTMP veya kesintisiz akış ile birden çok ses izleri alma şu anda desteklenmiyor. Yaparken ile gerçek zamanlı kodlama [şirket içi Canlı kodlar](media-services-live-streaming-with-onprem-encoders.md), ne olursa olsun AMS için gönderilen bir kanal herhangi başka bir işleme olmadan geçirdiği için böyle bir kısıtlama değil.
* Kodlama hazır 30 fps "max kare hızı" kavramı kullanır. Giriş 60 fps ise bunu / 59.97i, giriş çerçeveleri bırakılan/Kaldır-30/29.97 fps interlaced. Giriş 50 fps/50i ise, giriş çerçeveleri bırakılan/Kaldır-25 fps interlaced. Giriş 25 fps ise, çıktı 25 fps ile kalır.
* STOP YOUR yapıldığında kanala unutmayın. Yapmadığınız takdirde, faturalandırma devam eder.

## <a name="known-issues"></a>Bilinen Sorunlar
* Kanal başlama süresi 2 dakika ortalama için iyileştirilmiştir, ancak bazen artan talebi hala en fazla 20 + dakika ele geçirebilir.
* RTP destek profesyonel yayıncıları catered. Lütfen içinde RTP notları inceleyin [bu](https://azure.microsoft.com/blog/2015/04/13/an-introduction-to-live-encoding-with-azure-media-services/) blogu.
* Slate görüntüleri açıklanan kısıtlamaları uyması [burada](media-services-manage-live-encoder-enabled-channels.md#default_slate). Dener bir kanal oluşturmak 1920 x 1080 büyük bir varsayılan Kurşun ile istek olacak sonunda kullanıma alma hatası.
* Bir kez daha... İşiniz bittiğinde dur YOUR KANALLARA unutmayın akış. Yapmadığınız takdirde, faturalandırma devam eder.

## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>İlgili konular
[Azure Media Services ile etkinliklerin canlı akış teslim etme](media-services-overview.md)

[Gerçek zamanlı kodlama singe bit hızından bit hızı Uyarlamalı akışa portalı ile gerçekleştirmek kanal oluşturma](media-services-portal-creating-live-encoder-enabled-channel.md)

[Gerçek zamanlı kodlama bit hızı Uyarlamalı akışa .NET SDK'sı singe bit hızından gerçekleştiren kanallar oluşturmak](media-services-dotnet-creating-live-encoder-enabled-channel.md)

[REST API ile kanalları Yönet](https://docs.microsoft.com/rest/api/media/operations/channel)
 
[Media Services kavramları](media-services-concepts.md)

[Azure Media Services parçalanmış MP4 Canlı belirtimi alma](media-services-fmp4-live-ingest-overview.md)

[live-overview]: ./media/media-services-manage-live-encoder-enabled-channels/media-services-live-streaming-new.png

