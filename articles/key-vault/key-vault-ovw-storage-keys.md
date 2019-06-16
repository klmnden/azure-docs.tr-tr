---
title: Depolama hesabı anahtarları Azure Key Vault ve Azure CLI ile yönetme
description: Depolama hesabı anahtarları Azure Key Vault ve Azure depolama hesabınız için anahtar tabanlı erişim arasında sorunsuz bir tümleştirme sağlar.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 03/01/2019
ms.openlocfilehash: 91cc3f96f9cdd231c38232c972c2628d12b9f4b3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66476159"
---
# <a name="manage-storage-account-keys-with-azure-key-vault-and-the-azure-cli"></a>Depolama hesabı anahtarları Azure Key Vault ve Azure CLI ile yönetme 

Azure Key Vault, Azure depolama hesapları ve klasik depolama hesapları için anahtarları yönetir. Sizin için birkaç anahtar yönetimi işlevleri tamamlamak için anahtar kasası yönetilen depolama hesabı özelliğini kullanabilirsiniz.

Bir [Azure depolama hesabı](/azure/storage/storage-create-storage-account) bir hesap adı ve anahtar oluşan bir kimlik bilgisi kullanır. Anahtar otomatik olarak oluşturulan ve bir parola yerine bir olarak hizmet veren bir şifreleme anahtarı. Key Vault depolama hesabı anahtarlarını depolayarak bunları olarak yöneten [Key Vault gizli dizileri](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets). Anahtarları ile bir Azure depolama hesabı (eşitlenen) listelenir ve düzenli aralıklarla yeniden veya _Döndürülmüş_. 

Yönetilen depolama hesabı anahtar özelliğini kullandığınızda, aşağıdaki noktaları göz önünde bulundurun:

- Anahtar değerler hiçbir zaman bir çağıranın yanıt döndürülür.
- Key Vault depolama hesabı anahtarlarınızı yönetmeniz gerekir. Yoksa, anahtarları kendiniz yönetebilir ve anahtar kasası işlemleri ile müdahaleyi önlemek.
- Yalnızca tek bir anahtar kasası nesne depolama hesabı anahtarlarını yönetmeniz gerekir. Anahtar Yönetimi'nden birden çok nesne izin vermez.
- Bir kullanıcı asıl olan, ancak bir hizmet sorumlusu ile değil, depolama hesabınızı yönetmek için Key Vault isteyebilir.
- Key Vault yalnızca kullanarak anahtarları yeniden oluştur. El ile depolama hesabı anahtarlarını yeniden yok. 

