### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
* `listener.js` adlı yeni bir JavaScript dosyası oluşturun.

### <a name="add-the-relay-npm-package"></a>Geçiş NPM paketini ekleme
* Proje klasörünüzdeki bir Düğüm komut isteminden `npm install hyco-ws` komutunu çalıştırın.

### <a name="write-some-code-to-receive-messages"></a>İleti almak için bazı kodlar yazma
1. Aşağıdaki `constant` öğesini `listener.js` dosyasının başına ekleyin.
   
    ```js
    const WebSocket = require('hyco-ws');
    ```
2. Karma Bağlantının bağlantı ayrıntıları için şu Geçiş `constants` öğesini `listener.js` dosyasına ekleyin. Köşeli ayraçlar içindeki yer tutucuları Karma Bağlantı oluşturulurken edinilen uygun değerlerle değiştirin.
   
   1. `const ns` - Geçiş ad alanı (FQDN kullanma - örn. `{namespace}.servicebus.windows.net`)
   2. `const path` - Karma Bağlantının adı
   3. `const keyrule` - SAS anahtarının adı
   4. `const key` - SAS anahtarının değeri
3. `listener.js` dosyasına aşağıdaki kodu ekleyin:
   
    ```js
    var wss = WebSocket.createRelayedServer(
    {
        server : WebSocket.createRelayListenUri(ns, path),
        token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(event.data);
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
    });
   
    console.log('listening');
   
    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```
    listener.js dosyanız şu şekilde görünmelidir:
   
    ```js
    const WebSocket = require('hyco-ws');
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    var wss = WebSocket.createRelayedServer(
        {
            server : WebSocket.createRelayListenUri(ns, path),
            token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
        }, 
        function (ws) {
            console.log('connection accepted');
            ws.onmessage = function (event) {
                console.log(event.data);
            };
            ws.on('close', function () {
                console.log('connection closed');
            });       
    });
   
    console.log('listening');
   
    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```

