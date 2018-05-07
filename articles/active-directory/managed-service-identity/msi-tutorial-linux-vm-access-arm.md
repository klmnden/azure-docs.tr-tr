---
title: Azure Kaynak Yöneticisi'ne erişmek için kullanıcı tarafından atanan bir Linux VM MSI kullanın
description: Azure Resource Manager erişmek için bir Linux VM üzerinde bir User-Assigned yönetilen hizmet kimliği (MSI) kullanarak sürecinde anlatan öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: arluca
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 40134f92abaa785ce76f36ec19b8888410585231
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="use-a-user-assigned-identity-on-a-linux-vm-to-access-azure-resource-manager"></a>Atanan kullanıcı kimliği, Azure Resource Manager erişmek için bir Linux VM üzerinde kullanın.

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğretici, bir kullanıcı kimliği atanır oluşturmak, bir Linux sanal makine (VM) atayın ve Azure Kaynak Yöneticisi API'si erişmek için kimliğini kullanın açıklanmaktadır. Yönetilen hizmet kimliği, Azure tarafından otomatik olarak yönetilir. Bunlar Azure AD kimlik doğrulaması, kimlik bilgileri kodunuza katıştırmak gerek kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. 

Yönetilen hizmet kimliği, Azure tarafından otomatik olarak yönetilir. Bunlar Azure AD kimlik doğrulaması, kimlik bilgileri kodunuza katıştırmak gerek kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir kullanıcı kimliği atanır oluşturun
> * Bir Linux VM kimlik atanmış kullanıcı atama 
> * Atanan kullanıcı kimliğini bir kaynak grubu Azure Kaynak Yöneticisi'nde erişim 
> * Kullanıcı kimliği atanır kullanarak bir erişim belirteci alın ve Azure Resource Manager çağırmak için kullanın 

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile tanınmayan olduğunuz kullanıma [genel bakış](overview.md) bölümü. **Gözden geçirmeyi unutmayın [sistem ve kullanıcı arasındaki farklar atanan kimlikleri](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.
- Bu öğreticide gerekli kaynak oluşturulması ve rol yönetimi adımları gerçekleştirmek için hesabınızı (abonelik veya kaynak grubunuz) uygun kapsamda "Sahip" izinleri gerekiyor. Rol ataması yardıma ihtiyacınız varsa bkz [Azure aboneliği kaynaklarınıza erişimi yönetmek üzere Use Role-Based erişim denetimi](/azure/role-based-access-control/role-assignments-portal).

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makine oluşturun

Bu öğretici için öncelikle yeni bir Linux VM oluşturun. Mevcut bir VM'yi kullanmayı tercih.

1. Tıklatın **kaynak oluşturma** Azure portalının sol üst köşedeki üzerinde.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarını** veya **parola**. Oluşturulan kimlik bilgileri, VM'ye oturum açmak izin verir.

    ![Linux VM oluşturma](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makine açılır.
5. Yeni bir seçmek için **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM boyutunu seçin. Daha fazla boyutları görmek için seçin **tüm görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

## <a name="create-a-user-assigned-identity"></a>Bir kullanıcı kimliği atanır oluşturun

1. CLI konsol (yerine bir Azure bulut kabuk oturumu) kullanıyorsanız, Azure'da oturum açma tarafından başlatın. Altında yeni kullanıcı kimliği atanır oluşturmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

    ```azurecli
    az login
    ```

2. Kullanarak bir kullanıcı tarafından atanan kimlik oluşturmak [az kimliği oluşturma](/cli/azure/identity#az_identity_create). `-g` Parametresi, burada MSI oluşturulur, kaynak grubu belirtir ve `-n` parametresi adını belirtir. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<MSI NAME>` parametre değerlerini kendi değerlerinizi ile:
    
    > [!IMPORTANT]
    > Atanan kullanıcı kimlikleri yalnızca destekler alfasayısal oluşturma ve tire (0-9 veya a-z veya A-Z veya -) karakter. Ayrıca, ad atama düzgün çalışması için VM/VMSS için 24 karakter uzunluğu sınırlı olmalıdır. Geri güncelleştirmeleri denetleyin. Daha fazla bilgi için bkz: [SSS ve bilinen sorunlar](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Yanıt kimlik oluşturulan, aşağıdaki örneğe benzer şekilde atanmış kullanıcı ayrıntılarını içerir. Not `id` sonraki adımda kullanılacak olan olarak atanan kullanıcı kimliğinizi için değer:

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=12344643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "9012",
    "resourceGroup": "<RESOURCE GROUP>",
    "tags": {},
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
    }
    ```

## <a name="assign-a-user-assigned-identity-to-your-linux-vm"></a>Kimlik, Linux VM'ye atanan kullanıcı atama

Bir kullanıcı kimliği atanır, birden çok Azure kaynaklarına istemciler tarafından kullanılabilir. Tek bir VM'ye atanan kullanıcı kimliğini atamak için aşağıdaki komutları kullanın. Kullanım `Id` özelliği döndürülen için önceki adımda `-IdentityID` parametresi.

Kullanıcı tarafından atanan MSI kullanarak, Linux VM atamak [az vm Ata-identity](/cli/azure/vm#az_vm_assign_identity). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. Kullanım `id` özelliği döndürülen için önceki adımda `--identities` parametre değeri.

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="grant-your-user-assigned-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Bir kaynak grubu Azure Kaynak Yöneticisi'nde, atanan kullanıcı kimliğini erişim izni 

Yönetilen hizmet kimliği (MSI) kodunuzu kaynak API'leri, destek Azure AD kimlik doğrulaması kimlik doğrulaması için erişim belirteci istemek için kullanabileceği kimlikleri sağlar. Bu öğreticide, Azure Resource Manager API kodunuzu erişir.  

Kodunuzu API erişebilmeniz için önce bir kaynağa Azure Kaynak Yöneticisi'nde kimlik erişim vermeniz gerekir. Bu durumda, kaynak grubu VM yer alır. Değeri güncelleştirme `<SUBSCRIPTION ID>` ve `<RESOURCE GROUP>` ortamınız için uygun şekilde. Ayrıca, yerine `<MSI PRINCIPALID>` ile `principalId` özellik tarafından döndürülen `az identity create` komutunu [kullanıcı tarafından atanan bir MSI oluşturmak](#create-a-user-assigned-msi):

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role 'Reader' --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP> "
```

Yanıt oluşturulan, aşağıdaki örneğe benzer şekilde rol ataması ayrıntılarını içerir:

```json
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}

```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>VM kimliğini kullanarak bir erişim belirteci alın ve Resource Manager çağırmak için kullanın 

Öğretici kalanı için size daha önce oluşturduğumuz sanal makineden çalışmaz.

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/about). 

1. Azure'da oturum aç [portal](https://portal.azure.com).
2. Portalı'nda gidin **sanal makineleri** ve Linux sanal makineye gidin ve buna **genel bakış**, tıklatın **Bağlan**. VM'nize bağlanmak için dizesini kopyalayın.
3. Tercih ettiğiniz SSH istemcisi VM'ye bağlanın. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/about). SSH istemcinin anahtarları yapılandırma yardıma gereksinim duyarsanız, bkz: [kullanmak SSH anahtarları nasıl Windows Azure üzerinde ile](~/articles/virtual-machines/linux/ssh-from-windows.md), veya [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmak](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).
4. Terminal penceresinde CURL, kullanarak Azure kaynak yöneticisi için bir erişim belirteci almak için Azure örneği meta veri hizmeti (IMDS) kimlik uç noktası için bir isteği oluşturun.  

   Bir erişim belirteci almak üzere CURL isteği aşağıdaki örnekte gösterilir. Değiştirdiğinizden emin olun `<CLIENT ID>` ile `clientId` özellik tarafından döndürülen `az identity create` komutunu [bir kullanıcı tarafından atanan kimliği oluşturma](#create-a-user-assigned-msi): 
    
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>"   
   ```
    
    > [!NOTE]
    > Değeri `resource` parametresi, Azure AD tarafından beklenen bir tam eşleşme olmalıdır. Resource Manager kaynak kimliği'ni kullanırken eğik URI üzerinde eklemeniz gerekir. 
    
    Yanıt, Azure Resource Manager erişim için gereken erişim belirteci içeriyor. 
    
    Yanıt örnek:  

    ```bash
    {
    "access_token":"eyJ0eXAiOi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"
    } 
    ```

5. Azure Resource Manager erişmek için erişim belirtecini kullanır ve için daha önce atanan kullanıcı kimliğini erişim izni kaynak grubunun özelliklerini okur. Değiştirdiğinizden emin olun `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` daha önce belirttiğiniz değerleri içeren ve `<ACCESS TOKEN>` önceki adımda döndürülen belirteci ile.

    > [!NOTE]
    > URL büyük/küçük harfe duyarlıdır, bu nedenle, kullanılması daha önce kaynak grubu ve büyük harf "G" adlı tam aynı durumda kullandığınızdan emin olun `resourceGroups`.  

    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Yanıt belirli kaynak grubu, aşağıdaki örneğe benzer bilgiler içerir: 

    ```bash
    {
    "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/DevTest",
    "name":"DevTest",
    "location":"westus",
    "properties":{"provisioningState":"Succeeded"}
    } 
    ```
    
## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen hizmet kimliği genel bakış için bkz: [genel bakış](overview.md).

