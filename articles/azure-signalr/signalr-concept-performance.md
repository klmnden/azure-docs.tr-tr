---
title: Azure SignalR hizmeti için performans Kılavuzu
description: Azure SignalR Service'nın performans genel bakış.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: zhshang
ms.openlocfilehash: 53139dd253c491ea6578fd0b9cbada4e7b331c7d
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59502046"
---
# <a name="performance-guide-for-azure-signalr-service"></a>Azure SignalR hizmeti için performans Kılavuzu

Azure SignalR hizmeti kullanmak için önemli avantajlarından biri, SignalR uygulamalarını ölçeklendirme kolaylığıdır. Büyük ölçekli bir senaryoda performansı önemli bir etmen haline gelir. Bu kılavuzda biz altında farklı kullanım örneği senaryoları, tipik performans nedir ve SignalR uygulama performansı etkiler faktörleri tanıtacak? Sonunda, biz de performans raporu oluşturmak için kullanılan araçlar ve ortamı başlatacaktır.

## <a name="terms-definition"></a>Koşulları tanımı

*ASRS*: Azure SignalR Service

*Gelen*: Azure SignalR hizmeti için gelen ileti

*Giden*: Azure SignalR Hizmeti giden ileti

*Bant genişliği*: 1 saniye toplam boyutu tüm iletileri

*Varsayılan mod*: Tüm istemci bağlantılarını kabul önce onunla bağlantı kurmak için uygulama sunucusu ASRS bekliyor. Bir ASRS oluşturulduğunda, varsayılan çalışma modudur.

*Sunucusuz modu*: ASRS yalnızca istemci bağlantılarını kabul eder. Hiçbir sunucu bağlantısı izin verilir.

## <a name="overview"></a>Genel Bakış

Yedi standart katmanları için farklı performans kapasiteler ASRS tanımlar ve şu soruları yanıtlamak bu kılavuzu amaçlamaktadır:

-   Her katman için tipik ASRS performans nedir?

-   Saniyede 100.000 gönderme ASRS ileti işleme hızı, örneğin, Belgelerim gereksinimi karşılar mı?

-   Hangi katmanı benim için uygun olan my belirli senaryo için? Veya nasıl uygun katman seçebilirsiniz?

-   Ne tür bir uygulama sunucusu (sanal makine boyutu) benim için uygun olan ve kaç tanesinin gelecektir dağıtabilirim?

Bu soruları yanıtlamak için bu performans kılavuzunu ilk performansı etkiler Etkenler hakkında üst düzey bir açıklama sunar ve tipik kullanım durumları için her katman için en fazla gelen ve giden iletileri gösterir: **echo**, **yayın**, **gönderme grubu**, ve **gönderme bağlantı** (eşler arası sohbet).

Bu belge için tüm senaryoları (ve farklı kullanım örneği, farklı bir ileti boyutu veya ileti gönderen deseni vs.) karşılamak mümkün değildir. Ancak, yaklaşık kendi gereksinimine gelen veya giden iletilerin değerlendirmek ve ardından performans tablonuzu kontrol ederek doğru katmanları Bul kullanıcılara yardımcı olmak için bazı değerlendirme yöntemleri sağlar.

## <a name="performance-insight"></a>Performansı içgörüleri

Bu bölümde performans değerlendirme yöntemleri açıklar ve performansı etkiler tüm faktörleri listeler. Sonunda, performans gereksinimlerini değerlendirmek için yöntemler sağlar.

### <a name="methodology"></a>Yöntemi

**Aktarım hızı** ve **gecikme** olan iki tipik yönlerini performans denetleniyor. ASRS için farklı SKU katmanı farklı aktarım hızı azaltma ilkesi vardır. Bu belge tanımlar **izin verilen en fazla aktarım hızı (gelen ve giden bant genişliği)** iletilerin %99 1 saniyeden az gecikme süresi olduğunda en fazla elde edilen aktarım hızı gibi.

