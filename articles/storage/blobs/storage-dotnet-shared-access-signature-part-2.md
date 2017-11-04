---
title: "Oluşturma ve paylaşılan erişim imzası (SAS) ile Azure Blob storage kullanma | Microsoft Docs"
description: "Bu öğretici, Blob storage'ı kullanmak için paylaşılan erişim imzaları oluşturma ve istemci uygulamalarında kullanma gösterir."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 491e0b3c-76d4-4149-9a80-bbbd683b1f3e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: tamram
ms.openlocfilehash: 9dde12acde748c48b56f9f96ee772fca49954358
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Paylaşılan erişim imzası, bölüm 2: Oluşturma ve bir SAS Blob storage'ı kullanma

[Bölüm 1](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) keşfedilen Bu öğretici paylaşılan erişim imzası (SAS) ve bunları kullanmak için en iyi uygulamalar açıklanmıştır. Bölüm 2 oluşturmak ve ardından paylaşılan erişim imzaları Blob storage'ı nasıl kullanılacağını gösterir. Örnekler C# dilinde yazılmıştır ve .NET için Azure Storage istemci kitaplığı kullanın. Bu öğreticide örnekler:

* Üzerinde bir kapsayıcı paylaşılan erişim imzası oluşturma
* Blob üzerindeki paylaşılan erişim imzası oluşturma
* Bir kapsayıcının kaynakları imzalarını yönetmek için bir saklı erişim ilkesi oluşturun
* Bir istemci uygulamasında paylaşılan erişim imzaları test

## <a name="about-this-tutorial"></a>Bu öğretici hakkında
Bu öğreticide, oluşturma ve kapsayıcılar ve bloblar için paylaşılan erişim imzaları kullanma gösteren iki konsol uygulamaları oluşturun:

**Uygulama 1**: yönetimi uygulaması. Bir kapsayıcı ve bir blob için bir paylaşılan erişim imzası oluşturur. Depolama hesabı erişim tuşu kaynak kodunu içerir.

