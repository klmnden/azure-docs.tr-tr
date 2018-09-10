---
title: Azure işlevlerini test etme | Microsoft Docs
description: Postman, cURL ve Node.js kullanarak Azure işlevlerinizi test.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimari, test etme
ms.assetid: c00f3082-30d2-46b3-96ea-34faf2f15f77
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/02/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1aa1c3a3c0dd4985662b5ceb4acd7250cf2d0186
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093235"
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a>Kodunuzu Azure işlevleri'nde test stratejileri

Bu konu, aşağıdaki genel yaklaşımları kullanarak dahil işlevlerini test etmek için çeşitli yollar gösterir:

+ CURL ve Postman bile Tetikleyicileri web tabanlı bir web tarayıcısı gibi HTTP tabanlı araçlar
+ Azure depolama tabanlı Tetikleyicileri test etmek için Azure Depolama Gezgini,
+ Azure işlevleri portalındaki test sekmesi
+ Zamanlayıcı ile tetiklenen işlevi
+ Uygulama veya framework test etme

Bu test yöntemleri aracılığıyla bir sorgu dizesi parametresi veya istek gövdesinde giriş kabul eden bir HTTP tetikleyici işlevi kullanın. Bu işlev ilk bölümde Azure portalını kullanarak oluşturduğunuz.

## <a name="create-a-simple-function-for-testing-using-the-azure-portal"></a>Azure portalını kullanarak test etmek için basit bir işlev oluşturma
Bu öğreticinin çoğu için bir işlev oluşturduğunuzda kullanılabilir HttpTrigger JavaScript işlev şablonu biraz değiştirilmiş bir sürümünü kullanırız. Bir işlev oluşturma yardıma ihtiyacınız varsa, bilgileri gözden geçirdikten [öğretici](functions-create-first-azure-function.md). Seçin **HttpTrigger - JavaScript** test işlevinde oluştururken şablon [Azure portal].

Varsayılan işlevi şablonu temel geri istek gövdesi veya sorgu dizesi parametresi, adından yankılayan bir "Merhaba Dünya" işlevi olarak `name=<your name>`.  Ayrıca istek gövdesindeki JSON içeriği olarak adı ve adresi sağlamanıza izin verecek kod güncelleştireceğiz. Ardından işlev istemci kullanılabilir olduğunda bu geri görüntülemektedir.   

İşlevi test etmek için kullanacağımız aşağıdaki kodla güncelleştirin:

```javascript
module.exports = function (context, req) {
    context.log("HTTP trigger function processed a request. RequestUri=%s", req.originalUrl);
    context.log("Request Headers = " + JSON.stringify(req.headers));
    var res;

    if (req.query.name || (req.body && req.body.name)) {
        if (typeof req.query.name != "undefined") {
            context.log("Name was provided as a query string param...");
            res = ProcessNewUserInformation(context, req.query.name);
        }
        else {
            context.log("Processing user info from request body...");
            res = ProcessNewUserInformation(context, req.body.name, req.body.address);
        }
    }
    else {
        res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done(null, res);
};
function ProcessNewUserInformation(context, name, address) {
    context.log("Processing user information...");
    context.log("name = " + name);
    var echoString = "Hello " + name;
    var res;

    if (typeof address != "undefined") {
        echoString += "\n" + "The address you provided is " + address;
        context.log("address = " + address);
    }
    res = {
        // status: 200, /* Defaults to 200 */
        body: echoString
    };
    return res;
}
```

## <a name="test-a-function-with-tools"></a>Test araçları ile bir işlev
Azure portal dışında işlevlerinizi test etmek için tetiklemek için kullanabileceğiniz çeşitli araçları vardır. Bu test araçları (kullanıcı Arabirimi tabanlı hem de komut satırı), Azure depolama erişimi araçları ve basit bir web tarayıcısı HTTP içerir.

