---
title: "Azure Geçiş düğümü API'leri genel bakış | Microsoft Docs"
description: "Geçiş düğümü API genel bakış"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b7d6e822-7c32-4cb5-a4b8-df7d009bdc85
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: 696e3f77a283cc31d3c8f6007a839480ae8eb984
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="relay-hybrid-connections-node-api-overview"></a>Karma bağlantılar düğümü API genel bakış geçiş

## <a name="overview"></a>Genel Bakış

[ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) Azure geçişi karma bağlantılar için düğüm paketi üzerine kurulmuştur ve genişleten ['ws'](https://www.npmjs.com/package/ws) NPM paket. Bu paket, temel pakette, tüm dışarı yeniden dışa aktarır ve Azure geçiş hizmeti karma bağlantılar özelliği ile tümleştirmeyi etkinleştir yeni dışarı ekler. 

Var olan uygulamalar, `require('ws')` bu paketle birlikte kullanabilirsiniz `require('hyco-ws')` bunun yerine, ayrıca sağlayan, bir uygulama dinleme yerel olarak güvenlik duvarından"" ve karma bağlantılar aracılığıyla WebSocket bağlantılar için tüm aynı anda karma senaryoları.
  
## <a name="documentation"></a>Belgeler

API'leri [ana 'ws' paketinde belgelenen](https://github.com/websockets/ws/blob/master/doc/ws.md). Bu makalede, bu paket, taban çizgisinden nasıl farklı açıklanmaktadır. 

Temel paket ve bu 'hyco-ws' arasındaki temel farklılıklar olan aracılığıyla verilen yeni bir sunucu sınıfı ekler `require('hyco-ws').RelayedServer`ve birkaç yardımcı yöntemler.

### <a name="package-helper-methods"></a>Paket yardımcı yöntemler

Aşağıdaki gibi başvurabilir paketi verme sırasında birkaç yardımcı program yöntemleri vardır:

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

Yardımcı yöntemler bu paketi ile kullanmak için olan, ancak dinleyicileri veya Gönderenler oluşturmak web veya aygıt istemcileri etkinleştirmek için bir düğümü sunucu tarafından da kullanılabilir. Sunucu, kısa süreli belirteçler katıştırmak URI'ler geçirerek bu yöntemleri kullanır. Bu URI ayarı HTTP üstbilgilerini WebSocket el sıkışma için desteklemeyen ortak WebSocket yığınları ile de kullanılabilir. Yetkilendirme belirteçleri URI katıştırma öncelikle bu kitaplığı dış kullanım senaryoları için desteklenir. 

#### <a name="createrelaylistenuri"></a>createRelayListenUri

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

Verilen ad ve yol için geçerli bir Azure geçişi karma bağlantı dinleyici URI oluşturur. Bu URI WebSocketServer sınıfı geçiş sürümü ile kullanılabilir.

- `namespaceName`(gerekli) - etki alanı adını kullanmak için Azure geçiş ad alanı.
- `path`(gerekli) - bu ad alanında mevcut bir Azure geçişi karma bağlantıyı adı.
- `token`(isteğe bağlı) - daha önce verilen geçiş erişim belirteci, dinleyicisi URI'si katıştırılmış (aşağıdaki örneğe bakın).
- `id`(isteğe bağlı) - istekleri uçtan uca tanılama izlemeyi sağlayan bir izleme tanımlayıcısı.

`token` Değer isteğe bağlıdır ve yalnızca W3C WebSocket yığınına sahip olduğu gibi WebSocket el sıkışma birlikte HTTP üst bilgileri göndermesine izin mümkün olmayan olduğunda kullanılmalıdır.                  


#### <a name="createrelaysenduri"></a>createRelaySendUri
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

Verilen ad ve yol için geçerli bir Azure geçişi karma bağlantı gönderme URI oluşturur. Bu URI herhangi bir Web yuvası istemci ile kullanılabilir.

- `namespaceName`(gerekli) - etki alanı adını kullanmak için Azure geçiş ad alanı.
- `path`(gerekli) - bu ad alanında mevcut bir Azure geçişi karma bağlantıyı adı.
- `token`(isteğe bağlı) - daha önce verilen geçiş erişim belirteci, gönderme URI'si katıştırılmış (aşağıdaki örneğe bakın).
- `id`(isteğe bağlı) - istekleri uçtan uca tanılama izlemeyi sağlayan bir izleme tanımlayıcısı.

`token` Değer isteğe bağlıdır ve yalnızca W3C WebSocket yığınına sahip olduğu gibi WebSocket el sıkışma birlikte HTTP üst bilgileri göndermesine izin mümkün olmayan olduğunda kullanılmalıdır.                   


#### <a name="createrelaytoken"></a>createRelayToken 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Verilen hedef URI için Azure geçiş paylaşılan erişim imzası (SAS) belirteci, SAS kuralı ve süre sonu bağımsız değişken belirtilmezse belirtilen sayıda saniye veya geçerli anlık bir saati için geçerli olan SAS kural anahtarı oluşturur.

- `uri`(gerekli) - belirteç verilmesine olduğu URI. URI HTTP düzeni kullanmak için normalleştirilmiş ve sorgu dize bilgilerini kesilmiş olabilir.
- `ruleName`(gerekli) - SAS adı verilen URI tarafından temsil edilen varlık için ya da URI ana bilgisayar bölümü tarafından temsil edilen ad alanı için kural.
- `key`(gerekli) - SAS kural için geçerli anahtar. 
- `expirationSeconds`(isteğe bağlı) - oluşturulan belirteç sona kadar saniye sayısı. Belirtilmezse, varsayılan değer 1 saat (3600) olur.

Verilen belirteç için verilen süreyi belirtilen SAS kuralla ilişkili hakları confers.

#### <a name="appendrelaytoken"></a>appendRelayToken

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Bu yöntem işlevsel olarak eşdeğerdir `createRelayToken` daha önce belirtildiği yöntemi ancak doğru giriş URI'si eklenen belirteci döndürür.

### <a name="class-wsrelayedserver"></a>Class ws.RelayedServer

`hycows.RelayedServer` Sınıftır alternatif `ws.Server` yerel ağ, ancak Azure geçiş hizmeti dinleme temsilciler dinlemez sınıfı.

Çoğunlukla kullanarak varolan bir uygulama, yani uyumlu sözleşme iki sınıflardır `ws.Server` sınıfı kolayca değiştirilebilir geçişli sürüm kullanılacak. Temel farklılıklar oluşturucusu ve kullanılabilir seçenekler şunlardır.

#### <a name="constructor"></a>Oluşturucusu  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

`RelayedServer` Oluşturucu bağımsız değişkenleri farklı bir kümesini destekler `Server`bir tek başına dinleyicisi olmadığından veya varolan bir HTTP dinleyicisi Framework'e katıştırılmış yapılamıyor. Ayrıca daha az seçenek kullanılabilir olduğundan WebSocket yönetimini büyük ölçüde geçiş hizmeti temsilci beri.

Oluşturucu bağımsız değişkenleri:

- `server`(gerekli) - Dinlemenin yapılacağı karma bağlantı adı için tam URI, WebSocket.createRelayListenUri() Yardımcısı yöntemiyle genellikle oluşturulur.
- `token`(gerekli) - bu bağımsız değişken, daha önce verilen bir belirteç dizesi veya böyle bir belirteç dizesini elde etmek için çağrılan bir geri çağırma işlevini tutar. Geri arama seçeneğini tercih edilen, aynıdır belirteci yenileme sağlar.

#### <a name="events"></a>Olaylar

`RelayedServer`örnekleri gelen istekleri işleyen, bağlantıları kurmak ve hata koşullarını algılamak sağlayan üç olayları yayma. İçin abone olmalısınız `connect` iletileri işlemek için olay. 

##### <a name="headers"></a>headers

```JavaScript 
function(headers)
```

`headers` Yalnızca gelen bağlantının, istemciye göndermek için üstbilgileri değiştirilmesini etkinleştirme kabul edilmeden önce olayı oluşturulur. 

##### <a name="connection"></a>bağlantı

```JavaScript
function(socket)
```

Yeni bir WebSocket bağlantı kabul edildiğinde yayılan. Nesne türünde `ws.WebSocket`, temel paketi ile aynı.


##### <a name="error"></a>error

```JavaScript
function(error)
```

Temel alınan sunucusunda bir hata yayar, burada iletilir.  

#### <a name="helpers"></a>Yardımcıları

Geçişli sunucusu başlatılıyor ve hemen gelen bağlantılar için abone basitleştirmek için paket de örneklerde, aşağıdaki gibi kullanılır basit yardımcı bir işlev gösterir:

##### <a name="createrelayedlistener"></a>createRelayedListener

```JavaScript
var WebSocket = require('hyco-ws');

var wss = WebSocket.createRelayedServer(
    {
        server: WebSocket.createRelayListenUri(ns, path),
        token: function() { return WebSocket.createRelayToken('http://' + ns, keyrule, key); }
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(JSON.parse(event.data));
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
});
``` 

##### <a name="createrelayedserver"></a>createRelayedServer

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

Bu yöntem RelayedServer yeni bir örneğini oluşturmak için oluşturucuyu çağırır ve sağlanan geri çağırma 'bağlantı' olaya abone olur.
 
##### <a name="relayedconnect"></a>relayedConnect

Yalnızca yansıtma `createRelayedServer` işlevinde yardımcı `relayedConnect` istemci bağlantısını oluşturur ve elde edilen yuva 'açık' olaya abone olur.

```JavaScript
var uri = WebSocket.createRelaySendUri(ns, path);
WebSocket.relayedConnect(
    uri,
    WebSocket.createRelayToken(uri, keyrule, key),
    function (socket) {
        ...
    }
);
```

## <a name="next-steps"></a>Sonraki adımlar
Azure geçişi hakkında daha fazla bilgi için bu bağlantıları ziyaret edin:
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Kullanılabilir geçiş API'leri](relay-api-overview.md)
