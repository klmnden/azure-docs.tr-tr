---
title: "Azure CDN aracılığıyla dinamik Site hızlandırma"
description: "Dinamik site hızlandırma derinlemesine bakış"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: v-semcev
ms.openlocfilehash: be2719e0e02c8bc69800ef4a3e7da3c3164cb9dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Azure CDN aracılığıyla dinamik Site hızlandırma

Sosyal medya, elektronik ticareti ve kişiselleştirilmiş hyper web açılımı ile son kullanıcılara sunulan içeriği hızlı bir şekilde artan yüzdesi gerçek zamanlı olarak oluşturulur. Kullanıcıların hızlı, güvenilir ve kişiselleştirilmiş web deneyimleri, kullanıcıların tarayıcı, konum, cihaz veya ağ bağımsız bekler. Ancak, bu nedenle de çekici bu deneyimleri olun çok yenilikleri yavaş sayfa yüklemeleri ve tüketici kalitesinden riske. 

Standart CDN yetenek, statik dosyaların teslimat hızlandırmak için önbellek dosyaları yakın son kullanıcılara özelliği içerir. Sunucunun yanıt kullanıcı davranışı olarak içerik oluşturduğundan ancak, dinamik web uygulamalarıyla kenar konumlarda o içeriği önbelleğe alma mümkün değildir. Bu tür bir içeriğin teslimat hızlandırma daha geleneksel sınır önbelleğe alma daha karmaşıktır ve teslim başlangıcından tüm veri yoluna boyunca her öğe ince ayar yapılıyor bir uçtan uca çözümü gerektirir. Azure CDN dinamik Site Hızlandırma (DSA ile), dinamik içerik web sayfalarıyla performansını sorunlarında geliştirildi.

Azure CDN Akamai ve Verizon sunar DSA iyileştirme ile **için en iyi duruma getirilmiş** uç nokta oluşturma sırasında menüsü.

## <a name="configuring-cdn-endpoint-to-accelerate-delivery-of-dynamic-files"></a>Dinamik dosyaları teslimini hızlandırmak için CDN uç noktası yapılandırma

Azure portalı üzerinden dinamik dosyaları teslimini seçerek iyileştirmek için CDN uç noktanız yapılandırabilirsiniz **dinamik site hızlandırma** altında seçeneği **için en iyi duruma getirilmiş** sırasında özellik seçimi uç nokta oluşturma. Aynı şey programlı olarak yapmak için bizim REST API'leri veya herhangi bir istemci SDK ' de kullanabilirsiniz. 

### <a name="probe-path"></a>Araştırma yolu
Dinamik Site hızlandırma belirli bir özellik araştırma yoludur ve geçerli bir tane oluşturmak için gereklidir. DSA kullanan küçük bir *araştırma yolu* dosya yerleştirilen kaynağındaki ağ yönlendirme yapılandırmaları CDN için en iyi duruma getirme. Karşıdan yükle ve sitenize bizim örnek dosya karşıya yükleme veya mevcut bir varlık varlık varsa olan yaklaşık 10 KB araştırma yolu için bunun yerine, kaynak kullanın.

> [!Note]
> DSA ekstra ücret doğurur. Daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cdn/) daha fazla bilgi için.

Aşağıdaki ekran görüntüleri Azure Portalı aracılığıyla işlemi gösterilmektedir.
 
![Yeni bir CDN uç noktası ekleme](./media/cdn-dynamic-site-acceleration/01_Endpoint_Profile.png) 

*Şekil 1: yeni bir CDN uç noktası CDN profilinden ekleme*
 
![DSA ile yeni bir CDN uç noktası oluşturma](./media/cdn-dynamic-site-acceleration/02_Optimized_DSA.png)  

*Şekil 2: dinamik site hızlandırma seçili iyileştirme bir CDN uç noktası oluşturma*

CDN uç noktası oluşturulduğunda, DSA iyileştirmeler belirli ölçütlere uyan tüm dosyaları için geçerlidir. Aşağıdaki bölümde ayrıntılı DSA iyileştirme açıklanmaktadır.

## <a name="dsa-optimization-using-azure-cdn"></a>DSA Azure CDN kullanarak en iyi duruma getirme

Azure CDN dinamik Site hızlandırmasını aşağıdaki teknikler kullanılarak dinamik varlıklarını teslimat hızlandırır:

-   Rota iyileştirme
-   TCP en iyi duruma getirme
-   Nesne önceden getirme (yalnızca Akamai)
-   Mobil görüntü sıkıştırma (yalnızca Akamai)

### <a name="route-optimization"></a>Rota iyileştirme

