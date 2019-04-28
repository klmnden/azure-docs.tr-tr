---
title: Azure SignalR hizmeti için performans Kılavuzu
description: Azure SignalR Hizmeti performansını genel bakış.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: zhshang
ms.openlocfilehash: f7cc05c8c2a299d809c4386d119fef58fa2548d5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269511"
---
# <a name="performance-guide-for-azure-signalr-service"></a>Azure SignalR hizmeti için performans Kılavuzu

Azure SignalR hizmeti kullanmanın önemli avantajları SignalR uygulamalarını ölçeklendirme kolaylığı biridir. Büyük ölçekli bir senaryoda performansı önemli bir faktördür. 

Bu kılavuzda, biz SignalR uygulama performansını etkileyen faktörler tanıtacağız. Tipik performans farklı kullanım örneği senaryoları açıklayacağız. Sonunda size bir performans raporu oluşturmak için kullanabileceğiniz araçları ve ortam tanıtacağız.

## <a name="term-definitions"></a>Terim tanımları

*Gelen*: Azure SignalR hizmeti için gelen ileti.

*Giden*: Azure SignalR Hizmeti giden ileti.

*Bant genişliği*: 1 saniye tüm iletilerin toplam boyutu.

*Varsayılan mod*: Azure SignalR hizmeti örneği oluşturulduğunda varsayılan çalışma modu. Azure SignalR hizmeti, tüm istemci bağlantılarını kabul etmeden önce onunla bağlantı kurmak için uygulama sunucusu bekliyor.

*Sunucusuz modu*: Azure SignalR hizmeti yalnızca istemci bağlantılarını kabul eden bir modu. Hiçbir sunucu bağlantısı izin verilir.

## <a name="overview"></a>Genel Bakış

Azure SignalR hizmeti yedi standart katmanları için farklı performans kapasiteler tanımlar. Bu kılavuz, aşağıdaki soruları yanıtlamaktadır:

-   Her katman için normal Azure SignalR hizmeti performans nedir?

-   Azure SignalR hizmeti ileti aktarım hızı (örneğin, 100.000 iletileri gönderme saniye başına) için benim gereksinimlerini karşılıyor mu?

-   Hangi katmanı benim için uygun olan my belirli senaryo için? Veya nasıl uygun katman seçebilirsiniz?

-   Ne tür bir uygulama sunucusu (sanal makine boyutu) benim için uygun mu? Kaç tanesinin dağıtabilirim?

Bu soruları yanıtlamak için bu kılavuzun ilk performansını etkileyen faktörler, üst düzey bir açıklama sağlar. Ardından tipik kullanım durumları için her katman için en fazla gelen ve giden iletileri gösterilmektedir: **Yankı**, **yayın**, **grubuna Gönder**, ve **gönderin Bağlantı** (için Sohbet eşler arası).

Bu kılavuzda tüm senaryoları kapsamamaktadır (ve farklı kullanım örnekleri, ileti boyutları, ileti gönderen desenleri ve benzeri). Ancak yardımcı olması için bazı yöntemler sağlar:

- Gelen veya giden iletiler için yaklaşık, gereksinim değerlendirin.
- Performans tablonuzu kontrol ederek doğru katmanları bulun.

## <a name="performance-insight"></a>Performansı içgörüleri

Bu bölümde, performans değerlendirmesi yöntemleri açıklar ve ardından performansı etkileyen tüm faktörleri listeler. Sonunda, performans gereksinimlerini değerlendirmenize yardımcı olmak için yöntemleri sağlar.

### <a name="methodology"></a>Yöntemi

*Aktarım hızı* ve *gecikme* olan iki tipik yönlerini performans denetleniyor. Azure SignalR hizmeti için kendi aktarım hızı azaltma ilkesi her SKU katmanı vardır. İlkeyi tanımlar *izin verilen en fazla aktarım hızı (gelen ve giden bant genişliği)* iletileri yüzde 99'unu 1 saniyeden kısa olan gecikme süresi olduğunda elde edilen aktarım hızı üst sınırı gibi.

