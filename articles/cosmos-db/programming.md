---
title: Sunucu tarafı JavaScript programlama Azure Cosmos DB için | Microsoft Docs
description: Saklı yordamlar, veritabanı tetikleyiciler ve kullanıcı tanımlı işlevler (UDF'ler) JavaScript'te yazmak için Azure Cosmos DB kullanmayı öğrenin. Veritabanı programing ipuçları ve daha fazla bilgi edinin.
keywords: Veritabanı tetikleyici, saklı yordam, saklı yordamı, veritabanı programı, sproc, azure, Microsoft azure
services: cosmos-db
documentationcenter: ''
author: aliuy
manager: kfile
ms.assetid: 0fba7ebd-a4fc-4253-a786-97f1354fbf17
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/26/2018
ms.author: andrl
ms.openlocfilehash: e6fd51cb2550549e14934c3f4774a40d42281247
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Azure Cosmos DB sunucu tarafı programlama: saklı yordamlar, veritabanı tetikleyiciler ve UDF'lerin

JavaScript Azure Cosmos veritabanı dil ile tümleşik, işlem yürütme yazma geliştiriciler nasıl sağladığını öğrenin **saklı yordamlar**, **Tetikleyicileri**, ve **kullanıcı tanımlı işlevler (UDF'ler)**  yerel olarak bir [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript. JavaScript tümleştirme sevk edilmiş ve veritabanı depolama bölümlere doğrudan içinde yürütülen program mantığı yazmanızı sağlar. 

Burada Barış Liu Azure Cosmos DB'ın sunucu tarafı veritabanı programlama modeli tanıtılmaktadır aşağıdaki videoyu izleyerek çalışmaya başlamanızı öneririz. 

> [!VIDEO https://www.youtube.com/embed/s0cXdHNlVI0]
>
> 

Ardından, bu makalede, aşağıdaki soruların yanıtlarını burada öğreneceksiniz döndürün:  

* Nasıl ı saklı yordam, tetikleyici veya JavaScript kullanarak UDF yazma?
* Cosmos DB ACID nasıl garanti?
* İşlemler Cosmos DB'de nasıl çalışır?
* Önceden tetikler ve sonrası tetikler nedir ve nasıl t bir yazma?
* Nasıl kaydetmek ve HTTP kullanarak bir RESTful şekilde bir saklı yordam, tetikleyici veya UDF yürütme?
* Depolanmış yordamlar, tetikleyiciler ve UDF'lerin ne Cosmos DB SDK'ları oluşturmak ve çalıştırmak kullanılabilir olan?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Saklı yordam ve UDF programlamaya giriş
Bu yaklaşım *"JavaScript T-SQL modern gün olarak"* tür sistem uyuşmazlıkları ve nesne ilişkisel eşleme teknolojileri karmaşıklığını uygulama geliştiricilerden boşaltır. Ayrıca, bir dizi zengin uygulamaları oluşturmak için kullanılan iç avantajları vardır:  

* **Yordam mantığı:** JavaScript üst düzey bir programlama dili olarak iş mantığı ifade etmek için zengin ve tanıdık bir arabirim sağlar. Karmaşık sıraları yakın veri işlemleri gerçekleştirebilir.
* **Atomik işlemleri:** tek bir saklı yordam veya tetikleyici içinde gerçekleştirilen işlemler veritabanı Cosmos DB garanti atomik. Bu atomik işlevselliği tek bir toplu ilgili işlemlerinde bunların tümünün başarılı ya da bunların hiçbiri başarılı birleştirmek bir uygulama sağlar. 
* **Performans:** iyileştirmeler arabellek havuzunda JSON belgelerinin yavaş materialization gibi bir dizi JSON Javascript dil türü sisteme doğası gereği eşlenen ve ayrıca temel depolama Cosmos DB'de birimidir olgu sağlar ve bunları, yürütülen kod kullanılabilir İsteğe bağlı hale getirme. Veritabanı dağıtımı iş mantığı ile ilgili daha fazla performans avantajı vardır:
  
  * Toplu işleme – Geliştiriciler grubu ekleme gibi işlemleri ve toplu olarak gönderin. Maliyet ağ trafiği gecikme süresi ve ayrı hareketleri oluşturmak için depolama yükünü önemli ölçüde azalır. 
  * Ön derleme – Cosmos DB saklı yordamlar, tetikleyiciler ve her çağırma için JavaScript derleme maliyetini önlemek için kullanıcı tanımlı işlevler (UDF'ler) işlemini gerçekleştirir. Yordam mantığı bayt kod oluşturmanın ek yükü için en az bir değer amortized.
  * Sıralama – birçok işlemleri gerek yan içeren büyük olasılıkla bir veya daha fazla ikincil depolama işlemleri yapılması etki ("tetikleyicisi"). Kararlılık yanı sıra, bu sunucuya taşındıklarında daha fazla kullanıcı olur. 
* **Kapsülleme:** saklı yordamlar, iş mantığı iki avantajları vardır tek bir yerde gruplandırmak için kullanılabilir:
  * Uygulamalarını bağımsız olarak verilerden gelişmesi veri mimarları etkinleştirir ham verileri en üstünde bir soyutlama katmanı ekler. Bu soyutlama katmanını veriler verilerle doğrudan dağıtılacak varsa uygulamasına baked gerekebilir kırılır varsayımlar nedeniyle şema küçüktür, olduğunda avantajlıdır.  
  * Bu soyutlama kuruluşların betikleri erişimden hızlandırma tarafından verilerine güvenli kalmasına izin verir.  

Oluşturma ve yürütme veritabanı tetikleyici, saklı yordamları ve özel sorgu işleçleri aracılığıyla desteklenir [Azure portal](https://portal.azure.com), [REST API](/rest/api/cosmos-db/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), ve [istemci SDK'ları](sql-api-sdk-dotnet.md) .NET, Node.js ve JavaScript gibi birçok platformda.

Bu öğretici kullanır [Node.js SDK'sı ile Q öneriler](http://azure.github.io/azure-documentdb-node-q/) sözdizimi ve saklı yordamlar, tetikleyiciler ve UDF'lerin kullanımını göstermek için.   

## <a name="stored-procedures"></a>Saklı yordamlar
### <a name="example-write-a-stored-procedure"></a>Örnek: bir saklı yordam yazma
"Hello World" yanıt veren basit bir saklı yordam ile başlayalım.

    var helloWorldStoredProc = {
        id: "helloWorld",
        serverScript: function () {
            var context = getContext();
            var response = context.getResponse();

            response.setBody("Hello, World");
        }
    }


Saklı yordamlar koleksiyonu başına kaydedilir ve herhangi bir belge ve ek koleksiyonda mevcut üzerinde çalışabilir. Aşağıdaki kod parçacığını bir koleksiyonla helloWorld saklı yordamın nasıl gösterir. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Saklı yordam kaydedildiğinde, koleksiyon karşı yürütün ve sonuçları istemcide geri okuyun. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Context nesnesi Cosmos DB depolama üzerinde gerçekleştirilen tüm işlemlerin erişimin yanı sıra, istek ve yanıt nesnelere erişim sağlar. Bu durumda, yanıt nesnesini istemciye gönderilen yanıtın gövdesini ayarlamak için kullanın. Daha fazla bilgi için bkz: [Azure Cosmos DB JavaScript server SDK Belgeleri](http://azure.github.io/azure-documentdb-js-server/).  

Bize göre bu örnekte genişletin ve veritabanı ile ilgili daha fazla işlevsellik için saklı yordam ekleyin. Saklı yordamlar oluşturmak, güncelleştirmek, okuyabilir, sorgu ve belgeler ve koleksiyon içindeki ekleri silin.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Örnek: bir belge oluşturmak için bir saklı yordam yazma
Sonraki kod parçacığını context nesnesi Cosmos DB kaynakları ile etkileşim kurmak için nasıl kullanılacağını gösterir.

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


Bu saklı yordam giriş documentToCreate, geçerli koleksiyonunda oluşturulmasına belgeye gövdesini alır. Tüm işlemleri zaman uyumsuzdur ve JavaScript işlevi geri aramalar üzerinde bağlıdır. Geri çağırma işlevi iki parametre, işlem başarısız durumda hata nesnesi için diğeri için oluşturulan nesnesi vardır. Geri çağırma içinde kullanıcıların özel durumu işlemek ya da bir hata durum. Bir geri çağırma değil sağlanır ve bir hata durumunda, Azure Cosmos DB çalışma zamanı bir hata oluşturur.   

İşlem başarısız olursa yukarıdaki örnekte, bir hata geri çağırma oluşturur. Aksi durumda, istemci yanıt gövdesi olarak oluşturulan belge Kimliğini ayarlar. İşte bu saklı yordam giriş parametreleriyle nasıl yürütülür.

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


Bu saklı yordam, belge gövdeleri bir dizi girişi olarak almak ve bunları tüm yerine bunların her birini ayrı ayrı oluşturmak için birden çok isteği aynı saklı yordam yürütme oluşturmak için değiştirilebilir. Bu saklı yordam Cosmos DB (Bu öğreticinin ilerleyen bölümlerinde açıklanmıştır) için verimli toplu içeri Aktarıcı uygulamak için kullanılabilir.   

Açıklanan örnek saklı yordamları kullanma gösterilmektedir. Sonraki öğreticide daha sonra tetikleyiciler ve kullanıcı tanımlı işlevler (UDF'ler) hakkında bilgi edineceksiniz.

## <a name="database-program-transactions"></a>Veritabanı program işlemleri
Tipik bir veritabanında işlem tek bir mantıksal birim iş olarak gerçekleştirilen işlemler dizisi olarak tanımlanabilir. Her işlem sağlar **ACID garanti**. ACID dört özellikleri - kararlılık, tutarlılık, yalıtım ve dayanıklılık anlamına gelir iyi bilinen bir kısaltma ' dir.  

Kısaca, bir işlem içinde tüm çalışmanın tek bir birim olarak davranılır kararlılık garanti burada ya da tamamını kaydedilmiş veya yok. Tutarlılık verilerin işlemleri arasında her zaman iyi bir iç durumda olduğundan emin olur. Yalıtım iki işlem birbiriyle – genellikle etkilemesine, çoğu ticari sistemleri kullanılabilir birden çok yalıtım düzeyi uygulama gereksinimlerine göre sağlayın güvence altına alır. Dayanıklılık veritabanında kaydedilen herhangi bir değişiklik her zaman mevcut olmasını sağlar.   

Cosmos DB'de JavaScript veritabanıyla aynı bellek alanı barındırılır. Bu nedenle, yapılan istekleri içinde saklı yordamları ve Tetikleyicileri veritabanı oturumu aynı kapsamda yürütün. Bu özellik, tek bir saklı yordam/tetikleyici parçası olan tüm işlemler için ACID güvence altına almak Cosmos DB sağlar. Aşağıdaki saklı yordamı tanımı göz önünde bulundurun:

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

Bu saklı yordam hareketleri ticari öğelere tek bir işlemde iki oyuncu arasında bir oyun uygulaması içinde kullanır. Saklı yordam bir bağımsız değişken olarak geçirilen her player kimlikleri karşılık gelen iki belge okumaya çalışır. Her iki player belge bulunamazsa, saklı yordamı öğelerin takas tarafından belgeleri güncelleştirir. Yol boyunca herhangi bir hatayla karşılaşılmazsa örtük olarak hareket durdurur bir JavaScript özel durumu oluşturur.

Saklı yordam koleksiyonu kaydedilmişse karşı tek bölümlü bir koleksiyon olduğu sonra işlem koleksiyonundaki tüm belgelerin kapsamlıdır. Ardından koleksiyon bölümlendiğinde ise, saklı yordamlar tek bölüm anahtarı işlem kapsamı içinde yürütülür. Her saklı yordam yürütme sonra işlemin altında çalıştırmalısınız kapsamına karşılık gelen bir bölüm anahtarı değerini içermelidir. Daha fazla bilgi için bkz: [Azure DB Cosmos bölümleme](partition-data.md).

### <a name="commit-and-rollback"></a>Kaydetme ve geri alma
İşlemleri iç ve yerel olarak Cosmos veritabanı JavaScript programlama modeline tümleşiktir. JavaScript işlevinin içinde tüm işlemleri otomatik olarak tek bir işlem altında sarılır. JavaScript bir özel durumla tamamlarsa, veritabanı işlemleri kabul edilir. İlişkisel veritabanları "BEGIN TRANSACTION" ve "COMMIT TRANSACTION" deyimlerinde Cosmos DB'de örtük etkindir.  

Komut dosyasını yayılır herhangi bir özel durum ise, Cosmos veritabanı JavaScript çalışma zamanı tüm işlem döndürülmesine neden olur. Önceki örnekte gösterildiği gibi bir özel durum atma etkili bir şekilde bir "geri alma işlemi" Cosmos DB'de eşdeğerdir.

### <a name="data-consistency"></a>Veri tutarlılığı
Saklı yordamları ve Tetikleyicileri her zaman birincil Çoğaltmada Azure Cosmos DB kapsayıcısının yürütülür. Bu, okuma içindeki yordamları teklif güçlü tutarlılık depolanan sağlar. Kullanıcı tanımlı işlevler kullanarak sorguları birincil veya ikincil bir çoğaltma üzerinde çalıştırılabilir, ancak uygun çoğaltma seçerek istenen tutarlılık düzeyi karşılamak üzere emin olun.

## <a name="bounded-execution"></a>Sınırlanmış yürütme
Belirtilen sunucu içinde tüm Cosmos DB işlemleri tamamlamalısınız isteği zaman aşımı süresi. Bu sınırlama JavaScript işlevleri (saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler) için de geçerlidir. Bir işlem bu zaman sınırı ile tamamlanmazsa, işlem geri alındı. JavaScript işlevleri süre sınırı içinde son veya toplu/sürdürmeden yürütme için devamlılık tabanlı modeli uygulamak gerekir.  

Saklı yordamları ve Tetikleyicileri süre sınırlarını, tüm işlevler (için oluşturma, okuma, değiştirin ve belgeler ve ekleri silme) koleksiyon nesnesi altında işlemek için geliştirmeyi kolaylaştırmak için return bir Boole değeri bu temsil olup olmadığını bu işlemi tamamlanır. Bu değeri false ise, bu zaman sınırı dolmak üzere olduğunu ve yordam yürütme sarmalamanız gerekir göstergesidir.  İlk kabul edilmeyen deposu işlemi sıraya alınan öncesinde Operations saklı yordamı zamanında tamamlandıktan ve başka istek sıraya değil tamamlamak için garanti edilir.  

JavaScript işlevleri de kaynak tüketimine ilişkin ilişkisindeki. Cosmos DB işleme koleksiyon başına veya bir dizi kapsayıcıları için ayırır. Üretilen iş CPU, bellek ve g/ç tüketim istek birimleri veya RUs adlı normalleştirilmiş bir birim cinsinden ifade edilir. JavaScript işlevleri potansiyel olarak çok sayıda RUs kısa bir süre içinde yukarı kullanabilirsiniz ve oranı koleksiyonunun sınırına ulaşıldığında sınırlı alabilirsiniz. Yoğun bir kaynak saklı yordamlar ilkel veritabanı işlemleri kullanılabilirliğini sağlamak için de karantinaya.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Örnek: toplu bir veritabanı programa veri alma
Aşağıda, belgeler bir koleksiyona toplu içeri için yazılmış bir saklı yordam örneğidir. Saklı yordam Boolean denetleyerek sınırlanmış yürütme nasıl işlediğini Not createDocument dönüş değeri ve izlemek ve toplu işlemler arasında ilerleme sürdürmek için saklı yordam her çalıştırılışı eklenen belge sayısını kullanır.

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

## <a id="trigger"></a> Veritabanı Tetikleyicileri
### <a name="database-pre-triggers"></a>Veritabanı öncesi Tetikleyicileri
Cosmos DB yürütülen ya da bir belge üzerinde bir işlemi tarafından tetiklenen Tetikleyiciler sağlar. Örneğin, ön tetikleyici belirtebilmeniz için bir belge oluşturma – bu ön tetikleyici belgeyi oluşturulmadan önce çalışacak olduğunda. Aşağıdaki örnek oluşturulmakta olan bir belgenin özelliklerini doğrulamak için ön Tetikleyicileri'nın nasıl kullanılabileceğini gösterir:

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


Ve karşılık gelen Node.js istemci tarafı kaydı kodu tetikleyici için:

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


Giriş parametreleri öncesi tetikleyici bulunamaz. İstek nesnesi, işlemle ilişkili İstek iletisini işlemek için kullanılabilir. Burada, ön tetikleyici belgeyi oluşturma ile çalıştırın ve JSON biçiminde oluşturulacak belge isteği ileti gövdesi içerir.   

Tetikleyiciler kayıtlı olduğunda, kullanıcılar ile çalıştırabilirsiniz operations belirtebilirsiniz. Bu tetikleyici, tetikleyici aşağıdaki kodda gösterildiği gibi bir değiştirme işlemi kullanılarak izin anlamına gelir TriggerOperation.Create ile oluşturuldu.

    var options = { preTriggerInclude: "validateDocumentContents" };

    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });

    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Veritabanı sonrası Tetikleyicileri
Ön tetikleyiciler gibi sonrası Tetikleyicileri bir belge üzerinde bir işlemi ile ilişkili ve giriş parametreleri gerçekleştirin yok. Çalışırlar **sonra** işlemi tamamlandıktan ve istemciye gönderilen yanıt iletisi erişebilir.   

Aşağıdaki örnek, eylemde sonrası Tetikleyicileri gösterir:

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


Tetikleyici, aşağıdaki örnekte gösterildiği gibi kaydedilebilir.

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


Bu tetikleyici için meta veri belgesi sorgular ve yeni oluşturulan belge hakkında ayrıntılarla güncelleştirir.  

Dikkat edilecek önemli bir şey **işlem** Cosmos DB Tetikleyicileri yürütülmesi. Aynı işlem özgün belgeye oluşturulmasını olarak bir parçası olarak bu sonrası tetikleyici çalışır. Bu nedenle, sonrası tetikleyici (meta veri belgesi güncelleştirme ise say) bir özel durum, tüm işlem başarısız olur ve geri alındı. Bir belge oluşturulur ve bir özel durum döndürdü.  

## <a id="udf"></a>Kullanıcı tanımlı işlevler
Kullanıcı tanımlı işlevler (UDF'ler), özel iş mantığı uygulamanız ve Azure Cosmos DB SQL Sorgu Dili Dilbilgisi genişletmek için kullanılır. Öğesinden yalnızca çağrılabilir sorguları içinde. Bunlar erişim kapsamı nesnesine sahip değil ve yalnızca işlem JavaScript kullanılması amaçlanmıştır. Bu nedenle, UDF'ler Cosmos DB hizmet ikincil çoğaltmalar üzerinde çalıştırılabilir.  

Aşağıdaki örnek gelir çeşitli gelir köşeli için hızlarını göre vergi hesaplamak için bir UDF oluşturur ve ardından birden fazla $20.000 vergiler Ücretli tüm kişilerin bulmak için bir sorgu içinde kullanır.

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


UDF daha sonra aşağıdaki örnekteki gibi sorgularında kullanılabilir:

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

## <a name="javascript-language-integrated-query-api"></a>JavaScript dil ile tümleşik sorgu API
Azure Cosmos veritabanı SQL dil bilgisinin kullanarak sorgu göndermeye ek olarak, sunucu tarafı SDK'sı SQL bilgisi olmadan fluent JavaScript arabirimi kullanarak en iyi duruma getirilmiş sorguları gerçekleştirmenizi sağlar. API chainable işlevdeki koşul işlevleri geçirerek sorguları programlı olarak oluşturmanıza olanak sağlayan JavaScript sorgu ECMAScript5'ın dizi öğelerin ve Lodash gibi popüler JavaScript kitaplıklarını tanıdık bir sözdizimi ile çağırır. Sorguları verimli bir şekilde Azure Cosmos veritabanı dizinlerini kullanarak çalıştırılacak JavaScript çalışma zamanı tarafından ayrıştırılır.

> [!NOTE]
> `__` (çift alt çizgi) olan bir diğer ad `getContext().getCollection()`.
> <br/>
> Diğer bir deyişle, kullanabileceğiniz `__` veya `getContext().getCollection()` JavaScript sorgu API erişmek için.
> 
> 

Desteklenen işlevler aşağıdakileri içerir:

<ul>
<li>
<b>... chain(). değeri ([geri çağırma] [, Seçenekleri])</b>
<ul>
<li>
Value()) ile bitmelidir zincirleme bir arama başlatır.
</li>
</ul>
</li>
<li>
<b>Filtre (predicateFunction [, Seçenekleri] [, geri çağırma])</b>
<ul>
<li>
Giriş belgeleri giriş/çıkış sonuç kümesine filtrelemek için true/false döndürür bir koşul işlevini kullanarak giriş filtreler. Bu işlev bir WHERE yan tümcesi SQL benzer şekilde davranır.
</li>
</ul>
</li>
<li>
<b>Harita (transformationFunction [, Seçenekleri] [, geri çağırma])</b>
<ul>
<li>
Her bir giriş öğesini bir JavaScript nesne veya değer eşleyen bir dönüşüm işlevi verilen bir yansıtma geçerlidir. Bu işlev bir SELECT yan tümcesinde SQL benzer şekilde davranır.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, Seçenekleri] [, geri çağırma])</b>
<ul>
<li>
Bu işlev bir kısayol tek bir özellik değeri her giriş öğesinden ayıklar eşlemesi için kullanılır.
</li>
</ul>
</li>
<li>
<b>düzleştirmek ([isShallow] [, Seçenekleri] [, geri çağırma])</b>
<ul>
<li>
Birleştirir ve tek bir dizi giriş her öğeden dizilerinin düzleştirir. Bu işlev LINQ SelectMany benzer şekilde davranır.
</li>
</ul>
</li>
<li>
<b>sortBy ([koşulu] [, Seçenekleri] [, geri çağırma])</b>
<ul>
<li>
Yeni belgeler birtakım verilen koşulu kullanarak artan giriş belgesi akış belgelerde sıralayarak üretir. Bu işlev bir ORDER BY yan tümcesi SQL benzer şekilde davranır.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([koşulu] [, Seçenekleri] [, geri çağırma])</b>
<ul>
<li>
Yeni belgeler birtakım verilen koşulu kullanarak azalan giriş belgesi akış belgelerde sıralayarak üretir. Bu işlev bir x DESC ORDER BY yan tümcesi SQL benzer şekilde davranır.
</li>
</ul>
</li>
</ul>


Koşul ve/veya Seçici işlevlerinin içine dahil edilirse, aşağıdaki JavaScript yapılarından otomatik olarak doğrudan Azure Cosmos DB dizinlerini çalıştırmak için en iyi duruma getirilmiş:

* Basit işleçleri: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Değişmez değer nesnesi de dahil olmak üzere hazır: {}
* dönüş var

Şu JavaScript yapıları Azure Cosmos DB dizinler için en iyi duruma getirilmiş değil:

* Denetim akışı (örneğin, varsa, sırada)
* İşlev çağrıları

Daha fazla bilgi için bkz: [sunucu tarafı JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Örnek: JavaScript sorgu API kullanarak bir saklı yordam yazma
Aşağıdaki kod örneği bağlamında bir saklı yordam, JavaScript sorgu API'sı nasıl kullanılabileceği bir örnektir. Saklı yordam bir giriş parametresi tarafından verilen bir belge ekler ve bir meta veri güncelleştirmeleri kullanarak belge `__.filter()` minSize, maxSize ve giriş belgenin boyut özelliği dayalı totalSize yöntemi.

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

## <a name="sql-to-javascript-cheat-sheet"></a>SQL Javascript kopya sayfası
Aşağıdaki tabloda, çeşitli SQL sorguları ve karşılık gelen JavaScript sorguları gösterir.

Özellik anahtarları içeren SQL sorguları, belge olarak (örneğin, `doc.id`) büyük küçük harfe duyarlıdır.

|SQL| JavaScript sorgu API|Aşağıdaki açıklama|
|---|---|---|
|SEÇİN *<br>Belgelerinden| __.map(function(doc) { <br>&nbsp;&nbsp;&nbsp;&nbsp;Belge dönüş;<br>});|1|
|SELECT docs.id, docs.message olarak msg, docs.actions <br>Belgelerinden|__.map(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;{Döndür<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Kimliği: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Msg: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|2|
|SEÇİN *<br>Belgelerinden<br>WHERE docs.id="X998_Y998"|__.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;doc.id iade === "X998_Y998";<br>});|3|
|SEÇİN *<br>Belgelerinden<br>WHERE ARRAY_CONTAINS (belgeleri. Etiketler, 123)|__.filter(function(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;x.Tags iade & & x.Tags.indexOf(123) > -1;<br>});|4|
|SELECT docs.id, docs.message msg olarak<br>Belgelerinden<br>WHERE docs.id="X998_Y998"|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;doc.id iade === "X998_Y998";<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.map(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{Döndür<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Kimliği: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Msg: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.Value();|5|
|DEĞER etiketi<br>Belgelerinden<br>Etiket IN belgeleri katılın. Etiketleri<br>ORDER BY docs._ts|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Belge döndür. Etiketleri & & Array.isArray (belge. Etiketleri);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;doc._ts döndürür;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Value()|6|

Aşağıdaki açıklamaları Yukarıdaki tablodaki her sorgu açıklanmaktadır.
1. Sonuç tüm belgelerde (devamlılık belirteci ile anlatır) olur.
2. Kimliği, ileti (diğer msg için) ve tüm belgeleri eylemden projeleri.
3. Koşul belgeler için sorgular: kimlik = "X998_Y998".
4. Etiketler özelliği ve etiketleri olan belgeler için sorgular 123 değerini içeren bir dizi olur.
5. Sorguları bir koşul ile belgeler için kimliği = "X998_Y998" ve ileti (diğer msg için) ve kimliği projeleri.
6. Etiketler, bir dizi özelliği olan belgeler için filtreleri ve elde edilen belgeler _ts zaman damgası sistem özelliği, ardından projeler tarafından sıralar + etiketler dizisi düzleştirir.


## <a name="runtime-support"></a>Çalışma zamanı desteği
Azure Cosmos DB [JavaScript sunucu tarafı API](http://azure.github.io/azure-documentdb-js-server/) tarafından standartlaştırılmış olarak temel JavaScript dil özelliklerinin çoğu için destek sağlayan [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Güvenlik
JavaScript saklı yordamları ve Tetikleyicileri korumalı, böylece tek bir betik etkilerini diğer veritabanı düzeyinde snapshot işlem yalıtım üzerinden geçmeden sızıntısı değil. Çalışma zamanı ortamları havuza alınmış ancak sonra her çalışma bağlamında temizlendi. Bu nedenle bunlar birbirinden herhangi istenmeyen yan etkileri güvenli olması garanti.

### <a name="pre-compilation"></a>Ön derleme
Saklı yordamlar, tetikleyiciler ve UDF'lerin her komut dosyası çağırma aynı anda derleme maliyet önlemek için bayt kodu biçimine örtük olarak önceden derlenmiş. Saklı yordamlar çalıştırılışı hızlı bir işlemdir ve az alan kaplaması sahip ön derleme sağlar.

## <a name="client-sdk-support"></a>İstemci SDK'sı desteği
Azure Cosmos DB yanı sıra [Node.js](sql-api-sdk-node.md) API, Azure Cosmos DB sahip [.NET](sql-api-sdk-dotnet.md), [.NET Core](sql-api-sdk-dotnet-core.md), [Java](sql-api-sdk-java.md), [JavaScript ](http://azure.github.io/azure-documentdb-js/), ve [Python SDK'ları](sql-api-sdk-python.md) SQL API'si de. Saklı yordamlar, tetikleyiciler ve UDF'lerin oluşturulabilir ve bu SDK de birini kullanarak çalıştırılabilir. Aşağıdaki örnekte, oluşturma ve .NET İstemcisi'ni kullanarak bir saklı yordam yürütme gösterilmektedir. .NET türleri nasıl JSON olarak saklı yordam içinde geçirilen ve geri okuma unutmayın.

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


Bu örnek nasıl kullanılacağını göstermektedir [SQL .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) için ön tetikleyici ve etkinleştirilmiş tetikleyici ile bir belge oluşturun. 

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


Ve aşağıdaki örnek, bir kullanıcı tanımlı işlev (UDF) oluşturmak ve bunu kullanmak gösterilmiştir bir [SQL sorgusu](sql-api-sql-query.md).

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

## <a name="rest-api"></a>REST API
Tüm Azure Cosmos DB işlemleri RESTful bir şekilde gerçekleştirilebilir. Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler altından bir HTTP POST kullanılarak kaydedilebilir. Aşağıdaki örnek, bir saklı yordam kaydetmek gösterilmektedir:

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


Saklı yordam oluşturmak için saklı yordam içeren gövde ile bir POST isteği URI dbs/testdb/colls/testColl/sprocs karşı yürüterek kayıtlı değil. Tetikleyiciler ve UDF'lerin benzer şekilde bir POST/tetikleyiciler ve /udfs karşı sırasıyla vererek kaydedilebilir.
Bu yordam can depolanan sonra kaynak bağlantısını karşı bir POST isteği göndererek yürütülebilir:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Burada, saklı yordam giriş istek gövdesinde geçirilir. Giriş, giriş parametresi bir JSON dizisi olarak geçirilir. Saklı yordam ilk girdi yanıt gövdesi bir belge olarak alır. Aldığınız yanıt aşağıdaki gibidir:

    HTTP/1.1 200 OK

    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Saklı yordamlar aksine Tetikleyicileri doğrudan yürütülemez. Bunun yerine, bir belge üzerinde bir işlemi bir parçası olarak yürütülür. Bir istek HTTP üstbilgilerini kullanma ile çalıştırmak için Tetikleyiciler belirtebilirsiniz. Aşağıdaki kod, bir belge oluşturma isteği gösterir.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger


    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Burada istekle çalıştırılması için ön tetikleyici x-ms-documentdb-pre-trigger-include üstbilgisinde belirtilir. Buna bağlı olarak, hiçbir sonrası tetikleyici x-ms-documentdb-post-trigger-include üst bilgi verilir. Her ikisi de öncesi ve sonrası tetikleyicileri, belirli bir istek için belirtilebilir.

## <a name="sample-code"></a>Örnek kod
Daha fazla sunucu tarafı kodu örnekleri bulabilirsiniz (de dahil olmak üzere [toplu silme](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), ve [güncelleştirme](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) içinde [GitHub deposunu](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Harika, saklı yordam paylaşmak ister misiniz? depoya katkıda bulunan ve bir çekme isteği oluşturun! 

## <a name="next-steps"></a>Sonraki adımlar
Bir veya daha fazla saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler oluşturulan olduktan sonra bunları yükleyin ve Veri Gezgini'ni kullanarak Azure portalında görüntüleyin.

Aynı zamanda aşağıdaki başvuru ve kaynaklar Azure Cosmos dB sunucu tarafı programlama hakkında daha fazla bilgi için yolundaki yararlı olabilir:

* [Azure Cosmos DB SDK'ları](sql-api-sdk-dotnet.md)
* [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
* [JSON](http://www.json.org/) 
* [JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [Güvenli ve taşınabilir veritabanı genişletilebilirliği](http://dl.acm.org/citation.cfm?id=276339) 
* [Hizmet odaklı veritabanı mimarisi](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [Microsoft SQL Server'da .NET çalışma zamanı barındırma](http://dl.acm.org/citation.cfm?id=1007669)

