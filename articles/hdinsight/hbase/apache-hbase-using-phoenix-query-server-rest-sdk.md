---
title: Phoenix sorgu sunucusu REST SDK - Azure HDInsight
description: Yükleme ve Azure HDInsight, Phoenix sorgu sunucusu REST SDK'yı kullanın.
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/04/2017
ms.openlocfilehash: 1f468cac29579d8748f61a47b548a67d36ff8279
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123068"
---
# <a name="apache-phoenix-query-server-rest-sdk"></a>Apache Phoenix sorgu sunucusu REST SDK'sı

[Apache Phoenix](https://phoenix.apache.org/) açık kaynaklı, yüksek düzeyde paralel ilişkisel veritabanı katmanı üst kısmındaki [Apache HBase](apache-hbase-overview.md). Phoenix gibi SSH Araçlar üzerinden SQL benzeri sorguları ile HBase kullanan olanak tanır [SQLLine](apache-hbase-phoenix-squirrel-linux.md). Phoenix, istemci iletişimi için iki aktarım mekanizması destekleyen bir ince istemciyi Phoenix sorgu sunucusu (PQS) adlı bir HTTP sunucusu da sağlar: JSON ve protokol arabellekleri. Protokol arabellekleri varsayılan mekanizmasıdır ve JSON daha verimli bir iletişim sunar.

Bu makalede, ayrı ayrı ve toplu tablolar, upsert satır oluşturma ve SQL deyimlerini kullanarak verileri seçme PQS REST SDK'sını kullanmayı açıklar. Örneklerde [Apache Phoenix sorgu sunucusu için Microsoft .NET sürücüsü](https://www.nuget.org/packages/Microsoft.Phoenix.Client). Bu SDK'sı oluşturulan [Apache Calcite'nın Avatica](https://calcite.apache.org/avatica/) API'ler, protokol arabellekleri için serileştirme biçimi kullanımda.

Daha fazla bilgi için [Apache Calcite Avatica protokol arabellekleri başvuru](https://calcite.apache.org/avatica/docs/protobuf_reference.html).

## <a name="install-the-sdk"></a>SDK yükle

Apache Phoenix sorgu sunucusu için Microsoft .NET sürücüsü Visual Studio'dan yüklenebilir bir NuGet paketi olarak sağlanan **NuGet Paket Yöneticisi Konsolu** aşağıdaki komutla:

    Install-Package Microsoft.Phoenix.Client

## <a name="instantiate-new-phoenixclient-object"></a>Yeni PhoenixClient nesnesinin örneğini oluşturma

Kitaplık'ı kullanmaya başlamak için yeni bir örneğini `PhoenixClient` tümleştirilmesidir nesnesi `ClusterCredentials` içeren `Uri` kümesi ve kümenin Apache Hadoop kullanıcı adı ve parola.

```csharp
var credentials = new ClusterCredentials(new Uri("https://CLUSTERNAME.azurehdinsight.net/"), "USERNAME", "PASSWORD");
client = new PhoenixClient(credentials);
```

CLUSTERNAME küme oluşturma üzerinde Hadoop kimlik bilgilerinin HDInsight HBase küme adı ve kullanıcı adı ve parola ile değiştirin. Varsayılan Hadoop kullanıcı adı **yönetici**.

## <a name="generate-unique-connection-identifier"></a>Benzersiz bağlantı kimliği oluştur

PQS için bir veya daha fazla isteği göndermek için istekleri bağlantı ile ilişkilendirmek için bir benzersiz bağlantı kimliği eklemeniz gerekir.

```csharp
string connId = Guid.NewGuid().ToString();
```

Her örneğin ilk çağrıda `OpenConnectionRequestAsync` yöntemini içinde benzersiz bağlantı kimliği. Ardından, tanımlama `ConnectionProperties` ve `RequestOptions`, nesneleri ve üretilen bağlantı kimliği için geçirme `ConnectionSyncRequestAsync` yöntemi. PQS'ın `ConnectionSyncRequest` nesne yardımcı olur istemci ve sunucu veritabanı özellikleri tutarlı bir görünüm olduğundan emin olun.

## <a name="connectionsyncrequest-and-its-connectionproperties"></a>ConnectionSyncRequest ve kendi ConnectionProperties

Çağrılacak `ConnectionSyncRequestAsync`, geçirin bir `ConnectionProperties` nesne.

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

İlgilendiğiniz bazı özellikler şunlardır:

| Özellik | Açıklama |
| -- | -- |
| Otomatik yürütme | Bir boolean belirten olmadığını `autoCommit` Phoenix işlemleri için etkinleştirilir. |
| ReadOnly | Bağlantı salt okunur olup olmadığını belirten bir Boole değeri. |
| TransactionIsolation | Düzeyini belirten bir tamsayı JDBC belirtimi - başına işlem yalıtım aşağıdaki tabloya bakın.|
| Katalog | Bağlantı özellikleri alınırken kullanılacak kataloğu adı. |
| Şema | Bağlantı özellikleri alınırken kullanılacak şemasının adı. |
| IsDirty | Özellikleri değiştirilmiş olup olmadığını belirten bir Boole değeri. |

İşte `TransactionIsolation` değerleri:

| Yalıtım değeri | Açıklama |
| -- | -- |
| 0 | İşlemler desteklenmez. |
| 1 | Kirli okuma, tekrarlanabilir olmayan okuma ve hayali okuma ortaya çıkabilir. |
| 2 | Kirli okuma engellenir ve tekrarlanabilir olmayan okuma ve hayali okumaları oluşabilir. |
| 4 | Kirli okuma ve tekrarlanabilir olmayan okuma engellenir ancak hayali okumaları oluşabilir. |
| 8 | Kirli okuma, tekrarlanabilir olmayan okuma ve hayali okuma tüm engellenir. |

## <a name="create-a-new-table"></a>Yeni bir tablo oluşturma

HBase, diğer RDBMS gibi tablolardaki verileri depolar. Phoenix, birincil anahtar ve sütun türleri tanımlarken yeni tablolar oluşturmak için standart SQL sorgularını kullanır.

Bu örnek ve tüm sonraki örneklerde, örneklenen kullanın `PhoenixClient` nesne sınıfında tanımlandığı gibi [yeni bir PhoenixClient nesnesinin örneğini oluşturma](#instantiate-new-phoenixclient-object).

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

Yukarıdaki örnekte adlı yeni bir tablo oluşturur `Customers` kullanarak `IF NOT EXISTS` seçeneği. `CreateStatementRequestAsync` Çağrı Avitica (PQS) sunucuda yeni bir bildirimi oluşturur. `finally` Blok kapatır döndürülen `CreateStatementResponse` ve `OpenConnectionResponse` nesneleri.

## <a name="insert-data-individually"></a>Tek tek veri ekleme

Bu örnek, tek bir veri gösterir. INSERT, başvuran bir `List<string>` Amerikan eyalet ve bölge kısaltmalar koleksiyonu:

```csharp
var states = new List<string> { "AL", "AK", "AS", "AZ", "AR", "CA", "CO", "CT", "DE", "DC", "FM", "FL", "GA", "GU", "HI", "ID", "IL", "IN", "IA", "KS", "KY", "LA", "ME", "MH", "MD", "MA", "MI", "MN", "MS", "MO", "MT", "NE", "NV", "NH", "NJ", "NM", "NY", "NC", "ND", "MP", "OH", "OK", "OR", "PW", "PA", "PR", "RI", "SC", "SD", "TN", "TX", "UT", "VT", "VI", "VA", "WA", "WV", "WI", "WY" };
```

Tablonun `StateProvince` bir sonraki select işlemi sütun değeri kullanılır.

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

INSERT deyimi yürütmek yapısı, yeni bir tablo oluşturma işlemiyle benzerdir. Sonunda unutmayın `try` blok, işlem açıkça kaydedilmiş. Bu örnekte bir INSERT işlem 300 kez yineler. Aşağıdaki örnek, daha verimli bir toplu ekleme işlemi gösterilmektedir.

## <a name="batch-insert-data"></a>Toplu ekleme verileri

Aşağıdaki kod, veri tek tek ekleme kodu hemen hemen aynıdır. Bu örnekte `UpdateBatch` nesne bir çağrıda `ExecuteBatchRequestAsync`, tekrar tekrar çağırmak yerine `ExecuteRequestAsync` hazırlanmış bir deyim ile.

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

Bir test ortamında, tek başına 300 yeni kayıtlar ekleme neredeyse 2 dakika sürdü. Buna karşılık, 300 kayıtları toplu olarak ekleme yalnızca 6 (sn) gereklidir.

## <a name="select-data"></a>Verileri seçme

Bu örnek, birden çok sorgu yürütmek için bir bağlantı yeniden gösterilmektedir:

1. Tüm kayıtları seçmek ve varsayılan en fazla 100 döndürülür sonra kalan kayıtlar ardından getirilemedi.
2. Toplam satır sayısı select deyimiyle tek skaler sonuç almak için kullanın.
3. Müşteriler eyalet veya bölge başına toplam sayısını döndüren bir select deyimi yürütün.

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

Çıkışı `select` ifadeleri, aşağıdaki sonucu olmalıdır:

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

* [HDInsight üzerinde Apache Phoenix](../hdinsight-phoenix-in-hdinsight.md)
* [Apache HBase REST SDK'sını kullanma](apache-hbase-rest-sdk.md)
