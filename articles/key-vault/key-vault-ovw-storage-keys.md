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
ms.openlocfilehash: adc8b84f0f22e85de88c4bd80c10a2a35d7b490a
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114609"
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
    - Lütfen bu adımları izleyerek [belge](https://docs.microsoft.com/azure/storage/) bir depolama hesabı oluşturmak için  
    - **Adlandırma:** depolama hesabı adları 3 ila 24 karakter uzunluğunda olmalıdır ve yalnızca sayılar ve küçük harfler içerebilir.        
      
<a name="step-by-step-instructions"></a>Adım adım yönergeleri ile
-------------------------

1. Yönetmek istediğiniz Azure depolama hesabının kaynak Kimliğini alın.
    a. Bir depolama hesabı oluşturduktan sonra yönetmek istediğiniz depolama hesabının kaynak Kimliğini almak için aşağıdaki komutu çalıştırın
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
   Komut, depolamanın erişim anahtarlarını düzenli aralıklarla yeniden üretme nokta ile yeniden oluşturmak için Key Vault ister. Aşağıda, bir oluşturma süresini 90 gün ayarlıyorsunuz. 90 günün sonunda, Key Vault yeniden 'anahtar1' ve 'anahtar1' active 'anahtar2' anahtarını değiştirme.
   ### <a name="key-regeneration"></a>Yeniden anahtar oluşturma
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-generate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    Kullanıcı depolama hesabı oluşturmadıysanız ve depolama hesabı için izinlere sahip değil durumunda, aşağıdaki adımları, anahtar Kasası'nda tüm depolama izinleri yönetebilirsiniz emin olmak hesabınız için izinleri ayarlayın.
    [!NOTE] Kullanıcı önce kullanıcının nesne kimliği aldığımız depolama hesabının izni vermiyor durumunda

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover purge restore set setsas update
    ```

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
