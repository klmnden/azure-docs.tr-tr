---
title: Azure Cosmos DB'de saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler yazma
description: Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler, Azure Cosmos DB'de tanımlamayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 12/11/2018
ms.author: mjbrown
ms.openlocfilehash: c94509fb39d1c5ebb9aec1acfe1cbacc9cd6fd4a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59268437"
---
# <a name="how-to-write-stored-procedures-triggers-and-user-defined-functions-in-azure-cosmos-db"></a>Azure Cosmos DB'de saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler yazma

Azure Cosmos DB sağlar yazmanızı sağlayan bir JavaScript dil ile tümleşik, işlem yürütme **saklı yordamlar**, **Tetikleyicileri**, ve **kullanıcı tanımlı işlevler (UDF'ler)**. Azure Cosmos DB SQL API'sini kullanarak, saklı yordamlar, tetikleyiciler ve UDF'ler JavaScript dilinde tanımlayabilirsiniz. JavaScript'te, mantığınızı yazmanıza ve veritabanı altyapısının içinden yürütün. Oluşturabilir ve tetikleyicileri, saklı yordamlar ve UDF'ler kullanarak yürütme [Azure portalında](https://portal.azure.com/), [JavaScript dil tümleşik Azure Cosmos DB'de sorgu API'si](javascript-query-api.md) ve [Cosmos DB SQL API istemcisi SDK'ları](sql-api-dotnet-samples.md). 

