---
title: Azure HDInsight HBase .NET SDK'sı - kullanın
description: Oluşturma ve tabloları, silme ve okuma ve yazma veri HBase .NET SDK'sını kullanın.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/13/2017
ms.author: ashishth
ms.openlocfilehash: 707869880c5df619def2d707264b59e22e03c521
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64720308"
---
# <a name="use-the-net-sdk-for-apache-hbase"></a>Apache HBase için .NET SDK'sını kullanma

[Apache HBase](apache-hbase-overview.md) , verilerle çalışmak için iki birincil seçenek sağlar: [Apache Hive sorguları ve HBase'nın RESTful API çağrıları](apache-hbase-tutorial-get-started-linux.md). REST API kullanarak doğrudan ile çalışabilir `curl` komut veya benzer bir yardımcı program.

C# ve .NET uygulamaları için [.NET için Microsoft HBase REST istemci Kitaplığı](https://www.nuget.org/packages/Microsoft.HBase.Client/) HBase REST API'SİNİN üstünde bir istemci kitaplığı sağlar.

## <a name="install-the-sdk"></a>SDK yükle

HBase .NET SDK'sı Visual Studio yüklü bir NuGet paketi olarak sağlanan **NuGet Paket Yöneticisi Konsolu** aşağıdaki komutla:

    Install-Package Microsoft.HBase.Client

## <a name="instantiate-a-new-hbaseclient-object"></a>Yeni bir HBaseClient nesnesinin örneğini oluşturma

SDK'yı kullanmak için yeni bir örneğini `HBaseClient` tümleştirilmesidir nesnesi `ClusterCredentials` oluşan `Uri` kümenize Hadoop kullanıcı adı ve parola.

```csharp
var credentials = new ClusterCredentials(new Uri("https://CLUSTERNAME.azurehdinsight.net"), "USERNAME", "PASSWORD");
client = new HBaseClient(credentials);
```

CLUSTERNAME küme oluşturma Apache Hadoop kimlik bilgilerinin HDInsight HBase küme adı ve kullanıcı adı ve parola ile değiştirin. Varsayılan Hadoop kullanıcı adı **yönetici**.

## <a name="create-a-new-table"></a>Yeni bir tablo oluşturma

HBase tabloları verilerini depolar. Bir tablo oluşan bir *Rowkey*, birincil anahtar ve bir veya daha fazla sütun grupları adlı *sütun ailesi*. Her tablodaki verileri yatay olarak Rowkey aralığına göre dağıtılmış *bölgeleri*. Her bölgede bir başlangıç ve bitiş anahtarına sahiptir. Bir tabloda bir veya daha fazla bölgede olabilir. Tablodaki veri miktarı arttıkça, HBase büyük bölge daha küçük bölgeye böler. Bölge içinde depolanıyorsa *bölge sunucuları*, birden çok bölgede bir bölge sunucusu burada depolayabilirsiniz.

Verileri fiziksel olarak depolanan *HFiles*. Tek bir Hfıle bir tablo, tek bir bölge ve bir sütun ailesi veri içerir. Hfıle satırlarda Rowkey üzerinde sıralanmış depolanır. Her Hfıle sahip bir *B + Ağaç* hızlı alma satır dizini.

Yeni bir tablo oluşturmak için belirtin bir `TableSchema` ve sütun. Aşağıdaki kod 'RestSDKTable' zaten var - tablo Aksi halde tablo oluşturuldu olup olmadığını denetler.

```csharp
if (!client.ListTablesAsync().Result.name.Contains("RestSDKTable"))
{
    // Create the table
    var newTableSchema = new TableSchema {name = "RestSDKTable" };
    newTableSchema.columns.Add(new ColumnSchema() {name = "t1"});
    newTableSchema.columns.Add(new ColumnSchema() {name = "t2"});

    await client.CreateTableAsync(newTableSchema);
}
```

Bu yeni bir tablo, iki sütun ailesi, t1 ve t2 ' var. Sütun ailesi içinde farklı HFiles ayrı olarak depolandığından, sık sık sorgulanan veriler için ayrı bir sütun ailesi için mantıklıdır. Aşağıdaki [veri ekleme](#insert-data) örnek, sütunların t1 sütun ailesinde eklenir.

## <a name="delete-a-table"></a>Bir tablo silme

Bir tablo silmek için:

```csharp
await client.DeleteTableAsync("RestSDKTable");
```

## <a name="insert-data"></a>Veri ekleme

Veri eklemek için bir benzersiz bir satır anahtarı satır tanımlayıcısı olarak belirtin. Tüm veri depolanan bir `byte[]` dizi. Aşağıdaki kod, tanımlar ve ekler `title`, `director`, ve `release_date` t1 sütun ailesi için bir sütun olarak bu sütunların en sık erişilen olan. `description` Ve `tagline` sütunları, t2 sütun ailesinde eklenir. Verilerinizi gerektiği şekilde sütun ailesi bölümleyebilirsiniz.

```csharp
var key = "fifth_element";
var row = new CellSet.Row { key = Encoding.UTF8.GetBytes(key) };
var value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:title"),
    data = Encoding.UTF8.GetBytes("The Fifth Element")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:director"),
    data = Encoding.UTF8.GetBytes("Luc Besson")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:release_date"),
    data = Encoding.UTF8.GetBytes("1997")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t2:description"),
    data = Encoding.UTF8.GetBytes("In the colorful future, a cab driver unwittingly becomes the central figure in the search for a legendary cosmic weapon to keep Evil and Mr Zorg at bay.")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t2:tagline"),
    data = Encoding.UTF8.GetBytes("The Fifth is life")
};
row.values.Add(value);

var set = new CellSet();
set.rows.Add(row);

await client.StoreCellsAsync("RestSDKTable", set);
```

HBase uygulayan [bulut BigTable](https://cloud.google.com/bigtable/), veri biçimi aşağıdaki gibi görünür:

![Küme kullanıcı rolüne sahip kullanıcı](./media/apache-hbase-rest-sdk/table.png)

## <a name="select-data"></a>Verileri seçme

Bir HBase tablosunda verileri okumak için tablo adı ve satır anahtarı geçirmek `GetCellsAsync` döndürülecek yöntemi `CellSet`.

```csharp
var key = "fifth_element";

var cells = await client.GetCellsAsync("RestSDKTable", key);
// Get the first value from the row.
Console.WriteLine(Encoding.UTF8.GetString(cells.rows[0].values[0].data));
// Get the value for t1:title
Console.WriteLine(Encoding.UTF8.GetString(cells.rows[0].values
    .Find(c => Encoding.UTF8.GetString(c.column) == "t1:title").data));
// With the previous insert, it should yield: "The Fifth Element"
```

Bu durumda, yalnızca olması gerektiğinden benzersiz bir anahtar için bir satır kod yalnızca ilk eşleşen satırı döndürür. Döndürülen değer ile değiştirilir `string` öğesinden biçim `byte[]` dizisi. Değeri, tamsayı filmin yayın tarihi gibi diğer türlere de dönüştürebilirsiniz:

```csharp
var releaseDateField = cells.rows[0].values
    .Find(c => Encoding.UTF8.GetString(c.column) == "t1:release_date");
int releaseDate = 0;

if (releaseDateField != null)
{
    releaseDate = Convert.ToInt32(Encoding.UTF8.GetString(releaseDateField.data));
}
Console.WriteLine(releaseDate);
// Should return 1997
```

## <a name="scan-over-rows"></a>Satır üzerinde tarama

HBase kullanır `scan` bir veya daha fazla satır alınamıyor. Bu örnekte, birden çok satır 10 toplu işler halinde ister ve 35 ile 25 arasında anahtar değerleri olan verileri alır. Tüm satırları aldıktan sonra kaynakları temizlemek için tarayıcı silin.

```csharp
var tableName = "mytablename";

// Assume the table has integer keys and we want data between keys 25 and 35
var scanSettings = new Scanner()
{
    batch = 10,
    startRow = BitConverter.GetBytes(25),
    endRow = BitConverter.GetBytes(35)
};
RequestOptions scanOptions = RequestOptions.GetDefaultOptions();
scanOptions.AlternativeEndpoint = "hbaserest0/";
ScannerInformation scannerInfo = null;
try
{
    scannerInfo = await client.CreateScannerAsync(tableName, scanSettings, scanOptions);
    CellSet next = null;
    while ((next = client.ScannerGetNextAsync(scannerInfo, scanOptions).Result) != null)
    {
        foreach (var row in next.rows)
        {
            // ... read the rows
        }
    }
}
finally
{
    if (scannerInfo != null)
    {
        await client.DeleteScannerAsync(tableName, scannerInfo, scanOptions);
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight, Apache HBase örneğiyle çalışmaya başlama](apache-hbase-tutorial-get-started-linux.md)
* Uçtan uca uygulamayla yapı [Apache HBase ile gerçek zamanlı Twitter düşüncelerini çözümleme](../hdinsight-hbase-analyze-twitter-sentiment.md)