Rota iyileştirme Internet burada trafiği ve geçici olarak kesintileri sürekli ağ topolojisini değişen bir dinamik yer olduğundan önemlidir. Sınır Ağ Geçidi Protokolü (BGP), Internet yönlendirme protokolünün olmakla birlikte daha hızlı yollar Ara durum noktası (PoP) sunucuları aracılığıyla olabilir. 

Rota iyileştirme, bir site sürekli olarak erişilebilir ve dinamik içerik olmasını sağlamak en iyi yolu kaynağa olası en hızlı ve en güvenilir yol aracılığıyla son kullanıcılara teslim edilir seçer. 

Akamai ağ teknikleri gerçek zamanlı veri toplamak ve kaynak ve CDN uç arasındaki en hızlı yolu belirlemek için açık Internet üzerinden varsayılan BGP yolu yanı sıra Akamai sunucunun farklı düğümlerde aracılığıyla çeşitli yolları karşılaştırmak için kullanır. Bu teknikler Internet kaçının tıkanıklık noktaları ve uzun yollar. 

Benzer şekilde, her noktaya yayın DNS birleşimi Verizon ağ kullanır, yüksek kapasiteli destek POP ve sistem durumu denetimlerini, en iyi rota verileri için en iyi ağ geçitleri kaynağa istemciden belirlemek için.
 
Uncacheable olsa bile sonuç olarak, tam olarak dinamik ve işlem içeriği daha hızlı ve daha güvenilir bir şekilde son kullanıcılara teslim edilir. 

### <a name="tcp-optimizations"></a>TCP en iyi duruma getirme

İletim Denetimi Protokolü (TCP), bir IP ağ üzerinde uygulamalar arasında bilgi sağlamak için kullanılan Internet Protokolü paketi standardıdır.  Varsayılan olarak, vardır birkaç geri ve İleri istekleri ölçekte verimsiz neden ağ congestions önlemek için bir TCP bağlantısı yanı sıra sınırlarını ayarlamak için gerekli. Akamai'den Azure CDN üç alana iyileştirerek bu sorunla ilgilidir: 

 - Yavaş başlatma ortadan
 - Yararlanmayı kalıcı bağlantılar
 - TCP paket parametrelerini (yalnızca Akamai) ayarlama

#### <a name="eliminating-slow-start"></a>Yavaş başlatma ortadan

*Yavaş başlatma* ağ üzerinden gönderilen veri miktarını sınırlandırarak Ağ Tıkanıklığı engeller TCP protokolü, bir parçasıdır. Bu küçük tıkanıklık pencere boyutları gönderici ve alıcı arasındaki ile maksimum sınıra veya paket kaybı algılandığında kadar başlamanızı sağlar.

Azure CDN Akamai ve Verizon üç adımda yavaş başlatma ortadan kaldırır:

1.  Akamai ve Verizon'ın ağ durumunu ve bant genişliği izleme kenar PoP sunucuları arasındaki bağlantılar bant ölçmek için kullanın.
2. Her sunucu etrafında diğer POP sunucu durumunu ve ağ koşulları farkında olmasını sağlayın ölçümleri kenar PoP sunucular arasında paylaşılır.  
3. CDN uç sunucuların kendi yakınlık diğer CDN uç sunucularıyla iletişim kurarken hangi en iyi pencere boyutunu olmalıdır gibi bazı iletim parametreleri hakkında varsayımlar yapamaz. Bu adım, ilk tıkanıklık pencere boyutu CDN uç sunucuları arasındaki bağlantının durumunu daha yüksek paket veri aktarımını yeteneğine sahipse artırılabilir anlamına gelir.  

#### <a name="leveraging-persistent-connections"></a>Yararlanmayı kalıcı bağlantılar

Bir CDN kullanarak, daha az benzersiz makine doğrudan kaynağınıza doğrudan bağlanan kullanıcıları ile karşılaştırıldığında, kaynak sunucuya bağlanın. Azure CDN Akamai ve Verizon de birlikte kaynağı ile daha az bağlantıları kurmak için kullanıcı istekleri havuza alır.

Daha önce belirtildiği gibi TCP bağlantıları birçok istek yeni bir bağlantı kurmak için bir anlaşması İleri ve geri alın. Kalıcı bağlantılar, olarak da bilinen "HTTP Etkin tutmayı," gidiş dönüş kez kaydetmek ve teslimat hızlandırmak birden çok HTTP isteklerini varolan TCP bağlantılarını yeniden kullanabilirsiniz. 

Verizon ağ düzenli tutma paketleri kapatılan gelen açık bir bağlantı önlemek için TCP bağlantı üzerinden de gönderir.

#### <a name="tuning-tcp-packet-parameters"></a>TCP paket parametrelerini ayarlama

