---
title: Azure CDN kurallar Altyapısı X EC Debug HTTP üstbilgilerini | Microsoft Docs
description: X EC Debug hata ayıklama önbellek istek üstbilgisi istenen varlık için uygulanan önbellek İlkesi hakkında ek bilgi sağlar. Bu üstbilgileri Verizon için özeldir.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2018
ms.author: v-deasim
ms.openlocfilehash: 3a99e322d81748c54585e7dd0eb06959bfeb9569
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="x-ec-debug-http-headers-for-azure-cdn-rules-engine"></a>Azure CDN kurallar altyapısı için X EC Debug HTTP üstbilgileri
Hata ayıklama önbellek istek üstbilgisi `X-EC-Debug`, istenen varlık uygulanan önbellek İlkesi hakkında ek bilgi sağlar. Bu üstbilgileri özgü **verizon'dan Azure CDN Premium** ürünler.

## <a name="usage"></a>Kullanım
Bir kullanıcı POP sunuculardan gönderilen yanıtı içeren `X-EC-Debug` yalnızca aşağıdaki koşullar karşılandığında üstbilgisi:

- [Önbellek yanıt üstbilgilerini hata ayıklama özelliği](cdn-rules-engine-reference-features.md#debug-cache-response-headers) belirtilen istek için kurallar altyapısı üzerinde etkin.
- Belirtilen istek yanıta dahil hata ayıklama önbellek yanıt üstbilgilerini kümesini tanımlar.

## <a name="requesting-debug-cache-information"></a>Hata ayıklama önbellek bilgilerini isteyen
Aşağıdaki yönergeleri belirtilen istek yanıta dahil hata ayıklama önbellek bilgileri tanımlamak için kullanın:

İstek üstbilgisi | Açıklama |
---------------|-------------|
X-EC-Debug: x ec önbelleği | [Önbellek durum kodu](#cache-status-code-information)
X-EC-Debug: x-ec-önbellek-uzaktan | [Önbellek durum kodu](#cache-status-code-information)
X-EC-Debug: x-ec-onay-alınabilir | [Önbelleğe alınabilir](#cacheable-response-header)
X-EC-Debug: x-ec-önbellek-anahtarı | [Önbellek anahtarı](#cache-key-response-header)
X-EC-Debug: x-ec-önbellek-durumu | [Önbellek durumu](#cache-state-response-header)

### <a name="syntax"></a>Sözdizimi

Hata ayıklama önbellek yanıt üstbilgileri aşağıdaki üstbilgi ve belirtilen yönergeleri istekte ekleyerek istenebilir:

`X-EC-Debug: Directive1,Directive2,DirectiveN`

### <a name="sample-x-ec-debug-header"></a>Örnek X-EC-Debug üstbilgisi

`X-EC-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state`

## <a name="cache-status-code-information"></a>Durum kodu bilgilerini önbelleğe al
X EC Debug yanıt üst bilgisi bir sunucu ve aşağıdaki yönergeleri aracılığıyla yanıt işlenme tanımlayabilirsiniz:

Üst bilgi | Açıklama
-------|------------
X-EC-Debug: x ec önbelleği | İçerik CDN yönlendirilir olduğunda bu başlığı bildirilir. İsteği yerine getiren POP sunucu tanımlar.
X-EC-Debug: x-ec-önbellek-uzaktan | Bir kaynak kalkan sunucu veya bir ADN ağ geçidi sunucusu üzerinde istenen içeriği yalnızca önbelleğe ilişkili olduğunda bu başlığı bildirilir.

### <a name="response-header-format"></a>Yanıt üstbilgi biçimi

X EC Debug üstbilgi önbellek durum kodu bilgilerini aşağıdaki biçimde raporları:

- `X-EC-Debug: x-ec-cache: <StatusCode from Platform (POP/ID)>`

- `X-EC-Debug: x-ec-cache-remote: <StatusCode from Platform (POP/ID)>`

Yukarıdaki yanıt üstbilgisi sözdiziminde kullanılan terimler şu şekilde tanımlanır:
- StatusCode: İstenen içerik önbelleği durum kodu ile temsil edilen CDN tarafından nasıl işlendiğini gösterir.
    
    Belirteç tabanlı kimlik doğrulaması nedeniyle yetkisiz bir istek reddedildiğinde TCP_DENIED durum kodu hiçbiri yerine bildirilebilir. Ancak, NONE durum kodu önbellek durum raporları ya da ham günlük verileri görüntülerken kullanılacak devam eder.

- Platform: içeriği istendi platform gösterir. Aşağıdaki kodları Bu alan için geçerlidir:

    Kod  | Platform
    ------| --------
    ECAcc | HTTP büyük
    ECS   | HTTP küçük
    ECD   | Uygulama teslim ağı (ADN)

- POP: Gösterir [POP](cdn-pop-abbreviations.md) istek işlenmiş. 

### <a name="sample-response-headers"></a>Örnek yanıt üstbilgileri

Aşağıdaki örnek üstbilgileri bir istek için önbellek durum kodu bilgileri sağlayın:

- `X-EC-Debug: x-ec-cache: TCP_HIT from ECD (lga/0FE8)`

- `X-EC-Debug: x-ec-cache-remote: TCP_HIT from ECD (dca/EF00)`

## <a name="cacheable-response-header"></a>Önbelleğe alınabilir yanıt üstbilgisi
`X-EC-Debug: x-ec-check-cacheable` Yanıt üstbilgisi istenen içerik önbelleğe alınıp alınmadığını olup olmadığını gösterir.

Bu yanıt üstbilgisi önbelleğe alma gerçekleşen olup olmadığını göstermez. Bunun yerine, istek önbelleğe alma işlemi için uygun olup olmadığını gösterir.

### <a name="response-header-format"></a>Yanıt üstbilgi biçimi

`X-EC-Debug` Bir isteği önbelleğe alınıp alınmadığını olup olmadığını raporlama yanıt üstbilgisi olan şu biçimde:

`X-EC-Debug: x-ec-check-cacheable: <cacheable status>`

Yukarıdaki yanıt üstbilgisi sözdiziminde kullanılan terim şu şekilde tanımlanır:

Değer  | Açıklama
-------| --------
EVET    | İstenen içerik önbelleğe alma işlemi için uygun olmadığını gösterir.
HAYIR     | İstenen içerik önbelleğe alma işlemi için uygun olmadığını gösterir. Bu durum aşağıdaki nedenlerden biri olabilir: <br /> -Müşteri özgü yapılandırma: hesabınıza belirli bir yapılandırma bir varlık olarak önbelleğe alınan pop sunucuları engelleyebilirsiniz. Örneğin, kurallar altyapısı, bir varlık istekleri uygun için atlama önbellek özelliği etkinleştirerek önbelleğe alınması engelleyebilir.<br /> -Cache yanıt üstbilgilerini: İstenen varlığın Cache-Control ve Expires üstbilgileri önbelleğe alınan POP sunucuları engelleyebilir.
BİLİNMEYEN | Sunucuları istenen varlık alınabilir gerektirmediğine açamıyoruz gösterir. Bu durum genellikle istek belirteç tabanlı kimlik doğrulaması nedeniyle reddedilmesi durumunda oluşur.

### <a name="sample-response-header"></a>Örnek yanıt üstbilgisi

Aşağıdaki örnek yanıt üst bilgisi, istenen içerik önbelleğe alınıp alınmadığını olup olmadığını gösterir:

`X-EC-Debug: x-ec-check-cacheable: YES`

## <a name="cache-key-response-header"></a>Önbellek anahtarı yanıt üstbilgisi
`X-EC-Debug: x-ec-cache-key` Yanıt üstbilgisi istenen içerikle ilişkili fiziksel önbellek anahtarını belirtir. Önbelleğe alma amacıyla bir varlığı tanımlayan bir yolu, bir fiziksel önbellek anahtarını oluşur. Diğer bir deyişle, sunucuları önbelleğe alınmış bir yolunu göre bir varlık sürümü, önbellek anahtarını tarafından tanımlandığı şekilde kontrol eder.

Bu fiziksel önbellek anahtarını çift eğik çizgiyle başlatır (/ /) (HTTP veya HTTPS) içerik istemek için kullanılan protokol tarafından izlenen. Bu protokol içerik erişim noktasıyla başlar istenen varlık için göreli yol tarafından izlenir (örneğin, _/000001/_).

Varsayılan olarak, HTTP platformları kullanmak üzere yapılandırılmış *standart önbellek*, yani sorgu dizelerini önbelleğe alma mekanizması tarafından göz ardı edilir. Bu tür bir yapılandırma önbellek anahtarını sorgu dizesi verileri dahil etmesini engeller.

Bir sorgu dizesi önbellek anahtarında kaydedildiyse, bu karma eşdeğerine dönüştürülür ve istenen varlık adı ve dosya uzantısını arasında eklenen (örneğin, varlık&lt;karma değer&gt;.html).

### <a name="response-header-format"></a>Yanıt üstbilgi biçimi

`X-EC-Debug` Yanıt üstbilgisi raporları fiziksel önbellek anahtarı bilgileri şu biçimde:

`X-EC-Debug: x-ec-cache-key: CacheKey`

### <a name="sample-response-header"></a>Örnek yanıt üstbilgisi

Aşağıdaki örnek yanıt üstbilgisi fiziksel önbellek anahtarını istenen içerik için gösterir:

`X-EC-Debug: x-ec-cache-key: //http/800001/origin/images/foo.jpg`

## <a name="cache-state-response-header"></a>Önbellek durumu yanıt üstbilgisi
`X-EC-Debug: x-ec-cache-state` Yanıt üstbilgisi işlemi istendi aynı anda istenen içerik önbelleği durumunu gösterir.

### <a name="response-header-format"></a>Yanıt üstbilgi biçimi

`X-EC-Debug` Yanıt üstbilgisi raporları önbellek durumu bilgilerini şu biçimde:

`X-EC-Debug: x-ec-cache-state: max-age=MASeconds (MATimePeriod); cache-ts=UnixTime (ddd, dd MMM yyyy HH:mm:ss GMT); cache-age=CASeconds (CATimePeriod); remaining-ttl=RTSeconds (RTTimePeriod); expires-delta=ExpiresSeconds`

Yukarıdaki yanıt üstbilgisi sözdiziminde kullanılan terimler şu şekilde tanımlanır:

- MASeconds: İstenen içeriğin Cache-Control üstbilgileri tarafından tanımlandığı şekilde, max-age (saniye cinsinden) gösterir.

- MATimePeriod: Maksimum yaş değeri (diğer bir deyişle, MASeconds) daha büyük bir birimi yaklaşık denktir dönüştürür (örneğin, gün). 

- UnixTime: (paketini UNIX zamanında istenen içerik önbelleği zaman damgasını gösterir POSIX zaman ya da UNIX dönem). Önbellek zaman damgası, bir varlığın TTL hesaplanır başlangıç tarihi/saati belirtir. 

    Kaynak sunucu, sunucu veya sunucu Age yanıtı üstbilgisi döndürmezse, önbelleğe alma bir üçüncü taraf HTTP kullanmaz, önbellek zaman damgası her zaman zaman varlık alınan yeniden doğrulanır veya tarih/saat olacaktır. Aksi takdirde, POP sunucuları yaş alanı varlığın TTL şu şekilde hesaplamak için kullanır: alma/RevalidateDateTime - yaş.

- ggg, gg aaa yyyy ss: dd: GMT: İstenen içerik önbelleği zaman damgasını gösterir. Daha fazla bilgi için lütfen yukarıdaki UnixTime Terime bakın.

- CASeconds: önbellek zaman damgası bu yana geçen saniye sayısını gösterir.

- RTSeconds: kendisi için önbelleğe alınmış içeriği yeni olarak kabul edilecek kalan saniye sayısını gösterir. Bu değer aşağıdaki gibi hesaplanır: RTSeconds max-age - = önbelleğe yaş.

- RTTimePeriod: kalan TTL değeri (diğer bir deyişle, RTSeconds) daha büyük bir birimi yaklaşık denktir dönüştürür (örneğin, gün).

- ExpiresSeconds: Belirtilen tarih/saat önce kalan saniye sayısını gösterir. `Expires` yanıtı üstbilgisi. Varsa `Expires` yanıt üstbilgisi yanıtta dahil ve ardından bu terim değeri *hiçbiri*.

### <a name="sample-response-header"></a>Örnek yanıt üstbilgisi

Aşağıdaki örnek yanıt üstbilgisi istendi aynı anda istenen içerik önbelleği durumunu gösterir:

```X-EC-Debug: x-ec-cache-state: max-age=604800 (7d); cache-ts=1341802519 (Mon, 09 Jul 2012 02:55:19 GMT); cache-age=0 (0s); remaining-ttl=604800 (7d); expires-delta=none```