Azure SignalR hizmetten yanıt iletisi almayı ileti gönderen bağlantının zaman aralığını gecikme var. Alalım **Yankı** örnek olarak. Her istemci bağlantı iletisi zaman damgası ekler. Uygulama sunucusu hub özgün iletinin istemciye geri gönderir. Bu nedenle yayma gecikme kolayca her istemci bağlantısı tarafından hesaplanır. Zaman damgası her ileti için bağlı **yayın**, **grubuna Gönder**, ve **bağlantı gönderme**.

Yinelenen istemci bağlantılarının binlerce benzetimini yapmak için azure'da özel bir sanal ağda birden çok VM oluşturulur. Tüm bu VM'lerin aynı Azure SignalR hizmeti örneğine bağlanın.

Azure SignalR hizmeti varsayılan modunda uygulama sunucusu Vm'leri aynı sanal ağdaki özel istemci sanal makinelerine olarak dağıtılır. Tüm istemci Vm'leri ve uygulama sunucusu Vm'leri bölgeler arası gecikmeyi önlemek için aynı bölgede aynı ağda dağıtılır.

### <a name="performance-factors"></a>Performans etmenleri

Teorik olarak, Azure SignalR hizmeti kapasite tarafından kullanılan hesaplama kaynağı sınırlıdır: CPU, bellek ve ağ. Örneğin, Azure SignalR hizmeti için daha fazla bağlantı hizmet daha fazla bellek kullanmasına neden olur. Büyük ileti trafik için (örneğin, her ileti 2.048 bayttan büyük), Azure SignalR hizmeti trafiğini işlemek için daha fazla CPU döngülerini kullanması gerekir. Bu arada, Azure ağ bant genişliği, ayrıca maksimum trafiği için bir sınır uygular.

