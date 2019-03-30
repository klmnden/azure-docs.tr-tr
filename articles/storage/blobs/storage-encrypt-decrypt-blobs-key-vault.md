---
title: "Öğretici: Şifreleme ve şifre çözme Azure anahtar Kasası'nı kullanarak Azure Depolama'daki blobları | Microsoft Docs"
description: Şifreleme ve şifre çözme için Azure Key Vault ile Microsoft Azure depolama istemci tarafı şifreleme kullanarak blob nasıl.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: c7a185e1c7f271cdca0c688ce7838f6390594da5
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650419"
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Öğretici: Şifreleme ve şifre çözme Azure anahtar Kasası'nı kullanarak Microsoft Azure depolama BLOB'ları

## <a name="introduction"></a>Giriş
Bu öğreticide nasıl yapılacağını kapsayan, Azure Key Vault ile istemci tarafı depolama şifrelemesi kullanın. Bu, şifreleme ve şifre çözme teknolojiler kullanarak bir konsol uygulamasında bir blob konusunda yol göstermektedir.

**Tahmini tamamlanma süresi:** 20 dakika

Azure Key Vault hakkında genel bilgi için bkz. [Azure anahtar kasası nedir?](../../key-vault/key-vault-whatis.md).

Azure depolama için istemci tarafı şifreleme hakkında genel bilgi için bkz. [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Bir Azure depolama hesabı
* Visual Studio 2013 veya üzeri
* Azure PowerShell

## <a name="overview-of-client-side-encryption"></a>İstemci Tarafı Şifrelemesi'ne genel bakış

Azure depolama için istemci tarafı şifrelemesi genel bakış için bkz. [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

Aşağıda, istemci tarafı şifreleme nasıl çalışır hakkında kısa bir açıklaması verilmiştir:

1. Azure depolama istemci SDK'sı, bir kerelik kullanım simetrik anahtarı olan bir içerik şifreleme anahtarı (CEK) oluşturur.
2. Müşteri verileri, bu CEK kullanılarak şifrelenir.
3. CEK (anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir) paketlenir. KEK anahtar bir tanımlayıcıyla tanımlanır ve asimetrik anahtar çifti veya bir simetrik anahtar ve yerel olarak yönetilebilir veya Azure anahtar Kasası'nda depolanır. Depolama istemcisi KEK hiçbir zaman erişebilir. Yeni Key Vault tarafından sağlanan anahtar sarmalama algoritması da çağırır. Müşteriler, özel sağlayıcılar için kaydırma/istediğinde çözülme anahtarı kullanmayı da tercih edebilirsiniz.
4. Şifrelenmiş veriler, ardından Azure depolama hizmetine yüklenir.

## <a name="set-up-your-azure-key-vault"></a>Azure anahtar kasası ayarlama

Öğreticide gösterilen aşağıdaki adımları gerçekleştirmeniz gereken bu öğreticiyle devam edebilmek için [Azure anahtar kasası nedir?](../../key-vault/key-vault-overview.md):

* Bir anahtar kasası oluşturma.
* Bir anahtar veya gizli anahtar Kasası'na ekleyin.
* Bir uygulamayı Azure Active Directory ile kaydedin.
* Anahtar veya gizli anahtarı kullanması için uygulamayı yetkilendirme.

Azure Active Directory ile bir uygulama kaydı sırasında oluşturulan ClientSecret ve ClientID not edin.

Her iki anahtar, anahtar Kasası'nda oluşturun. Biz, bu öğreticinin geri kalanını için şu adı kullandığınızı varsayar: ContosoKeyVault ve TestRSAKey1.

## <a name="create-a-console-application-with-packages-and-appsettings"></a>Paketler ve AppSettings ile bir konsol uygulaması oluşturun

Visual Studio'da yeni bir konsol uygulaması oluşturun.

Paket Yöneticisi Konsolu'nda gerekli nuget paketlerini ekleyin.

```powershell
Install-Package WindowsAzure.Storage
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

AppSettings için App.Config ekleyin.

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

Aşağıdaki `using` yönergeleri ve projeye bir başvuru System.Configuration ekleyin emin olun.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Configuration;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.Azure.KeyVault;
using System.Threading;        
using System.IO;
```

## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Konsol uygulamanıza bir belirteç almak için yöntem ekleme

Aşağıdaki yöntem, anahtar kasanıza erişim için kimlik doğrulaması gerekli Key Vault sınıflar tarafından kullanılır.

```csharp
private async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(
        ConfigurationManager.AppSettings["clientId"],
        ConfigurationManager.AppSettings["clientSecret"]);
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

## <a name="access-storage-and-key-vault-in-your-program"></a>Programınızda, depolama ve anahtar kasası erişim

Main işlevi içinde aşağıdaki kodu ekleyin.

```csharp
// This is standard code to interact with Blob storage.
StorageCredentials creds = new StorageCredentials(
    ConfigurationManager.AppSettings["accountName"],
       ConfigurationManager.AppSettings["accountKey"]);
CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
CloudBlobClient client = account.CreateCloudBlobClient();
CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
contain.CreateIfNotExists();

// The Resolver object is used to interact with Key Vault for Azure Storage.
// This is where the GetToken method from above is used.
KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);
```

> [!NOTE]
> Anahtar kasası nesne modelleri
> 
> Dikkat edilmesi gereken aslında iki anahtar kasası nesne modeli olduğunu anlamak önemlidir: bir REST API (KeyVault ad alanı) dayalıdır ve diğer istemci tarafı şifreleme bir uzantısıdır.
> 
> Anahtar kasası istemci REST API ile etkileşime geçer ve JSON Web anahtarları ve gizli anahtarları için anahtar Kasası'nda bulunan nesnelerin iki tür anlar.
> 
> Anahtar kasası uzantıları Azure depolama istemci tarafı şifreleme için özel olarak oluşturulmuş gibi görünüyor sınıflardır. Anahtarları (IKey) ve anahtar çözümleyici kavramını temel sınıflar için bir arabirim içerirler. Bilmeniz gereken, IKey'nün iki uygulamasından vardır: RSAKey ve SymmetricKey. Artık bir anahtar Kasası'nda bulunan öğeleri ile çakıştığı için meydana, ancak bu noktada (anahtar ve gizli anahtar kasası istemci tarafından alınan IKey uygulamak için) bağımsız sınıfları olur.
> 
> 

## <a name="encrypt-blob-and-upload"></a>BLOB şifreleme ve karşıya yükleme

Bir blobu şifreleme ve Azure depolama hesabınıza yüklemek için aşağıdaki kodu ekleyin. **ResolveKeyAsync** yöntemi bir ikey değerini döndürür.

```csharp
// Retrieve the key that you created previously.
// The IKey that is returned here is an RsaKey.
// Remember that we used the names contosokeyvault and testrsakey1.
var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();

// Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Reference a block blob.
CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

// Upload using the UploadFromStream method.
using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
    blob.UploadFromStream(stream, stream.Length, null, options, null);
```

> [!NOTE]
> BlobEncryptionPolicy Oluşturucusu bakarsanız, bir anahtar ve/veya bir çözümleyici kabul edebilirsiniz olduğunu görürsünüz. Şu anda mevcut sağa bir çözümleyici şifreleme için kullanamazsınız, çünkü şimdi varsayılan anahtar destekleyen dikkat edin.
> 
> 

## <a name="decrypt-blob-and-download"></a>Blob şifresini çözmek ve indirin

Şifre çözme gerçekten ne zaman çözümleyici sınıflarını kullanarak mantıklı olur. Sebep anahtarı almak ve blob ile anahtar arasındaki ilişkiyi unutmayın. Bu nedenle şifreleme için kullanılan anahtarı kimliği blob meta ile ilişkilidir. Yalnızca anahtar, anahtar Kasası'nda kalmasından emin olmak gerekir.   

Özel anahtarı bir RSA anahtarı anahtar Kasası'nda kalır. böylece gerçekleşmesi şifre çözme için blob meta verilerini içeren CEK şifrelenmiş anahtarı şifre çözme için anahtar Kasası'na gönderilir.

Yüklediğiniz blob şifresini çözmek için aşağıdakileri ekleyin.

```csharp
// In this case, we will not pass a key and only pass the resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> Birkaç anahtar yönetimi dahil olmak üzere kolaylaştırmak için Çözümleyicileri diğer tür vardır: AggregateKeyResolver ve CachingKeyResolver.
> 
> 

## <a name="use-key-vault-secrets"></a>Key Vault gizli dizileri kullanma

Gizli dizi aslında bir simetrik anahtar olduğundan bir gizli dizi istemci tarafı şifreleme ile kullanılacak şekilde SymmetricKey sınıftır. Ancak, yukarıda belirtildiği gibi bir gizli anahtar Kasası'nda tam olarak bir SymmetricKey eşleşmiyor. Anlamanız gereken bazı noktalar vardır:

* Bir SymmetricKey anahtarı sabit uzunlukta olması gerekir: 128, 192, 256, 384 veya 512 bit.
* Bir SymmetricKey anahtarında, Base64 olarak kodlanmış olmalıdır.
* Bir SymmetricKey kullanılacak bir Key Vault gizli anahtar Kasası'nda bir içerik türü "application/octet-stream" olmalıdır.

PowerShell'de Key vault'ta bir SymmetricKey kullanılabilir bir gizli dizi oluşturma örneği aşağıdadır.
Lütfen $key, sabit kodlanmış değeri yalnızca Tanıtım amaçlı olduğuna dikkat edin. Kendi kodunuzda bu anahtarı oluşturmak isteyebilirsiniz.

```csharp
// Here we are making a 128-bit key so we have 16 characters.
//     The characters are in the ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute the VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

Konsol uygulamanızı olarak aynı çağrısından önce bu gizli bir SymmetricKey olarak almak için kullanabilirsiniz.

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
İşte bu kadar. Keyfini çıkarın!

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Azure depolama C# ile kullanma hakkında daha fazla bilgi için bkz. [.NET için Microsoft Azure depolama istemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Blob REST API'si hakkında daha fazla bilgi için bkz. [Blob hizmeti REST API'si](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Microsoft Azure depolama üzerinde en son bilgiler için Git [Microsoft Azure depolama ekibi blogu](https://blogs.msdn.com/b/windowsazurestorage/).