**Uygulama 2**: istemci uygulaması. İlk uygulama ile oluşturulan paylaşılan erişim imzaları kullanarak erişse kapsayıcı ve blob kaynakları. Yalnızca paylaşılan erişim imzalarını erişim kapsayıcısına ve blob kaynaklarını--kullanır mevcut *değil* depolama hesabının erişim anahtarı içerir.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>1. Kısım: paylaşılan erişim imzaları üretmek için bir konsol uygulaması oluşturun
İlk olarak, yüklü .NET için Azure Storage istemci kitaplığı olduğundan emin olun. Yükleyebileceğiniz [NuGet paketi](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet paketi") istemci kitaplığı için en güncel derlemelerini içeren. Bu, en son düzeltmeler olmasını sağlamaya yönelik önerilen yöntemdir. İstemci kitaplığının en son sürümünü bir parçası olarak indirebilirsiniz [.NET için Azure SDK](https://azure.microsoft.com/downloads/).

Visual Studio'da yeni bir Windows konsol uygulaması oluşturun ve adlandırın **GenerateSharedAccessSignatures**. Başvurular ekleyin [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) ve [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/) aşağıdaki yaklaşımlardan birini kullanarak:

* Kullanım [NuGet Paket Yöneticisi](https://docs.nuget.org/consume/installing-nuget) Visual Studio. Seçin **proje** > **NuGet paketlerini Yönet**, her paket için (Microsoft.WindowsAzure.ConfigurationManager ve WindowsAzure.Storage) çevrimiçi olarak arayın ve yükleyin.
* Alternatif olarak, bu derlemeler yüklemenizde Azure SDK'sının bulun ve bunları başvurular ekleyin:
  * Microsoft.WindowsAzure.Configuration.dll
  * Microsoft.WindowsAzure.Storage.dll

Program.cs dosyasının üst kısmında, aşağıdaki ekleyin **kullanarak** yönergeleri:

```csharp
using System.IO;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

Bir yapılandırma ayarı depolama hesabınıza işaret eden bir bağlantı dizesini içeren app.config dosyasını düzenleyin. App.config dosyasını buna benzer görünmelidir:

```xml
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
  </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
  </appSettings>
</configuration>
```

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Bir kapsayıcı için bir paylaşılan erişim imzası URI oluşturma
Başından itibaren üzerinde yeni bir kapsayıcı paylaşılan erişim imzası oluşturmak için bir yöntem ekleyin. Bu durumda, sona erme saati ve verir izinleri olduğunu gösteren bilgiler URI üzerinde taşıyan şekilde imza depolanmış erişim ilkesi ile ilişkili değil.

İlk olarak, kodu ekleyin **Main()** yöntemi depolama hesabınıza erişim için kimlik doğrulaması yapmak ve yeni bir kapsayıcı oluşturmak için:

```csharp
static void Main(string[] args)
{
    //Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container to use for the sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Insert calls to the methods created below here...

    //Require user input before closing the console window.
    Console.ReadLine();
}
```

Ardından, kapsayıcı paylaşılan erişim imzası oluşturur ve imza URI döndüren bir yöntem ekleyin:

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
    //Set the expiry time and permissions for the container.
    //In this case no start time is specified, so the shared access signature becomes valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Write;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

Sonuna şu satırları ekleyin **Main()** yöntemi çağırmadan önce **Console.ReadLine()**, çağırmak için **GetContainerSasUri()** ve imza URI yazmak için Konsol penceresinde:

```csharp
//Generate a SAS URI for the container, without a stored access policy.
Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
Console.WriteLine();
```

Derleme ve yeni kapsayıcı paylaşılan erişim imzası URI çıktısını almak için çalıştırın. URI aşağıdakine benzer olacaktır:

```
https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D
```

Kod çalıştırdıktan sonra oluşturduğunuz için kapsayıcı paylaşılan erişim imzası sonraki 24 saat için geçerli olur. İmza listesi BLOB kapsayıcısında ve yeni BLOB kapsayıcıya yazma için bir istemci izni verir.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Bir blob için bir paylaşılan erişim imzası URI oluşturma
Ardından, yeni blob kapsayıcı içinde oluşturmak ve paylaşılan erişim imzası oluşturmak için benzer bir kod yazın. URI'de başlangıç zamanı, bitiş saati ve izin bilgileri içerecek şekilde bu paylaşılan erişim imzası depolanmış erişim ilkesi ile ilişkili değil.

Yeni bir blob oluşturur ve bazı metin, yazar sonra paylaşılan erişim imzası oluşturur ve URI imza döndüren yeni bir yöntem ekleyin:

```csharp
static string GetBlobSasUri(CloudBlobContainer container)
{
    //Get a reference to a blob within the container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

    //Upload text to the blob. If the blob does not yet exist, it will be created.
    //If the blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible to clients via a shared access signature (SAS).";
    blob.UploadText(blobContent);

    //Set the expiry time and permissions for the blob.
    //In this case, the start time is specified as a few minutes in the past, to mitigate clock skew.
    //The shared access signature will be valid immediately.
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
    sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

    //Generate the shared access signature on the blob, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

Ekranın alt kısmındaki **Main()** yöntemini çağırmak için aşağıdaki satırları ekleyin **GetBlobSasUri()**, çağırmadan önce **Console.ReadLine()**ve paylaşılan erişim imzası yazma URI konsol penceresine:

```csharp
//Generate a SAS URI for a blob within the container, without a stored access policy.
Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
Console.WriteLine();
```

Derleme ve yeni blob paylaşılan erişim imzası URI çıktısını almak için çalıştırın. URI aşağıdakine benzer olacaktır:

```
https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D
```

### <a name="create-a-stored-access-policy-on-the-container"></a>Kapsayıcıda depolanmış erişim ilkesi oluşturma
Şimdi bir saklı erişim ilkesi kendisiyle ilişkilendirilmiş herhangi bir paylaşılan erişim imzaları kısıtlamalarını tanımlayacaksınız kapsayıcısı üzerinde oluşturalım.

Önceki örneklerde, biz başlangıç saati (örtük veya açık olarak), sona erme saati ve izinleri paylaşılan erişim imzası URI kendisini belirtilen. Aşağıdaki örneklerde, biz Bu saklı erişim ilkesi, paylaşılan erişim imzası değil, belirtin. Bunun yapılması paylaşılan erişim imzası vermeden bu kısıtlamaların değiştirmek için bize sağlar.

Bir veya daha fazla paylaşılan erişim imzası kısıtlamalar ve saklı erişim ilkesinde kalanı olması mümkündür. Ancak, yalnızca başlangıç zamanı, bitiş saati ve izinleri tek bir yerde veya diğer belirtebilirsiniz. Örneğin, paylaşılan erişim imzası üzerinde izinleri belirtmek ve ayrıca bunları depolanmış erişim ilkesine belirtin.

Bir saklı erişim ilkesi için bir kapsayıcı eklediğinizde, kapsayıcının var olan izinleri almak, yeni Erişim İlkesi Ekle ve kapsayıcının izinlerini ayarlayın.

Bir kapsayıcıda yeni bir saklı erişim ilkesi oluşturur ve ilkenin adını döndüren yeni bir yöntem ekleyin:

```csharp
static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
    string policyName)
{
    //Get the container's existing permissions.
    BlobContainerPermissions permissions = container.GetPermissions();

    //Create a new shared access policy and define its constraints.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    //Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    container.SetPermissions(permissions);
}
```

Ekranın alt kısmındaki **Main()** yöntemi çağırmadan önce **Console.ReadLine()**, aşağıdaki satırları ilk Temizle varolan tüm erişim ilkeleri ekleyin ve ardından çağrısı  **CreateSharedAccessPolicy()** yöntemi:

```csharp
//Clear any existing access policies on container.
BlobContainerPermissions perms = container.GetPermissions();
perms.SharedAccessPolicies.Clear();
container.SetPermissions(perms);

//Create a new access policy on the container, which may be optionally used to provide constraints for
//shared access signatures on the container and the blob.
string sharedAccessPolicyName = "tutorialpolicy";
CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);
```

Bir kapsayıcı erişim ilkeleri temizlediğinizde, gerekir ilk kapsayıcının var olan izinleri almak sonra izinleri temizleyin ve ardından yeniden izinlerini ayarlayın.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Paylaşılan erişim imzası bir erişim ilkesi kullanan kapsayıcısı üzerinde URI oluşturma
Ardından, daha önce ancak biz imza önceki örnekte oluşturduğumuz depolanmış erişim ilkesi ilişkilendirmek bu kez oluşturduğumuz kapsayıcısı için başka bir paylaşılan erişim imzası oluşturun.

Kapsayıcı için başka bir paylaşılan erişim imzası oluşturmak için yeni bir yöntem ekleyin:

```csharp
static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Generate the shared access signature on the container. In this case, all of the constraints for the
    //shared access signature are specified on the stored access policy.
    string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

    //Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

Ekranın alt kısmındaki **Main()** yöntemi çağırmadan önce **Console.ReadLine()**, çağırmak için aşağıdaki satırları ekleyin **GetContainerSasUriWithPolicy** yöntemi:

```csharp
//Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>URI üstünde bir erişim ilkesi kullanan Blob paylaşılan erişim imzası oluşturma
Son olarak, başka bir blob oluşturun ve saklı erişim ilkesi ile ilişkili bir paylaşılan erişim imzası oluşturmak için benzer bir yöntem ekleyin.

Bir blob oluşturun ve paylaşılan erişim imzası oluşturmak için yeni bir yöntem ekleyin:

```csharp
static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
{
    //Get a reference to a blob within the container.
    CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

    //Upload text to the blob. If the blob does not yet exist, it will be created.
    //If the blob does exist, its existing content will be overwritten.
    string blobContent = "This blob will be accessible to clients via a shared access signature. " +
    "A stored access policy defines the constraints for the signature.";
    MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    ms.Position = 0;
    using (ms)
    {
        blob.UploadFromStream(ms);
    }

    //Generate the shared access signature on the blob.
    string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

Ekranın alt kısmındaki **Main()** yöntemi çağırmadan önce **Console.ReadLine()**, çağırmak için aşağıdaki satırları ekleyin **GetBlobSasUriWithPolicy** yöntemi:

```csharp
//Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
Console.WriteLine();
```

**Main()** yöntemi şimdi şöyle görünmelidir tamamının. Paylaşılan erişim imzası URI'ler konsol penceresine yazma sonra kopyalayın ve bu öğreticinin ikinci bölümünde kullanmak için bir metin dosyası yapıştırmak için çalıştırın.

```csharp
static void Main(string[] args)
{
    //Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create the blob client object.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container to use for the sample code, and create it if it does not exist.
    CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
    container.CreateIfNotExists();

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

    Console.ReadLine();
}
```

GenerateSharedAccessSignatures konsol uygulamasını çalıştırdığınızda, aşağıdakine benzer bir çıktı göreceksiniz. Bu öğreticinin Kısım 2'de kullanmak paylaşılan erişim imzaları değil.

```
Container SAS URI: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=pFlEZD%2F6sJTNLxD%2FQ26Hh85j%2FzYPxZav6mP1KJwnvJE%3D&se=2017-05-16T16%3A16%3A47Z&sp=wl

Blob SAS URI: https://storagesample.blob.core.windows.net/sascontainer/sasblob.txt?sv=2016-05-31&sr=b&sig=%2FiBWAZbXESzCMvRcm7JwJBK0gT0BtPSWEq4pRwmlBRI%3D&st=2017-05-15T16%3A11%3A48Z&se=2017-05-16T16%3A16%3A48Z&sp=rw

Container SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&si=tutorialpolicy&sig=aMb6rKDvvpfiGVsZI2rCmyUra6ZPpq%2BZ%2FLyTgAeec%2Bk%3D

Blob SAS URI using stored access policy: https://storagesample.blob.core.windows.net/sascontainer/sasblobpolicy.txt?sv=2016-05-31&sr=b&si=tutorialpolicy&sig=%2FkTWkT23SS45%2FoF4bK2mqXkN%2BPKs%2FyHuzkfQ4GFoZVU%3D
```

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>2. Kısım: paylaşılan erişim imzaları test etmek için bir konsol uygulaması oluşturun
Önceki örneklerde oluşturulan paylaşılan erişim imzaları sınamak için biz ve blob kapsayıcısı üzerinde işlem gerçekleştirmeye imzalarını kullanır ikinci bir konsol uygulaması oluşturun.

> [!NOTE]
> Öğreticinin ilk kısmı tamamlandı beri 24 saatten fazla geçmişse, üretilen imzalar artık geçerli olmayacak. Bu durumda, öğreticinin ikinci bölümünde kullanmak için yeni paylaşılan erişim imzaları üretmek için ilk konsol uygulamasındaki kod çalıştırmanız gerekir.
>

Visual Studio'da yeni bir Windows konsol uygulaması oluşturun ve adlandırın **ConsumeSharedAccessSignatures**. Başvurular ekleyin [Microsoft.WindowsAzure.ConfigurationManager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager) ve [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/), daha önce yaptığınız gibi.

Program.cs dosyasının üst kısmında, aşağıdaki ekleyin **kullanarak** yönergeleri:

```csharp
using System.IO;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

Gövdesinde **Main()** yöntemi, değerlerine öğreticinin 1 bölümünde oluşturulan paylaşılan erişim imzaları değiştirerek aşağıdaki dize sabitleri ekleyin.

```csharp
static void Main(string[] args)
{
    const string containerSAS = "<your container SAS>";
    const string blobSAS = "<your blob SAS>";
    const string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    const string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
}
```

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Paylaşılan erişim imzası kullanarak kapsayıcı işlemleri denemek için bir yöntem ekleyin
Ardından, bazı kapsayıcı işlemleri için kapsayıcı paylaşılan erişim imzası kullanarak testleri bir yöntem ekleyin. Paylaşılan erişim imzası, tek başına imza tabanlı kapsayıcıya erişimi kimlik doğrulaması kapsayıcıya başvuru döndürmek için kullanılır.

Aşağıdaki yöntemi ekleyin:

```csharp
static void UseContainerSAS(string sas)
{
    //Try performing container operations with the SAS provided.

    //Return a reference to the container using the SAS URI.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

    //Create a list to store blob URIs returned by a listing operation on the container.
    List<ICloudBlob> blobList = new List<ICloudBlob>();

    //Write operation: write a new blob to the container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
        string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
        blob.UploadText(blobContent);

        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //List operation: List the blobs in the container.
    try
    {
        foreach (ICloudBlob blob in container.ListBlobs())
        {
            blobList.Add(blob);
        }
        Console.WriteLine("List operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("List operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Get a reference to one of the blobs in the container and read it.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        MemoryStream msRead = new MemoryStream();
        msRead.Position = 0;
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            Console.WriteLine(msRead.Length);
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    Console.WriteLine();

    //Delete operation: Delete a blob in the container.
    try
    {
        CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

Güncelleştirme **Main()** çağrılacak yöntem **UseContainerSAS()** ikisini de kapsayıcısında oluşturduğunuz paylaşılan erişim imzaları:

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call the test methods with the shared access signatures created on the container, with and without the access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    Console.ReadLine();
}
```

### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Paylaşılan erişim imzası kullanarak blob işlemleri denemek için bir yöntem ekleyin
Son olarak, bir paylaşılan erişim imzası blob üzerindeki kullanarak bazı blobu işlemleri testleri bir yöntem ekleyin. Bu durumda, biz Oluşturucu kullanın **CloudBlockBlob(String)**, blob bir başvuru döndürmek için paylaşılan erişim imzası geçen. Diğer kimlik doğrulaması gerekli değildir; tek başına imza dayanır.

Aşağıdaki yöntemi ekleyin:

```csharp
static void UseBlobSAS(string sas)
{
    //Try performing blob operations using the SAS provided.

    //Return a reference to the blob using the SAS URI.
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

    //Write operation: Write a new blob to the container.
    try
    {
        string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
        MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        msWrite.Position = 0;
        using (msWrite)
        {
            blob.UploadFromStream(msWrite);
        }
        Console.WriteLine("Write operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Write operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Read operation: Read the contents of the blob.
    try
    {
        MemoryStream msRead = new MemoryStream();
        using (msRead)
        {
            blob.DownloadToStream(msRead);
            msRead.Position = 0;
            using (StreamReader reader = new StreamReader(msRead, true))
            {
                string line;
                while ((line = reader.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
        Console.WriteLine("Read operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Read operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }

    //Delete operation: Delete the blob.
    try
    {
        blob.Delete();
        Console.WriteLine("Delete operation succeeded for SAS " + sas);
        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine("Delete operation failed for SAS " + sas);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
}
```

Güncelleştirme **Main()** çağrılacak yöntem **UseBlobSAS()** hem blob üzerindeki oluşturduğunuz paylaşılan erişim imzaları:

```csharp
static void Main(string[] args)
{
    string containerSAS = "<your container SAS>";
    string blobSAS = "<your blob SAS>";
    string containerSASWithAccessPolicy = "<your container SAS with access policy>";
    string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

    //Call the test methods with the shared access signatures created on the container, with and without the access policy.
    UseContainerSAS(containerSAS);
    UseContainerSAS(containerSASWithAccessPolicy);

    //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
    UseBlobSAS(blobSAS);
    UseBlobSAS(blobSASWithAccessPolicy);

    Console.ReadLine();
}
```

Konsol uygulamasını çalıştırın ve hangi işlemleri hangi imzalar için izin verilen görmek için çıktıyı inceleyin. Çıkış konsol penceresinde aşağıdakine benzer görünecektir:

```
Write operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

List operation succeeded for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl

Read operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: The remote server returned an error: (403) Forbidden.

Delete operation failed for SAS https://storagesample.blob.core.windows.net/sascontainer?sv=2016-05-31&sr=c&sig=32EaQGuFyDMb3yOAey3wq%2B%2FLwgPQxAgSo7UhzLdyIDU%3D&se=2017-05-16T15%3A41%3A20Z&sp=wl
Additional error information: The remote server returned an error: (403) Forbidden.

...
```

## <a name="next-steps"></a>Sonraki Adımlar

* [Paylaşılan erişim imzası, bölüm 1: SAS modelini anlama](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](storage-manage-access-to-resources.md)
* [Paylaşılan erişim imzası (REST API) ile erişim için temsilci seçme](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Tablo ve kuyruk SAS Tanıtımı](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
