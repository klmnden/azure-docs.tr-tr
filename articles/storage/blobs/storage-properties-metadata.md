---
title: Ayarlayın ve nesne özellikleri ve Azure Depolama'da meta verileri alma | Microsoft Docs
description: Azure depolama, nesneler üzerinde özel meta verileri Store ayarlayın ve Sistem özellikleri almak.
services: storage
documentationcenter: ''
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 036f9006-273e-400b-844b-3329045e9e1f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2018
ms.author: tamram
ms.openlocfilehash: d2c95139d42f42e43e2fa6e587d5b1b13afdf1e9
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39112452"
---
# <a name="set-and-retrieve-properties-and-metadata"></a>Özellikler ile meta verileri ayarlama ve alma

Azure depolama desteği Sistem özellikleri ve kullanıcı tanımlı meta veriler, içerdikleri verilere ek olarak nesneleri. Bu makalede yönetme Sistem özellikleri anlatılmaktadır ve kullanıcı tanımlı meta veriler ile [.NET için Azure depolama istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/).

* **Sistem Özellikleri**: Sistem özellikleri her depolama kaynakta yok. Bunlardan bazıları okunabilir veya başkalarının salt okunur olsa ayarlayın. Perde bazı sistem özellikleri standart belirli HTTP üstbilgilerine karşılık gelir. Azure depolama istemci kitaplıkları sizin için bu özellikleri korur.

* **Kullanıcı tanımlı meta veriler**: kullanıcı tanımlı meta veriler için bir Azure depolama kaynağı belirttiğiniz bir veya daha fazla ad-değer çiftleri oluşur. Meta veri kaynağı olan ek değerleri depolamak için kullanabilirsiniz. Meta veri değerlerini kendi yalnızca amaçlıdır ve kaynak nasıl davranacağını etkilemez.

Depolama kaynak için özellik ve meta verileri değerlerini alma iki adımlı bir işlemdir. Bu değerleri okumak önce siz açıkça bunları çağırarak getirmelisiniz **FetchAttributes** veya **FetchAttributesAsync** yöntemi. Aradığınız varsa istisnadır **EXISTS** veya **ExistsAsync** kaynakta yöntemi. Bu yöntemi çağırdığınızda, Azure Storage'a uygun çağrı **FetchAttributes** kapsar yöntem çağrısı bir parçası olarak **EXISTS** yöntemi.

> [!IMPORTANT]
> Özellik veya meta veri değerlerini depolama kaynağı değil doldurulduğunu fark ederseniz, sonra kodunuzun çağrı yaptığını denetleyin **FetchAttributes** veya **FetchAttributesAsync** yöntemi.
>
> Meta veri adı/değer çiftleri yalnızca ASCII karakterleri içerebilir. Meta veri adı/değer çiftleri geçerli bir HTTP üst bilgileri ve bu nedenle tüm kısıtlamaları HTTP üst bilgilerini yöneten uyması gerekir. URL kodlaması veya adlarını ve değerlerini içeren ASCII olmayan karakterler için Base64 kodlaması kullanmanız önerilir.
>

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
* [Azure depolama istemci kitaplığı için .NET başvurusu](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [.NET NuGet paketi için Azure depolama istemci kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/)