Bir saklı yordam, tetikleyici ve kullanıcı tanımlı işlevi çağırmak için onu kaydetmeniz gerekir. Daha fazla bilgi için [saklı yordamlar, Tetikleyiciler, Azure Cosmos DB'de kullanıcı tanımlı işlevleri ile çalışmaya nasıl](how-to-use-stored-procedures-triggers-udfs.md).

> [!NOTE]
> Bir saklı yordamı yürütülürken bölümlenmiş kapsayıcılar için bir bölüm anahtarı değeri istek seçenekleri sağlanmalıdır. Saklı yordamlar, her zaman bir bölüm anahtarı için kapsama eklenir. Farklı bir bölüm anahtarı değerine sahip öğeleri saklı yordam için görünür olmaz. Bu da Tetikleyiciler için de geçerli.

## <a id="stored-procedures"></a>Saklı yordamlar yazma

Saklı yordamlar, JavaScript kullanarak yazılır, bunlar oluşturabilir, güncelleştirme, okuma, sorgulama ve bir Azure Cosmos kapsayıcı içindeki öğeleri silin. Saklı yordamlar, koleksiyon kaydedilir ve herhangi bir belge veya koleksiyonda mevcut bir ek çalışabilir.

**Örnek**

"Hello World" yanıt döndüren basit bir saklı yordam aşağıda verilmiştir.

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

Bağlam nesnesi, Azure Cosmos DB içinde gerçekleştirilen tüm işlemler erişimin yanı sıra, istek ve yanıt nesnelere erişimi sağlar. Bu durumda, istemciye geri gönderilecek yanıt gövdesinin ayarlamak için yanıt nesnesini kullanın.

Yazıldıktan sonra saklı yordamı bir koleksiyon ile kayıtlı olması gerekir. Daha fazla bilgi için bkz. [Azure Cosmos DB'de saklı yordamlar kullanma](how-to-use-stored-procedures-triggers-udfs.md#stored-procedures) makalesi.

### <a id="create-an-item"></a>Saklı yordamı kullanarak bir öğe oluşturun

Saklı yordamı kullanarak bir öğe oluşturduğunuzda, Azure Cosmos DB kapsayıcısı ve yeni oluşturulan öğeyi döndürdüğü için bir kimlik öğesi eklenir. Öğe oluşturmaya, zaman uyumsuz bir işlemdir ve JavaScript geri çağırma işlevlere bağlıdır. Geri çağırma işlevi iki parametre - bir işlemin başarısız olması durumunda hata nesnesi ve bir dönüş değeri için başka; yine de sahip istiyor musunuz? Bu durumda, oluşturulan nesnesi. Geri çağırma içinde özel durumu işlemek veya bir hata oluşturur. Azure Cosmos DB çalışma zamanı, bir geri çağırma sağlanmadı ve bir hata durumunda, bir hata atar. 

Saklı yordamı da açıklama ayarlamak için bir parametre içerir, bir Boole değeri. Parametre ayarlandığında true ve tanımı eksik, saklı yordamı bir özel durum oluşturur. Aksi takdirde, saklı yordamın geri kalanını çalışmaya devam eder.

Aşağıdaki örnek depolanan yordamı, yeni bir Azure Cosmos DB öğe girdi olarak alır, Azure Cosmos DB kapsayıcıya ekler ve yeni oluşturulan öğesinin kimliğini döndürür. Bu örnekte biz ToDoList örnekten yararlanıyor [.NET SQL API'si hızlı başlangıç](create-sql-api-dotnet.md)

```javascript
function createToDoItem(itemToCreate) {

    var context = getContext();
    var container = context.getCollection();

    var accepted = container.createDocument(container.getSelfLink(),
        itemToCreate,
        function (err, itemCreated) {
            if (err) throw new Error('Error' + err.message);
            context.getResponse().setBody(itemCreated.id)
        });
    if (!accepted) return;
}
```

### <a name="arrays-as-input-parameters-for-stored-procedures"></a>Saklı yordamlar için giriş parametrelerini olarak diziler 

Azure portalında bir saklı yordam tanımlarken, giriş parametreleri için saklı yordam her zaman bir dize olarak gönderilir. Dizelerden oluşan bir dizi girdi olarak geçirdiğiniz olsa bile, dizi dizeye dönüştürülür ve saklı yordamı gönderilir. Bu sorunu çözmek için bir işlev bir dize dizisi olarak ayrıştırılacak, saklı yordam içinde tanımlayabilirsiniz. Aşağıdaki kod, bir dize giriş parametresi olarak bir dizi ayrıştırılacak gösterilmektedir:

```javascript
function sample(arr) {
    if (typeof arr === "string") arr = JSON.parse(arr);

    arr.forEach(function(a) {
        // do something here
        console.log(a);
    });
}
```

### <a id="transactions"></a>Saklı yordamlar işlemleri

Bir saklı yordamı kullanarak bir kapsayıcı içindeki öğeleri üzerinde işlem uygulayabilirsiniz. Aşağıdaki örnek, fantezi futbol oyun uygulaması tek bir işlemde iki ekipler arasında ticari oyuncu için işlemleri kullanır. Saklı yordam bağımsız değişken olarak geçirilen player kimlikleri için karşılık gelen her iki Azure Cosmos DB öğeleri okuma girişiminde bulunur. Her iki oyuncuların bulunamazsa, saklı yordam ekiplerinin değiştirerek öğeleri güncelleştirir. Saklı yordam, süreç boyunca herhangi bir hatayla karşılaşılmazsa, örtük olarak işlem durdurur bir JavaScript özel durumu oluşturur.

```javascript
// JavaScript source code
function tradePlayers(playerId1, playerId2) {
    var context = getContext();
    var container = context.getCollection();
    var response = context.getResponse();

    var player1Document, player2Document;

    // query for players
    var filterQuery = 
    {     
        'query' : 'SELECT * FROM Players p where p.id = @playerId1',
        'parameters' : [{'name':'@playerId1', 'value':playerId1}] 
    };
            
    var accept = container.queryDocuments(container.getSelfLink(), filterQuery, {},
        function (err, items, responseOptions) {
            if (err) throw new Error("Error" + err.message);

            if (items.length != 1) throw "Unable to find both names";
            player1Item = items[0];

            var filterQuery2 = 
            {     
                'query' : 'SELECT * FROM Players p where p.id = @playerId2',
                'parameters' : [{'name':'@playerId2', 'value':playerId2}] 
            };
            var accept2 = container.queryDocuments(container.getSelfLink(), filterQuery2, {},
                function (err2, items2, responseOptions2) {
                    if (err2) throw new Error("Error" + err2.message);
                    if (items2.length != 1) throw "Unable to find both names";
                    player2Item = items2[0];
                    swapTeams(player1Item, player2Item);
                    return;
                });
            if (!accept2) throw "Unable to read player details, abort ";
        });

    if (!accept) throw "Unable to read player details, abort ";

    // swap the two players’ teams
    function swapTeams(player1, player2) {
        var player2NewTeam = player1.team;
        player1.team = player2.team;
        player2.team = player2NewTeam;

        var accept = container.replaceDocument(player1._self, player1,
            function (err, itemReplaced) {
                if (err) throw "Unable to update player 1, abort ";

                var accept2 = container.replaceDocument(player2._self, player2,
                    function (err2, itemReplaced2) {
                        if (err) throw "Unable to update player 2, abort"
                    });

                if (!accept2) throw "Unable to update player 2, abort";
            });

        if (!accept) throw "Unable to update player 1, abort";
    }
}
```

### <a id="bounded-execution"></a>Saklı yordam içinde sınırlanmış yürütme

Toplu-öğeleri bir Azure Cosmos kapsayıcıya almalar bir saklı yordam örneği verilmiştir. Saklı yordam sınırlanmış yürütme Boole dönüş değerini kontrol ederek işleme `createDocument`ve ardından izlemek ve arasında toplu ilerleme durumunu sürdürmek için saklı yordamın her çağrı eklenen öğe sayısını kullanır.

```javascript
function bulkImport(items) {
    var container = getContext().getCollection();
    var containerLink = container.getSelfLink();

    // The count of imported items, also used as current item index.
    var count = 0;

    // Validate input.
    if (!items) throw new Error("The array is undefined or null.");

    var itemsLength = items.length;
    if (itemsLength == 0) {
        getContext().getResponse().setBody(0);
    }

    // Call the create API to create an item.
    tryCreate(items[count], callback);

    // Note that there are 2 exit conditions:
    // 1) The createDocument request was not accepted.
    //    In this case the callback will not be called, we just call setBody and we are done.
    // 2) The callback was called items.length times.
    //    In this case all items were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
    function tryCreate(item, callback) {
        var isAccepted = container.createDocument(containerLink, item, callback);

        // If the request was accepted, callback will be called.
        // Otherwise report current count back to the client,
        // which will call the script again with remaining set of items.
        if (!isAccepted) getContext().getResponse().setBody(count);
    }

    // This is called when container.createDocument is done in order to process the result.
    function callback(err, item, options) {
        if (err) throw err;

        // One more item has been inserted, increment the count.
        count++;

        if (count >= itemsLength) {
            // If we created all items, we are done. Just set the response.
            getContext().getResponse().setBody(count);
        } else {
            // Create next document.
            tryCreate(items[count], callback);
        }
    }
}
```

## <a id="triggers"></a>Tetikleyiciler yazma

Azure Cosmos DB Tetikleyicileri öncesi ve sonrası Tetikleyicileri destekler. Öncesi tetikleyiciler, bir veritabanı öğesini değiştirmeden önce yürütülür ve sonrası Tetikleyicileri veritabanı öğesi değiştirdikten sonra yürütülür.

### <a id="pre-triggers"></a>Öncesi Tetikleyicileri

Aşağıdaki örnek, bir ön tetikleyici oluşturulmakta olan bir Azure Cosmos DB öğesinin özelliklerini doğrulamak için nasıl kullanıldığını gösterir. Bu örnekte biz ToDoList örnekten yararlanıyor [.NET SQL API'si Hızlı Başlangıç](create-sql-api-dotnet.md), bir tane içermiyorsa, bir zaman damgası özelliği için yeni eklenen bir öğe eklemek için.

```javascript
function validateToDoItemTimestamp() {
    var context = getContext();
    var request = context.getRequest();

    // item to be created in the current operation
    var itemToCreate = request.getBody();

    // validate properties
    if (!("timestamp" in itemToCreate)) {
        var ts = new Date();
        itemToCreate["timestamp"] = ts.getTime();
    }

    // update the item that will be created
    request.setBody(itemToCreate);
}
```

Öncesi tetikleyici, giriş parametreleri bulunamaz. İstek nesnesi tetikleyicisinin işlemle ilişkilendirilen istek iletisi işlemek için kullanılır. Önceki örnekte, bir Azure Cosmos DB öğesi oluşturulurken öncesi tetikleyici çalıştırılır ve istek iletisi gövdesi JSON biçiminde oluşturulacak öğesi içeriyor.

Tetikleyiciler kaydettiğinizde, birlikte çalışabilen işlemleri belirtebilirsiniz. Bu tetikleyici ile oluşturulması gereken bir `TriggerOperation` değerini `TriggerOperation.Create`, tetikleyici aşağıdaki kodda gösterildiği gibi bir değiştirme işlemi kullanılarak verilmez anlamına gelir.

Kaydolun ve öncesi bir tetikleyici çağırmak nasıl bir örnekleri için bkz: [öncesi Tetikleyicileri](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) ve [sonrası Tetikleyicileri](how-to-use-stored-procedures-triggers-udfs.md#post-triggers) makaleler. 

### <a id="post-triggers"></a>Sonrası Tetikleyicileri

Aşağıdaki örnek, bir sonrası tetikleyici gösterir. Bu tetikleyici, meta veri öğesi için sorgular ve yeni oluşturulan öğeyi ilişkin ayrıntılarla güncelleştirir.


```javascript
function updateMetadata() {
var context = getContext();
var container = context.getCollection();
var response = context.getResponse();

// item that was created
var createdItem = response.getBody();

// query for metadata document
var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
var accept = container.queryDocuments(container.getSelfLink(), filterQuery,
    updateMetadataCallback);
if(!accept) throw "Unable to update metadata, abort";

function updateMetadataCallback(err, items, responseOptions) {
    if(err) throw new Error("Error" + err.message);
        if(items.length != 1) throw 'Unable to find metadata document';

        var metadataItem = items[0];

        // update metadata
        metadataItem.createdItems += 1;
        metadataItem.createdNames += " " + createdItem.id;
        var accept = container.replaceDocument(metadataItem._self,
            metadataItem, function(err, itemReplaced) {
                    if(err) throw "Unable to update metadata, abort";
            });
        if(!accept) throw "Unable to update metadata, abort";
        return;
}
```

Dikkat edilecek önemli olan bir Azure Cosmos DB Tetikleyicileri işlem tabanlı olarak yürütülmesi şeydir. Temel alınan öğe için aynı işlemin bir parçası olarak sonrası tetikleyici çalıştırır. Tüm işlem sonrası tetikleyici yürütme sırasında bir özel durumla başarısız olur. Herhangi bir şey kabul edilen geri alınacak ve bir özel durum döndürdü.

Kaydolun ve öncesi bir tetikleyici çağırmak nasıl bir örnekleri için bkz: [öncesi Tetikleyicileri](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) ve [sonrası Tetikleyicileri](how-to-use-stored-procedures-triggers-udfs.md#post-triggers) makaleler. 

## <a id="udfs"></a>Kullanıcı tanımlı işlevler yazma

Aşağıdaki örnek, çeşitli gelir köşeli ayraçlar için gelir vergi hesaplamak için bir UDF oluşturur. Bu kullanıcı tanımlı işlev sonra bir sorgu içinde kullanılabilir. Bu örneğin amacı doğrultusunda, "Yüksek" gibi özelliklerle adlı bir kapsayıcı olduğu varsayılır:

```json
{
   "name": "Satya Nadella",
   "country": "USA",
   "income": 70000
}
```

Çeşitli gelir köşeli ayraçlar için gelir vergi hesaplamak için işlev tanımı aşağıda verilmiştir:

```javascript
function tax(income) {

        if(income == undefined)
            throw 'no input';

        if (income < 1000)
            return income * 0.1;
        else if (income < 10000)
            return income * 0.2;
        else
            return income * 0.4;
    }
```

Kaydolun ve bir kullanıcı tanımlı işlev kullanma örnekleri için bkz. [Azure Cosmos DB'de kullanıcı tanımlı işlevler kullanma](how-to-use-stored-procedures-triggers-udfs.md#udfs) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

Kavramları ve nasıl yapılır yazma daha fazla bilgi edinin veya saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler, Azure Cosmos DB'de kullanın:

* [Kaydetme ve kullanma hakkında yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevleri Azure Cosmos DB'de depolanan](how-to-use-stored-procedures-triggers-udfs.md)

* [Saklı yordamları ve Tetikleyicileri Javascript sorgu API'si kullanarak Azure Cosmos DB'de yazma](how-to-write-javascript-query-api.md)

* [Yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevleri Azure Cosmos DB'de depolanan Azure Cosmos DB ile çalışma](stored-procedures-triggers-udfs.md)

* [Azure Cosmos DB'de sorgu API'si çalışma JavaScript dil ile tümleşik](javascript-query-api.md)
