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
ms.openlocfilehash: e9b9620c3c631a9984bc6d1d02dc792c592b6e69
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078398"
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
    az storage account show -n storageaccountname (Copy ID field out of the result of this command)
    ```
    
2. Uygulama kimliği, Azure anahtar Kasası'nın hizmet sorumlusu alma 

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
3. Azure Key Vault kimlik için depolama anahtarı operatörü rolünü atama

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ApplicationIdOfKeyVault> --scope <IdOfStorageAccount>
    ```
    
4. Anahtar kasası oluşturma yönetilen depolama hesabı.     <br /><br />
   Aşağıda, bir oluşturma süresini 90 gün ayarlıyorsunuz. 90 günün sonunda, Key Vault yeniden 'anahtar1' ve 'anahtar1' active 'anahtar2' anahtarını değiştirme.
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-regenerate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    Kullanıcı depolama hesabı oluşturmadıysanız ve depolama hesabı için izinlere sahip değil durumunda, aşağıdaki adımları, anahtar Kasası'nda tüm depolama izinleri yönetebilirsiniz emin olmak hesabınız için izinleri ayarlayın.
 > [!NOTE] 
    Kullanıcının depolama hesabı için izinler yok durumda biz ilk kullanıcının nesne kimliği alın

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```
### <a name="relavant-azure-cli-cmdlets"></a>İşaretlemek Azure CLI cmdlet'leri
- [Azure CLI'yı depolama cmdlet'leri](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

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
