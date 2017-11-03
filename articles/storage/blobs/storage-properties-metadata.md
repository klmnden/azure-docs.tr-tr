---
title: "Ayarlayın ve nesne özellikleri ve Azure depolama alanında meta veri alma | Microsoft Docs"
description: "Azure Storage nesnelerde özel meta verileri depolamak ve ayarlayın ve sistem alınamıyor."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 036f9006-273e-400b-844b-3329045e9e1f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: tamram
ms.openlocfilehash: a3eb598b2dabd4986c72b8814926eb0944707050
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-and-retrieve-properties-and-metadata"></a>Özellikler ile meta verileri ayarlama ve alma

Azure Storage destek Sistem özellikleri ve kullanıcı tanımlı meta veriler, içerdikleri verilere ek olarak nesneleri. Bu makalede ele yönetme Sistem özellikleri ve kullanıcı tanımlı meta verileriyle [.NET için Azure Storage istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/).

* **Sistem Özellikleri**: Sistem özellikleri her depolama kaynağı yok. Bunlardan bazıları okunabilir veya başkalarının salt okunur durumdayken ayarlayın. Perde arkasında bazı sistem özellikleri belirli standart HTTP üstbilgilerine karşılık gelir. Azure storage istemci kitaplığı bunları tutar.

* **Kullanıcı tanımlı meta veriler**: kullanıcı tanımlı meta veriler olan bir ad-değer çifti biçiminde belirli bir kaynak üzerinde belirtin meta verileri. Meta veri depolama kaynağı ek değerlerle depolamak için kullanabilirsiniz. Bu ek meta veri değerleri kendi yalnızca amaçlıdır ve kaynak biçimini etkilemez.

Depolama kaynak için özellik ve meta veri değerlerini alma iki adımlı bir işlemdir. Bu değerleri okumadan önce açıkça bunları çağırarak alması gerekir **FetchAttributesAsync** yöntemi.

> [!IMPORTANT]
> Özellik ve meta veri değerlerini depolama kaynağı için aşağıdakilerden birini gerektirmediği sürece doldurulmamış **FetchAttributesAsync** yöntemleri.
>
> Alacağınız bir `400 Bad Request` tüm ad/değer çiftleri ASCII olmayan karakterler içeriyorsa. Meta veri ad/değer çiftleri geçerli HTTP üstbilgi olduğundan ve bu nedenle tüm kısıtlamaları HTTP üstbilgileri yöneten uyması gerekir. Bu nedenle, URL kodlaması veya adları ve ASCII olmayan karakterler içeren bir değer için Base64 kodlaması kullanmanız önerilir.
>

## <a name="setting-and-retrieving-properties"></a>Özellikler alma ve ayarlama
Özellik değerlerini almak için arama **FetchAttributesAsync** yöntemi blob veya özelliklerini doldurmak için kapsayıcı ardından değerleri okuyun.

Bir nesne üzerinde özelliklerini ayarlamak için özellik belirtmek değer sonra çağırın **SetProperties** yöntemi.

Aşağıdaki kod örneğinde bir kapsayıcı oluşturur, ardından özellik değerlerini bazıları konsol penceresine yazar.

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create the service client object for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
await container.FetchAttributesAsync();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a>Meta veri alma ve ayarlama
Meta veriler blob veya kapsayıcı kaynak üzerinde bir veya daha fazla ad-değer çiftleri olarak belirtebilirsiniz. Meta veri ayarlamak için ad-değer çiftlerini eklemek **meta veri** kaynak koleksiyonu'ı çağırın **SetMetadata** değerleri hizmete kaydetmek için yöntem.

> [!NOTE]
> Meta verilerinizin adını C# tanımlayıcıları için adlandırma kuralları için uygun olmalıdır.
>
>

Aşağıdaki kod örneğinde bir kapsayıcıda meta verilerini ayarlar. Bir değer koleksiyonunun kullanılarak ayarlanır **Ekle** yöntemi. Başka bir değer örtük anahtar/değer sözdizimi kullanılarak yapılır. Her ikisi de geçerlidir.

```csharp
public static async Task AddContainerMetadataAsync(CloudBlobContainer container)
{
    // Add some metadata to the container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    // Set the container's metadata.
    await container.SetMetadataAsync();
}
```

Meta verilerini almak için çağrı **FetchAttributes** yöntemi blob veya doldurmak için kapsayıcı **meta veri** koleksiyonu, aşağıdaki örnekte gösterildiği gibi değerler ardından okuyun.

```csharp
public static async Task ListContainerMetadataAsync(CloudBlobContainer container)
{
    // Fetch container attributes in order to populate the container's properties and metadata.
    await container.FetchAttributesAsync();

    // Enumerate the container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* [.NET başvurusu için Azure Storage istemci kitaplığı](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [.NET NuGet paketi için Azure Storage istemci kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/)
