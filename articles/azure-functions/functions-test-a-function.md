---
title: "Azure işlevlerini test etme | Microsoft Docs"
description: "Postman, cURL ve Node.js kullanarak Azure işlevlerinizi test edin."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme, Web kancalarını, dinamik işlem, sunucusuz mimari, test etme"
ms.assetid: c00f3082-30d2-46b3-96ea-34faf2f15f77
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/02/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 41796a8cdde0756e5157ba276463a56b07679d04
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a>Azure işlevleri, kodunuzu test etmek için stratejileri

Bu konuda aşağıdaki genel yaklaşım kullanarak dahil işlevlerini test etmek için çeşitli yollar gösterilir:

+ CURL, Postman ve web tabanlı tetikleyiciler için bile bir web tarayıcısı gibi HTTP tabanlı araçları
+ Azure depolama tabanlı tetikleyiciler sınamak için Azure Storage Gezgini,
+ Azure işlevleri portalındaki test sekmesi
+ Zamanlayıcı tarafından tetiklenen işlevi
+ Uygulama veya framework test etme

Bu test yöntemleri bir sorgu dizesi parametresi veya istek gövdesi girişini kabul eden bir HTTP tetikleyicisi işlevini kullanın. Bu işlev ilk bölümde oluşturun.

## <a name="create-a-function-for-testing"></a>Test etmek için bir işlev oluşturun
Bu öğretici çoğu için bir işlev oluşturduğunuzda kullanılabilir olan HttpTrigger JavaScript işlevi şablon biraz değiştirilmiş bir sürümünü kullanın. Bir işlev oluşturma yardıma gereksinim duyarsanız, bu gözden [Öğreticisi](functions-create-first-azure-function.md). Seçin **HttpTrigger - JavaScript** test işlevinde oluştururken şablonu [Azure portal].

Varsayılan işlev temelde geri istek gövdesi veya sorgu dizesi parametresi, adından görüntülemektedir "hello world" işlevi şablonudur `name=<your name>`.  İstek gövdesinde JSON içeriği olarak ad ve bir adres sağlamak üzere izin için kodu güncelleştiriyoruz. Ardından bir istemciye kullanılabilir olduğunda bu arka işlevi görüntülemektedir.   

İşlevi test etmek için kullanacağız aşağıdaki kod ile güncelleştirin:

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

## <a name="test-a-function-with-tools"></a>Test araçları ile işlevi
Azure portal dışında işlevlerinizi test etmek için tetiklemek için kullanabileceğiniz çeşitli araçlar vardır. Bu araçlar (hem kullanıcı Arabirimi tabanlı ve komut satırı), Azure depolama erişim araçları ve basit bir web tarayıcısı test HTTP içerir.

### <a name="test-with-a-browser"></a>Bir tarayıcı ile test
Web tarayıcısı, HTTP üzerinden tetikleyici işlevlerine basit bir yoludur. Gövde yükü gerektirmeyen GET istekleri için bir tarayıcı kullanabilir ve kullanım yalnızca sorgu parametreleri dize.

Daha önce tanımladığımız işlevi sınamak için kopyalama **işlevi Url** portalından. Aşağıdaki biçime sahiptir:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Append `name` sorgu dizesi parametresi. İçin gerçek bir ad kullanmak `<Enter a name here>` yer tutucu.

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>&name=<Enter a name here>

URL'sini tarayıcınıza yapıştırın ve aşağıdakine benzer bir yanıt almalısınız.

![Test yanıt ekran görüntüsü, Chrome tarayıcı sekmesi](./media/functions-test-a-function/browser-test.png)

Bu, döndürülen dize XML'de sarmalar Chrome tarayıcısı örneğidir. Diğer tarayıcılarda yalnızca dize değeri görüntüler.

