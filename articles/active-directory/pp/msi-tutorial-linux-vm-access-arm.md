---
title: "Azure Kaynak Yöneticisi'ne erişmek için kullanıcı tarafından atanan bir Linux VM MSI kullanın"
description: "Azure Resource Manager erişmek için bir Linux VM üzerinde bir User-Assigned yönetilen hizmet kimliği (MSI) kullanarak sürecinde anlatan öğretici."
services: active-directory
documentationcenter: 
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
ms.openlocfilehash: c2b6d70e441dc3d300f49adff1c02d7cc65788d2
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-resource-manager"></a>Bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI), Azure Resource Manager erişmek için bir Linux VM üzerinde kullanın.

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğretici, bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) oluşturmak, bir Linux sanal makine (VM) atayın ve ardından Azure Kaynak Yöneticisi API'si erişmek için kimliğini kullanın açıklanmaktadır. 

Yönetilen hizmet kimliği, Azure tarafından otomatik olarak yönetilir. Bunlar Azure AD kimlik doğrulaması, kimlik bilgileri kodunuza katıştırmak gerek kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Kullanıcı tarafından atanan bir MSI oluşturma
> * Bir Linux VM MSI atayın 
> * Bir kaynak grubu Azure Kaynak Yöneticisi'nde MSI erişim 
> * MSI kullanarak bir erişim belirteci alın ve Azure Resource Manager çağırmak için kullanın 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Bu öğreticide CLI komut dosyası örnekleri çalıştırmak için iki seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](~/articles/cloud-shell/overview.md) Azure portalından veya "deneyin" düğmesini, aracılığıyla her kod bloğunun sağ üst köşesinde bulunan.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Oturum açmak için Azure portalında [ https://portal.azure.com ](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makine oluşturun

Bu öğretici için öncelikle yeni bir Linux VM oluşturun. Mevcut bir VM'yi kullanmayı tercih.

1. Tıklatın **kaynak oluşturma** Azure portalının sol üst köşedeki üzerinde.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarını** veya **parola**. Oluşturulan kimlik bilgileri, VM'ye oturum açmak izin verir.

    ![Linux VM oluşturma](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makine açılır.
5. Yeni bir seçmek için **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM boyutunu seçin. Daha fazla boyutları görmek için seçin **tüm görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

## <a name="create-a-user-assigned-msi"></a>Kullanıcı tarafından atanan bir MSI oluşturma

1. CLI konsol (yerine bir Azure bulut kabuk oturumu) kullanıyorsanız, Azure'da oturum açma tarafından başlatın. Altında yeni MSI oluşturmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

    ```azurecli
    az login
    ```

2. Bir kullanıcı tarafından atanan MSI kullanarak oluşturduğunuz [az kimliği oluşturma](/cli/azure/identity#az_identity_create). `-g` Parametresi, burada MSI oluşturulur, kaynak grubu belirtir ve `-n` parametresi adını belirtir. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<MSI NAME>` parametre değerlerini kendi değerlerinizi ile:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Yanıt, kullanıcı tarafından atanan MSI oluşturulan, aşağıdaki örneğe benzer şekilde ayrıntıları içerir. Not `id` sonraki adımda kullanılacağından, MSI, değer:

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

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>Kullanıcı tarafından atanan MSI Linux VM'NİZDE atayın

Sistem tarafından atanan bir MSI, bir kullanıcı tarafından atanan MSI birden çok Azure kaynaklarına istemciler tarafından kullanılabilir. Bu öğretici için bunu tek bir VM öğesine atayın. Ayrıca birden fazla VM atayabilirsiniz.

Kullanıcı tarafından atanan MSI kullanarak, Linux VM atamak [az vm Ata-identity](/cli/azure/vm#az_vm_assign_identity). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. Kullanım `id` özelliği döndürülen için önceki adımda `--identities` parametre değeri:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="grant-your-user-assigned-msi-access-to-a-resource-group-in-azure-resource-manager"></a>Bir kaynak grubu Azure Kaynak Yöneticisi'nde, kullanıcı tarafından atanan MSI erişim izni 

MSI kodunuzu Azure AD kimlik doğrulama destekler API'lerini kaynak için kimlik doğrulaması için bir erişim belirteci ile sağlar. Bu öğreticide, Azure Resource Manager API kodunuzu erişir. 

Kodunuzu API ancak erişebilmeniz için önce bir kaynağa Azure Kaynak Yöneticisi'nde MSI kimlik erişim vermeniz gerekir. Bu durumda, kaynak grubu VM yer alır. İçin değerleri güncelleştirmek `<SUBSCRIPTION ID>` ve `<RESOURCE GROUP>` ortamınız için uygun şekilde. Ayrıca, yerine `<MSI PRINCIPALID>` ile `principalId` özellik tarafından döndürülen `az identity create` komutunu [kullanıcı tarafından atanan bir MSI oluşturmak](#create-a-user-assigned-msi):

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

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/about). SSH istemcinin anahtarları yapılandırma yardıma gereksinim duyarsanız, bkz: [kullanmak SSH anahtarları nasıl Windows Azure üzerinde ile](~/articles/virtual-machines/linux/ssh-from-windows.md), veya [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmak](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında gidin **sanal makineleri**gidin, Linux sanal makine, daha sonra **genel bakış** sayfasında **Bağlan** üstünde. VM'nize bağlanmak için dizesini kopyalayın.
2. **Connect** tercih ettiğiniz SSH istemcisi ile VM.  
3. Terminal penceresinde CURL, kullanarak Azure Resource Manager için bir erişim belirteci almak üzere yerel MSI uç nokta için bir isteği oluşturun.  

   Bir erişim belirteci almak üzere CURL isteği aşağıdaki örnekte gösterilir. Değiştirdiğinizden emin olun `<CLIENT ID>` ile `clientId` özellik tarafından döndürülen `az identity create` komutunu [kullanıcı tarafından atanan bir MSI oluşturmak](#create-a-user-assigned-msi): 
    
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

4. Artık Azure Resource Manager erişmek için erişim belirtecini kullanır ve için daha önce kullanıcı tarafından atanan MSI erişim izni kaynak grubunun özelliklerini okur. Değiştirdiğinizden emin olun `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` daha önce belirttiğiniz değerleri içeren ve `<ACCESS TOKEN>` önceki adımda döndürülen belirteci ile.

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

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
