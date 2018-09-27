---
title: Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme) - Azure depolama etkinleştirin.
description: Kimlik tabanlı kimlik doğrulaması, Azure Active Directory (Azure AD) etki alanı Hizmetleri aracılığıyla Azure dosyaları için SMB üzerinden (sunucu ileti bloğu) (Önizleme) etkinleştirme konusunda bilgi edinin. Etki alanına katılmış Windows sanal makinelerinizi (VM), ardından Azure AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarını erişebilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/19/2018
ms.author: tamram
ms.openlocfilehash: ec8ad5a509b4fd4b6fd59212ac0df17f98f417fd
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222446"
---
# <a name="enable-azure-active-directory-authentication-over-smb-for-azure-files-preview"></a>Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme) için etkinleştirin.

[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Azure dosyaları SMB üzerinden Azure AD kimlik doğrulamasını genel bakış için bkz. [genel bakış, Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme)](storage-files-active-directory-overview.md).

## <a name="workflow-overview"></a>İş akışı genel bakış

Azure dosyaları için SMB üzerinden Azure AD'ye etkinleştirmeden önce doğrulayın, Azure AD ve Azure depolama ortamları düzgün yapılandırılmış. İzlenecek yol, önerilen [önkoşulları](#prerequisites) gerekli tüm adımları gerçekleştirdiğiniz emin olmak için. 

Ardından, aşağıdaki adımları izleyerek Azure AD kimlik bilgileriyle Azure dosyaları kaynaklarına erişimi verin: 

1. Depolama hesabı ile ilişkili Azure AD Domain Services dağıtım kaydetmek için depolama hesabınız için SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirin.
2. Bir Azure AD kimlik (kullanıcı, Grup veya hizmet sorumlusu) paylaşımı için erişim izinleri atayın.
3. NTFS izinleri, dizinleri ve dosyaları için SMB üzerinden yapılandırın.
4. Etki alanına katılmış bir VM'den bir Azure dosya paylaşımı bağlayın.

Azure AD kimlik doğrulaması, Azure dosyaları için SMB üzerinden etkinleştirmek için uçtan uca iş akışı aşağıdaki diyagramda gösterilmiştir. 

![Azure AD SMB üzerinden Azure dosyaları iş akışını gösteren diyagram](media/storage-files-active-directory-enable/azure-active-directory-over-smb-workflow.png)

## <a name="prerequisites"></a>Önkoşullar 

