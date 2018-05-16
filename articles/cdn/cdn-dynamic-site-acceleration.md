---
title: Azure CDN aracılığıyla dinamik site hızlandırma
description: Azure CDN dinamik site Hızlandırma (DSA) iyileştirme dinamik içerik olan dosyaları destekler.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: rli; v-deasim
ms.openlocfilehash: 4c0a68fd7b6cdf96bb495f6b447299bdbc5772f7
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Azure CDN aracılığıyla dinamik site hızlandırma

Sosyal medya, elektronik ticareti ve kişiselleştirilmiş hyper web açılımı ile son kullanıcılara sunulan içeriği hızlı bir şekilde artan yüzdesi gerçek zamanlı olarak oluşturulur. Kullanıcılar kendi tarayıcı, konum, cihaz veya ağ bağımsız hızlı, güvenilir ve kişiselleştirilmiş web deneyimi bekler. Ancak, bu nedenle de çekici bu deneyimleri olun çok yenilikleri yavaş sayfa yüklemeleri ve tüketici kalitesinden riske. 

Standart içerik teslim ağı (CDN) özelliği, statik dosyaların teslimat hızlandırmak için önbellek dosyaları yakın son kullanıcılara özelliği içerir. Sunucunun yanıt kullanıcı davranışı olarak içerik oluşturduğundan ancak, dinamik web uygulamalarıyla kenar konumlarda o içeriği önbelleğe alma mümkün değildir. Bu tür bir içeriğin teslimat hızlandırma daha geleneksel sınır önbelleğe alma daha karmaşıktır ve teslim başlangıcından tüm veri yoluna boyunca her öğe ince ayar yapılıyor bir uçtan uca çözümü gerektirir. Azure CDN dinamik site Hızlandırma (DSA) en iyi duruma getirme ile dinamik içerik web sayfalarıyla performansını sorunlarında geliştirildi.

**Akamai'den Azure CDN** ve **verizon'dan Azure CDN** her ikisi de DSA iyileştirme ile teklif **için en iyi duruma getirilmiş** uç nokta oluşturma sırasında menüsü.

> [!Important]
> İçin **akamai'den Azure CDN** profilleri, oluşturulduktan sonra bir CDN uç noktası iyileştirme değiştirmek izin verilir.
>   
> İçin **verizon'dan Azure CDN** profilleri değiştiremediğiniz bir CDN uç noktası iyileştirme oluşturulduktan sonra.

## <a name="configuring-cdn-endpoint-to-accelerate-delivery-of-dynamic-files"></a>Dinamik dosyaları teslimini hızlandırmak için CDN uç noktası yapılandırma

Dinamik dosyaları teslimini en iyi duruma getirmek için bir CDN uç noktası yapılandırmak için ya da Azure portal, REST API'leri veya herhangi bir istemci SDK aynı şeyi programlı olarak yapmak için kullanabilirsiniz. 

**Azure portalı kullanarak bir CDN uç noktası DSA iyileştirme için yapılandırmak için:**

1. İçinde **CDN profili** sayfasında, **Endpoint**.

   ![Yeni bir CDN uç noktası ekleme](./media/cdn-dynamic-site-acceleration/cdn-endpoint-profile.png) 

   **Uç nokta ekleyin** bölmesi görünür.

2. Altında **için en iyi duruma getirilmiş**seçin **dinamik site hızlandırma**.

    ![DSA ile yeni bir CDN uç noktası oluşturma](./media/cdn-dynamic-site-acceleration/cdn-endpoint-dsa.png)

3. İçin **araştırma yolu**, bir dosyaya geçerli bir yol girin.

    DSA için belirli bir özellik araştırma yoludur ve geçerli bir yol oluşturma için gereklidir. DSA kullanan küçük bir *araştırma yolu* dosya yerleştirilen ağ yönlendirme yapılandırmaları CDN için en iyi duruma getirmek için kaynak sunucuda. Araştırma yolu dosyası için indirin ve sitenize örnek dosya karşıya veya var olan bir varlık yaklaşık 10 KB boyutunda kaynağınıza kullanın.

