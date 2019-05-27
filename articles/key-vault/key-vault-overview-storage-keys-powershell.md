---
title: Azure anahtar kasası yönetilen depolama hesabı - PowerShell sürümü
description: Yönetilen depolama hesabı Özelliği Azure anahtar kasası ve Azure depolama hesabınız arasında bir seemless tümleştirme sağlar.
ms.topic: conceptual
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 03/01/2019
ms.openlocfilehash: 9b6089aa828b5667f100c1a8cbff3e69345e4512
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66150419"
---
# <a name="azure-key-vault-managed-storage-account---powershell"></a>Azure anahtar kasası yönetilen depolama hesabı - PowerShell

> [!NOTE]
> [Azure Active Directory (Azure AD) ile Azure depolama tümleştirme şu anda Önizleme aşamasındadır](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Azure AD kimlik doğrulaması ve yetkilendirme, Azure Key Vault gibi Azure depolama OAuth2 belirteç tabanlı erişim sağlayan kullanmanızı öneririz. Bu, sağlar:
> - Depolama hesabı kimlik bilgileri yerine bir uygulama veya kullanıcıya kimlik kullanarak istemci uygulamanızın kimlik doğrulaması. 
> - Kullanım bir [Azure AD kimlik yönetilen](/azure/active-directory/managed-identities-azure-resources/) Azure'da çalıştırırken. Kimlikleri Kaldır hep birlikte istemci kimlik doğrulaması için gereken ve depolama kimlik bilgileri veya uygulamanız ile yönetilir.
> - Key Vault tarafından da desteklenen, yetkilendirme için rol tabanlı erişim denetimi (RBAC) kullanın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir [Azure depolama hesabı](/azure/storage/storage-create-storage-account) bir hesap adı ve anahtar oluşan bir kimlik bilgisi kullanır. Bu anahtar, otomatik olarak oluşturulan ve daha farklı bir şifreleme anahtarı "parola" olarak görev yapar. Key Vault depolayarak bunları olarak bu depolama hesabı anahtarları yönetebilir [Key Vault gizli dizileri](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets). 

## <a name="overview"></a>Genel Bakış

Key Vault depolama hesabı özelliği birkaç yönetim işlevlerini sizin adınıza gerçekleştirdiği yönetilen:

- Bir Azure depolama hesabı anahtarları listeleri (eşitlemeler).
- Yeniden oluşturur (döndürür) anahtarlarını düzenli aralıklarla.
- Depolama hesapları hem de klasik depolama hesapları için anahtarları yönetir.
- Anahtar değerler hiçbir zaman yanıt arayana döndürülür.

Yönetilen depolama hesabı anahtar özelliğini kullanırken:

- **Yalnızca depolama hesap anahtarlarınızı yönetmek Key Vault izin verir.** Key Vault'un işlemlerle engel şekilde kendiniz yönetmek denemeyin.
- **Depolama hesabı anahtarları birden fazla anahtar kasası nesne tarafından yönetilecek izin verme**.
- **Depolama hesabı anahtarlarınızı el ile yeniden yoksa**. Key Vault aracılığıyla bunları yeniden öneririz.

Aşağıdaki örnek, depolama hesabı anahtarlarını yönetmek Key Vault kullanmasının nasıl sağlanacağını gösterir.

## <a name="authorize-key-vault-to-access-to-your-storage-account"></a>Key Vault, depolama hesabınıza erişimi yetkilendirin

> [!IMPORTANT]
> Azure AD kiracısı ile kayıtlı her uygulama sağlayan bir  **[hizmet sorumlusu](/azure/active-directory/develop/developer-glossary#service-principal-object)**, uygulama kimliği olarak görev yapar. Hizmet sorumlusunun uygulama kimliği, rol tabanlı erişim denetimi (RBAC) diğer Azure kaynaklarına erişmek için yetki verirken kullanılır. Key Vault Microsoft uygulama olduğundan, tüm Azure AD kiracılarıyla aynı uygulama kimliği, her Azure bulut içinde altında önceden kaydedilir:
> - Azure kamu bulutunda Azure AD kiracıları kullanmak uygulama kimliği `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - Azure genel bulutunda ve diğer tüm Azure AD kiracıları kullanmak uygulama kimliği `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.

Key Vault erişimi ve depolama hesap anahtarlarınızı yönetme önce depolama hesabınıza erişimini yetkilendirmesi gerekecektir. Anahtar kasası uygulama izinleri bulunmalıdır *listesi* ve *yeniden* anahtarları, depolama hesabınız için. Bu izinleri yerleşik RBAC rolü etkinleştirilir [depolama hesabı anahtarı işleci hizmet rolü](/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role). 

Bu rol, aşağıdaki adımları kullanarak depolama hesabınıza kapsamı sınırlayarak Key Vault hizmet sorumlusuna atayın. Güncelleştirdiğinizden emin olun `$resourceGroupName`, `$storageAccountName`, `$storageAccountKey`, ve `$keyVaultName` betiği çalıştırmadan önce değişkenleri:

```azurepowershell-interactive
# TODO: Update with the resource group where your storage account resides, your storage account name, the name of your active storage account key, and your Key Vault instance name
$resourceGroupName = "rgContoso"
$storageAccountName = "sacontoso"
$storageAccountKey = "key1"
$keyVaultName = "kvContoso"
$keyVaultSpAppId = "cfa8b339-82a2-471a-a3c9-0fc0be7a4093" # See "IMPORTANT" block above for information on Key Vault Application IDs

# Authenticate your PowerShell session with Azure AD, for use with Azure Resource Manager cmdlets
$azureProfile = Connect-AzAccount

# Get a reference to your Azure storage account
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName

# Assign RBAC role "Storage Account Key Operator Service Role" to Key Vault, limiting the access scope to your storage account. For a classic storage account, use "Classic Storage Account Key Operator Service Role." 
New-AzRoleAssignment -ApplicationId $keyVaultSpAppId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storageAccount.Id
```

Başarılı bir rol ataması sırasında aşağıdaki örneğe benzer bir çıktı görmeniz gerekir:

```console
RoleAssignmentId   : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso/providers/Microsoft.Authorization/roleAssignments/189cblll-12fb-406e-8699-4eef8b2b9ecz
Scope              : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
DisplayName        : Azure Key Vault
SignInName         :
RoleDefinitionName : storage account Key Operator Service Role
RoleDefinitionId   : 81a9662b-bebf-436f-a333-f67b29880f12
ObjectId           : 93c27d83-f79b-4cb2-8dd4-4aa716542e74
ObjectType         : ServicePrincipal
CanDelegate        : False
```

Key Vault, depolama hesabınızın rolüne zaten eklenmiş durumunda alırsınız bir *"rol ataması zaten var."* Bir hata oluştu. Rol ataması, Azure portalında depolama hesabı "Erişim denetimi (IAM)" sayfasını kullanarak da doğrulayabilirsiniz.  

## <a name="give-your-user-account-permission-to-managed-storage-accounts"></a>Yönetilen depolama hesapları, kullanıcı hesabı izni verin

>[!TIP] 
> Azure AD sağlar gibi bir **hizmet sorumlusu** uygulamanın kimliği için bir **kullanıcı asıl** bir kullanıcı kimliği için sağlanan. Kullanıcı asıl ardından Key Vault ile anahtar kasası erişim ilkesi izinleri erişim yetkisi verilebilir.

Aynı PowerShell oturumunda kullanarak, yönetilen depolama hesapları için anahtar kasası erişim ilkesini güncelleştirin. Bu adım, kullanıcı hesabınıza yönetilen depolama hesabı özellikleri eriştiğinden emin olun, depolama hesabı izinleri geçerlidir: 

```azurepowershell-interactive
# Give your user principal access to all storage account permissions, on your Key Vault instance

Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $azureProfile.Context.Account.Id -PermissionsToStorage get, list, listsas, delete, set, update, regeneratekey, recover, backup, restore, purge
```

Depolama hesapları için izinleri Azure portalında depolama hesabı "Erişim ilkeleri" sayfasında kullanılabilir olmadığını unutmayın.

## <a name="add-a-managed-storage-account-to-your-key-vault-instance"></a>Key Vault Örneğinize bir yönetilen depolama hesabı ekleme

Aynı PowerShell oturumunda kullanarak Key Vault Örneğinizde bir yönetilen depolama hesabı oluşturun. `-DisableAutoRegenerateKey` Anahtar depolama hesabı anahtarlarını yeniden değil belirtir.

```azurepowershell-interactive
# Add your storage account to your Key Vault's managed storage accounts
Add-AzKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -DisableAutoRegenerateKey
```

Hiçbir anahtarını yeniden üretme depolama hesabıyla başarılı eklenmesi sırasında aşağıdaki örneğe benzer bir çıktı görmeniz gerekir:

```console
Id                  : https://kvcontoso.vault.azure.net:443/storage/sacontoso
Vault Name          : kvcontoso
AccountName         : sacontoso
Account Resource Id : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
Active Key Name     : key1
Auto Regenerate Key : False
Regeneration Period : 90.00:00:00
Enabled             : True
Created             : 11/19/2018 11:54:47 PM
Updated             : 11/19/2018 11:54:47 PM
Tags                : 
```

### <a name="enable-key-regeneration"></a>Anahtarı yeniden üretme etkinleştir

Key Vault, depolama hesabı anahtarlarınızı düzenli aralıklarla yeniden oluşturmak istiyorsanız, bir oluşturma süresini ayarlayabilirsiniz. Aşağıdaki örnekte, bir oluşturma süresini üç gün ayarladık. Üç gün sonra Key Vault yeniden 'anahtar1' ve 'anahtar1' active 'anahtar2' anahtarını değiştirme.

```azurepowershell-interactive
$regenPeriod = [System.Timespan]::FromDays(3)
Add-AzKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -RegenerationPeriod $regenPeriod
```

Depolama hesabı anahtarını yeniden üretme sırasında başarılı eklenmesi için aşağıdaki örneğe benzer bir çıktı görmeniz gerekir:

```console
Id                  : https://kvcontoso.vault.azure.net:443/storage/sacontoso
Vault Name          : kvcontoso
AccountName         : sacontoso
Account Resource Id : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
Active Key Name     : key1
Auto Regenerate Key : True
Regeneration Period : 3.00:00:00
Enabled             : True
Created             : 11/19/2018 11:54:47 PM
Updated             : 11/19/2018 11:54:47 PM
Tags                : 
```

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen depolama hesabı anahtar örnekleri](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=)
- [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
- [Anahtar kasası PowerShell başvurusu](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)
