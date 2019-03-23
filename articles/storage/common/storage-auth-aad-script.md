---
title: Azure depolamaya erişmek için Azure CLI veya PowerShell komutları bir Azure AD kimlik altında çalıştırma | Microsoft Docs
description: Azure CLI ve PowerShell, Azure depolama kapsayıcıları ve Kuyruklar ve verileri komutlarını çalıştırmak için bir Azure AD kimlik oturum destekler. Bir erişim belirteci oturum için sağlanan ve arama işlemleri yetkilendirmek için kullanılır. İzinler için Azure AD kimlik atanan role göre değişir.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 6c57367a3a11aeb5bdded8e19ce57b7e265aeea9
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369250"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell"></a>CLI veya PowerShell ile Azure depolamaya erişmek için bir Azure AD kimliğini kullanın.

Azure depolama, Azure CLI ve PowerShell oturum açın ve Azure Active Directory (Azure AD) kimlik altında betik komutlarını çalıştırmak etkinleştirdiğiniz için uzantılar sağlar. Azure AD kimlik, bir kullanıcı, Grup veya uygulama hizmet sorumlusu olabilir veya olabilir bir [yönetilen Azure kaynakları için kimliği](../../active-directory/managed-identities-azure-resources/overview.md). Rol tabanlı erişim denetimi (RBAC) aracılığıyla Azure AD kimlik depolama kaynaklarına erişim izinleri atayabilirsiniz. Azure Depolama'daki RBAC rolleri hakkında daha fazla bilgi için bkz. [RBAC Azure depolama verileriyle Yönet erişim hakları](storage-auth-aad-rbac.md).

Azure CLI veya PowerShell ile bir Azure AD kimlik oturum açtığınızda, bu kimliği altında Azure depolama erişimi için bir erişim belirteci döndürülür. Bu belirteci daha sonra otomatik olarak CLI veya PowerShell ile Azure depolama üzerinde işlemler yetkilendirmek için kullanılır. Desteklenen işlemler için artık bir hesap anahtarı veya SAS belirteci komutu geçmesi gerekir.

## <a name="supported-operations"></a>Desteklenen işlemler

Uzantıları, kapsayıcılar ve kuyrukları işlemleri için desteklenir. Çağrı işlemleri ile Azure CLI veya PowerShell oturumunuzu açma Azure AD kimlik için verilen izinler bağlıdır. Rol tabanlı erişim denetimi (RBAC) Azure depolama kapsayıcıları veya sıralara izinleri atanır. Örneğin, bir veri okuyucu rolü kimliği atanır, bir kapsayıcı veya kuyruktan veri okuma betik komutları çalıştırabilirsiniz. Ardından verileri katkıda bulunan rolü kimliği atanır, okuma, yazma veya bir kapsayıcı veya kuyruk veya içerdikleri veriler silme betik komutlarını çalıştırabilirsiniz. 

