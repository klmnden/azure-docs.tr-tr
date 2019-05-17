---
title: Ayarlayın ve nesne özellikleri ve Azure Depolama'da meta verileri alma | Microsoft Docs
description: Azure depolama, nesneler üzerinde özel meta verileri Store ayarlayın ve Sistem özellikleri almak.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/03/2019
ms.author: tamram
ms.openlocfilehash: e8a85319a12f04a11e3914716d9ff84cdb6de8d8
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787871"
---
# <a name="set-and-retrieve-properties-and-metadata"></a>Özellikler ile meta verileri ayarlama ve alma

Azure depolama desteği Sistem özellikleri ve kullanıcı tanımlı meta veriler, içerdikleri verilere ek olarak nesneleri. Bu makalede yönetme Sistem özellikleri anlatılmaktadır ve kullanıcı tanımlı meta veriler ile [.NET için Azure depolama istemci Kitaplığı](/dotnet/api/overview/azure/storage/client).

* **Sistem Özellikleri**: Sistem özellikleri her depolama kaynakta yok. Bunlardan bazıları okunabilir veya başkalarının salt okunur olsa ayarlayın. Perde bazı sistem özellikleri standart belirli HTTP üstbilgilerine karşılık gelir. Azure depolama istemci kitaplıkları sizin için bu özellikleri korur.

* **Kullanıcı tanımlı meta veriler**: Kullanıcı tanımlı meta veriler için bir Azure depolama kaynağı belirttiğiniz bir veya daha fazla ad-değer çiftleri içerir. Meta veri kaynağı olan ek değerleri depolamak için kullanabilirsiniz. Meta veri değerlerini kendi yalnızca amaçlıdır ve kaynak nasıl davranacağını etkilemez.

Depolama kaynak için özellik ve meta verileri değerlerini alma iki adımlı bir işlemdir. Bu değerleri okumak önce siz açıkça bunları çağırarak getirmelisiniz **FetchAttributes** veya **FetchAttributesAsync** yöntemi. Aradığınız varsa istisnadır **EXISTS** veya **ExistsAsync** kaynakta yöntemi. Bu yöntemi çağırdığınızda, Azure Storage'a uygun çağrı **FetchAttributes** kapsar yöntem çağrısı bir parçası olarak **EXISTS** yöntemi.

> [!IMPORTANT]
> Özellik veya meta veri değerlerini depolama kaynağı değil doldurulduğunu fark ederseniz, sonra kodunuzun çağrı yaptığını denetleyin **FetchAttributes** veya **FetchAttributesAsync** yöntemi.
>
> Meta veri adı/değer çiftleri geçerli bir HTTP üst bilgileri ve bu nedenle tüm kısıtlamaları HTTP üst bilgilerini yöneten uymanız gerekir. Meta veri adları, geçerli HTTP üst bilgi adları olmalıdır ve geçerli C# tanımlayıcıları, yalnızca ASCII karakterler içerebilir ve olarak büyük küçük harf duyarsız olarak değerlendirilmelidir. ASCII olmayan karakterler içeren bir meta veri değer Base64 kodlamalı veya URL olarak kodlanmış olması gerekir.

## <a name="setting-and-retrieving-properties"></a>Ayar ve özellikleri alınıyor
Özellik değerlerini almak için arama **FetchAttributesAsync** yöntemi blob veya kapsayıcı özelliklerini doldurmak için değerleri sonra okuyun.

Bir nesne özellikleri ayarlamak için özelliği belirtin. değer, ardından arama **SetProperties** yöntemi.

Aşağıdaki kod örneği, bir kapsayıcı oluşturur, sonra bazı özellik değerleri için bir konsol penceresi yazar.

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
Meta veriler, blob veya kapsayıcı kaynak üzerinde bir veya daha fazla ad-değer çiftleri olarak belirtebilirsiniz. Ad-değer çiftleri kümesi meta verileri ekleyin **meta verileri** kaynak koleksiyonu daha sonra arama **SetMetadata** veya **SetMetadataAsync** değerlere kaydetmek için yöntemi hizmeti.

> [!NOTE]
> Meta verilerinizi adını C# tanımlayıcı adlandırma kurallarına uymalıdır.
>
>

Aşağıdaki kod örneği, bir kapsayıcı meta verilerini ayarlar. Bir değer koleksiyonun kullanılarak ayarlanır **Ekle** yöntemi. Örtük anahtar/değer söz dizimini kullanarak başka bir değer ayarlanır. Her ikisi de geçerlidir.

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

Meta verilerini almak için arama **FetchAttributes** veya **FetchAttributesAsync** yöntemi blob veya kapsayıcı doldurmak için **meta verileri** koleksiyonu, sonra okuyun Aşağıdaki örnekte gösterildiği gibi değerleri.

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
* [Azure depolama istemci kitaplığı için .NET başvurusu](/dotnet/api/?term=Microsoft.Azure.Storage)
* [.NET paketi için Azure Blob Depolama istemci kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Blob/)
* [.NET paketi için Azure kuyruk depolama istemci kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Queue/)
* [.NET paketi için Azure dosya depolama istemci kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.File/)

