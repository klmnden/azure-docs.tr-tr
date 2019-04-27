---
title: Azure SQL veritabanında JSON verileri ile çalışma | Microsoft Docs
description: Azure SQL veritabanı, ayrıştırma, sorgu ve JavaScript nesne gösterimi (JSON) gösterimi verileri biçimlendirme sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
manager: craigg
ms.date: 01/15/2019
ms.openlocfilehash: 77f6125980c43817230b8a8d4beb32757f23e6c2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60702964"
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Azure SQL veritabanı'nda JSON özelliklerini kullanmaya başlama
Azure SQL veritabanı sağlar ayrıştırma ve JavaScript nesne gösterimi ' gösterilen veri sorgulama [(JSON)](https://www.json.org/) biçimlendirmek ve JSON metni olarak, ilişkisel verilerinizi dışarı aktarın. Azure SQL veritabanı'nda aşağıdaki JSON senaryolarda kullanılabilir:
- [İlişkisel verileri JSON biçiminde biçimlendirme](#formatting-relational-data-in-json-format) kullanarak `FOR JSON` yan tümcesi.
- [JSON verileri ile çalışma](#working-with-json-data)
- [JSON verilerini sorgulama](#querying-json-data) JSON skaler işlevlerini kullanma.
- [Tablo biçiminde JSON dönüştürme](#transforming-json-into-tabular-format) kullanarak `OPENJSON` işlevi.

## <a name="formatting-relational-data-in-json-format"></a>İlişkisel verileri JSON biçiminde biçimlendirme
Veritabanındaki verileri JSON biçiminde bir yanıt sağlar ve katman alır biçimlendirmek veya istemci tarafı JavaScript çerçevesi veya verileri kabul kitaplıkları JSON olarak biçimlendirilmiş bir web hizmetiniz varsa, bir SQL sorgusunun doğrudan JSON olarak veritabanı içeriğinizi biçimlendirebilirsiniz. Artık sonuçlar JSON olarak Azure SQL veritabanı'ndan biçimleri uygulama kodları yazmak zorunda veya Tablo sorgusu sonuçlarını dönüştürmek ve ardından JSON biçimine nesneleri serileştirmek için bazı JSON seri hale getirme kitaplığı içerir. Bunun yerine, SQL sorgu sonuçları Azure SQL veritabanı'nda JSON olarak biçimlendirmek ve doğrudan uygulamanızda kullanmak için FOR JSON yan tümcesini kullanabilirsiniz.

Aşağıdaki örnekte, Sales.Customer tablodaki FOR JSON yan tümcesi kullanılarak JSON olarak biçimlendirilir:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

JSON yolu yan tümcesi sorgusunun sonuçları JSON metin olarak biçimlendirir. Hücre değerlerini JSON değer olarak oluşturulan sütun adları anahtarlar olarak kullanılabilir:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Sonuç kümesini burada her satır ayrı bir JSON nesnesi olarak biçimlendirilmiş bir JSON dizisi olarak biçimlendirilir.

Sütun diğer adları noktalı gösterim kullanılarak JSON sonuç çıkış biçimini özelleştirebilirsiniz YOLUNU gösterir. Aşağıdaki sorguyu "CustomerName" anahtarı çıkış JSON biçiminde adını değiştirir ve telefon ve Faks numaraları "Kişi" alt nesne getirir:

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Bu sorgu çıktısı şuna benzer:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

Bu örnekte biz dizi yerine tek bir JSON nesnesi belirterek döndürülen [wıthout_array_wrapper](https://msdn.microsoft.com/library/mt631354.aspx) seçeneği. Sorgu sonucu olarak tek bir nesne döndürme biliyorsanız, bu seçeneği kullanabilirsiniz.

Ana FOR JSON yan tümcesi, iç içe geçmiş JSON nesneleri veya diziler biçimlendirilmiş veritabanınızdan karmaşık hiyerarşik veri döndürmeyen sağlar değeridir. Aşağıdaki örnek, satırları içerecek şekilde gösterilmektedir `Orders` ait tablo `Customer` iç içe bir dizi olarak `Orders`:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Müşteri verilerini almak için sorguları ayırın ve ardından ilgili Siparişler listesi getirmek için gerekli tüm verilerin tek bir sorgu ile alabilirsiniz aşağıdaki örnek çıktıda gösterildiği gibi göndermek yerine:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>JSON verileriyle çalışma
Karmaşık alt nesneler, diziler veya hiyerarşik veriler varsa, kesin olarak yapılandırılmış veriler, yoksa veya kendi veri yapılarını zamanla gelişmesinin, JSON biçimi, karmaşık veri yapıları temsil etmek için yardımcı olabilir.

JSON gibi herhangi bir dize türü Azure SQL veritabanı'nda kullanılabilir değerinin metinsel bir biçimidir. Bir standart NVARCHAR JSON verilerini depolamak ya da gönderin:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

Bu örnekte kullanılan JSON verilerini NVARCHAR(MAX) türü kullanarak temsil edilir. JSON, bu tabloya eklenen veya saklı yordamın aşağıdaki örnekte gösterildiği gibi standart Transact-SQL söz dizimini kullanarak bir bağımsız değişken olarak sağlanan:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Ayrıca, herhangi bir istemci-tarafı dil veya dize verileri Azure SQL veritabanı'nda çalışır kitaplığı JSON verileri ile çalışır. JSON bellek için iyileştirilmiş bir tablo veya bir sistem sürümü tutulan tablo gibi NVARCHAR türünü destekleyen herhangi bir tabloda depolanır. JSON, istemci tarafı kod veya veritabanı katmanındaki herhangi bir kısıtlama sunmaz.

## <a name="querying-json-data"></a>JSON verileri Sorgulama
Azure SQL tablolarında depolanan JSON olarak biçimlendirilmiş veri varsa, JSON işlevleri içinde herhangi bir SQL sorgu bu verileri kullanmanıza izin verir.

Azure SQL veritabanı sağlar içinde kullanılabilen JSON işlevlerin herhangi bir SQL veri türü olarak JSON olarak biçimlendirilmiş verileri kabul eder. Kolayca, JSON metin değerlerini ayıklamak ve JSON verilerini herhangi bir sorgu kullanın:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

JSON_VALUE işlevi veri sütunda depolanan JSON metin değeri ayıklar. Bu işlev, bir değeri ayıklamak için JSON metnini başvuru için bir JavaScript benzeri yolunu kullanır. Ayıklanan değerin herhangi bir SQL sorgusunun parçası kullanılabilir.

JSON_QUERY işlevi için JSON_VALUE benzerdir. JSON_VALUE, bu işlev, karmaşık bir alt nesne dizileri ya da JSON metninde yerleştirilen nesneleri gibi ayıklar.

Json_modıfy işlevi değeri yolunu güncelleştirilmelidir JSON metnini, yanı sıra eskisinin üzerine yazılacak yeni bir değer belirtmenizi sağlar. Bu şekilde JSON metnini yapının tamamını reparsing olmadan kolayca güncelleştirebilirsiniz.

JSON standart bir metin olarak saklandığı metin sütunlarda depolanan değerleri doğru şekilde biçimlendirildiğini tutarlılık garantisi yoktur. Standart Azure SQL veritabanı Denetim kısıtlamalarını ve ISJSON işlevi kullanarak JSON sütunda depolanan metin düzgün biçimlendirildiğinden doğrulayabilirsiniz:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Giriş metni doğru biçimlendirilmiş JSON, ISJSON işlevi, 1 değerini döndürür. Her ekleme veya güncelleştirme JSON sütununun, bu kısıtlama yeni metin değeri hatalı biçimlendirilmiş JSON olmadığından emin olun.

## <a name="transforming-json-into-tabular-format"></a>JSON tablo biçimine dönüştürme
Azure SQL veritabanı ayrıca JSON koleksiyonlara tablo biçimi ve yükleme veya sorgu JSON veri dönüştürme sağlar.

OPENJSON JSON metnini ayrıştırır, bir JSON nesne dizisi bulur, dizinin öğeleri yinelenir ve çıkış sonuç dizideki her öğe için bir satır döndürür. Tablo değerli bir işlevdir.

![JSON tablo](./media/sql-database-json-features/image_2.png)

Yukarıdaki örnekte, biz nereye yerleştireceğinize açılması gerekir (içinde $. JSON dizisi belirtebilirsiniz Siparişler yol), hangi sütunların sonuç ve hücreler olarak döndürülecek JSON değerlerinin nerede bulacağını olarak döndürülmelidir.

Biz de bir JSON dizisi dönüştürebilirsiniz @orders satır, bir dizi değişkenine bu sonuç kümesi çözümleyebilir veya standart tabloya satır ekleme:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    FROM OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
Sipariş koleksiyonunu bir JSON dizisi olarak biçimlendirilmiş ve saklı yordam parametresi ayrıştırılır ve siparişler tabloya eklenen gibi sağlanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar
JSON uygulamanızla tümleştirme öğrenmek için şu kaynaklara bakın:

* [TechNet blogu](https://blogs.technet.microsoft.com/dataplatforminsider/20../../json-in-sql-server-2016-part-1-of-4/)
* [MSDN belgeleri](https://msdn.microsoft.com/library/dn921897.aspx)
* [Kanal 9 videosu](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

JSON'ı uygulamanıza tümleştirmek için çeşitli senaryoları hakkında bilgi edinmek için bu demolar [kanal 9 videosu](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) veya kullanım Örneğinize uyan bir senaryo Bul [JSON Blog yazılarını](https://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).

