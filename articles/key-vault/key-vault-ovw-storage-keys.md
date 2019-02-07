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
ms.openlocfilehash: 152e1e5892e3a72286205c2f5bf4e18b2a2bcbf7
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814852"
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
İçinde yönergeler, size Key Vault operatör izinleri depolama hesabınız için bir hizmet olarak atama

> [!NOTE]
> Lütfen Azure anahtar kasası yönetilen ayarladıktan sonra depolama hesabı anahtarları, bunlar Not gereken **Hayır** artık değiştirilmesi dışında Key Vault aracılığıyla. Yönetilen depolama hesabı anahtarları Key Vault depolama hesabı anahtarını döndürme yönetmeyi tercih anlamına gelir

1. Depolama hesabının kaynak Kimliğini almak için aşağıdaki komutu çalıştırın, bir depolama hesabı oluşturulduktan sonra yönetmek istediğiniz

    ```
    az storage account show -n storageaccountname 
    ```
    Kopya Kimliği alanı dışında yukarıdaki komutunun sonucu
    
2. Nesne kimliği, Azure anahtar Kasası'nın hizmet sorumlusu çalıştırarak alın aşağıdaki komutu

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
    Bu komutun işlemin başarıyla tamamlanmasından sonra sonucu nesne Kimliğini bulun.
    ```console
        {
            ...
            "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
            ...
        }
    ```
    
3. Azure Key Vault kimlik için depolama anahtarı operatörü rolünü atama

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope <IdOfStorageAccount>
    ```
    
4. Anahtar kasası oluşturma yönetilen depolama hesabı.     <br /><br />
   Aşağıda, bir oluşturma süresini 90 gün ayarlıyorsunuz. 90 günün sonunda, Key Vault yeniden 'anahtar1' ve 'anahtar1' active 'anahtar2' anahtarını değiştirme. Artık etkin anahtar Key1 işaret. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```
    Kullanıcı depolama hesabı oluşturmadıysanız ve depolama hesabı için izinlere sahip değil durumunda, aşağıdaki adımları, anahtar Kasası'nda tüm depolama izinleri yönetebilirsiniz emin olmak hesabınız için izinleri ayarlayın.
    
 > [!NOTE] 
 > Kullanıcının depolama hesabı için izinler yok durumda biz ilk kullanıcının nesne kimliği alın


    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    
    ```
    
## <a name="how-to-access-your-storage-account-with-sas-tokens"></a>SAS belirteçleri kullanarak depolama hesabınızın erişim

Bu bölümde getirilirken tarafından depolama hesabınızda işlemleri nasıl yapabilirsiniz ele alınacaktır [SAS belirteçlerini](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) Key vault'tan

İçinde aşağıdaki bölümüne, biz Key Vault'ta depolanan depolama hesabı anahtarınızı getirme nasıl gerçekleştirileceğini gösterir ve depolama hesabınız için bir SAS (paylaşılan erişim imzası) tanımı oluşturmak için kullandığı.

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
