---
title: Phoenix sorgu sunucusu REST SDK - Azure Hdınsight | Microsoft Docs
description: ''
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: ashishth
ms.openlocfilehash: ef89bcea3eab92c3137a6f532398764462ae204c
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="phoenix-query-server-rest-sdk"></a>Phoenix sorgu sunucusu REST SDK'sı

[Apache Phoenix](http://phoenix.apache.org/) bir, açık kaynaklı, yüksek düzeyde paralel ilişkisel veritabanı katmanı üstünde [HBase](apache-hbase-overview.md). Phoenix SSH araçları üzerinden HBase ile SQL benzeri sorguları gibi kullanmanıza olanak verir [SQLLine](apache-hbase-phoenix-squirrel-linux.md). Phoenix, istemci iletişimi için iki aktarım mekanizmayı destekler ince bir istemcinin Phoenix sorgu sunucu (PQS) adlı bir HTTP sunucusu da sağlar: JSON ve protokolü arabellek. Protokol arabellek varsayılan mekanizmasıdır ve JSON'den daha verimli iletişimi sunar.

Bu makalede, ayrı ayrı ve toplu tabloları, upsert satırları oluşturmak ve SQL deyimlerini kullanarak verileri seçmek için PQS REST SDK'sını kullanmayı açıklar. Örneklerde [Apache Phoenix sorgu Server için Microsoft .NET sürücüsü](https://www.nuget.org/packages/Microsoft.Phoenix.Client). Bu SDK'sı yerleşik [Apache Calcite'nın Avatica](https://calcite.apache.org/avatica/) Protokolü arabellekleri için seri hale getirme biçimi kullanımda API'leri.

Daha fazla bilgi için bkz: [Apache Calcite Avatica Protokolü arabellekleri başvurusu](https://calcite.apache.org/avatica/docs/protobuf_reference.html).

## <a name="install-the-sdk"></a>SDK yükle

Apache Phoenix sorgu Server için Microsoft .NET sürücüsü Visual Studio'dan yüklenebilir bir NuGet paketi olarak sağlanır **NuGet Paket Yöneticisi Konsolu** aşağıdaki komutla:

    Install-Package Microsoft.Phoenix.Client

## <a name="instantiate-new-phoenixclient-object"></a>Yeni PhoenixClient nesne örneği

Kitaplık'ı kullanmaya başlamak için yeni bir örneğini `PhoenixClient` tümleştirilmesidir nesne `ClusterCredentials` içeren `Uri` kümesi ve kümenin Hadoop kullanıcı adı ve parola.

```csharp
var credentials = new ClusterCredentials(new Uri("https://CLUSTERNAME.azurehdinsight.net/"), "USERNAME", "PASSWORD");
client = new PhoenixClient(credentials);
```

CLUSTERNAME Hdınsight HBase küme adı ve kullanıcı adı ve parola ile küme oluşturma belirtilen Hadoop kimlik bilgilerini değiştirin. Varsayılan Hadoop kullanıcı adı **yönetici**.

## <a name="generate-unique-connection-identifier"></a>Benzersiz bağlantı kimliği oluştur

Bir veya daha fazla istekleri için PQS göndermek için istekleri bağlantı ile ilişkilendirmek için benzersiz bağlantı kimliği eklemeniz gerekir.

```csharp
string connId = Guid.NewGuid().ToString();
```

Her örneğin ilk çağrıda bulunur `OpenConnectionRequestAsync` yöntemini benzersiz bağlantı kimliği. Ardından, tanımlamak `ConnectionProperties` ve `RequestOptions`, bu nesnelerin ve oluşturulan bağlantı tanımlayıcı geçirme `ConnectionSyncRequestAsync` yöntemi. PQS'ın `ConnectionSyncRequest` nesne yardımcı olur istemci ve sunucu veritabanı özellikleri tutarlı bir görünüm olduğundan emin olun.

## <a name="connectionsyncrequest-and-its-connectionproperties"></a>ConnectionSyncRequest ve kendi ConnectionProperties

Çağrılacak `ConnectionSyncRequestAsync`, geçirin bir `ConnectionProperties` nesnesi.

```csharp
ConnectionProperties connProperties = new ConnectionProperties
{
    HasAutoCommit = true,
    AutoCommit = true,
    HasReadOnly = true,
    ReadOnly = false,
    TransactionIsolation = 0,
    Catalog = "",
    Schema = "",
    IsDirty = true
};
await client.ConnectionSyncRequestAsync(connId, connProperties, options);
```

İlgilenilen bazı özellikler şunlardır:

| Özellik | Açıklama |
| -- | -- |
| Otomatik kayıt işlemleri | Boole belirten olup olmadığını `autoCommit` Phoenix işlemleri için etkinleştirilir. |
| SaltOkunur | Bağlantı salt okunur olup olmadığını belirten bir Boole değeri. |
| TransactionIsolation | Düzeyi belirten Tamsayı JDBC belirtimi - başına işlem yalıtım aşağıdaki tabloya bakın.|
| Katalog | Bağlantı özellikleri getirilirken kullanmak için katalog adı. |
| Şema | Bağlantı özellikleri getirilirken kullanılacak şema adı. |
| IsDirty | Özellikleri değiştirilmiş olup olmadığını belirten bir Boole değeri. |

Burada `TransactionIsolation` değerler:

| Yalıtım değeri | Açıklama |
| -- | -- |
| 0 | İşlemleri desteklenmez. |
| 1 | Kirli okuma, tekrarlanabilir olmayan okur ve hayali okuma ortaya çıkabilir. |
| 2 | Kirli okuma engellenir, ancak yinelenebilir olmayan okur ve hayali okuma ortaya çıkabilir. |
| 4 | Kirli okur ve tekrarlanabilir olmayan okuma engellenir, ancak hayali okuma ortaya çıkabilir. |
| 8 | Kirli okuma, tekrarlanabilir olmayan okur ve hayali okuma tüm engellenir. |

## <a name="create-a-new-table"></a>Yeni bir tablo oluştur

Diğer RDBMS gibi HBase tablolarındaki verileri depolar. Phoenix, birincil anahtar ve sütun türleri tanımlarken yeni tablolar oluşturmak için standart SQL sorgularını kullanır.

Bu örnek ve tüm sonraki örnekler, örneklenen kullanmak `PhoenixClient` nesne sınıfında tanımlandığı gibi [yeni PhoenixClient nesne örneği](#instantiate-new-phoenixclient-object).

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();

// You can set certain request options, such as timeout in milliseconds:
options.TimeoutMillis = 300000;

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
CreateStatementResponse createStatementResponse = null;
OpenConnectionResponse openConnResponse = null;

try
{
    // Opening connection
    var info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);

    // Create the statement
    createStatementResponse = client.CreateStatementRequestAsync(connId, options).Result;
    
    // Create the table if it does not exist
    string sql = "CREATE TABLE IF NOT EXISTS Customers (Id varchar(20) PRIMARY KEY, FirstName varchar(50), " +
        "LastName varchar(100), StateProvince char(2), Email varchar(255), Phone varchar(15))";
    await client.PrepareAndExecuteRequestAsync(connId, sql, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);

    Console.WriteLine($"Table \"Customers\" created.");
}
catch (Exception e)
{
    Console.WriteLine(e);
    throw;
}
finally
{
    if (createStatementResponse != null)
    {
        client.CloseStatementRequestAsync(connId, createStatementResponse.StatementId, options).Wait();
        createStatementResponse = null;
    }

    if (openConnResponse != null)
    {
        client.CloseConnectionRequestAsync(connId, options).Wait();
        openConnResponse = null;
    }
}
```

Önceki örnekte adlı yeni bir tablo oluşturur `Customers` kullanarak `IF NOT EXISTS` seçeneği. `CreateStatementRequestAsync` Çağrısı Avitica (PQS) sunucuda yeni bir bildirimi oluşturur. `finally` Blok kapatır döndürülen `CreateStatementResponse` ve `OpenConnectionResponse` nesneleri.

## <a name="insert-data-individually"></a>Tek tek veri Ekle

Bu örnek, bir tek veri gösterir Ekle, başvuran bir `List<string>` Amerikan durumu ve bölge kısaltmalar koleksiyonu:

```csharp
var states = new List<string> { "AL", "AK", "AS", "AZ", "AR", "CA", "CO", "CT", "DE", "DC", "FM", "FL", "GA", "GU", "HI", "ID", "IL", "IN", "IA", "KS", "KY", "LA", "ME", "MH", "MD", "MA", "MI", "MN", "MS", "MO", "MT", "NE", "NV", "NH", "NJ", "NM", "NY", "NC", "ND", "MP", "OH", "OK", "OR", "PW", "PA", "PR", "RI", "SC", "SD", "TN", "TX", "UT", "VT", "VI", "VA", "WA", "WV", "WI", "WY" };
```

Tablonun `StateProvince` sütun değeri bir sonraki select işleminde kullanılacak.

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();
options.TimeoutMillis = 300000;

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
OpenConnectionResponse openConnResponse = null;
StatementHandle statementHandle = null;
try
{
    // Opening connection
    pbc::MapField<string, string> info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);

    string sql = "UPSERT INTO Customers VALUES (?,?,?,?,?,?)";
    PrepareResponse prepareResponse = await client.PrepareRequestAsync(connId, sql, 100, options);
    statementHandle = prepareResponse.Statement;
    
    var r = new Random();

    // Insert 300 rows
    for (int i = 0; i < 300; i++)
    {
        var list = new pbc.RepeatedField<TypedValue>();
        var id = new TypedValue
        {
            StringValue = "id" + i,
            Type = Rep.String
        };
        var firstName = new TypedValue
        {
            StringValue = "first" + i,
            Type = Rep.String
        };
        var lastName = new TypedValue
        {
            StringValue = "last" + i,
            Type = Rep.String
        };
        var state = new TypedValue
        {
            StringValue = states.ElementAt(r.Next(0, 49)),
            Type = Rep.String
        };
        var email = new TypedValue
        {
            StringValue = $"email{1}@junkemail.com",
            Type = Rep.String
        };
        var phone = new TypedValue
        {
            StringValue = $"555-229-341{i.ToString().Substring(0,1)}",
            Type = Rep.String
        };
        list.Add(id);
        list.Add(firstName);
        list.Add(lastName);
        list.Add(state);
        list.Add(email);
        list.Add(phone);

        Console.WriteLine("Inserting customer " + i);

        await client.ExecuteRequestAsync(statementHandle, list, long.MaxValue, true, options);
    }

    await client.CommitRequestAsync(connId, options);

    Console.WriteLine("Upserted customer data");

}
catch (Exception ex)
{

}
finally
{
    if (statementHandle != null)
    {
        await client.CloseStatementRequestAsync(connId, statementHandle.Id, options);
        statementHandle = null;
    }
    if (openConnResponse != null)
    {
        await client.CloseConnectionRequestAsync(connId, options);
        openConnResponse = null;
    }
}
```

INSERT deyimi yürütmek yapısı, yeni bir tablo oluşturmak için benzer. Sonunda unutmayın `try` bloğu, işlem açıkça kaydedilmiş. Bu örnek, bir ekleme işlemi 300 kez yineler. Aşağıdaki örnek daha verimli bir toplu ekleme işlemi gösterilmektedir.

## <a name="batch-insert-data"></a>Verileri toplu Ekle

Aşağıdaki kod, veri tek tek ekleme kodunu neredeyse aynıdır. Bu örnekte `UpdateBatch` yapılan bir çağrı nesnesinde `ExecuteBatchRequestAsync`, art arda çağırmak yerine `ExecuteRequestAsync` hazırlanmış deyim ile.

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();
options.TimeoutMillis = 300000;

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
OpenConnectionResponse openConnResponse = null;
CreateStatementResponse createStatementResponse = null;
try
{
    // Opening connection
    pbc::MapField<string, string> info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);

    // Creating statement
    createStatementResponse = await client.CreateStatementRequestAsync(connId, options);

    string sql = "UPSERT INTO Customers VALUES (?,?,?,?,?,?)";
    PrepareResponse prepareResponse = client.PrepareRequestAsync(connId, sql, long.MaxValue, options).Result;
    var statementHandle = prepareResponse.Statement;
    var updates = new pbc.RepeatedField<UpdateBatch>();

    var r = new Random();

    // Insert 300 rows
    for (int i = 300; i < 600; i++)
    {
        var list = new pbc.RepeatedField<TypedValue>();
        var id = new TypedValue
        {
            StringValue = "id" + i,
            Type = Rep.String
        };
        var firstName = new TypedValue
        {
            StringValue = "first" + i,
            Type = Rep.String
        };
        var lastName = new TypedValue
        {
            StringValue = "last" + i,
            Type = Rep.String
        };
        var state = new TypedValue
        {
            StringValue = states.ElementAt(r.Next(0, 49)),
            Type = Rep.String
        };
        var email = new TypedValue
        {
            StringValue = $"email{1}@junkemail.com",
            Type = Rep.String
        };
        var phone = new TypedValue
        {
            StringValue = $"555-229-341{i.ToString().Substring(0, 1)}",
            Type = Rep.String
        };
        list.Add(id);
        list.Add(firstName);
        list.Add(lastName);
        list.Add(state);
        list.Add(email);
        list.Add(phone);

        var batch = new UpdateBatch
        {
            ParameterValues = list
        };
        updates.Add(batch);

        Console.WriteLine($"Added customer {i} to batch");
    }

    var executeBatchResponse = await client.ExecuteBatchRequestAsync(connId, statementHandle.Id, updates, options);

    Console.WriteLine("Batch upserted customer data");

}
catch (Exception ex)
{

}
finally
{
    if (openConnResponse != null)
    {
        await client.CloseConnectionRequestAsync(connId, options);
        openConnResponse = null;
    }
}
```

Bir test ortamında, ayrı ayrı 300 yeni kayıtlar ekleme yaklaşık 2 dakika sürdü. Buna karşılık, 300 kayıtları toplu ekleme yalnızca 6 saniye gereklidir.

## <a name="select-data"></a>Verileri seçme

Bu örnek, birden çok sorgularını yürütmek için bir bağlantı yeniden gösterilmektedir:

1. Tüm kayıtları seçin ve varsayılan en fazla 100 döndürülür sonra geri kalan kayıtları getirilemedi.
2. Tek bir skaler sonuç almak için toplam satır sayısı select deyimi kullanın.
3. Müşteriler eyalet veya bölge başına toplam sayısı döndüren bir select deyimi yürütün.

```csharp
string connId = Guid.NewGuid().ToString();
RequestOptions options = RequestOptions.GetGatewayDefaultOptions();

// In gateway mode, PQS requests will be https://<cluster dns name>.azurehdinsight.net/hbasephoenix<N>/
// Requests sent to hbasephoenix0/ will be forwarded to PQS on workernode0
options.AlternativeEndpoint = "hbasephoenix0/";
OpenConnectionResponse openConnResponse = null;
StatementHandle statementHandle = null;

try
{
    // Opening connection
    pbc::MapField<string, string> info = new pbc::MapField<string, string>();
    openConnResponse = await client.OpenConnectionRequestAsync(connId, info, options);
    // Syncing connection
    ConnectionProperties connProperties = new ConnectionProperties
    {
        HasAutoCommit = true,
        AutoCommit = true,
        HasReadOnly = true,
        ReadOnly = false,
        TransactionIsolation = 0,
        Catalog = "",
        Schema = "",
        IsDirty = true
    };
    await client.ConnectionSyncRequestAsync(connId, connProperties, options);
    var createStatementResponse = await client.CreateStatementRequestAsync(connId, options);

    string sql = "SELECT * FROM Customers";
    ExecuteResponse executeResponse = await client.PrepareAndExecuteRequestAsync(connId, sql, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);

    pbc::RepeatedField<Row> rows = executeResponse.Results[0].FirstFrame.Rows;
    // Loop through all of the returned rows and display the first two columns
    for (int i = 0; i < rows.Count; i++)
    {
        Row row = rows[i];
        Console.WriteLine(row.Value[0].ScalarValue.StringValue + " " + row.Value[1].ScalarValue.StringValue);
    }
    
    // 100 is hard-coded on the server side as the default firstframe size
    // FetchRequestAsync is called to get any remaining rows
    Console.WriteLine("");
    Console.WriteLine($"Number of rows: {rows.Count}");

    // Fetch remaining rows, offset is not used, simply set to 0
    // When FetchResponse.Frame.Done is true, all rows were fetched
    FetchResponse fetchResponse = await client.FetchRequestAsync(connId, createStatementResponse.StatementId, 0, int.MaxValue, options);
    Console.WriteLine($"Frame row count: {fetchResponse.Frame.Rows.Count}");
    Console.WriteLine($"Fetch response is done: {fetchResponse.Frame.Done}");
    Console.WriteLine("");

    // Running query 2
    string sql2 = "select count(*) from Customers";
    ExecuteResponse countResponse = await client.PrepareAndExecuteRequestAsync(connId, sql2, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);
    long count = countResponse.Results[0].FirstFrame.Rows[0].Value[0].ScalarValue.NumberValue;

    Console.WriteLine($"Total customer records: {count}");
    Console.WriteLine("");

    // Running query 3
    string sql3 = "select StateProvince, count(*) as Number from Customers group by StateProvince order by Number desc";
    ExecuteResponse groupByResponse = await client.PrepareAndExecuteRequestAsync(connId, sql3, createStatementResponse.StatementId, long.MaxValue, int.MaxValue, options);

    pbc::RepeatedField<Row> stateRows = groupByResponse.Results[0].FirstFrame.Rows;
    for (int i = 0; i < stateRows.Count; i++)
    {
        Row row = stateRows[i];
        Console.WriteLine(row.Value[0].ScalarValue.StringValue + ": " + row.Value[1].ScalarValue.NumberValue);
    }
}
catch (Exception ex)
{

}
finally
{
    if (statementHandle != null)
    {
        await client.CloseStatementRequestAsync(connId, statementHandle.Id, options);
        statementHandle = null;
    }
    if (openConnResponse != null)
    {
        await client.CloseConnectionRequestAsync(connId, options);
        openConnResponse = null;
    }
}
```

Çıktısını `select` deyimleri aşağıdaki sonucu olması gerekir:

```
id0 first0
id1 first1
id10 first10
id100 first100
id101 first101
id102 first102
. . .
id185 first185
id186 first186
id187 first187
id188 first188

Number of rows: 100
Frame row count: 500
Fetch response is done: True

Total customer records: 600

NJ: 21
CA: 19
GU: 17
NC: 16
IN: 16
MA: 16
AZ: 16
ME: 16
IL: 15
OR: 15
. . . 
MO: 10
HI: 10
GA: 10
DC: 9
NM: 9
MD: 9
MP: 9
SC: 7
AR: 7
MH: 6
FM: 5
```

## <a name="next-steps"></a>Sonraki adımlar 

* [Phoenix hdınsight'ta](../hdinsight-phoenix-in-hdinsight.md)
* [HBase REST SDK'sını kullanarak](apache-hbase-rest-sdk.md)
