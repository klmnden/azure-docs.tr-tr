---
title: Azure geçişi düğüm API'leri genel bakış | Microsoft Docs
description: Geçiş düğüm API'sine genel bakış
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: b7d6e822-7c32-4cb5-a4b8-df7d009bdc85
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: spelluru
ms.openlocfilehash: 794e797e504d6064c13ffe0a4ed131e668d86e97
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60421616"
---
# <a name="relay-hybrid-connections-node-api-overview"></a>Geçiş karma bağlantıları düğümünü API'sine genel bakış

## <a name="overview"></a>Genel Bakış

[ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) Azure geçiş karma bağlantıları için düğüm paketi üzerine kurulmuştur ve genişleten ['ws'](https://www.npmjs.com/package/ws) NPM paketi. Bu paket, temel paketin tüm aktarmaları yeniden dışarı aktarır ve Azure geçişi hizmeti karma bağlantılar özelliği ile tümleştirme sağlayan yeni dışarı aktarma ekler. 

Mevcut uygulamaları, `require('ws')` bu paketle birlikte kullanabileceğiniz `require('hyco-ws')` bunun yerine, ayrıca sağlayan, bir uygulama dinler WebSocket bağlantılarını "içinde"güvenlik duvarının ve karma bağlantılar aracılığıyla yerel olarak hepsi karma senaryoları aynı anda.
  
## <a name="documentation"></a>Belgeler

API'leri [ana 'ws' paketinde belgelenen](https://github.com/websockets/ws/blob/master/doc/ws.md). Bu makalede, bu paket, taban çizgisinden farkı açıklar. 

Temel paket ve bu 'hyco-ws' arasındaki temel farklılıklar olduğundan aracılığıyla dışarı aktarılan yeni bir sunucu sınıfı ekler `require('hyco-ws').RelayedServer`ve birkaç yardımcı yöntemler.

### <a name="package-helper-methods"></a>Paket yardımcı yöntemler

Şu şekilde başvurabileceğiniz paketi verme sırasında bazı yardımcı program yöntemleri vardır:

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

Yardımcı yöntemler Bu pakette kullanılmak içindir, ancak dinleyicileri veya Gönderenler oluşturmak web veya cihaz istemcileri etkinleştirmek için bir düğümü sunucu tarafından da kullanılabilir. Sunucu ekleme belirteçleri kısa süreli bir URI'leri geçirerek bu yöntemleri kullanır. Bu bir URI'leri ayarı HTTP üstbilgileri için WebSocket el sıkışması desteklemez ortak WebSocket yığınları ile de kullanılabilir. URI yetkilendirme belirteçleri ekleme, öncelikle bu kitaplığı dış kullanım senaryoları için desteklenir. 

#### <a name="createrelaylistenuri"></a>createRelayListenUri

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

Verilen ad ve yol için geçerli bir Azure geçişi karma bağlantı dinleyicisi URI oluşturur. Bu URI, WebSocketServer sınıfı geçiş sürümü ile kullanılabilir.

- `namespaceName` (gerekli) - etki alanı adını kullanmak için Azure geçiş ad alanı.
- `path` (gerekli) - bu ad alanında mevcut bir Azure geçişi karma bağlantı adı.
- `token` (isteğe bağlı) - daha önce verilmiş bir geçiş erişim belirtecini dinleyicisi URI katıştırılmış (aşağıdaki örneğe bakın).
- `id` (isteğe bağlı) - isteklerin uçtan uca Tanılama izleme sağlayan bir izleme tanımlayıcısı.

`token` Değer isteğe bağlıdır ve yalnızca W3C WebSocket yığın ile olduğu gibi WebSocket el sıkışması yanı sıra HTTP üst bilgileri gönderme mümkün olmadığında kullanılmalıdır.                  


#### <a name="createrelaysenduri"></a>createRelaySendUri
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

Verilen ad ve yol için geçerli bir Azure geçişi karma bağlantı gönderme URI oluşturur. Bu URI, herhangi bir WebSocket istemcisi ile kullanılabilir.

- `namespaceName` (gerekli) - etki alanı adını kullanmak için Azure geçiş ad alanı.
- `path` (gerekli) - bu ad alanında mevcut bir Azure geçişi karma bağlantı adı.
- `token` (isteğe bağlı) - daha önce verilmiş bir geçiş erişim belirtecini gönderme URI katıştırılmış (aşağıdaki örneğe bakın).
- `id` (isteğe bağlı) - isteklerin uçtan uca Tanılama izleme sağlayan bir izleme tanımlayıcısı.

`token` Değer isteğe bağlıdır ve yalnızca W3C WebSocket yığın ile olduğu gibi WebSocket el sıkışması yanı sıra HTTP üst bilgileri gönderme mümkün olmadığında kullanılmalıdır.                   


#### <a name="createrelaytoken"></a>createRelayToken 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Bir belirli hedef URI için Azure geçişi paylaşılan erişim imzası (SAS) belirteci, SAS kuralı ve süre sonu bağımsız değişken yoksayılırsa geçerli anlık bir saat veya belirtilen sayıda saniye için geçerli olan SAS kural anahtarı oluşturur.

- `uri` (gerekli) - belirteç kesilecek olduğu URI. URI HTTP düzeni kullanılacak normalleştirilir ve sorgu dize bilgilerini çıkartılır.
- `ruleName` (gerekli) - SAS adı verilen URI tarafından temsil edilen varlık için ya da URI ana bilgisayar bölümü tarafından temsil edilen ad alanı için kural.
- `key` (gerekli) - SAS kural için geçerli anahtar. 
- `expirationSeconds` (isteğe bağlı) - oluşturulan belirtecin süresi dolarsa kadar saniye sayısı. Belirtilmezse 1 saat (3600) varsayılandır.

Belirli bir süre için belirtilen SAS kuralla ilişkili hakları verilen belirteç confers.

#### <a name="appendrelaytoken"></a>appendRelayToken

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Bu yöntem işlevsel olarak eşdeğerdir `createRelayToken` yöntemi daha önce ancak doğru giriş URI'si eklenen belirteç döndürür.

### <a name="class-wsrelayedserver"></a>Sınıf ws. RelayedServer

`hycows.RelayedServer` Sınıftır alternatif `ws.Server` yerel ağ, ancak Azure geçişi hizmetine dinleme temsilciler dinlemez sınıfı.

Çoğunlukla kullanarak mevcut bir uygulamaya güncelleştirmeyeceği uyumlu sözleşme iki sınıflardır `ws.Server` sınıfı kolayca değiştirilebilir geçişli sürümünü kullanmak için. Oluşturucu ve kullanılabilir seçenekler temel farklılıklar şunlardır.

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

`RelayedServer` Oluşturucu bağımsız değişkenleri farklı bir kümesini destekler `Server`, tek başına bir dinleyici olduğundan veya mevcut bir HTTP dinleyicisi çerçeve gömülü olması kullanabilirsiniz. Kullanılabilir ayrıca daha az seçenek WebSocket yönetimini büyük ölçüde alan geçiş hizmetine temsilci olduğundan.

Oluşturucu bağımsız değişkenleri:

- `server` (gerekli) - dinlemek bir karma bağlantı adı için tam uygun URI, WebSocket.createRelayListenUri() yardımcı yöntemi genellikle oluşturulur.
- `token` (gerekli) - bu bağımsız değişken, daha önce verilmiş bir belirteç dizesi ya da böyle bir belirteç dizesini almak için çağrılan bir geri çağırma işlevini içerir. Belirteci yenileme olanak tanıdığından geri çağırma seçeneği tercih edilen, içindir.

#### <a name="events"></a>Olaylar

`RelayedServer` örnekleri, gelen istekleri işleyen, bağlantı ve hata koşulları algılamak sağlayan üç olayları Yayımla. Abone olmalısınız `connect` olay iletileri işlemek için. 

##### <a name="headers"></a>Üst bilgileri

```JavaScript 
function(headers)
```

`headers` Olayı, yalnızca bir gelen bağlantı, üstbilgileri istemciye gönderilecek değiştirilmesini etkinleştirme kabul edilmeden önce oluşturulur. 

##### <a name="connection"></a>bağlantı

```JavaScript
function(socket)
```

Yeni bir WebSocket bağlantısı kabul edildiğinde yayılır. Nesne türünde `ws.WebSocket`, temel paketi ile aynı.


##### <a name="error"></a>error

```JavaScript
function(error)
```

Temel alınan sunucusunda bir hata gösterir, burada iletilir.  

#### <a name="helpers"></a>Yardımcıları

Geçişli server'ı başlatıp gelen bağlantılar için hemen abone basitleştirmek için paket ayrıca örneklerde, şu şekilde kullanılan bir basit yardımcı işlevini kullanıma sunar:

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

Bu yöntem RelayedServer yeni bir örneğini oluşturmak için bir oluşturucuyu çağırır ve ardından sağlanan geri çağırma 'bağlantı' olaya abone olur.
 
##### <a name="relayedconnect"></a>relayedConnect

Yalnızca yansıtma `createRelayedServer` işlevde Yardımcısı `relayedConnect` istemci bağlantısı oluşturur ve elde edilen yuvadaki 'açık' olaya abone olur.

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
Azure geçişi hakkında daha fazla bilgi edinmek için şu bağlantıları ziyaret edin:
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Kullanılabilir geçiş API'leri](relay-api-overview.md)