### <a name="test-with-a-browser"></a>Bir tarayıcı ile test etme
Web tarayıcısı üzerinden HTTP tetikleyicisi işlevlerini basit bir yoludur. Bir gövde yükü gerektirmeyen GET istekleri için bir tarayıcı kullanabilir ve kullanımı yalnızca sorgu parametreleri dize.

Daha önce tanımladığımız işlevi test etmek için kopyalama **işlev URL'sini** portalından. Bunu, aşağıdaki biçime sahiptir:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Append `name` sorgu dizesi parametresi. İçin gerçek bir ad kullanın `<Enter a name here>` yer tutucu.

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>&name=<Enter a name here>

URL'yi tarayıcınıza yapıştırın ve aşağıdakine benzer bir yanıt almalısınız.

![Ekran görüntüsü, Chrome tarayıcı sekmesinde test yanıt](./media/functions-test-a-function/browser-test.png)

Döndürülen dize XML'de sarmalar Chrome tarayıcı örnektir. Diğer tarayıcılarda yalnızca dize değeri görüntüler.

Portalda **günlükleri** penceresinde çıktısı aşağıdakine benzer bir günlüğe kaydedilir işlev yürütülürken:

    2016-03-23T07:34:59  Welcome, you are now connected to log-streaming service.
    2016-03-23T07:35:09.195 Function started (Id=61a8c5a9-5e44-4da0-909d-91d293f20445)
    2016-03-23T07:35:10.338 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==&name=Glenn from a browser
    2016-03-23T07:35:10.338 Request Headers = {"cache-control":"max-age=0","connection":"Keep-Alive","accept":"text/html","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T07:35:10.338 Name was provided as a query string param.
    2016-03-23T07:35:10.338 Processing User Information...
    2016-03-23T07:35:10.369 Function completed (Success, Id=61a8c5a9-5e44-4da0-909d-91d293f20445)

### <a name="test-with-postman"></a>Postman ile test
Chrome tarayıcısı ile tümleşen Postman çoğu işlevlerinizi test etmek için önerilen araçtır. Bkz: Postman'ı yüklemek için [alma Postman](https://www.getpostman.com/). Postman, HTTP isteğinin çok daha fazla öznitelik üzerinde denetim sağlar.

> [!TIP]
> HTTP en rahat kullanabileceğiniz araç testini kullanın. Postman için bazı seçenekler şunlardır:  
>
> * [Fiddler](http://www.telerik.com/fiddler)  
> * [Paw](https://luckymarmot.com/paw)  
>
>

Postman içinde birlikte bir istek gövdesi işlevi test etmek için:

1. Gelen Postman'i başlatın **uygulamaları** Chrome tarayıcı penceresinin sol alt köşesindeki düğme.
2. Kopyalama, **işlev URL'sini**, Postman yapıştırın. Bu erişim kodu sorgu dizesi parametresi içerir.
3. HTTP yöntemine değiştirme **POST**.
4. Tıklayın **gövdesi** > **ham**, aşağıdakine benzer bir JSON istek gövdesi ekleyin:

    ```json
    {
        "name" : "Wes testing with Postman",
        "address" : "Seattle, WA 98101"
    }
    ```
5. Tıklayın **Gönder**.

Bu öğreticide basit echo işlevi örneği sınama aşağıdaki resimde gösterilmektedir.

![Ekran Postman'ı, kullanıcı arabirimi](./media/functions-test-a-function/postman-test.png)

Portalda **günlükleri** penceresinde çıktısı aşağıdakine benzer bir günlüğe kaydedilir işlev yürütülürken:

    2016-03-23T08:04:51  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:04:57.107 Function started (Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)
    2016-03-23T08:04:57.763 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==
    2016-03-23T08:04:57.763 Request Headers = {"cache-control":"no-cache","connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:04:57.763 Processing user info from request body...
    2016-03-23T08:04:57.763 Processing User Information...
    2016-03-23T08:04:57.763 name = Wes testing with Postman
    2016-03-23T08:04:57.763 address = Seattle, W.A. 98101
    2016-03-23T08:04:57.795 Function completed (Success, Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)

### <a name="test-with-curl-from-the-command-line"></a>CURL komut satırından test
Genellikle, yazılım test ettiğiniz, herhangi bir uygulamanızda hata ayıklamak amacıyla komut satırı daha aramak ise gerekli değildir. Bu, işlevlerini test etme ile farklı değildir. CURL Linux tabanlı sistemler üzerinde varsayılan olarak kullanılabilir olduğunu unutmayın. Windows üzerinde indirmeniz ve yüklemeniz [cURL aracını](https://curl.haxx.se/).

Daha önce tanımladığımız işlevi test etmek için kopyalama **işlev URL'sini** portalından. Bunu, aşağıdaki biçime sahiptir:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Bu URL, işlevinizi tetiklemek için kullanılır. Bir GET yapmak için komut satırında cURL komutu kullanarak bu test (`-G` veya `--get`) işlev isteği:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Bu belirli bir örnek veri geçirilen bir sorgu dizesi parametresi gerektirir (`-d`) cURL komutu içinde:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code> -d name=<Enter a name here>

Komutunu çalıştırın ve komut satırında işlevin şu çıktıyı görürsünüz:

![Komut istemi ekran görüntüsü çıkış](./media/functions-test-a-function/curl-test.png)

Portalda **günlükleri** penceresinde çıktısı aşağıdakine benzer bir günlüğe kaydedilir işlev yürütülürken:

    2016-04-05T21:55:09  Welcome, you are now connected to log-streaming service.
    2016-04-05T21:55:30.738 Function started (Id=ae6955da-29db-401a-b706-482fcd1b8f7a)
    2016-04-05T21:55:30.738 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/HttpTriggerNodeJS1?code=XXXXXXX&name=Azure Functions
    2016-04-05T21:55:30.738 Function completed (Success, Id=ae6955da-29db-401a-b706-482fcd1b8f7a)

### <a name="test-a-blob-trigger-by-using-storage-explorer"></a>Blob tetikleyicisi Depolama Gezgini'ni kullanarak test edin.
Blob tetikleyicisi işlevi kullanarak test edebilirsiniz [Azure Depolama Gezgini](http://storageexplorer.com/).

1. İçinde [Azure portal] işlev uygulamanız için bir C#, F # veya JavaScript blob tetikleme işlevi oluşturma. Blob kapsayıcınızın adını izlemek için yolunu ayarlayın. Örneğin:

        files
2. Tıklayın **+** düğmesini kullanmak istediğiniz depolama hesabı seçin veya oluşturun. Sonra **Oluştur**’a tıklayın.
3. Şu metinle birlikte bir metin dosyası oluşturun ve kaydedin:

        A text file for blob trigger function testing.
4. Çalıştırma [Azure Depolama Gezgini](http://storageexplorer.com/), izlenmekte olan depolama hesabındaki blob kapsayıcısına bağlanın.
5. Tıklayın **karşıya** metin dosyasını karşıya yüklemek için.

    ![Depolama Gezgini ekran görüntüsü](./media/functions-test-a-function/azure-storage-explorer-test.png)

Varsayılan blob tetikleyici işlev kodunu günlüklerinde blob işlenmesini raporları:

    2016-03-24T11:30:10  Welcome, you are now connected to log-streaming service.
    2016-03-24T11:30:34.472 Function started (Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)
    2016-03-24T11:30:34.472 C# Blob trigger function processed: A text file for blob trigger function testing.
    2016-03-24T11:30:34.472 Function completed (Success, Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)

## <a name="test-a-function-within-functions"></a>İşlevler içinde bir işlevi test etme
Zamanlayıcı ile tetiklenen işlevleri ve Azure işlevleri portalına HTTP test etmenize izin vermek için tasarlanmıştır. Test ettiğiniz diğer işlevleri tetiklemek için işlevleri de oluşturabilirsiniz.

### <a name="test-with-the-functions-portal-run-button"></a>İşlevleri portal Çalıştır düğmesini test etme
Portal sağlar bir **çalıştırma** yapmak için kullanabileceğiniz düğme bazı sınırlı test etme. Düğmesini kullanarak, bir istek gövdesi sağlayabilirsiniz, ancak sorgu dizesi parametreleri belirtin veya istek üst bilgilerini güncelleştirin.

Oluşturduğumuz önceki aşağıdakine benzer bir JSON dizesi ekleyerek HTTP tetikleyici işlevi test **istek gövdesi** alan. Ardından **çalıştırma** düğmesi.

```json
{
    "name" : "Wes testing Run button",
    "address" : "USA"
}
```

Portalda **günlükleri** penceresinde çıktısı aşağıdakine benzer bir günlüğe kaydedilir işlev yürütülürken:

    2016-03-23T08:03:12  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:03:17.357 Function started (Id=753a01b0-45a8-4125-a030-3ad543a89409)
    2016-03-23T08:03:18.697 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/wesmchttptriggernodejs1
    2016-03-23T08:03:18.697 Request Headers = {"connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:03:18.697 Processing user info from request body...
    2016-03-23T08:03:18.697 Processing User Information...
    2016-03-23T08:03:18.697 name = Wes testing Run button
    2016-03-23T08:03:18.697 address = USA
    2016-03-23T08:03:18.744 Function completed (Success, Id=753a01b0-45a8-4125-a030-3ad543a89409)


### <a name="test-with-a-timer-trigger"></a>Bir zamanlayıcı tetikleyicisi ile test
Bazı işlevler, daha önce bahsedilen araçlarıyla yeterince sınanamıyor. Örneğin, bir ileti içine bırakıldığında çalıştırılan bir kuyruğu tetikleme işlevi düşünün [Azure kuyruk depolama](../storage/queues/storage-dotnet-how-to-use-queues.md). Her zaman, kuyruğa bir ileti bırakmak için kod yazabilirsiniz ve bu örnek bir konsol projesinde bu makalenin sonraki bölümlerinde verilmiştir. Ancak, doğrudan işlevleri test kullanabileceğiniz başka bir yaklaşım yoktur.  

Bir sıra ile yapılandırılmış bir zamanlayıcı tetikleyicisi kullanabileceğiniz çıktı bağlaması. Zamanlayıcı tetikleyicisi kod, sonra sınama iletileri kuyruğa yazabilirsiniz. Bu bölümde, bir örneği açıklanmaktadır.

Azure işlevleri ile bağlamaları kullanma hakkında daha ayrıntılı bilgi için bkz: [Azure işlevleri Geliştirici Başvurusu](functions-reference.md).

#### <a name="create-a-queue-trigger-for-testing"></a>Test etmek için bir kuyruk tetikleyicisi oluşturma
Bu yaklaşım göstermek için biz öncelikle test adında bir kuyruk için istediğimiz bir kuyruğu tetikleme işlevi oluşturma `queue-newusers`. Bu işlev, yeni bir kullanıcı için kuyruk depolamaya bırakılan ad ve adres bilgilerini işler.

> [!NOTE]
> Farklı bir kuyruk adı kullanırsanız, emin kullandığınız ad uyan [adlandırma kuyrukları ve meta verileri](https://msdn.microsoft.com/library/dd179349.aspx) kuralları. Aksi takdirde bir hata alırsınız.
>
>

1. İçinde [Azure portal] işlev uygulamanıza tıklayın **yeni işlev** > **QueueTrigger - C#**.
2. Kuyruk işlevi tarafından izlenmesi için kuyruk adı girin:

        queue-newusers
3. Tıklayın **+** düğmesini kullanmak istediğiniz depolama hesabı seçin veya oluşturun. Sonra **Oluştur**’a tıklayın.
4. Varsayılan sıra işlev şablonu kodu için günlük girişlerini izleyebilmek bu portal tarayıcı penceresini açık bırakın.

#### <a name="create-a-timer-trigger-to-drop-a-message-in-the-queue"></a>Kuyrukta bir ileti bırakmak için bir zamanlayıcı tetikleyicisi oluşturma
1. Açık [Azure portal] yeni bir tarayıcı penceresinde ve işlev uygulamanıza gidin.
2. Tıklayın **yeni işlev** > **TimerTrigger - C#**. Ne sıklıkta Zamanlayıcı kod kuyruk işlevinizi test ayarlamak için bir cron ifadesi girin. Sonra **Oluştur**’a tıklayın. Testin her 30 saniyede çalışmasını istiyorsanız, aşağıdakileri kullanabilirsiniz [CRON ifadesi](https://wikipedia.org/wiki/Cron#CRON_expression):

        */30 * * * * *
3. Tıklayın **tümleştir** , yeni bir zamanlayıcı tetikleyicisi için sekmesinde.
4. Altında **çıkış**, tıklayın **+ yeni çıkış**. Ardından **kuyruk** ve **seçin**.
5. Kullandığınız adını Not **kuyruğa ileti nesnesi**. Bu zamanlayıcı işlev kodu kullanın.

        myQueue
6. Burada mesajın gönderilip gönderilmediği kuyruk adı girin:

        queue-newusers
7. Tıklayın **+** düğmesini kullandığınız daha önce kuyruğu tetikleyici ile depolama hesabı seçin. Daha sonra **Kaydet**'e tıklayın.
8. Tıklayın **geliştirme** , Zamanlayıcı tetikleyicisi için sekmesinde.
9. Daha önce gösterilen aynı kuyruk iletisi nesne adını kullandığınız sürece, C# Zamanlayıcı işlevi için aşağıdaki kodu kullanabilirsiniz. Daha sonra **Kaydet**'e tıklayın.

    ```cs
    using System;

    public static void Run(TimerInfo myTimer, out String myQueue, TraceWriter log)
    {
        String newUser =
        "{\"name\":\"User testing from C# timer function\",\"address\":\"XYZ\"}";

        log.Verbose($"C# Timer trigger function executed at: {DateTime.Now}");   
        log.Verbose($"{newUser}");   

        myQueue = newUser;
    }
    ```

Bu noktada, örnek cron ifadesi kullandıysanız C# Zamanlayıcı işlevinin her 30 saniyede yürütür. Her yürütme için Zamanlayıcı işlev günlükleri raporu:

    2016-03-24T10:27:02  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.004 Function started (Id=04061790-974f-4043-b851-48bd4ac424d1)
    2016-03-24T10:27:30.004 C# Timer trigger function executed at: 3/24/2016 10:27:30 AM
    2016-03-24T10:27:30.004 {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.004 Function completed (Success, Id=04061790-974f-4043-b851-48bd4ac424d1)

Kuyruk işlevi için tarayıcı penceresinde işlenmekte olan her bir ileti görebilirsiniz:

    2016-03-24T10:27:06  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)

## <a name="test-a-function-with-code"></a>Bir işlev kodu ile test
İşlevlerinizi test etmek için bir dış uygulama veya framework oluşturmanız gerekebilir.

### <a name="test-an-http-trigger-function-with-code-nodejs"></a>Bir HTTP tetikleyici işlevi kodu ile test: Node.js
İşlevinizi test etmek için bir HTTP isteği yürütmek için bir Node.js uygulaması'nı kullanabilirsiniz.
Ayarladığınızdan emin olun:

* `host` , İşlev uygulamasını barındırmak için istek seçenekleri.
* İşlev adınızın `path`.
* Erişim kodunuzu (`<your code>`) içinde `path`.

Kod örneği:

```javascript
var http = require("http");

var nameQueryString = "name=Wes%20Query%20String%20Test%20From%20Node.js";

var nameBodyJSON = {
    name : "Wes testing with Node.JS code",
    address : "Dallas, T.X. 75201"
};

var bodyString = JSON.stringify(nameBodyJSON);

var options = {
  host: "functions841def78.azurewebsites.net",
  //path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9&" + nameQueryString,
  path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9",
  method: "POST",
  headers : {
      "Content-Type":"application/json",
      "Content-Length": Buffer.byteLength(bodyString)
    }    
};

callback = function(response) {
  var str = ""
  response.on("data", function (chunk) {
    str += chunk;
  });

  response.on("end", function () {
    console.log(str);
  });
}

var req = http.request(options, callback);
console.log("*** Sending name and address in body ***");
console.log(bodyString);
req.end(bodyString);
```


Çıktı:

    C:\Users\Wesley\testing\Node.js>node testHttpTriggerExample.js
    *** Sending name and address in body ***
    {"name" : "Wes testing with Node.JS code","address" : "Dallas, T.X. 75201"}
    Hello Wes testing with Node.JS code
    The address you provided is Dallas, T.X. 75201

Portalda **günlükleri** penceresinde çıktısı aşağıdakine benzer bir günlüğe kaydedilir işlev yürütülürken:

    2016-03-23T08:08:55  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:08:59.736 Function started (Id=607b891c-08a1-427f-910c-af64ae4f7f9c)
    2016-03-23T08:09:01.153 HTTP trigger function processed a request. RequestUri=http://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1/?code=XXXXXXXXXX==
    2016-03-23T08:09:01.153 Request Headers = {"connection":"Keep-Alive","host":"functionsExample.azurewebsites.net"}
    2016-03-23T08:09:01.153 Name not provided as query string param. Checking body...
    2016-03-23T08:09:01.153 Request Body Type = object
    2016-03-23T08:09:01.153 Request Body = [object Object]
    2016-03-23T08:09:01.153 Processing User Information...
    2016-03-23T08:09:01.215 Function completed (Success, Id=607b891c-08a1-427f-910c-af64ae4f7f9c)


### <a name="test-a-queue-trigger-function-with-code-c"></a>Bir kuyruğu tetikleme işlevi kodu ile test: C# #
Kuyruk tetikleyicisi, kuyrukta bir ileti bırakmak kod kullanarak sınayabilirsiniz daha önce bahsedilen. Aşağıdaki kod örneği, C# kod içinde sunulan dayalı [Azure kuyruk depolama ile çalışmaya başlama](../storage/queues/storage-dotnet-how-to-use-queues.md) öğretici. Diğer diller için kod da bu bağlantıdan kullanılabilir.

Bu kodu bir konsol uygulamasında test etmek için şunları yapmalısınız:

* [App.config dosyasında depolama bağlantı dizenizi yapılandırma](../storage/queues/storage-dotnet-how-to-use-queues.md).
* Başarılı bir `name` ve `address` uygulamasına parametre olarak. Örneğin, `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`. (Bu kod adı ve adresi yeni bir kullanıcı komut satırı bağımsız değişkenleri çalışma zamanı sırasında kabul eder.)

Örnek C# kodu:

```cs
static void Main(string[] args)
{
    string name = null;
    string address = null;
    string queueName = "queue-newusers";
    string JSON = null;

    if (args.Length > 0)
    {
        name = args[0];
    }
    if (args.Length > 1)
    {
        address = args[1];
    }

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["StorageConnectionString"]);

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference(queueName);

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    if (name != null)
    {
        if (address != null)
            JSON = String.Format("{{\"name\":\"{0}\",\"address\":\"{1}\"}}", name, address);
        else
            JSON = String.Format("{{\"name\":\"{0}\"}}", name);
    }

    Console.WriteLine("Adding message to " + queueName + "...");
    Console.WriteLine(JSON);

    CloudQueueMessage message = new CloudQueueMessage(JSON);
    queue.AddMessage(message);
}
```

Kuyruk işlevi için tarayıcı penceresinde işlenmekte olan her bir ileti görebilirsiniz:

    2016-03-24T10:27:06  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"Wes testing queues","address":"in a console app"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)


<!-- URLs. -->

[Azure portal]: https://portal.azure.com