Portalda **günlükleri** penceresinde, çıktı aşağıdakine benzer bir günlüğe kaydedilir işlev yürütülürken:

    2016-03-23T07:34:59  Welcome, you are now connected to log-streaming service.
    2016-03-23T07:35:09.195 Function started (Id=61a8c5a9-5e44-4da0-909d-91d293f20445)
    2016-03-23T07:35:10.338 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==&name=Glenn from a browser
    2016-03-23T07:35:10.338 Request Headers = {"cache-control":"max-age=0","connection":"Keep-Alive","accept":"text/html","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T07:35:10.338 Name was provided as a query string param.
    2016-03-23T07:35:10.338 Processing User Information...
    2016-03-23T07:35:10.369 Function completed (Success, Id=61a8c5a9-5e44-4da0-909d-91d293f20445)

### <a name="test-with-postman"></a>Postman ile test
Chrome tarayıcı ile tümleşir Postman çoğu işlevlerinizi test etmek için önerilen araçtır. Postman yüklemek için bkz: [almak Postman](https://www.getpostman.com/). Postman pek çok daha fazla öznitelik bir HTTP isteği üzerinde denetim sağlar.

> [!TIP]
> HTTP en uygun olanını araç testini kullanın. Postman için bazı seçenekleri şunlardır:  
>
> * [Fiddler](http://www.telerik.com/fiddler)  
> * [Pençe](https://luckymarmot.com/paw)  
>
>

Bir istek gövdesi işlev içinde Postman test etmek için:

1. Postman gelen Başlat **uygulamaları** Chrome tarayıcı penceresinin sol üst köşesindeki düğmesi.
2. Kopyalama, **işlevi Url**, Postman yapıştırın. Erişim kodu sorgu dizesi parametresi içerir.
3. HTTP yöntemini değiştirme **POST**.
4. Tıklatın **gövde** > **ham**, aşağıdakine benzer bir JSON istek gövdesini ekleyin:

    ```json
    {
        "name" : "Wes testing with Postman",
        "address" : "Seattle, WA 98101"
    }
    ```
5. Tıklatın **Gönder**.

Aşağıdaki resimde, bu öğreticide basit Yankı işlevi örneği sınama gösterir.

![Ekran görüntüsü, Postman kullanıcı arabirimi](./media/functions-test-a-function/postman-test.png)

Portalda **günlükleri** penceresinde, çıktı aşağıdakine benzer bir günlüğe kaydedilir işlev yürütülürken:

    2016-03-23T08:04:51  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:04:57.107 Function started (Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)
    2016-03-23T08:04:57.763 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==
    2016-03-23T08:04:57.763 Request Headers = {"cache-control":"no-cache","connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:04:57.763 Processing user info from request body...
    2016-03-23T08:04:57.763 Processing User Information...
    2016-03-23T08:04:57.763 name = Wes testing with Postman
    2016-03-23T08:04:57.763 address = Seattle, W.A. 98101
    2016-03-23T08:04:57.795 Function completed (Success, Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)

### <a name="test-with-curl-from-the-command-line"></a>Komut satırından cURL sınayın
Genellikle zaman yazılım test ettiğiniz, herhangi bir uygulamanızda hata ayıklama yardımcı olmak için komut satırını daha aramak ise gerekli değildir. Bu işlevler testi ile farklı değildir. CURL Linux tabanlı sistemlerinde varsayılan olarak kullanılabilir olduğunu unutmayın. Windows, önce indirmeniz gerekir ve yükleme [cURL aracı](https://curl.haxx.se/).

Daha önce tanımladığımız işlevi sınamak için kopyalama **işlevi URL** portalından. Aşağıdaki biçime sahiptir:

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Bu işlevinizi tetiklemek URL'dir. Bu bir GET yapmak için komut satırında cURL komutunu kullanarak test (`-G` veya `--get`) işlevi karşı isteği:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

Bu belirli örnek verileri olarak geçirilen bir sorgu dizesi parametresi gerektirir (`-d`) cURL komutta:

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code> -d name=<Enter a name here>

Komutunu çalıştırın ve komut satırında aşağıdaki çıkış işlevinin bakın:

![Komut istemi ekran çıktı](./media/functions-test-a-function/curl-test.png)

Portalda **günlükleri** penceresinde, çıktı aşağıdakine benzer bir günlüğe kaydedilir işlev yürütülürken:

    2016-04-05T21:55:09  Welcome, you are now connected to log-streaming service.
    2016-04-05T21:55:30.738 Function started (Id=ae6955da-29db-401a-b706-482fcd1b8f7a)
    2016-04-05T21:55:30.738 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/HttpTriggerNodeJS1?code=XXXXXXX&name=Azure Functions
    2016-04-05T21:55:30.738 Function completed (Success, Id=ae6955da-29db-401a-b706-482fcd1b8f7a)

### <a name="test-a-blob-trigger-by-using-storage-explorer"></a>Depolama Gezgini'ni kullanarak bir blob tetikleyici test
Bir blob Tetik işlevi kullanarak test edebilirsiniz [Azure Storage Gezgini](http://storageexplorer.com/).

1. İçinde [Azure portal] işlevi uygulamanız için bir C#, F # veya JavaScript blob tetikleyici işlev oluşturun. Blob kapsayıcı adı için izleme yolu ayarlayın. Örneğin:

        files
2. Tıklatın  **+**  düğmesini seçin veya kullanmak istediğiniz depolama hesabı oluşturun. Sonra **Oluştur**’a tıklayın.
3. Şu metinle birlikte bir metin dosyası oluşturun ve kaydedin:

        A text file for blob trigger function testing.
4. Çalıştırma [Azure Storage Gezgini](http://storageexplorer.com/), izlenmekte olan depolama hesabındaki blob kapsayıcısını bağlanın.
5. Tıklatın **karşıya** metin dosyasını karşıya yüklemek için.

    ![Depolama Gezgini ekran görüntüsü](./media/functions-test-a-function/azure-storage-explorer-test.png)

Varsayılan blob Tetik işlevi kodu günlüklerine blob işlenmesini raporları:

    2016-03-24T11:30:10  Welcome, you are now connected to log-streaming service.
    2016-03-24T11:30:34.472 Function started (Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)
    2016-03-24T11:30:34.472 C# Blob trigger function processed: A text file for blob trigger function testing.
    2016-03-24T11:30:34.472 Function completed (Success, Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)

## <a name="test-a-function-within-functions"></a>İşlevler içinde işlevi test etme
Azure işlevleri portal HTTP test olanak sağlamak için tasarlanmıştır ve tetiklenen Zamanlayıcı işlevleri. Test ettiğiniz diğer işlevleri tetiklemek için işlevleri de oluşturabilirsiniz.

### <a name="test-with-the-functions-portal-run-button"></a>İşlevler portal Çalıştır düğmesi ile test
Portal sağlayan bir **çalıştırmak** yapmak için kullanabileceğiniz düğmesi bazı sınırlı test etme. Düğmesini kullanarak bir istek gövdesi sağlayabilirsiniz, ancak sorgu dizesi parametreleri belirtin veya istek üstbilgileri güncelleştirin.

Oluşturduğumuz önceki aşağıdakine benzer bir JSON dizesinde ekleyerek HTTP tetikleyicisi işlevi test **istek gövdesinde** alan. Ardından **çalıştırmak** düğmesi.

```json
{
    "name" : "Wes testing Run button",
    "address" : "USA"
}
```

Portalda **günlükleri** penceresinde, çıktı aşağıdakine benzer bir günlüğe kaydedilir işlev yürütülürken:

    2016-03-23T08:03:12  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:03:17.357 Function started (Id=753a01b0-45a8-4125-a030-3ad543a89409)
    2016-03-23T08:03:18.697 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/wesmchttptriggernodejs1
    2016-03-23T08:03:18.697 Request Headers = {"connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:03:18.697 Processing user info from request body...
    2016-03-23T08:03:18.697 Processing User Information...
    2016-03-23T08:03:18.697 name = Wes testing Run button
    2016-03-23T08:03:18.697 address = USA
    2016-03-23T08:03:18.744 Function completed (Success, Id=753a01b0-45a8-4125-a030-3ad543a89409)


### <a name="test-with-a-timer-trigger"></a>Zamanlayıcı tetikleyicisi ile test
Bazı işlevler yeterli daha önce bahsedilen araçlarıyla sınanamıyor. Örneğin, bir ileti içine bırakıldığında çalıştırılan bir sıra tetikleyici işlevi göz önünde bulundurun [Azure kuyruk depolama](../storage/queues/storage-dotnet-how-to-use-queues.md). Her zaman, sıraya bir ileti bırakma üzere kod yazabilirsiniz ve bu örnek bir konsol projesinde bu makalenin sonraki bölümlerinde sağlanır. Ancak, işlevleri doğrudan test kullanabileceğiniz başka bir yaklaşım yoktur.  

Bir sıra ile yapılandırılmış bir süreölçer tetikleyici kullanabilirsiniz bağlama çıktı. Bu zamanlayıcı tetikleyicisi kodu, ardından sıraya sınama iletileri yazabilirsiniz. Bu bölüm bir örnek anlatılmaktadır.

Azure işlevleriyle bağlamaları kullanma hakkında daha ayrıntılı bilgi için bkz: [Azure işlevleri Geliştirici Başvurusu](functions-reference.md).

#### <a name="create-a-queue-trigger-for-testing"></a>Test etmek için bir sıra Tetikleyici oluşturma
Bu yaklaşım tanıtmak için önce test adlı bir kuyruk için istiyoruz bir sıra Tetik işlevi oluşturuyoruz `queue-newusers`. Bu işlev, yeni bir kullanıcı için kuyruk depolama alanına bırakılan ad ve adres bilgilerini işler.

> [!NOTE]
> Farklı sıra adı kullanırsanız, emin olun, kullandığınız adı uyan [adlandırma kuyrukları ve meta verileri](https://msdn.microsoft.com/library/dd179349.aspx) kuralları. Aksi takdirde bir hata alıyorsunuz.
>
>

1. İçinde [Azure portal] işlev uygulamanız için tıklatın **yeni işlev** > **QueueTrigger - C#**.
2. Sıra işlevi tarafından izlenmesi için sıra adı girin:

        queue-newusers
3. Tıklatın  **+**  düğmesini seçin veya kullanmak istediğiniz depolama hesabı oluşturun. Sonra **Oluştur**’a tıklayın.
4. Varsayılan sıra işlevi şablon kodu günlük girişlerini izleyebilmek bu portal tarayıcı penceresini açık bırakın.

#### <a name="create-a-timer-trigger-to-drop-a-message-in-the-queue"></a>Kuyruğa bir ileti bırakmaya Zamanlayıcı tetikleyicisi oluşturma
1. Açık [Azure portal] yeni bir tarayıcı penceresinde ve işlev uygulamanıza gidin.
2. Tıklatın **yeni işlev** > **TimerTrigger - C#**. Ne sıklıkta Zamanlayıcı kod kuyruk işlevinizi test ayarlamak için bir cron ifadesi girin. Sonra **Oluştur**’a tıklayın. Her 30 saniyede çalıştırmak için test isterseniz, aşağıdakileri kullanabilirsiniz [CRON ifade](https://wikipedia.org/wiki/Cron#CRON_expression):

        */30 * * * * *
3. Tıklatın **tümleştir** yeni Zamanlayıcı tetikleyicinizin sekmesi.
4. Altında **çıkış**, tıklatın **+ yeni çıktı**. Ardından **sıra** ve **seçin**.
5. Not kullanmak için ad **sıraya ileti nesnesi**. Bu zamanlayıcı işlev kodu kullanın.

        myQueue
6. İletinin nerede gönderilen sıra adı girin:

        queue-newusers
7. Tıklatın  **+**  düğmesine tıklayarak, kullanılan önceden sıra tetikleyiciyle depolama hesabı seçin. Daha sonra **Kaydet**'e tıklayın.
8. Tıklatın **geliştirme** Zamanlayıcı tetikleyicinizin sekmesi.
9. Daha önce gösterilen aynı sıraya ileti nesne adını kullandığınız sürece, C# Zamanlayıcı işlevi için aşağıdaki kodu kullanabilirsiniz. Daha sonra **Kaydet**'e tıklayın.

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

Bu noktada, örnek cron ifade kullandıysanız C# Zamanlayıcı işlevinin her 30 saniyede yürütür. Zamanlayıcı işlevi için günlükleri her yürütme raporu:

    2016-03-24T10:27:02  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.004 Function started (Id=04061790-974f-4043-b851-48bd4ac424d1)
    2016-03-24T10:27:30.004 C# Timer trigger function executed at: 3/24/2016 10:27:30 AM
    2016-03-24T10:27:30.004 {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.004 Function completed (Success, Id=04061790-974f-4043-b851-48bd4ac424d1)

Sıra işlevi için tarayıcı penceresinde işlenmekte olan her bir ileti görebilirsiniz:

    2016-03-24T10:27:06  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)

## <a name="test-a-function-with-code"></a>Kod ile işlevi test etme
İşlevlerinizi test etmek için harici bir uygulama veya framework oluşturmanız gerekebilir.

### <a name="test-an-http-trigger-function-with-code-nodejs"></a>Bir HTTP tetikleyicisi işlevini koduyla test: Node.js
Bir Node.js uygulaması işlevinizi test etmek için bir HTTP isteği yürütmek için kullanabilirsiniz.
Ayarladığınızdan emin olun:

* `host` İşlevi uygulamasını barındırmak için istek seçenekleri.
* İşlev adınızı `path`.
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

Portalda **günlükleri** penceresinde, çıktı aşağıdakine benzer bir günlüğe kaydedilir işlev yürütülürken:

    2016-03-23T08:08:55  Welcome, you are now connected to log-streaming service.
    2016-03-23T08:08:59.736 Function started (Id=607b891c-08a1-427f-910c-af64ae4f7f9c)
    2016-03-23T08:09:01.153 HTTP trigger function processed a request. RequestUri=http://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1/?code=XXXXXXXXXX==
    2016-03-23T08:09:01.153 Request Headers = {"connection":"Keep-Alive","host":"functionsExample.azurewebsites.net"}
    2016-03-23T08:09:01.153 Name not provided as query string param. Checking body...
    2016-03-23T08:09:01.153 Request Body Type = object
    2016-03-23T08:09:01.153 Request Body = [object Object]
    2016-03-23T08:09:01.153 Processing User Information...
    2016-03-23T08:09:01.215 Function completed (Success, Id=607b891c-08a1-427f-910c-af64ae4f7f9c)


### <a name="test-a-queue-trigger-function-with-code-c"></a>Bir kuyruk tetikleyici işlevini koduyla test: C# #
Bir ileti, sıra bırakma için kod kullanarak bir sıra tetikleyici test edebilirsiniz daha önce bahsedilen. Aşağıdaki kod örneği, C# kod içinde sunulan dayalı [Azure kuyruk depolamaya başlama](../storage/queues/storage-dotnet-how-to-use-queues.md) Öğreticisi. Kod diğer diller için de bu bağlantıdan bulunmaktadır.

Bu kod, bir konsol uygulamasında sınamak için şunları yapmalısınız:

* [App.config dosyasında depolama bağlantı dizenizi yapılandırma](../storage/queues/storage-dotnet-how-to-use-queues.md).
* Geçirmek bir `name` ve `address` uygulama için parametre olarak. Örneğin, `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`. (Bu kodu adını ve adresini yeni bir kullanıcı için komut satırı bağımsız değişkenleri olarak çalışma zamanı sırasında kabul eder.)

Örnek C# kod:

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

Sıra işlevi için tarayıcı penceresinde işlenmekte olan her bir ileti görebilirsiniz:

    2016-03-24T10:27:06  Welcome, you are now connected to log-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"Wes testing queues","address":"in a console app"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)


<!-- URLs. -->

[Azure portal]: https://portal.azure.com