Gecikme süresi zaman aralığını ASRS yanıt iletisi almayı ileti gönderen bağlantının ' dir. Alalım **Yankı** örnek olarak, her istemci bağlantı bir zaman damgası iletisinde ekler. Uygulama sunucusu hub özgün iletinin istemciye geri gönderir. Bu nedenle yayma gecikme kolayca her istemci bağlantısı tarafından hesaplanır. Zaman damgası her ileti için bağlı **yayın**, **gönderme grubu**, ve **gönderme bağlantı**.

Eşzamanlı istemciler bağlantıları binlerce benzetimini yapmak için azure'da özel bir sanal ağda birden çok VM oluşturulur. Tüm bu VM'lerin aynı ASRS örneğine bağlanın.

ASRS varsayılan modunda uygulama sunucusu Vm'leri ayrıca dağıtılan istemci Vm'leri aynı sanal özel ağ içinde.

Tüm istemci Vm'leri ve uygulama sunucusu bölge gecikmeyi önlemek için aynı bölgede aynı ağındaki VM'ler dağıtılır.

### <a name="performance-factors"></a>Performans etmenleri

Teorik olarak, ASRS kapasite tarafından kullanılan hesaplama kaynağı sınırlıdır: CPU, bellek ve ağ. Örneğin, daha fazla bağlantı ASRS için daha fazla bellek ASRS tüketilen. Büyük ileti trafiği, örneğin, her ileti 2048 bayttan büyük, de işlemek için daha fazla CPU döngülerini kullanması ASRS gerektirir. Bu arada, Azure ağ bant genişliği, ayrıca maksimum trafiği için bir sınır uygular.