Aktarım Türü performansını etkileyen başka bir faktördür. Üç tür şunlardır [WebSocket](https://en.wikipedia.org/wiki/WebSocket), [sunucu gönderilen olay](https://en.wikipedia.org/wiki/Server-sent_events), ve [uzun yoklaması](https://en.wikipedia.org/wiki/Push_technology). 

WebSocket tek bir TCP bağlantısı bir çift yönlü ve çift yönlü iletişim protokolü olan. Sunucu gönderilen olay bir tek yönlü anında iletme iletileri için sunucudan istemciye protokolüdür. Uzun yoklaması HTTP isteği aracılığıyla sunucu bilgileri düzenli aralıklarla yoklamaya istemcilerin gerektirir. Aynı koşullar altında aynı API, WebSocket en iyi performansa sahip olduğundan, sunucu gönderilen olay yavaştır ve uzun yoklama en yavaş olduğu. Azure SignalR hizmeti varsayılan olarak WebSocket önerir.

İleti yönlendirme maliyeti de performans sınırlandırır. Azure SignalR hizmeti, diğer istemciler veya sunucular için istemciler veya sunucular kümesi iletiden yönlendiren bir ileti yönlendirici olarak bir rol oynar. Farklı senaryo veya API farklı bir yönlendirme ilkesi gerektirir. 

İçin **Yankı**istemci kendisine bir ileti gönderir ve yönlendirme hedefi kendisi de değildir. Bu düzen, en düşük yönlendirme maliyeti vardır. Ancak **yayın**, **grubuna Gönder**, ve **bağlantı gönderme**, Azure SignalR hizmeti gereken hedef bağlantıları aracılığıyla iç Dağıtılmış veri aramak yapısı. Bu ek işlem daha fazla CPU, bellek ve ağ bant genişliği kullanır. Sonuç olarak, performans daha yavaştır.

Varsayılan modda uygulama sunucusu Ayrıca belirli senaryolar için bir performans sorunu olabilir. Azure SignalR SDK'sı her istemci sinyal sinyalleri aracılığıyla canlı bir bağlantıyla tutar ancak hub çağırma gerekir.

Sunucusuz modunda istemci WebSocket kadar verimli olmayan HTTP post ile bir ileti gönderir.

Başka bir faktördür protokolü: JSON ve [MessagePack](https://msgpack.org/index.html). MessagePack JSON daha küçük boyutlu ve teslim edilen daha hızlıdır. MessagePack, ancak performansını değil. Azure SignalR Hizmeti performansını sunuculara ya da tam tersi istemcilerden gelen ileti iletme sırasında ileti yükü çözülmesi değil çünkü protokolleri için önemli değil.

Özet olarak, aşağıdaki etmenlere gelen ve giden kapasite etkiler:

-   SKU Katmanı (CPU/bellek)

-   Bağlantı sayısı

-   İleti boyutu

-   ileti gönderme oranı

-   Aktarım Türü (WebSocket, sunucu gönderilen olay veya uzun yoklaması)

-   Kullanım örneği senaryosu (yönlendirme maliyeti)

-   Uygulama sunucusu ve hizmet bağlantıları (sunucu modunda)


### <a name="finding-a-proper-sku"></a>Uygun bir SKU bulma

Nasıl, gelen/giden kapasitesini değerlendirmek veya hangi katmanda bulmak için belirli bir kullanım örneği uygundur?

Uygulama sunucusu yeterince güçlü ve performans sorunu olmadığını varsayar. Ardından, her katman için en büyük gelen ve giden bant olup olmadığını denetleyin.

#### <a name="quick-evaluation"></a>Hızlı değerlendirme

Şimdi değerlendirme önce bazı varsayılan ayarların olduğunu düşünerek tarafından kolaylaştırma: 

- Aktarım WebSocket türüdür.
- İleti boyutu 2.048 bayttır.
- Her 1 saniyede bir ileti gönderilir.
- Azure SignalR hizmeti varsayılan moddur.

Her katman kendi gelen en yüksek bant genişliği ve giden bant genişliği var. Gelen veya giden bağlantı sınırını aştıktan sonra sorunsuz bir kullanıcı deneyimi garanti edilmez.

**Yankı** düşük yönlendirme maliyeti olduğundan en fazla gelen bant genişliği sağlar. **Yayın** en fazla giden ileti bant genişliğini tanımlar.

Yapmak *değil* aşağıdaki iki tabloda vurgulanan değerler aşıyor.

|       echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar                       | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| **Gelen bant genişliği** | **2 MB/sn**    | **4 MB/sn**    | **10 MB/sn**   | **20 MB/sn**    | **40 MB/sn**    | **100 MB/sn**   | **200 MB/sn**    |
| Giden bant genişliği | 2 Mb/sn   | 4 Mb/sn   | 10 Mb/sn  | 20 MB/sn   | 40 MB/sn   | 100 MB/sn  | 200 MB/sn   |


|     Yayın             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000  | 100.000 |
| Gelen bant genişliği  | 4 KBps   | 4 KBps   | 4 KBps    | 4 KBps    | 4 KBps    | 4 KBps     | 4 KBps    |
| **Giden bant genişliği** | **4 MB/sn**    | **8 MB/sn**    | **20 MB/sn**    | **40 MB/sn**    | **80 MB/sn**    | **200 MB/sn**    | **400 MB/sn**   |

*Gelen bant genişliği* ve *giden bant genişliğini* saniye başına toplam ileti boyutu.  Bunlar için formüller şunlardır:
```
  inboundBandwidth = inboundConnections * messageSize / sendInterval
  outboundBandwidth = outboundConnections * messageSize / sendInterval
```

- *inboundConnections*: İletiyi gönderen bağlantılarının sayısı.

- *outboundConnections*: İletiyi alan bağlantıları sayısı.

- *messageSize*: (Ortalama değer) tek bir ileti boyutu. Bu kısa 1024 bayt 1024 bayt iletiye benzer bir performans etkisi olan küçük bir ileti.

- *SendInterval*: Bir ileti gönderme zamanı. Bu genellikle, her saniye bir ileti gönderilirken anlamına gelir, ileti başına 1 saniyedir. Daha küçük bir aralıkta bir zaman diliminde daha fazla ileti gönderilirken anlamına gelir. Örneğin, ileti başına 0,5 saniye anlamına gelir her saniye iki ileti gönderme.

- *Bağlantıları*: Taahhüt edilen en yüksek eşik Azure SignalR hizmeti için her katman için. Daha fazla bağlantı sayısı artar, gelen bağlantı daraltma düşer.

#### <a name="evaluation-for-complex-use-cases"></a>Karmaşık kullanım örnekleri için değerlendirme

##### <a name="bigger-message-size-or-different-sending-rate"></a>Büyük ileti boyutu veya farklı gönderme oranı

Gerçek kullanım durumu daha karmaşıktır. Bir ileti 2.048 bayttan büyük gönderebilir veya saniye başına değil bir mesaj gönderen ileti oranıdır. Unit100'ın yayın performansını değerlendirmek nasıl bulmak için örnek olarak alalım.

Aşağıdaki tabloda, gerçek kullanım örneği gösterilmektedir. **yayın**. Ancak, ileti boyutu, bağlantı sayısı ve ileti gönderirken oranı ne biz önceki bölümde kabul öğesinden farklıdır. Biz yalnızca iki tanesi biliyorsanız nasıl biz (ileti boyutu, bağlantı sayısı veya ileti gönderme hızını) bu öğelerden herhangi birini çıkarabilir soru şudur.

| Yayın  | İleti boyutu | Saniye başına gelen iletiler | Bağlantılar | Aralıkları Gönder |
|---|---------------------|--------------------------|-------------|-------------------------|
| 1 | 20 KB                | 1                        | 100.000     | 5 sn                      |
| 2 | 256 KB               | 1                        | 8,000       | 5 sn                      |

Aşağıdaki formülü önceki formüle göre çıkarsanacak kolaydır:

```
outboundConnections = outboundBandwidth * sendInterval / messageSize
```

Unit100 için en fazla giden bant genişliğini 400 önceki tablosundan MB'dir. 20-KB'lık mesaj boyutu için en fazla giden bağlantı 400 MB olmalıdır \* 5 / 20 KB = 100.000, gerçek değer eşleşen.

##### <a name="mixed-use-cases"></a>Karma kullanım örnekleri

Birlikte, genellikle karıştırır dört temel kullanım durumları gerçek kullanım örneği: **Yankı**, **yayın**, **grubuna Gönder**, ve **bağlantı gönderme**. Kapasite değerlendirmek için kullandığınız metodolojiyi olmaktır:

1. Karma kullanım örneklerini dört temel kullanım örneklerinden bölün.
1. En fazla ileti gelen ve giden bant genişliği ayrı olarak önceki formüllerin kullanarak hesaplayın.
1. Toplam en fazla gelen/giden bant genişliği almak için bant genişliği hesaplamalar toplayın. 

En fazla gelen/giden bant genişliği tablolardan uygun katmanındaki ardından seçin.

> [!NOTE]
> Yönlendirme maliyeti, yüzlerce veya binlerce küçük gruplar için bir ileti göndermek ya da binlerce istemciden birbirlerine ileti göndermek için baskın olur. Bu etkiyi göz önüne alın.

Kullanım örneği istemcilere bir ileti gönderme uygulama sunucusu olduğundan emin olun *değil* performans sorunu. Aşağıdaki "İncelemesini" bölümü yönergeleri sağlar ve kaç sunucu bağlantıları hakkında ne kadar uygulama sunucuları ihtiyacınız yapılandırmalısınız.

## <a name="case-study"></a>Örnek olay incelemesi

Dört tipik kullanım durumları WebSocket aktarım için aşağıdaki bölümleri inceleyin: **Yankı**, **yayın**, **grubuna Gönder**, ve **bağlantısıGönder**. Her senaryo için Azure SignalR hizmeti geçerli gelen ve giden kapasiteyi bölümünde listelenir. Ayrıca, performansı etkileyen ana faktörler açıklanmaktadır.

Varsayılan modda uygulama sunucusu ile Azure SignalR hizmeti beş sunucu bağlantısı oluşturur. Azure SignalR hizmeti SDK'sı, uygulama sunucusu varsayılan olarak kullanır. Aşağıdaki performans test sonuçları içinde sunucu bağlantısı 15 (veya daha fazla bilgi için yayın ve büyük bir gruba bir ileti gönderme) artırıldı.

Farklı kullanım örnekleri, uygulama sunucuları için farklı gereksinimlere sahiptir. **Yayın** uygulama sunucuları, küçük bir sayı olmalıdır. **Yankı** veya **bağlantı gönderme** birçok uygulama sunucusu gerekir.

Tüm kullanım örnekleri, varsayılan ileti boyutu 2.048 bayttır ve ileti gönderme aralığı 1 saniyedir.

### <a name="default-mode"></a>Varsayılan mod

Azure SignalR hizmeti istemcileri ve web uygulama sunucuları varsayılan modunda faydalanırsınız. Her istemci için tek bir bağlantı gösterir.

#### <a name="echo"></a>echo

İlk olarak, bir web uygulaması için Azure SignalR hizmeti bağlanır. İkinci olarak, istemciler için Azure SignalR hizmeti ile yeniden yönlendiren bir erişim belirteci ve uç noktası ile web uygulaması için birden çok istemci bağlanın. Ardından, istemcilerin Azure SignalR hizmeti WebSocket bağlantı kurar.

Tüm istemcilerin bağlantı kurduktan sonra bunlar her saniyede belirli hub'ına zaman damgası içeren bir ileti göndermeye başlayın. Hub, kendi özgün istemcisine ileti görüntülemektedir. Geri Yankı ileti aldığında, her istemci gecikme hesaplar.

Aşağıdaki diyagramda, bir döngüde 5 ila 8 (kırmızı vurgulanan trafiği) var. Döngü, varsayılan süre (5 dakika) çalışır ve tüm ileti gecikme istatistiği alır.

![Yankı kullanım örneği için trafiği](./media/signalr-concept-performance/echo.png)

Davranışını **Yankı** maksimum gelen bant genişliğini en fazla giden bant genişliğini eşit olduğunu belirler. Ayrıntılar için aşağıdaki tabloya bakın.

|       echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar                       | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Saniye başına gelen/giden iletiler | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Gelen/giden bant genişliği | 2 Mb/sn   | 4 Mb/sn   | 10 Mb/sn  | 20 MB/sn   | 40 MB/sn   | 100 MB/sn  | 200 MB/sn   |

Bu kullanım örneğinde her istemci uygulaması sunucusunda tanımlanan hub'ı çağırır. Hub'ı yalnızca orijinal istemci tarafı içinde tanımlanan yöntem çağırır. Bu hub, hub'ı için en basit olan **Yankı**.

```
        public void Echo(IDictionary<string, object> data)
        {
            Clients.Client(Context.ConnectionId).SendAsync("RecordLatency", data);
        }
```

Uygulama sunucusu trafiği Basıncı bile bu basit hub için belirgin olarak **Yankı** gelen ileti yük artar. Bu trafik baskısı birçok uygulama sunucuları için büyük SKU katmanı gerektirir. Aşağıdaki tabloda, her katman için uygulama sunucu sayısını listeler.


|    echo          | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> İstemci bağlantı sayısı, ileti boyutu, uygulama sunucusunun CPU/bellek oranı ve SKU katmanı gönderme ileti genel performansını etkileyen **Yankı**.

#### <a name="broadcast"></a>Yayın

İçin **yayın**, web uygulaması iletisi aldığında tüm istemcilere yayınlar. Daha fazla istemciye yayınlamak için tüm istemcilere var. daha fazla ileti trafiği var. Aşağıdaki diyagramda bakın.

![Yayın kullanım örneği için trafiği](./media/signalr-concept-performance/broadcast.png)

Az sayıda istemciyi yayın. Gelen ileti bant genişliği küçüktür, ancak giden bant genişliği çok büyük. Giden ileti bant genişliği istemci bağlantısı artırır veya yayın hızı artar.

Aşağıdaki tabloda, en fazla istemci bağlantısı, gelen/giden ileti sayısı ve bant genişliği özetlenmektedir.

|     Yayın             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000  | 100.000 |
| Saniye başına gelen iletiler  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Saniye başına giden iletiler | 2,000 | 4,000 | 10,000 | 20.000 | 40,000 | 100.000 | 200,000 |
| Gelen bant genişliği  | 4 KBps   | 4 KBps   | 4 KBps    | 4 KBps    | 4 KBps    | 4 KBps     | 4 KBps     |
| Giden bant genişliği | 4 Mb/sn   | 8 Mb/sn   | 20 MB/sn   | 40 MB/sn   | 80 MB/sn   | 200 MB/sn   | 400 MB/sn   |

İletiler yayınlayın yayın istemcileri, en fazla dört altındadır. İle karşılaştırıldığında daha az uygulama sunucuları ihtiyaç duydukları **Yankı** gelen mesaj tutarı küçük olduğundan. İki uygulama sunucuları hem SLA ve performans konuları için yeterli altındadır. Ancak, özellikle Unit50 ve Unit100 dengesizliği, önlemek için varsayılan sunucu bağlantıları artırmalısınız.

|   Yayın      | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
> Azure SignalR hizmeti olası dengesiz sunucu bağlantılarını önlemek için her uygulama sunucusunda 40, 5'ten varsayılan sunucu bağlantılarını artırın.
>
> İstemci bağlantı sayısı, ileti boyutu, ileti gönderme hızını ve SKU katmanı için genel performansını etkileyen **yayın**.

#### <a name="send-to-group"></a>Grubuna Gönder

**Grubuna Gönder** kullanım durumda olması için benzer bir trafik desenini **yayın**. Azure SignalR hizmeti ile WebSocket bağlantılarını istemciler kurduktan sonra belirli bir gruba bir ileti göndermeden önce grupları katılmaları gerekir, farktır. Aşağıdaki diyagram, trafik akışını gösterir.

![Trafiği gönderme grubu kullanım durumu için](./media/signalr-concept-performance/sendtogroup.png)

Grup üyesi ve grup sayısı performansı etkileyen iki faktörlerdir. İki tür Grup tanımlarız analizi kolaylaştırmak için:

- **Küçük grup**: Her grup, 10 bağlantılarına sahiptir. Grup numarası (en fazla bağlantı sayısı için) eşittir / 10. 1.000 bağlantı sayısı, varsa, Unit1 için sonra 1000 sahibiz / 10 = 100 grupları.

- **Büyük grup**: Grup numarası her zaman 10'dur. Grup üyesi sayısı (en fazla bağlantı sayısı için) eşittir / 10. 1.000 bağlantı sayısı, varsa örneğin Unit1 için ardından her grubun 1000 sahiptir / 10 = 100 üye.

**Gönderme grubuna** yönlendirme Dağıtılmış veri yapısı ile hedef bağlantıları bulmak sahip olduğu için Azure SignalR hizmeti maliyet getirir. Gönderme bağlantı arttıkça maliyet artar.

##### <a name="small-group"></a>Küçük Grup

Yönlendirme maliyeti, birden çok küçük gruba ileti göndermek için önemlidir. Şu anda Azure SignalR hizmeti uygulaması Unit50 yönlendirme maliyeti sınırında denk gelir. Daha fazla CPU ve bellek eklemeyi Yardım, tasarımı gereği Unit100 geliştirmek olamaz bu nedenle daha fazla değil. Daha fazla gelen bant genişliği gerekiyorsa, müşteri desteğine başvurun.

|   Küçük grup gönderin     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50 | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|--------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000 | 100.000
| Grup üye sayısı        | 10    | 10    | 10     | 10     | 10     | 10     | 10 
| Grup sayısı               | 100   | 200   | 500    | 1000  | 2,000  | 5.000  | 10,000 
| Saniye başına gelen iletiler  | 200   | 400   | 1000  | 2,500  | 4,000  | 7,000  | 7,000   |
| Gelen bant genişliği  | 400 KBps  | 800 KBps  | 2 Mb/sn     | 5 Mb/sn     | 8 Mb/sn     | 14 MB/sn    | 14 MB/sn     |
| Saniye başına giden iletiler | 2,000 | 4,000 | 10,000 | 25,000 | 40,000 | 70,000 | 70,000  |
| Giden bant genişliği | 4 Mb/sn    | 8 Mb/sn    | 20 MB/sn    | 50 MB/sn     | 80 MB/sn    | 140 MB/sn   | 140 MB/sn    |

Uygulama sunucusu numarasını da performansı için kritik olacak şekilde çok sayıda istemci bağlantıları hub'ı arıyoruz. Önerilen uygulama sunucusu sayısı aşağıdaki tabloda listelenmektedir.

|  Küçük grup gönderin   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> İstemci bağlantı sayısı, ileti boyutu, ileti oranı, yönlendirme maliyeti, SKU katmanı ve uygulama sunucusunun CPU/bellek göndermeyi genel performansını etkileyen **küçük bir gruba gönderme**.

##### <a name="big-group"></a>Büyük Grup

İçin **büyük Grup gönderin**, yönlendirme ulaşmaktan sınırı maliyet önce giden bant genişliği performans sorunu haline gelir. Aşağıdaki tabloda neredeyse aynıdır en fazla giden bant için olarak **yayın**.

|    Büyük Grup gönderin      | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000  | 100.000
| Grup üye sayısı        | 100   | 200   | 500    | 1000  | 2,000  | 5.000   | 10,000 
| Grup sayısı               | 10    | 10    | 10     | 10     | 10     | 10      | 10
| Saniye başına gelen iletiler  | 20    | 20    | 20     | 20     | 20     | 20      | 20      |
| Gelen bant genişliği  | 80 KBps   | 40 KBps   | 40 KBps    | 20 KBps    | 40 KBps    | 40 KBps     | 40 KBps     |
| Saniye başına giden iletiler | 2,000 | 4,000 | 10,000 | 20.000 | 40,000 | 100.000 | 200,000 |
| Giden bant genişliği | 8 Mb/sn    | 8 Mb/sn    | 20 MB/sn    | 40 MB/sn    | 80 MB/sn    | 200 MB/sn    | 400 MB/sn    |

Gönderen bağlantısı sayısı en fazla 40 olur. Uygulama sunucusu üzerindeki yükü web apps önerilen sayısını küçük olacak şekilde küçüktür.

|  Büyük Grup gönderin  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
> Azure SignalR hizmeti olası dengesiz sunucu bağlantılarını önlemek için her uygulama sunucusunda 40, 5'ten varsayılan sunucu bağlantılarını artırın.
> 
> İstemci bağlantı sayısı, ileti boyutu, ileti gönderme hızını, yönlendirme maliyeti ve SKU katmanı genel performansını etkileyen **büyük Grup gönderin**.

#### <a name="send-to-connection"></a>Bağlantı Gönder

İçinde **bağlantı gönderme** kullanım örneği, istemcilerin Azure SignalR hizmeti, her istemci bağlantı olduğunda kendi bağlantı kimliğini almak için özel bir hub çağırır Performans Kıyaslama tüm bağlantı kimlikleri toplar, bunları seçimdeki ve bunları tüm istemcilere gönderen hedef olarak yeniden atar. Performans testi tamamlanana kadar istemcilerin hedef bağlantı ileti göndermeye devam.

![Trafiği gönderme istemci kullanım durumu için](./media/signalr-concept-performance/sendtoclient.png)

Yönlendirme maliyeti **bağlantı gönderme** maliyeti benzer **küçük bir gruba gönderme**.

Bağlantı sayısı arttıkça, yönlendirme maliyeti genel performansı sınırlar. Unit50 sınırına ulaştı. Sonuç olarak, Unit100 daha da geliştirmek olamaz.

Aşağıdaki tabloda bir istatistik sonra çalıştırmanın birçok yuvarlar özetidir **bağlantı gönderme** Kıyaslama.

|   Bağlantı Gönder   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50          | Unit100         |
|------------------------------------|-------|-------|-------|--------|--------|-----------------|-----------------|
| Bağlantılar                        | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000          | 100.000         |
| Saniye başına gelen/giden iletiler | 1000 | 2,000 | 5.000 | 8,000  | 9,000  | 20.000 | 20.000 |
| Gelen/giden bant genişliği | 2 Mb/sn    | 4 Mb/sn    | 10 Mb/sn   | 16 MB/sn    | 18 MB/sn    | 40 MB/sn       | 40 MB/sn       |

Bu kullanım örneği uygulama sunucu tarafında yüksek yük gerektirir. Önerilen uygulama sunucusu aşağıdaki tabloda sayısı.

|  Bağlantı Gönder  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> İstemci bağlantı sayısı, ileti boyutu, ileti oranı, yönlendirme maliyeti, SKU katmanı ve CPU/bellek uygulama sunucusu göndermeyi genel performansını etkileyen **bağlantı gönderme**.

#### <a name="aspnet-signalr-echo-broadcast-and-send-to-small-group"></a>ASP.NET SignalR Yankı, yayın ve küçük bir grup Gönder

Azure SignalR hizmeti ASP.NET SignalR için aynı performans kapasitesi sağlar. 

Azure Web uygulamaları performans testi kullanan [standart hizmet planı S3](https://azure.microsoft.com/pricing/details/app-service/windows/) ASP.NET SignalR için.

Aşağıdaki tabloda, ASP.NET SignalR için önerilen web uygulama sayısı gösterilir **Yankı**.

|   echo           | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

Aşağıdaki tabloda, ASP.NET SignalR için önerilen web uygulama sayısı gösterilir **yayın**.

|  Yayın       | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

Aşağıdaki tabloda, ASP.NET SignalR için önerilen web uygulama sayısı gösterilir **küçük bir gruba gönderme**.

|  Küçük grup gönderin     | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

### <a name="serverless-mode"></a>Sunucusuz modu

İstemcileri ve Azure SignalR hizmeti sunucusuz modunda faydalanırsınız. Her istemci için tek bir bağlantı gösterir. İstemci, başka bir istemci veya yayın iletilerine tüm REST API aracılığıyla iletileri gönderir.

REST API aracılığıyla yüksek yoğunluklu gönderme WebSocket kullanmak kadar verimli değildir. Her seferinde yeni bir HTTP bağlantı oluşturmanızı gerektirir ve ek ücret sunucusuz modunda.

#### <a name="broadcast-through-rest-api"></a>REST API aracılığıyla yayın
Tüm istemcilerin Azure SignalR hizmeti WebSocket bağlantı kurar. Daha sonra bazı istemciler, REST API aracılığıyla yayın başlatın. (Gelen) gönderme WebSocket ile karşılaştırıldığında etkin değilse HTTP Post aracılığıyla iletisidir.

|   REST API aracılığıyla yayın     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000  | 100.000 |
| Saniye başına gelen iletiler  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Saniye başına giden iletiler | 2,000 | 4,000 | 10,000 | 20.000 | 40,000 | 100.000 | 200,000 |
| Gelen bant genişliği  | 4 KBps    | 4 KBps    | 4 KBps     | 4 KBps     | 4 KBps     | 4 KBps      | 4 KBps      |
| Giden bant genişliği | 4 Mb/sn    | 8 Mb/sn    | 20 MB/sn    | 40 MB/sn    | 80 MB/sn    | 200 MB/sn    | 400 MB/sn    |

#### <a name="send-to-user-through-rest-api"></a>REST API aracılığıyla kullanıcı gönderin
Azure SignalR hizmetine başlamadan önce Kıyaslama tüm istemcilerin kullanıcı adlarını atar. Azure SignalR hizmeti ile WebSocket bağlantılarını istemcilerin kurduktan sonra bunlar iletileri başkalarına HTTP Post göndermeye başlayın.

|   REST API aracılığıyla kullanıcı gönderin | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000  | 100.000 |
| Saniye başına gelen iletiler  | 300   | 600   | 900    | 1,300  | 2,000  | 10,000  | 18,000  |
| Saniye başına giden iletiler | 300   | 600   | 900    | 1,300  | 2,000  | 10,000  | 18,000 |
| Gelen bant genişliği  | 600 KBps  | 1.2 MB/sn  | 1.8 MB/sn   | 2.6 MB/sn   | 4 Mb/sn     | 10 Mb/sn     | 36 MB/sn    |
| Giden bant genişliği | 600 KBps  | 1.2 MB/sn  | 1.8 MB/sn   | 2.6 MB/sn   | 4 Mb/sn     | 10 Mb/sn     | 36 MB/sn    |

## <a name="performance-test-environments"></a>Performans ve test ortamları

Tüm daha önce listelenen kullanım, size Azure ortamınızda performans testlerini yürütülür. En fazla 50 istemci VM'ler ve 20 uygulama sunucusu sanal makineleri kullanılır. Aşağıda, bazı ayrıntılar verilmiştir:

- İstemci VM boyutu: StandardDS2V2 (2 vCPU, bellek 7G)

- Uygulama sunucusu VM boyutu: StandardF4sV2 (4 vCPU, bellek 8G)

- Azure SignalR SDK sunucu bağlantısı: 15

## <a name="performance-tools"></a>Performans araçları

Üzerinde Azure SignalR hizmeti için performans araçları bulabilirsiniz [GitHub](https://github.com/Azure/azure-signalr-bench/tree/master/SignalRServiceBenchmarkPlugin).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure SignalR hizmeti performans tipik kullanım örneği senaryoları için genel bir bakış alındı.

Hizmet ve için ölçeklendirme iç işlevleri hakkında bilgi edinmek için aşağıdaki kılavuzlara okuyun:

* [Azure SignalR hizmeti dahili bileşenleri](signalr-concept-internals.md)
* [Azure SignalR hizmeti ölçeklendirme](signalr-howto-scale-multi-instances.md)