1.  **Seçin veya Azure AD kiracısı oluşturun.**

    Yeni veya mevcut bir kiracı SMB üzerinden Azure AD kimlik doğrulaması için kullanabilirsiniz. Kiracı ve erişmek için istediğiniz dosya paylaşımının, aynı abonelikle ilişkilendirilmiş olması gerekir.

    Yeni bir Azure AD kiracısı yapabilecekleriniz [Azure AD kiracısı Azure AD aboneliğiniz ekleyip](https://docs.microsoft.com/windows/client-management/mdm/add-an-azure-ad-tenant-and-azure-ad-subscription). Mevcut bir Azure AD kiracısına sahip, ancak Azure dosyaları ile kullanım için yeni bir kiracı oluşturmak istiyorsanız, bkz. [bir Azure Active Directory kiracısı oluşturma](https://docs.microsoft.com/rest/api/datacatalog/create-an-azure-active-directory-tenant).

2.  **Azure AD kiracısı Azure AD Domain Services'ı etkinleştirin.**

    Azure AD kimlik bilgileriyle kimlik doğrulamasını desteklemek için Azure AD kiracınız için Azure AD Domain Services'ı etkinleştirmeniz gerekir. Azure AD Kiracı yöneticisi değilseniz, yöneticisine başvurun ve adım adım yönergeleri [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/active-directory-ds-getting-started.md).

    Genellikle, bir Azure AD Domain Services dağıtımının tamamlanması yaklaşık 15 dakika sürer. Azure AD etki alanı Hizmetleri'niz sistem durumunu gösterdiğini doğrulayın **çalıştıran**, parola karma eşitlemesi etkin önce sonraki adıma devam ediliyor.

3.  **Etki alanına katılım, Azure VM ile Azure AD etki alanı Hizmetleri.**

    Bir VM'den Azure AD kimlik bilgilerini kullanarak bir dosya paylaşımına erişmek için VM, Azure AD Domain Services etki alanı ile birleşik olmalıdır. Etki alanına katılım VM hakkında daha fazla bilgi için bkz. [bir Windows Server sanal makinesi için yönetilen etki alanına Katıl](../../active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal.md).

    > [!NOTE]
    > Azure dosyaları ile SMB üzerinden Azure AD kimlik doğrulaması, yalnızca işletim sistemi sürümleri, Windows 7 veya Windows Server 2008 R2 üzerinde çalışan Azure Vm'leri üzerinde desteklenir.

4.  **Seçin veya bir Azure dosya paylaşımı oluşturun.**

    Azure AD kiracınız ile aynı abonelikle ilişkilendirilmiş bir yeni veya mevcut dosya paylaşımını seçin. Yeni bir dosya paylaşımı oluşturma hakkında daha fazla bilgi için bkz: [Azure dosyaları'nda bir dosya paylaşımı oluşturma](storage-how-to-create-file-share.md). 

    SMB üzerinden Azure AD Önizleme için desteklenen bir bölge için Azure AD kiracısını dağıtılması gerekir. Dışındaki tüm ortak bölgelerde Önizleme kullanılabilir: Batı ABD, Batı ABD 2, Orta Güney ABD, Doğu ABD, Doğu ABD 2, Orta ABD, Kuzey Orta ABD, Doğu Avustralya, Batı Avrupa, Kuzey Avrupa.

    En iyi performans için dosya paylaşımınızı paylaşıma erişim planladığınız VM ile aynı bölgede olan Microsoft önerir.

5.  **Depolama hesabı anahtarınızı kullanarak Azure dosya paylaşımlarını bağlayarak Azure dosyaları bağlantısını kontrol edin.**

    Sanal makine ve dosya paylaşımınızı düzgün yapılandırıldığından emin doğrulamak için depolama hesabı anahtarınızı kullanarak dosya paylaşımını bağlama deneyin. Daha fazla bilgi için [bir Azure dosya paylaşımını bağlama ve Windows paylaşımına erişim](storage-how-to-use-files-windows.md).

## <a name="enable-azure-ad-authentication"></a>Azure AD kimlik doğrulamasını etkinleştir

Tamamladıktan sonra [önkoşulları](#prerequisites), SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirebilirsiniz.

### <a name="step-1-enable-azure-ad-authentication-over-smb-for-your-storage-account"></a>1. adım: SMB üzerinden Azure AD kimlik doğrulaması, depolama hesabınız için etkinleştirin.

Azure AD kimlik doğrulaması, Azure dosyaları için SMB üzerinden etkinleştirmek için bir özellik 29 Ağustos 2018'den sonra oluşturulan depolama hesapları PowerShell veya Azure CLI'yı Azure depolama kaynak Sağlayıcısı'nı kullanarak ayarlayabilirsiniz. Azure portalında özelliğini ayarlayarak Önizleme sürümü için desteklenmiyor. 

Bu özelliğin ayarlanması, depolama hesabı ile ilişkili Azure AD Domain Services dağıtım kaydeder. SMB üzerinden Azure AD kimlik doğrulaması, ardından depolama hesabındaki tüm yeni ve mevcut dosya paylaşımları için etkinleştirilir. 

Yalnızca başarıyla Azure AD Domain Services, Azure AD kiracınıza dağıttıktan sonra SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirmek göz önünde bulundurun. Daha fazla bilgi için [önkoşulları](#prerequisites).

**PowerShell**

SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirmek için yükleme `AzureRM.Storage 6.0.0-preview` PowerShell modülü. PowerShell'i yükleme hakkında daha fazla bilgi için bkz: [PowerShellGet ile Windows üzerindeki Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

Ardından, arama [Set-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/set-azurermstorageaccount) ayarlayıp **EnableAzureFilesAadIntegrationForSMB** parametresi **true**. Aşağıdaki örnekte, yer tutucu değerlerini kendi değerlerinizle değiştirin unutmayın.

```powershell
# Create a new storage account
New-AzureRmStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -Location "<azure-region>" `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableAzureFilesAadIntegrationForSMB $true

# Update an existing storage account
# Supported for storage accounts created after August 29, 2018 only
Set-AzureRmStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -EnableAzureFilesAadIntegrationForSMB $true```
```

**CLI**

Azure CLI 2.0 SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirmek için önce yükleme *depolama Önizleme* uzantısı:

```azurecli-interactive
az extension add --name storage-preview
```

Ardından, arama [az depolama hesabını güncelleştirme](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-update) ayarlayıp `--file-aad` özelliğini **true**. Aşağıdaki örnekte, yer tutucu değerlerini kendi değerlerinizle değiştirin unutmayın.

```azurecli-interactive
# Create a new storage account
az storage account create -n <storage-account-name> -g <resource-group-name> --file-aad true

# Update an existing storage account
# Supported for storage accounts created after August 29, 2018 only
az storage account update -n <storage-account-name> -g <resource-group-name> --file-aad true
```

### <a name="step-2-assign-access-permissions-to-an-identity"></a>2. adım: Ata erişim izinleri bir kimlik 

Azure AD kimlik bilgilerini kullanarak Azure dosyaları kaynaklarına erişmek için bir kimlik (kullanıcı, Grup veya hizmet sorumlusu) paylaşım düzeyinde gerekli izinlere sahip olmalıdır. Aşağıdaki adım adım yönergeleri, okuma, atamak gösterilmiştir yazma veya silme bir kimlik için bir dosya paylaşımı için izinleri.

> [!IMPORTANT]
> Depolama hesabı anahtarını kullanarak bir kimlik için rol atama olanağı dahil olmak üzere bir dosya paylaşımının tam yönetim denetimi gerektirir. Yönetim denetimi, Azure AD kimlik bilgileriyle desteklenmez. 

#### <a name="step-21-define-a-custom-role"></a>2.1. adım: özel bir rol tanımlama

Paylaşım düzeyi izinleri vermek için özel bir RBAC rolü tanımlamak ve belirli bir dosya paylaşımına kapsamı, bir kimlik atayın. Bu işlem belirli bir kullanıcı bir dosya paylaşımına sahip erişimi türünü belirttiğiniz Windows paylaşım izinleri, belirtmeye benzerdir.  

Aşağıdaki bölümlerde gösterilen şablonları, bir dosya paylaşımı için okuma veya değiştirme izinleri sağlar. Özel rol tanımlamak için bir JSON dosyası oluşturun ve uygun şablonu bu dosyaya kopyalayın. Özel bir RBAC rollerini tanımlama hakkında daha fazla bilgi için bkz. [azure'da özel roller](../../role-based-access-control/custom-roles.md).

**Rol tanımı için paylaşım düzeyi izinlerini değiştirme**

Aşağıdaki özel rolü şablonu, bir kimlik okuma, yazma ve silme erişimi paylaşımına verme paylaşım düzeyi değiştirme izinleri sağlar.

```json
{
  "Name": "<Custom-Role-Name>",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read, write and delete access to Azure File Share",
  "Actions": [
    "*"
  ],
  "NotActions": [
      "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ],
  "DataActions": [
    "*"
  ],
  "AssignableScopes": [
        "/subscriptions/<Subscription-ID>"
  ]
}
```

**Rol tanımı paylaşım düzeyi Okuma izinleri**

Aşağıdaki özel rolü şablonu, paylaşım düzeyi Okuma izinleri, paylaşıma bir kimlik okuma erişimi verme sağlar.

```json
{
  "Name": "<Custom-Role-Name>",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure File Share",
  "Actions": [
    "*/read"
  ],
  "DataActions": [
    "*/read"
  ],
  "AssignableScopes": [
        "/subscriptions/<Subscription-ID>"
  ]
}
```

#### <a name="step-22-create-the-custom-role-and-assign-it-to-the-target-identity"></a>2.2. adım: özel rol oluşturma ve hedef kimliğine atayın

Ardından, bir rol oluşturmak ve bir Azure AD kimliğine nasıl atamak için PowerShell veya Azure CLI kullanın. 

**PowerShell**

SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirmek için yükleme `AzureRM.Storage 6.0.0-preview` PowerShell modülü. PowerShell'i yükleme hakkında daha fazla bilgi için bkz: [PowerShellGet ile Windows üzerindeki Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

Aşağıdaki PowerShell komutu, özel bir rol oluşturur ve oturum açma adına dayalı bir Azure AD kimlik rolü atar. PowerShell ile RBAC rollerini atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure PowerShell kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-powershell.md).

Aşağıdaki örnek komut dosyasını çalıştırırken, yer tutucu değerlerini kendi değerlerinizle değiştirin unutmayın.

```powershell
#Create a custom role based on the sample template above
New-AzureRmRoleDefinition -InputFile "<custom-role-def-json-path>"
#Get the name of the custom role
$FileShareContributorRole = Get-AzureRmRoleDefinition "<role-name>"
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzureRmRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

**CLI**

Aşağıdaki CLI 2.0 komut özel bir rol oluşturur ve oturum açma adına dayalı bir Azure AD kimlik rolü atar. Azure CLI ile RBAC rollerini atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure CLI kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-cli.md). 

Aşağıdaki örnek komut dosyasını çalıştırırken, yer tutucu değerlerini kendi değerlerinizle değiştirin unutmayın.

```azurecli-interactive
#Create a custom role based on the sample templates above
az role definition create --role-definition "<Custom-role-def-JSON-path>"
#List the custom roles
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
#Assign the custom role to the target identity
az role assignment create --role "<custome-role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
```

### <a name="step-3-configure-ntfs-permissions-over-smb"></a>3. adım: SMB üzerinden NTFS izinleri yapılandırma 

RBAC ile paylaşım düzeyi izinleri atadıktan sonra kök, dizin veya dosya düzeyinde uygun NTFS izinleri atamanız gerekir. NTFS izinleri hangi işlemleri belirlemek için daha ayrıntılı bir düzeyde kullanıcı hareket ederken, paylaşım kullanıcı erişip erişemeyeceğini belirler üst düzey bir ağ geçidi dizin veya dosya düzeyinde gerçekleştirebileceğiniz paylaşım düzeyi izinlerini düşünün. 

Azure dosyaları NTFS temel ve gelişmiş izinler kümesini destekler. Görüntüleyebilir ve paylaşımı bağlayarak ve ardından Windows çalıştıran dizinleri ve dosyaları Azure dosya paylaşımının, NTFS izinlerini yapılandırın [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) veya [kümesi ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl) komutu. 

> [!NOTE]
> Önizleme sürümü yalnızca görüntüleme izinleri Windows dosya Gezgini ile destekler. Düzenleme izinlerine henüz desteklenmiyor.

Süper kullanıcı ayrıcalıkları olan NTFS izinlerini yapılandırmak için etki alanına katılmış VM'den depolama hesabı anahtarınız ile paylaşım bağlamalısınız. Komut isteminden bir Azure dosya paylaşımını bağlama ve NTFS izinleri uygun şekilde yapılandırmak için sonraki bölümünde yer alan yönergeleri izleyin.

Aşağıdaki adımlardan birini izinler dosya paylaşım kök dizininin desteklenir:

- BUILTIN\Administrators:(OI)(CI)(F)
- NT AUTHORITY\SYSTEM:(OI)(CI)(F)
- BUILTIN\USERS:(rx)
- BUILTIN\USERS:(OI)(CI)(IO)(GR,ge)
- NT AUTHORITY\Authenticated Users:(OI)(CI)(M)
- NT AUTHORITY\SYSTEM:(F)
- OLUŞTURUCU OWNER:(OI)(CI)(IO)(F)

#### <a name="step-31-mount-an-azure-file-share-from-the-command-prompt"></a>Adım 3.1, komut isteminden Azure dosya paylaşımını bağlama

Windows kullanan **net kullanım** Azure dosya paylaşımını bağlayabilmeniz için komutu. Örnek yer tutucu değerleri kendi değerlerinizle değiştirin unutmayın. Dosya paylaşımlarını bağlamayı hakkında daha fazla bilgi için bkz: [bir Azure dosya paylaşımını bağlama ve Windows paylaşımına erişim](storage-how-to-use-files-windows.md).

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
```

#### <a name="step-32-configure-ntfs-permissions-with-icacls"></a>Adım 3.2 yapılandırma NTFS izinleri icacls ile

Tüm dizin ve dosyaların kök dizinine dahil olmak üzere dosya paylaşımının altında tam izinleri vermek için aşağıdaki Windows komutu kullanın. Örnek yer tutucu değerleri kendi değerlerinizle değiştirin unutmayın.

```
icacls <mounted-drive-letter> /grant <user-email>:(f)
```

NTFS izinleri ayarlamanızı ve desteklenen izinleri farklı türden üzerinde görmek için icacls kullanma hakkında daha fazla bilgi için [icacls için komut satırı başvurusu](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

### <a name="step-4-mount-an-azure-file-share-from-a-domain-joined-vm"></a>4. adım: etki alanına katılmış bir VM'den bir Azure dosya paylaşımını bağlama 

Yukarıdaki adımları başarıyla kullanarak tamamladınız olduğunu doğrulamak artık etki alanına katılmış bir VM'den bir Azure dosya erişmek için Azure AD kimlik bilgilerinizi paylaşın. İlk olarak, aşağıdaki görüntüde gösterildiği gibi izinleri vermiş olması Azure AD kimlik kullanarak VM'ye oturum açın.

![Oturum açma ekran görüntüsü gösteren Azure AD ekran için kullanıcı kimlik doğrulaması](media/storage-files-active-directory-enable/azure-active-directory-authentication-dialog.png)

Ardından, Azure dosya paylaşımını bağlamak için aşağıdaki komutu kullanın. Yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın. Zaten doğrulandı olduğundan, depolama hesabı anahtarı veya Azure AD kullanıcı adı ve parola sağlamanız gerekmez. SMB üzerinden Azure AD, Azure AD kimlik bilgilerini kullanarak çoklu oturum açma deneyimini destekler.

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```

Artık başarıyla SMB üzerinden Azure AD kimlik doğrulaması etkin ve bir Azure AD kimlik için bir dosya paylaşımına erişim sağlayan bir özel rolü atanmış. Ek kullanıcılar için dosya paylaşımına erişim izni vermek için 2 ve 3. adımda verilen yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure dosyaları ve SMB üzerinden Azure AD kullanma hakkında daha fazla bilgi için şu kaynaklara bakın:

- [Azure dosyaları'na giriş](storage-files-introduction.md)
- [SMB üzerinden Azure Active Directory kimlik doğrulaması için Azure dosyaları (Önizleme) genel bakış](storage-files-active-directory-overview.md)
- [SSS](storage-files-faq.md)
