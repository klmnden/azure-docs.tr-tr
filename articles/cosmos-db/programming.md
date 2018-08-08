---
title: Azure Cosmos DB için sunucu tarafı JavaScript programlama | Microsoft Docs
description: Azure Cosmos DB içinde JavaScript saklı yordamları, veritabanı Tetikleyicileri ve kullanıcı tanımlı işlevlerle (UDF) yazmak için kullanmayı öğrenin. Veritabanı ölçeklenebilirliğinden ipuçları ve daha fazla bilgi edinin.
keywords: Veritabanı tetikleyicileri, saklı yordam, saklı yordamı, veritabanı programı, sproc, azure, Microsoft azure
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: andrl
ms.openlocfilehash: 6374fcf1477d56b9803b63476f3fef38fc12def1
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618905"
---
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Azure Cosmos DB sunucu tarafı programlama: saklı yordamlar, veritabanı tetikleyiciler ve UDF'ler

Azure Cosmos DB'nin dil ile tümleşik, işlem yürütme JavaScript Yazma geliştiricilerin nasıl sağladığını öğrenin **saklı yordamlar**, **Tetikleyicileri**, ve **kullanıcı tanımlı işlevler (UDF'ler)**  içinde yerel olarak bir [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript. JavaScript tümleştirme sevk edilen ve doğrudan veritabanı depolama bölümleri içinde yürütülen program mantığını yazmanızı sağlar. 

Biz burada Andrew Liu Azure Cosmos DB'nin veritabanı sunucu tarafı programlama modeli tanıtılmaktadır aşağıdaki videoyu izleyerek çalışmaya başlamanızı öneririz. 

> [!VIDEO https://www.youtube.com/embed/s0cXdHNlVI0]
>
> 

Ardından, bu makalede, aşağıdaki soruların yanıtlarını burada öğreneceksiniz dönün:  

* Bir saklı yordam, tetikleyici veya JavaScript kullanarak UDF nasıl yazılır?
* Cosmos DB, ACID nasıl garanti?
* İşlem Cosmos DB'de nasıl çalışır?
* Önceden tetikler ve sonrası tetikler nedir ve nasıl bir yazılır?
* Nasıl kaydedin ve HTTP kullanarak bir RESTful şekilde bir saklı yordam, tetikleyici veya UDF yürütme?
* Cosmos DB SDK'ları oluşturma ve yürütme kullanılabilir nedir, yordamlar, tetikleyiciler ve UDF'ler depolanan?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Saklı yordam ve UDF programlamaya giriş
Bu yaklaşım *"JavaScript T-SQL modern gün olarak"* karmaşıklığını tür sistemi uyuşmazlıkları ve nesne ilişkisel eşleme teknolojileri uygulama geliştiricilerden serbest bırakır. Ayrıca, çeşitli zengin uygulamalar oluşturmak için kullanılan iç avantajlar vardır:  

* **Yordam mantıksal:** JavaScript üst düzey bir programlama dili olarak iş mantığı ifade etmek için zengin ve tanıdık bir arabirim sağlar. Karmaşık dizileri işlemlerin verilere yakın gerçekleştirebilirsiniz.
* **Atomik işlemler:** atomik bir tek bir saklı yordam veya tetikleyici içinde gerçekleştirilen işlemleri veritabanına Cosmos DB garanti eder. İlgili işlemleri tek bir toplu işlemde tümünün başarılı ya da bunların hiçbiri başarılı birleştirerek uygulama atomik bu işlevselliği sağlar. 
* **Performans:** birkaç en iyi duruma getirme gibi JSON belgelerinin arabellek havuzunda yavaş gerçekleştirmesi için JSON Javascript dil türü sistemine doğası gereği eşlendi ve ayrıca temel Cosmos DB'de depolama birimidir olgu sağlar ve bunları için kodu yürütürken üzerine uygun hale getirir. Veritabanı dağıtımı iş mantığı ile ilgili daha fazla performans avantajları vardır:
  
  * Toplu işleme – Geliştiriciler grubu ekleme gibi işlemleri ve bunları toplu olarak gönderin. Maliyet ağ trafiği gecikme süresi ve ayrı işlemler oluşturmak için mağaza ek yükünü önemli ölçüde azaltılır. 
  * Önceden derleme – Cosmos DB, saklı yordamlar, tetikleyiciler ve her JavaScript derleme maliyet önlemek için kullanıcı tanımlı işlevler (UDF'ler) işlemini gerçekleştirir. Yordam mantığı bayt kod oluşturma ek yükü en az bir değere amorti edilmiş.
  * Sıralama – birçok işlemleri gereksinimini yan olabilecek bir veya daha fazla ikincil depolama işlemleri gerçekleştirdikten içeren etki ("tetikleyici"). Kararlılık yanı sıra sunucuya taşıdığınızda, daha yüksek performanslı budur. 
* **Kapsülleme:** saklı yordamlar, iş mantığı iki avantajı sahip tek bir yerde gruplandırmak için kullanılabilir:
  * Veri mimarları veri uygulamalarından bağımsız olarak gelişmesi sağlar ham veriler en üstünde bir soyutlama katmanı ekler. Bu Soyutlama Katmanı veriler ile verileri doğrudan dağıtılacak varsa uygulamasına desteklenmiş gereken kırılır varsayımların nedeniyle şemasız, olduğunda avantajlıdır.  
  * Bu soyutlama betikleri erişimden kolaylaştırarak inovasyonu verilerine güvenli tutmaya kuruluşların olanak tanır.  

Aracılığıyla oluşturma ve yürütme veritabanı tetikleyicileri, saklı yordamlar ve özel sorgu işleçleri desteklenir [Azure portalında](https://portal.azure.com), [REST API](/rest/api/cosmos-db/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), ve [istemci SDK'ları](sql-api-sdk-dotnet.md) .NET, Node.js ve JavaScript gibi birçok platformda.

Bu öğreticide [Node.js SDK'sı ile soru gösterir](http://azure.github.io/azure-documentdb-node-q/) söz dizimi ve saklı yordamlar, tetikleyiciler ve UDF'ler kullanımını göstermek için.   

## <a name="stored-procedures"></a>Saklı yordamlar
### <a name="example-write-a-stored-procedure"></a>Örnek: bir saklı yordam yazma
"Hello World" yanıt döndüren basit bir saklı yordam ile başlayalım.

```javascript
var helloWorldStoredProc = {
    id: "helloWorld",
    serverScript: function () {
        var context = getContext();
        var response = context.getResponse();

        response.setBody("Hello, World");
    }
}
```

Saklı yordamlar, koleksiyon kaydedilir ve herhangi bir belge ve ek koleksiyonda mevcut üzerinde çalışabilir. Aşağıdaki kod parçacığı, helloWorld saklı yordamı bir koleksiyon ile kaydetme işlemi gösterilmektedir. 


```javascript
// register the stored procedure
var createdStoredProcedure;
client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
    .then(function (response) {
        createdStoredProcedure = response.resource;
        console.log("Successfully created stored procedure");
    }, function (error) {
        console.log("Error", error);
    });
```


Saklı yordamı kaydedildiğinde, koleksiyonunda yürütmek ve sonuçları istemcide yeniden okuyun. 

```javascript
// execute the stored procedure
client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
    .then(function (response) {
        console.log(response.result); // "Hello, World"
    }, function (err) {
        console.log("Error", error);
    });
```

Bağlam nesnesi, Cosmos DB depolama üzerinde gerçekleştirilen tüm işlemler erişimin yanı sıra, istek ve yanıt nesnelere erişimi sağlar. Bu durumda, istemciye gönderilen yanıt gövdesinin ayarlamak için yanıt nesnesini kullanın. Daha fazla bilgi için [Azure Cosmos DB JavaScript server SDK Belgeleri](http://azure.github.io/azure-documentdb-js-server/).  

Bize bu örneğe göre genişletin ve veritabanı ile ilgili daha fazla işlevsellik saklı yordamı ekleyin. Saklı yordamlar oluşturabilir, güncelleştirme, okuma, sorgulama ve belgeleri ve koleksiyon içinde ekleri silin.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Örnek: bir belge oluşturmak için bir saklı yordam yazma
Sonraki kod parçacığı, Cosmos DB kaynaklarıyla etkileşim kurmak için bağlam nesnesi kullanma işlemi gösterilmektedir.

```javascript
var createDocumentStoredProc = {
    id: "createMyDocument",
    serverScript: function createMyDocument(documentToCreate) {
        var context = getContext();
        var collection = context.getCollection();

        var accepted = collection.createDocument(collection.getSelfLink(),
              documentToCreate,
              function (err, documentCreated) {
                  if (err) throw new Error('Error' + err.message);
                  context.getResponse().setBody(documentCreated.id)
              });
        if (!accepted) return;
    }
}
```


Bu saklı yordam giriş documentToCreate, geçerli koleksiyon içinde oluşturulacak belge gövdesinin alır. Tüm bu işlemler zaman uyumsuz ve JavaScript işlevi geri çağırmaları üzerinde bağlıdır. Geri çağırma işlevi, iki parametre, işlem başarısız durumda hata nesnesi için diğeri için oluşturulan nesnesi vardır. Geri çağırma içinde kullanıcıların özel durumu işlemek veya bir hata oluşturur. Azure Cosmos DB çalışma zamanı, bir geri çağırma sağlanmadı ve bir hata durumunda, bir hata oluşturur.   

İşlem başarısız olursa yukarıdaki örnekte, bir hata geri çağırma oluşturur. Aksi takdirde, istemciye yanıt gövdesi olarak oluşturulan belgenin bir kimlik ayarlar. Bu saklı yordam giriş parametreleriyle nasıl yürütüleceğini aşağıda verilmiştir.

```javascript
// register the stored procedure
client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
    .then(function (response) {
        var createdStoredProcedure = response.resource;

        // run stored procedure to create a document
        var docToCreate = {
            id: "DocFromSproc",
            book: "The Hitchhiker’s Guide to the Galaxy",
            author: "Douglas Adams"
        };

        return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
              docToCreate);
    }, function (error) {
        console.log("Error", error);
    })
.then(function (response) {
    console.log(response); // "DocFromSproc"
}, function (error) {
    console.log("Error", error);
});
```

Belge gövdesi bir dizi girişi olarak almak ve bunları tüm bunların her birini ayrı ayrı oluşturmak için birden çok istek yerine aynı saklı yordam yürütme oluşturmak için bu saklı yordamı değiştirilebilir. Bu saklı yordamı, Cosmos DB (Bu öğreticinin ilerleyen bölümlerinde açıklanmıştır) için bir etkin toplu içeri Aktarıcı uygulamak için kullanılabilir.   

Açıklanan örnek saklı yordamlar kullanma gösterilmektedir. Sonraki öğreticide daha sonra tetikleyiciler ve kullanıcı tanımlı işlevler (UDF'ler) hakkında bilgi edineceksiniz.

### <a name="known-issues"></a>Bilinen sorunlar

Azure portalını kullanarak bir saklı yordam tanımlarken, giriş parametreleri için saklı yordam her zaman bir dize olarak gönderilir. Dizelerden oluşan bir dizi girdi olarak geçirdiğiniz olsa bile, dizi dizeye dönüştürülür ve saklı yordamı gönderilir. Geçici çözüm bu sorunu dize dizisi olarak ayrıştırılacak, saklı yordam içinde bir işlev tanımlayabilirsiniz. Aşağıdaki kod, bir dizi olarak dizeyi ayrıştırmak için bir örnek verilmiştir: 

```javascript
function sample(arr) {
    if (typeof arr === "string") arr = JSON.parse(arr);
    
    arr.forEach(function(a) {
        // do something here
        console.log(a);
    });
}
```

## <a name="database-program-transactions"></a>Veritabanı programı işlemleri
Tipik bir veritabanındaki işlem, tek bir mantıksal birim iş olarak gerçekleştirilen işlemleri öğesinin bir dizisi olarak tanımlanabilir. Her işlem sağlar **ACID garantileri**. ACID dört özellikleri için - kararlılık, tutarlılık, yalıtım ve dayanıklılık anlamına gelir iyi bilinen bir kısaltma olduğundan.  

Kısaca, kararlılık, bir işlem içinde yapılan bütün işler tek bir birim olarak kabul edilir garanti eder nerede ya da tümünün kaydedilmiş veya yok. Tutarlılık veri işlemleri arasında her zaman iyi iç durumda olduğundan emin olur. Yalıtım iki işlem diğer çalışanlarla genellikle müdahale, çoğu ticari sistemleri kullanılabilecek birden fazla yalıtım düzeyleri uygulama ihtiyaçlarına göre sağlar garanti eder. Dayanıklılık, veritabanında kaydedilmiş herhangi bir değişikliği her zaman mevcut olmasını sağlar.   

Cosmos DB'de JavaScript veritabanıyla aynı bellek alanına barındırılır. Bu nedenle, saklı yordamları içinden yapılan istekleri ve Tetikleyicileri veritabanı oturumunun aynı kapsam içinde yürütün. Bu özellik, Cosmos DB'ın tek bir saklı yordam/tetikleyici parçası olan tüm işlemler için ACID garantisi sağlar. Aşağıdaki saklı yordam tanımında göz önünde bulundurun:

```javascript
// JavaScript source code
var exchangeItemsSproc = {
    id: "exchangeItems",
    serverScript: function (playerId1, playerId2) {
        var context = getContext();
        var collection = context.getCollection();
        var response = context.getResponse();

        var player1Document, player2Document;

        // query for players
        var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
        var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
            function (err, documents, responseOptions) {
                if (err) throw new Error("Error" + err.message);

                if (documents.length != 1) throw "Unable to find both names";
                player1Document = documents[0];

                var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                    function (err2, documents2, responseOptions2) {
                        if (err2) throw new Error("Error" + err2.message);
                        if (documents2.length != 1) throw "Unable to find both names";
                        player2Document = documents2[0];
                        swapItems(player1Document, player2Document);
                        return;
                    });
                if (!accept2) throw "Unable to read player details, abort ";
            });

        if (!accept) throw "Unable to read player details, abort ";

        // swap the two players’ items
        function swapItems(player1, player2) {
            var player1ItemSave = player1.item;
            player1.item = player2.item;
            player2.item = player1ItemSave;

            var accept = collection.replaceDocument(player1._self, player1,
                function (err, docReplaced) {
                    if (err) throw "Unable to update player 1, abort ";

                    var accept2 = collection.replaceDocument(player2._self, player2,
                        function (err2, docReplaced2) {
                            if (err) throw "Unable to update player 2, abort"
                        });

                    if (!accept2) throw "Unable to update player 2, abort";
                });

            if (!accept) throw "Unable to update player 1, abort";
        }
    }
}

// register the stored procedure in Node.js client
client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
    .then(function (response) {
        var createdStoredProcedure = response.resource;
    }
);
```

Bu saklı yordamı hareketleri içinde tek bir işlemde iki oyuncuların arasındaki ticari öğelerine bir oyun uygulaması kullanır. Saklı yordam bağımsız değişken olarak geçirilen player kimlikleri için karşılık gelen her iki belge okuma girişiminde bulunur. Her iki player belge bulunamazsa, saklı yordam öğelerin değiştirerek belgeleri güncelleştirir. Süreç boyunca herhangi bir hatayla karşılaşılmazsa, örtük olarak işlem durdurur bir JavaScript özel durumu oluşturur.

Saklı yordam koleksiyon kayıtlıysa karşı tek bölümlü bir koleksiyon olduğundan ve işlem, koleksiyon içindeki bütün belgelere kapsamlıdır. Koleksiyon bölümlere ayrılmışsa, saklı yordamlar tek bir bölüm anahtarı işlem kapsamı içinde yerine getirilir. Her saklı yordam yürütme ardından işlem altında çalışmalıdır kapsamına karşılık gelen bir bölüm anahtarı değerini içermelidir. Daha fazla bilgi için [Azure Cosmos DB bölümleme](partition-data.md).

### <a name="commit-and-rollback"></a>Kaydetme ve geri alma
İşlem, iç ve yerel olarak Cosmos DB'nin JavaScript programlama modeline tümleştirilmiştir. Bir JavaScript işlevinin içinde tüm işlemleri otomatik olarak tek bir işlem altında sarmalanır. JavaScript bir özel durum tamamlanırsa işlemleri veritabanına kararlıyız. Aslında, ilişkisel veritabanlarını "BEGIN TRANSACTION" ve "COMMIT TRANSACTION" deyimlerinde Cosmos DB'de örtüktür.  

Betikten yayılmadan herhangi bir özel durumu ise, Cosmos DB'nin JavaScript çalışma zamanı tüm işlem döndürülmesine neden olur. Önceki örnekte gösterildiği gibi bir özel durum etkili bir şekilde bir "geri alma işlemi" Cosmos DB'de eşdeğerdir.

### <a name="data-consistency"></a>Veri tutarlılığı
Saklı yordamları ve tetikleyicileri, Azure Cosmos DB kapsayıcısının birincil Çoğaltmada her zaman yürütülür. Bu yordamlar teklif güçlü tutarlılık okumalardan içinde depolanan sağlar. Kullanıcı tanımlı işlevleri kullanarak sorguları birincil veya ikincil bir çoğaltmaya yürütülmesi gerekir, ancak uygun çoğaltma seçerek istenen tutarlılık düzeyi karşılayacak şekilde emin olun.

## <a name="bounded-execution"></a>Sınırlanmış yürütme
Belirtilen sunucu içinde tüm Cosmos DB işlemlerini tamamlamanız gerekir istek zaman aşımı süresi. Bu kısıtlama, JavaScript işlevleri (saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler) için de geçerlidir. Bir işlem ile bu zaman sınırı tamamlanmazsa, işlem geri alındı. JavaScript işlevleri, süre sınırı içinde son veya toplu iş/sürdürme yürütme için bir devamlılık tabanlı modelini uygular.  

Saklı yordamları ve Tetikleyicileri zaman sınırları, tüm işlevleri koleksiyon nesnesi (oluşturma, okuma, değiştirin ve belgeleri ve eklerini Sil) işleneceğini geliştirilmesini basitleştirmek için döndürülecek bir Boolean değeri temsil eden olup bu işlemi tamamlanır. Bu değer, false ise, bu süre dolmak üzere olduğunu ve yordam yürütme sarmalamanız gerekir göstergesidir.  İşlemler kuyruğa alınmış önceki ilk kabul edilmeyen deposu işlemi için saklı yordam sürede tamamlanır ve başka istek kuyruğa almadığından tamamlanması garanti edilir.  

JavaScript işlevleri de kaynak tüketimine ilişkin ilişkisindeki. Cosmos DB kapsayıcıları bir dizi veya koleksiyon başına aktarım hızını ayırır. Aktarım hızı, CPU, bellek ve GÇ tüketimi istek birimleri veya RU adlı normalleştirilmiş birimi açısından ifade edilir. JavaScript işlevleri olabilecek çok sayıda RU kısa süre içinde yukarı kullanabilirsiniz ve koleksiyonun sınırına ulaşıldığında oranı-limited alabilirsiniz. Temel veritabanı işlemleri kullanılabilirliğini sağlamak için kaynak kullanımı yoğun saklı yordamlar da karantinaya.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Örnek: bir veritabanı programa veri alma toplu
Toplu-belgeler bir koleksiyona içe aktarımı için yazılan bir saklı yordam örneği aşağıdadır. Saklı yordam Boolean denetleyerek sınırlanmış yürütme nasıl işlediğini unutmayın createDocument dönüş değeri ve ardından izlemek ve arasında toplu ilerleme durumunu sürdürmek için saklı yordam her çalıştırılışı içinde eklenen belge sayısını kullanır.

```javascript
function bulkImport(docs) {
    var collection = getContext().getCollection();
    var collectionLink = collection.getSelfLink();

    // The count of imported docs, also used as current doc index.
    var count = 0;

    // Validate input.
    if (!docs) throw new Error("The array is undefined or null.");

    var docsLength = docs.length;
    if (docsLength == 0) {
        getContext().getResponse().setBody(0);
    }

    // Call the create API to create a document.
    tryCreate(docs[count], callback);

    // Note that there are 2 exit conditions:
    // 1) The createDocument request was not accepted. 
    //    In this case the callback will not be called, we just call setBody and we are done.
    // 2) The callback was called docs.length times.
    //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
    function tryCreate(doc, callback) {
        var isAccepted = collection.createDocument(collectionLink, doc, callback);

        // If the request was accepted, callback will be called.
        // Otherwise report current count back to the client, 
        // which will call the script again with remaining set of docs.
        if (!isAccepted) getContext().getResponse().setBody(count);
    }

    // This is called when collection.createDocument is done in order to process the result.
    function callback(err, doc, options) {
        if (err) throw err;

        // One more document has been inserted, increment the count.
        count++;

        if (count >= docsLength) {
            // If we created all documents, we are done. Just set the response.
            getContext().getResponse().setBody(count);
        } else {
            // Create next document.
            tryCreate(docs[count], callback);
        }
    }
}
```

## <a id="trigger"></a> Veritabanı Tetikleyicileri
### <a name="database-pre-triggers"></a>Veritabanı öncesi Tetikleyicileri
Cosmos DB, yürütülen ya da bir belge üzerindeki bir işlem tarafından tetiklenen Tetikleyiciler sağlar. Örneğin, önceden bir tetikleyici belirtebilirsiniz bir belgesi oluşturuyorsunuz – belge oluşturulmadan önce bu ön tetikleyici çalıştırılır. Aşağıdaki örnek, ön Tetikleyicileri oluşturulmakta olan bir belge özelliklerini doğrulamak için nasıl kullanılabileceğini gösterir:

```javascript
var validateDocumentContentsTrigger = {
    id: "validateDocumentContents",
    serverScript: function validate() {
        var context = getContext();
        var request = context.getRequest();

        // document to be created in the current operation
        var documentToCreate = request.getBody();

        // validate properties
        if (!("timestamp" in documentToCreate)) {
            var ts = new Date();
            documentToCreate["my timestamp"] = ts.getTime();
        }

        // update the document that will be created
        request.setBody(documentToCreate);
    },
    triggerType: TriggerType.Pre,
    triggerOperation: TriggerOperation.Create
}
```

Ve karşılık gelen Node.js istemci tarafı kaydı kod tetikleyicisi için:

```javascript
// register pre-trigger
client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
    .then(function (response) {
        console.log("Created", response.resource);
        var docToCreate = {
            id: "DocWithTrigger",
            event: "Error",
            source: "Network outage"
        };

        // run trigger while creating above document 
        var options = { preTriggerInclude: "validateDocumentContents" };

        return client.createDocumentAsync(collection.self,
              docToCreate, options);
    }, function (error) {
        console.log("Error", error);
    })
.then(function (response) {
    console.log(response.resource); // document with timestamp property added
}, function (error) {
    console.log("Error", error);
});
```

Öncesi tetikleyici, giriş parametreleri bulunamaz. İstek nesnesi, işlemle ilişkili İstek iletisini işlemek için kullanılabilir. Burada, ön tetikleyici bir belge oluşturulmasını çalıştırın ve istek iletisi gövdesi JSON biçiminde oluşturulacak belge içeriyor.   

Tetikleyiciler kaydettiğinizde, kullanıcılar ile çalışabilmesi için işlemleri belirtebilir. Bu tetikleyici, tetikleyici aşağıdaki kodda gösterildiği gibi bir değiştirme işlemi kullanılarak izin verilmeyen anlamına gelir TriggerOperation.Create ile oluşturuldu.

```javascript
var options = { preTriggerInclude: "validateDocumentContents" };

client.replaceDocumentAsync(docToReplace.self,
              newDocBody, options)
.then(function (response) {
    console.log(response.resource);
}, function (error) {
    console.log("Error", error);
});

// Fails, can’t use a create trigger in a replace operation
```
### <a name="database-post-triggers"></a>Veritabanı sonrası Tetikleyicileri
Öncesi Tetikleyicileri gibi sonrası Tetikleyicileri belgesinde bir işlemle ilişkili ve tüm giriş parametrelerini yakalayana. Çalışan **sonra** işlemi tamamlandı ve istemciye gönderilen yanıt iletisi erişebilir.   

Aşağıdaki örnek, eylem sonrası Tetikleyicileri gösterir:
```javascript
var updateMetadataTrigger = {
    id: "updateMetadata",
    serverScript: function updateMetadata() {
        var context = getContext();
        var collection = context.getCollection();
        var response = context.getResponse();

        // document that was created
        var createdDocument = response.getBody();

        // query for metadata document
        var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
        var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
            updateMetadataCallback);
        if(!accept) throw "Unable to update metadata, abort";

        function updateMetadataCallback(err, documents, responseOptions) {
            if(err) throw new Error("Error" + err.message);
                     if(documents.length != 1) throw 'Unable to find metadata document';

                     var metadataDocument = documents[0];

                     // update metadata
                     metadataDocument.createdDocuments += 1;
                     metadataDocument.createdNames += " " + createdDocument.id;
                     var accept = collection.replaceDocument(metadataDocument._self,
                           metadataDocument, function(err, docReplaced) {
                                  if(err) throw "Unable to update metadata, abort";
                           });
                     if(!accept) throw "Unable to update metadata, abort";
                     return;                    
        }                                                                                            
    },
    triggerType: TriggerType.Post,
    triggerOperation: TriggerOperation.All
}

```
Tetikleyiciyi aşağıdaki örnekte gösterildiği gibi kaydedilebilir.
```javascript
// register post-trigger
client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
    .then(function(createdTrigger) { 
        var docToCreate = { 
            name: "artist_profile_1023",
            artist: "The Band",
            albums: ["Hellujah", "Rotators", "Spinning Top"]
        };

        // run trigger while creating above document 
        var options = { postTriggerInclude: "updateMetadata" };

        return client.createDocumentAsync(collection.self,
              docToCreate, options);
    }, function(error) {
        console.log("Error" , error);
    })
.then(function(response) {
    console.log(response.resource); 
}, function(error) {
    console.log("Error" , error);
});
```

Bu tetikleyici için meta veri belgesi sorgular ve yeni oluşturulan belgeyi ilişkin ayrıntılarla güncelleştirir.  

Unutmayın, bir şey **işlem** Cosmos DB Tetikleyicileri yürütülmesi. Bu sonrası tetikleyici oluşturulamadığından orijinal belgenin aynı işlemin bir parçası olarak çalışır. Bu nedenle, sonrası tetikleyiciden (meta veri belgesi güncelleştirilecek bulamıyorsanız, örneğin) bir özel durum, tüm işlem başarısız olur ve geri alınamaz. Herhangi bir belge oluşturulur ve bir özel durum döndürdü.  

## <a id="udf"></a>Kullanıcı tanımlı işlevler
Kullanıcı tanımlı işlevlerle (UDF), Azure Cosmos DB SQL sorgu dil dilbilgisi genişletmek ve özel iş mantığı uygulamak için kullanılır. Gelen yalnızca çağrılabilir sorguları içinde. Bunlar, context nesnesi için erişimi yoktur ve yalnızca işlem JavaScript olarak kullanılmak üzere tasarlanmıştır. Bu nedenle, UDF'ler, Cosmos DB hizmetinin ikincil çoğaltmalarda çalıştırılabilir.  

Aşağıdaki örnek çeşitli gelir köşeli ayraçlar için tarifeleri üzerinden göre gelir vergi hesaplamak için bir UDF oluşturur ve ardından birden fazla 20.000 $ vergiler Ücretli tüm kişilerin bulmak için bir sorgu içinde kullanır.

```javascript
var taxUdf = {
    id: "tax",
    serverScript: function tax(income) {

        if(income == undefined) 
            throw 'no input';

        if (income < 1000) 
            return income * 0.1;
        else if (income < 10000) 
            return income * 0.2;
        else
            return income * 0.4;
    }
}
```

Aşağıdaki örnek sorgularda gibi UDF sonradan kullanılabilir:

```javascript
// register UDF
client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
    .then(function(response) { 
        console.log("Created", response.resource);

        var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
        return client.queryDocuments('dbs/testdb/colls/testColl',
               query).toArrayAsync();
    }, function(error) {
        console.log("Error" , error);
    })
.then(function(response) {
    var documents = response.feed;
    console.log(response.resource); 
}, function(error) {
    console.log("Error" , error);
});
```

## <a name="javascript-language-integrated-query-api"></a>JavaScript dil ile tümleşik sorgu API'si
Azure Cosmos DB SQL dil bilgisi kullanarak sorgu göndermeye ek olarak, sunucu tarafı SDK'sı SQL bilgileri olmadan fluent bir JavaScript arabirimi kullanarak en iyi duruma getirilmiş sorgular gerçekleştirmenize olanak sağlar. API koşul işlevleri chainable işleve geçirerek sorguları programlı olarak oluşturmanıza olanak sağlayan JavaScript sorgu ECMAScript5'ın dizi öğelerin ve yaygın olarak kullanılan JavaScript kitaplıklarını Lodash gibi tanıdık bir söz dizimi ile çağırır. Sorgular, Azure Cosmos DB'nin dizinleri verimli bir şekilde kullanarak yürütülecek JavaScript çalışma zamanı tarafından ayrıştırılan.

> [!NOTE]
> `__` (çift alt çizgi) olan bir diğer `getContext().getCollection()`.
> <br/>
> Diğer bir deyişle, kullanabileceğiniz `__` veya `getContext().getCollection()` JavaScript sorgu API'si erişmek için.
> 
> 

Desteklenen işlevler şunlardır:

<ul>
<li>
<b>... chain(). değer ([geri çağırma] [, Seçenekleri])</b>
<ul>
<li>
Value()) ile bitirilmelidir zincirleme bir arama başlatır.
</li>
</ul>
</li>
<li>
<b>Filtre (predicateFunction [, Seçenekler] [, geri çağırma])</b>
<ul>
<li>
Sonuç kümesine girdi belgelerini içeri/dışarı filtrelemek için true/false döndüren bir koşul işlevini kullanarak giriş filtreler. Bu işlev bir WHERE yan tümcesinde SQL benzer şekilde davranır.
</li>
</ul>
</li>
<li>
<b>Harita (transformationFunction [, Seçenekler] [, geri çağırma])</b>
<ul>
<li>
Her giriş öğesini bir JavaScript nesnesi veya değer eşleyen bir dönüştürme işlevi verilen bir projeksiyon geçerlidir. Bu işlev bir SELECT yan tümcesi SQL benzer şekilde davranır.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, Seçenekler] [, geri çağırma])</b>
<ul>
<li>
Bu işlev, her giriş öğesinden tek bir özelliğinin değerini ayıklar bir eşleme için bir kısayoldur.
</li>
</ul>
</li>
<li>
<b>düzleştirme ([isShallow] [, Seçenekler] [, geri çağırma])</b>
<ul>
<li>
Bir araya getirir ve her bir giriş öğesini dizilerden tek bir diziye düzleştirir. Bu işlev SelectMany LINQ benzer şekilde davranır.
</li>
</ul>
</li>
<li>
<b>sortBy ([karşılaştırma] [, Seçenekler] [, geri çağırma])</b>
<ul>
<li>
Yeni bir belge kümesini kullanarak belirli bir koşul artan giriş belgesi stream belgeleri sıralayarak üretir. Bu işlev bir ORDER BY yan tümcesi içinde SQL benzer şekilde davranır.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([karşılaştırma] [, Seçenekler] [, geri çağırma])</b>
<ul>
<li>
Belgeler giriş belgesi akış, belirli bir koşul kullanarak azalan düzende sıralayarak yeni bir belge kümesi üretir. Bu işlev bir x DESC ORDER BY yan tümcesinde SQL benzer şekilde davranır.
</li>
</ul>
</li>
</ul>


Koşul ve/veya Seçici işlevlerinin içine dahil edilirse, aşağıdaki JavaScript yapıları otomatik olarak doğrudan Azure Cosmos DB dizinlerini çalıştırılmak üzere iyileştirilen:

* Basit işleçleri: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Nesne sabit değeri de dahil olmak üzere hazır: {}
* dönüş var

Aşağıdaki JavaScript yapıları için Azure Cosmos DB dizinleri en iyi duruma getirilmiş değil:

* Denetim akışı (örneğin, varsa, ancak)
* İşlev çağrıları

Daha fazla bilgi için [sunucu tarafı JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Örnek: JavaScript sorgu API'si kullanarak bir saklı yordam yazma
Aşağıdaki kod örneği bir saklı yordam bağlamında JavaScript sorgu API'si nasıl kullanılabileceğini bir örnektir. Saklı yordamı bir giriş parametresi tarafından belirtilen bir belge, ekler ve bir meta veri güncelleştirmeleri kullanarak belge `__.filter()` minSize, Maxsıze ve giriş belgenin boyut özelliğinin üzerinde temel totalSize yöntemi.

```javascript
/**
 * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
 */
function insertDocumentAndUpdateMetadata(doc) {
  // HTTP error codes sent to our callback funciton by DocDB server.
  var ErrorCode = {
    RETRY_WITH: 449,
  }

  var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
    if (err) throw err;

    // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
    if (!doc.isMetadata && doc.size > 0) {
      // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
      var result = __.filter(function(x) {
        return x.isMetadata === true
      }, function(err, feed, options) {
        if (err) throw err;

        // We assume that metadata doc was pre-created and must exist when this script is called.
        if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

        // The metadata document.
        var metaDoc = feed[0];

        // Update metaDoc.minSize:
        // for 1st document use doc.Size, for all the rest see if it's less than last min.
        if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
        else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

        // Update metaDoc.maxSize.
        metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

        // Update metaDoc.totalSize.
        metaDoc.totalSize += doc.size;

        // Update/replace the metadata document in the store.
        var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
          if (err) throw err;
          // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
          //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
          //       We have to take care of that on the client side.
        });
        if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
      });
      if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
    }
  });
  if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
}
```

## <a name="sql-to-javascript-cheat-sheet"></a>SQL için Javascript kopya kağıdı
Aşağıdaki tabloda, çeşitli SQL sorguları ve karşılık gelen JavaScript sorguları sunulmaktadır.

Özellik anahtarları içeren SQL sorguları, belge olarak (örneğin, `doc.id`) büyük küçük harfe duyarlıdır.

|SQL| JavaScript sorgu API'si|Aşağıdaki açıklama|
|---|---|---|
|SEÇİN *<br>Belgelerinden| __.map(function(doc) { <br>&nbsp;&nbsp;&nbsp;&nbsp;Belge döndürür;<br>});|1|
|SELECT docs.id, olarak docs.message msg, docs.actions <br>Belgelerinden|__.map(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;{döndürür<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Msg: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|2|
|SEÇİN *<br>Belgelerinden<br>WHERE docs.id="X998_Y998"|__.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;doc.id dönüş === "X998_Y998";<br>});|3|
|SEÇİN *<br>Belgelerinden<br>WHERE ARRAY_CONTAINS (belgeleri. Etiketler, 123)|__.filter(function(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;x.Tags dönüş & & x.Tags.indexOf(123) > -1;<br>});|4|
|SELECT docs.id, docs.message msg olarak<br>Belgelerinden<br>WHERE docs.id="X998_Y998"|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;doc.id dönüş === "X998_Y998";<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.map(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{döndürür<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Msg: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.Value();|5|
|DEĞER etiketi<br>Belgelerinden<br>Etiket IN docs katılın. Etiketleri<br>ORDER BY docs._ts|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Belge döndürür. Etiketler & & Array.isArray (doc. Etiketler);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;doc._ts döndürür;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Value()|6|

Yukarıdaki tabloda her bir sorguda aşağıdaki açıklamaları açıklanmaktadır.
1. Sonuç tüm belgelerde (devamlılık belirteci ile sayfalandırılmış) olur.
2. Kimliği, ileti (diğer adlı msg için) ve tüm belgeleri eylemden projeleri.
3. Sorgular için koşul belgelerle: kimlik = "X998_Y998".
4. Etiketler özelliği ve etiketlere sahip olan belgeler için sorgulardır 123 değerini içeren bir dizi.
5. Bir koşul ile belgeler için sorgu kimliği = "X998_Y998" ve ardından kodu ve iletinin (ileti için diğer adlı) projeleri.
6. Etiketler, bir dizi özelliği ve elde edilen belgeler _ts zaman damgası sistem özelliği sıralar ve ardından projeleri + etiketler dizisi düzleştirir belgeler için filtreler.


## <a name="runtime-support"></a>Çalışma zamanı desteği
Azure Cosmos DB [JavaScript sunucu tarafı API'si](http://azure.github.io/azure-documentdb-js-server/) en temel JavaScript dil özellikleri tarafından standart olarak iyi destek sağlayan [ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Güvenlik
JavaScript saklı yordamları ve Tetikleyicileri korumalı, böylece bir betik etkilerini diğer veritabanı düzeyinde snapshot işlem yalıtım olmadan sızıntı değil. Çalışma zamanı ortamlarını havuza ancak her sonrasında çalıştırma bağlamında temizlendi. Bu nedenle bunlar birbirinden istenmeyen yan etkileri güvenli olmasını garanti edilir.

### <a name="pre-compilation"></a>Önceden derleme
Saklı yordamlar, tetikleyiciler ve UDF'ler her betik çağırmayı zamanında derleme maliyet önlemek için bayt kod biçimine örtülü olarak önceden derlenmiş. Saklı yordam çağırma hızlı ve düşük bir ayak izine sahip önceden derleme sağlar.

## <a name="client-sdk-support"></a>İstemci SDK desteği
Azure Cosmos DB yanı sıra [Node.js](sql-api-sdk-node.md) API, Azure Cosmos DB sahip [.NET](sql-api-sdk-dotnet.md), [.NET Core](sql-api-sdk-dotnet-core.md), [Java](sql-api-sdk-java.md), [JavaScript ](http://azure.github.io/azure-documentdb-js/), ve [Python SDK'ları](sql-api-sdk-python.md) de SQL API'si için. Saklı yordamlar, tetikleyiciler ve UDF'ler oluşturulabilir ve bu SDK'ları da birini kullanarak. Aşağıdaki örnek, oluşturma ve .NET istemcisini kullanarak bir saklı yordam yürütme gösterilmektedir. .NET türleri JSON olarak saklı yordamı yöntemlere geçirilen ve geri nasıl unutmayın.

```javascript
var markAntiquesSproc = new StoredProcedure
{
    Id = "ValidateDocumentAge",
    Body = @"
            function(docToCreate, antiqueYear) {
                var collection = getContext().getCollection();    
                var response = getContext().getResponse();    

                if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                    docToCreate.antique = true;
                }

                collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                    function(err, docCreated, options) { 
                        if(err) throw new Error('Error while creating document: ' + err.message);                              
                        if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                        response.setBody(docCreated);
                });
         }"
};

// register stored procedure
StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
dynamic document = new Document() { Id = "Borges_112" };
document.Title = "Aleph";
document.Year = 1949;

// execute stored procedure
Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "ValidateDocumentAge"), document, 1920);
```

Bu örnek nasıl kullanılacağını gösterir [SQL .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) öncesi bir tetikleyici oluşturursunuz ve etkin bir tetikleyici ile belge oluşturma. 

```javascript
Trigger preTrigger = new Trigger()
{
    Id = "CapitalizeName",
    Body = @"function() {
        var item = getContext().getRequest().getBody();
        item.id = item.id.toUpperCase();
        getContext().getRequest().setBody(item);
    }",
    TriggerOperation = TriggerOperation.Create,
    TriggerType = TriggerType.Pre
};

Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
    new RequestOptions
    {
        PreTriggerInclude = new List<string> { "CapitalizeName" },
    });
```

Ve aşağıdaki örnek, bir kullanıcı tanımlı işlev (UDF) oluşturmak ve bunu kullanmak gösterilmiştir bir [SQL sorgusu](sql-api-sql-query.md).

```javascript
UserDefinedFunction function = new UserDefinedFunction()
{
    Id = "LOWER",
    Body = @"function(input) 
    {
        return input.toLowerCase();
    }"
};

foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
    "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
{
    Console.WriteLine("Read {0} from query", book);
}
```

## <a name="rest-api"></a>REST API
Tüm Azure Cosmos DB işlemleri bir RESTful şekilde gerçekleştirilebilir. Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler koleksiyonu altında HTTP POST kullanılarak kaydedilebilir. Aşağıdaki örnek, bir saklı yordam kaydettirmek gösterilmektedir:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT


    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


Saklı yordam oluşturmak için saklı yordam içeren gövdesi bir POST isteği URI dbs/testdb/colls/testColl/sprocs karşı yürüterek kaydedilir. Tetikleyiciler ve UDF'ler benzer bir POST/tetikleyiciler ve /udfs karşı sırasıyla göndererek kaydedilebilir.
Bu saklı yordamı can ardından bir POST isteği karşı kaynak bağlantısını keserek yürütülmesi gerekir:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Burada, saklı yordam için giriş istek gövdesinde geçirilir. Giriş, giriş parametrelerinin JSON dizisi geçirilir. Saklı yordam ilk girdi yanıt gövdesi bir belge olarak alır. Aldığınız yanıt aşağıdaki gibidir:

    HTTP/1.1 200 OK

    { 
      name: 'TestDocument',
      book: 'Autumn of the Patriarch',
      id: 'V7tQANV3rAkDAAAAAAAAAA==',
      ts: 1407830727,
      self: 'dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/',
      etag: '6c006596-0000-0000-0000-53e9cac70000',
      attachments: 'attachments/',
      Price: 200
    }


Tetikleyicileri, saklı yordamlar, aksine, doğrudan yürütülemez. Bunun yerine, bir belgedeki bir işlemin parçası olarak yürütülür. HTTP üst bilgileri kullanarak bir istekle çalıştırılacak tetikleyici belirtebilirsiniz. Aşağıdaki kod, bir belge oluşturma isteği gösterir.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger

    {
       "name": "newDocument",
       "title": "The Wizard of Oz",
       "author": "Frank Baum",
       "pages": 92
    }


Burada istekle çalıştırılacak öncesi tetikleyici x-ms-documentdb-pre-trigger-include üstbilgisinde belirtilir. Gelenlere, hiçbir sonrası tetikleyici x-ms-documentdb-post-trigger-include üst bilgi verilir. Her ikisi de öncesi ve sonrası tetikleyicileri, belirli bir istek için belirtilebilir.

## <a name="sample-code"></a>Örnek kod
Daha fazla sunucu tarafındaki kod örnekleri bulabilirsiniz (dahil olmak üzere [toplu silme](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), ve [güncelleştirme](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) içinde [GitHub deposu](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Harika, saklı yordam paylaşmak istiyorsunuz? depoya katkıda bulunmak ve bir çekme isteği oluşturun! 

## <a name="next-steps"></a>Sonraki adımlar
Bir veya daha fazla saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevleri oluşturulan aldıktan sonra bunları yüklemek ve Veri Gezgini'ni kullanarak Azure portalında görüntüleyebilirsiniz.

Ayrıca aşağıdaki başvurular ve kaynaklar yolunuzu Azure Cosmos dB sunucu tarafı programlama hakkında daha fazla bilgi edinmek için yararlı bulabilirsiniz:

* [Azure Cosmos DB SDK'ları](sql-api-sdk-dotnet.md)
* [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
* [JSON](http://www.json.org/) 
* [JavaScript ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [Güvenli ve taşınabilir veritabanı genişletilebilirliği](http://dl.acm.org/citation.cfm?id=276339) 
* [Hizmet yönelimli veritabanı mimarisi](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [Microsoft SQL Server'da .NET çalışma zamanı barındırma](http://dl.acm.org/citation.cfm?id=1007669)
