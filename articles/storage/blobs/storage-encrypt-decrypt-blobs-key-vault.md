---
title: 'Öğretici: Şifrelemek ve Azure anahtar kasası kullanarak Azure Storage blobları şifresini | Microsoft Docs'
description: Şifrelemek ve şifresini çözmek için Azure anahtar kasası ile Microsoft Azure Storage istemci tarafı şifreleme kullanarak bir blob nasıl.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: adhurwit
ms.openlocfilehash: 405ccb44c9daf8d555946e6c68ef318ed2b82505
ms.sourcegitcommit: a0d2423f1f277516ab2a15fe26afbc3db2f66e33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
ms.locfileid: "27815822"
---
# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Öğretici: Şifrelemek ve şifresini Azure anahtar kasası kullanılarak Microsoft Azure Storage blobları
## <a name="introduction"></a>Giriş
Bu öğretici nasıl kapsayan Azure anahtar kasası ile istemci-tarafı depolama şifreleme kullanın. Bu, şifrelemek ve bu teknolojilerden bir konsol uygulamasında bir blob şifresini çözmek nasıl açıklanmaktadır.

**Tahmini tamamlanma süresi:** 20 dakika

Azure anahtar kasası hakkında genel bilgi için bkz: [Azure anahtar kasası nedir?](../../key-vault/key-vault-whatis.md).