4. Diğer gerekli bitiş noktası seçeneklerini girin (daha fazla bilgi için bkz: [yeni bir CDN uç noktası oluşturma](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint)) seçeneğini belirleyip **Ekle**.

   CDN uç noktası oluşturulduktan sonra DSA iyileştirmeler belirli ölçütlere uyan tüm dosyaları için geçerlidir. 


**DSA (yalnızca Akamai profillerinden Azure CDN) için mevcut bir uç noktası yapılandırmak için:**

1. İçinde **CDN profili** sayfasında, değiştirmek istediğiniz uç nokta seçin.

2. Sol bölmeden seçin **en iyi duruma getirme**. 

   **En iyi duruma getirme** sayfası görüntülenir.

3. Altında **için en iyi duruma getirilmiş**seçin **dinamik site hızlandırma**seçeneğini belirleyip **kaydetmek**.

> [!Note]
> DSA ekstra ücret doğurur. Daha fazla bilgi için bkz: [içerik teslim ağı fiyatlandırma](https://azure.microsoft.com/pricing/details/cdn/).

## <a name="dsa-optimization-using-azure-cdn"></a>DSA Azure CDN kullanarak en iyi duruma getirme

Azure CDN dinamik Site hızlandırmasını aşağıdaki teknikler kullanılarak dinamik varlıklarını teslimat hızlandırır:

-   [Rota iyileştirme](#route-optimization)
-   [TCP en iyi duruma getirme](#tcp-optimizations)
-   [Nesne önceden getirme (yalnızca akamai'den Azure CDN)](#object-prefetch-azure-cdn-from-akamai-only)
-   [Uyarlamalı görüntü sıkıştırma (yalnızca akamai'den Azure CDN)](#adaptive-image-compression-azure-cdn-from-akamai-only)

### <a name="route-optimization"></a>Rota iyileştirme

Rota iyileştirme Internet burada trafiği ve geçici olarak kesintileri sürekli ağ topolojisini değişen bir dinamik yer olduğundan önemlidir. Sınır Ağ Geçidi Protokolü (BGP), Internet yönlendirme protokolünün olmakla birlikte daha hızlı yollar Ara durum noktası (PoP) sunucuları aracılığıyla olabilir. 

Rota iyileştirme, bir site sürekli olarak erişilebilir ve dinamik içerik olmasını sağlamak en iyi yolu kaynağa olası en hızlı ve en güvenilir yol aracılığıyla son kullanıcılara teslim edilir seçer. 

Akamai ağ teknikleri gerçek zamanlı veri toplamak ve kaynak ve CDN uç arasındaki en hızlı yolu belirlemek için açık Internet üzerinden varsayılan BGP yolu yanı sıra Akamai sunucunun farklı düğümlerde aracılığıyla çeşitli yolları karşılaştırmak için kullanır. Bu teknikler Internet kaçının tıkanıklık noktaları ve uzun yollar. 

Benzer şekilde, her noktaya yayın DNS birleşimi Verizon ağ kullanır, yüksek kapasiteli destek POP ve sistem durumu denetimlerini, en iyi rota verileri için en iyi ağ geçitleri kaynağa istemciden belirlemek için.
 
Uncacheable olsa bile sonuç olarak, tam olarak dinamik ve işlem içeriği daha hızlı ve daha güvenilir bir şekilde son kullanıcılara teslim edilir. 

### <a name="tcp-optimizations"></a>TCP en iyi duruma getirme

İletim Denetimi Protokolü (TCP), bir IP ağ üzerinde uygulamalar arasında bilgi sağlamak için kullanılan Internet Protokolü paketi standardıdır.  Varsayılan olarak, çeşitli arka İleri istekler ölçekte verimsiz neden ağ congestions önlemek için bir TCP bağlantısı yanı sıra sınırlarını ayarlamak için gerekli değildir. **Akamai'den Azure CDN** üç alana iyileştirerek bu sorunu işler: 

 - [Kaldırmayı TCP yavaş başlatma](#eliminating-tcp-slow-start)
 - [Yararlanmayı kalıcı bağlantılar](#leveraging-persistent-connections)
 - [TCP paket parametrelerini ayarlama](#tuning-tcp-packet-parameters)

#### <a name="eliminating-tcp-slow-start"></a>Kaldırmayı TCP yavaş başlatma

TCP *yavaş başlangıç* bir algoritmadır TCP protokolünün ağ üzerinden gönderilen veri miktarını sınırlandırarak Ağ Tıkanıklığı engeller. Bu küçük tıkanıklık pencere boyutları gönderici ve alıcı arasındaki ile maksimum sınıra veya paket kaybı algılandığında kadar başlamanızı sağlar.

 Her ikisi de **akamai'den Azure CDN** ve **verizon'dan Azure CDN** profilleri ortadan TCP yavaş aşağıdaki üç adımı başlayın:

1. Sistem durumu ve bant genişliği izleme kenar PoP sunucuları arasındaki bağlantılar bant ölçmek için kullanılır.
    
2. Her sunucu etrafında diğer POP sunucu durumunu ve ağ koşulları farkında olmasını sağlayın ölçümleri kenar PoP sunucular arasında paylaşılır.  
    
3. CDN uç sunucuların kendi yakınlık diğer CDN uç sunucularıyla iletişim kurarken hangi en iyi pencere boyutunu olmalıdır gibi bazı iletim parametreleri hakkında varsayımlar olun. Bu adım, ilk tıkanıklık pencere boyutu CDN uç sunucuları arasındaki bağlantının durumunu daha yüksek paket veri aktarımını yeteneğine sahipse artırılabilir anlamına gelir.  

#### <a name="leveraging-persistent-connections"></a>Yararlanmayı kalıcı bağlantılar

Bir CDN kullanarak, daha az benzersiz makine doğrudan kaynağınıza doğrudan bağlanan kullanıcıları ile karşılaştırıldığında, kaynak sunucuya bağlanın. Azure CDN de birlikte kaynağı ile daha az bağlantıları kurmak için kullanıcı istekleri havuza alır.

Daha önce belirtildiği gibi çeşitli el sıkışma istekler TCP bağlantı kurmak için gereklidir. Tarafından uygulanan kalıcı bağlantılar `Keep-Alive` HTTP üstbilgisi, mevcut TCP bağlantılarını iletim süreleriyle kaydetmek ve teslimat hızlandırmak birden çok HTTP isteklerini yeniden. 

**Verizon'dan Azure CDN** aynı zamanda kapatılan gelen açık bir bağlantı önlemek için TCP bağlantı üzerinden düzenli tutma paketleri gönderir.

#### <a name="tuning-tcp-packet-parameters"></a>TCP paket parametrelerini ayarlama

**Akamai'den Azure CDN** sunucu-sunucu bağlantıları yöneten parametreleri ayarladığını ve uzun mesafe gidiş dönüş sitede aşağıdaki teknikleri kullanarak katıştırılmış içerik almak için gereken miktarını azaltır:

- Daha fazla paket için onay beklemeden gönderilebilecek ilk tıkanıklık penceresi artan.
- İlk yeniden aktarım zaman aşımı, böylece kaybı algılanır ve aktarım daha hızlı bir şekilde gerçekleşir kısaltır.
- Paketlerin iletim kaybolduğundan varsayılarak önce bekleme süresini azaltmak için minimum ve maksimum yeniden aktarım zaman aşımı kısaltır.

### <a name="object-prefetch-azure-cdn-from-akamai-only"></a>Nesne önceden getirme (yalnızca akamai'den Azure CDN)

Çoğu Web siteleri görüntüler ve komut dosyaları gibi çeşitli diğer kaynaklara başvuran bir HTML sayfası oluşur. Genellikle, bir istemci bir Web sayfası istediğinde, tarayıcı ilk indirir ve HTML nesneyi ayrıştırır ve sayfa tam olarak yüklemek için gerekli bağlı varlıklar için ek istekler hale getirir. 

*Önceden getirme* görüntüler ve komut dosyalarını almak için bir yöntem HTML sayfasında HTML tarayıcıya hizmet ederken ve tarayıcı bile bu nesne istek yaptığında önce katıştırılır. 

Ne zaman istemci tarayıcısına HTML temel sayfasına CDN hizmet zaman açık hazırlık seçeneğiyle CDN HTML dosyası ayrıştırır ve tüm bağlı kaynaklar için ek istekler yapıp önbelleğinde depolamak. İstemci bağlı varlıkları için istek yaptığında, CDN uç sunucusu zaten istenen nesneler sahiptir ve bunları hemen kaynağa gidiş dönüş olmadan hizmet verebilir. Bu iyileştirme alınabilir ve alınabilir olmayan içerik fayda sağlar.

### <a name="adaptive-image-compression-azure-cdn-from-akamai-only"></a>Uyarlamalı görüntü sıkıştırma (yalnızca akamai'den Azure CDN)

Bazı aygıtlar, özellikle mobil olanları zaman zaman daha yavaş ağ hızları karşılaşırsınız. Bu senaryolarda, kullanıcının daha küçük resimleri, Web sayfasında daha hızlı bir şekilde almasını yerine için tam çözüm görüntüleri uzun süredir bekleyen daha faydalı olacaktır.

Bu özellik otomatik olarak ağ kalite izler ve ağ hızları teslim süresini kısaltmak daha yavaş olduğunda standart JPEG sıkıştırma yöntemlerini uygular.

Uyarlamalı görüntü sıkıştırma | Dosya uzantıları  
--- | ---  
JPEG sıkıştırma | .jpg, .jpeg, .jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Önbelleğe alma

Bile kaynak içerdiğinde DSA ile önbelleğe alma CDN varsayılan olarak kapalıdır `Cache-Control` veya `Expires` yanıt üstbilgileri. DSA genellikle her bir istemciye benzersiz olduğundan, önbelleğe alınması gereken olmayan dinamik varlıklar için kullanılır. Önbelleğe alma Bu davranış çalışmamasına neden.

Statik ve dinamik varlıklar karışımını içeren bir Web sitesi varsa, bir karma yaklaşımı en iyi performansı elde etmek en iyisidir. 

İçin **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** profilleri kapatma kullanarak belirli DSA uç noktaları için önbelleğe almayı [kuralları önbelleğe alma](cdn-caching-rules.md).

Önbelleğe alma kurallarını erişmek için:

1. Gelen **CDN profili** sayfasında, ayarları altında select **kuralları önbelleğe alma**.  
    
    ![CDN Önbelleğe alma kuralları düğmesi](./media/cdn-dynamic-site-acceleration/cdn-caching-rules-btn.png)

    **Kuralları önbelleğe alma** sayfası açılır.

2. DSA uç noktanız için önbelleğe almayı etkinleştirmek için genel veya özel önbelleğe alma bir kural oluşturun. 

İçin **verizon'dan Azure CDN Premium** yalnızca profillerini, Aç kullanarak belirli DSA uç noktaları için önbelleğe almayı [kurallar altyapısı](cdn-rules-engine.md). Oluşturulan herhangi bir kuralın yalnızca profilinizi DSA için en iyi duruma getirilir uç etkiler. 

Kurallar altyapısı erişmek için:
    
1. Gelen **CDN profili** sayfasında, **Yönet**.  
    
    ![CDN profili Yönet düğmesi](./media/cdn-dynamic-site-acceleration/cdn-manage-btn.png)

    CDN Yönetim Portalı'nı açar.

2. CDN Yönetim Portalı'ndan seçin **ADN**seçeneğini belirleyip **kurallar altyapısı**. 

    ![DSA için kurallar altyapısı](./media/cdn-dynamic-site-acceleration/cdn-dsa-rules-engine.png)



Alternatif olarak, iki CDN uç kullanabilirsiniz: bir uç nokta en iyi duruma getirilmiş dinamik varlıklar ve başka bir uç nokta genel gibi bir statik en iyi duruma getirme türü ile en iyi duruma getirilmiş sunmak için DSA ile teslim alınabilir varlıklar için web teslim. Varlık kullanmayı planladığınız bir CDN uç noktada doğrudan bağlantı oluşturmak için Web sayfası URL'leri değiştirin. 

Örneğin: `mydynamic.azureedge.net/index.html` dinamik bir sayfa ve DSA uç noktasından yüklenir.  Html sayfaya JavaScript kitaplıkları veya statik CDN uç noktasından gibi yüklenen görüntüleri gibi birden çok statik varlıklar başvuruda `mystatic.azureedge.net/banner.jpg` ve `mystatic.azureedge.net/scripts.js`. 



