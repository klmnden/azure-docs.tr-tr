---
title: "Azure SQL veritabanı JSON özellikleri | Microsoft Docs"
description: "Azure SQL veritabanı ayrıştırma, sorgu ve JavaScript nesne gösterimi (JSON) gösteriminde biçim verileri sağlar."
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.custom: develop databases
ms.date: 11/15/2016
ms.author: jovanpop
ms.topic: article
ms.openlocfilehash: 353a1cc99f122c773f0a005a20f04391236ad913
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Azure SQL veritabanında JSON özellikleri ile çalışmaya başlama
Ayrıştırma ve sorgu JavaScript nesne gösterimi gösterilen veriler azure SQL veritabanı sağlar [(JSON)](http://www.json.org/) biçimlendirmek ve ilişkisel verilerinizi JSON metni olarak verin.

JSON, modern web ve mobil uygulamalarda veri değişimi için kullanılan bir popüler veri biçimidir. JSON, günlük dosyalarında veya NoSQL veritabanları gibi yarı yapılandırılmış verileri depolamak için de kullanılır [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/). Dönüş sonuçları JSON metin olarak biçimlendirilmiş veya verileri kabul birçok REST web hizmeti JSON olarak biçimlendirilmiş. Çoğu Azure Hizmetleri gibi [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), ve [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) dönün veya JSON tüketen REST uç noktalar vardır.

Azure SQL veritabanı, JSON verileriyle kolayca çalışmanızı ve veritabanınızı modern Hizmetleri ile tümleştirme sağlar.

## <a name="overview"></a>Genel Bakış
Azure SQL veritabanı, JSON verilerle çalışmak için aşağıdaki işlevleri sağlar:

![JSON işlevleri](./media/sql-database-json-features/image_1.png)

JSON metni varsa, verileri JSON öğesinden çıkarın veya yerleşik işlevlerini kullanarak JSON doğru şekilde yapılandırıldığını doğrulayın [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), ve [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). [Json_modıfy](https://msdn.microsoft.com/library/dn921892.aspx) işlevi JSON metni içindeki değeri güncelleştirmenize olanak sağlar. Daha gelişmiş sorgulama ve analiz, için [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) işlevi bir dizi satır JSON nesnelerinin bir dizisi dönüştürme. Herhangi bir SQL sorgu döndürülen sonuç kümesi üzerinde çalıştırılabilir. Son olarak, var olan bir [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) olanak sağlayan yan tümcesi biçimlendirmek ilişkisel tabloları JSON metni olarak depolanan veriler.

## <a name="formatting-relational-data-in-json-format"></a>JSON biçiminde ilişkisel veri biçimlendirme
JSON içinde bir yanıt sağlar ve katman veritabanından veri alır biçimlendirin veya istemci tarafı JavaScript çerçeveler veya verileri kabul kitaplıkları JSON olarak biçimlendirilmiş bir web hizmeti varsa, doğrudan SQL sorgusunda JSON olarak veritabanı içeriğinizi biçimlendirebilirsiniz. Artık sonuçları JSON olarak Azure SQL veritabanından biçimler uygulama kod yazmak zorunda veya Tablo sorgusu sonuçları dönüştürmek ve JSON biçimine nesneleri seri hale getirmek için bazı JSON seri hale getirme kitaplığı içerir. Bunun yerine, FOR JSON yan tümcesi SQL sorgu sonuçları Azure SQL veritabanında JSON olarak biçimlendirmek ve doğrudan, uygulamanızda kullanmak üzere kullanabilirsiniz.

Aşağıdaki örnekte, Sales.Customer tablodaki FOR JSON yan tümcesi kullanılarak JSON olarak biçimlendirilir:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

FOR JSON PATH'i yan tümcesi sorgunun sonuçları JSON metin olarak biçimlendirir. Hücre değerlerini JSON değerleri olarak oluşsa sütun adları anahtarlar kullanılır:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Sonuç kümesi, burada her satır ayrı bir JSON nesnesi olarak biçimlendirilmiş bir JSON dizisi olarak biçimlendirilir.

Sütun diğer adları noktalı gösterim kullanılarak JSON Sonuç kümenizi çıkış biçimini özelleştirebilirsiniz YOLUNU gösterir. Aşağıdaki sorgu çıkış JSON biçiminde "CustomerName" anahtar adını değiştirir ve telefon ve Faks numaraları "Başvurun" alt nesnesinde yerleştirir:

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Bu sorgu çıktısı şu şekildedir:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

Bu örnekte, biz dizi yerine tek bir JSON nesnesi belirterek döndürülen [wıthout_array_wrapper](https://msdn.microsoft.com/library/mt631354.aspx) seçeneği. Sorgu sonucu olarak tek bir nesne döndürme biliyorsanız, bu seçeneği kullanabilirsiniz.

Ana FOR JSON yan tümcesi, iç içe geçmiş JSON nesnelerinin veya dizi olarak biçimlendirilmiş veritabanınızı karmaşık hiyerarşik veri döndürmek sağlar değeri. Aşağıdaki örnek, iç içe geçmiş bir sipariş dizisi olarak müşteriye ait siparişleri dahil et gösterilmektedir:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Müşteri verilerini almak için sorguları ayırın ve ardından ilgili siparişleri listesini getirmek için gerekli tüm verileri tek bir sorgu ile alabilirsiniz aşağıdaki örnek çıktıda görüldüğü gibi göndermek yerine:

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
Karmaşık alt nesneler, dizileri veya hiyerarşik veri varsa, kesinlikle yapılandırılmış veri yoksa veya zaman içinde veri yapılarını gelişmesi, JSON biçimi, herhangi bir karmaşık veri yapısını temsil etmek için yardımcı olabilir.

JSON herhangi bir dize türü Azure SQL veritabanında gibi kullanılabilecek bir metinsel biçimidir. Standart NVARCHAR JSON verilerini depolamak ya da gönder:

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

Bu örnekte kullanılan JSON verilerini NVARCHAR(MAX) türü kullanılarak temsil edilir. JSON bu tabloya eklenen veya aşağıdaki örnekte gösterildiği gibi standart Transact-SQL söz dizimini kullanarak saklı yordam bağımsız değişken olarak sağlanan:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Ayrıca herhangi bir istemci tarafı dili veya Azure SQL veritabanında dize verilerle çalışır kitaplığı JSON verilerle çalışır. JSON, bellek için iyileştirilmiş bir tablo veya bir sistem sürümü tutulan tablo gibi NVARCHAR türü destekleyen herhangi bir tabloda depolanabilir. JSON herhangi bir kısıtlama istemci-tarafı koduna veya veritabanı katmanı sunmaz.

## <a name="querying-json-data"></a>JSON veri sorgulama
Azure SQL tablolarda depolanan JSON olarak biçimlendirilmiş verileri varsa, JSON işlevler içinde herhangi bir SQL sorgu bu verileri kullanmanıza olanak tanır.

Azure SQL veritabanı let kullanılabilir JSON işlevleri başka bir SQL veri türü olarak JSON olarak biçimlendirilmiş verileri kabul eder. Kolayca değerleri JSON Metinden ayıklamak ve herhangi bir sorgu JSON verilerini kullanın:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

JSON_VALUE işlevi bir değer veri sütununda depolanan JSON metinden ayıklar. Bu işlev bir JavaScript benzeri yolu ayıklamak için JSON metin değerinde başvurmak için kullanır. Ayıklanan değerin herhangi bir kısmını SQL sorgusu kullanılabilir.

JSON_QUERY işlevi için JSON_VALUE benzer. JSON_VALUE farklı olarak, bu işlev dizileri veya JSON metninde yerleştirilir nesneleri gibi karmaşık alt nesne ayıklar.

Json_modıfy işlevi değerinin yolunu güncelleştirilmelidir JSON metnini, aynı zamanda eskisinin üzerine yeni bir değer belirtmenize olanak sağlar. Bu şekilde tüm yapısını yeniden ayrıştırmayı olmadan kolayca JSON metnini güncelleştirebilirsiniz.

JSON standart metin olarak kaydedildiği metin sütunlarında depolanan değerleri düzgün biçimlendirilmiş garanti vardır. Standart Azure SQL veritabanı denetimi kısıtlamaları ve ISJSON işlevi kullanarak, JSON sütunda depolanan metin düzgün biçimlendirildiğinden doğrulayabilirsiniz:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Giriş metni düzgün biçimlendirildiğinden, JSON, ISJSON işlevi, 1 değerini döndürür. Her INSERT veya update JSON sütunun üzerinde bu kısıtlamayı yeni metin değeri hatalı biçimlendirilmiş JSON olmadığından emin olun.

## <a name="transforming-json-into-tabular-format"></a>JSON tablo biçimine dönüştürme
Azure SQL veritabanı, tablo biçiminde ve yükleme veya sorgu JSON veri JSON koleksiyonlara dönüştürme sağlar.

OPENJSON JSON metni ayrıştırır, JSON nesnelerinin bir dizisi bulur, dizinin öğeleri tekrarlanan ve elde edilen sonucu dizideki her öğe için bir satır döndürür tablo değerli bir işlevdir.

![JSON tablo](./media/sql-database-json-features/image_2.png)

Yukarıdaki örnekte, biz açılması gerekir ($expand. JSON dizisi bulmanın nerede depolanacağını belirtebilirsiniz Siparişleri yolu), hangi sütunların sonuç ve hücreler olarak döndürülecek JSON değerlerin nereden olarak döndürülmelidir.

Biz bir JSON dizisi dönüştürebilirsiniz @orders değişkenine bir dizi satır, bu sonuç kümesi çözümlemek veya standart bir tabloya satır ekleme:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
Bir JSON dizisi olarak biçimlendirilmiş ve saklı yordama parametre ayrıştırılır ve siparişleri tabloya eklenen verilen siparişler topluluğu.

## <a name="next-steps"></a>Sonraki adımlar
JSON uygulamanıza tümleştirmek öğrenmek için aşağıdaki kaynaklara gözatın:

* [TechNet blogu](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
* [MSDN belgelerine](https://msdn.microsoft.com/library/dn921897.aspx)
* [Kanal 9 video](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

JSON uygulamanıza tümleştirmek için çeşitli senaryoları hakkında bilgi edinmek için bu demo görmek [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) veya kullanım örneğinde eşleşen bir senaryo Bul [JSON Blog yazılarını](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).

