---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: ff7ba04271c150018f2c55b62e40542a686608cf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188881"
---
## <a name="create-client"></a>İstemci bağlantısı oluşturma
Bir `WindowsAzure.MobileServiceClient` nesnesi oluşturarak istemci bağlantısı oluşturun.  `appUrl` ifadesini Mobile Uygulamanızın URL’si ile değiştirin.

```javascript
var client = WindowsAzure.MobileServiceClient(appUrl);
```

## <a name="table-reference"></a>Tablolarla çalışma
Verilere erişmek veya verileri güncelleştirmek için arka uç tablosuna başvuru oluşturun. `tableName` ifadesini tablonuzun adıyla değiştirin

```javascript
var table = client.getTable(tableName);
```

Bir tablo başvurusu oluşturduktan sonra tablonuzla başka işlemler yapabilirsiniz:

* [Tablo sorgulama](#querying)
  * [Verileri filtreleme](#table-filter)
  * [Verileri sayfalama](#table-paging)
  * [Verileri sıralama](#sorting-data)
* [Veri ekleme](#inserting)
* [Verileri değiştirme](#modifying)
* [Veri silme](#deleting)

### <a name="querying"></a>Nasıl Yapılır: Bir tablo başvurusu sorgulama
Bir tablo başvurusu oluşturduktan sonra sunucudaki verileri sorgulamak için kullanabilirsiniz.  Sorgular "LINQ benzeri" bir dilde yapılır.
Tablodan tüm verileri döndürmek için aşağıdaki kodu kullanın:

```javascript
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Sonuçlarla birlikte başarı işlevi çağrılır.  Başarı işlevinde `for (var i in results)` öğesini kullanmayın, aksi takdirde diğer sorgu işlevleri (`.includeTotalCount()` gibi) kullanıldığında sonuçlara eklenen bilgilerin üzerine yinelenir.

Sorgu söz dizimi hakkında daha fazla bilgi için [Query nesnesi belgelerine] bakın.

#### <a name="table-filter"></a>Sunucu üzerindeki verileri filtreleme
Tablo başvurusunda bir `where` yan tümcesi kullanabilirsiniz:

```javascript
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Ayrıca, nesneyi filtreleyen bir işlev de kullanabilirsiniz.  Bu durumda, `this` değişkeni filtre uygulanan geçerli nesneye atanır.  Aşağıdaki kod önceki örnek ile işlevsel olarak eşdeğerdir:

```javascript
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

#### <a name="table-paging"></a>Verileri sayfalama
`take()` ve `skip()` yöntemlerini kullanın.  Örneğin, tabloyu 100 satırlı kayıtlara bölmek istiyorsanız:

```javascript
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

Sonuç nesnesine bir totalCount alanı eklemek için `.includeTotalCount()` yöntemi kullanılır.  Sayfalama kullanılmazsa döndürülecek toplam kayıt sayısı TotalCount alanına doldurulur.

Bir sayfa listesi sağlamak için pages değişkenini ve bazı kullanıcı arabirimi düğmelerini kullanabilirsiniz; her sayfanın yeni kayıtlarını yüklemek için `loadPage()` seçeneğini kullanın.  Daha önce yüklenmiş kayıtlara hızlı erişim için önbelleğe almayı uygulayın.

#### <a name="sorting-data"></a>Nasıl Yapılır: Sıralanmış verileri döndürür
`.orderBy()` veya `.orderByDescending()` sorgu yöntemlerini kullanın:

```javascript
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Query nesnesi hakkında daha fazla bilgi için [Query nesnesi belgelerine] bakın.

### <a name="inserting"></a>Nasıl Yapılır: Veri ekleme
Uygun tarihle bir JavaScript nesnesi oluşturun ve `table.insert()` öğesini zaman uyumsuz olarak çağırın:

```javascript
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Ekleme başarılı olduğunda, eklenen öğe eşitleme işlemleri için gereken diğer alanlarla birlikte döndürülür.  Sonraki güncelleştirmeler için önbelleğinizi bu bilgilerle güncelleştirin.

Azure Mobile Apps Node.js Sunucu SDK’sı, geliştirme için dinamik şemayı destekler.  Dinamik Şema, bir insert veya update işleminde sütun belirterek tabloya sütun eklemenize olanak tanır.  Uygulamanızı üretime taşımadan önce dinamik şemanın kapatılması önerilir.

### <a name="modifying"></a>Nasıl Yapılır: Verileri değiştirme
`.insert()` yöntemine benzer şekilde, bir Update nesnesi oluşturup `.update()` öğesini çağırmanız gerekir.  Update nesnesi, güncelleştirilecek kaydın kimliğini içermelidir; bu kimlik, kayıt okunurken veya `.insert()` çağrılırken elde edilir.

```javascript
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

### <a name="deleting"></a>Nasıl Yapılır: Verileri silme
Bir kaydı silmek için `.del()` yöntemini çağırın.  Kimliği bir nesne başvurusuna geçirin:

```javascript
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
