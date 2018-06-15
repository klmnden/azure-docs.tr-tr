---
title: Azure Hdınsight HBase .NET SDK'sı - kullanma | Microsoft Docs
description: Oluşturma ve tabloları, silme ve okumak ve yazmak için HBase .NET SDK'yı kullanın.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: ashishth
ms.openlocfilehash: f0e2c6412a965c73b0055a24c799e05ad582a8c7
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34164525"
---
# <a name="use-the-hbase-net-sdk"></a>HBase .NET SDK'sını kullanma

[HBase](apache-hbase-overview.md) , verilerle çalışmak için iki birincil seçenek sağlar: [Hive sorguları ve HBase'nın RESTful API'si çağrılarını](apache-hbase-tutorial-get-started-linux.md). REST API kullanarak doğrudan ile çalışabilirsiniz `curl` komut veya benzer bir yardımcı programı.

C# ve .NET uygulamaları için [.NET için Microsoft HBase REST istemci Kitaplığı](https://www.nuget.org/packages/Microsoft.HBase.Client/) HBase REST API'SİNİN üstünde bir istemci kitaplığı sağlar.

## <a name="install-the-sdk"></a>SDK yükle

HBase .NET SDK'sı Visual Studio'dan yüklenebilir bir NuGet paketi olarak sağlanan **NuGet Paket Yöneticisi Konsolu** aşağıdaki komutla:

    Install-Package Microsoft.HBase.Client

## <a name="instantiate-a-new-hbaseclient-object"></a>Yeni bir HBaseClient nesnesi

SDK'yı kullanmak için yeni bir örneğini `HBaseClient` tümleştirilmesidir nesne `ClusterCredentials` oluşan `Uri` , küme ve Hadoop kullanıcı adı ve parola.

```csharp
var credentials = new ClusterCredentials(new Uri("https://CLUSTERNAME.azurehdinsight.net"), "USERNAME", "PASSWORD");
client = new HBaseClient(credentials);
```

CLUSTERNAME Hdınsight HBase küme adı ve kullanıcı adı ve parola ile küme oluşturma belirtilen Hadoop kimlik bilgilerini değiştirin. Varsayılan Hadoop kullanıcı adı **yönetici**.

## <a name="create-a-new-table"></a>Yeni bir tablo oluştur

HBase tablolarındaki verileri depolar. Bir tablo oluşan bir *Rowkey*, birincil anahtar ve bir veya daha fazla grupları sütunların adlı *sütun ailesi*. Her tablodaki verileri yatay olarak bir Rowkey aralığına göre dağıtılmış *bölgeleri*. Her bölge bir başlangıç ve bitiş anahtara sahip. Bir tabloda bir veya daha fazla bölge bulunabilir. Tablodaki verileri büyüdükçe, HBase büyük bölgeler küçük bölgeye böler. Bölgeler depolanır *bölge sunucuları*, burada bir bölge sunucu birden çok bölgeye depolayabilirsiniz.

Verileri fiziksel olarak depolanan *HFiles*. Tek bir Hfıle bir tablo, tek bir bölge ve bir sütun ailesi için verileri içerir. Hfıle satır üzerinde Rowkey sıralanmış depolanır. Her Hfıle sahip bir *B + ağacında* hızlı alma satır dizini.

Yeni bir tablo oluşturmak için bu seçeneği belirtin bir `TableSchema` ve sütun. Aşağıdaki kod 'RestSDKTable' zaten - tablo değilse, tablonun oluşturulduğu olup olmadığını denetler.

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

Bu yeni bir tablo iki sütun ailesi, t1 ve t2 sahiptir. Sütun ailesi içinde farklı HFiles ayrı olarak depolandığından, ayrı sütun ailesi sık Sorgulanmış veriler için anlamlıdır. Aşağıdaki [veri Ekle](#insert-data) örnek, sütunları t1 sütun ailesine eklenir.

## <a name="delete-a-table"></a>Bir tablo silme

Bir tabloyu silmek için:

```csharp
await client.DeleteTableAsync("RestSDKTable");
```

## <a name="insert-data"></a>Veri ekleme

Veri eklemek için bir benzersiz bir satır anahtarı satır tanımlayıcısı olarak belirtin. İçinde depolanan tüm verileri bir `byte[]` dizi. Aşağıdaki kod tanımlar ve ekler `title`, `director`, ve `release_date` t1 sütun ailesi sütunları bu sütunlar en sık erişilen olur. `description` Ve `tagline` sütunları t2 sütun ailesine eklenir. Verilerinizi gerektiği şekilde sütun ailesi bölüm.

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

Veri biçimi aşağıdakine benzer şekilde BigTable, HBase uygular:

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

Bu durumda, yalnızca olmamalıdır gibi benzersiz bir anahtar için bir satır kod yalnızca ilk eşleşen satır, döndürür. Döndürülen değer içine değiştirilir `string` gelen biçimlendirmek `byte[]` dizi. Tamsayı film yayın tarihi gibi diğer türleri için değer de dönüştürebilirsiniz:

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

## <a name="scan-over-rows"></a>Satır tarama

HBase kullanır `scan` bir veya daha fazla satır alınamadı. Bu örnek, birden çok satır 10 toplu istekleri ve 35 25 arasında anahtar değerleri olan verileri alır. Tüm satırları aldıktan sonra kaynakları temizlemek için tarayıcı silin.

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

* [Hdınsight'ta Apache HBase örneği kullanmaya başlama](apache-hbase-tutorial-get-started-linux.md)
* Uçtan uca uygulamayla yapı [HBase ile Twitter düşüncelerini gerçek zamanlı analiz](../hdinsight-hbase-analyze-twitter-sentiment.md)