> [!NOTE]
> Azure Active Directory (Azure AD) ile Azure depolama tümleştirme Microsoft'un bulut tabanlı kimlik ve erişim yönetim hizmetidir.
> Azure AD tümleştirmesi için kullanılabilir [Azure BLOB'ları ve kuyrukları](https://docs.microsoft.com/azure/storage/common/storage-auth-aad).
> Azure AD kimlik doğrulama ve yetkilendirme için kullanın.
> Azure AD, Azure Key Vault gibi Azure Depolama'ya OAuth2 belirteç tabanlı erişim sağlar.
>
> Azure AD, bir uygulama veya kullanıcı kimliği yerine depolama hesabı kimlik bilgilerini kullanarak istemci uygulamanızın kimlik doğrulaması sağlar.
> Kullanabileceğiniz bir [Azure AD kimlik yönetilen](/azure/active-directory/managed-identities-azure-resources/) Azure'da çalıştırdığınızda. Yönetilen kimlikleri, istemci kimlik doğrulaması ve kimlik bilgilerini veya uygulamanız ile depolama ihtiyacını ortadan kaldırıyor.
> Azure AD, anahtar kasası tarafından da desteklenir, rol tabanlı erişim denetimi (RBAC), yetkilendirmeyi yönetmek için kullanır.

### <a name="service-principal-application-id"></a>Hizmet sorumlusu uygulama kimliği

Azure AD kiracısı ile kayıtlı her uygulama sağlayan bir [hizmet sorumlusu](/azure/active-directory/develop/developer-glossary#service-principal-object). Hizmet sorumlusu uygulama kimliği (ID) olarak işlev görür. Uygulama kimliği, yetkilendirme Kurulum sırasında RBAC aracılığıyla diğer Azure kaynaklarına erişim için kullanılır.

Anahtar kasası tüm Azure AD kiracıları önceden kayıtlı bir Microsoft uygulamasıdır. Key Vault, aynı uygulama kimliği ve her Azure bulut içinde kayıtlı değil.

| Kiracılar | Bulut | Uygulama Kimliği |
| --- | --- | --- |
| Azure AD | Azure Kamu | `7e7c393b-45d0-48b1-a35e-2905ddf8183c` |
| Azure AD | Azure genel | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |
| Diğer  | Tüm | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="prerequisites"></a>Önkoşullar

Depolama hesabı anahtarınızı yönetmek için anahtar Kasası'nı kullanmadan önce önkoşulları gözden geçirin:

- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)’yi yükleyin.
- Oluşturma bir [Azure depolama hesabı](https://azure.microsoft.com/services/storage/). İzleyin [adımları](https://docs.microsoft.com/azure/storage/).
- Depolama hesabı adı yalnızca küçük harflerden ve rakamlardan kullanmanız gerekir. Adının uzunluğu 3 ile 24 karakter arasında olmalıdır.        
      
## <a name="manage-storage-account-keys"></a>Depolama hesabı anahtarlarını yönetme

Key Vault depolama hesabı anahtarlarını yönetmek için kullanmak için dört temel adım vardır:

1. Mevcut bir depolama hesabını alın.
1. Mevcut bir anahtar kasasını getirin.
1. Anahtar kasası yönetilen depolama hesabı kasaya ekler. Ayarlama `key1` etkin anahtar ile bir oluşturma süresini 180 gün olarak.
1. Kullanım `key1` belirtilen depolama hesabı için bir depolama bağlamını ayarlamak için.

> [!NOTE]
> Key Vault hizmeti, depolama hesabınızda operatör izinleri atanır.
> 
> Yalnızca Azure anahtar kasası yönetilen depolama hesabı anahtarlarını ayarladıktan sonra anahtarları Key Vault'u kullanarak değiştirin.
> Yönetilen depolama hesabı anahtarlarını Key Vault depolama hesabı anahtarını döndürmesini yönetir.

1. Bir depolama hesabı oluşturduktan sonra yönetmek için depolama hesabının kaynak Kimliğini almak için aşağıdaki komutu çalıştırın:
    ```
    az storage account show -n storageaccountname
    ```

    Kaynak Kimliği değeri komut çıktısını kopyalayın:
    ```
    /subscriptions/<subscription ID>/resourceGroups/ResourceGroup/providers/Microsoft.Storage/storageAccounts/StorageAccountName
    ```

    Örnek çıktı:
    ```
    "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
    ```
    
1. Anahtar Kasası'na "Depolama hesabı anahtarı işleci hizmet rolü" RBAC rolü atayın. Bu rol, depolama hesabınıza erişim kapsamını sınırlar. Bir Klasik depolama hesabı için "Klasik depolama hesabı anahtarı işleci hizmet rolü" rolünü kullanın.

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope 93c27d83-f79b-4cb2-8dd4-4aa716542e74
    ```
    
    `93c27d83-f79b-4cb2-8dd4-4aa716542e74` Nesne Kimliğini, anahtar Kasası'nda Azure genel bulutunda içindir. Nesne kimliği için Key Vault, Azure kamu bulutunda edinmek için bkz [hizmet sorumlusu uygulama kimliği](#service-principal-application-id).
    
1. Bir anahtar kasası yönetilen depolama hesabı oluşturun:

    Bir oluşturma süresini 90 gün olarak ayarlayın. 90 günün sonunda, anahtar kasası oluşturur `key1` ve etkin anahtarı değiştirir `key2` için `key1`. `key1` ardından etkin bir anahtar olarak işaretlenir. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="create-and-generate-tokens"></a>Oluşturma ve belirteçleri oluşturmak

Paylaşılan erişim imzası belirteçleri oluşturmak için Key Vault da sorabilirsiniz. Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. İstemciler hesap anahtarlarınızı paylaşmadan depolama hesabınızdaki kaynaklara erişim verebilirsiniz. Paylaşılan erişim imzası, hesap anahtarlarınızı tehlikeye atmadan depolama kaynaklarınızı paylaşmanın güvenli bir yol sağlar.

Bu bölümdeki komutlar aşağıdaki işlemleri tamamlayın:

- Kümesi bir hesabı paylaşılan erişim imzası tanımı `<YourSASDefinitionName>`. Bir anahtar kasası yönetilen depolama hesabına kümesi tanımı `<YourStorageAccountName>` anahtar kasanızdaki `<VaultName>`.
- Blob, dosya, tablo ve kuyruk Hizmetleri için hesabı bir paylaşılan erişim imzası belirtecini oluşturun. Belirteç, kaynak türleri için hizmet ve kapsayıcı nesnesi oluşturulur. Belirteç, belirtilen başlangıç ve bitiş tarihleri ve https üzerinden tüm izinleri ile oluşturulur.
- Kasaya bir anahtar kasası yönetilen paylaşılan depolama erişim imzası tanımı ayarlayın. Tanım oluşturulan paylaşılan erişim İmza belirteci URI Şablonu ' var. Paylaşılan erişim imzası türü tanımına sahip `account` ve N gün boyunca geçerli olur.
- Paylaşılan erişim imzası tanımına karşılık gelen bir Key Vault gizli dizilerinden gerçek bir erişim belirteci alın.

Önceki bölümdeki adımları tamamladıktan sonra paylaşılan erişim imzası belirteçleri oluşturmak için Key Vault istemek için aşağıdaki komutları çalıştırın. 

1. Bir paylaşılan erişim imzası tanımı oluşturun. Paylaşılan erişim imzası tanımı oluşturulduktan sonra fazla paylaşılan erişim İmza belirteci oluşturmak için Key Vault isteyin. Bu işlem gerektiriyor `storage` ve `setsas` izinleri.
    ```
    $sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
    ```

    İşlemi hakkında daha fazla yardım için bkz: [az depolama hesabı SAS-Oluştur](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas) başvuru belgeleri.

    İşlem başarıyla çalıştıktan sonra çıktısını kopyalayın.
    ```console
       "se=2020-01-01&sp=***"
    ```

1. Kullanım `$sasToken` önceki komutu tarafından oluşturulan ve paylaşılan erişim imzası tanımı oluşturun. Komut parametreleri hakkında daha fazla bilgi için bkz: [az keyvault depolama sas tanımı oluşturma](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters) başvuru belgeleri.
    ```
    az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
    ```

    İlk kullanıcı izinleri depolama hesabına sahip olmadığında kullanıcının nesne kimliği alın:
    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="fetch-tokens-in-code"></a>Kodunda belirteçleri alma

Yakalama tarafından depolama hesabınızda işlemleri yürütmek [paylaşılan erişim imzası belirteçlerinin](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) Key vault'tan.

Anahtar Kasası'na kimlik doğrulaması için üç yolu vardır:

- Yönetilen hizmet kimliği kullanın. Bu yaklaşım önerilir.
- Bir hizmet sorumlusu ve sertifika kullanın. 
- Bir hizmet sorumlusu ve parolayı kullanın. Bu yaklaşım önerilmez.

Daha fazla bilgi için [Azure anahtar Kasası: Temel kavramlar](key-vault-whatis.md#basic-concepts).

Aşağıdaki örnek, paylaşılan erişim imzası belirteçlerinin getirilecek gösterilmiştir. Paylaşılan erişim imzası tanımı oluşturduktan sonra belirteçleri getirin. 

```cs
// After you get a security token, create KeyVaultClient with vault credentials.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a shared access signature token for your storage from Key Vault.
// The format for SecretUri is https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials by using the shared access signature token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Dolmak üzere paylaşılan erişim İmza belirteci ise, paylaşılan erişim imzası belirtecini Key Vault'tan yeniden getirmek ve kod güncelleştirin.

```cs
// If your shared access signature token is about to expire,
// get the shared access signature token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

### <a name="azure-cli-commands"></a>Azure CLI komutları

Yönetilen depolama hesapları için uygun olan Azure CLI komutları hakkında daha fazla bilgi için bkz: [az keyvault depolama](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest) başvuru belgeleri.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [anahtarlara, parolalara ve sertifikalara](https://docs.microsoft.com/rest/api/keyvault/).
- Makaleleri gözden [Azure anahtar kasası ekibi blogunu](https://blogs.technet.microsoft.com/kv/).