Ayrıca aşağıdaki teknikleri kullanarak sunucu-sunucu bağlantıları yönetir ve uzun mesafe miktarını azaltır parametreleri içerik almak için gerekli dönüşleri yuvarlamak parçaları sitede katıştırılmış akamai'den Azure CDN:

1.  Daha fazla paket için onay beklemeden gönderilebilecek ilk tıkanıklık penceresi artan.
2.  İlk yeniden aktarım zaman aşımı, böylece kaybı algılanır ve aktarım daha hızlı bir şekilde gerçekleşir kısaltır.
3.  Paketlerin iletim kaybolduğundan varsayılarak önce bekleme süresini azaltmak için minimum ve maksimum yeniden aktarım zaman aşımı kısaltır.

### <a name="object-prefetch-akamai-only"></a>Nesne önceden getirme (yalnızca Akamai)

Çoğu Web siteleri görüntüler ve komut dosyaları gibi çeşitli diğer kaynaklara başvuran bir HTML sayfası oluşur. Genellikle, bir istemci bir Web sayfası istediğinde, tarayıcı ilk indirir ve HTML nesneyi ayrıştırır ve sayfa tam olarak yüklemek için gerekli bağlı varlıklar için ek istekler hale getirir. 

*Önceden getirme* görüntüler ve komut dosyalarını almak için bir yöntem HTML sayfasında HTML tarayıcıya hizmet ederken ve tarayıcı bile bu nesne istek yaptığında önce katıştırılır. 

İle **hazırlık** seçeneği açık olduğunda CDN görevi görür temel HTML sayfası CDN istemci tarayıcısına ayrıştırıyor aynı anda HTML dosyası ve yapma ek istekler herhangi bağlı kaynakları ve önbelleğinde depolamak. İstemci bağlı varlıkları için istek yaptığında, CDN uç sunucusu zaten istenen nesneler sahiptir ve bunları hemen kaynağa gidiş dönüş olmadan hizmet verebilir. Bu iyileştirme alınabilir ve alınabilir olmayan içerik fayda sağlar.

### <a name="adaptive-image-compression-akamai-only"></a>Uyarlamalı görüntü sıkıştırma (yalnızca Akamai)

Bazı aygıtlar, özellikle mobil olanları zaman zaman daha yavaş ağ hızları karşılaşırsınız. Bu senaryolarda, kullanıcının daha küçük resimleri, Web sayfasında daha hızlı bir şekilde almasını yerine için tam çözüm görüntüleri uzun süredir bekleyen daha faydalı olacaktır.

Bu özellik otomatik olarak ağ kalite izler ve ağ hızları teslim süresini kısaltmak daha yavaş olduğunda standart JPEG sıkıştırma yöntemlerini uygular.

Uyarlamalı görüntü sıkıştırma | Dosya uzantıları  
--- | ---  
JPEG sıkıştırma | .jpg, .jpeg, .jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Önbelleğe alma

Bile kaynak yanıtta önbellek denetimi/expires üst bilgileri içerdiğinde DSA ile önbelleğe alma CDN varsayılan olarak kapalıdır. DSA genellikle her bir istemciye benzersiz olduğundan, önbelleğe alınması gereken olmayan dinamik varlıklar için kullanılır çünkü bu varsayılan kapalıdır ve varsayılan olarak önbelleğe alma özelliğini açmak Bu davranış bölünebilir.

Statik ve dinamik varlıklar karışımını içeren bir Web sitesi varsa, bir karma yaklaşımı en iyi performansı elde etmek en iyisidir. 

ADN Verizon Premium ile kullanıyorsanız, geri kurallar altyapısı kullanarak özel durumlarda önbelleğe alma kapatabilirsiniz.  

Alternatif iki CDN uç noktası kullanmaktır. Dinamik varlıklar ve başka bir uç nokta teslim alınabilir varlıklar için genel web teslim gibi bir statik en iyi duruma getirme türü ile teslim etmek için DSA biriyle. Bu alternatif gerçekleştirmek için kullanmayı planladığınız CDN uç varlık doğrudan bağlantı oluşturmak için Web sayfası URL'leri değiştirecektir. 

Örneğin: `mydynamic.azureedge.net/index.html` dinamik bir sayfa ve DSA uç noktasından yüklenir.  Html sayfaya JavaScript kitaplıkları veya statik CDN uç noktasından gibi yüklenen görüntüleri gibi birden çok statik varlıklar başvuruda `mystatic.azureedge.net/banner.jpg` ve `mystatic.azureedge.net/scripts.js`. 

Bir örnek bulabilirsiniz [burada](https://docs.microsoft.com/azure/cdn/cdn-cloud-service-with-cdn#controller) belirli bir CDN URL yoluyla içerik sunmak için bir ASP.NET web uygulamasında denetleyicileri kullanma.