Azure Storage istemci tarafı şifreleme hakkında genel bilgi için bkz: [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Bir Azure Storage hesabı
* Visual Studio 2013 veya üzeri
* Azure PowerShell

## <a name="overview-of-client-side-encryption"></a>İstemci tarafı şifreleme genel bakış
Azure Storage istemci tarafı şifreleme genel bakış için bkz: [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

Aşağıda, istemci tarafı şifreleme çalışma biçimine kısa bir açıklaması verilmiştir:

1. Azure Storage istemci SDK simetrik anahtar bir kerelik kullan bir içerik şifreleme anahtarı (CEK) oluşturur.
2. Müşteri verileri bu CEK kullanılarak şifrelenir.
3. CEK (anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir) paketlenir. KEK anahtar bir tanımlayıcıyla tanımlanır ve asimetrik anahtar çifti ya da bir simetrik anahtar olması ve yerel olarak yönetilebilir veya Azure anahtar Kasası ' depolanır. Depolama istemcisi KEK hiçbir zaman erişebilir. Yalnızca, anahtar kasası tarafından sağlanan anahtar kaydırma algoritması çağırır. Müşteriler için anahtar kaydırma/bunlar istiyorsanız açmak özel sağlayıcılar kullanmayı seçebilirsiniz.
4. Şifrelenmiş veriler daha sonra Azure Storage hizmetine yüklenir.

## <a name="set-up-your-azure-key-vault"></a>Azure anahtar kasanızı ayarlayın
Öğreticide özetlenen aşağıdaki adımları gerçekleştirmeniz gereken Bu öğretici ile devam etmek için [Azure anahtar kasası ile çalışmaya başlama](../../key-vault/key-vault-get-started.md):

* Bir anahtar kasası oluşturun.
* Bir anahtar veya gizli anahtar Kasası'na ekleyin.
* Bir uygulamayı Azure Active Directory ile kaydedin.
* Anahtar veya gizli kullanmak için uygulamayı yetkilendirin.

İstemci kimliği ve bir uygulamayı Azure Active Directory'ye kaydedilirken oluşturulan ClientSecret not edin.

Her iki anahtarı anahtar kasasını oluşturun. Aşağıdaki adları kullanılan öğreticinin geri kalanını için varsayıyoruz: ContosoKeyVault ve TestRSAKey1.

## <a name="create-a-console-application-with-packages-and-appsettings"></a>Paketler ve AppSettings bir konsol uygulaması oluşturun
Visual Studio'da yeni bir konsol uygulaması oluşturun.

Gerekli nuget paketleri Paket Yöneticisi konsolunda ekleyin.

```
Install-Package WindowsAzure.Storage

// This is the latest stable release for ADAL.
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

AppSettings App.Config ekleyin.

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

Aşağıdakileri ekleyin `using` yönergeleri ve projeye System.Configuration başvuru eklemek emin olun.

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

## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Konsol uygulamanız için bir belirteç almak üzere bir yöntem ekleyin
Aşağıdaki yöntem, anahtar kasanızı erişim için kimlik doğrulaması gerekli anahtar kasası sınıflar tarafından kullanılır.

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

## <a name="access-storage-and-key-vault-in-your-program"></a>Depolama ve anahtar kasası programınıza erişim
Main işlevi aşağıdaki kodu ekleyin.

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
> Dikkat edilmesi gereken gerçekte iki anahtar kasası nesne modeli olduğunu anlamak önemlidir: bir REST API (KeyVault ad alanı) dayanır ve diğer istemci tarafı şifreleme uzantısıdır.
> 
> Anahtar kasası istemci REST API'si ile etkileşim kurar ve JSON Web anahtarları ve gizli anahtarları için anahtar kasasına içerdiği şeyler iki tür anlar.
> 
> Anahtar kasası uzantıları Azure Storage istemci tarafı şifreleme için özel olarak oluşturulmuş görünen sınıflarıdır. Anahtarları (IKey) ve anahtar çözümleyici kavramını esas sınıfları için bir arabirimi içerir. Bilmeniz gereken IKey iki uygulamaları vardır: RSAKey ve SymmetricKey. Artık bir anahtar kasası içerdiği şeyler ile çakıştığı için gerçekleşmeden, ancak bu noktada (anahtarı ve gizli anahtar kasası istemci tarafından alınan IKey uygulamak için) bağımsız sınıfları etmektedir.
> 
> 

## <a name="encrypt-blob-and-upload"></a>BLOB şifrelemek ve karşıya yükleme
Bir blob şifrelemek ve Azure depolama hesabınıza yüklemek için aşağıdaki kodu ekleyin. **ResolveKeyAsync** yöntemi bir IKey döndürür.

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
> BlobEncryptionPolicy Oluşturucusu bakarsanız, bu bir anahtar ve/veya bir çözümleyici alabilen görürsünüz. Şu anda mevcut sağ çözümleyici için şifreleme kullanamazsınız, çünkü şimdi varsayılan anahtar destekleyen unutmayın.
> 
> 

## <a name="decrypt-blob-and-download"></a>Blob şifresini çözmek ve indirme
Şifre çözme, aslında çözümleyici sınıflarını kullanarak yaptığınızda algılama adıdır. Bu yüzden anahtarı almak ve anahtar blob arasındaki ilişkiyi unutmayın için bir sebep şifreleme için kullanılan anahtar kimliği blob meta verilerinde ilişkilendirilir. Yalnızca anahtar anahtar kasasına olmaya devam ettiğinden emin olmak gerekir.   

Özel anahtar bir RSA anahtarı, anahtar kasasına kalır şekilde gerçekleşmesi şifre çözme için CEK içeren blob meta verilerden şifrelenmiş anahtar şifre çözme için anahtar Kasası'na gönderilir.

Yeni karşıya yüklediğiniz blob şifresini çözmek için aşağıdakileri ekleyin.

```csharp
// In this case, we will not pass a key and only pass the resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> Birkaç anahtar yönetimi, dahil olmak üzere kolaylaştırmak için çözümleyiciler diğer tür vardır: AggregateKeyResolver ve CachingKeyResolver.
> 
> 

## <a name="use-key-vault-secrets"></a>Anahtar kasası gizlilikleri kullanın
Aslında bir simetrik anahtar gizli olduğu için bir gizli anahtar istemci tarafı şifreleme ile kullanılacak şekilde SymmetricKey sınıftır. Ancak, yukarıda belirtildiği gibi anahtar kasasında bir gizlilik SymmetricKey için tam olarak eşleşmiyor. Anlamak için birkaç şey vardır:

* Bir SymmetricKey anahtarında sabit uzunlukta olması gerekir: 128, 192, 256, 384 veya 512 bit.
* Bir SymmetricKey anahtarında Base64 ile kodlanmış olmalıdır.
* SymmetricKey kullanılacak bir anahtar kasası gizli anahtarı kasaya "application/octet-stream" içerik türü olmalıdır.

İşte bir örnek SymmetricKey kullanılabilir anahtar kasasında bir gizli anahtar oluşturma PowerShell'de.
Lütfen $key, sabit kodlanmış değeri yalnızca Tanıtım amaçlı olduğuna dikkat edin. Kendi kodunuzu bu anahtarı oluşturmak istersiniz.

```csharp
// Here we are making a 128-bit key so we have 16 characters.
//     The characters are in the ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute the VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

Konsol uygulamanızın bu gizlilik bir SymmetricKey olarak almak için önce araması olarak kullanabilirsiniz.

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```
Bu kadar. Keyfini çıkarın!

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure depolama C# ile kullanma hakkında daha fazla bilgi için bkz: [.NET için Microsoft Azure Storage istemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Blob REST API'si hakkında daha fazla bilgi için bkz: [Blob hizmeti REST API'si](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Microsoft Azure depolama en son bilgiler için Git [Microsoft Azure depolama ekibi blogu](http://blogs.msdn.com/b/windowsazurestorage/).
