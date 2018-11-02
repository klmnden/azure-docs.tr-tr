---
ms.assetid: ''
title: Azure anahtar kasası depolama hesabı anahtarları
description: Depolama hesabı anahtarları Azure Key Vault ile anahtar tabanlı erişim arasında bir seemless tümleştirme için Azure depolama hesabı sağlayın.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 10/03/2018
ms.openlocfilehash: f3f310c247aea3842b5ec7a9a1409032d5bdc0bf
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912926"
---
# <a name="azure-key-vault-storage-account-keys"></a>Azure anahtar kasası depolama hesabı anahtarları

> [!NOTE]
> [Azure storage artık AAD yetkilendirme destekler](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Kullanıcılar, kendi depolama hesabı anahtarlarını döndürme hakkında endişelenmenize gerek yok mıydı gibi Azure Active Directory kimlik doğrulaması ve yetkilendirme için depolama kullanmanızı öneririz.

- Azure Key Vault, anahtarları bir Azure depolama hesabı (ASA) yönetir.
    - Dahili olarak, Azure Key Vault (eşitleme) Azure depolama hesabı anahtarları listeleyebilirsiniz.    
    - Azure Key Vault oluşturur (döndürür) anahtarlarını düzenli aralıklarla.
    - Anahtar değerler hiçbir zaman yanıt arayana döndürülür.
    - Azure Key Vault, depolama hesapları hem de klasik depolama hesapları, anahtarları yönetir.

<a name="prerequisites"></a>Önkoşullar
--------------
1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) Azure CLI yükleme   
2. [Depolama hesabı oluşturma](https://azure.microsoft.com/services/storage/)
    - Bu adımları izleyerek [belge](https://docs.microsoft.com/azure/storage/) bir depolama hesabı oluşturmak için  
    - **Adlandırma:** depolama hesabı adları 3 ila 24 karakter uzunluğunda olmalıdır ve yalnızca sayılar ve küçük harfler içerebilir.        
      
<a name="step-by-step-instructions-on-how-to-use-key-vault-to-manage-storage-account-keys"></a>Key Vault depolama hesabı anahtarlarını yönetmek için kullanımı konusunda adım yönergeleri ile adım
--------------------------------------------------------------------------------
İçinde yönergeler, size Key Vault operatör izinleri depolama hesabınız için bir hizmet olarak atama

> [!NOTE]
> Lütfen Azure anahtar kasası yönetilen ayarladıktan sonra depolama hesabı anahtarları, bunlar Not gereken **Hayır** artık değiştirilmesi dışında Key Vault aracılığıyla. Yönetilen depolama hesabı anahtarları Key Vault depolama hesabı anahtarını döndürme yönetmeyi tercih anlamına gelir

1. Depolama hesabının kaynak Kimliğini almak için aşağıdaki komutu çalıştırın, bir depolama hesabı oluşturulduktan sonra yönetmek istediğiniz

    ```
    az storage account show -n storageaccountname (Copy ID out of the result of this command)
    ```
    
2. Uygulama kimliği, Azure anahtar Kasası'nın hizmet sorumlusu alma 

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
3. Azure Key Vault kimlik için depolama anahtarı operatörü rolünü atama

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id hhjkh --scope idofthestorageaccount
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
- [Güncelleştirme AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="see-also"></a>Ayrıca bkz.

- [Anahtarlar, gizli diziler ve sertifikalar hakkında](https://docs.microsoft.com/rest/api/keyvault/)
- [Anahtar kasası ekibi blogu](https://blogs.technet.microsoft.com/kv/)