Bir kapsayıcı veya sıra her bir Azure depolama işlemi için gereken izinler hakkında daha fazla ayrıntı için bkz: [REST işlemlerini çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-using-azure-ad-credentials"></a>Azure AD kimlik bilgilerini kullanarak CLI komutları çağırın

Azure CLI'yı destekleyen `--auth-mode` parametresi Azure Storage'a karşı veri işlemleri için:

- Ayarlama `--auth-mode` parametresi `login` bir Azure AD güvenlik sorumlusu ile oturum açması.
- Ayarlama `--auth-mode` parametresi eski `key` hesabı için hiçbir kimlik doğrulama parametreleri için bir hesap anahtarı if sorgulama girişimi için değer sağlanır. 

Aşağıdaki örnek, Azure AD kimlik bilgilerinizi kullanarak Azure clı'dan yeni bir depolama hesabında bir kapsayıcı oluşturulacağını gösterir. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın: 

1. Sonraki veya Azure CLI sürümünü 2.0.46 yüklediğinizden emin olun. Çalıştırma `az --version` yüklü sürümünüzü denetlemek için.

1. Çalıştırma `az login` ve tarayıcı penceresi içinde kimlik doğrulaması: 

    ```azurecli
    az login
    ```
    
1. İstenen aboneliğinizi belirtin. [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) ile bir kaynak grubu oluşturun. Bu kaynak grubunu kullanarak içinde depolama hesabı oluşturma [az depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create): 

    ```azurecli
    az account set --subscription <subscription-id>

    az group create \
        --name sample-resource-group-cli \
        --location eastus

    az storage account create \
        --name <storage-account> \
        --resource-group sample-resource-group-cli \
        --location eastus \
        --sku Standard_LRS \
        --encryption-services blob
    ```
    
1. Kapsayıcı oluşturmadan önce Ata [depolama Blob verileri katkıda bulunan](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview) kendiniz rol. Hesap sahibi olmasına karşın, veri depolama hesabına yönelik açık izinler gerekir. RBAC rollerini atama hakkında daha fazla bilgi için bkz. [Azure kapsayıcıları ve Azure portalında RBAC ile kuyruk için erişim verin](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > RBAC rolü atamalarını yaymak için birkaç dakika sürebilir.
    
1. Çağrı [az depolama kapsayıcısı oluşturma](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) komutunu `--auth-mode` parametresini `login` Azure AD kimlik bilgilerinizi kullanarak kapsayıcıyı oluşturmak için:

    ```azurecli
    az storage container create \ 
        --account-name <storage-account> \ 
        --name sample-container \
        --auth-mode login
    ```

İle ilişkili ortam değişkeni `--auth-mode` parametresi `AZURE_STORAGE_AUTH_MODE`. Bir Azure depolama veri işleme her çağrıda eklenmesini önlemek için ortam değişkenine uygun değeri belirtebilirsiniz.

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Azure AD kimlik kullanarak PowerShell komutlarını çağıran

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure PowerShell oturum açın ve Azure AD kimlik bilgilerini kullanarak Azure Depolama'ya sonraki işlemleri çalıştırmak için kullanılacak depolama hesabı başvurmak için bir depolama bağlamı oluşturur ve dahil olmak üzere `-UseConnectedAccount` parametresi.

Aşağıdaki örnek, Azure AD kimlik bilgilerinizi kullanarak Azure powershell'den yeni bir depolama hesabında bir kapsayıcı oluşturulacağını gösterir. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

1. Azure aboneliğinizde oturum açın `Connect-AzAccount` izleyin ve komut ekrandaki yönergeleri, Azure AD kimlik bilgilerinizi girmek için: 

    ```powershell
    Connect-AzAccount
    ```
    
1. Çağırarak bir Azure kaynak grubu oluşturma [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. Çağırarak bir depolama hesabı oluşturma [yeni AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount).

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. Çağırarak yeni depolama hesabını belirtir. depolama hesabı bağlamını alın [yeni AzStorageContext](/powershell/module/az.storage/new-azstoragecontext). Bir depolama hesabı üzerinde hareket ederken, kimlik bilgilerini tekrar tekrar geçirmek yerine bağlam başvurabilirsiniz. Dahil `-UseConnectedAccount` Azure AD kimlik bilgilerinizi kullanarak herhangi bir sonraki veri işlemi çağırmak için parametre:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Kapsayıcı oluşturmadan önce Ata [depolama Blob verileri katkıda bulunan](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview) kendiniz rol. Hesap sahibi olmasına karşın, veri depolama hesabına yönelik açık izinler gerekir. RBAC rollerini atama hakkında daha fazla bilgi için bkz. [Azure kapsayıcıları ve Azure portalında RBAC ile kuyruk için erişim verin](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > RBAC rolü atamalarını yaymak için birkaç dakika sürebilir.

1. Çağırarak bir kapsayıcı oluşturma [yeni AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer). Bu çağrı önceki adımlarda oluşturulan bağlamı kullandığından, Azure AD kimlik bilgilerinizi kullanarak kapsayıcı oluşturulur. 

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Sonraki adımlar

- RBAC rolleri için Azure depolama hakkında daha fazla bilgi için bkz: [RBAC ile depolama verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).
- Azure depolama ile Azure kaynakları için yönetilen kimlikleri kullanma hakkında bilgi edinmek için [BLOB'lar ve Kuyruklar Azure ile kimlik doğrulama erişim kimliklerini Azure kaynakları için yönetilen](storage-auth-aad-msi.md).
- Kapsayıcılar ve kuyruklardaki depolama uygulamalarınızda erişim yetkisi verme konusunda bilgi almak için bkz: [depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).