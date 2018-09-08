---
title: Azure depolama (Önizleme) erişmek için Azure CLI veya PowerShell komutları bir Azure AD kimlik altında çalıştırma | Microsoft Docs
description: Azure CLI ve PowerShell, Azure depolama kapsayıcıları ve Kuyruklar ve verileri komutlarını çalıştırmak için bir Azure AD kimlik oturum destekler. Bir erişim belirteci oturum için sağlanan ve arama işlemleri yetkilendirmek için kullanılır. İzinler için Azure AD kimlik atanan role göre değişir.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/29/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: b90050291a936027f66a76c14458e717b63c7257
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44090736"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell-preview"></a>CLI veya PowerShell (Önizleme) ile Azure depolamaya erişmek için bir Azure AD kimliğini kullanın.

Azure depolama, Azure CLI ve PowerShell oturum açın ve Azure Active Directory (Azure AD) kimlik altında betik komutlarını çalıştırmak etkinleştirdiğiniz için Önizleme uzantıları sağlar. Azure AD kimlik, bir kullanıcı, Grup veya uygulama hizmet sorumlusu olabilir veya olabilir bir [yönetilen hizmet kimliği](../../active-directory/managed-identities-azure-resources/overview.md). Rol tabanlı erişim denetimi (RBAC) aracılığıyla Azure AD kimlik depolama kaynaklarına erişim izinleri atayabilirsiniz. Azure Depolama'daki RBAC rolleri hakkında daha fazla bilgi için bkz. [RBAC (Önizleme) Azure depolama verileriyle Yönet erişim hakları](storage-auth-aad-rbac.md).

Azure CLI veya PowerShell ile bir Azure AD kimlik oturum açtığınızda, bu kimliği altında Azure depolama erişimi için bir erişim belirteci döndürülür. Bu belirteci daha sonra otomatik olarak CLI veya PowerShell ile Azure depolama üzerinde işlemler yetkilendirmek için kullanılır. Desteklenen işlemler için artık bir hesap anahtarı veya SAS belirteci komutu geçmesi gerekir.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="supported-operations"></a>Desteklenen işlemler

Önizleme uzantıları, kapsayıcılar ve kuyrukları işlemleri için desteklenir. Çağrı işlemleri ile Azure CLI veya PowerShell oturum Azure AD kimlik için verilen izinler bağlıdır. Rol tabanlı erişim denetimi (RBAC) Azure depolama kapsayıcıları veya sıralara izinleri atanır. Örneğin, bir veri okuyucu rolü kimliği atanır, bir kapsayıcı veya kuyruktan veri okuma betik komutları çalıştırabilirsiniz. Ardından verileri katkıda bulunan rolü kimliği atanır, okuma, yazma veya bir kapsayıcı veya kuyruk veya içerdikleri veriler silme betik komutlarını çalıştırabilirsiniz. 

Bir kapsayıcı veya sıra her bir Azure depolama işlemi için gereken izinler hakkında daha fazla ayrıntı için bkz: [REST işlemlerini çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-with-an-azure-ad-identity"></a>CLI komutları ile bir Azure AD kimliğini arayın

Azure CLI için Önizleme uzantıyı yüklemek için:

1. Sonraki veya Azure CLI sürümünü 2.0.32 yüklediğinizden emin olun. Çalıştırma `az --version` yüklü sürümünüzü denetlemek için.
2. Önizleme uzantıyı yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurecli
    az extension add -n storage-preview
    ```

Yeni bir önizleme uzantısı ekler `--auth-mode` parametresi desteklenen komutlar:

- Ayarlama `--auth-mode` parametresi `login` için bir Azure AD kimliğini kullanarak oturum açın.
- Ayarlama `--auth-mode` parametresi eski `key` hesabı için hiçbir kimlik doğrulama parametreleri için bir hesap anahtarı if sorgulama girişimi için değer sağlanır. 

Örneğin, Azure CLI kullanarak bir Azure AD kimlik bir blobu indirmek için ilk çalıştırma `az login`, ardından komut ile çağrı `--auth-mode` kümesine `login`:

```azurecli
az login
az storage blob download --account-name storagesamples --container sample-container --name myblob.txt --file myfile.txt --auth-mode login 
```

İle ilişkili ortam değişkeni `--auth-mode` parametresi `AZURE_STORAGE_AUTH_MODE`.

## <a name="call-powershell-commands-with-an-azure-ad-identity"></a>PowerShell komutları ile bir Azure AD kimliğini arayın

Bir Azure AD kimlik bilgilerinizle oturum açmak için Azure PowerShell kullanmak için:

1. PowerShellGet yüklü en son sürümüne sahip olduğunuzdan emin olun. En son yüklemek için aşağıdaki komutu çalıştırın:
 
    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber -RequiredVersion "4.4.1-preview"
    ```

2. Azure PowerShell'in önceki yüklemeleri kaldırın.
3. AzureRM yükleyin:

    ```powershell
    Install-Module AzureRM –Repository PSGallery –AllowClobber
    ```

4. Önizleme modülünü yükleyin:

    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber 
    ```

5. Çağrı [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext) bir bağlam oluşturur ve eklemek için cmdlet `-UseConnectedAccount` parametresi. 
6. Cmdlet ile bir Azure AD kimlik çağırmak için bağlamı cmdlet'e geçirin.

Aşağıdaki örnek, bir Azure AD kimliğini kullanarak Azure powershell'den bir kapsayıcıdaki blobları listelemek nasıl gösterir: 

```powershell
$ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -UseConnectedAccount 
Get-AzureStorageBlob -Container $sample-container -Context $ctx 
```

## <a name="next-steps"></a>Sonraki adımlar

- RBAC rolleri için Azure depolama hakkında daha fazla bilgi için bkz: [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md).
- Azure depolama ile Azure kaynakları için yönetilen kimlikleri kullanma hakkında bilgi edinmek için [BLOB'lar ve Kuyruklar Azure ile kimlik doğrulama erişim kimlikleri (Önizleme) Azure kaynakları için yönetilen](storage-auth-aad-msi.md).
- Kapsayıcılar ve kuyruklardaki depolama uygulamalarınızda erişim yetkisi verme konusunda bilgi almak için bkz: [depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
- Azure AD tümleştirmesi için Azure BLOB'ları ve kuyrukları hakkında ek bilgi için bkz: gönderin, Azure depolama ekibi blogu [Önizleme, Azure AD kimlik doğrulaması için Azure depolama ile tanışın](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
