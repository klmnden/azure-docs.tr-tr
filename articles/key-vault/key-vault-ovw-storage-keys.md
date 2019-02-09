---
ms.assetid: ''
title: Azure anahtar kasası yönetilen depolama hesabı - CLI
description: Depolama hesabı anahtarları Azure Key Vault ile anahtar tabanlı erişim arasında bir seemless tümleştirme için Azure depolama hesabı sağlayın.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: prashanthyv
ms.author: pryerram
manager: mbaldwin
ms.date: 10/03/2018
ms.openlocfilehash: 9bff93fbec73eb73dca01660d46e35e194edb626
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55963497"
---
# <a name="azure-key-vault-managed-storage-account---cli"></a>Azure anahtar kasası yönetilen depolama hesabı - CLI

> [!NOTE]
> [Azure Active Directory (Azure AD) ile Azure depolama tümleştirme şu anda Önizleme aşamasındadır](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Azure AD kimlik doğrulaması ve yetkilendirme, Azure Key Vault gibi Azure depolama OAuth2 belirteç tabanlı erişim sağlayan kullanmanızı öneririz. Bu, sağlar:
> - Depolama hesabı kimlik bilgileri yerine bir uygulama veya kullanıcıya kimlik kullanarak istemci uygulamanızın kimlik doğrulaması. 
> - Kullanım bir [Azure AD kimlik yönetilen](/azure/active-directory/managed-identities-azure-resources/) Azure'da çalıştırırken. Kimlikleri Kaldır hep birlikte istemci kimlik doğrulaması için gereken ve depolama kimlik bilgileri veya uygulamanız ile yönetilir.
> - Key Vault tarafından da desteklenen, yetkilendirme için rol tabanlı erişim denetimi (RBAC) kullanın.

- Azure Key Vault, anahtarları bir Azure depolama hesabı (ASA) yönetir.
    - Dahili olarak, Azure Key Vault (eşitleme) Azure depolama hesabı anahtarları listeleyebilirsiniz.    
    - Azure Key Vault oluşturur (döndürür) anahtarlarını düzenli aralıklarla.
    - Anahtar değerler hiçbir zaman yanıt arayana döndürülür.
    - Azure Key Vault, depolama hesapları hem de klasik depolama hesapları, anahtarları yönetir.
    
> [!IMPORTANT]
> Azure AD kiracısı ile kayıtlı her uygulama sağlayan bir  **[hizmet sorumlusu](/azure/active-directory/develop/developer-glossary#service-principal-object)**, uygulama kimliği olarak görev yapar. Hizmet sorumlusunun uygulama kimliği, rol tabanlı erişim denetimi (RBAC) diğer Azure kaynaklarına erişmek için yetki verirken kullanılır. Key Vault Microsoft uygulama olduğundan, tüm Azure AD kiracılarıyla aynı uygulama kimliği, her Azure bulut içinde altında önceden kaydedilir:
> - Azure kamu bulutunda Azure AD kiracıları kullanmak uygulama kimliği `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - Azure genel bulutunda ve diğer tüm Azure AD kiracıları kullanmak uygulama kimliği `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.

<a name="prerequisites"></a>Önkoşullar
--------------
1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) Azure CLI yükleme   
2. [Depolama hesabı oluşturma](https://azure.microsoft.com/services/storage/)
    - Bu adımları izleyerek [belge](https://docs.microsoft.com/azure/storage/) bir depolama hesabı oluşturmak için  
    - **Adlandırma yönergeleri:** Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.        
      
<a name="step-by-step-instructions-on-how-to-use-key-vault-to-manage-storage-account-keys"></a>Key Vault depolama hesabı anahtarlarını yönetmek için kullanımı konusunda adım yönergeleri ile adım
--------------------------------------------------------------------------------
Kavramsal olarak izlendiğini adımları listesidir
- Biz ilk (önceden mevcut olan) depolama hesabı alın
- Biz sonra (önceden mevcut olan) bir anahtar kasası getirilemedi
- Ardından KeyVault ile yönetilen depolama hesabı, kasaya Key1 etkin anahtar olarak ve 180 günlük bir oluşturma süresini ayarlama ekliyoruz
- Son olarak Key1 ile belirtilen depolama hesabı için bir depolama bağlamı ayarladık

İçinde yönergeler, size Key Vault operatör izinleri depolama hesabınız için bir hizmet olarak atama

> [!NOTE]
> Lütfen Azure anahtar kasası yönetilen ayarladıktan sonra depolama hesabı anahtarları, bunlar Not gereken **Hayır** artık değiştirilmesi dışında Key Vault aracılığıyla. Yönetilen depolama hesabı anahtarları Key Vault depolama hesabı anahtarını döndürme yönetmeyi tercih anlamına gelir

1. Depolama hesabının kaynak Kimliğini almak için aşağıdaki komutu çalıştırın, bir depolama hesabı oluşturulduktan sonra yönetmek istediğiniz

    ```
    az storage account show -n storageaccountname 
    ```
    Kopya Kimliği alanı dışında yukarıdaki komutunun sonucu
    
2. Azure anahtar kasası nesne kimliği hizmet sorumlusu çalıştırarak alın aşağıdaki komutu

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
    Bu komutun işlemin başarıyla tamamlanmasından sonra sonucu nesne Kimliğini bulun:
    ```console
        {
            ...
            "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
            ...
        }
    ```
    
3. Azure anahtar kasası kimliği için depolama anahtarı operatörü rolü atayın.

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope <IdOfStorageAccount>
    ```
    
4. Anahtar kasası oluşturma yönetilen depolama hesabı.     <br /><br />
   Aşağıda, bir oluşturma süresini 90 gün ayarlıyorsunuz. 90 günün sonunda, Key Vault yeniden 'anahtar1' ve 'anahtar1' active 'anahtar2' anahtarını değiştirme. Artık etkin anahtar Key1 işaret. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```
    Kullanıcı depolama hesabı oluşturmadıysanız ve depolama hesabı için izinlere sahip değil durumunda, aşağıdaki adımları, anahtar Kasası'nda tüm depolama izinleri yönetebilirsiniz emin olmak hesabınız için izinleri ayarlayın.
    

<a name="step-by-step-instructions-on-how-to-use-key-vault-to-create-and-generate-sas-tokens"></a>Key Vault oluşturun ve SAS belirteçleri oluşturmak için kullanımı konusunda adım yönergeleri ile adım
--------------------------------------------------------------------------------
SAS (paylaşılan erişim imzası) belirteçleri oluşturmak için Key Vault da sorabilirsiniz. Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Bir SAS ile hesap anahtarlarınızı paylaşmadan depolama hesabınızdaki kaynaklara erişim istemcileri verebilirsiniz. Bu, uygulamalarınızda paylaşılan erişim imzaları kullanmanın anahtar noktasıdır. SAS, hesap anahtarlarınızı tehlikeye atmadan depolama kaynaklarınızı paylaşmanın güvenli bir yoludur.

Oluşturmayı tamamladıktan sonra listelenen adımları SAS belirteçleri oluşturmak için Key Vault istemek için aşağıdaki komutları çalıştırabilirsiniz. 

Listenin içinde ulaşılacağına şeyleri adımlarla ilgili daha fazla bilgi
- Bir hesap SAS tanımı adlı ayarlar '<YourSASDefinitionName>'KeyVault ile yönetilen depolama hesabındaki'<YourStorageAccountName>'kasanızdaki'<VaultName>'. 
- Belirtilen başlangıç ve bitiş tarihleri ve https üzerinden tüm izinlere sahip bir hesap SAS belirtecini Hizmetleri Blob, dosya, tablo ve kuyruk hizmeti, kapsayıcı ve nesne, kaynak türleri için oluşturur
- Kasadaki yukarıda, SAS türü 'account' ve geçerli N gün için oluşturulan SAS belirteciyle şablon URI ile bir KeyVault ile yönetilen depolama SAS tanımı ayarlar
- KeyVault gizli SAS tanımına karşılık gelen gerçek erişim belirtecini alır

1. Bu adımda bir SAS tanımı oluşturacağız. Bu SAS tanımı oluşturulduktan sonra daha fazla SAS belirteçleri oluşturmak için Key Vault sorabilirsiniz. Bu işlem, depolama/setsas izni gerektirir.

```
$sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
```
Yukarıdaki işlemi hakkında daha fazla yardım gördüğünüz [burada](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas)

Bu işlem başarıyla çalıştığında, aşağıda gösterildiği gibi benzer bir çıktı görmeniz gerekir. Bu kopyalama

```console
   "se=2020-01-01&sp=***"
```

2. Bu adımda bir SAS tanımı oluşturmak için yukarıda oluşturulan çıktı ($sasToken) kullanacağız. Daha fazla belgelerini okuyun [burada](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters)   

```
az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
```
                        

 > [!NOTE] 
 > Kullanıcının depolama hesabı için izinler yok durumda biz ilk kullanıcının nesne kimliği alın

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    
    ```
    
## <a name="fetch-sas-tokens-in-code"></a>SAS belirteçleri kodda getirilemedi

Bu bölümde getirilirken tarafından depolama hesabınızda işlemleri nasıl yapabilirsiniz ele alınacaktır [SAS belirteçlerini](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) Key vault'tan

İçinde aşağıdaki bölümüne, biz bir SAS tanımında yukarıda da gösterildiği gibi oluşturulduktan sonra SAS belirteçlerini almak nasıl ekleyebileceğiniz gösterilmektedir.

> [!NOTE] 
  İçinde okumak için Key Vault kimlik doğrulaması için izleyebileceğiniz 3 yol vardır [temel kavramlar](key-vault-whatis.md#basic-concepts)
> - Yönetilen hizmet kimliği (kesinlikle önerilir) kullanma
> - Hizmet sorumlusu ve sertifika kullanma 
> - Hizmet sorumlusu ve parola (önerilmez) kullanma

```cs
// Once you have a security token from one of the above methods, then create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault. SecretUri is of the format https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Değeriniz SAS belirtecinizle dolmak üzere ise, SAS belirteci yeniden Key Vault'tan getirme sonra kodunu güncelleştirme

```cs
// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```


### <a name="relavant-azure-cli-cmdlets"></a>İşaretlemek Azure CLI cmdlet'leri
[Azure CLI'yı depolama cmdlet'leri](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

### <a name="relevant-powershell-cmdlets"></a>İlgili Powershell cmdlet'leri

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="see-also"></a>Ayrıca bkz.

- [Anahtarlar, gizli diziler ve sertifikalar hakkında](https://docs.microsoft.com/rest/api/keyvault/)
- [Anahtar kasası ekibi blogu](https://blogs.technet.microsoft.com/kv/)
