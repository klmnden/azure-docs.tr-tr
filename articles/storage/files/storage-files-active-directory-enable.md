---
title: Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme) - Azure depolama etkinleştirin.
description: Kimlik tabanlı kimlik doğrulaması, Azure Active Directory (Azure AD) etki alanı Hizmetleri aracılığıyla Azure dosyaları için SMB üzerinden (sunucu ileti bloğu) (Önizleme) etkinleştirme konusunda bilgi edinin. Etki alanına katılmış Windows sanal makinelerinizi (VM), ardından Azure AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarını erişebilirsiniz.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 07/05/2019
ms.author: rogarana
ms.openlocfilehash: cd4c952caa336f2602d3c30e0db3e10ebee59cb9
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67696117"
---
# <a name="enable-azure-active-directory-domain-service-authentication-over-smb-for-azure-files-preview"></a>Azure Active Directory etki alanı hizmeti kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme) için etkinleştirin.
[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Azure dosyaları SMB üzerinden Azure AD kimlik doğrulamasını genel bakış için bkz. [genel bakış, Azure Active Directory kimlik doğrulaması SMB üzerinden Azure dosyaları (Önizleme)](storage-files-active-directory-overview.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview-of-the-workflow"></a>İş akışına genel bakış
Azure dosyaları için SMB üzerinden Azure AD DS kimlik doğrulamasını etkinleştirmeden önce doğrulayın, Azure AD ve Azure depolama ortamları düzgün yapılandırılmış. İzlenecek yol, önerilen [önkoşulları](#prerequisites) gerekli tüm adımları gerçekleştirdiğiniz emin olmak için. 

Ardından, aşağıdaki adımları izleyerek Azure AD kimlik bilgileriyle Azure dosyaları kaynaklarına erişimi verin: 

1. Depolama hesabı ile ilişkili Azure AD Domain Services dağıtım kaydetmek için depolama hesabınız için SMB üzerinden Azure AD DS kimlik doğrulamasını etkinleştirin.
2. Bir Azure AD kimlik (kullanıcı, Grup veya hizmet sorumlusu) paylaşımı için erişim izinleri atayın.
3. NTFS izinleri, dizinleri ve dosyaları için SMB üzerinden yapılandırın.
4. Etki alanına katılmış bir VM'den bir Azure dosya paylaşımı bağlayın.

Azure AD DS kimlik doğrulaması, Azure dosyaları için SMB üzerinden etkinleştirmek için uçtan uca iş akışı aşağıdaki diyagramda gösterilmiştir. 

![Azure AD SMB üzerinden Azure dosyaları iş akışını gösteren diyagram](media/storage-files-active-directory-enable/azure-active-directory-over-smb-workflow.png)

## <a name="prerequisites"></a>Önkoşullar 

Azure dosyaları için SMB üzerinden Azure AD'ye etkinleştirmeden önce aşağıdaki önkoşulları tamamladığınızdan emin olun:

1.  **Seçin veya Azure AD kiracısı oluşturun.**

    Yeni veya mevcut bir kiracı SMB üzerinden Azure AD kimlik doğrulaması için kullanabilirsiniz. Kiracı ve erişmek için istediğiniz dosya paylaşımının, aynı abonelikle ilişkilendirilmiş olması gerekir.

    Yeni bir Azure AD kiracısı yapabilecekleriniz [Azure AD kiracısı Azure AD aboneliğiniz ekleyip](https://docs.microsoft.com/windows/client-management/mdm/add-an-azure-ad-tenant-and-azure-ad-subscription). Mevcut bir Azure AD kiracısına sahip, ancak Azure dosyaları ile kullanım için yeni bir kiracı oluşturmak istiyorsanız, bkz. [bir Azure Active Directory kiracısı oluşturma](https://docs.microsoft.com/rest/api/datacatalog/create-an-azure-active-directory-tenant).

2.  **Azure AD kiracısı Azure AD Domain Services'ı etkinleştirin.**

    Azure AD kimlik bilgileriyle kimlik doğrulamasını desteklemek için Azure AD kiracınız için Azure AD Domain Services'ı etkinleştirmeniz gerekir. Azure AD Kiracı yöneticisi değilseniz, yöneticisine başvurun ve adım adım yönergeleri [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/create-instance.md).

    Genellikle, bir Azure AD Domain Services dağıtımının tamamlanması yaklaşık 15 dakika sürer. Azure AD etki alanı Hizmetleri'niz sistem durumunu gösterdiğini doğrulayın **çalıştıran**, parola karma eşitlemesi etkin önce sonraki adıma devam ediliyor.

3.  **Etki alanına katılım, Azure VM ile Azure AD etki alanı Hizmetleri.**

    Bir VM'den Azure AD kimlik bilgilerini kullanarak bir dosya paylaşımına erişmek için VM, Azure AD Domain Services etki alanı ile birleşik olmalıdır. Etki alanına katılım VM hakkında daha fazla bilgi için bkz. [bir Windows Server sanal makinesi için yönetilen etki alanına Katıl](../../active-directory-domain-services/join-windows-vm.md).

    > [!NOTE]
    > Azure dosyaları ile SMB üzerinden Azure AD DS kimlik doğrulaması, yalnızca işletim sistemi sürümleri, Windows 7 veya Windows Server 2008 R2 üzerinde çalışan Azure Vm'leri üzerinde desteklenir.

4.  **Seçin veya bir Azure dosya paylaşımı oluşturun.**

    Azure AD kiracınız ile aynı abonelikle ilişkilendirilmiş bir yeni veya mevcut dosya paylaşımını seçin. Yeni bir dosya paylaşımı oluşturma hakkında daha fazla bilgi için bkz: [Azure dosyaları'nda bir dosya paylaşımı oluşturma](storage-how-to-create-file-share.md). 
    En iyi performans için dosya paylaşımınızı paylaşıma erişim planladığınız VM ile aynı bölgede olan Microsoft önerir.

5.  **Depolama hesabı anahtarınızı kullanarak Azure dosya paylaşımlarını bağlayarak Azure dosyaları bağlantısını kontrol edin.**

    Sanal makine ve dosya paylaşımınızı düzgün yapılandırıldığından emin doğrulamak için depolama hesabı anahtarınızı kullanarak dosya paylaşımını bağlama deneyin. Daha fazla bilgi için [bir Azure dosya paylaşımını bağlama ve Windows paylaşımına erişim](storage-how-to-use-files-windows.md).

## <a name="enable-azure-ad-ds-authentication-for-your-account"></a>Hesabınız için Azure AD DS kimlik doğrulamasını etkinleştir

Azure AD DS kimlik doğrulaması, Azure dosyaları için SMB üzerinden etkinleştirmek için bir özellik 24 Eylül 2018'den sonra oluşturulan depolama hesapları Azure portalı, Azure PowerShell veya Azure CLI kullanarak ayarlayabilirsiniz. Bu özelliğin ayarlanması, depolama hesabı ile ilişkili Azure AD Domain Services dağıtım kaydeder. SMB üzerinden Azure AD DS kimlik doğrulaması, ardından depolama hesabındaki tüm yeni ve mevcut dosya paylaşımları için etkinleştirilir. 

Yalnızca başarıyla Azure AD Domain Services, Azure AD kiracınıza dağıttıktan sonra SMB üzerinden Azure AD DS kimlik doğrulamasını etkinleştirmek göz önünde bulundurun. Daha fazla bilgi için [önkoşulları](#prerequisites).

### <a name="azure-portal"></a>Azure portal

SMB kullanarak üzerinden Azure AD DS kimlik doğrulamasını etkinleştirmek için [Azure portalında](https://portal.azure.com), şu adımları izleyin:

1. Azure portalında, mevcut depolama hesabınıza gidin veya [depolama hesabı oluşturma](../common/storage-quickstart-create-account.md).
2. İçinde **ayarları** bölümünden **yapılandırma**.
3. Etkinleştirme **Azure Active Directory kimlik doğrulaması için Azure dosyaları (Önizleme)** .

Aşağıdaki resimde, Azure AD kimlik doğrulaması üzerinden SMB depolama hesabınız için etkinleştirmek gösterilmektedir.

![Azure AD kimlik doğrulaması SMB üzerinden Azure portalında etkinleştirin.](media/storage-files-active-directory-enable/portal-enable-active-directory-over-smb.png)
  
### <a name="powershell"></a>PowerShell  

SMB üzerinden Azure PowerShell kullanarak Azure AD DS kimlik doğrulamasını etkinleştirmek için: İlk olarak en son Az modül (2.4 veya daha yeni) veya Az.Storage Modülü (1.5 veya daha yeni) yükleyin. PowerShell'i yükleme hakkında daha fazla bilgi için bkz. [PowerShellGet ile Windows üzerindeki Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps):

```powershell
Install-Module -Name Az.Storage -AllowPrerelease -Force -AllowClobber
```

Ardından, yeni bir depolama oluşturma çağrı sonra hesap [kümesi AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount) ve **EnableAzureActiveDirectoryDomainServicesForFile** parametresi **true**. Aşağıdaki örnekte, yer tutucu değerlerini kendi değerlerinizle değiştirin unutmayın. (Önceki Önizleme modülünü kullanıyorsanız, özellik etkinleştirme parametredir **EnableAzureFilesAadIntegrationForSMB**.)

```powershell
# Create a new storage account
New-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -Location "<azure-region>" `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```
Mevcut depolama hesapları bu özelliği etkinleştirmek için aşağıdaki komutu kullanın:

```powershell
# Update a storage account
Set-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```


### <a name="azure-cli"></a>Azure CLI

Azure CLI 2.0 SMB üzerinden Azure AD kimlik doğrulamasını etkinleştirmek için önce yükleme `storage-preview` uzantısı:

```cli-interactive
az extension add --name storage-preview
```
  
Ardından, yeni bir depolama oluşturma çağrı sonra hesap [az depolama hesabını güncelleştirme](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-update) ve `--file-aad` özelliğini **true**. Aşağıdaki örnekte, yer tutucu değerlerini kendi değerlerinizle değiştirin unutmayın.

```azurecli-interactive
# Create a new storage account
az storage account create -n <storage-account-name> -g <resource-group-name> --file-aad true
```

## <a name="assign-access-permissions-to-an-identity"></a>Bir kimlik için erişim izinleri atayın 

Azure AD kimlik bilgilerini kullanarak Azure dosyaları kaynaklarına erişmek için bir kimlik (kullanıcı, Grup veya hizmet sorumlusu) paylaşım düzeyinde gerekli izinlere sahip olmalıdır. Bu işlem belirli bir kullanıcı bir dosya paylaşımına sahip erişimi türünü belirttiğiniz Windows paylaşım izinleri, belirtmeye benzerdir. Bu bölümdeki yönergeler, okuma, atamak gösterilmiştir yazma veya silme bir kimlik için bir dosya paylaşımı için izinleri.

Kullanıcılara paylaşım düzeyi izinleri vermek için iki Azure yerleşik roller ekledik: Depolama dosya verileri SMB paylaşımı okuyucu ve depolama dosya verileri SMB paylaşımı katkıda bulunan. 

- **Depolama dosya verileri SMB paylaşımı okuyucu** SMB üzerinden dosya paylaşımlarını Azure depolama okuma erişimi verir
- **Depolama dosya verileri SMB paylaşımı katkıda bulunan** okuma, yazma ve silme erişimi SMB üzerinden dosya paylaşımlarını Azure depolama sağlar.

> [!IMPORTANT]
> Depolama hesabı anahtarını kullanarak bir kimlik için rol atama olanağı dahil olmak üzere bir dosya paylaşımının tam yönetim denetimi gerektirir. Azure AD kimlik bilgileriyle yönetim denetimi desteklenmiyor. 

Paylaşım düzeyi izinleri vermek için bir kullanıcı Azure AD kimliğine nasıl yerleşik rolleri atamak için Azure portal, PowerShell veya Azure CLI'yı kullanabilirsiniz. 

#### <a name="azure-portal"></a>Azure portal
Bir Azure AD kimlik için RBAC rolü atamak kullanarak [Azure portalında](https://portal.azure.com), şu adımları izleyin:

1. Dosya paylaşımınızı Azure portalında gezinme veya [Azure dosyaları'nda bir dosya paylaşımı oluşturma](storage-how-to-create-file-share.md).
2. Seçin **erişim denetimi (IAM)** .
3. Seçin **bir rol ataması Ekle**
4. İçinde **rol ataması Ekle** dikey penceresinde, (depolama dosya verileri SMB paylaşımı Okuyucu, depolama dosya verileri SMB paylaşımı katkıda bulunan) uygun yerleşik rolü seçin **rol** açılır liste. Tutun **erişim Ata** varsayılan olarak: "Azure AD kullanıcı, Grup veya hizmet sorumlusu" ve hedef Azure AD kimlik adı veya e-posta adresine göre seçin. 
5. Son olarak, seçin **Kaydet** rol atama işlemi tamamlamak için.

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell komutu, oturum açma adına dayalı bir Azure AD kimlik RBAC rolü atamak gösterilmektedir. PowerShell ile RBAC rollerini atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure PowerShell kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-powershell.md).

Aşağıdaki örnek komut dosyasını çalıştırırken, yer tutucu değerlerini kendi değerlerinizle köşeli ayraçlar dahil, unutmayın.

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

#### <a name="cli"></a>CLI
  
Aşağıdaki CLI 2.0 komutu, oturum açma adına dayalı bir Azure AD kimlik RBAC rolü atamak gösterilmektedir. Azure CLI ile RBAC rollerini atama hakkında daha fazla bilgi için bkz. [RBAC ve Azure CLI kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-cli.md). 

Aşağıdaki örnek komut dosyasını çalıştırırken, yer tutucu değerlerini kendi değerlerinizle köşeli ayraçlar dahil, unutmayın.

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
```

## <a name="configure-ntfs-permissions-over-smb"></a>SMB üzerinden NTFS izinleri yapılandırma 
RBAC ile paylaşım düzeyi izinleri atadıktan sonra kök, dizin veya dosya düzeyinde uygun NTFS izinleri atamanız gerekir. NTFS izinleri hangi işlemleri belirlemek için daha ayrıntılı bir düzeyde kullanıcı hareket ederken, paylaşım kullanıcı erişip erişemeyeceğini belirler üst düzey bir ağ geçidi dizin veya dosya düzeyinde gerçekleştirebileceğiniz paylaşım düzeyi izinlerini düşünün. 

Azure dosyaları NTFS temel ve gelişmiş izinler kümesini destekler. Görüntüleyebilir ve paylaşımı bağlayarak ve ardından Windows çalıştıran dizinleri ve dosyaları Azure dosya paylaşımının, NTFS izinlerini yapılandırın [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) veya [kümesi ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl) komutu. 

> [!NOTE]
> Önizleme sürümü yalnızca görüntüleme izinleri Windows dosya Gezgini ile destekler. Düzenleme izinlerine henüz desteklenmiyor.

Süper kullanıcı ayrıcalıkları olan NTFS izinlerini yapılandırmak için etki alanına katılmış VM'den depolama hesabı anahtarınız ile paylaşım bağlamalısınız. Komut isteminden bir Azure dosya paylaşımını bağlama ve NTFS izinleri uygun şekilde yapılandırmak için sonraki bölümünde yer alan yönergeleri izleyin.

Aşağıdaki adımlardan birini izinler dosya paylaşım kök dizininin desteklenir:

- BUILTIN\Administrators:(OI)(CI)(F)
- NT AUTHORITY\SYSTEM:(OI)(CI)(F)
- BUILTIN\Users:(RX)
- BUILTIN\USERS:(OI)(CI)(IO)(GR,ge)
- NT AUTHORITY\Authenticated Users:(OI)(CI)(M)
- NT AUTHORITY\SYSTEM:(F)
- OLUŞTURUCU OWNER:(OI)(CI)(IO)(F)

### <a name="mount-a-file-share-from-the-command-prompt"></a>Komut isteminden bir dosya paylaşımını bağlama

Windows kullanan **net kullanım** Azure dosya paylaşımını bağlayabilmeniz için komutu. Örnek yer tutucu değerleri kendi değerlerinizle değiştirin unutmayın. Dosya paylaşımlarını bağlamayı hakkında daha fazla bilgi için bkz: [bir Azure dosya paylaşımını bağlama ve Windows paylaşımına erişim](storage-how-to-use-files-windows.md).

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
```

### <a name="configure-ntfs-permissions-with-icacls"></a>NTFS izinlerini icacls ile yapılandırma
Tüm dizin ve dosyaların kök dizinine dahil olmak üzere dosya paylaşımının altında tam izinleri vermek için aşağıdaki Windows komutu kullanın. Yer tutucu değerlerini kendi değerlerinizle örnekte parantez içinde gösterilen unutmayın.

```
icacls <mounted-drive-letter>: /grant <user-email>:(f)
```

NTFS izinleri ayarlamanızı ve desteklenen izinleri farklı türden üzerinde görmek için icacls kullanma hakkında daha fazla bilgi için [icacls için komut satırı başvurusu](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

## <a name="mount-a-file-share-from-a-domain-joined-vm"></a>Etki alanına katılmış bir VM'den bir dosya paylaşımını bağlama 

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