Aktarım Türü [WebSocket](https://en.wikipedia.org/wiki/WebSocket), [sunucu gönderilen olay](https://en.wikipedia.org/wiki/Server-sent_events), veya [uzun yoklaması](https://en.wikipedia.org/wiki/Push_technology), olan başka bir faktör performansı etkiler. WebSocket tek bir TCP bağlantısı bir yönlü ve çift yönlü iletişimi protokolüdür. Ancak, sunucu gönderilen olay tek yönlü anında iletme iletisi için sunucudan istemciye protokolüdür. Uzun yoklaması HTTP isteği aracılığıyla sunucu bilgileri düzenli aralıklarla yoklamaya istemcilerin gerektirir. Aynı koşul altında aynı API, WebSocket en iyi performansa sahip olduğundan, sunucu gönderilen olay yavaştır ve uzun yoklama en yavaş olduğu. ASRS WebSocket varsayılan olarak önerir.

Ayrıca, ileti yönlendirme maliyeti de performans sınırlandırır. ASRS, diğer istemciler veya sunucular için istemciler veya sunucular kümesi iletiden yönlendiren bir ileti yönlendirici olarak bir rol oynar. Farklı senaryonuz ya da API farklı yönlendirme ilkesi gerektirir. İçin **Yankı**istemci kendisine bir ileti gönderir ve yönlendirme hedefi kendisi de değildir. Bu düzen, en düşük yönlendirme maliyeti vardır. Ancak **yayın**, **gönderme grubu**, **gönderme bağlantı**, ASRS gereken iç Dağıtılmış veri yapısı ile hedef bağlantıları aramak, Daha fazla CPU, bellek ve hatta ağ bant genişliği tüketir. Sonuç olarak, performans daha yavaş **Yankı**.

Azure SignalR SDK Hub'ı her istemci sinyal sinyalleri aracılığıyla Canlı bağlantıyla tutar gerçekleşecek. Bu sırada çağrılacak olduğundan varsayılan modunda uygulama sunucusu Ayrıca belirli senaryolar için performans sorunlarına.

Sunucusuz modunda istemci tarafından WebSocket kadar verimli olmayan HTTP post iletisi gönderir.

Başka bir faktördür protokolü: JSON ve [MessagePack](https://msgpack.org/index.html). MessagePack JSON daha küçük boyutlu ve teslim edilen daha hızlıdır. Sezgisel, MessagePack performans avantaj elde edecektir, ancak bu ileti yükü sunucular ya da tam tersi istemcilerden gelen ileti iletme sırasında çözmüyor beri ASRS performans kurallarıyla ilgili bir küçük harf duyarlı değildir.

Özet olarak, aşağıdaki etmenlere gelen ve giden kapasite temel etkileri vardır:

-   SKU Katmanı (CPU/bellek)

-   Bağlantı sayısı

-   İleti boyutu

-   ileti gönderme oranı

-   Aktarım Türü (WebSocket/sunucu-gönderilen-olay/uzun yoklaması)

-   Kullanım örneği senaryosu (yönlendirme maliyeti)

-   Uygulama sunucusu ve hizmet bağlantıları (sunucu modunda)


### <a name="find-a-proper-sku"></a>Uygun bir SKU Bul

Gelen/giden kapasite değerlendirme veya hangi katmanda belirli kullanım örneği için uygun olduğunu nasıl?

Uygulama sunucusu yeterince güçlü ve performans sorunu değil varsayıyoruz. Daha sonra size her katman için en büyük gelen ve giden bant kontrol edebilirsiniz.

#### <a name="quick-evaluation"></a>Hızlı değerlendirme

Şimdi değerlendirme önce bazı varsayılan ayarların olduğunu düşünerek tarafından kolaylaştırma: WebSocket kullanılır, ileti boyutu 2048 bayt cinsinden her 1 saniyede ileti gönderme ve varsayılan modu.

Her katman kendi gelen en yüksek bant genişliği ve giden bant genişliği var. Gelen veya giden sınırı aştığında kesintisiz kullanıcı deneyimi garanti edilmez.

**Yankı** düşük yönlendirme maliyeti olduğundan en fazla gelen bant genişliği sağlar. **Yayın** en fazla giden ileti bant genişliğini tanımlar.

Yapmak **değil** aşağıdaki iki tabloda vurgulanan değerler aşıyor.

|       echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar                       | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| **Gelen bant genişliği (bayt/sn)** | **2 DK**    | **4 DK**    | **10M**   | **20 MİLYON**    | **40M**    | **100 MİLYON**   | **200 MİLYON**    |
| Giden bant genişliği (bayt/sn) | 2 DK    | 4 DK    | 10M   | 20 MİLYON    | 40M    | 100 MİLYON   | 200 MİLYON    |


|     Yayın             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000  | 100.000 |
| Gelen bant genişliği (bayt/sn)  | 4K    | 4K    | 4K     | 4K     | 4K     | 4K      | 4K     |
| **Giden bant genişliği (bayt/sn)** | **4 DK**    | **8 DK**    | **20 MİLYON**    | **40M**    | **80 MİLYON**    | **200 MİLYON**    | **400 MİLYON**   |

Gelen bant genişliği ve giden bant genişliğini formül:
```
  inboundBandwidth = inboundConnections * messageSize / sendInterval
  outboundBandwidth = outboundConnections * messageSize / sendInterval
```

*inboundConnections*: ileti gönderme bağlantı sayısı

*outboundConnections*: ileti alma bağlantı sayısı

*messageSize*: (ortalama değer) tek bir ileti boyutu. Küçük ileti boyutu 1024 bayttan daha az olduğu için 1024 bayt iletisi olarak benzer performans etkisi vardır.

*sendInterval*: tek bir ileti gönderme genellikle, her saniye bir ileti gönderilirken anlamına gelir, ileti başına 1 saniye zamandır. Daha küçük sendInterval daha fazla ileti süre verilen gönderme anlamına gelir. Örneğin, ileti başına 0,5 ikinci saniyede iki ileti gönderme anlamına gelir.

*Bağlantıları* ASRS taahhüt edilen en yüksek eşik her katman için. Daha fazla bağlantı sayısı artar, gelen bağlantı daraltma düşer.

*Gelen bant genişliği* ve *giden bant genişliğini* saniye başına toplam ileti boyutu. Burada 'M ' megabayt kolaylık olması için anlamına gelir.

#### <a name="evaluation-for-complex-use-cases"></a>Karmaşık kullanım örnekleri için değerlendirme

##### <a name="bigger-message-size-or-different-sending-rate"></a>Büyük ileti boyutu veya farklı gönderme oranı

Gerçek kullanım durumu daha karmaşıktır. İleti 2048 bayt daha büyük gönderebilir veya saniye başına değil bir mesaj gönderen ileti oranıdır. Unit100'ın yayın performansını değerlendirmek nasıl bulmak için örnek olarak alalım.

Aşağıdaki tablo, gerçek bir durumu gösterir **yayın**, ancak ileti boyutu, bağlantı sayısı ve ileti gönderirken oranı ne biz önceki bölümde kabul öğesinden farklıdır. Biz yalnızca 2 tanesi biliyorsanız nasıl biz (ileti boyutu, bağlantı sayısı veya ileti gönderme hızını) bu öğelerden herhangi birini çıkarabilir soru şudur.

| Yayın  | İleti boyutu (bayt) | Gelen (İleti/sn) | Bağlantılar | Aralık (saniye) Gönder |
|---|---------------------|--------------------------|-------------|-------------------------|
| 1 | 20 BİN                 | 1                        | 100.000     | 5                       |
| 2 | 256 K                | 1                        | 8,000       | 5                       |

Şu formül olarak ayarlayın, kolayca üzerinde var olan bir önceki formül tabanlı olayla şöyledir:

```
outboundConnections = outboundBandwidth * sendInterval / messageSize
```

Unit100 için en fazla giden bant genişliğini 400 milyon önceki tablosundan olduğu ve 20-K ileti boyutu için en fazla giden bağlantı 400 milyon olmalıdır biliyoruz \* 5 / 20 bin = gerçek değeri ile eşleşen 100.000.

##### <a name="mixed-use-cases"></a>Karma kullanım örnekleri

Birlikte, genellikle karıştırır dört temel kullanım durumları gerçek kullanım örneği: **Yankı**, **yayın**, **grubuna Gönder**, veya **bağlantı gönderme**. Karma kullanım örneklerini dört temel kullanım örneklerine bölmek için kapasitesini değerlendirmek için kullanılan yöntemi olan **en fazla ileti gelen ve giden bant genişliğini hesaplamak** yukarıdaki formüller üzerinden ayrı olarak ve bunları toplam almak için en fazla gelen/giden bant genişliği. En fazla gelen/giden bant genişliği tablolardan uygun katmanındaki ardından seçin.

Bu arada, yüzlerce veya binlerce küçük gruplar veya binlerce istemciden birbirlerine ileti göndermek için ileti göndermek için yönlendirme maliyeti baskın olur. Bu etkiyi göz önünde bulundurulması. Daha fazla ayrıntı, aşağıdaki "İncelemesini" bölümlerde ele alınmıştır.

Kullanım örneği istemcilere ileti gönderme uygulama sunucusu olduğundan emin olun **değil** performans sorunu. "Örnek olay incelemesi" bölümü, kaç uygulama sunucuları hakkında ihtiyacınız olduğunu ve kaç sunucu bağlantılarını yapılandırılmalıdır kılavuz sağlar.

## <a name="case-study"></a>Örnek olay incelemesi

Dört tipik kullanım durumları WebSocket aktarım için aşağıdaki bölümleri inceleyin: **Yankı**, **yayın**, **gönderme grubu**, ve **gönderme bağlantı** . İsteğe bağlı olarak her senaryo için geçerli ASRS listeler gelen ve giden kapasite, bu arada anlatan ana Etkenler performansı nedir.

Varsayılan modu, varsayılan olarak, Azure SignalR hizmeti SDK'sı aracılığıyla uygulama sunucusu ile ASRS beş sunucu bağlantısı oluşturur. Test sonucu aşağıdaki sunucu bağlantıları 15 (veya daha fazla bilgi için yayın ve büyük gruba ileti gönderilmesi) artırılmış performans.

Farklı kullanım durumu, uygulama sunucularında farklı gereksinim vardır. **Yayın** uygulama sunucuları, küçük bir sayı olmalıdır. **Yankı** veya **gönderme bağlantı** birçok uygulama sunucusu gerekir.

Tüm kullanım örnekleri, varsayılan ileti boyutu 2048 bayt olabilir ve ileti gönderme aralığı 1 saniyedir.

## <a name="default-mode"></a>Varsayılan mod

Bu modda, istemcilerin, web uygulama sunucuları ve ASRS faydalanırsınız. Her istemci için tek bir bağlantı gösterir.

### <a name="echo"></a>echo

İlk olarak, web uygulamaları için ASRS bağlayın. İkincisi, birden çok istemci web uygulaması için erişim belirteci ve uç noktası ile ASRS için istemcilerin yeniden yönlendirme, bağlanın. Ardından, istemciler ASRS WebSocket bağlantı kurar.

Tüm istemcilerin bağlantı kurduktan sonra bunlar her saniyede belirli hub'ına zaman damgası içeren bir ileti göndermeye başlayın. Hub, kendi özgün istemcisine ileti görüntülemektedir. Geri Yankı ileti aldığında, her istemci gecikme hesaplar.

Adım 5\~8 (kırmızı vurgulanan trafiği) olan bir döngüde, varsayılan süre (5 dakika) çalıştırın ve tüm ileti gecikme istatistiği edinin.
En yüksek performans kılavuzunu gösterir istemci bağlantı sayısı.

![echo](./media/signalr-concept-performance/echo.png)

**Yankı**ın davranışını maksimum gelen bant genişliğini en fazla giden bant genişliğini eşit olduğunu belirler. Aşağıdaki tabloya bakın.

|       echo                        | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar                       | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Gelen/giden (İleti/sn) | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Gelen/giden bant genişliği (bayt/sn) | 2 DK    | 4 DK    | 10M   | 20 MİLYON    | 40M    | 100 MİLYON   | 200 MİLYON    |

Bu kullanım örneğinde her istemci uygulaması sunucusunda tanımlanan hub'ı çağırır. Hub'ı yalnızca orijinal istemci tarafı içinde tanımlanan yöntem çağırır. Bu hub en hafif tartılan hub için olan **Yankı**.

```
        public void Echo(IDictionary<string, object> data)
        {
            Clients.Client(Context.ConnectionId).SendAsync("RecordLatency", data);
        }
```

Bile bu basit hub için uygulama sunucusu trafiği Basıncı da belirgin olarak **Yankı** gelen ileti artar. Bu nedenle, çok sayıda uygulama sunucuları için büyük SKU katmanı gerektirir. Aşağıdaki tabloda, her katman için uygulama sunucu sayısını listeler.


|    echo          | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
>
> İstemci bağlantı sayısı, ileti boyutu, ileti oranı, SKU katmanı ve uygulama sunucusunun CPU/bellek göndermeyi sahip etkisi üzerinde genel performansını **Yankı**.

### <a name="broadcast"></a>Yayın

İçin **yayın**, web uygulaması iletisi aldığında tüm istemcilere yayınlar. Daha fazla istemciye yayınlamak için tüm istemciler için daha fazla ileti trafiği. Aşağıdaki diyagramda bakın.

![Yayın](./media/signalr-concept-performance/broadcast.png)

Gelen ileti bant genişliği küçük olduğu anlamına gelir, küçük bir sayı yayın istemcilerin vardır, ancak giden bant genişliği çok büyük olduğundan yayın özelliğidir. Giden ileti bant genişliği istemci bağlantısı artırır veya yayın hızı artar.

En fazla istemci bağlantısı, gelen/giden ileti sayısı ve bant genişliği aşağıdaki tabloda özetlenmiştir.

|     Yayın             | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000  | 100.000 |
| Gelen (İleti/sn)  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Giden (İleti/sn) | 2,000 | 4,000 | 10,000 | 20.000 | 40,000 | 100.000 | 200,000 |
| Gelen bant genişliği (bayt/sn)  | 4K    | 4K    | 4K     | 4K     | 4K     | 4K      | 4K      |
| Giden bant genişliği (bayt/sn) | 4 DK    | 8 DK    | 20 MİLYON    | 40M    | 80 MİLYON    | 200 MİLYON    | 400 MİLYON    |

İletiler yayınlayın yayın istemcilerin en fazla 4, böylece daha az uygulama sunucuları ile karşılaştırıldığında gerektirir **Yankı** gelen mesaj tutarı küçük olduğundan. İki uygulama yeterli SLA'sı hem de performans göz önünde bulundurmanız gereken sunucularıdır. Ancak, özellikle Unit50 ve Unit100 için dengesiz sorunu önlemek için varsayılan sunucu bağlantıları artırdık.

|   Yayın      | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
>
> Varsayılan sunucu bağlantılarını 5'ten 40 ASRS olası dengesiz sunucu bağlantılarını önlemek için her uygulama sunucusunda artırın.
>
> İstemci bağlantı sayısı, ileti boyutu, ileti gönderme hızını ve SKU katmanı için genel performans üzerindeki etkisi **yayın**

### <a name="send-to-group"></a>Grubuna Gönder

**Gönderme grubu** belirli bir gruba ileti göndermek için önce istemcileri ASRS ile WebSocket bağlantı kurma sonra grupları katılmaları gerekir dışında benzer trafik desenini sahiptir. Trafik akışı tarafından Aşağıdaki diyagramda gösterilmiştir.

![Grup gönderin](./media/signalr-concept-performance/sendtogroup.png)

Grup üyesi ve grup sayısı performans üzerindeki etkisi olan iki faktörlerdir. İki tür Grup tanımlarız analizi kolaylaştırmak için: küçük bir grup ve büyük grup.

- `small group`: Her Grup 10 bağlantıları. Grup numarası (en fazla bağlantı sayısı için) eşittir / 10. 1000 bağlantı sayısı, varsa, birim 1, ardından 1000 sahibiz / 10 = 100 grupları.

- `Big group`: Grup numarası her zaman 10'dur. Grup üyesi sayısı (en fazla bağlantı sayısı için) eşittir / 10. Vardır 1000 bağlantı sayısı, örneğin, birim 1, ardından her grubun 1000 sahipse / 10 = 100 üye.

**Gönderme grubu** Dağıtılmış veri yapısı ile hedef bağlantıları bulmak sahip olduğu için ASRS yönlendirme maliyet getirir. Gönderen bağlantıları arttıkça maliyet de artar.

#### <a name="small-group"></a>Küçük Grup

Yönlendirme maliyeti, birden çok küçük gruba ileti göndermek için önemlidir. Şu anda ASRS uygulama unit50 yönlendirme maliyeti sınırında denk gelir. Daha fazla CPU ve bellek eklemeyi değil Yardım, tasarımı gereği unit100 geliştirmek olamaz bu nedenle daha fazla. Daha fazla gelen bant genişliği istiyorsa, özelleştirme için müşteri desteğine başvurun.

|   Küçük grup gönderin     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50 | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|--------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000 | 100.000
| Grup üye sayısı        | 10    | 10    | 10     | 10     | 10     | 10     | 10 
| Grup sayısı               | 100   | 200   | 500    | 1000  | 2,000  | 5.000  | 10,000 
| Gelen (İleti/sn)  | 200   | 400   | 1000  | 2,500  | 4,000  | 7,000  | 7,000   |
| Gelen bant genişliği (bayt/sn)  | 400 BİNDEN  | 800 K  | 2 DK     | 5 DK     | 8 DK     | 14 DK    | 14 DK     |
| Giden (İleti/sn) | 2,000 | 4,000 | 10,000 | 25,000 | 40,000 | 70,000 | 70,000  |
| Giden bant genişliği (bayt/sn) | 4 DK    | 8 DK    | 20 MİLYON    | 50 MİLYON     | 80 MİLYON    | 140M   | 140M    |

Hub çağırma birçok istemci bağlantıları vardır, bu nedenle, uygulama sunucusu da performansı için kritik bir sayıdır. Önerilen uygulama sunucusu sayısı aşağıdaki tabloda listelenir.

|  Küçük grup gönderin   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
>
> İstemci bağlantı sayısı, ileti boyutu, ileti gönderme hızını, yönlendirme maliyeti, SKU katmanı ve uygulama sunucusunun CPU/bellek genel performansı etkisi **gönderme için küçük grubu**.

#### <a name="big-group"></a>Büyük Grup

İçin **gönderme büyük-gruba**, yönlendirme ulaşmaktan sınırı maliyet önce giden bant genişliği performans sorunu haline gelir. Aşağıdaki tabloda neredeyse aynıdır en fazla giden bant olarak **yayın**.

|    Büyük Grup gönderin      | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000  | 100.000
| Grup üye sayısı        | 100   | 200   | 500    | 1000  | 2,000  | 5.000   | 10,000 
| Grup sayısı               | 10    | 10    | 10     | 10     | 10     | 10      | 10
| Gelen (İleti/sn)  | 20    | 20    | 20     | 20     | 20     | 20      | 20      |
| Gelen bant genişliği (bayt/sn)  | 80 K   | 40 BİN   | 40 BİN    | 20 BİN    | 40 BİN    | 40 BİN     | 40 BİN     |
| Giden (İleti/sn) | 2,000 | 4,000 | 10,000 | 20.000 | 40,000 | 100.000 | 200,000 |
| Giden bant genişliği (bayt/sn) | 8 DK    | 8 DK    | 20 MİLYON    | 40M    | 80 MİLYON    | 200 MİLYON    | 400 MİLYON    |

Gönderen bağlantısı sayısı en fazla 40'tır. uygulama sunucu üzerindeki yükü küçük, bu nedenle önerilen web uygulama de küçük bir sayıdır.

|  Büyük Grup gönderin  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
>
> Varsayılan sunucu bağlantılarını 5'ten 40 ASRS olası dengesiz sunucu bağlantılarını önlemek için her uygulama sunucusunda artırın.
> 
> İstemci bağlantı sayısı, ileti boyutu, ileti gönderme hızını, yönlendirme maliyeti ve SKU katmanı genel performansı etkisi **gönderme büyük-gruba**.

### <a name="send-to-connection"></a>Bağlantı Gönder

Bu kullanım örneğinde istemciler ASRS, bağlantı, her istemci kendi bağlantı kimliğini almak için özel bir hub çağırır. Tüm bağlantı kimlikleri toplayın, bunları karışık ve bunları tüm istemcilere gönderen hedef olarak yeniden atama için performans Kıyaslama sorumludur. Performans testi tamamlanana kadar istemcilerin hedef bağlantı ileti göndermeye devam.

![İstemci gönderme](./media/signalr-concept-performance/sendtoclient.png)

Yönlendirme maliyeti **gönderme bağlantı** benzer olarak **gönderme için küçük grubu**.

Bağlantı sayısı arttıkça, genel performans yönlendirme maliyeti sınırlıdır. Birim 50 sınırına ulaştı. Sonuç olarak, birim 100 daha da geliştirmek olamaz.

Aşağıdaki tabloda bir Özet istatistikleri sonra çalıştırmanın birçok yuvarlar olan **gönderme bağlantı** Kıyaslama

|   Bağlantı Gönder   | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50          | Unit100         |
|------------------------------------|-------|-------|-------|--------|--------|-----------------|-----------------|
| Bağlantılar                        | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000          | 100.000         |
| Gelen / giden (İleti/sn) | 1000 | 2,000 | 5.000 | 8,000  | 9,000  | 20.000 | 20.000 |
| Gelen / giden bant genişliği (bayt/sn) | 2 DK    | 4 DK    | 10M   | 16 MİLYON    | 18M    | 40M       | 40M       |

Bu kullanım örneği uygulama sunucu tarafında yüksek yük gerektirir. Önerilen uygulama sunucusu aşağıdaki tabloda sayısı.

|  Bağlantı Gönder  | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
>
> İstemci bağlantı sayısı, ileti boyutu, ileti gönderme hızını, yönlendirme maliyeti, SKU katmanı ve uygulama sunucusunun CPU/bellek genel performansı etkisi **gönderme bağlantı**.

### <a name="aspnet-signalr-echobroadcastsend-to-connection"></a>ASP.NET SignalR Yankı/yayın/Gönder-bağlantı

ASRS ASP.NET SignalR için aynı performans kapasitesi sağlar. Bu bölümde, ASP.NET SignalR için önerilen web uygulama sayısı verir **Yankı**, **yayın**, ve **gönderme için küçük grubu**.

Azure Web uygulamasında performans testi kullanan [standart hizmet planı S3](https://azure.microsoft.com/pricing/details/app-service/windows/) ASP.NET SignalR için.

- `echo`

|   echo           | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

- `broadcast`

|  Yayın       | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

- `Send-to-small-group`

|  Küçük grup gönderin     | Unit1 | Unit2 | Unit5 | Unit10 | Unit20 | Unit50 | Unit100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Bağlantılar      | 1000 | 2,000 | 5.000 | 10,000 | 20.000 | 50,000 | 100.000 |
| Uygulama sunucusu sayısı | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

## <a name="serverless-mode"></a>Sunucusuz modu

Bu modda, istemciler ve ASRS faydalanırsınız. Her istemci için tek bir bağlantı gösterir. İstemci, başka bir istemci veya yayın iletilerine tüm REST API aracılığıyla iletileri gönderir.

REST API aracılığıyla yüksek yoğunluklu ileti gönderme, her seferinde - ek ücret sunucusuz modunda yeni bir HTTP bağlantısı oluşturmak için gerektirdiğinden WebSocket kadar verimli değildir.

### <a name="broadcast-through-rest-api"></a>REST API aracılığıyla yayın
Tüm istemciler ASRS WebSocket bağlantı kurar. Daha sonra bazı istemciler, REST API aracılığıyla yayın başlatın. İleti (gelen) göndermeyi olan WebSocket ile karşılaştırıldığında etkin değilse HTTP Post aracılığıyla.

|   REST API aracılığıyla yayın     | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000  | 100.000 |
| Gelen (İleti/sn)  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Giden (İleti/sn) | 2,000 | 4,000 | 10,000 | 20.000 | 40,000 | 100.000 | 200,000 |
| Gelen bant genişliği (bayt/sn)  | 4K    | 4K    | 4K     | 4K     | 4K     | 4K      | 4K      |
| Giden bant genişliği (bayt/sn) | 4 DK    | 8 DK    | 20 MİLYON    | 40M    | 80 MİLYON    | 200 MİLYON    | 400 MİLYON    |

### <a name="send-to-user-through-rest-api"></a>REST API aracılığıyla kullanıcı gönderin
Kıyaslama kullanıcı adları için ASRS bağlanma başlamadan önce tüm istemcilerin atar. İstemcilerin ASRS WebSocket bağlantılarıyla belirttikten sonra bunlar iletileri başkalarına HTTP Post göndermeye başlayın.

|   REST API aracılığıyla kullanıcı gönderin | Unit1 | Unit2 | Unit5  | Unit10 | Unit20 | Unit50  | Unit100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Bağlantılar               | 1000 | 2,000 | 5.000  | 10,000 | 20.000 | 50,000  | 100.000 |
| Gelen (İleti/sn)  | 300   | 600   | 900    | 1,300  | 2,000  | 10,000  | 18,000  |
| Giden (İleti/sn) | 300   | 600   | 900    | 1,300  | 2,000  | 10,000  | 18,000 |
| Gelen bant genişliği (bayt/sn)  | 600 K  | 1,2 MİLYON  | 1.8 M   | 2.6 M   | 4 DK     | 10M     | 36M    |
| Giden bant genişliği (bayt/sn) | 600 K  | 1,2 MİLYON  | 1.8 M   | 2.6 M   | 4 DK     | 10M     | 36M    |

## <a name="performance-test-environments"></a>Performans ve test ortamları

Yukarıda listelenen tüm kullanım durumları için performans testi Azure ortamında yürütülür. VM'ler, en çok 50 istemci Vm'leri ve 20 uygulama sunucusu kullanılır.

İstemci VM boyutu: StandardDS2V2 (2 vCPU, bellek 7G)

Uygulama sunucusu VM boyutu: StandardF4sV2 (4 vCPU, bellek 8G)

Azure SignalR SDK sunucu bağlantısı: 15

## <a name="performance-tools"></a>Performans araçları

https://github.com/Azure/azure-signalr-bench/tree/master/SignalRServiceBenchmarkPlugin

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, SignalR hizmet performansı tipik kullanım örneği senaryolarının genel bir bakış edinin.

SignalR hizmet ve SignalR hizmeti için ölçeklendirme iç işlevleri hakkında daha fazla bilgi edinmek için aşağıdaki kılavuzu okuyun.

* [Azure SignalR hizmeti iç işlevleri](signalr-concept-internals.md)
* [Azure SignalR hizmeti ölçeklendirme](signalr-howto-scale-multi-instances.md)