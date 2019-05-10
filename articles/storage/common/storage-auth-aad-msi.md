---
title: Azure kaynakları - Azure depolama BLOB'ları ve kuyrukları yönetilen kimliklerle için erişimi kimlik doğrulaması | Microsoft Docs
description: Azure Blob ve kuyruk depolama, Azure kaynakları için yönetilen kimliklerle Azure Active Directory kimlik doğrulamasını destekler. Azure kaynakları için yönetilen kimlikleri, Azure sanal makineleri, işlev uygulamaları, sanal makine ölçek kümeleri ve diğerleri çalışan uygulamalardan erişim BLOB'lar ve Kuyruklar için kimlik doğrulaması için kullanabilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 4245c44ceaf907512187d7db4a9d6f087a855f70
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507888"
---
# <a name="authenticate-access-to-blobs-and-queues-with-azure-active-directory-and-managed-identities-for-azure-resources"></a>Azure kaynakları için erişim BLOB'lar ve Kuyruklar ile Azure Active Directory ve yönetilen kimlikleri için kimlik doğrulaması

Azure Blob ve kuyruk depolama ile Azure Active Directory (Azure AD) kimlik doğrulaması desteği [kimliklerini Azure kaynakları için yönetilen](../../active-directory/managed-identities-azure-resources/overview.md). Kimlikler Azure kaynaklarına erişimini BLOB yetkilendirebilirsiniz için ve Azure sanal makineleri (VM'ler), işlev uygulamaları, sanal makine ölçek kümeleri ve diğer Hizmetleri çalışan uygulamalardan Azure AD kimlik bilgilerini kullanarak kuyruk verileri yönetilen. Azure AD kimlik doğrulaması ile birlikte Azure kaynakları için yönetilen kimlik kullanarak, bulut uygulamalarınız ile kimlik bilgilerini depolama önleyebilirsiniz.  

Bu makalede, bir Azure VM'den yönetilen bir kimlik ile blob veya kuyruğa verilere erişim yetkisi gösterilmektedir. 

## <a name="enable-managed-identities-on-a-vm"></a>Bir VM'de yönetilen kimlikleri etkinleştir

BLOB'lar ve Kuyruklar makinenizden erişim yetkisi vermek için Azure kaynakları için yönetilen kimlikleri kullanabilmeniz için önce VM üzerindeki Azure kaynakları için önce yönetilen kimlikleri etkinleştirmeniz gerekir. Azure kaynakları için yönetilen kimlikleri etkinleştirme konusunda bilgi edinmek için şu makalelerden birine bakın:

- [Azure portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure SDK'ları](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="grant-permissions-to-an-azure-ad-managed-identity"></a>Bir yönetilen Azure AD kimlik izinler

Azure Storage uygulamanızı içinde yönetilen bir kimlik gelen Blob veya kuyruk hizmetine bir istek yetkilendirmek için önce yönetilen kimliğe için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure depolama, blob ve kuyruk veriler için izinleri kapsayacak RBAC rolleri tanımlar. RBAC rolü için bir yönetilen kimlik atandığında, yönetilen kimlik blob veya sıra verilerinize uygun kapsamda bu izinleri verilir. 

RBAC rollerini atama hakkında daha fazla bilgi için aşağıdaki makalelerden birine bakın:

- [Azure blob ve kuyruk verilere RBAC ile Azure portalında erişim izni ver](storage-auth-aad-rbac-portal.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilere erişim izni ver](storage-auth-aad-rbac-cli.md)
- [PowerShell ile RBAC ile Azure blob ve kuyruk verilere erişim izni ver](storage-auth-aad-rbac-powershell.md)

## <a name="authorize-with-a-managed-identity-access-token"></a>Bir yönetilen kimlik erişim belirteciyle Yetkilendir

Blob ve kuyruk depolama ile bir yönetilen kimlik istekler yetkilendirmek için uygulama veya betiğin bir OAuth belirteci edinmeniz gerekir. [Microsoft Azure uygulama kimlik doğrulamasını](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) .NET (Önizleme) için istemci kitaplığı alınırken ve kodunuzdan bir belirteç yenileme işlemini basitleştirir.

Uygulama kimlik doğrulaması istemci kitaplığı, kimlik doğrulama otomatik olarak yönetir. Kitaplığı, yerel geliştirme sırasında kimlik doğrulaması yapmak için Geliştirici kimlik bilgilerini kullanır. Azure AD kimlik bilgileri oluşturun veya paylaşım geliştiricileri arasında kimlik bilgileri gerekmez çünkü, yerel geliştirme sırasında Geliştirici kimlik bilgilerinizi kullanarak daha güvenlidir. Çözüm, daha sonra Azure'a dağıtıldığında kitaplığı uygulama kimlik bilgilerini kullanarak otomatik olarak geçer.

Uygulama kimlik doğrulaması Kitaplığı'nda bir Azure Storage uygulamasını kullanmak için en son Önizleme paketinden yüklemek [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication), en son sürümünü yanı sıra [.NET için Azure depolama istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/). Aşağıdaki **kullanarak** kodunuzda ifadeleri:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.Auth;
```

Uygulama kimlik doğrulama kitaplığını sağlar **AzureServiceTokenProvider** sınıfı. Bu sınıfın bir örneği, bir belirteç alır ve ardından süresi dolmadan önce belirtecini yeniler. bir geri çağırma için geçirilebilir.

Aşağıdaki örnek bir belirteç alır ve yeni bir blob oluşturmak için kullanır, sonra bir blobun okunabilmesi için aynı belirteci kullanır.

```csharp
const string blobName = "https://storagesamples.blob.core.windows.net/sample-container/blob1.txt";

// Get the initial access token and the interval at which to refresh it.
AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
var tokenAndFrequency = TokenRenewerAsync(azureServiceTokenProvider, 
                                            CancellationToken.None).GetAwaiter().GetResult();

// Create storage credentials using the initial token, and connect the callback function 
// to renew the token just before it expires
TokenCredential tokenCredential = new TokenCredential(tokenAndFrequency.Token, 
                                                        TokenRenewerAsync,
                                                        azureServiceTokenProvider, 
                                                        tokenAndFrequency.Frequency.Value);

StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a blob using the storage credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri(blobName), 
                                            storageCredentials);

// Upload text to the blob.
blob.UploadTextAsync(string.Format("This is a blob named {0}", blob.Name));

// Continue to make requests against Azure Storage. 
// The token is automatically refreshed as needed in the background.
do
{
    // Read blob contents
    Console.WriteLine("Time accessed: {0} Blob Content: {1}", 
                        DateTimeOffset.UtcNow, 
                        blob.DownloadTextAsync().Result);

    // Sleep for ten seconds, then read the contents of the blob again.
    Thread.Sleep(TimeSpan.FromSeconds(10));
} while (true);
```

Geri çağırma yöntemi, belirtecin süre sonu zamanı denetler ve gerektiği şekilde yenilenir:

```csharp
private static async Task<NewTokenAndFrequency> TokenRenewerAsync(Object state, CancellationToken cancellationToken)
{
    // Specify the resource ID for requesting Azure AD tokens for Azure Storage.
    const string StorageResource = "https://storage.azure.com/";  

    // Use the same token provider to request a new token.
    var authResult = await ((AzureServiceTokenProvider)state).GetAuthenticationResultAsync(StorageResource);

    // Renew the token 5 minutes before it expires.
    var next = (authResult.ExpiresOn - DateTimeOffset.UtcNow) - TimeSpan.FromMinutes(5);
    if (next.Ticks < 0)
    {
        next = default(TimeSpan);
        Console.WriteLine("Renewing token...");
    }

    // Return the new token and the next refresh time.
    return new NewTokenAndFrequency(authResult.AccessToken, next);
}
```

Uygulama kimlik doğrulaması Kitaplığı hakkında daha fazla bilgi için bkz. [.NET kullanarak Azure Key Vault hizmetten hizmete kimlik doğrulaması](../../key-vault/service-to-service-authentication.md). 

Erişim belirteci alma hakkında daha fazla bilgi için bkz: [bir erişim belirteci almak için bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlikleri kullanmak nasıl](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

> [!NOTE]
> Azure AD ile blob veya kuyruğa verilerine yönelik isteklerini yetkilendirmek için bu istekleri için HTTPS kullanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- RBAC rolleri için Azure depolama hakkında daha fazla bilgi için bkz: [RBAC ile depolama verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).
- Kapsayıcılar ve kuyruklardaki depolama uygulamalarınızda erişim yetkisi verme konusunda bilgi almak için bkz: [depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
- Azure AD kimlik bilgileriyle Azure CLI ve PowerShell komutlarını çalıştırma hakkında bilgi edinmek için [blob veya sıra verilerinize erişmek için Azure CLI'yı çalıştırmak veya PowerShell komutları Azure AD kimlik bilgileriyle](storage-auth-aad-script.md).